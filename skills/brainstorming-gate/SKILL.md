---
name: brainstorming-gate
description: This skill should be used when the user requests a new feature, asks to "build", "create", "add", "implement", or "design" something, proposes a significant change to existing functionality, or when the conversation involves architectural decisions, new abstractions, or unfamiliar territory. Prevents premature implementation by enforcing a think-first gate with graduated response based on complexity.
version: 1.0.0
---

# Brainstorming Gate

Hard gate on premature implementation. No code until the problem is understood and the approach is agreed. The size of the gate scales with the complexity of the request.

This skill codifies the CLAUDE.md rules:
- **"Ambiguous requirements: Ask first"**
- **"Multiple valid approaches: Present options briefly, recommend one"**
- **"Clear intent, unclear implementation: Make a reasonable call, flag assumptions"**

---

## When This Skill Applies

Fires on any request to build something new or make a significant change. The response is **graduated** — not every request needs a full design session.

### Triage: Which Gate Level?

Assess the request against these criteria:

| Criterion | Quick Check | Full Gate |
|-----------|------------|-----------|
| Files affected | 1-2 files | 3+ files |
| Ambiguity | Clear intent, clear approach | Unclear intent OR multiple valid approaches |
| Novelty | Pattern exists in codebase | No existing pattern to follow |
| Reversibility | Easy to undo | Hard to undo (schema changes, API contracts) |
| Risk | Low blast radius | Touches shared code, public APIs, or data |

**Quick Check** (1-2 criteria on the left): Proceed to Quick Check Gate.
**Full Gate** (2+ criteria on the right): Proceed to Full Brainstorming Gate.

---

## Quick Check Gate

For simple, well-understood requests. Takes 30 seconds, not 5 minutes.

### The Three Sentences

Before writing any code, state:

1. **What**: One sentence describing what will change
2. **How**: One sentence describing the approach
3. **Assumption**: One sentence flagging the key assumption being made

```
What: Adding a `lastLogin` field to the user profile display.
How: Read from the existing `users.last_login_at` column, format with date-fns, render in ProfileCard.
Assumption: The column is already populated by the auth layer — not verifying that here.
```

Then proceed. If the user corrects an assumption, adjust before coding.

---

## Full Brainstorming Gate

For complex, ambiguous, or high-risk requests. No code until the gate clears.

### Phase 1: Context Exploration

Before proposing anything, understand the landscape:

1. **Read relevant code** — don't guess at the current implementation
2. **Identify existing patterns** — how does the codebase handle similar things?
3. **Surface constraints** — what's non-negotiable? (Types, conventions, performance, etc.)
4. **Ask clarifying questions** — max 3-4 targeted questions, not a questionnaire

**Questions should be specific**, not open-ended:
```
✗ "What are your requirements?"
✗ "How should this work?"

✓ "Should this endpoint require authentication, or is it public?"
✓ "The existing UserService uses class-based DI — should the new service follow that pattern or is this a good time to migrate to functions?"
✓ "There are two valid schemas for this: normalised (3 tables, complex joins) or denormalised (1 table, some duplication). Which matters more here — query simplicity or data integrity?"
```

### Phase 2: Approach Proposals

Present **2-3 approaches** with trade-offs. Not 5. Not 1. Recommend one.

Format:

```
## Approaches

### A: [Name] ← Recommended
[2-3 sentences describing the approach]
+ [Key advantage]
+ [Second advantage]
- [Key trade-off]

### B: [Name]
[2-3 sentences describing the approach]
+ [Key advantage]
- [Key trade-off]
- [Second trade-off]

### C: [Name] (only if genuinely distinct)
[2-3 sentences]
+ [advantage]
- [trade-off]

**Recommendation**: A, because [one sentence justification].
```

**Rules for approaches:**
- Each must be genuinely distinct, not minor variations
- Trade-offs must be honest — don't sandbag alternatives to make your recommendation look better
- If there's truly only one sensible approach, say so and explain why

### Phase 3: Design Presentation

Once an approach is selected, present the design in sections for approval:

1. **Data model** (if applicable) — entities, relationships, types
2. **Interface** — function signatures, API shape, component props
3. **Flow** — how data moves through the system
4. **Edge cases** — the 2-3 most likely failure modes and how they're handled

Present each section, get a nod, move to the next. Don't dump the entire design at once.

### Phase 4: Execution Handoff

Once the design is approved:
- Summarise what was agreed in a compact plan
- Hand off to the subagent-executor skill for disciplined execution
- Or proceed directly for smaller implementations (3 steps or fewer)

---

## Anti-Rationalisation Table

| Excuse | Rebuttal |
|--------|----------|
| "This is straightforward, I'll just build it" | If it were straightforward, explaining the approach takes 10 seconds. Do that first. |
| "The user wants speed, not a design session" | Users want working software. Five minutes of design prevents two hours of rework. |
| "I already know how to do this" | Then describing the approach is trivial. Write it down. |
| "There's really only one way to do this" | Then saying so takes one sentence. Say it and proceed. |
| "The user will tell me if it's wrong" | The user shouldn't have to catch your mistakes. Think first. |
| "Let me just get something working and iterate" | Iteration on a bad foundation is demolition and rebuild. Get the foundation right. |
| "I'll refactor later" | "Later" is a lie agents tell themselves. Do it properly now or scope it down. |

---

## What This Skill Is NOT

This is **not** a bureaucratic checkpoint designed to slow everything down. It's a thinking gate.

- It does NOT require formal documents
- It does NOT require lengthy write-ups for simple changes
- It does NOT prevent "just trying something" during a spike (see scope-coach's Spike First pattern)
- It DOES require stating your approach before writing code
- It DOES require surfacing assumptions
- It DOES require presenting options when multiple valid approaches exist

The graduated response means simple things stay simple. Only complex things get the full treatment.

---

## Integration Points

### With scope-coach
Brainstorming may reveal scope creep. Scope coach fires when the design starts expanding: "You're designing 3 features. Which one ships first?"

### With implementation-planner agent
For complex features, the brainstorming gate's output becomes the implementation planner's input. Brainstorming decides *what* and *roughly how*; the planner produces the detailed *step-by-step*.

### With subagent-executor
Once brainstorming concludes and a plan exists, the executor takes over. The brainstorming output is the spec that the executor's review gates check against.

### With domain-modeller
Phase 3's data model section should invoke domain-modeller thinking. Entity maps, relationship diagrams, boundary identification.

### With ethics-reviewer
Phase 1's context exploration should include an ethical sniff test. If the feature involves user data, behavioural nudges, or accessibility-impacting changes, flag it early.

---

## Success Criteria

The brainstorming gate is working when:
- Simple requests proceed quickly with a brief sanity check
- Complex requests get proper design attention before code is written
- Approaches are presented with honest trade-offs
- The user feels informed, not interrogated
- Rework caused by "just started building" drops significantly
- Assumptions are surfaced and validated before they become bugs
