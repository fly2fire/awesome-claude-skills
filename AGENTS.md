# Repository Guidelines

## Project Structure & Module Organization
- Each skill lives in its own folder: use lowercase-kebab-case (e.g., `lead-research-assistant/`).
- Required: `SKILL.md` with YAML frontmatter. Optional: `scripts/`, `examples/`, `templates/`, `resources/`, `LICENSE.txt`.
- Some collections group related skills (e.g., `document-skills/{docx,pdf,pptx,xlsx}/`).
- Top-level references: `README.md` (catalog + usage), `CONTRIBUTING.md` (submission flow).

## Build, Test, and Development Commands
- This repo has no global build. Run per-skill commands inside the skill folder.
- Python skills: create a venv and install local requirements if present.
  - `cd mcp-builder && python -m venv .venv && source .venv/bin/activate && pip install -r scripts/requirements.txt`
- Shell scripts: run with `--help` first when available.
  - `cd webapp-testing && python scripts/with_server.py --help`
- Frontend artifact bundling (example): run from your app root, not this repo root.
  - `bash artifacts-builder/scripts/bundle-artifact.sh`

## Coding Style & Naming Conventions
- Folders: lowercase-kebab-case; files: `SKILL.md` (uppercase) required.
- Markdown: clear, task-first writing; headings use Title Case; fenced code blocks with language tags.
- Scripts: keep dependencies minimal; Python follows PEP 8; Bash starts with `set -e` and helpful error messages.
- Include small, runnable examples; prefer relative paths and cross-platform behavior.

## Testing Guidelines
- Validate examples in `SKILL.md` end-to-end in: Claude.ai, Claude Code, or API (as applicable).
- For scripts, verify `--help` output, add dry-run or confirmation for destructive actions, and document flags.
- Keep assets small; avoid embedding large binaries in the repo.

## Commit & Pull Request Guidelines
- Commit messages: imperative, concise. For new skills: `Add [Skill Name] skill`.
- PRs must include: problem statement, who benefits, examples, and attribution (if inspired by others).
- Update `README.md` under the right category, alphabetical order, format: `- [Skill Name](./skill-name/) - One-sentence description.`
- Link related issues; include screenshots or small artifacts if they clarify usage.

## Security & Configuration Tips
- Never commit secrets or tokens; use placeholders in examples.
- Confirm before destructive operations; prefer opt-in network calls.
- License third-party assets clearly (e.g., `LICENSE.txt` in the skill folder).

