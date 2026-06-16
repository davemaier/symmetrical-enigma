# Angewandte Systemwissenschaften 1

This repository is the portable source copy of the course content managed by the learning platform.
It is designed to be readable by teachers and editable by careful automation.

## Course Details

- Description: Kurs für angewandte Systemwissenschaften 1
- Term: SS26
- Dates: 2026-06-16 to 2026-06-30
- Timezone: CEST

## Repository Map

- `syllabus.md` - the course syllabus. It uses YAML-style front matter followed by Markdown.
- `lessons/*.md` - Markdown lessons. Each file has front matter for title, schedule, location, status, and overview.
- `lessons/*.pptx` and `lessons/*.pdf` - presentation lessons. Each deck must have a matching JSON metadata file next to it, for example `lessons/week-1.pdf` and `lessons/week-1.json`.
- `tests/*.json` - exams and checks. Questions, options, points, and passing percentage are stored as JSON.
- `materials/materials.json` - the manifest for course materials, including links, file metadata, statuses, and attachments.
- `materials/files/<readable-name>/<filename>` - uploaded material files referenced by the manifest.
- `README.md`, `AGENT.md`, and `CLAUDE.md` - generated guidance for people and coding agents.

## How Sync Works

The platform imports and exports files by convention. Content edits made in this repository can be synced back into the course when they follow the formats below.

Stable source paths matter. Lesson attachments in `materials/materials.json` refer to `lesson_source_path`, not database IDs. Material identity comes from the manifest `id`; file paths stay stable after they are first exported.

## Syllabus

`syllabus.md` starts with front matter:

```markdown
---
title: Course Syllabus
---
# Welcome
```

The body is normal Markdown.

## Lessons

Markdown lessons live under `lessons/` and use this shape:

```markdown
---
title: Orientation
starts_at: 2026-06-12T09:30:00
location: Room 204
status: published
overview: Set up accounts and course tools.
---
# First slide

---

## Second slide
```

Valid lesson statuses are `draft`, `published`, and `archived`.

Presentation lessons use the deck file plus a JSON metadata sidecar:

```json
{
  "title": "Orientation",
  "starts_at": "2026-06-12T09:30:00",
  "location": "Room 204",
  "status": "published",
  "overview": "Set up accounts and course tools."
}
```

## Exams

Exams live in `tests/*.json`. Valid exam statuses are `draft`, `published`, and `archived`.

```json
{
  "title": "Protocol Check",
  "description": "Check the basics.",
  "status": "draft",
  "passing_percentage": 70,
  "questions": [
    {
      "kind": "single_choice",
      "prompt": "Pick the web protocol.",
      "points": 1,
      "options": [
        {"key": "http", "text": "HTTP", "correct": true}
      ]
    }
  ]
}
```

## Materials

Materials are restored from `materials/materials.json`.

```json
{
  "version": 1,
  "materials": [
    {
      "id": "stable-material-id",
      "kind": "file",
      "title": "Project Brief",
      "description": "Read before starting the project.",
      "status": "published",
      "external_url": null,
      "file": {
        "path": "materials/files/project-brief/brief.pdf",
        "filename": "brief.pdf",
        "original_filename": "Brief.pdf",
        "content_type": "application/pdf",
        "byte_size": 12345
      },
      "attachment": {"target": "course"}
    }
  ]
}
```

Link materials use `"kind": "link"` and put the URL in `external_url`. File materials must include a matching file under `materials/files/`.

Attachment targets:

- `{"target": "course"}` attaches to the course materials list.
- `{"target": "lesson", "lesson_source_path": "lessons/orientation.md"}` attaches to a lesson by repository path.
- `{"target": "none"}` leaves the material unattached.

Removing a sourced material from the manifest archives it in the platform. Archived materials stay in the manifest with `"status": "archived"` so a course can be restored fully.

## Editing Checklist

1. Keep JSON valid and end generated JSON files with a newline.
2. Preserve material `id` values and existing material file paths unless intentionally replacing a material.
3. Use repository paths for lesson attachments, never database IDs.
4. Put binary material files under `materials/files/<readable-name>/`.
5. Do not store secrets, credentials, student submissions, or private grades in this repository.
