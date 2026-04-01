You are creating a detailed, phase-by-phase implementation plan based on research findings.

**INPUT REQUIRED:**
Research document path: $ARGUMENTS
(Example: thoughts/shared/research/research-20241206-143022.md)

**CRITICAL: Use Research, Don't Re-Search**
The research document already contains all the codebase knowledge you need. DO NOT search the codebase again. Your job is to synthesize the research into an actionable plan.

**STEP 1: Read Research Document Thoroughly**
```bash
cat $ARGUMENTS
```

Read and internalize the ENTIRE research document:
- What components exist (file paths, functions, classes)
- How they connect together
- Patterns and conventions
- Testing approaches
- Known limitations

**All the information you need is in the research document. Reference it, don't re-search it.**

**STEP 2: Create Implementation Plan**

Think through:
- What are the distinct phases of work?
- What files need to be modified in each phase? (FROM RESEARCH DOC)
- What are the dependencies between phases?
- How will we verify each phase?

**IMPORTANT: All file paths, function names, and architectural details should come FROM THE RESEARCH DOCUMENT. Do not search the codebase - the research already did that work.**

When referencing code:
- Use the file paths documented in the research
- Reference the components identified in the research  
- Build on the architecture insights from the research
- Use the testing patterns noted in the research

**STEP 3: Write Plan File**

Create a detailed plan file:

```bash
TIMESTAMP=$(date +%Y%m%d-%H%M%S)
TOPIC="[short-topic-name]"  # Extract from research or ask user
cat > thoughts/shared/plans/plan-${TOPIC}-${TIMESTAMP}.md << 'EOF'
---
date: $(date -Iseconds)
topic: "[Feature/Task Name]"
research_doc: "[path to research file]"
status: ready
---

# Implementation Plan: [Feature Name]

## Goal
[1-2 sentence description of what we're implementing]

## Research Summary
[Key findings from research that inform this plan]

## Implementation Phases

### Phase 1: [Name - e.g., "Core Data Model Changes"]
**Status**: ⏳ Pending

**Files to Modify:**
- `path/to/file1.ts` (lines 50-75)
- `path/to/file2.ts` (lines 120-150)

**Changes:**
1. Modify function `doSomething()` to accept new parameter `xyz`
2. Update interface `SomeInterface` to include new field `abc: string`
3. Add validation logic in `validateInput()` function

**Testing/Verification:**
- [ ] Run unit tests: `npm test path/to/file1.test.ts`
- [ ] Verify types: `npm run typecheck`
- [ ] Manual check: [specific behavior to verify]

**Success Criteria:**
- All existing tests pass
- No type errors
- [Specific functionality works]

**Estimated Complexity**: Low/Medium/High

---

### Phase 2: [Name - e.g., "API Integration"]
**Status**: ⏳ Pending

**Dependencies**: Phase 1 must be complete

**Files to Modify:**
- `path/to/api.ts` (lines 200-250)
- `path/to/handler.ts` (new file)

**Changes:**
1. Create new API endpoint `/api/xyz`
2. Implement request handler with error handling
3. Add authentication middleware

**Testing/Verification:**
- [ ] Run integration tests: `npm test integration`
- [ ] Manual API test: `curl -X POST ...`
- [ ] Check error handling: [test error cases]

**Success Criteria:**
- API responds with correct status codes
- Auth is enforced
- Error handling works

---

### Phase 3: [Name]
**Status**: ⏳ Pending

**Dependencies**: Phases 1, 2

**Files to Modify:**
[List files]

**Changes:**
[Detailed changes]

**Testing/Verification:**
[Specific tests]

**Success Criteria:**
[How we know it works]

---

## Risks and Considerations
- [Potential issue 1]
- [Potential issue 2]

## Overall Verification
After all phases:
- [ ] Full test suite passes: `npm test`
- [ ] Linting passes: `npm run lint`
- [ ] Type checking passes: `npm run typecheck`
- [ ] Manual end-to-end test: [describe]

## Notes
[Any other important information]
EOF
```

**STEP 4: Review with User**

After creating the plan, show the user:
1. The plan file path
2. Number of phases
3. Ask: "Does this plan look reasonable? Should I adjust any phases?"

**Important:**
- Plans should have 3-7 phases typically
- Each phase should be completable in < 30 minutes
- Each phase must have clear verification steps
- Spend time getting this right - a bad plan = bad code

**Context Management:**
- /clear after creating the plan
- The plan file is now your source of truth