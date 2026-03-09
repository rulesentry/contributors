# RuleSentry Community Contributors

Community-contributed rules, policies, categories, profiles, and regions for [RuleSentry](https://rulesentry.io) — the rule evaluation engine for detecting sensitive data patterns, enforcing compliance policies, and applying transforms.

## Getting Started

1. **Fork** this repository
2. **Clone** your fork locally
3. **Create** your rule, policy, or other entity (see below)
4. **Open a PR** to `main`

## Directory Structure

```
contributors/
├── publishers/               # Registered publisher identities
│   ├── rulesentry.json       # Core publisher
│   └── mycos.json            # Community publisher
├── rules/
│   ├── core/                 # RuleSentry-authored rules (~153)
│   │   ├── contact/          # Category subdirectory
│   │   ├── financial/
│   │   ├── government/
│   │   └── ...
│   └── mycos/                # Community publisher
│       └── contact/
│           └── th-phone-number.json
├── policies/
│   ├── core/                 # Core policies (11)
│   └── {publisher}/          # Community policies
├── categories/
│   ├── core/                 # Core categories (8)
│   └── {publisher}/
├── profiles/
│   ├── core/                 # Core profiles (8)
│   └── {publisher}/
├── regions/
│   └── core/                 # Region registry
└── LICENSE
```

**Key conventions:**
- `core/` is reserved for RuleSentry-authored entities. Uses `qualified_id: "core.{id}"`.
- Other directories are community publishers. The directory name must match a registered publisher in `publishers/{name}.json`. Uses `qualified_id: "community.{publisher}.{category}.{name}"`.
- Category subdirectories are optional — uncategorized entities sit directly under the publisher directory.

## Contributing a Rule

1. **Register as a publisher** (if first contribution):

   Create `publishers/{your-id}.json`:
   ```json
   {
     "id": "your-id",
     "name": "Your Name or Org",
     "url": "https://your-site.com",
     "contact": "you@example.com",
     "description": "Brief description of your contributions"
   }
   ```

2. **Create your rule** in `rules/{your-id}/{category}/`:

   ```json
   {
     "$schema": "https://schemas.rulesentry.io/schema/v3/core/rule.schema.json",
     "id": "{category}.{rule-name}",
     "qualified_id": "community.{your-id}.{category}.{rule-name}",
     "type": "rule",
     "version": "1.0.0",
     "name": "Human-Readable Name",
     "description": "What this rule detects and why.",
     "category_id": "{category}",
     "rule_kind": "atomic",
     "severity": "medium",
     "evaluation": {
       "evaluation_type": "pattern_match",
       "target": "content",
       "pattern": {
         "pattern": "your-regex-here",
         "engine": "regex"
       },
       "effect": {
         "type": "transform",
         "transform": { "type": "redact", "params": {} },
         "message": "Description of the action taken"
       }
     },
     "status": "active",
     "regions": ["GLOBAL"],
     "origin": {
       "source_type": "local",
       "publisher": "{your-id}",
       "author": "Your Name"
     },
     "created_at": "2026-01-01T00:00:00Z",
     "updated_at": "2026-01-01T00:00:00Z"
   }
   ```

3. **Open a PR**

## Contributing a Policy

Policies assemble rules via direct references, category inclusion, and filters. Your policy can reference:

- **Your own rules** — by their bare `id` (e.g., `contact.mx-phone-number`)
- **Core rules** — built-in RuleSentry rules (e.g., `government.ssn_format`)

Create your policy in `policies/{your-id}/` and follow the same `community.{your-id}.*` ID convention.

## ID Conventions

| Prefix | Usage | Directory |
|--------|-------|-----------|
| `core.*` | RuleSentry-authored entities | `{entity-type}/core/` |
| `community.{publisher}.*` | Community contributions | `{entity-type}/{publisher}/` |

- The publisher directory name must match the publisher ID in the `qualified_id`
- Community publishers must be registered in `publishers/{id}.json`
- File path should reflect the ID: `rules/{publisher}/{category}/{rule-name}.json`

## Schema Reference

For the canonical JSON schemas that define rule, policy, category, profile, and region formats, see the `json-schema/` directory in the [main RuleSentry repository](https://github.com/rulesentry/rulesentry).

Full schema documentation: [rulesentry.io/docs/schemas](https://rulesentry.io/docs/schemas)

## License

MIT — see [LICENSE](LICENSE).
