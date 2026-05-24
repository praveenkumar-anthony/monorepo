# AI Assisted Development Using Figma Code Connect

This document explains how engineers can use Figma Code Connect together with the Figma MCP server and GitHub Copilot in VS Code to generate design-system-aligned code directly from Figma designs.

---

## 1. Figma Code Connect

### What it is

Figma Code Connect is a tool that links design system components in Figma to their real implementations in your codebase. Once linked, Figma's Dev Mode and AI coding agents stop showing generic auto-generated CSS and start showing the actual component call from your design system — with the right import path and the right prop values for whatever variant the designer used.

**The problem it solves:** by default, when a developer inspects a button in Figma's Dev Mode, Figma shows a generic snippet like `<div class="button" style="padding: 12px..."`. That output has no relationship to the codebase. Code Connect replaces it with `<Button variant="primary" size="md">Save</Button>` (or whatever your real component looks like), including the correct import.

### Plan requirements

Code Connect is available on **Organization and Enterprise plans**, and requires a full **Design seat or Dev Mode seat**.

### Two ways to set it up

Both feed into the same Figma MCP infrastructure, but the workflow differs:

- **Code Connect CLI** — runs locally in your repository as the `@figma/code-connect` npm package. You write `.figma.tsx` files alongside your components, define prop mappings and example snippets in code, and publish them to Figma. Best for engineers who want precise, version-controlled mappings.
- **Code Connect UI** — runs entirely inside Figma and connects directly to GitHub. It's language-agnostic, faster to set up, and supports mapping a single Figma component to multiple frameworks (React, Vue, SwiftUI, etc.). Best for collaborative design + engineering setup.

You can only use one connection type per component, but both can coexist in the same design system.

### Why it matters for AI code generation

When Code Connect is set up, the Figma MCP server enriches the context it sends to AI agents like GitHub Copilot. Instead of describing a node by its raw layout and pixels, the server tells the agent: *"this rectangle is a `Button` — here's how to import it, here's how to call it, here are the props that correspond to this variant."* This is what makes the difference between "AI invents UI from a screenshot" and "AI assembles known building blocks into the layout it sees."

---

## 2. Figma MCP Configuration and Authentication

### What MCP is

Model Context Protocol (MCP) is a standard for how applications expose structured context to AI agents. The Figma MCP server exposes Figma file data — frames, components, variables, and Code Connect mappings — as MCP tools that Copilot can call from VS Code.

### Our setup

We use the **remote Figma MCP server**, configured in `.vscode/mcp.json` at the root of the Nx workspace. This file declares the Figma server as an MCP source that VS Code's Copilot can connect to.

A typical entry looks like this:

```json
{
  "servers": {
    "figma": {
      "type": "http",
      "url": "https://mcp.figma.com/mcp"
    }
  }
}
```

### Authentication flow

1. Open `.vscode/mcp.json` in VS Code.
2. Click the **Start** button that appears above the `figma` server entry.
3. VS Code opens a browser window and routes you through **Figma SSO**.
4. After authentication, the token is stored by VS Code and the MCP server status switches to *Running*.
5. From this point on, GitHub Copilot in VS Code can call Figma MCP tools on your behalf — read selected frames, fetch variables, look up Code Connect mappings, etc.

You only need to repeat the Start + SSO step when the session expires or when the `mcp.json` config changes.

### Tools the Figma MCP server exposes

Once authenticated, Copilot has access to tools including:

| Tool | Purpose |
|---|---|
| `get_code` / `get_design_context` | Returns a React + Tailwind representation of the selected frame as a starting point |
| `get_variable_defs` | Extracts design tokens (colors, spacing, typography) used in the selection |
| `get_screenshot` | PNG render of the selection for visual context |
| `get_metadata` | Structural metadata about the node tree |
| `get_code_connect_map` | Returns Code Connect bindings: file path + component name for connected Figma nodes |
| `search_design_system` | Text search across design libraries for components, variables, and styles |
| `use_figma` | General-purpose write tool for creating or editing Figma objects |
| `upload_assets` | Uploads images (PNG/JPG/GIF/WebP) to a Figma file |
| `generate_diagram` | Creates a FigJam diagram from Mermaid syntax or natural language |

### Using it from Copilot

Once the server is running, you can prompt Copilot in Agent Mode with something like:

> *Use my current Figma selection and generate a React component using our design system. Call `get_code_connect_map` first, then `get_code` and `get_variable_defs`.*

Or with a direct link:

> *Implement this screen as a React component: `https://www.figma.com/design/<file-key>/...?node-id=<node-id>`*

The agent will call the appropriate MCP tools, receive the enriched context, and generate code that uses the real components from our codebase.

---

## 3. Connecting Components

This section explains how to actually bind a code component to its Figma counterpart using the Code Connect CLI.

### The `figma.connect` function

`figma.connect` is the entry point. It is a function from the `@figma/code-connect` npm package that you call inside a `.figma.tsx` file. Important: this file is **never executed at runtime** — the Code Connect CLI parses it as a TypeScript AST and treats your code as structured metadata, not executable logic.

Signature:

```ts
figma.connect(Component, figmaUrl, {
  props: { /* property mappings */ },
  example: (props) => /* JSX snippet shown in Dev Mode */,
})
```

### A complete example

```tsx
// Button.figma.tsx
import figma from '@figma/code-connect'
import { Button } from './Button'

figma.connect(
  Button,
  'https://www.figma.com/design/abc123/DesignSystem?node-id=1:2',
  {
    props: {
      label: figma.string('Text Content'),
      disabled: figma.boolean('Disabled'),
      size: figma.enum('Size', {
        Large: 'lg',
        Medium: 'md',
        Small: 'sm',
      }),
      icon: figma.instance('Icon'),
    },
    example: ({ label, disabled, size, icon }) => (
      <Button size={size} disabled={disabled} icon={icon}>
        {label}
      </Button>
    ),
  },
)
```

### The `props` block — mapping helpers

Each helper reads a property off the Figma node and exposes it as a code-side prop:

| Helper | Maps to |
|---|---|
| `figma.string('Name')` | A Figma text property → string |
| `figma.boolean('Name')` | A Figma boolean property → boolean |
| `figma.enum('Name', { ... })` | A Figma variant property → enum value, with explicit mapping of Figma option names to code values |
| `figma.instance('Name')` | A nested Figma instance → another component, rendered using its own Code Connect mapping |
| `figma.children(['LayerName'])` | The children of a slot layer → JSX children |
| `figma.nestedProps('Layer', { ... })` | Properties from a nested layer, exposed as a nested object |

### The `example` function

The `example` function returns the JSX snippet that Dev Mode (and the AI agent) will see. Because the file is parsed and not executed, the JSX you return is captured **as a string**. This has two consequences:

- You can use real types, hooks, and imports without mocking anything — they're never run.
- Conditionals and ternaries are emitted **verbatim** in the output, not evaluated. You can't dynamically build `figma.connect` calls in a `for` loop.

### Publishing

After writing the mapping, publish it so it appears in Figma:

```bash
npx figma connect publish
```

This uploads the parsed mapping (component reference, Figma node ID, prop mappings, example snippet as a string) via Figma's REST API. Figma stores it server-side, keyed by the Figma node ID.

### How this helps the AI agent generate code

When Copilot calls `get_code` or `get_code_connect_map` on a frame that contains connected components, the Figma MCP server doesn't just return layout data. For each connected instance it injects a synthetic `<CodeConnectSnippet>` marker into the context window that includes:

- **Design properties** — the Figma component's current property values (variant, boolean props, text content)
- **Import statement** — the exact import line to use
- **Component snippet** — the JSX example from your `figma.connect` call, with props filled in to match the design
- **Custom instructions** — any guidance you've added (accessibility notes, prop patterns, team conventions)

The agent receives this enriched context and, instead of inventing a fresh `<div className="...">` from pixels, writes the real component call with the correct import and props.

### Practical accuracy

Even with Code Connect set up well, expect roughly **85–90% accuracy** on well-structured frames. The remaining 10–15% typically requires manual refinement for responsive breakpoints, interaction states, and accessibility details — these are areas the agent still benefits from human review.

### Best practices

- Start by connecting your **most frequently used components** (Button, Input, Card, Modal).
- Keep prop mappings **in sync** with the codebase when component APIs change.
- Add **custom instructions** for components with non-obvious usage patterns.
- Use **Storybook integration** if you already maintain stories — it avoids duplicating examples.
- Treat `.figma.tsx` files as **part of the design system contract**: review them in PRs like any other code.

---

## Related references

- Code Connect docs: https://developers.figma.com/docs/code-connect/
- Code Connect React guide: https://developers.figma.com/docs/code-connect/react
- Figma MCP server docs: https://developers.figma.com/docs/figma-mcp-server/
- Code Connect integration with MCP: https://developers.figma.com/docs/figma-mcp-server/code-connect-integration/
- npm package: https://www.npmjs.com/package/@figma/code-connect
