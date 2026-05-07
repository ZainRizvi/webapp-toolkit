# webapp-toolkit

A Claude Code plugin packaging four opinionated, web-app-flavoured skills as a
single drop-in install. Companion to
[`git-workflow-guards`](https://github.com/ZainRizvi/git-workflow-guards) —
that one ships git/PR-workflow guards, this one ships the web-app-specific
domain skills you reach for once the repo's basics are in place.

## Install

Add the marketplace and enable the plugin:

```bash
claude /plugin marketplace add ZainRizvi/webapp-toolkit
claude /plugin install webapp-toolkit
```

Or — recommended — bake it into your repo's `.claude/settings.json` so every
contributor gets it automatically on first session:

```json
{
  "extraKnownMarketplaces": {
    "webapp-toolkit": {
      "source": { "source": "github", "repo": "ZainRizvi/webapp-toolkit" }
    }
  },
  "enabledPlugins": {
    "webapp-toolkit@webapp-toolkit": true
  }
}
```

Check that into git, and Claude Code will install/enable the plugin the next
time anyone opens the repo. No manual install step.

## What ships

| Skill | What it covers |
|---|---|
| `/frontend-design` | Build distinctive, production-grade frontend interfaces. Avoids generic AI aesthetics. Use when asked to build web components, pages, or applications. |
| `/dev-browser` | Persistent-state browser automation. Use for interactive exploration, ARIA-snapshot discovery of unknown layouts, or multi-step flows where login/cookies must persist between scripts. (For stateless one-shot scripts or full test suites, use the standalone `playwright-skill` plugin instead.) |
| `/paddle-integration` | Integrate Paddle payments — set up subscriptions, configure webhooks, debug billing issues. Covers sandbox testing and production deployment. |
| `/vercel-infrastructure` | Set up Vercel projects, configure blob storage, manage environment variables, set up custom domains with wildcard subdomains. Captures the gotchas that bite first-time Vercel users (env var loss, wildcard routing requirements). |

## First-run setup notes

### `/dev-browser`

The skill ships with its own Node-based browser server. The first time you
invoke it, the bundled `server.sh` runs `bun install` (or `npm install`)
and downloads Playwright Chromium — usually 30-60 seconds. Subsequent runs
start instantly. The server runs at `localhost:9222` by default.

Required: `bun` or `npm`. Recommended: `bun` (faster install, lockfile already
shipped).

### `/paddle-integration` and `/vercel-infrastructure`

These are pure prompt skills with no runtime to install. They guide you through
the relevant CLI / dashboard / API steps; you supply the credentials.

### `/frontend-design`

Pure prompt skill. No setup.

## Layout

```
.claude-plugin/plugin.json       # plugin manifest
marketplace.json                 # marketplace entry (single-plugin repo)
skills/
  frontend-design/SKILL.md       # design skill (adapted from Anthropic)
  dev-browser/                   # browser-automation skill + Node runtime
    SKILL.md
    server.sh
    package.json
    src/
    scripts/
  paddle-integration/SKILL.md
  vercel-infrastructure/SKILL.md
```

## Origin

- `/frontend-design` and `/dev-browser` are adapted from Anthropic's
  [claude-code-plugins](https://github.com/anthropics/claude-code-plugins)
  (MIT). Bundled here so consumers get them as part of one install rather
  than chasing two marketplaces.
- `/paddle-integration` and `/vercel-infrastructure` are personal skills
  written from incident-driven knowledge.

## Companion plugins

- [`git-workflow-guards`](https://github.com/ZainRizvi/git-workflow-guards) —
  git/gh footgun guards, multi-agent code review, ratchet-the-harness
  discipline, lefthook/CLAUDE.md/repo bootstrap skills. Pair with this plugin
  if you want the full opinionated agent setup.

## License

MIT — see [LICENSE](LICENSE).
