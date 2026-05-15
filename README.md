# agentic-workflows-playground

A throwaway sandbox for experimenting with [GitHub Agentic Workflows](https://github.github.com/gh-aw/) and other agentic workflow platforms.

See [`CLAUDE.md`](./CLAUDE.md) for project context, setup steps, and notes.

## Quick start

```bash
# One-time setup
brew install gh
gh auth login
gh extension install github/gh-aw

# Add an Anthropic API key as a repo secret
gh secret set ANTHROPIC_API_KEY

# Add and compile the first workflow
gh aw add githubnext/agentics/daily-repo-status --engine claude
gh aw compile

# Commit and push
git add .github/
git commit -m "Add first agentic workflow"
git push
```

Then trigger from the **Actions** tab on GitHub.
