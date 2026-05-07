# webapp-toolkit

A Claude Code plugin packaging opinionated, web-app-flavoured skills as a
single drop-in install. Declares
[`git-workflow-guards`](https://github.com/ZainRizvi/git-workflow-guards) as
a dependency, so installing this one transitively pulls in the git/PR-workflow
guards too. Install `webapp-toolkit` and you get both: the web-app skills
documented below, plus all of git-workflow-guards' hooks, agents, and skills.

## Install

There are two paths. Either way, the install is **a one-time, per-machine action** — Claude Code does not silently install plugins from a project's settings without the user's explicit say-so.

### User-level (just for you, across all repos)

```bash
claude plugin marketplace add ZainRizvi/webapp-toolkit
claude plugin install webapp-toolkit@webapp-toolkit
```

The marketplace and plugin are persisted to `~/.claude/`, and every Claude Code session has the plugin enabled.

### Project-level (everyone on a repo gets it)

Commit this into your repo's `.claude/settings.json`:

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

When a teammate opens the repo with Claude Code for the first time, Claude Code prompts them to trust the marketplace. One click, and the plugin is enabled for that machine — persistent across sessions, no further prompts.

If you'd rather not wait for the prompt, the user-level CLI commands above install the same thing immediately.

## What ships

| Skill | What it covers |
|---|---|
| `/frontend-design` | Build distinctive, production-grade frontend interfaces. Avoids generic AI aesthetics. Use when asked to build web components, pages, or applications. |
| `/dev-browser` | Persistent-state browser automation. Use for interactive exploration, ARIA-snapshot discovery of unknown layouts, or multi-step flows where login/cookies must persist between scripts. (For stateless one-shot scripts or full test suites, use the standalone `playwright-skill` plugin instead.) |
| `/paddle-integration` | Integrate Paddle payments — set up subscriptions, configure webhooks, debug billing issues. Covers sandbox testing and production deployment. |
| `/vercel-infrastructure` | Set up Vercel projects, configure blob storage, manage environment variables, set up custom domains with wildcard subdomains. Captures the gotchas that bite first-time Vercel users (env var loss, wildcard routing requirements). |
| `/seo` | Routing skill for SEO work — content writing, keyword strategy, meta tags, structure, authority building, snippets, content planning, freshness, cannibalization detection. Reaches into a set of focused sub-skills based on the task. |

## First-run setup notes

### `/dev-browser`

The skill ships with its own Node-based browser server. The first time you
invoke it, the bundled `server.sh` runs `bun install` (or `npm install`)
and downloads Playwright Chromium — usually 30-60 seconds. Subsequent runs
start instantly. The server runs at `localhost:9222` by default.

Required: `bun` or `npm`. Recommended: `bun` (faster install, lockfile already
shipped).

### `/paddle-integration`, `/vercel-infrastructure`, and `/seo`

These are pure prompt skills with no runtime to install. They guide you through
the relevant CLI / dashboard / API steps; you supply the credentials.

### `/frontend-design`

Pure prompt skill. No setup.

## Layout

```
.claude-plugin/
  plugin.json                    # plugin manifest
  marketplace.json               # marketplace entry (single-plugin repo)
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
  seo/                            # routing skill + 10 sub-skill files
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
