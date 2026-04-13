# Stage Agent Mode

Create and manage creative projects from any AI agent. Set up the full project plan, run research and strategy workflows, generate deliverables, and send outputs through Claude-connected tools such as Notion or Figma while Stage stays the source of truth.

## Install

```bash
npx skills add getstage/agent-mode
```

Works with **Claude Code**, **Cursor**, **Windsurf**, **Codex**, and any agent that supports skills.

## Setup

### 1. Get your API key

Go to [Stage](https://getstage.co) > Settings > Developer and generate an API key.

Pro plan required. Keys start with `stg_`.

### 2. Set the key

```bash
export STAGE_API_KEY=stg_your_key_here
```

Or pass it in your agent config. The skill uses Bearer auth:

```http
Authorization: Bearer stg_...
```

## What it does

The skill teaches your agent how to:

- **Create full projects** — phases, tasks, timeline, budget, client name
- **Read project status** — list projects, check progress, view tasks
- **Run research flows** — save context, create runs, and persist research artifacts
- **Run strategy flows** — generate sections and keep approval state in Stage
- **Run generate flows** — create outputs and keep export history in Stage
- **Handshake Claude with Stage** — verify the Claude connection from the setup page
- **Export through Claude** — send outputs to Notion or Figma and record the result back in Stage
- **Link Stitch projects** — connect a Google Stitch design workspace to a Stage project
- **Sync design previews** — pull the latest screens from Stitch into Stage
- **Follow safety rules** — read/create/update/destructive action policy built in

## Example prompts

```
"Create a branding project for a bakery. Budget 2500, deadline 4 weeks."

"What's the status of my latest project?"

"Link this Stitch project and sync the latest screens."

"Delete all tasks in the Discovery phase." → agent asks for confirmation first
```

## How it works

Stage is the executor. Your agent is the interpreter.

1. You give a messy prompt
2. The agent reads the skill file and understands Stage's API
3. The agent builds a structured API call
4. Stage validates, stores, and returns data
5. Claude uses connected tools only when needed and writes the result back into Stage

Stage is the operating system and source of truth. Claude is the reasoning layer and operator.

## Action policy

The skill enforces a strict action policy:

| Action | Rule |
|--------|------|
| **Read** | Execute immediately |
| **Create** | Only at high confidence from the prompt |
| **Update** | Only when target and change are unambiguous |
| **Destructive** | Always require explicit user confirmation |

## Preferred endpoints

### Projects

```
GET    /api/v1/projects
GET    /api/v1/projects/:id
POST   /api/v1/projects
POST   /api/v1/projects/import-plan
```

### Phases & Tasks

```
GET    /api/v1/projects/:id/phases
POST   /api/v1/projects/:id/phases
GET    /api/v1/phases/:id/tasks
POST   /api/v1/phases/:id/tasks
GET    /api/v1/tasks/:id
POST   /api/v1/tasks/:id/toggle
```

### Stitch (design)

```
POST   /api/v1/projects/:id/design-connections
GET    /api/v1/projects/:id/design-connections
POST   /api/v1/projects/:id/designs/upload-url
POST   /api/v1/projects/:id/designs/sync
GET    /api/v1/projects/:id/designs
```

### Claude and AI workflow

```
POST   /api/v1/agent/connections/claude/handshake
GET    /api/v1/projects/:id/ai/context
POST   /api/v1/projects/:id/ai/context
GET    /api/v1/projects/:id/ai/runs
POST   /api/v1/projects/:id/ai/runs
GET    /api/v1/projects/:id/ai/artifacts
POST   /api/v1/projects/:id/ai/artifacts
POST   /api/v1/ai/artifacts/:id/exports
```

## Supported clients

| Client | How |
|--------|-----|
| Claude Code | `npx skills add getstage/agent-mode` |
| Cursor | Drop `SKILL.md` into workspace rules |
| Windsurf | Add `SKILL.md` to project context |
| Codex | Add `SKILL.md` to project context |
| Any MCP client | Use skill file as agent context |

## Manual install

If your agent doesn't support `npx skills`, download the skill file directly:

```bash
curl -O https://getstage.co/SKILL.md
```

## Troubleshooting

**"Unauthorized" error**
Your API key is missing or invalid. Generate a new one in Settings > Developer.

**"Rate limited" error**
Too many requests. Wait a moment and try again.

**Agent doesn't follow the action policy**
Make sure the skill file is loaded in your agent's context. Re-run the install command if needed.

## Links

- [Stage](https://getstage.co)
- [API Reference](https://getstage.co/docs)
- [All integrations](https://getstage.co/agents)
- [Stitch integration](https://getstage.co/agents/stitch)

## License

MIT - see [LICENSE](LICENSE)

## Contributors

- [Werner Johannes Dieben](https://github.com/wernerjohannesdieben) - creator
- [Claude](https://claude.ai) - co-author
