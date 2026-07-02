# Sample GitHub Copilot Plugin Marketplace

A reference repository demonstrating how to package and share GitHub Copilot customizations as a **Copilot CLI plugin marketplace** — with agents, skills, and instructions for code quality workflows.

## What's Inside

| Type | Name | Purpose |
|------|------|---------|
| Plugin | `code-quality` | Bundles the agent and skills below into an installable CLI plugin |
| Agent | `code-reviewer` | Reviews code for bugs, style, and security issues |
| Skill | `generate-docs` | Generates JSDoc/TSDoc comments for functions and modules |
| Skill | `write-tests` | Writes unit tests for selected code |
| Instructions | `coding-standards` | General coding conventions (VS Code workspace) |
| Instructions | `typescript` | TypeScript-specific rules (VS Code workspace) |
| Instructions | `api-design` | REST API design guidelines (VS Code workspace) |

## Repository Structure

```
.github/
├── copilot-instructions.md              # VS Code only: global agent behavior
├── plugin/
│   └── marketplace.json                 # CLI: marketplace registry
└── instructions/                        # VS Code only: file-scoped instructions
    ├── coding-standards.instructions.md
    ├── typescript.instructions.md
    └── api-design.instructions.md

plugins/
└── code-quality/                        # Single source of truth for agents + skills
    ├── plugin.json                      # CLI plugin manifest
    ├── agents/
    │   └── code-reviewer.agent.md       # Loaded by CLI and VS Code once plugin is installed
    └── skills/
        ├── generate-docs/SKILL.md
        └── write-tests/SKILL.md
```

> **Note on instructions**: `.instructions.md` files are a VS Code-only feature. There is no equivalent in the CLI plugin spec — they only apply when the repo is open in VS Code, not when the plugin is installed via CLI.

## Installing via Copilot CLI

### Option A — Via marketplace (recommended for teams)

```shell
# 1. Register the marketplace (replace with your actual GitHub org/repo)
copilot plugin marketplace add your-org/sample-plugin-marketplace

# 2. Install using plugin-name@marketplace-name
copilot plugin install code-quality@sample-plugin-marketplace

# 3. Verify it loaded
copilot plugin list
```

> The marketplace name comes from the `name` field in `marketplace.json` — currently `sample-plugin-marketplace`.

### Option B — Direct install from GitHub (no marketplace registration needed)

```shell
# Install from a subdirectory of the GitHub repo
copilot plugin install your-org/sample-plugin-marketplace:plugins/code-quality
```

### Option C — Local install (for development/testing)

```shell
# From the repo root on your machine
copilot plugin install ./plugins/code-quality
```

To use it in a Copilot CLI session after installing:

```
/agent            # lists code-reviewer
/skills list      # lists generate-docs and write-tests
```

## Using in VS Code

Install the plugin locally first — this makes agents and skills available in both VS Code and the CLI:

```shell
copilot plugin install ./plugins/code-quality
```

| Customization | How to trigger |
|---------------|----------------|
| `code-reviewer` agent | Select in the agent picker or `@Code Reviewer` in chat |
| `generate-docs` skill | Type `/generate-docs` in chat |
| `write-tests` skill | Type `/write-tests` in chat |
| `coding-standards` instructions | Auto-applied to `src/**` files (VS Code only) |
| `typescript` instructions | Auto-applied to `*.ts` / `*.tsx` files (VS Code only) |
| `api-design` instructions | Loaded on demand when working on API routes (VS Code only) |

## Publishing to Your Organisation

1. Push this repo to GitHub under your org (e.g. `your-org/copilot-plugins`)
2. Tell team members to add the marketplace:
   ```shell
   copilot plugin marketplace add your-org/copilot-plugins
   ```
3. They install the plugin using `plugin-name@marketplace-name`:
   ```shell
   copilot plugin install code-quality@sample-plugin-marketplace
   ```
   Or directly, without registering the marketplace:
   ```shell
   copilot plugin install your-org/copilot-plugins:plugins/code-quality
   ```

To keep it **internal only**, set the repository visibility to **Private** — the CLI respects your GitHub auth token when fetching from private repos.

## Resources

- [Creating a plugin marketplace](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-marketplace)
- [Creating a plugin](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)
- [CLI plugin reference](https://docs.github.com/en/copilot/reference/cli-plugin-reference)
- [VS Code Copilot Customization Docs](https://code.visualstudio.com/docs/copilot/customization/overview)
