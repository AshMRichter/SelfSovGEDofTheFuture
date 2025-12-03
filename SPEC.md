A. Entities (nodes)

Definition: Relevant person ID variables infrastructure. 

Fields:

A1. id (string, required)

A2. primaryName (object, optional)

A3. given, middle, surname, prefix, suffix (strings, optional)

A4. altNames (array of objects, optional)

A5. Same structure as primaryName, plus optional note

A6. gender (string, optional; e.g. "male", "female", "unknown", "nonbinary")

A7. birthEventId, deathEventId (string, optional)

A8. IDs of Event nodes (see below)

A9. note (string, optional)

All entities share:

A required string id that must be unique within this file.

Example:
{
  "id": "P1",
  "primaryName": {
    "given": "Thomas",
    "middle": "Benjamin",
    "surname": "Vest",
    "prefix": null,
    "suffix": null
  },
  "altNames": [
    {
      "given": "Thomas",
      "surname": "West",
      "note": "Variant spelling in some KY records"
    }
  ],
  "gender": "male",
  "birthEventId": "E1",
  "deathEventId": "E2",
  "note": "Possible Revolutionary War service; see source S3."
}

B. Event

Definition: Represents a dated occurrence: birth, marriage, property transfer, etc.

Fields:

B1. id (string, required)

B12. type (string, required)

Examples: "birth", "death", "marriage", "baptism", "burial",
"residence", "immigration", "military", "propertyTransfer", "other"

B3. date (string, optional; ISO 8601 if exact or best guess, e.g. "1783-07-04")

B4. dateRange (object, optional)

B5. earliest, latest (strings, ISO 8601-ish)

B6. placeId (string, optional; ID of a Place)

B7. participantIds (array of strings, optional; IDs of Person)

B8. sourceIds (array of strings, optional; IDs of Source)

B9. note (string, optional)

C. Place

Definition: Represents a geographic location.

Fields:

C1.  (string, required)

C2. name (string, required)

c3. type (string, optional; free-text classification, e.g., "village", "parish")

C4. geo (object, optional)

C5. lat, lon (numbers, optional)

C6. note (string, optional)

Example: 
{
  "id": "PL1",
  "name": "Augusta County, Virginia",
  "type": "county",
  "geo": {
    "lat": 38.163,
    "lon": -79.057
  },
  "note": "County boundaries changed over time."
}

D. Source

Definition: Represents any source of information: certificate, census page, DNA test, book, etc.

D1. id (string, required)

D2. type (string, optional)

D3. title (string, optional)

D4. citation (string, optional)

D5. url (string, optional)

D6. note (string, optional)

Example: 
{
  "id": "S1",
  "type": "census",
  "title": "1783 Virginia State Census",
  "citation": "1783 Virginia State Census, Augusta County, p. 12.",
  "url": "https://example.org/scan/1783-VA-augusta-p12",
  "note": "Downloaded from XYZ archive 2025-01-01."
}

E. Property

Definition: Represents a parcel of land, building, or other tangible property.

Fields:

E1. id (string, required)

E2. name (string, required)

E3. placeId (string, optional; ID of Place)

E4. parcelId (string, optional; jurisdiction-specific identifier)

E5. note (string, optional)

Example: 
{
  "id": "PR1",
  "name": "200-acre tract on South River",
  "placeId": "PL1",
  "parcelId": "VA-AG-1783-001",
  "note": "Described in land grant S4."
}

F. Title

Definition: Represents a hereditary or official title (noble, chivalric, office, etc.).

Fields:

F1. id (string, required)

F2. name (string, required)

F3. jurisdiction (string, optional)

F4. inheritanceRule (string, optional; free-text for v0.1)

F5. note (string, optional)

G. Relations (edges)

Defnition: Relations connect entities and can carry evidence/confidence.

Fields:

G1. id (string, required)

G2. type (string, required)

 Suggested values (not exhaustive):

G2a. Kinship:

 PARENT_OF, CHILD_OF, SPOUSE_OF, SIBLING_OF

G2b. Events:

PARTICIPANT_IN (Person → Event)

G2c. Places:

OCCURRED_AT (Event → Place)

G2d. Property:

OWNED_PROPERTY, INHERITED_PROPERTY, GRANTED_PROPERTY

G2e. Titles:

HELD_TITLE, INHERITED_TITLE

G2f. Meta:

CONFLICTS_WITH, ALTERNATIVE_TO

G3.fromId (string, required; ID of any entity)

G4. toId (string, required; ID of any entity)

G6.sourceIds (array of strings, optional; IDs of Source)

G7. confidence (number, optional; 0.0–1.0)

G8.note (string, optional)

For kinship, PARENT_OF and CHILD_OF are conceptually inverses. Implementations
may choose to store only one direction and infer the other.

H. Identifiers and uniqueness

All id values must be unique within the file.

There is no mandated global ID scheme in v0.1.

Future versions may define recommended ID strategies (e.g. URIs, DIDs, etc.).

I. For v0.1, dates are stored as strings.

Recommend ISO-like formats where possible:

"YYYY-MM-DD", "YYYY-MM", "YYYY".

dateRange is intended for uncertainty:

Example: person known to be born between 1759 and 1761.

J. Backwards and forwards compatibility

Parsers should ignore unknown fields at any level.

Writers should not rely on any consumer rejecting extra fields.

K. Future directions (out of scope for 0.1, but worth discussing)

A more formal claim/assertion model:

Each claim as a first-class object with subject/predicate/object.

Standardised predicate vocabularies (RDF/OWL alignment).

Self-sovereign identity aspects:

Signing of relations/claims

Verifiable credentials for descents, memberships, etc.

Privacy tiers and encryption for modern data.

Examples: 

---

## 4. `schema/gengraph.schema.json` – Draft JSON Schema

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.org/gengraph.schema.json",
  "title": "GenGraph v0.1",
  "description": "Draft JSON Schema for the GenGraph genealogical knowledge graph format.",
  "type": "object",
  "required": ["gengraphVersion", "persons"],
  "properties": {
    "gengraphVersion": {
      "type": "string"
    },
    "meta": {
      "type": "object",
      "additionalProperties": true
    },
    "persons": {
      "type": "array",
      "items": {
        "$ref": "#/$defs/person"
      }
    },
    "events": {
      "type": "array",
      "items": {
        "$ref": "#/$defs/event"
      }
    },
    "places": {
      "type": "array",
      "items": {
        "$ref": "#/$defs/place"
      }
    },
    "sources": {
      "type": "array",
      "items": {
        "$ref": "#/$defs/source"
      }
    },
    "properties": {
      "type": "array",
      "items": {
        "$ref": "#/$defs/property"
      }
    },
    "titles": {
      "type": "array",
      "items": {
        "$ref": "#/$defs/title"
      }
    },
    "relations": {
      "type": "array",
      "items": {
        "$ref": "#/$defs/relation"
      }
    }
  },
  "additionalProperties": false,
  "$defs": {
    "person": {
      "type": "object",
      "required": ["id"],
      "properties": {
        "id": {
          "type": "string"
        },
        "primaryName": {
          "$ref": "#/$defs/name"
        },
        "altNames": {
          "type": "array",
          "items": {
            "$ref": "#/$defs/name"
          }
        },
        "gender": {
          "type": "string"
        },
        "birthEventId": {
          "type": "string"
        },
        "deathEventId": {
          "type": "string"
        },
        "note": {
          "type": "string"
        }
      },
      "additionalProperties": false
    },
    "name": {
      "type": "object",
      "properties": {
        "given": { "type": "string" },
        "middle": { "type": "string" },
        "surname": { "type": "string" },
        "prefix": { "type": "string" },
        "suffix": { "type": "string" },
        "note": { "type": "string" }
      },
      "additionalProperties": false
    },
    "event": {
      "type": "object",
      "required": ["id", "type"],
      "properties": {
        "id": { "type": "string" },
        "type": { "type": "string" },
        "date": { "type": "string" },
        "dateRange": {
          "type": "object",
          "properties": {
            "earliest": { "type": "string" },
            "latest": { "type": "string" }
          },
          "additionalProperties": false
        },
        "placeId": { "type": "string" },
        "participantIds": {
          "type": "array",
          "items": { "type": "string" }
        },
        "sourceIds": {
          "type": "array",
          "items": { "type": "string" }
        },
        "note": { "type": "string" }
      },
      "additionalProperties": false
    },
    "place": {
      "type": "object",
      "required": ["id", "name"],
      "properties": {
        "id": { "type": "string" },
        "name": { "type": "string" },
        "type": { "type": "string" },
        "geo": {
          "type": "object",
          "properties": {
            "lat": { "type": "number" },
            "lon": { "type": "number" }
          },
          "additionalProperties": false
        },
        "note": { "type": "string" }
      },
      "additionalProperties": false
    },
    "source": {
      "type": "object",
      "required": ["id"],
      "properties": {
        "id": { "type": "string" },
        "type": { "type": "string" },
        "title": { "type": "string" },
        "citation": { "type": "string" },
        "url": { "type": "string" },
        "note": { "type": "string" }
      },
      "additionalProperties": false
    },
    "property": {
      "type": "object",
      "required": ["id", "name"],
      "properties": {
        "id": { "type": "string" },
        "name": { "type": "string" },
        "placeId": { "type": "string" },
        "parcelId": { "type": "string" },
        "note": { "type": "string" }
      },
      "additionalProperties": false
    },
    "title": {
      "type": "object",
      "required": ["id", "name"],
      "properties": {
        "id": { "type": "string" },
        "name": { "type": "string" },
        "jurisdiction": { "type": "string" },
        "inheritanceRule": { "type": "string" },
        "note": { "type": "string" }
      },
      "additionalProperties": false
    },
    "relation": {
      "type": "object",
      "required": ["id", "type", "fromId", "toId"],
      "properties": {
        "id": { "type": "string" },
        "type": { "type": "string" },
        "fromId": { "type": "string" },
        "toId": { "type": "string" },
        "sourceIds": {
          "type": "array",
          "items": { "type": "string" }
        },
        "confidence": {
          "type": "number",
          "minimum": 0.0,
          "maximum": 1.0
        },
        "note": { "type": "string" }
      },
      "additionalProperties": false
    }
  }
}


