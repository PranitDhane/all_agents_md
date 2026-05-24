<!-- code-review-graph MCP tools -->

## Agent Resolver

| User intent / trigger        | Agent                  | File                                          | Use when                                           |
| ---------------------------- | ---------------------- | --------------------------------------------- | -------------------------------------------------- |
| Create or triage a bug story | bug-mode               | all_agents_md/bug-mode.agent.md               | User reports a defect or asks for a bug story      |
| Fix a documented bug         | debuggerCodefixer-mode | all_agents_md/debuggerCodefixer-mode.agent.md | User supplies a bug story and wants implementation |
| Create implementation plan   | plan-mode              | all_agents_md/plan-mode.agent.md              | User asks for a plan before coding                 |
| Review code changes          | review-mode            | all_agents_md/review-mode.agent.md            | User asks for code review                          |
| Create user story            | story-mode             | all_agents_md/story-mode.agent.md             | User wants a business/user story                   |
| Create epic                  | epic-creation          | all_agents_md/epic-creation.agent.md          | User wants an epic or functional spec              |
| Business/investor context    | business-context       | all_agents_md/business-context.agent.md       | User asks for investor/business framing            |
| Generate QA scenarios        | Scenario Generator     | all_agents_md/scenario-generator.agent.md     | User wants scenarios for a release                 |
| Validate QA scenarios        | Scenario Validator     | all_agents_md/scenario-validator.agent.md     | User wants scenarios checked against code          |

## MCP Tools: code-review-graph

**IMPORTANT: This project has a knowledge graph. ALWAYS use the
code-review-graph MCP tools BEFORE using Grep/Glob/Read to explore
the codebase.** The graph is faster, cheaper (fewer tokens), and gives
you structural context (callers, dependents, test coverage) that file
scanning cannot.

### When to use graph tools FIRST

- **Exploring code**: `semantic_search_nodes` or `query_graph` instead of Grep
- **Understanding impact**: `get_impact_radius` instead of manually tracing imports
- **Code review**: `detect_changes` + `get_review_context` instead of reading entire files
- **Finding relationships**: `query_graph` with callers_of/callees_of/imports_of/tests_for
- **Architecture questions**: `get_architecture_overview` + `list_communities`

Fall back to Grep/Glob/Read **only** when the graph doesn't cover what you need.

### Key Tools

| Tool                        | Use when                                               |
| --------------------------- | ------------------------------------------------------ |
| `detect_changes`            | Reviewing code changes — gives risk-scored analysis    |
| `get_review_context`        | Need source snippets for review — token-efficient      |
| `get_impact_radius`         | Understanding blast radius of a change                 |
| `get_affected_flows`        | Finding which execution paths are impacted             |
| `query_graph`               | Tracing callers, callees, imports, tests, dependencies |
| `semantic_search_nodes`     | Finding functions/classes by name or keyword           |
| `get_architecture_overview` | Understanding high-level codebase structure            |
| `refactor_tool`             | Planning renames, finding dead code                    |

### Workflow

1. The graph auto-updates on file changes (via hooks).
2. Use `detect_changes` for code review.
3. Use `get_affected_flows` to understand impact.
4. Use `query_graph` pattern="tests_for" to check coverage.
