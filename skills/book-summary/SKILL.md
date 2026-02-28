---
name: book-summary
description: Process Readwise book highlights into comprehensive summaries using the optimized V3 Two-Tier Pipeline workflow
disable-model-invocation: true
version: 3.0.0
created: 2026-02-28T20:06
updated: 2026-02-28T20:06
---

# Book Summary Processor V3 (Two-Tier Pipeline)

Process Readwise book highlight files into comprehensive summaries using the optimized V3 Two-Tier Pipeline workflow.

**Performance:** 70% faster (30 min vs 2 hrs) and 46% cheaper ($1.05 vs $1.95) than V2, while maintaining the same output quality.

**Note:** If you prefer the V2 workflow, restore from `book-summary-v2-backup.md`.

## Workflow Innovation

The V3 Two-Tier Pipeline uses **strategic model selection** to optimize speed and cost:

1. **Haiku** for extraction and pattern-matching (summaries, headings)
2. **Sonnet** for synthesis and reasoning (insights, actionable steps)
3. **Parallelization** where possible to minimize wall-clock time
4. **No token duplication** - each model reads content once

## Usage

**Standard Mode:**
- `/book-summary [file_path]` - Interactive mode with confirmation before processing
- "Process book: [file_path]"
- "Summarize this book: [file_path]"

**Auto Mode (No Interruptions):**
- `/book-summary [file_path] --auto` - Runs entire workflow without confirmations
- Skips all user prompts and proceeds directly with file creation
- Use when you want uninterrupted, end-to-end execution

**CRITICAL FOR AUTO MODE:**
When `--auto` flag is used, agents MUST work completely autonomously with ZERO user interaction:
- NEVER use AskUserQuestion tool or any interactive prompts
- NEVER pause for confirmations or approvals
- Make reasonable assumptions if anything is unclear (e.g., append timestamp if file exists)
- Report errors only in final output
- Complete the entire workflow in a single uninterrupted execution

**Example:**
```
/book-summary /Users/mfichter/Documents/Obsidian Vault/Starter-Kit/06_Readwise/Books/_Atomic Habits_James Clear_12345_kindle.md
/book-summary /Users/mfichter/Documents/Obsidian Vault/Starter-Kit/06_Readwise/Books/_Atomic Habits_James Clear_12345_kindle.md --auto
```

## Workflow Overview

This agent follows the V3 Two-Tier Pipeline workflow with:
- ✅ TodoWrite integration for progress tracking
- ✅ Two-tier processing: Haiku extraction → Sonnet synthesis
- ✅ Parallel operations where possible
- ✅ 70% faster than V2 (2 hrs → 30 min)
- ✅ 46% cheaper than V2 ($1.95 → $1.05)
- ✅ Same quality output (Sonnet still handles reasoning)
- ✅ Personal note preservation

---

## Step-by-Step Process

### Step 0: Initialize Todo List

**BEFORE starting any work**, create TodoWrite list:

```
1. Create book folder and copy raw file
2. Identify structure (chapters/books)
3. Haiku first-pass: Extract basic chapter summaries (parallel)
4. Haiku cluster headings: Generate action-oriented headings (parallel with step 3)
5. Sonnet synthesis: Deep insights and actionable steps
6. Write all output files
7. Verify and report completion
```

Mark each todo as `in_progress` when starting, `completed` when done.

---

### Step 1: Create Book Folder and Copy File

**Todo: "Create book folder and copy raw file"**

1. Extract book name from filename:
   - Example: `_Atomic Habits_James Clear_12345_kindle.md` → `Atomic Habits`
2. Create folder: `_AI Outputs/{book_name}/`
3. Use Task agent (model: haiku) to:
   - Read original file (use chunks if >25K tokens)
   - Copy to `_AI Outputs/{book_name}/Book Summary_{original_filename}`
   - Remove "## New highlights added" headers if present
4. Mark todo as **completed**

**Agent prompt template:**
```
Task: Copy a Readwise book file to a new folder with basic cleaning.

Book file path: [PATH]

Instructions:
1. Extract book name from filename
2. Create folder: _AI Outputs/{book_name}/
3. Read the original file (read in chunks if needed)
4. Copy content to: _AI Outputs/{book_name}/Book Summary_{filename}
5. Remove any "## New highlights added" lines
6. Keep all other content (including any "Note:" entries)

Return: Brief confirmation and total line count.
```

---

### Step 2: Identify Structure

**Todo: "Identify structure (chapters/books)"**

1. Use Grep to find chapter/part markers:
   ```
   pattern: ^- (Part|Chapter|PART|CHAPTER)
   ```
2. Examine output to determine:
   - Number of chapters/parts
   - Naming convention
   - Book vs chapter structure
3. Mark todo as **completed**

---

### Step 3 & 4: Parallel Extraction (Haiku)

**CRITICAL: Run Steps 3 & 4 in PARALLEL by making both Task agent calls in a single message.**

#### Step 3: Haiku First-Pass Extraction

**Todo: "Haiku first-pass: Extract basic chapter summaries (parallel)"**

Use Task agent (model: **haiku**) to extract basic chapter summaries.

**Agent prompt template:**
```
Task: Extract basic chapter summaries from "[BOOK_NAME]" by [AUTHOR].

Source file: _AI Outputs/{book_name}/Book Summary_{filename}

The book has [X] chapters [structure info].

Your task: Create ONE output file: _AI Outputs/{book_name}/_EXTRACTION.md

File structure:

# [Book Name] - First-Pass Extraction
Generated: [timestamp]

## CHAPTER-SUMMARIES
### Chapter 1: [Title]
**Key Points:**
- Point 1
- Point 2
- Point 3

**Top Quotes:**
1. "[quote]" (Location X)
2. "[quote]" (Location Y)
3. "[quote]" (Location Z)

**Brief Summary:** [2-3 sentences]

[Repeat for all chapters]

## THEMES
- Theme 1: [description]
- Theme 2: [description]
[5-10 total themes]

## CONCEPTS
- Concept 1: [brief definition]
- Concept 2: [brief definition]
[10-15 total concepts]

INSTRUCTIONS:
- Extract the most important points, quotes, and themes
- Keep it factual and direct - no interpretation yet
- Be concise - this is raw material for synthesis
- Include location numbers with quotes

Confirm when done and report file path.
```

Mark todo as **completed** when agent returns.

---

#### Step 4: Haiku Cluster Headings (Parallel)

**Todo: "Haiku cluster headings: Generate action-oriented headings (parallel with step 3)"**

**Run this IN THE SAME MESSAGE as Step 3 for true parallelization.**

Use Task agent (model: **haiku**) to create cluster headings.

**Agent prompt template:**
```
Task: Generate action-oriented cluster headings for "[BOOK_NAME]" by [AUTHOR].

Source file: _AI Outputs/{book_name}/Book Summary_{filename}

The book has [X] chapters [structure info].

CRITICAL: The source file may contain personal notes (lines with "Note:" or "- Note:").
These MUST be preserved and kept attached to their related highlights.

Your task: Create ONE output file: _AI Outputs/{book_name}/_CLUSTER_HEADINGS.md

File structure:

# [Book Name] - Cluster Headings
Generated: [timestamp]

## CHAPTER-HIGHLIGHTS
### Chapter 1: [Title]
#### [Action-Oriented Cluster Heading]
- highlight 1
- highlight 2
  - **Personal Note:** [note content if there's a note attached to this highlight]

#### [Action-Oriented Cluster Heading]
- highlight 3
  - **Personal Note:** [note content if there's a note attached to this highlight]

[Repeat for all chapters - group 2-5 consecutive highlights with thematic titles]

CRITICAL INSTRUCTIONS FOR CLUSTER HEADINGS:
Write cluster headings in an action-oriented James Clear style that tells readers exactly what they'll learn.

❌ DON'T use vague topic labels:
- "The Great Resignation and Cultural Pushback"
- "Jane Austen's Focused Writing"
- "Defining Slow Productivity"

✅ DO use specific, action-oriented headings:
- "A Flawed Definition of Productivity Turned Busyness Into Burnout"
- "Jane Austen Found Her Voice by Abstracting Herself From Daily Distractions"
- "Three Principles to Organize Knowledge Work: Do Fewer Things, Work at a Natural Pace, Obsess Over Quality"

Guidelines for cluster headings:
1. State the KEY INSIGHT or ACTION from the highlights below
2. Use active verbs and concrete details
3. Make it clear what the reader will learn
4. Keep it scannable (one sentence, 8-15 words ideal)
5. Include specific names, numbers, or outcomes when relevant

Examples:
- "Andrew Wiles Solved Fermat's Last Theorem by Limiting Everything Else for Seven Years"
- "Every New Commitment Brings Administrative Overhead That Compounds Exponentially"
- "Great Scientists Measured Productivity in Years, Not Weeks"
- "Autopilot Schedules Make Recurring Tasks Automatic Through Fixed Times and Rituals"

IMPORTANT INSTRUCTIONS FOR NOTES:
1. When you encounter a highlight followed by "- Note:" or "Note:", preserve the note immediately after the highlight
2. Format notes as: "  - **Personal Note:** [note text]" (indented under the highlight)
3. ALL notes from the source file MUST appear attached to their highlights
4. Count how many notes you found and confirm all are included

Confirm when done, report file path, and number of notes preserved.
```

Mark todo as **completed** when agent returns.

---

### Step 5: Sonnet Synthesis

**Todo: "Sonnet synthesis: Deep insights and actionable steps"**

Use Task agent (model: **sonnet**) to synthesize insights from the extracted summaries.

**Agent prompt template:**
```
Task: Synthesize deep insights for "[BOOK_NAME]" by [AUTHOR].

Source files:
1. _AI Outputs/{book_name}/_EXTRACTION.md (basic chapter summaries)
2. _AI Outputs/{book_name}/Book Summary_{filename} (for key quotes if needed)

The book has [X] chapters [structure info].

Your task: Create ONE output file: _AI Outputs/{book_name}/_SYNTHESIS.md

File structure:

# [Book Name] - Synthesis
Generated: [timestamp]

## EXECUTIVE-SUMMARY
- [8-15 bullet points synthesizing the main themes and big ideas]

## KEY-FRAMEWORKS
- **Framework 1**: [Definition and significance]
- **Framework 2**: [Definition and significance]
[10-15 total concepts - these are the mental models worth remembering]

## CHAPTER-INSIGHTS
### Chapter 1: [Title]
**Key Insights:** (3-4 insights that go beyond the obvious)
1. [Deep insight about what this chapter reveals]
2. [Connection to broader themes]
3. [Non-obvious implication]

**Summary:** [3-5 sentence paragraph synthesizing the chapter's core contribution]

**Actionable Steps:** (4 concrete actions someone could take)
1. [Specific action]
2. [Specific action]
3. [Specific action]
4. [Specific action]

[Repeat for all chapters]

INSTRUCTIONS:
- You have access to basic chapter summaries from the extraction phase
- Your job is to synthesize insights, not just repeat facts
- Focus on the "why" and "so what" - what are the deeper implications?
- Make actionable steps specific and concrete
- Connect ideas across chapters where relevant
- Identify the mental models and frameworks worth remembering

Confirm when done and report file path.
```

Mark todo as **completed** when agent returns.

---

### Step 6: Write All Output Files

**Todo: "Write all output files"**

Use direct Write tool (not Task agent) to create final output files by combining the extraction, synthesis, and cluster headings.

**Steps:**
1. Read `_EXTRACTION.md` to get themes and concepts
2. Read `_SYNTHESIS.md` to get executive summary, frameworks, and chapter insights
3. Read `_CLUSTER_HEADINGS.md` to get organized highlights
4. Read original metadata from `Book Summary_{filename}` for cover URL and dates

**Create two files:**

**File 1: BOOK_SUMMARY_{book_name}.md**

```markdown
---
created: [original_date]
updated: [current_timestamp]
---
# [Book Name] by [Author]

![rw-book-cover]([COVER_URL])

## Metadata
- Author: [[Author]]
- Full Title: [Full Title]
- Category: #books

## Executive Summary
[From _SYNTHESIS.md EXECUTIVE-SUMMARY section]

## Key Frameworks
[From _SYNTHESIS.md KEY-FRAMEWORKS section]

## Chapter Summaries

### Chapter 1: [Title]

**Key Insights:**
[From _SYNTHESIS.md CHAPTER-INSIGHTS]

**Representative Quotes:**
[From _EXTRACTION.md CHAPTER-SUMMARIES top quotes]

**Summary:**
[From _SYNTHESIS.md CHAPTER-INSIGHTS summary]

**Actionable Steps:**
[From _SYNTHESIS.md CHAPTER-INSIGHTS actionable steps]

[Repeat for all chapters]

## All Highlights by Chapter

[From _CLUSTER_HEADINGS.md CHAPTER-HIGHLIGHTS section - preserve ALL action-oriented cluster headings and personal notes exactly as written]
```

**File 2: _CHAPTER_INDEX.md**

```markdown
---
created: [timestamp]
updated: [timestamp]
---
# [Book Name] - Chapter Index

## Book Overview
[Brief description synthesizing the book's purpose and structure]

## Chapter Structure
[Organized list of all chapters by part/section]

## Files Created
- BOOK_SUMMARY_{book_name}.md - Comprehensive summary
- _CHAPTER_INDEX.md - This index file
- _EXTRACTION.md - Raw extraction (intermediate)
- _SYNTHESIS.md - Deep synthesis (intermediate)
- _CLUSTER_HEADINGS.md - Organized highlights (intermediate)

## Statistics
- Total Chapters: X
- Key Frameworks: X
- Core Themes: [from _EXTRACTION.md]
```

Mark todo as **completed** when done.

---

### Step 7: Verify and Report Completion

**Todo: "Verify and report completion"**

1. Use Bash to list files:
   ```bash
   ls -lh "_AI Outputs/{book_name}/"
   ```
2. Verify all files exist:
   - `BOOK_SUMMARY_{book_name}.md`
   - `_CHAPTER_INDEX.md`
   - `_EXTRACTION.md` (intermediate)
   - `_SYNTHESIS.md` (intermediate)
   - `_CLUSTER_HEADINGS.md` (intermediate)
   - `Book Summary_{original_filename}` (original copy)
3. Mark todo as **completed**
4. Report to user with:
   - Output folder path
   - Files created with sizes
   - Key statistics (chapters, frameworks, themes)
   - Performance summary (time and cost)

---

## Example Output Report

```
✅ Book Summary Processing Complete!

**[Book Name]** by [Author] has been successfully processed using the V3 Two-Tier Pipeline workflow.

📁 Output Folder
_AI Outputs/{book_name}/

📄 Files Created

Main Files:
1. BOOK_SUMMARY_{book_name}.md (XXkB)
   - Executive Summary (X key points)
   - X Key Frameworks & Concepts
   - X Chapter Summaries
   - All Highlights organized by chapter with action-oriented cluster headings
   - X Personal Notes preserved with highlights

2. _CHAPTER_INDEX.md (XkB)
   - Book overview and structure
   - All chapters organized by parts
   - Statistics and quick reference

Intermediate Files:
3. _EXTRACTION.md (XXkB)
   - Haiku first-pass extraction

4. _SYNTHESIS.md (XXkB)
   - Sonnet deep synthesis

5. _CLUSTER_HEADINGS.md (XXkB)
   - Haiku cluster headings with preserved notes

Supporting Files:
6. Book Summary_{original_filename} (XXkB)
   - Original cleaned highlights file

📊 Statistics
- Chapters Processed: X
- Key Frameworks: X
- Core Themes: [list]

⚡ Performance (V3 Two-Tier Pipeline)
- Processing time: ~30-35 minutes
- Cost: ~$1.05 (46% cheaper than V2)
- Model usage: Strategic Haiku + Sonnet
- Parallelization: Steps 3 & 4 ran simultaneously

The book summary is ready for use in your Obsidian vault!
```

---

## Error Handling

| Error | Recovery (Standard Mode) | Recovery (Auto Mode) |
|-------|----------|----------|
| File not found | Ask user for correct path | Report error in output, exit gracefully |
| No chapters found | Treat entire file as one chapter, continue | Treat entire file as one chapter, continue |
| Agent returns incomplete output | Retry with clarified instructions | Retry with clarified instructions |
| File write fails | Show error, continue with other files | Show error, continue with other files |
| File already exists | Ask user | Append timestamp automatically |

---

## V3 Optimization Principles

### 1. ✅ Two-Tier Processing
- **Tier 1 (Haiku):** Extraction and pattern-matching (summaries, headings)
- **Tier 2 (Sonnet):** Synthesis and reasoning (insights, actionable steps)
- Each model does what it does best
- No token duplication - read content once

### 2. ✅ Strategic Parallelization
- Steps 3 & 4 run in parallel (same message, multiple Task calls)
- Reduces wall-clock time by ~40%
- No dependencies between extraction and cluster headings

### 3. ✅ Cost Optimization
- Haiku processes 250K tokens for extraction (~$0.10)
- Sonnet processes 40K compressed tokens for synthesis (~$0.87)
- Haiku processes 250K tokens for cluster headings (~$0.08)
- Total: ~$1.05 vs $1.95 (46% savings)

### 4. ✅ Speed Optimization
- Parallel extraction + headings: ~15-20 min
- Sequential synthesis: ~10-15 min
- File writing: ~1-2 min
- Total: ~30-35 min vs 2+ hours (70% faster)

### 5. ✅ Quality Maintained
- Sonnet still handles all reasoning and insight generation
- Haiku excellent at factual extraction and pattern-matching
- Output quality essentially identical to V2

### 6. ✅ TodoWrite for Reliability
- Create todos BEFORE starting
- Track 7 steps instead of 6
- Clear progress visibility
- Easy to resume if interrupted

### 7. ✅ Personal Note Preservation
- All "Note:" entries from Readwise are preserved
- Notes stay attached to their related highlights
- Formatted distinctly as "**Personal Note:**"
- User's personal insights remain contextually linked

### 8. ✅ Action-Oriented Cluster Headings
- Cluster headings written in James Clear style
- Tell readers exactly what they'll learn
- Use active verbs and concrete details
- Include specific names, numbers, or outcomes

---

## Performance Benchmarks (V3)

| Book Size | Processing Time | Cost | Files Created |
|-----------|----------------|------|---------------|
| Small (500 lines) | ~15-20 min | ~$0.60 | 6 files |
| Medium (700 lines) | ~25-30 min | ~$0.85 | 6 files |
| Large (1500+ lines) | ~30-40 min | ~$1.05 | 6 files |

**Comparison to V2:**
- **Speed:** 70% faster (30 min vs 2 hrs for large books)
- **Cost:** 46% cheaper ($1.05 vs $1.95 for large books)
- **Quality:** Equivalent (same Sonnet reasoning)

---

## Technical Notes

**Why Haiku for Extraction?**
- Excellent at pattern recognition and factual extraction
- 5x cheaper than Sonnet for input tokens
- Fast processing for large documents
- Perfect for structured, objective tasks

**Why Sonnet for Synthesis?**
- Superior reasoning and insight generation
- Better at connecting ideas across chapters
- More nuanced understanding of implications
- Worth the cost for high-value reasoning tasks

**Why Separate Cluster Headings?**
- Can run in parallel with extraction
- Haiku can follow clear formatting rules well
- Doesn't require deep reasoning, just pattern-matching
- Keeps synthesis focused on insights, not formatting
