# AGENTS.md - AI Agent Guidelines for awork Developer Docs

This document explains the structure of the awork developer documentation repository and provides guidance for AI agents creating or updating API documentation.

## Repository Overview

This is a Fern-based API documentation project for the awork API. The documentation is generated from OpenAPI specifications combined with manually written overview pages.

```
fern/
├── docs.yml                      # Main navigation and structure config
├── fern.config.json              # Fern version configuration
├── apis/
│   ├── v1/
│   │   ├── openapi/openapi.json  # OpenAPI spec (auto-pulled from servers)
│   │   ├── summary.mdx           # API v1 landing page
│   │   └── generators.yml        # Spec origin configuration
│   └── v2/
│       └── ...                   # Same structure for v2
├── pages/
│   ├── api/                      # Feature overview pages (manually written)
│   │   ├── projects.mdx
│   │   ├── tasks.mdx
│   │   └── ...
│   ├── introduction.mdx          # General documentation pages
│   ├── authentication.mdx
│   └── ...
└── assets/                       # Images and resources
```

## How OpenAPI Specs Work

**OpenAPI files are automatically pulled from awork servers.** Do not manually edit these files:
- `fern/apis/v1/openapi/openapi.json` - pulled from `https://app.awork.com/openapi/v1`
- `fern/apis/v2/openapi/openapi.json` - pulled from `https://app.awork.com/openapi/v2`

The origin is configured in `fern/apis/v1/generators.yml`.

## API Groups and docs.yml

The `fern/docs.yml` file defines the documentation structure. The `layout:` section under `api: API v1 Reference` maps documentation pages to OpenAPI tags.

### Critical Rule: Tag Name Matching

**API group names in `docs.yml` must match OpenAPI tags exactly (case-insensitive, spaces normalized).**

Examples:
| docs.yml list item | OpenAPI tag |
|-------------------|-------------|
| `task statuses` | `TaskStatuses` |
| `project templates` | `ProjectTemplates` |
| `type of work` | `TypeOfWork` |
| `time entries` | `TimeEntries` |

### Layout Structure Pattern

```yaml
layout:
- page: Projects Overview       # Custom overview page (manually written)
  path: ./pages/api/projects.mdx
  icon: diagram-project         # Font Awesome icon
- projects                      # OpenAPI tag - auto-generates endpoints
- project types                 # Another OpenAPI tag
- project statuses
- page: Tasks Overview          # Next feature group
  path: ./pages/api/tasks.mdx
  icon: list-check
- tasks
- task statuses
```

## Adding a New API Group

1. **Ensure the OpenAPI tag exists** - The tag must exist in the OpenAPI spec (managed by the backend team)

2. **Add to docs.yml layout** in the appropriate position:

```yaml
   - page: Feature Overview
     path: ./pages/api/feature.mdx
     icon: appropriate-icon
   - feature tag name           # Must match OpenAPI tag
   - related tag name
```

3. **Create the overview page** at `fern/pages/api/feature.mdx`

## Writing Overview Pages

Each API group needs an overview page. Create these at `fern/pages/api/{feature}.mdx`.

### Required Structure

```yaml
---
title: Feature Name
description: Feature Name API Reference.
slug: featurename
---

Brief introduction to the feature.

## How to work with {feature}

### Creating a {feature}

```sh
curl -X POST 'https://api.awork.com/api/v1/features' \
  -H 'Authorization: Bearer {token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "requiredField": "value"
  }'
```

### Style Guidelines

- Use progressive disclosure: basic concepts first, then advanced
- Include curl examples with required fields clearly shown
- Add JSON response examples when helpful
- Link to support documentation at `support.awork.com` when available
- Note permission requirements if applicable
- Use CardGroup and Card components for visual organization when appropriate

### Complexity Levels

**Minimal** (Search, Workflows): Brief description, optional support link
**Simple** (Companies, Login): Brief description, permission notes
**Comprehensive** (Projects, Tasks): Full examples, advanced concepts, multiple operations

## Existing API Groups

The following groups are currently documented in docs.yml:

1. **Projects** - projects, project types, project statuses, project roles, project members, etc.
2. **Tasks** - tasks, task statuses, task lists, task views, checklist items, etc.
3. **Time Tracking** - time entries, time tracking, time reports
4. **Documents** - documents, document spaces, document comments
5. **Search** - search
6. **Companies** - companies, company tags
7. **Users** - users, user capacities, user tags, absences, dashboards
8. **Files & Images** - files, file upload, project files, task files, etc.
9. **Custom Fields** - custom fields
10. **Workload & Planning** - workload
11. **Project Templates** - project templates, task bundles, task templates, etc.
12. **Workflows** - workflow automations
13. **Login & Access** - accounts, invitations, permissions, roles, teams, workspaces
14. **API Management** - api users, webhooks, client applications

## Common Mistakes to Avoid

1. **Editing OpenAPI files directly** - These are auto-generated; changes will be overwritten
2. **Mismatched tag names** - Ensure docs.yml entries match OpenAPI tags exactly
3. **Missing overview pages** - Every group needs an overview page referenced in docs.yml
4. **Wrong file paths** - Overview pages go in `fern/pages/api/`, not the root pages folder
5. **Missing frontmatter** - All MDX files need title, description, and slug in YAML frontmatter

## Local Development

Run the documentation locally with:

```sh
fern docs dev
```

This starts a local development server for previewing changes.

## Publishing

Documentation is published via Fern to:
- https://aworkapi.docs.buildwithfern.com (Fern hosting)
- https://developers.awork.com (custom domain)
