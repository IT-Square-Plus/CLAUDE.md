# 📋 CLAUDE.md - Optimized Configuration for Claude Code

> **Purpose:** Optimized instruction file for Claude Code CLI
> **Version:** 1.0
> **Last Updated:** November 2025

---

## What is this?

This project contains a carefully designed `CLAUDE.md` file - a special configuration that Claude Code automatically loads at startup. It serves as executable specifications and behavioral instructions that shape Claude's behavior across all sessions.

**The problem we solved:** Default Claude Code behavior lacks project-specific context, consistent rules enforcement, and awareness of available MCP (Model Context Protocol) servers.

**Our solution:** A structured, research-backed CLAUDE.md that:

- Enforces critical rules consistently
- Adapts dynamically to available MCP tools
- Maintains optimal size for attention patterns
- Uses the "Sandwich Pattern" for maximum rule retention

---

## Why a New CLAUDE.md?

### The Attention Problem

Claude's attention mechanism for system prompts (like CLAUDE.md) follows specific patterns:

```
╔═══════════════════════════════════════════════════════╗
║ Beginning of file    → HIGHEST attention (5/5)        ║
║ Middle of file       → MEDIUM attention  (3/5)        ║
║ End of file          → HIGH attention.   (4/5)        ║
╚═══════════════════════════════════════════════════════╝
```

**Key insight:** This is NOT the same as conversation history where recent = highest priority. For system prompts, the **beginning** has highest priority.

**Important:** We don't use CLAUDE.md as memory or history storage. We have dedicated tools for that - [Yggdrasil](https://github.com/IT-Square-Plus/Yggdrasil) (Semantic Memory Server) and other MCP Memory servers<sup>[1](#footnote-mcp-memory-servers)</sup> handle persistent context. CLAUDE.md should contain **instructions and rules**, not accumulated knowledge.

### The Size Problem

Research and testing show:

- **Recommended:** 500-600 lines maximum
- **Beyond that:** Attention degrades, rules get "forgotten"
- **Solution:** Concise sections, references to detailed configs when needed

Our CLAUDE.md is optimized for attention without losing essential information.

---

## The Sandwich Pattern Explained

We use a specific priority structure called the "Sandwich Pattern":

```
┌─────────────────────────────────────────────────────┐
│ PRIORITY #1 - CRITICAL RULES                        │ Rating: ⭐⭐⭐⭐⭐
│ Lines 1-120: Non-negotiable foundation              │
│ • Language requirements                             │
│ • Code modification boundaries                      │
│ • File creation constraints                         │
│ • MCP server awareness                              │
│ • Session start protocol                            │
├─────────────────────────────────────────────────────┤
│ PRIORITY #2 - GENERAL PREFERENCES                   │ Rating: ⭐⭐⭐⭐
│ Lines 124-282: Standard operating procedures        │
│ • Communication style                               │
│ • Code architecture                                 │
│ • Testing approach                                  │
│ • Git operations                                    │
│ • Web research protocol                             │
├─────────────────────────────────────────────────────┤
│ PRIORITY #4 - MCP CONFIGURATIONS.                   │ Rating: ⭐⭐⭐ ← WHY HERE?
│ Lines 285-515: Conditional tool configurations      │
│ • Context7, Sequential Thinking, Serena             │
│ • Chrome DevTools, Exa, Argus, Yggdrasil            │
├─────────────────────────────────────────────────────┤
│ PRIORITY #3 - CRITICAL REMINDER                     │ Rating: ⭐⭐⭐⭐
│ Lines 519-574: Reinforcement of top rules           │
│ • Checklist format                                  │
│ • Repeats Priority #1 rules                         │
└─────────────────────────────────────────────────────┘
```

### Why MCP Configs are in the Middle (Priority #4)?

**Question:** Why not put MCP configurations higher since they're important?

**Answer:** Three reasons:

1. **Conditional Nature**

   - MCP configs only apply when specific servers are detected
   - Not every session needs every MCP config
   - Placing them in "CRITICAL" would waste highest-attention space on potentially irrelevant content

2. **Deterministic Logic**

   - MCP detection is clear: tool pattern exists → apply config
   - No ambiguity requiring high attention
   - Clear trigger conditions (`mcp__servername__*`) are self-explanatory

3. **Reference Material**
   - MCP configs are "consulted when needed" not "memorized always"
   - Middle position is perfect for reference material
   - Claude checks MCP tools at session start, then applies relevant configs

**The rule:** Critical non-negotiable rules go at beginning and end. Conditional/reference material goes in the middle.

### Why Repeat Rules at the End?

The "Sandwich Pattern" places critical rules at BOTH beginning AND end:

```
Beginning: DEFINE the rules (highest authority)
Middle: Details, examples, conditional configs
End: REINFORCE the rules (recency within system prompt)
```

**Important:** The end is NOT higher priority than the beginning. It's **reinforcement through repetition**, like a pilot's checklist before takeoff - the flight manual (beginning) has authority, but the checklist (end) ensures nothing is forgotten.

**Analogy:**

```
Teacher at start: "Today's 3 key concepts: A, B, C"
[45 minutes of detailed lesson]
Teacher at end: "Remember: A, B, C were the key points"
```

The end doesn't override the beginning - it reinforces it.

---

## MCP Conditional Loading

CLAUDE.md files are static Markdown - they don't support IF-ELSE statements. But Claude is intelligent enough to:

1. Check which tools are available (sees `mcp__servername__*` in function list)
2. Interpret natural language conditional instructions
3. Apply only relevant sections based on detected tools

### Our Approach

```markdown
## Yggdrasil MCP (Semantic Memory Server)

**Trigger:** `mcp__Yggdrasil__*` tools detected

[Configuration only applies when trigger condition is met]

**When NOT available:** Session-only context, no persistence
```

Each MCP section includes:

- **Trigger:** What tool pattern activates this config
- **Purpose:** What the MCP provides
- **YOU MUST:** Required behaviors when MCP is available
- **Key Patterns:** Usage examples
- **When NOT available:** Fallback behavior

This allows one CLAUDE.md to work across different environments with different MCP servers installed.

---

## XML-like Tags for Semantic Grouping

You'll notice our CLAUDE.md uses XML-like tags:

```markdown
<critical priority="HIGHEST">
...critical rules here...
</critical>

<mcp_configs>
...MCP configurations here...
</mcp_configs>

<reminder priority="HIGH">
...reminder checklist here...
</reminder>
```

### Why XML Tags?

**1. Semantic Boundaries**

XML tags create clear, unambiguous section boundaries. Unlike Markdown headers (`##`) which are hierarchical but flat, XML tags explicitly mark where a logical block **starts** and **ends**.

```
## Section A        ← Where does Section A end?
content...
## Section B        ← Here? Or was there supposed to be more?

<section_a>         ← Section A starts here
content...
</section_a>        ← Section A definitively ends here
<section_b>         ← Section B starts here
```

**2. Priority Attributes**

XML allows attributes that convey metadata:

```markdown
<critical priority="HIGHEST">
```

This tells Claude not just "this is critical" but **how critical** - the `priority="HIGHEST"` attribute reinforces importance in a machine-parseable way.

**3. LLM Pattern Recognition**

Large Language Models are trained on vast amounts of structured data including:

- HTML/XML documents
- Configuration files
- API responses

Claude recognizes XML patterns and understands:

- `<tag>...</tag>` = contained, related content
- Attributes like `priority="HIGH"` = metadata about the content
- Nested structures = hierarchical relationships

**4. Hybrid Markdown + XML**

We use **both** Markdown and XML strategically:

| Element          | Format         | Reason                           |
| ---------------- | -------------- | -------------------------------- |
| Headers          | Markdown `##`  | Human readability, navigation    |
| Content          | Markdown       | Formatting, lists, code blocks   |
| Logical sections | XML tags       | Clear boundaries, priority hints |
| Priority markers | XML attributes | Machine-parseable importance     |

**Example from our CLAUDE.md:**

```markdown
# 🔴 CRITICAL RULES (NON-NEGOTIABLE) ← Markdown for human readers

<critical priority="HIGHEST"> ← XML for semantic grouping

## Language Requirements ← Markdown for structure

**Code & Technical Content:** ← Markdown for formatting

- **ALWAYS** use English for:
  - All code
    ...

</critical> ← Clear end of critical section
```

### Tags We Use

| Tag                             | Purpose                        | Location                |
| ------------------------------- | ------------------------------ | ----------------------- |
| `<critical priority="HIGHEST">` | Non-negotiable rules           | Beginning (Priority #1) |
| `<mcp_configs>`                 | Conditional MCP configurations | Middle (Priority #4)    |
| `<reminder priority="HIGH">`    | Rule reinforcement checklist   | End (Priority #3)       |

### Why Not Pure Markdown?

Pure Markdown lacks:

- **Explicit end markers** - `##` starts sections but doesn't end them
- **Attribute support** - no way to add `priority="HIGHEST"` metadata
- **Semantic grouping** - headers are hierarchical, not grouped

Pure XML would lack:

- **Human readability** - harder to scan and edit
- **Rich formatting** - lists, bold, code blocks are verbose in XML

**Hybrid approach** gives us best of both worlds.

---

## Project Identification with meta.yaml

Instead of inline tags (`#user=John #project=Alpha`), we use a structured approach:

### meta.yaml in project root:

```yaml
codename: ProjectName # Required - used for tagging
user: Username # Required - used for tagging
team: TeamName # Optional - stored in metadata
related_projects: # Optional - stored in metadata
  - Alpha
  - Beta
  - Enigma
```

### How it works:

1. **Session Start:** Claude checks for `./meta.yaml` in project root
2. **If exists:** Uses `codename` and `user` for automatic Yggdrasil tagging
3. **If missing:** Falls back to `user:default_user` and `project:default_project`

### Benefits:

- No need to remember tags in every message
- Consistent tagging across sessions
- Project metadata stored alongside code
- Team collaboration context available

---

## File Structure

```
~/.claude/
├── CLAUDE.md              # Main configuration (this is the CLAUDE.md from this repo!).
└── settings.json          # Claude Code global user settings.

project-root/
├── meta.yaml              # Project identification for Yggdrasil.
├── .mcp.json              # MCP server configurations.
├── .claude/
│   └── settings.local.json    # Project-specific settings.
└── CLAUDE.md              # Optional project-specific overrides.
```

---

## Key Design Decisions

### 1. Size Optimization

**Why:** Files over 600 lines show degraded attention to middle content.

**How:**

- Removed verbose tool descriptions (Claude sees them in function list)
- Focused on HOW and WHEN, not WHAT exists
- Used concise patterns instead of exhaustive documentation

**Example - avoid listing all MCP Server Tools (Claude knows what these tools do thanks to Tool Descriptions):**

```markdown
## Yggdrasil MCP

### Available Tools:

- save_memory - Store new information...
- get_memory - Retrieve specific memory...
  [list of 27 tools with descriptions]
```

**Example - instead tell Claude how to behave in certain moments and situations:**

```markdown
## Yggdrasil MCP (Semantic Memory Server)

**Trigger:** `mcp__Yggdrasil__*` tools detected
**YOU MUST:**

- Read meta.yaml at session start
- Tag memories with user:{user} and project:{codename}
- Use search_memories for semantic search
  [Key patterns and when to save]
```

### 2. Sandwich Priority Structure

**Why:** Beginning and end of system prompts get highest attention.

**How:**

- Critical rules at beginning (Priority #1)
- General preferences next (Priority #2)
- MCP configs in middle (Priority #4)
- Critical reminder at end (Priority #3)

### 3. MCP Conditional Loading

**Why:** Different environments have different MCP servers.

**How:**

- Each MCP section has clear trigger condition
- Fallback behavior defined for when MCP unavailable
- Detection protocol at session start

### 4. meta.yaml Integration

**Why:** Consistent project identification without inline tags.

**How:**

- Structured YAML file in project root
- Required fields: `codename`, `user`
- Optional fields stored in Yggdrasil metadata

### 5. Memory Guidelines (When to Save)

**Why:** Claude needs guidance on what's worth persisting.

**Categories:**

- Decisions & solutions
- User & project context
- Technical knowledge
- Project progress
- Session context

---

## Usage

### Installation

1. Copy `CLAUDE.md` to `~/.claude/CLAUDE.md`:

   ```bash
   cp CLAUDE.md ~/.claude/CLAUDE.md
   ```

2. Create `meta.yaml` in your project root:

   ```yaml
   codename: MyProject
   user: YourName
   team: Personal
   ```

3. Configure MCP servers in `.mcp.json` (project root or `~/.claude/`):
   ```json
   {
     "mcpServers": {
       "Yggdrasil": {
         "type": "http",
         "url": "http://localhost:8080/mcp-project"
       }
     }
   }
   ```

---

## Testing Your Configuration

### Verification Checklist

```
✓ Structure Test:
  [ ] Critical rules in first 100 lines?
  [ ] MCP configs in middle section?
  [ ] Reminder in last 60 lines?
  [ ] Total under 600 lines?

✓ Content Test:
  [ ] Priority #1 rules are truly non-negotiable?
  [ ] MCP sections have clear triggers?
  [ ] Reminder repeats Priority #1 rules?

✓ Effectiveness Test:
  [ ] Start new Claude session
  [ ] Ask "What are your critical rules?"
  [ ] Verify Priority #1 rules mentioned first
  [ ] Test MCP detection with available servers
```

---

## Contributing

When modifying CLAUDE.md:

1. **Keep size under 600 lines**
2. **Maintain sandwich structure** (Priority #1 → #2 → #4 → #3)
3. **Update both beginning AND end** for critical rules
4. **Test with actual Claude Code session**
5. **Ask Claude Code directly** - Start a conversation and ask: `Can you read my CLAUDE.md and tell me if it's clear and well-structured? What works well and what could be improved?`
6. **Document changes in your own research files**

---

## Credits

Special thanks to [Jakub Gościniak](https://github.com/jgosciniak) for inspiring this project through our discussions about optimized CLAUDE.md structure and attention patterns.

---

## References

- [Anthropic Claude Code Documentation](https://docs.anthropic.com/claude-code)
- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)

---

## 📚 Footnotes

### <a name="footnote-mcp-memory-servers">[1]</a> MCP Memory Servers

Available MCP Memory servers for persistent context storage:

- **[Yggdrasil](https://github.com/IT-Square-Plus/Yggdrasil)** - Production-ready MCP memory server with Chroma Cloud vector storage (Recommended)
- **[MCP Memory Service](https://github.com/doobidoo/mcp-memory-service)** - Original MCP memory implementation by Heinrich Krupp
- **[Memory MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/memory)** - Official Anthropic MCP memory server
- **Browse more:** [MCP Servers Directory](https://github.com/modelcontextprotocol/servers) - Official collection of MCP servers
