# Kory's setup: PR-Agent on OpenRouter

This fork's only change from upstream PR-Agent is the default model config:
`pr_agent/settings/configuration.toml` now defaults to OpenRouter
(`openrouter/anthropic/claude-sonnet-5`, fallback `openrouter/deepseek/deepseek-chat`).
Upstream already supports OpenRouter natively, so no code changes were needed.
Change the model by editing that one `[config]` line; any `openrouter/...`
model ID works.

## Run it locally (Windows / PowerShell)

1. Clone this fork and install from source:

```powershell
git clone https://github.com/Kory111111111111111111/pr-agent.git
cd pr-agent
python -m pip install -e .
```

2. Set your keys as environment variables (never commit them):

```powershell
$env:OPENROUTER__KEY = "sk-or-..."        # from openrouter.ai/settings/keys
$env:GITHUB__USER_TOKEN = "ghp_..."       # GitHub classic PAT with repo scope
```

   Alternative: copy `pr_agent/settings/.secrets_template.toml` to
   `pr_agent/settings/.secrets.toml` and fill in `[openrouter] key` and
   `[github] user_token` there instead. `.secrets.toml` is gitignored.

3. Review a pull request:

```powershell
pr-agent --pr_url https://github.com/OWNER/REPO/pull/123 review
```

Other commands on the same PR URL:

- `describe` : title, summary, walkthrough
- `improve` : code suggestions
- `ask "your question"` : ask anything about the PR
- `review` is the main one: line comments, issues, security notes

Reviews are posted to the PR as the account that owns the GitHub token.

## Notes

- Cost is just OpenRouter token usage on your own key. A normal PR review
  with Sonnet-class models runs in the cents range; the DeepSeek fallback
  is there for when the primary model errors or times out.
- To review your own repos before a PR exists, open a draft PR and point
  `--pr_url` at it.
- Sync with upstream later: `git remote add upstream https://github.com/The-PR-Agent/pr-agent.git`
  (already set if you cloned this working copy), then
  `git fetch upstream && git merge upstream/main`. The config change is two
  lines in one file, so conflicts are unlikely and trivial to redo.
