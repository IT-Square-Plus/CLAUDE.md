# 📋 CLAUDE.md User Preferences

> **Purpose:** Global user preferences for Claude Code CLI,
> **Scope:** Applies to all projects and sessions,
> **Version:** 1.0.0,
> **Last Updated:** November 2025

---

# 🔴 CRITICAL RULES (NON-NEGOTIABLE)

<critical priority="HIGHEST">

## Language Requirements

**Code & Technical Content:**

- **ALWAYS** use English for:
  - All code
  - All comments in code
  - All documentation files
  - Commit messages
  - Technical terminology

**User Communication:**

- Detect the language user starts conversation with
- Continue conversation in the SAME language user is using
- If user switches language → adapt accordingly

**NEVER:**

- Mix languages within code (e.g., Polish variable names)
- Use non-English in code comments
- Write technical documentation in languages other than English

---

## Code Modification Boundaries

**YOU MUST:**

- Change **ONLY** what user explicitly requests
- Stay within the exact scope of the request
- Ask before making additional "improvements"
- If tempted to refactor something extra, ask first

**NEVER:**

- Modify files beyond user's request
- Add "helpful" features without permission
- Refactor unrelated code while making requested changes
- Rename variables/functions not mentioned by user
- "Fix" code style in files you're not supposed to touch

**Examples:**

```
✅ GOOD:
User: "Change line 42 in api.ts to add error handling"
You: [Modify only line 42]

❌ BAD:
User: "Change line 42 in api.ts to add error handling"
You: [Modify line 42 + refactor lines 30-50 + rename variables]
```

---

## File Creation Constraints

**ALWAYS:**

- Use filesystem tools directly (Read, Edit, Write)
- Edit files in-place, not via artifacts
- Prefer editing existing files over creating new ones

**NEVER:**

- Create files unless explicitly requested
- Generate documentation (README.md, \*.md) without asking
- Create artifacts
- Auto-generate files "to be helpful"

---

## MCP Server Awareness

**YOU MUST:**

- Check available MCP tools at the START of every session
- Identify which MCP servers are present (pattern: `mcp__{SERVER_NAME}__*` or `mcp__{SERVER_NAME}__{TOOL_NAME}`)
- Apply conditional configurations based on detected servers
- Adapt workflow to available capabilities

**Detection pattern:**

- `mcp__*` → Any MCP server tool
- `mcp__{SERVER_NAME}__*` → All tools from specific server
- `mcp__{SERVER_NAME}__{TOOL_NAME}` → Specific tool from server

**NEVER:**

- Assume MCP tools are available without checking
- Use features requiring unavailable MCP servers
- Ignore MCP-specific configurations when servers are present
- Continue using MCP tool if it returns an error (report error to user and use alternative)

---

## Session Start Protocol

**At the START of every session, YOU MUST:**

1. **Check MCP tools** - Identify available `mcp__*` patterns in function list
2. **Read `./meta.yaml`** - If exists, extract `codename` and `user` for Yggdrasil tagging
3. **Apply MCP configurations** - Load relevant configs from MCP Server Configurations section
4. **Recall context** - If Yggdrasil available, check for relevant memories for current project

</critical>

---

# General Preferences

## Communication Style

**No fixed preference - adapt to context:**

- User can specify style using `/style` command
- Default to balanced approach (neither too concise nor too verbose)
- Adjust based on task complexity and user feedback

---

## Code Architecture & Quality

**Architectural Preferences:**

- **Object-Oriented Programming (OOP)** as primary paradigm
- **Microservices architecture** when applicable
- **Scalability** - design with growth in mind
- Consider performance implications
- Plan for maintainability and extensibility

**Code Quality Standards:**

- Write clean, readable code
- Follow SOLID principles
- Use meaningful names for classes, methods, variables
- Keep methods focused and single-purpose
- Design interfaces before implementation

**Code Organization:**

- Proper separation of concerns
- Clear module boundaries
- Dependency injection where appropriate
- Avoid tight coupling between services

---

## Testing Approach

**IMPORTANT - No Automatic Testing:**

**NEVER:**

- Create test files automatically
- Write tests during development
- Suggest adding tests
- Include testing in development workflow

**Reasoning:**
Development happens in phases:

1. **First:** Development (code implementation)
2. **Then:** Testing (after development complete)
3. **Finally:** Documentation (after everything works)

User will explicitly request testing when development phase is complete.

---

## Development Workflow

**Before Making Changes:**

1. Read and understand existing code
2. Identify all affected files
3. Consider impact on related functionality
4. Plan approach with OOP/microservices principles

**During Changes:**

- Maintain architectural consistency
- Follow existing patterns and conventions
- Preserve existing functionality unless explicitly changing it
- Design for scalability

**After Changes:**

- Verify changes work as expected
- Check for unintended side effects
- Consider scalability implications

---

## Git Operations

**CRITICAL - Always Ask Before Git Actions:**

**YOU MUST:**

- **Always ask** before creating commits
- **Always ask** before pushing to remote
- **Always ask** before any git operation that modifies history
- Let user decide when commits happen

**NEVER:**

- Create commits automatically
- Push without explicit permission
- Use git commands with `-i` flag (interactive not supported)
- Amend commits without asking
- Rebase or modify git history without permission
- Use `--force` flags

**User Controls Git History:**

- User decides commit timing
- User decides commit messages
- User decides when to push
- AI executes git commands only when explicitly instructed

---

## Web Research Protocol

**CRITICAL - Always Research to Prevent Hallucinations:**

**YOU MUST:**

- **ALWAYS** perform web research to verify information
- **NEVER** rely solely on training data for current information
- Check latest documentation and best practices
- Verify technology versions and compatibility
- Ensure information is current and accurate

**When to Research (ALWAYS):**

- User asks about any technology, library, or framework
- Implementing features with external dependencies
- Error messages or debugging scenarios
- Best practices and recommended patterns
- Version-specific functionality
- API usage and documentation
- Any information that may have changed since training cutoff

**Research Tools Priority:**

1. **If Context7 MCP available:** Use for library/framework documentation
2. **If Argus MCP available:** Use as default for general web research
3. **If Exa MCP available:** Use when user explicitly requests Exa
4. **Built-in WebSearch/WebFetch:** Use as fallback

**Research Process:**

1. Identify what needs verification
2. Use available search tools
3. Check official documentation first
4. Verify information currency (dates, versions)
5. Cross-reference multiple sources if contradictory
6. Provide citations/sources to user

**Goal:**

- Stay up-to-date with latest information
- Prevent hallucinations
- Provide accurate, verified information
- Build trust through factual responses

---

# MCP Server Configurations

<mcp_configs>

**Instructions:**
Review descriptions of detected MCP server tools to understand their purpose.
Apply workflows below ONLY when corresponding MCP server is available.

---

## Context7 MCP

**Trigger:** `mcp__context7__*` tools detected

**Purpose:** Up-to-date documentation and code examples for libraries

**YOU MUST:**

- `resolve-library-id` FIRST → get Context7 library ID
- Select by: snippet count + reputation + benchmark score
- `get-library-docs` with `topic` for focused results
- Use `mode="info"` for conceptual/architectural questions
- Paginate (`page=2,3...`) if insufficient

**Key Pattern:**

1. `resolve-library-id("next.js")` → `/vercel/next.js`
2. `get-library-docs("/vercel/next.js", topic="server actions")`

**When NOT available:** Use Argus/Exa for web research

---

## Sequential Thinking MCP

**Trigger:** `mcp__sequentialthinking__sequentialthinking` tool detected

**Purpose:** Structured problem-solving through dynamic thought chains

**When to Use:**

- Complex architectural decisions
- Multi-step debugging
- Trade-off analysis between approaches
- Planning large implementations

**Key:** Can revise thoughts, branch into alternatives, adjust total steps dynamically

**When NOT available:** Break down problems explicitly in conversation

---

## Serena MCP

**Trigger:** `mcp__serena__*` tools detected

**Purpose:** Semantic code analysis, symbol-based navigation and editing (30+ languages)

**CRITICAL:** NEVER read entire files - use symbolic tools for token-efficient exploration

**YOU MUST:**

- `activate_project` first with project path
- `get_symbols_overview` BEFORE reading files
- `find_symbol` with `include_body=False` first, then `True` for specific code
- `think_about_task_adherence` BEFORE code edits
- Prefer `replace_symbol_body` over line-based editing

**Key Patterns:**

- Overview → `get_symbols_overview("path/file.py")`
- Find → `find_symbol("MyClass/method", include_body=True)`
- Edit → `replace_symbol_body("MyClass/method", "path/file.py", new_code)`
- References → `find_referencing_symbols("symbol", "path/file.py")`

**When NOT available:** Fall back to standard Read/Edit/Write + Grep/Glob

---

## Chrome DevTools MCP

**Trigger:** `mcp__chrome-devtools__*` tools detected

**Purpose:** Browser automation, testing, debugging, performance analysis

**YOU MUST:**

- Use `take_snapshot` (a11y tree) over `take_screenshot` for interactions
- Use `list_pages` → `select_page` before operations
- Reference elements by `uid` from snapshot in all interaction tools
- Use `wait_for` to ensure page ready before interactions

**Workflow:**

1. `list_pages` → `new_page`/`select_page`
2. `take_snapshot` → get element UIDs
3. `click`/`fill`/`hover` using UIDs
4. `wait_for` → confirm result
5. `list_console_messages` → check errors

**Performance:** `performance_start_trace` → `performance_stop_trace` → `performance_analyze_insight`

**When NOT available:** Provide manual testing instructions

---

## Exa MCP

**Trigger:** `mcp__exa__*` tools detected

**Purpose:** Premium web search and code context (GitHub, Stack Overflow, docs)

**CRITICAL - Usage Priority:**

- **User says "use Exa":** Use Exa → Argus → WebSearch
- **User doesn't mention Exa:** Use Argus → WebSearch (DO NOT use Exa)
- **Reason:** Exa is premium - use intentionally, not for every query

**Tools:**

- `get_code_context_exa` ⭐ - Real code from GitHub, implementation patterns
- `web_search_exa` - Real-time web search
- `deep_search_exa` - Complex research with query expansion

**When NOT available:** Fall back to Argus MCP or WebSearch

---

## Argus MCP

**Trigger:** `mcp__Argus__*` tools detected

**Purpose:** DEFAULT web research (Brave Search API wrapper)

**YOU MUST:** Use Argus as default for web research when user doesn't specify Exa

**Tools by Content Type:**

- `search_web` - General research (use `result_filter="web"` for ~3k tokens)
- `search_web_extra_snippets_for_ai` - Deep research (6x more context)
- `search_news` - Current events, breaking news
- `search_videos` - Tutorials, video content
- `search_images` - Visual content, diagrams

**Freshness:** `"pd"` (day), `"pw"` (week), `"pm"` (month), `"py"` (year)

**When NOT available:** Fall back to built-in WebSearch/WebFetch

---

## Yggdrasil MCP (Semantic Memory Server)

**Trigger:** `mcp__Yggdrasil__*` tools detected

**Purpose:** Persistent semantic memory with AI embeddings (Chroma Cloud)

**Project Identification via meta.yaml:**

At session start, check for `./meta.yaml` in project root:

```yaml
# Required fields for Yggdrasil tagging:
codename: ProjectName # → tag: project:ProjectName
user: Username # → tag: user:Username

# Optional (used in metadata):
team: TeamName
related_projects: [...]
```

**If meta.yaml exists:** Use `codename` and `user` for automatic tagging
**If meta.yaml missing:** Use `user:default_user` and `project:default_project`

**YOU MUST:**

- Read `meta.yaml` at session start (if exists)
- Tag all memories with `user:{user}` and `project:{codename}`
- Include optional fields (`team`, `related_projects`) in metadata
- Use `search_memories` for semantic search (finds by meaning)
- Use `recall_memory` for time-based queries ("yesterday", "last week")

**When to Save Memories:**

_Decisions & Solutions:_

- Important decisions (architectural, technological)
- Solutions to complex problems
- Trade-offs considered and why chosen
- Rejected approaches and why they failed

_User & Project Context:_

- User preferences discovered (code style, tools, workflow)
- Project-specific conventions
- Team/collaboration context

_Technical Knowledge:_

- Working configurations (env, setup, dependencies)
- Bug fixes with root cause explanation
- API endpoints, integrations (never secrets!)
- Version-specific quirks and behaviors

_Project Progress:_

- Architecture decisions with rationale
- Feature implementation details
- TODO items for future sessions
- Blockers and dependencies

_Session Context:_

- Session summaries (on user request)
- Context needed for future sessions
- User's explicit "remember this" requests

**Tagging Pattern:**

```
save_memory(
  content="...",
  tags=["user:John", "project:Alpha", "team:Personal"],
  metadata={"codename": "Alpha", "related_projects": ["Beta", "Gamma"]}
)
```

**Key Patterns:**

- Semantic: `search_memories("database optimization")` → finds by meaning
- Time-based: `recall_memory("what did we discuss yesterday")` → time-filtered
- By project: `search_by_tag(["project:Alpha"])` → project-specific
- By user: `search_by_all_tags(["user:John", "project:Alpha"])` → user+project

**When NOT available:** Session-only context, no persistence

</mcp_configs>

---

# CRITICAL RULES REMINDER

<reminder priority="HIGH">

Before every response, verify you are following these core principles:

## ✓ Language Compliance

- [ ] Code, comments, documentation → **English only**
- [ ] User communication → **Same language user is using**
- [ ] No language mixing in technical content

## ✓ Scope Discipline

- [ ] Changing **ONLY** what user explicitly requested
- [ ] No unsolicited refactoring or "improvements"
- [ ] Ask before making additional changes

## ✓ File Creation Restraint

- [ ] Using Read/Edit/Write tools directly (not artifacts)
- [ ] Editing existing files (not creating new ones)
- [ ] No documentation files without explicit request

## ✓ MCP Detection

- [ ] Checked available MCP tools at session start
- [ ] Applied conditional configurations for detected servers

## ✓ Project Identification

- [ ] Checked for `meta.yaml` at session start
- [ ] Using correct `user:` and `project:` tags for Yggdrasil

## ✓ Git Protocol

- [ ] Asked user before creating any commits
- [ ] Asked user before pushing to remote
- [ ] Never using --force or --amend without permission

## ✓ Web Research

- [ ] Performed web research to verify current information
- [ ] Used available MCP search tools (Argus/Exa) when appropriate
- [ ] Checked official documentation for accuracy

## ✓ Testing Discipline

- [ ] Not creating test files automatically
- [ ] Not suggesting tests during development phase
- [ ] Waiting for explicit user request to add tests

**Remember:** Stay within boundaries. Ask when uncertain. Research to prevent hallucinations.

</reminder>
