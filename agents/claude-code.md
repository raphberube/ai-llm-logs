# Claude Code

- Documentation : https://docs.anthropic.com/en/docs/claude-code/overview
- Useful tips about Claude Code : https://claudelog.com/

## Environment Variables

- API timeout (3 minutes) : `export API_TIMEOUT_MS=180000` 

## Settings

- Project settings : `.claude/settings.json`
- User settings : `~/.claude/settings.json`

### Permissions

```json
"permissions": {
    "allow": [
        "Bash(grep:*)"
    ]
  }
```

## Install 

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

## Plan Mode

- Enable **Plan Mode** : press `shift + tab` twice

## Use Claude Code with FuelIx

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.fuelix.ai",
    "ANTHROPIC_AUTH_TOKEN": "<api key>",
    "ANTHROPIC_MODEL": "claude-4-sonnet"
  }
}
```

## Python code

### Permissions

Useful permissions to let Claude Code execute basic tasks without asking permission.

```json
{
    "permissions": {
        "allow": [
            "Bash(poe:*)",
            "Bash(pytest:*)",
            "Bash(black:*)",
            "Bash(flake8:*)"
        ]
    }
}
```

### Hooks

```json
{
  "hooks": {
        "PostToolUse": [
            {
                "matcher": "Edit|MultiEdit|Write",
                "hooks": [
                    {
                        "type": "command",
                        "command": "jq -r '.tool_input.file_path' | { read file_path; if echo \"$file_path\" | grep -q '\\.py$'; then pyright \"$file_path\" >&2; fi; }"
                    },
                    {
                        "type": "command",
                        "command": "jq -r '.tool_input.file_path' | { read file_path; if echo \"$file_path\" | grep -q '\\.py$'; then black \"$file_path\" >&2; fi; }"
                    },
                    {
                        "type": "command",
                        "command": "jq -r '.tool_input.file_path' | { read file_path; if echo \"$file_path\" | grep -q '\\.py$'; then autoflake \"$file_path\" >&2; fi; }"
                    },
                    {
                        "type": "command",
                        "command": "jq -r '.tool_input.file_path' | { read file_path; if echo \"$file_path\" | grep -q '\\.py$'; then flake8 \"$file_path\" >&2; fi; }"
                    }
                ]
            }
        ]
    }
}
```

## Appendix

### Settings - Complete user settings example

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.fuelix.ai",
    "ANTHROPIC_AUTH_TOKEN": "<api key>",
    "ANTHROPIC_MODEL": "claude-4-sonnet"
  },
  "permissions": {
    "allow": [
        "Bash(poe pytest:*)",
        "Bash(pytest:*)",
        "Bash(black:*)",
        "Bash(flake8:*)",
        "Bash(grep:*)"
    ]
  }
}
```

```
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.fuelix.ai",
    "ANTHROPIC_MODEL": "claude-sonnet-4-5[1m]",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "claude-haiku-4-5",
    "DISABLE_TELEMETRY": "1"
  },
  "hooks": {
    "PostToolUse": [
    ]
  },
  "permissions": {
    "ask": [
      "mcp__github__issue_write",
      "mcp__github__update_pull_request "
    ],
    "allow": [
      "Bash(rg:*)",
      "Bash(grep:*)",
      "Bash(sed:*)",
      "Bash(wc:*)",
      "Bash(pdfgrep:*)",
      "Bash(gh pr view:*)",
      "Bash(gh pr diff:*)",
      "Bash(gh issue view:*)",
      "Bash(git log:*)",
      "Bash(git diff:*)",
      "Bash(git show:*)",
      "Bash(test -f:*)",
      "mcp__github__get_issue",
      "mcp__github__get_issue_comments",
      "mcp__github__pull_request_read",
      "mcp__github__issue_read",
      "mcp__github__get_me",
      "mcp__github__list_issues",
      "mcp__github__search_issues",
      "mcp__context7__resolve-library-id",
      "mcp__context7__get-library-docs"
    ]
  },
  "extraKnownMarketplaces": {
    "claude-code-plugins": {
      "source": {
        "source": "github",
        "repo": "anthropics/claude-code"
      }
    }
  },
  "enabledPlugins": {
    "pr-review-toolkit@claude-code-plugins": true,
    "feature-dev@claude-code-plugins": true,
    "code-review@claude-code-plugins": true
  }
}
```

```
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.fuelix.ai",
    "ANTHROPIC_AUTH_TOKEN": "<fuelix api key>",
    "ANTHROPIC_MODEL": "claude-sonnet-4-5-20250929[1m]",
    "ANTHROPIC_CUSTOM_HEADERS": "anthropic-beta: context-1m-2025-08-07",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "claude-haiku-4-5"
  },
  "permissions": {
    "allow": [
      "Bash(poe pytest:*)",
      "Bash(pytest:*)",
      "Bash(black:*)",
      "Bash(flake8:*)",
      "Bash(grep:*)",
      "Bash(ast-grep:*)",
      "Bash(grep:*)",
      "Bash(gh pr view:*)",
      "Bash(git log:*)"
    ]
  },
  "enabledPlugins": {
    "pr-review-toolkit@claude-code-plugins": true,
    "feature-dev@claude-code-plugins": true,
    "code-review@claude-code-plugins": true
  },
  "alwaysThinkingEnabled": true
}
```