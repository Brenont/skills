You are implementing ONE PHASE of a plan at a time.

**INPUT FORMAT:**
Usage: /implement <plan-file-path> Phase <N>
Example: /implement thoughts/shared/plans/plan-auth-20241206.md Phase 1

**PARSE ARGUMENTS:**
Plan file: (first argument)
Phase: (extract number from "Phase N")

**STEP 1: Read the Plan**
```bash
cat [plan-file-path]
```

Read the ENTIRE plan to understand context, then focus on the specified phase.

**STEP 2: Pre-Implementation Check**
- Read the plan to understand the phase's goals
- Check dependencies (are previous phases complete?)
- **CRITICAL: Identify the research document referenced in the plan**
- Review research document sections relevant to this phase
- Understand the verification steps

**Before implementing, load the research context:**
```bash
# Extract research file path from plan frontmatter
RESEARCH_FILE=$(grep "^research_doc:" [plan-file-path] | cut -d' ' -f2-)

# Read the relevant sections from research
echo "📖 Loading research context for this phase..."
cat "${RESEARCH_FILE}"
```

**Use the research document to guide implementation:**
- File paths are documented in research
- Component structure is documented in research
- Patterns and conventions are documented in research
- Don't search unnecessarily - reference the research

**STEP 3: Implement the Phase**

Follow the plan's "Changes" section exactly:
1. Make the specified modifications
2. Follow the order listed in the plan
3. Use the file paths and line numbers as guidance
4. If you discover the plan needs adjustment, STOP and tell the user

**STEP 4: Verify Your Work**

Execute ALL verification steps from the plan:
```bash
# Run tests specified in the plan
npm test [specific-test-file]

# Run type checking if specified
npm run typecheck

# Run linting if specified
npm run lint
```

**STEP 5: Update the Plan File**

Mark the phase as complete:
```bash
# Update the phase status in the plan file
sed -i 's/\*\*Status\*\*: ⏳ Pending/\*\*Status\*\*: ✅ Complete/' [plan-file-path]

# Or manually update the specific phase section to show:
**Status**: ✅ Complete (implemented [timestamp])
```

**STEP 6: Report to User**

Provide a clear summary:
```
✅ Phase [N] Complete: [Phase Name]

Changes Made:
- Modified path/to/file1.ts
- Updated path/to/file2.ts
- Added new functionality X

Verification:
✓ All tests passed
✓ Type checking clean
✓ Linting passed

Next Step:
Ready for Phase [N+1]. Run: /implement [plan-file] Phase [N+1]
```

**CRITICAL RULES:**
- Implement ONLY the specified phase
- Do NOT move ahead to the next phase
- If tests fail, debug and fix before marking complete
- If you need to deviate from the plan, ask the user first
- Stay focused on the phase goals

**Context Management:**
- Check /context after reading the plan
- If context > 60%, consider if you need to /clear and reload just what's needed
- After phase completion and verification, recommend /clear before next phase

**Error Handling:**
If something goes wrong:
1. Don't mark the phase complete
2. Report the issue clearly
3. Ask user if plan needs adjustment
4. Don't proceed to next phase