# sf-org-knowledge

`sf-org-knowledge` analyzes a connected Salesforce org with read-only SF CLI commands and generates
an org atlas under `docs/org/`.

The atlas is designed for fast orientation. A new AI session should be able to open
`docs/org/ORG_INDEX.md`, follow the routing links, and understand the org's structure, major
components, likely business domains, and notable risks without re-querying the org first.

## Safety

This skill must never write to the Salesforce org.

Allowed SF CLI commands are limited to:

- `sf org display`
- `sf org list metadata-types`
- `sf org list metadata`
- `sf sobject list`
- `sf sobject describe`
- `sf data query` with `SELECT` only
- `sf project retrieve start`
- `sf project generate manifest`

The generated collection script must be scanned before execution and rejected if it contains deploy,
source-push, org-mutation, or data-mutation commands.

## Trigger Phrases

Use this skill when the user says:

- `analyze org`
- `scan org`
- `map org`
- `generate org knowledge`
- `update org knowledge`
- `refresh org knowledge`
- `build org atlas`

## Workflow

1. Interview for org alias, validate the org connection, and restate the read-only contract.
2. Generate and validate the collection script from `scripts/collect-metadata.sh.template`.
3. Collect raw metadata into `docs/org/_raw/`.
4. Run seven parallel specialist agents to generate the reference docs.
5. Assemble `ORG_INDEX.md` and `concerns.md`.
6. Propose business-domain groupings interactively and generate approved domain docs.
7. Delete `_raw/` and commit generated docs with the correct `feat:` or `chore:` message.

## Output

```text
docs/org/
  ORG_INDEX.md
  reference/
    object-field-inventory.md
    automation-inventory.md
    apex-inventory.md
    integration-map.md
    security-model.md
    installed-packages.md
    data-model.md
  data-model.mermaid
  domains/
    <domain-name>.md
  concerns.md
```

## Notes

- The skill is rerunnable and should overwrite stale `_raw/` and reference docs on refresh.
- If Tooling API queries or metadata lists fail, the skill should produce a partial atlas and note the gap.
- This skill inventories what exists. It does not analyze Apex source logic or Flow decision trees.
