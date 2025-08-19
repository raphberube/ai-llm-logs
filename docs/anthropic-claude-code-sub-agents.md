# Custom Subagents in Claude Code

Custom subagents in Claude Code are specialized AI assistants that can be invoked to handle specific types of tasks. They enable more efficient problem-solving by providing task-specific configurations with customized system prompts, tools and a separate context window.

## What are subagents?

Subagents are pre-configured AI personalities that Claude Code can delegate tasks to. Each subagent:

* Has a specific purpose and expertise area
* Uses its own context window separate from the main conversation
* Can be configured with specific tools it's allowed to use
* Includes a custom system prompt that guides its behavior

When Claude Code encounters a task that matches a subagent's expertise, it can delegate that task to the specialized subagent, which works independently and returns results.

## Key benefits

* **Context Preservation**: Each subagent operates within its own separate context window, preventing the main conversation from being cluttered with task-specific details
* **Specialized Expertise**: Subagents can be fine-tuned with detailed instructions and custom system prompts for specific domains, leading to higher success rates
* **Reusability**: Once created, subagents can be utilized across different projects and shared with team members
* **Flexible Permissions**: Granular control over which tools each subagent can access

## Quick start

To create your first subagent:

1. **Open the agents interface**: Use the `/agents` command in Claude Code
2. **Create the directory**: `mkdir -p .claude/agents`
3. **Create your agent file**: Create a markdown file with YAML frontmatter
4. **Test your agent**: Invoke it explicitly or let Claude Code use it automatically

### Example: Creating a test runner subagent

```bash
mkdir -p .claude/agents
echo '---
name: test-runner
description: Use proactively to run tests and fix failures
---

You are a test automation expert. When you see code changes, proactively run the appropriate tests. If tests fail, analyze the failures and fix them while preserving the original test intent.' > .claude/agents/test-runner.md
```

## Subagent configuration

### File locations

Subagents are stored as Markdown files with YAML frontmatter in two possible locations:

| Type | Location | Scope | Priority |
| --- | --- | --- | --- |
| **Project subagents** | `.claude/agents/` | Available in current project | Highest |
| **User subagents** | `~/.claude/agents/` | Available across all projects | Lower |

When subagent names conflict, project-level subagents take precedence over user-level subagents.

### File format

Each subagent is defined in a Markdown file with this structure:

#### Configuration fields

| Field | Required | Description |
| --- | --- | --- |
| `name` | Yes | Unique identifier using lowercase letters and hyphens |
| `description` | Yes | Natural language description of the subagent's purpose |
| `tools` | No | Comma-separated list of specific tools. If omitted, inherits all tools from the main thread |

### Available tools

Subagents can be granted access to any of Claude Code's internal tools. See the [tools documentation](about:/en/docs/claude-code/settings#tools-available-to-claude) for a complete list of available tools.

You have two options for configuring tools:

* **Omit the `tools` field** to inherit all tools from the main thread (default), including MCP tools
* **Specify individual tools** as a comma-separated list for more granular control (can be edited manually or via `/agents`)

**MCP Tools**: Subagents can access MCP tools from configured MCP servers. When the `tools` field is omitted, subagents inherit all MCP tools available to the main thread.

## Managing subagents

### Using the /agents command (Recommended)

The `/agents` command provides a comprehensive interface for subagent management:

This opens an interactive menu where you can:

* View all available subagents (built-in, user, and project)
* Create new subagents with guided setup
* Edit existing custom subagents, including their tool access
* Delete custom subagents
* See which subagents are active when duplicates exist
* **Easily manage tool permissions** with a complete list of available tools

### Direct file management

You can also manage subagents by working directly with their files:

## Using subagents effectively

### Automatic delegation

Claude Code proactively delegates tasks based on:

* The task description in your request
* The `description` field in subagent configurations
* Current context and available tools

### Explicit invocation

Request a specific subagent by mentioning it in your command:

```bash
# Explicitly invoke a specific subagent
> Use the python-expert to optimize this algorithm
> Get the react-expert to refactor this component
> Use the code-reviewer sub agent to check my recent changes
```

## Example subagents

### Code reviewer

```markdown
---
name: code-reviewer
description: Expert code review specialist. Proactively reviews code for quality, security, and maintainability. Use immediately after writing or modifying code.
tools: Read, Grep, Glob, Bash
---

You are a senior code reviewer ensuring high standards of code quality and security.

When invoked:
1. Run git diff to see recent changes
2. Focus on modified files
3. Begin review immediately

Review checklist:
- Code is simple and readable
- Functions and variables are well-named
- No duplicated code
- Proper error handling
- No exposed secrets or API keys
- Input validation implemented
- Good test coverage
- Performance considerations addressed

Provide feedback organized by priority:
- Critical issues (must fix)
- Warnings (should fix)
- Suggestions (consider improving)

Include specific examples of how to fix issues.
```

### Debugger

```markdown
---
name: debugger
description: Debugging specialist for errors, test failures, and unexpected behavior. Use proactively when encountering any issues.
tools: Read, Edit, Bash, Grep, Glob
---

You are an expert debugger specializing in root cause analysis.

When invoked:
1. Capture error message and stack trace
2. Identify reproduction steps
3. Isolate the failure location
4. Implement minimal fix
5. Verify solution works

Debugging process:
- Analyze error messages and logs
- Check recent code changes
- Form and test hypotheses
- Add strategic debug logging
- Inspect variable states

For each issue, provide:
- Root cause explanation
- Evidence supporting the diagnosis
- Specific code fix
- Testing approach
- Prevention recommendations

Focus on fixing the underlying issue, not just symptoms.
```

### Data scientist

```markdown
---
name: data-scientist
description: Data analysis expert for SQL queries, BigQuery operations, and data insights. Use proactively for data analysis tasks and queries.
tools: Bash, Read, Write
---

You are a data scientist specializing in SQL and BigQuery analysis.

When invoked:
1. Understand the data analysis requirement
2. Write efficient SQL queries
3. Use BigQuery command line tools (bq) when appropriate
4. Analyze and summarize results
5. Present findings clearly

Key practices:
- Write optimized SQL queries with proper filters
- Use appropriate aggregations and joins
- Include comments explaining complex logic
- Format results for readability
- Provide data-driven recommendations

For each analysis:
- Explain the query approach
- Document any assumptions
- Highlight key findings
- Suggest next steps based on data

Always ensure queries are efficient and cost-effective.
```

## Agent engineering principles

### Token efficiency and performance

Each custom agent invocation carries a **variable initialization cost** based on tool count and configuration complexity. Understanding this is crucial for effective agent design:

**Agent Weight Classifications:**
* **Lightweight agents**: Under 3k tokens - Low initialization cost, maximum composability
* **Medium-weight agents**: 10-15k tokens - Moderate initialization cost
* **Heavy agents**: 25k+ tokens - High initialization cost, use sparingly

**Performance by Tool Count (Experimental Data):**

| Tool Count | Token Usage | Initialization Time | Recommendation |
|------------|-------------|-------------------|----------------|
| 0-1 tools  | 640-2.6k   | 2.6-3.9s         | Optimal for frequent use |
| 2-4 tools  | 2.9-3.4k   | 4.3-6.1s         | Good balance |
| 5-10 tools | 3.9-7.9k   | 5.1-7.0s         | Use judiciously |
| 15+ tools  | 13.9-25k   | 6.4s+            | Reserve for complex tasks |


## Best practices for defining subagents

### 1. Naming conventions

* Use **lowercase letters and hyphens** for agent names: `react-expert`, `database-optimizer`
* Keep names **descriptive but concise**: `api-security-audit` not `api-security-audit-specialist-expert`
* Avoid generic names like `helper` or `assistant`
* **Consider nicknames for efficiency**: `UX Agent (A1)`, `Security Agent (S1)` for quick invocation

### 2. Writing effective descriptions

* **Be specific about when to use the agent**: Include trigger conditions and use cases
* **Use action-oriented language**: "Use proactively when..." or "Deploy immediately after..."
* **Include examples in the description**: Show concrete scenarios where the agent applies
* **Optimize for automatic delegation**: Include terms like `use PROACTIVELY` or `MUST BE USED` to encourage Claude to invoke the agent

```markdown
# Good description
description: Expert code review specialist. Proactively reviews code for quality, security, and maintainability. Use immediately after writing or modifying code.

# Poor description  
description: Helps with code review tasks.

# Optimized for auto-delegation
description: Security analysis specialist for authentication and authorization code. Use PROACTIVELY when reviewing auth.ts, login systems, or security-related files.
```

### 3. System prompt best practices

* **Define the agent's persona clearly**: "You are a senior security engineer specializing in..."
* **Provide structured workflows**: Use numbered steps or bullet points for processes
* **Include quality checklists**: Define what constitutes good output
* **Set clear boundaries**: Specify what the agent should and shouldn't do
* **Use examples and templates**: Show expected input/output formats

### 4. Tool selection strategy

* **Principle of least privilege**: Only grant necessary tools
* **Consider the agent's workflow**: What tools does it need to complete its tasks?
* **Common tool combinations**:
  - Code analysis: `Read, Grep, Glob`
  - File modification: `Read, Edit, Write`
  - System operations: `Bash, Read, Write`
  - Research tasks: `WebSearch, Read, Write`

### 5. Agent structure template

```markdown
---
name: agent-name
description: Specific description with use cases and trigger conditions
tools: Tool1, Tool2, Tool3  # Optional - omit to inherit all tools
model: claude-sonnet-4-20250514  # Optional - specify model if needed
---

You are a [role/persona] specializing in [domain/expertise].

## Core responsibilities:
- Primary responsibility 1
- Primary responsibility 2
- Primary responsibility 3

## When invoked:
1. Step-by-step process
2. Clear workflow
3. Expected outcomes

## Quality standards:
- Criterion 1
- Criterion 2
- Criterion 3

## Output format:
[Specify expected output structure]

## Examples:
[Provide concrete examples of usage]
```

### 6. Testing and iteration

* **Test with real scenarios**: Use actual project data and workflows
* **Iterate based on performance**: Refine prompts based on agent behavior
* **Monitor tool usage**: Ensure agents use tools efficiently
* **Gather team feedback**: If shared, collect input from other users


### 7. Progressive tool expansion

* **Start minimal**: Begin with carefully scoped tool sets
* **Validate behavior**: Test agent reliability before expanding capabilities
* **Add incrementally**: Expand tool scope as you identify additional needs
* **Monitor performance**: Track how tool additions affect initialization time

### 8. Documentation and maintenance

* **Document agent purpose**: Keep clear records of what each agent does
* **Version control**: Track changes to agent configurations
* **Regular reviews**: Periodically assess agent effectiveness
* **Update dependencies**: Keep tool lists current as new tools become available
* **Share discoveries**: Contribute findings about effective agent patterns to the community

### 9. Agent nicknames and efficiency

Configure short nicknames for frequently used agents to enable quick invocation:

```markdown
---
name: UX Agent (A1)
description: UX specialist for user experience analysis. Use PROACTIVELY when evaluating interfaces and user workflows.
tools: Read, Grep, Glob
---

You are a UX specialist focused on user experience analysis and interface evaluation.
```

**Usage with nicknames:**
```bash
# Quick invocation
ask agent a1 to review the navigation menu UX
ask a1 to review the navigation menu UX

# Multiple agents
ask A1, P2, C1 to review the changes
```

## Advanced usage

### Chaining subagents

For complex workflows, you can chain multiple subagents:

```bash
# Example: Complete code review workflow
> First use the code-analyzer sub agent to find performance issues, then use the optimizer sub agent to fix them

# Example: Research and implementation
> Use the research-coordinator to gather requirements, then the technical-architect to design the solution
```

**Best practices for chaining:**
* Design agents with clear input/output contracts
* Ensure each agent can work with the previous agent's output
* Consider using orchestrator agents for complex multi-step workflows

### Dynamic subagent selection

Claude Code intelligently selects subagents based on context. Make your `description` fields specific and action-oriented for best results.

**Optimization tips:**
* Use **domain-specific keywords** in descriptions
* Include **file type associations**: "Use for .py files" or "Deploy for React components"
* Specify **project characteristics**: "Use in microservices architectures"

### Agent orchestration patterns

**1. Sequential Processing:**
```markdown
Agent A → Agent B → Agent C
(Each agent processes the output of the previous one)
```

**2. Parallel Processing:**
```markdown
Input → Agent A
      → Agent B  → Synthesizer Agent
      → Agent C
```

**3. Conditional Routing:**
```markdown
Input → Decision Agent → Specialist Agent A (if condition X)
                      → Specialist Agent B (if condition Y)
```

## Troubleshooting

### Common issues and solutions

**Agent not responding appropriately:**
* Check the agent's configuration file for syntax errors
* Verify the description matches your use case
* Ensure relevant context is provided in your request
* Update agent configuration if outdated

**Performance impact:**
* Monitor response times - too many agents can slow Claude Code
* Selectively activate only relevant agents for current work
* Regular cleanup of unused or outdated agent configurations

**Conflicting agent advice:**
* Review agent overlap - check if multiple agents cover similar areas
* Prioritize by relevance - focus on the most relevant agent for your task
* Customize configurations to reduce conflicts between similar agents

### Agent debugging

* **Test in isolation**: Invoke agents explicitly to test behavior
* **Check tool permissions**: Ensure agents have access to required tools
* **Review system prompts**: Verify instructions are clear and unambiguous
* **Monitor tool usage**: Check if agents are using tools efficiently

## Performance considerations

* **Context efficiency**: Agents help preserve main context, enabling longer overall sessions
* **Latency**: Subagents start off with a clean slate each time they are invoked and may add latency as they gather context that they require to do their job effectively
* **Resource management**: Monitor response times and selectively activate relevant agents
* **Tool optimization**: Agents with focused tool sets perform better than those with broad access

## Conclusion

Subagents in Claude Code represent a powerful paradigm for specialized AI assistance. By following the best practices outlined in this guide, you can create highly effective, focused agents that enhance your development workflow. Remember to:

1. **Start simple**: Begin with focused, single-purpose agents
2. **Iterate and improve**: Refine based on real-world usage
3. **Share and collaborate**: Leverage community collections and contribute back
4. **Monitor performance**: Keep track of agent effectiveness and resource usage

The key to successful subagent implementation is balancing specialization with maintainability, ensuring each agent has a clear purpose and well-defined boundaries.

## Related documentation

* [Slash commands](/en/docs/claude-code/slash-commands) - Learn about other built-in commands
* [Settings](/en/docs/claude-code/settings) - Configure Claude Code behavior
* [Hooks](/en/docs/claude-code/hooks) - Automate workflows with event handlers
* [MCP Servers](/en/docs/claude-code/mcp) - Extend functionality with Model Context Protocol
