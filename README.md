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
├── copilot-instructions.md              # VS Code: global agent behavior
├── plugin/
│   └── marketplace.json                 # CLI: marketplace registry
├── agents/
│   └── code-reviewer.agent.md           # VS Code: agent discovery
├── skills/
│   ├── generate-docs/SKILL.md           # VS Code: skill discovery
│   └── write-tests/SKILL.md
└── instructions/
    ├── coding-standards.instructions.md
    ├── typescript.instructions.md
    └── api-design.instructions.md

plugins/
└── code-quality/                        # CLI: installable plugin
    ├── plugin.json                      #   Plugin manifest
    ├── agents/
    │   └── code-reviewer.agent.md
    └── skills/
        ├── generate-docs/SKILL.md
        └── write-tests/SKILL.md
```

## Installing via Copilot CLI

Add this marketplace and install the plugin in one step:

```shell
# Add the marketplace (replace with your org/repo name after pushing to GitHub)
copilot plugin marketplace add your-org/sample-plugin-marketplace

# Install the code-quality plugin
copilot plugin install code-quality

# Verify it loaded
copilot plugin list
```

To use it in a Copilot CLI session:

```
/agent            # lists code-reviewer
/skills list      # lists generate-docs and write-tests
```

## Using in VS Code

The `.github/` folder is automatically discovered by VS Code Copilot:

| Customization | How to trigger |
|---------------|----------------|
| `code-reviewer` agent | Select in the agent picker or `@Code Reviewer` in chat |
| `generate-docs` skill | Type `/generate-docs` in chat |
| `write-tests` skill | Type `/write-tests` in chat |
| `coding-standards` instructions | Auto-applied to `src/**` files |
| `typescript` instructions | Auto-applied to `*.ts` / `*.tsx` files |
| `api-design` instructions | Loaded on demand when working on API routes |

## Publishing to Your Organisation

1. Push this repo to GitHub under your org (e.g. `your-org/copilot-plugins`)
2. Tell team members to add the marketplace:
   ```shell
   copilot plugin marketplace add your-org/copilot-plugins
   ```
3. They can then install any plugin listed in `marketplace.json`:
   ```shell
   copilot plugin install code-quality
   ```

To keep it **internal only**, set the repository visibility to **Private** — the CLI respects your GitHub auth token when fetching from private repos.

## Resources

- [Creating a plugin marketplace](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-marketplace)
- [Creating a plugin](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)
- [CLI plugin reference](https://docs.github.com/en/copilot/reference/cli-plugin-reference)
- [VS Code Copilot Customization Docs](https://code.visualstudio.com/docs/copilot/customization/overview)
