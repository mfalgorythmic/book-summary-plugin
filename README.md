---
created: 2026-02-28T20:06
updated: 2026-02-28T20:08
---
# Book Summary Plugin for Claude Code

> Transform Readwise book highlights into comprehensive, actionable summaries using AI-powered two-tier processing.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude_Code-Plugin-blue)](https://claude.ai/code)

## Overview

The Book Summary Plugin processes Readwise book highlight files into comprehensive summaries using an optimized **V3 Two-Tier Pipeline** that strategically combines Haiku (for extraction) and Sonnet (for synthesis).

**Key Features:**
- 📚 **Executive summaries** with 8-15 key points
- 🧠 **Key frameworks** and mental models (10-15 concepts)
- 📖 **Chapter-by-chapter summaries** with insights, quotes, and actionable steps
- ✨ **Action-oriented cluster headings** (James Clear style)
- 📝 **Personal notes preservation** from Readwise
- ⚡ **70% faster** and **46% cheaper** than traditional approaches
- 🔄 **Auto mode** for uninterrupted processing

## Performance

| Metric | V3 Two-Tier Pipeline | Traditional Approach |
|--------|---------------------|---------------------|
| **Processing Time** | 30-35 min | 2+ hours |
| **Cost** | ~$1.05 | ~$1.95 |
| **Speed Improvement** | 70% faster | baseline |
| **Cost Savings** | 46% cheaper | baseline |
| **Output Quality** | Same (Sonnet synthesis) | Same |

## Installation

### Option 1: From Claude Marketplace (Recommended)

```bash
/plugin install book-summary
```

### Option 2: From GitHub

```bash
/plugin marketplace add mfalgorythmic/book-summary-plugin
/plugin install book-summary
```

### Option 3: Manual Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/mfalgorythmic/book-summary-plugin.git
   ```

2. Install the plugin:
   ```bash
   cd book-summary-plugin
   claude --plugin-dir .
   ```

## Usage

### Basic Usage

```bash
/book-summary /path/to/readwise/highlights.md
```

### Auto Mode (No Interruptions)

```bash
/book-summary /path/to/readwise/highlights.md --auto
```

Auto mode runs the entire workflow without confirmations—perfect for batch processing multiple books.

### Examples

**Standard mode with confirmation:**
```bash
/book-summary ~/Obsidian/06_Readwise/Books/_Atomic Habits_James Clear_12345_kindle.md
```

**Auto mode for uninterrupted processing:**
```bash
/book-summary ~/Obsidian/06_Readwise/Books/_Atomic Habits_James Clear_12345_kindle.md --auto
```

**Natural language:**
```
Process book: ~/Obsidian/06_Readwise/Books/_Thinking in Systems_Donella Meadows_67890_kindle.md
```

## What You Get

The plugin creates 6 files in `_AI Outputs/{book_name}/`:

### Main Output Files

1. **BOOK_SUMMARY_{book_name}.md** - Comprehensive summary with:
   - Executive summary (8-15 key points)
   - Key frameworks and mental models (10-15 concepts)
   - Chapter summaries with insights, quotes, and actionable steps
   - All highlights organized by chapter with action-oriented headings
   - Personal notes preserved with highlights

2. **_CHAPTER_INDEX.md** - Quick reference with:
   - Book overview and structure
   - Complete chapter list
   - Statistics and metadata

### Intermediate Files (For Reference)

3. **_EXTRACTION.md** - Haiku first-pass extraction
4. **_SYNTHESIS.md** - Sonnet deep synthesis
5. **_CLUSTER_HEADINGS.md** - Organized highlights with cluster headings
6. **Book Summary_{original_filename}** - Cleaned copy of original

## How It Works

The V3 Two-Tier Pipeline uses **strategic model selection** for optimal speed and cost:

```
┌─────────────────────────────────────────────────────────────┐
│                    V3 Two-Tier Pipeline                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Step 1: Create folder & copy file (Haiku)                  │
│  Step 2: Identify structure (Grep)                          │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  PARALLEL PROCESSING (Steps 3 & 4)                    │  │
│  ├──────────────────────────────────────────────────────┤  │
│  │  Step 3: Extract summaries (Haiku) ─┐                │  │
│  │                                      │  15-20 min     │  │
│  │  Step 4: Generate headings (Haiku) ─┘                │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  Step 5: Synthesize insights (Sonnet) ────── 10-15 min     │
│  Step 6: Write output files ──────────────── 1-2 min       │
│  Step 7: Verify & report                                    │
│                                                              │
│  Total: ~30-35 minutes                                      │
└─────────────────────────────────────────────────────────────┘
```

### Model Selection Strategy

- **Haiku:** Fast extraction, pattern-matching, formatting (5x cheaper)
- **Sonnet:** Deep synthesis, insights, reasoning (higher quality)
- **Parallelization:** Steps 3 & 4 run simultaneously (40% time savings)

## Requirements

- **Claude Code CLI** (v1.0.0 or higher)
- **Readwise integration** (for book highlights)
- **Markdown files** from Readwise exports

### Input File Format

The plugin expects Readwise highlight files with this structure:

```markdown
# Book Title

## Metadata
Full Title: Book Title
Author: Author Name
Category: #books
Cover: ![cover](url)

## Highlights

- Highlight text here (Location 123)
  - Note: Personal note here (optional)

- Another highlight (Location 456)
```

## Configuration

No configuration needed! The plugin works out-of-the-box with Readwise exports.

## Use Cases

Perfect for:
- 📚 **Knowledge workers** building Second Brains
- ✍️ **Writers** researching topics
- 🎓 **Students** processing textbooks
- 🧠 **Lifelong learners** managing reading queues
- 📊 **Researchers** synthesizing literature
- 🗂️ **Obsidian users** with PARA/Zettelkasten systems

## Integration with Obsidian

This plugin is designed for Obsidian PKM workflows:

1. Export highlights from Readwise to `06_Readwise/Books/`
2. Run `/book-summary` on the highlight file
3. AI-generated summary appears in `_AI Outputs/{book_name}/`
4. Link summary from your book tracking file
5. Extract concepts into Zettelkasten notes

### Example Workflow

```bash
# 1. Sync Readwise highlights to Obsidian
# 2. Process with book-summary
/book-summary ~/Vault/06_Readwise/Books/_Atomic Habits_James Clear_12345_kindle.md --auto

# 3. Link from book tracking file
# In: 03_RESOURCES/Types/Books/Atomic Habits — James Clear.md
# Add: book_summary_ai: "[[BOOK_SUMMARY_Atomic Habits]]"

# 4. Extract key concepts to Idea Compass notes
# Create individual notes for frameworks and mental models
```

## Performance Benchmarks

| Book Size | Lines | Processing Time | Cost | Output Files |
|-----------|-------|----------------|------|--------------|
| Small | 500 | ~15-20 min | ~$0.60 | 6 files |
| Medium | 700 | ~25-30 min | ~$0.85 | 6 files |
| Large | 1500+ | ~30-40 min | ~$1.05 | 6 files |

**Comparison to V2 workflow:**
- 70% faster (30 min vs 2 hrs)
- 46% cheaper ($1.05 vs $1.95)
- Same output quality

## Troubleshooting

### File Not Found
- Ensure the path is absolute, not relative
- Check that the file exists and is readable

### No Chapters Detected
- Plugin will treat the entire file as one chapter
- Processing continues normally

### Missing Personal Notes
- Notes must follow format: `- Note: [text]` or `Note: [text]`
- Must be directly after the highlight they reference

### Output Files Not Created
- Check write permissions for `_AI Outputs/` folder
- Ensure sufficient disk space
- Review Claude Code logs for errors

## FAQ

**Q: What format do highlights need to be in?**
A: Standard Readwise markdown exports. The plugin handles various chapter/part formats automatically.

**Q: Are my personal notes from Readwise preserved?**
A: Yes! All notes are preserved and attached to their related highlights.

**Q: Can I process books without chapters?**
A: Yes. The plugin treats the entire book as one chapter if no chapter markers are found.

**Q: What's the difference between standard and auto mode?**
A: Standard mode asks for confirmation before processing. Auto mode runs completely uninterrupted.

**Q: How much does it cost per book?**
A: ~$0.60-$1.05 depending on book size (using Claude API pricing).

**Q: Can I customize the output format?**
A: Not currently, but you can edit the intermediate files and regenerate the final output manually.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Changelog

### Version 3.0.0 (Current)
- ✅ Two-tier pipeline (Haiku + Sonnet)
- ✅ Parallel processing for 40% speed improvement
- ✅ 46% cost reduction vs V2
- ✅ Action-oriented cluster headings (James Clear style)
- ✅ Personal note preservation
- ✅ Auto mode for uninterrupted processing

### Version 2.0.0 (Deprecated)
- Single-tier Sonnet processing
- Sequential workflow
- Basic cluster headings

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Author

**Michael Fichter**
- GitHub: [@mfalgorythmic](https://github.com/mfalgorythmic)

## Acknowledgments

- Built for [Claude Code](https://claude.ai/code) by Anthropic
- Inspired by the Second Brain / PARA method community
- Optimized for [Obsidian](https://obsidian.md) knowledge management
- Cluster heading style inspired by [James Clear](https://jamesclear.com)

## Support

If you find this plugin useful, please:
- ⭐ Star the repository
- 🐛 Report bugs via [GitHub Issues](https://github.com/mfalgorythmic/book-summary-plugin/issues)
- 💡 Suggest features via [GitHub Discussions](https://github.com/mfalgorythmic/book-summary-plugin/discussions)
- 📢 Share with others who might benefit

---

**Made with ❤️ for the PKM community**
