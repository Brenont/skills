You are conducting comprehensive research to understand the codebase before making changes.

**RESEARCH TOPIC:** $ARGUMENTS

**CRITICAL RULES:**
- DO NOT suggest improvements or write code
- DO NOT perform root cause analysis unless explicitly asked
- ONLY describe what exists, where it exists, and how it works
- Focus on creating a technical map of the existing system

**STEP 0: Check for Existing Research (Avoid Duplication)**

Before starting new research, check if relevant research already exists:

```bash
mkdir -p thoughts/shared/research thoughts/shared/plans thoughts/shared/handoffs thoughts/local/notes

if [ -d "thoughts/shared/research" ] && [ "$(ls -A thoughts/shared/research 2>/dev/null)" ]; then
  echo "🔍 Checking for existing research on: $ARGUMENTS"
  echo ""
  
  # Search by keywords in arguments
  KEYWORDS=$(echo "$ARGUMENTS" | tr '[:upper:]' '[:lower:]' | tr ' ' '|')
  
  # Find relevant files
  FOUND_FILES=$(grep -l -i -E "$KEYWORDS" thoughts/shared/research/*.md 2>/dev/null || echo "")
  
  if [ ! -z "$FOUND_FILES" ]; then
    echo "📚 Found potentially relevant research:"
    echo "$FOUND_FILES" | while read file; do
      echo ""
      echo "File: $file"
      grep "^topic:" "$file" 2>/dev/null | head -1
      echo "Summary:"
      sed -n '/## Summary/,/## Detailed Findings/p' "$file" 2>/dev/null | head -6
      echo "---"
    done
    echo ""
    echo "💡 RECOMMENDATION: Review existing research before continuing."
    echo "   - To view: cat [filename]"
    echo "   - To supplement: /research-supplement [filename] [new-questions]"
    echo "   - To start fresh: Continue with this command"
    echo ""
    read -p "Continue with new research? (y/n): " CONTINUE
    if [ "$CONTINUE" != "y" ]; then
      echo "Stopping. Review existing research first."
      exit 0
    fi
  else
    echo "✓ No existing research found for this topic. Starting fresh..."
  fi
else
  echo "✓ No research directory yet. Starting fresh..."
fi
```

**STEP 1: Conduct Research**

Research topic: $ARGUMENTS

1. Understand the user's question/feature request
2. Use search tools extensively to explore relevant components
3. Document findings with file references (file.ext:line)
4. Map out how components interact
5. Note any patterns or conventions

Search strategies:
- Search for relevant file names
- Search for function/class names
- Search for keywords from the question
- Look at imports/dependencies
- Check test files for usage examples

**STEP 2: Create Research Document**

After completing research:

```bash
TIMESTAMP=$(date +%Y%m%d-%H%M%S)
# Create short slug from topic for filename
TOPIC_SLUG=$(echo "$ARGUMENTS" | tr '[:upper:]' '[:lower:]' | tr ' ' '-' | tr -cd '[:alnum:]-' | cut -c1-30)
RESEARCH_FILE="thoughts/shared/research/research-${TOPIC_SLUG}-${TIMESTAMP}.md"

cat > ${RESEARCH_FILE} << 'RESEARCH_EOF'
---
date: $(date -Iseconds)
topic: "$ARGUMENTS"
keywords: [keyword1, keyword2, keyword3]
status: complete
related_research: []
---

# Research: $ARGUMENTS

## Research Question
$ARGUMENTS

## Summary
[High-level findings in 2-3 paragraphs maximum]
[What exists, how it works, key patterns found]

## Detailed Findings

### Component 1: [Name/Description]
- **Location**: `path/to/file.ts:123-145`
- **Purpose**: [What this component does]
- **Key Functions/Classes**: [List main functions/classes]
- **Connections**: [How it relates to other components]
- **Implementation Notes**: [Important details]

### Component 2: [Name/Description]  
- **Location**: `path/to/file.ts:200-250`
- **Purpose**: [What this component does]
- **Key Functions/Classes**: [List main functions/classes]
- **Connections**: [Dependencies and related code]

### Component 3: [Name/Description]
[Continue for all relevant components found]

## Architecture Insights
[Patterns, conventions, and design decisions discovered]
[Any notable architectural approaches]

## Code References
Quick reference of all relevant files:
- `path/to/file1.ts:50` - [Brief description]
- `path/to/file2.ts:120-150` - [Brief description]
- `path/to/file3.test.ts:30` - [Test examples]

## Data Flow
[If relevant, describe how data flows through the system]
[Input → Processing → Output]

## Configuration & Setup
[Any configuration files, environment variables, or setup requirements]

## Testing Strategy
[How this code is currently tested]
[Test files and patterns]

## Known Limitations or Issues
[Any edge cases, TODOs, or known issues discovered]

## Next Steps for Planning
[What needs to be considered when planning implementation]
[Potential impact areas]
[Dependencies to watch out for]

RESEARCH_EOF

echo ""
echo "✅ Research Complete"
echo "📄 Saved to: ${RESEARCH_FILE}"
echo ""
```

**STEP 3: Index This Research**

Add metadata to help future searches:

```bash
# Create a research index if it doesn't exist
INDEX_FILE="thoughts/shared/research/INDEX.md"

if [ ! -f "$INDEX_FILE" ]; then
  cat > "$INDEX_FILE" << 'INDEX_EOF'
# Research Index

Quick reference of all research documents.

## Documents

INDEX_EOF
fi

# Add this research to the index
cat >> "$INDEX_FILE" << EOF

### $(basename ${RESEARCH_FILE})
- **Topic**: $ARGUMENTS
- **Date**: $(date +%Y-%m-%d)
- **Keywords**: [extract from frontmatter]
- **Path**: ${RESEARCH_FILE}

EOF

echo "✓ Updated research index"
```

**STEP 4: Report to User**

Provide clear summary and next steps:

```
✅ Research Complete: $ARGUMENTS

📊 Coverage:
- Found [N] relevant components
- Documented [N] files
- Identified [N] key patterns

📄 Research Document:
${RESEARCH_FILE}

🔍 Key Findings:
[2-3 bullet points of most important discoveries]

💾 Context Check:
[Show current context percentage]

📋 Next Steps:
1. Review the research document
2. Ready to plan? Run: /plan ${RESEARCH_FILE}
3. Need more detail? Run: /research-supplement ${RESEARCH_FILE} "your questions"

[If context > 60%: Recommend /clear before planning]
```

**CONTEXT MANAGEMENT:**
- Check /context after research - should be under 60%
- If over 60%, save and /clear before moving to planning
- The research file is your source of truth
- Chat history is ephemeral, files persist

**REUSABILITY:**
This research document can be:
- Referenced in future related work
- Supplemented with additional findings
- Combined with other research documents
- Used as-is for planning