# SelfSovGEDofTheFuture
Improved Geneaological File Format for Multimodal data, towards secure self sovereign identity ownership of past and present for the future
# GenGraph: A Modern Genealogical Knowledge Graph Format (Draft)

> ⚠️ **Very early draft.** This repo is meant as a conversation starter, not a finished standard.

Traditional genealogical data formats (like GEDCOM) are great at representing
**trees of people**, but struggle with:

- Rich **evidence and provenance**
- **Non-tree** structures (multiple hypotheses, conflicting lines, complex kinship)
- Modern data types (DNA, geospatial, legal statuses, titles, properties)
- Integration with web, graphs, and self-sovereign identity ideas

**GenGraph** is a proposal for a lightweight, JSON-based, graph-shaped format
for genealogical and historical data that:

- Treats **people, events, places, sources, properties, and titles** as first-class nodes
- Uses **relations** to connect them (e.g. `PARENT_OF`, `SPOUSE_OF`, `OWNED_PROPERTY`)
- Is easy to read/edit as JSON, and easy to map into graph DBs or other systems
- Is designed to **coexist with GEDCOM**, not replace it:
  - Import from GEDCOM → richer graph structure
  - Export to GEDCOM → maintain basic compatibility with existing tools

This repository contains:

- A **v0.1 specification** (`SPEC.md`)
- A **draft JSON Schema** (`schema/gengraph.schema.json`)
- **Starter Python code** to load and lightly validate GenGraph files
- A **minimal example file** (`examples/minimal_example.gengraph.json`)
- Seed **discussion topics** for people who want to evolve the idea

---

## Status

This is:

- A **sketch** of a format, not an official standard
- Intended to be **argued with, forked, and improved**
- Focused on: *clear enough to be useful, small enough to be non-scary*

If you work in genealogy, archives, digital humanities, data modelling, or SSI,
please consider opening issues / PRs or adding comments to `DISCUSSION_TOPICS.md`.

---

## Quick glance at the core idea

At its core, a GenGraph file is a JSON object with typed node lists and relations:

```json
{
  "gengraphVersion": "0.1.0",
  "meta": {
    "label": "Vest / West family sketch",
    "createdBy": "ash@example.com",
    "createdAt": "2025-12-03T10:00:00Z"
  },
  "persons": [
    {
      "id": "P1",
      "primaryName": {
        "given": "Thomas",
        "surname": "Vest"
      }
    }
  ],
  "events": [],
  "places": [],
  "sources": [],
  "properties": [],
  "titles": [],
  "relations": []
}
