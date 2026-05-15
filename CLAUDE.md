# Agentic Workflows Playground

A sandbox for learning and experimenting with [GitHub Agentic Workflows](https://github.github.com/gh-aw/) (`gh-aw`) and other agentic workflow platforms.

## Project goal

Try out `gh-aw` with the **Claude Code engine**, starting with pre-built sample
workflows from [`githubnext/agentics`](https://github.com/githubnext/agentics)
and then building custom ones. May also branch out into other agentic
workflow tools (n8n, CrewAI, LangGraph, Dify, etc.) for comparison.

This is a throwaway repo for experimentation. Nothing important lives here.

## Environment

- macOS
- VS Code with Claude Code in the integrated terminal
- `gh-aw` uses the **Claude Code engine** here (not Copilot)
- Anthropic API key stored as the `ANTHROPIC_API_KEY` GitHub repo secret
- Pay-as-you-go Anthropic API billing (not a Pro/Max subscription)

---

## Setup status

Tick these off as you go:

- [x] Homebrew installed
- [x] `brew install gh`
- [x] `gh auth login` (GitHub.com, HTTPS, browser auth)
- [x] `gh extension install github/gh-aw`
- [x] `gh aw --version` reports a version **newer than 0.71.3**
      (versions 0.68.4 through 0.71.3 are retired due to a billing bug)
- [ ] Anthropic API key generated at <https://console.anthropic.com>
      with a payment method on file
- [ ] `gh secret set ANTHROPIC_API_KEY` in this repo
- [ ] First workflow added (see next section)
- [ ] First run triggered successfully from the **Actions** tab
- [ ] First output reviewed (typically a new issue or comment)

---

## First workflow: Daily Repo Status

Recommended starter because it can be manually dispatched and outputs a
plain GitHub issue you can read.

```bash
# From inside this repo
gh aw add-wizard
# Pick: githubnext/agentics/daily-repo-status
# Engine: claude

# Or in one shot:
gh aw add githubnext/agentics/daily-repo-status --engine claude
gh aw compile
```

Then:

```bash
git add .github/
git commit -m "Add daily-repo-status workflow"
git push
```

Trigger from the GitHub UI: **Actions** tab → **Daily Repo Status** → **Run workflow**.
Output appears as a new issue under the **Issues** tab.

---

## Other gh-aw workflows to play with later

| Workflow | What it does | Trigger |
|---|---|---|
| `githubnext/agentics/issue-triage` | Labels and routes new issues | On issue open |
| `githubnext/agentics/repo-ask` | Answers questions about the repo | `/ask` comment |
| `githubnext/agentics/ci-doctor` | Investigates CI failures | Failed workflow run |
| `githubnext/agentics/pr-nitpick-reviewer` | Fine-grained PR review | `/nitpick` comment |
| `githubnext/agentics/repo-assist` | All-in-one repo helper | Scheduled |

Browse the full list: <https://github.com/githubnext/agentics>

---

## Useful `gh aw` commands

| Command | What it does |
|---|---|
| `gh aw add-wizard` | Interactive workflow picker |
| `gh aw add <source>` | Add a specific workflow by path |
| `gh aw compile` | Regenerate `.lock.yml` from the `.md` source |
| `gh aw logs` | View recent run output from the CLI |
| `gh aw audit` | Check workflow configuration and health |
| `gh aw upgrade` | Upgrade the engine version |
| `gh aw update` | Update added workflows to latest |

---

## How `gh-aw` works (mental model)

1. You write a **Markdown file** with YAML frontmatter (triggers, permissions, safe-outputs, tools) followed by **natural language instructions**.
2. `gh aw compile` turns it into a GitHub Actions `.lock.yml` file.
3. On the trigger event, the workflow spins up a **sandboxed container** with:
   - A read-only GitHub token (writes go through "safe outputs" in a separate job)
   - The **Agent Workflow Firewall (AWF)** restricting network egress
   - Tool allowlisting
4. The Claude Code agent reads the natural language instructions and acts.
5. Any write actions (comments, labels, issues, PRs) are emitted as structured artifacts, scanned for safety, then applied by a separate job with scoped write permissions.

Both the `.md` source and the generated `.lock.yml` should be committed.

---

## Other agentic workflow platforms to explore later

Outside of GitHub, these are worth a look:

**Developer-leaning, code-first**
- **n8n** ([n8n.io](https://n8n.io)) - open-source, self-hostable workflow tool with strong AI agent nodes. Great for connecting AI to hundreds of services.
- **LangGraph** ([langchain-ai.github.io/langgraph](https://langchain-ai.github.io/langgraph/)) - Python framework for stateful, multi-agent workflows. Closest to "writing agents as code".
- **CrewAI** ([crewai.com](https://www.crewai.com/)) - Python framework for multi-agent collaboration with role-based agents.
- **OpenAI Agents SDK** - lightweight Python/TypeScript framework from OpenAI.

**Visual / low-code**
- **Dify** ([dify.ai](https://dify.ai)) - open-source, self-hostable, visual agent builder with LLM-agnostic backend.
- **Flowise** ([flowiseai.com](https://flowiseai.com)) - drag-and-drop builder, open-source.
- **Langflow** ([langflow.org](https://www.langflow.org)) - similar to Flowise, visual front-end for LangChain.

**SaaS, low-friction**
- **Zapier Agents** - extends Zapier with reasoning agents across thousands of apps.
- **Relay.app** - friendlier alternative aimed at beginners.
- **Gumloop** - visual agent builder pitched at marketing/ops workflows.

Each occupies a different niche compared to `gh-aw` (which is repo-native and Actions-bound).

---

## Custom workflow ideas to try

Once a sample is working, ideas for first custom workflows:

- A workflow that posts a welcome comment on first-time contributor issues
- A workflow that flags stale issues with no activity for 30+ days
- A workflow that summarises the week's merged PRs into a release-notes draft

For each one, ask Claude Code to draft the markdown and explain the
frontmatter choices before running `gh aw compile`.

---

## Cost note

The Claude engine bills against the Anthropic API key, not a Claude
subscription. A handful of test runs with Sonnet costs cents, not dollars,
but worth watching the Anthropic console usage page if running scheduled
workflows.

---

## References

- gh-aw home: <https://github.github.com/gh-aw/>
- gh-aw GitHub repo: <https://github.com/github/gh-aw>
- Sample workflows: <https://github.com/githubnext/agentics>
- Engine reference: <https://github.com/github/gh-aw/blob/main/docs/src/content/docs/reference/engines.md>
- Quick start guide: <https://github.github.com/gh-aw/setup/quick-start/>
