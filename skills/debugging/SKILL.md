# Debugging Skill

## Purpose
Provide systematic debugging methodology for all types of bugs: runtime errors, test failures, logic bugs, performance issues, and production incidents.

## Core Methodology

### Five-Step Universal Framework
1. **REPRODUCE** - Make the bug happen reliably
2. **ISOLATE** - Narrow down where the problem is  
3. **DIAGNOSE** - Understand why it's happening
4. **FIX** - Implement the solution
5. **VERIFY** - Confirm fix works and doesn't break anything

This applies to every debugging scenario without exception.

## Runtime Detection Pattern

Projects use different runtimes (npm, bun, deno). Always detect before running commands:

```bash
if [ -f "bun.lockb" ]; then
  # bun project
elif [ -f "deno.json" ] || [ -f "deno.lock" ]; then
  # deno project
else
  # npm/pnpm/yarn project
fi
```

## Debugging Scenarios

### Runtime Errors (Crashes/Exceptions)
**Reproduce**: Capture full error with stack trace and context  
**Isolate**: Identify exact line and values at failure  
**Diagnose**: Type error, logic error, or environment issue?  
**Fix**: Add validation, handle error case, fix root cause  
**Verify**: Add test case for the error scenario

### Test Failures
**Reproduce**: Run test in isolation with verbose output  
**Isolate**: Read failure message, check expected vs actual  
**Diagnose**: Async issues, mock problems, environment differences, shared state  
**Fix**: Fix implementation or test, add better assertions  
**Verify**: Run test multiple times to ensure stability

### Logic Bugs (Wrong Behavior)
**Reproduce**: Create minimal reproduction with input/output comparison  
**Isolate**: Add console.logs/breakpoints at each step  
**Diagnose**: Use scientific method - form hypothesis, test it  
**Fix**: Off-by-one, wrong condition, state mutation, edge cases  
**Verify**: Add test for the specific broken case

### Performance Issues
**Reproduce**: Measure with console.time or performance.now  
**Isolate**: Use profiler, binary search slow sections  
**Diagnose**: N+1 queries, large data, sync ops, memory leaks, inefficient algorithms  
**Fix**: Optimize algorithm (O(n²) → O(n)), add caching, parallelize  
**Verify**: Benchmark before and after

### Production Issues
**Reproduce**: Check logs, try to reproduce with production-like conditions  
**Isolate**: Identify differences between local and production  
**Diagnose**: Review logs, check monitoring, verify external services  
**Fix**: Add defensive code, graceful degradation, retry logic  
**Verify**: Deploy to staging, test with production-like data, monitor closely

## Key Tools

### Browser DevTools
- `debugger` - Stop execution
- Conditional breakpoints - Break only when condition true
- `console.table()` - Format objects
- `console.trace()` - Show call stack
- Network tab - Check requests/responses

### Git Bisect
Find exactly when a bug was introduced by binary searching commit history.

### Type Debugging (TypeScript)
```typescript
type Debug<T> = T extends infer U ? U : never;
const test: never = value; // Force error to see inferred type
```

### Binary Search Debugging
Comment out half the code. Bug still happens? It's in the other half. Repeat.

## Common Anti-Patterns

❌ **Random changes** without hypothesis  
✅ Form hypothesis based on evidence, test it

❌ **No reproduction** before fixing  
✅ Create minimal test case first

❌ **Masking errors** with empty catch blocks  
✅ Log errors, handle gracefully

❌ **No documentation** of the fix  
✅ Add test, update docs if significant

❌ **Runtime assumptions** (always using npm)  
✅ Detect runtime first

## Integration Points

### With Work Records
Document debugging sessions in work records:
- What was the bug?
- How did you find it?
- What was the fix?
- What did you learn?

### With Testing
Every fixed bug should get a test to prevent regression.

### With ADRs
Significant debugging decisions (architecture changes to fix performance, switching algorithms) deserve ADRs.

### With Evidence Tracking
Debugging demonstrates:
- **S7**: Problem solving and debugging techniques
- **B2**: Logical thinking in diagnosis
- **K9**: Understanding algorithms when optimizing

## Debugging Checklist

**Before:**
- [ ] Can you reproduce it?
- [ ] Captured error/stack trace?
- [ ] Know when it started?

**During:**
- [ ] Isolated problem area?
- [ ] Checked assumptions?
- [ ] Reviewed recent changes?
- [ ] Documenting attempts?

**After:**
- [ ] Root cause fixed?
- [ ] Test added?
- [ ] Nothing else broken?
- [ ] Solution documented?

## Runtime-Specific Commands

### Deno
```bash
deno test --watch path/to/test.ts
deno run --inspect-brk script.ts  # Debug with Chrome DevTools
```

### Bun
```bash
bun test --watch path/to/test.ts
bun --inspect script.ts  # Debug
bun --hot script.ts      # Hot reload
```

### npm
```bash
npm test -- --watch path/to/test.ts
node --inspect-brk script.js  # Debug
```

## Reference Document

Full debugging patterns documented in: `~/.claude/debugging-workflow-patterns.md`

## When to Apply

This skill auto-invokes when conversation includes:
- "bug", "error", "broken", "failing", "crash"
- "test failing", "test won't pass"
- "slow", "performance", "timeout"
- "production issue", "only happens in prod"
- "TypeError", "undefined", "null"
- "works locally but not in production"

Always apply the five-step framework regardless of bug type.
