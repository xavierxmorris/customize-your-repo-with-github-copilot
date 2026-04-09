# File-Based Instructions

[← Always-On Instructions](primitive-1-always-on-instructions.md) | [Part II Overview](part-2-primitives.md)

---

## Overview

File-based instructions activate through glob pattern matching via the `applyTo` frontmatter field, or through semantic matching of the `description` field to the current task. This allows teams to define area-specific rules that only apply when working in relevant contexts.

**Location:** `.github/instructions/*.instructions.md`

Configure additional search paths with the `chat.instructionsFilesLocations` setting to load instruction files from outside `.github/instructions/` — useful for monorepos or shared instruction libraries.

**Official docs:** [Custom instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)

**See it in action:** For a live demo, watch Courtney Webster in [Customize Your Agents](https://www.youtube.com/watch?v=flpKLkZla2Q).

**Use Cases:**
- Different conventions for frontend vs. backend code
- Specialized rules for test files
- Language-specific guidelines in a monorepo
- Framework-specific patterns for different modules

## File Format

File-based instructions use the `.instructions.md` extension with YAML frontmatter:

| Field | Description |
|-------|-------------|
| `description` | Description shown on hover in Chat view; also used for semantic matching to the current task |
| `name` | Display name (defaults to filename) |
| `applyTo` | Glob pattern for automatic application |

## Example

**File:** `.github/instructions/api-routes.instructions.md`

``````markdown
---
name: 'API Route Guidelines'
description: 'Conventions for REST API endpoints'
applyTo: 'src/api/**/*'
---

# Instructions for API Routes

## API-Specific Rules
- All routes must include authentication middleware
- Use standardized error response format
- Include OpenAPI documentation comments
- Rate limiting is required for public endpoints

## Response Format
All API responses must use this shape:
```typescript
interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: { code: string; message: string; };
}
```

## Error Handling
```typescript
// ✅ Good: Standardized error response
return NextResponse.json({ 
  success: false, 
  error: { code: 'NOT_FOUND', message: 'Resource not found' }
}, { status: 404 });

// ❌ Bad: Inconsistent error format
return NextResponse.json({ error: 'Not found' }, { status: 404 });
```
``````

## Creating File-Based Instructions

1. In the Chat view, click the **gear icon** (Configure Chat)
2. Select an option that opens the instruction file picker
3. Choose **New instruction file...** from the picker
4. Select **Workspace** to store in `.github/instructions/`
5. Use the agent to generate context-specific rules

Alternatively, ask the agent directly:

> **💬 Try this prompt:**
>
> *Create a file-based instruction at .github/instructions/react-components.instructions.md that applies to src/components/\*\*/\* and includes our React component conventions. Analyze existing components for patterns to document.*

## Anti-Patterns to Avoid

| Anti-Pattern | Why It's Problematic | Better Approach |
|--------------|---------------------|------------------|
| **Overlapping applyTo patterns** | Multiple instructions may conflict | Use specific, non-overlapping patterns |
| **Duplicating always-on rules** | Redundancy, potential conflicts | Only include area-specific additions |
| **Too broad patterns** | Instructions apply unexpectedly | Use precise glob patterns |
| **No description** | Hard to understand what rules apply | Add clear description in frontmatter |

File-based instructions complement always-on instructions by providing contextual specialization.

### CLI Support

[GitHub Copilot CLI](https://docs.github.com/en/copilot/concepts/agents/about-copilot-cli) supports path-specific instruction files (`.github/instructions/**/*.instructions.md`). When working in a repository from the terminal, the CLI loads matching instructions the same way VS Code does, ensuring consistent context across surfaces.

### File-Based Instructions vs. Skills

File-based instructions and skills have overlapping use cases. Both can provide context-specific knowledge to Copilot — file-based instructions activate by file pattern, skills activate by description matching.

**Quick guidance:**
- **File-based instructions**: Best for rules tied to specific file locations that won't be reused elsewhere
- **Skills**: Best for reusable knowledge that applies across multiple contexts, or when you need supporting files (templates, scripts)

There's no definitively "right" choice — this space is still evolving. For a detailed exploration of when to use each, see [Skills vs. File-Based Instructions: Overlapping Territory](primitive-4-skills.md#skills-vs-file-based-instructions-overlapping-territory).

---

[← Always-On Instructions](primitive-1-always-on-instructions.md) | [Next: Prompt Files →](primitive-3-prompts.md)
