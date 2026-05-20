# Friendly names over IDs

> **Status: TODO.** Notion source has bullets but no finalised guidance. Treat the points below as drafts and escalate to `@design` for anything binding.

## Draft notes from Notion

- Use an alias / friendly name as much as possible.
- IDs are too abstract — they don't tell the user whether they're looking at a template, a domain, or a contact.
- Not every entity needs a friendly name. Some are too ephemeral to deserve one — webhook events, for instance.

## How to apply this

- For long-lived resources users will refer to (domains, templates, contacts, API keys), show the friendly name as the primary identifier and the ID as a secondary detail (or in a tooltip / detail view).
- For ephemeral or system-generated entities (single events, transient jobs), an ID is fine.

## Open questions

- The exact rule for "which resources need a friendly name?" — likely the resources that appear in lists or that users link to externally.
- Fallback behaviour when the user hasn't named something yet (use the ID? a generated nickname?).
