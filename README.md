# paper-to-skill

**Turn research paper PDFs into structured agent skills — ready to study, reference, and use while you work in Claude Code or OpenCode.**

## What It Does

paper-to-skill takes one or more research paper PDFs and converts them into a structured, on-demand skill that your AI coding agent can load and reason with. Instead of re-reading a paper every session, teach your agent the paper's methods, findings, and frameworks once.

The generated skill includes:
- **SKILL.md** — core contributions, methods, and a navigable index
- **sections/** — per-section summaries with key concepts, methodology, findings
- **glossary.md** — all technical terms with definitions
- **methods.md** — all methods, algorithms, and experimental procedures
- **cheatsheet.md** — decision rules, comparison tables, key results

## Supported Agents

| Agent | Personal Skill Root | Project-local Root |
|-------|--------------------|--------------------|
| **Claude Code** | `~/.claude/skills/` | `.claude/skills/` |
| **OpenCode** | `~/.opencode/skills/` | `.opencode/skills/` |

## Quick Start

### 1. Install the skill

**Claude Code:**
```bash
mkdir -p ~/.claude/skills/
cp -r . ~/.claude/skills/paper-to-skill/
```

**OpenCode:**
```bash
mkdir -p ~/.opencode/skills/
cp -r . ~/.opencode/skills/paper-to-skill/
```

### 2. Install PDF extraction dependencies

```bash
# System tool (recommended, fastest)
sudo apt install poppler-utils

# Or Python packages (any one is enough)
pip install pypdf
# or
pip install pdfminer.six

# For technical papers with tables/formulas (optional)
pip install docling
```

### 3. Use it

In Claude Code or OpenCode, just say:

```
paper-to-skill ~/papers/attention-is-all-you-need.pdf
```

Or convert multiple papers at once:

```
paper-to-skill ~/papers/*.pdf my-literature-review
```

Or analyze before generating:

```
paper-to-skill ~/papers/paper.pdf
> analyze only
```

## Modes of Operation

| Mode | Trigger | Output |
|------|---------|--------|
| **Full Conversion** | Provide PDF path(s) | Complete skill with all files |
| **Analyze Only** | Say "analyze" or "analyze only" | Extraction report for review |
| **Generate from Analysis** | Provide prior analysis notes | Skill files from analysis |
| **Update / Fold-in** | Point to existing skill + new PDFs | Updated skill with new papers |

## Paper Types

The converter optimizes extraction based on paper type:

- **Technical/Quantitative** — ML papers, CS, engineering (uses Docling for tables, formulas, algorithms)
- **Text-heavy/Qualitative** — social science, humanities, reviews (uses fast text extraction)

## Project Structure

```
paper-to-skill/
├── SKILL.md                    # The main skill definition (install this)
├── scripts/
│   └── extract.py              # PDF text extraction entrypoint
├── paper_to_skill/             # Python package
│   ├── __init__.py
│   ├── __main__.py
│   ├── cli.py                  # CLI entrypoint
│   ├── config.py               # Configuration constants
│   ├── dependencies.py         # Dependency management
│   ├── exceptions.py           # Custom exceptions
│   ├── utils.py                # Main extraction logic
│   └── parsers/
│       ├── __init__.py
│       └── pdf.py              # PDF extraction methods
├── pyproject.toml
└── README.md
```

## Requirements

- Python >= 3.9
- At least one PDF extractor:
  - `pdftotext` (from poppler-utils) — recommended, fastest
  - `pypdf` — pure Python fallback
  - `pdfminer.six` — pure Python fallback
  - `docling` — for technical papers with tables/formulas (optional)

## Dependency Check

Run the preflight check to see what's installed:

```bash
python3 scripts/extract.py --check
```

## Acknowledgments

This project is inspired by and adapted from [book-to-skill](https://github.com/virgiliojr94/book-to-skill) by [@virgiliojr94](https://github.com/virgiliojr94), adapted for research paper PDFs with support for Claude Code and OpenCode.

## License

MIT
