# Agent Instructions

This is a course content repository for Angewandte Systemwissenschaften 1. Treat it as source-controlled curriculum data, not as an application codebase.

## Primary Rules

- Read `README.md` before editing course content.
- Preserve stable IDs, source paths, and manifest structure.
- Do not invent database IDs. Use repository paths such as `lessons/orientation.md`.
- Keep all JSON valid, pretty-printed when possible, and newline-terminated.
- Do not move material file folders during simple title or description edits.
- Do not add secrets, access tokens, student submissions, private grades, or production configuration.

## Content Conventions

- Syllabus: edit `syllabus.md`.
- Markdown lessons: edit `lessons/*.md` with front matter.
- Presentation lessons: keep deck files and sidecar metadata JSON together.
- Exams: edit `tests/*.json`.
- Materials: edit `materials/materials.json` and add referenced binaries under `materials/files/`.

## Materials Rules

Every material has a stable `id`. New file material paths should use:

```text
materials/files/<readable-name>/<filename>
```

Lesson material attachments must use `lesson_source_path`:

```json
{"target": "lesson", "lesson_source_path": "lessons/orientation.md"}
```

If a material is no longer active, prefer setting `"status": "archived"` instead of deleting it. Deleting a sourced material from the manifest tells the platform to archive it locally.

## Before Finishing

- Check that referenced material file paths exist.
- Check that lesson attachment paths match real lesson files.
- Check that statuses are one of `draft`, `published`, or `archived`.
- Summarize changed files and any assumptions.
