# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a curated collection of Claude Skills - modular packages that extend Claude's capabilities with specialized workflows, domain knowledge, and tool integrations. Skills work across Claude.ai, Claude Code, and the Claude API.

## Repository Structure

The repository follows a flat directory structure where each top-level directory represents a skill:

```
awesome-claude-skills/
├── skill-name/
│   ├── SKILL.md           # Required: Skill instructions with YAML frontmatter
│   ├── scripts/           # Optional: Executable code (Python/Bash)
│   ├── references/        # Optional: Documentation loaded as needed
│   └── assets/            # Optional: Files used in output (templates, fonts, etc.)
├── CONTRIBUTING.md         # Contribution guidelines
└── README.md              # Main documentation
```

### Skill Anatomy

Every skill must contain a `SKILL.md` file with:

1. **YAML frontmatter** (required):
   - `name`: Skill identifier (lowercase-with-hyphens)
   - `description`: When/why to use the skill (third-person, specific)
   - `license`: Optional licensing information

2. **Markdown body** with imperative/infinitive instructions for Claude

3. **Optional bundled resources**:
   - `scripts/`: Executable code for deterministic tasks
   - `references/`: Documentation loaded into context as needed
   - `assets/`: Files used in skill output (templates, images, fonts)

## Installing Skills

To use skills from this repository in Claude Code:

```bash
# Create the skills directory if it doesn't exist
mkdir -p ~/.claude/skills/

# Copy a skill to the installation directory
cp -r skill-name ~/.claude/skills/

# Verify the skill was installed
ls ~/.claude/skills/skill-name/
```

Skills are automatically loaded when Claude Code starts and activate when relevant to the task.

## Common Development Tasks

### Creating a New Skill

Use the initialization script to generate a properly structured skill:

```bash
./skill-creator/scripts/init_skill.py <skill-name> --path /Users/caijie/Project/awesome-claude-skills
```

This creates the skill directory with:
- SKILL.md template with proper frontmatter and TODO placeholders
- Example `scripts/`, `references/`, and `assets/` directories
- Example files that can be customized or deleted

### Validating a Skill

Before packaging or submitting, validate the skill structure:

```bash
./skill-creator/scripts/quick_validate.py <path/to/skill-folder>
```

Checks:
- YAML frontmatter format and required fields
- Naming conventions and directory structure
- Description completeness and quality
- File organization and resource references

### Packaging a Skill

Create a distributable zip file (automatically validates first):

```bash
./skill-creator/scripts/package_skill.py <path/to/skill-folder>
```

Optional output directory:
```bash
./skill-creator/scripts/package_skill.py <path/to/skill-folder> ./dist
```

### Adding a Skill to README

After creating a skill, add it to README.md:

1. Choose the appropriate category (alphabetically organized):
   - Document Processing
   - Development & Code Tools
   - Data & Analysis
   - Business & Marketing
   - Communication & Writing
   - Creative & Media
   - Productivity & Organization
   - Collaboration & Project Management
   - Security & Systems

2. Add entry in alphabetical order within category:
   ```markdown
   - [Skill Name](./skill-name/) - One-sentence description. *By [@username](https://github.com/username)* (for external contributors)
   ```

3. Follow existing format: no emojis, consistent punctuation

## Skill Writing Guidelines

### Description Quality (Critical)

The `description` field in YAML frontmatter determines when Claude will use the skill. Requirements:

- **Be specific** about what the skill does and when to use it
- **Use third person**: "This skill should be used when..." not "Use this skill when..."
- **Include file types** when relevant: ".xlsx, .xlsm, .csv, .tsv"
- **List concrete use cases**: "(1) Creating new spreadsheets, (2) Reading or analyzing data..."

Example:
```yaml
description: "Comprehensive spreadsheet creation, editing, and analysis with support for formulas, formatting, data analysis, and visualization. When Claude needs to work with spreadsheets (.xlsx, .xlsm, .csv, .tsv, etc) for: (1) Creating new spreadsheets with formulas and formatting, (2) Reading or analyzing data, (3) Modify existing spreadsheets while preserving formulas, (4) Data analysis and visualization in spreadsheets, or (5) Recalculating formulas"
```

### Writing Style

Write SKILL.md using **imperative/infinitive form** (verb-first instructions), not second person:
- ✅ "To accomplish X, do Y"
- ✅ "Place all assumptions in separate cells"
- ❌ "You should do X"
- ❌ "If you need to do X"

Use objective, instructional language suitable for AI consumption.

### Progressive Disclosure Principle

Skills use a three-level loading system:
1. **Metadata** (name + description) - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<5k words recommended)
3. **Bundled resources** - As needed by Claude

Keep SKILL.md lean - move detailed information to `references/` files. Include only essential procedural instructions and workflow guidance in SKILL.md.

### Avoid Duplication

Information should live in either SKILL.md or references files, not both. Prefer references files for:
- Detailed schemas or API documentation
- Examples and templates
- Company policies or domain knowledge
- Large reference material (>10k words)

If references files are large, include grep search patterns in SKILL.md to help Claude find relevant sections.

## Contribution Requirements

All skills must (from CONTRIBUTING.md):

1. **Solve a real problem** - Based on actual usage, not theoretical
2. **Be well-documented** - Clear instructions, examples, and use cases
3. **Be accessible** - Written for non-technical users when possible
4. **Include examples** - Show practical, real-world usage
5. **Be tested** - Verify across Claude.ai, Claude Code, and/or API
6. **Be safe** - Confirm before destructive operations
7. **Be portable** - Work across Claude platforms when applicable

### Attribution

Include proper attribution for skills based on existing workflows:

```markdown
**Inspired by:** [Person Name]'s workflow
```

or

```markdown
**Credit:** Based on [Company/Team]'s process
```

## Git Workflow

This is a standard GitHub repository:

- **Main branch**: `master` (use for PRs)
- **Current untracked files**: `AGENTS.md`, `SECURITY_AUDIT.md`

When contributing:
1. Fork the repository
2. Create a branch: `git checkout -b add-skill-name`
3. Add skill folder with SKILL.md
4. Update README.md in appropriate category
5. Commit: `git commit -m "Add [Skill Name] skill"`
6. Push and open a Pull Request

PR title format: "Add [Skill Name] skill"

PR description should include:
- What problem it solves
- Who uses this workflow
- Attribution/inspiration source
- Example of how it's used

## Key Skills to Reference

When creating new skills, refer to these well-structured examples:

- **skill-creator/** - Meta-skill for creating skills, comprehensive documentation
- **mcp-builder/** - Complex skill with references/ folder showing best practices
- **document-skills/xlsx/** - Shows detailed formatting standards and requirements
- **template-skill/** - Minimal template for quick starts
