# Context7 MCP

## Claude Code 

```bash
claude mcp add-json context7 '{
      "autoApprove": [
        "resolve-library-id",
        "get-library-docs"
      ],
      "timeout": 60,
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@upstash/context7-mcp"
      ]
    }' --scope user
```

## Cline 

```json
{
      "autoApprove": [
        "resolve-library-id",
        "get-library-docs"
      ],
      "timeout": 60,
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@upstash/context7-mcp"
      ]
    }
```