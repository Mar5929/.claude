---
name: sf-develop
description: >
  Salesforce development reference skill. Provides guidance for generating SFDX metadata XML,
  writing Apex (services, selectors, trigger handlers, domain classes), building LWC components,
  and writing tests. Activates when Claude is writing or generating Salesforce metadata, Apex,
  LWC, or SFDX source format files. Use when the user says "build this", "implement this",
  "generate the metadata", "create the Apex", "write the tests", or is working on any Salesforce
  development task. Do NOT use for architecture or design decisions (use sf-architect-solutioning
  instead). Do NOT use for project initialization (use sf-consulting-framework instead).
---

# Salesforce Development Reference

Development guidance for Salesforce metadata, Apex, LWC, and testing. This skill provides the technical standards and patterns Claude should follow when writing Salesforce code or generating SFDX source.

This skill covers *how to build correctly*. It does not manage process (git, commits, PRs, Linear, document updates). Those concerns belong to the sf-consulting-framework and your project-level rules.

---

## Metadata XML Generation

When generating SFDX metadata XML from designed components:

### Workflow

1. **Load the relevant metadata reference** from the shared reference files. Use only the files needed for the current component.

2. **Replace placeholders** with the designed values (object names, field names, criteria, entry conditions, etc.).

3. **Verify API version** matches `sfdx-project.json` or the latest GA version. Use Context7 MCP (`resolve-library-id` then `query-docs`) to confirm the latest GA Salesforce API version if uncertain.

4. **Check for client conventions** in the project's CLAUDE.md for a `## Client Metadata Conventions` section. If present, these conventions override defaults where they conflict.

5. **Write XML to the correct SFDX source path** following the standard SFDX source directory structure.

### Reference Lookup Table

| System Area | Reference File |
|---|---|
| Automation (Flows) | `skills/sf-architect-solutioning/references/metadata/flows.md` |
| Objects, Fields, Relationships | `skills/sf-architect-solutioning/references/metadata/objects-fields.md` |
| Validation Rules | `skills/sf-architect-solutioning/references/metadata/validation-rules.md` |
| Permissions, Security | `skills/sf-architect-solutioning/references/metadata/permission-sets.md` |
| Page Layouts, Record Types | `skills/sf-architect-solutioning/references/metadata/page-layouts-record-types.md` |
| Configuration, Labels, Settings | `skills/sf-architect-solutioning/references/metadata/custom-metadata-types.md` |
| Approval Workflows | `skills/sf-architect-solutioning/references/metadata/approval-processes.md` |
| Events, Named Credentials | `skills/sf-architect-solutioning/references/metadata/platform-events-other.md` |

---

## Apex Development

### Core Rules

1. **Bulkification** required. All Apex handles collections, never single records.
2. **No SOQL/DML in loops.** Query and DML operations outside loops, always.
3. **Trigger handler pattern.** One trigger per object, delegates to handler class.
4. **CRUD/FLS enforcement.** `WITH SECURITY_ENFORCED` or `stripInaccessible()` in every query and DML.
5. **Declarative first.** Only write Apex when Flows can't handle the requirement.
6. **Test coverage 85%+.** Bulk tests, assert outcomes, not just coverage.

### Naming Conventions

Reference `skills/sf-architect-solutioning/references/naming-conventions.md` for the firm's standard naming conventions for all Apex classes, LWC components, custom objects, fields, and other metadata.

### Architectural Patterns

Reference `skills/sf-architect-solutioning/references/architectural-patterns.md` for standard patterns:

- **Trigger Handler Dispatch**: one trigger per object, handler class with before/after methods
- **Service Layer**: business logic in service classes, called from triggers, LWC, flows, and APIs
- **Selector Pattern**: SOQL queries isolated in selector classes for reuse and mockability
- **Domain Layer**: object-specific validation and behavior in domain classes
- **LWC Composition**: parent orchestrator with child display/input components

### Development Order

Build in this order for proper dependency resolution:

1. **Handler classes**: trigger handlers with before/after method stubs
2. **Service classes**: business logic implementation
3. **Selector classes**: SOQL query methods
4. **Domain classes**: object-specific validation and behavior
5. **Wire together**: connect trigger, handler, service, selector, domain

### Verify API Signatures

Use Context7 MCP (`resolve-library-id` then `query-docs`) to verify current Apex API signatures before writing code. Salesforce APIs change between releases.

---

## LWC Development

Follow the **LWC Composition pattern** from `skills/sf-architect-solutioning/references/architectural-patterns.md`:

- **Parent orchestrator** with child display/input components
- **SLDS compliance**: use Salesforce Lightning Design System classes and components
- **@wire for reads**: use `@wire` adapters for data retrieval
- **Imperative for writes**: use imperative Apex calls for DML operations
- **Accessibility**: include `aria-` attributes, keyboard navigation, and screen reader support
- **Error handling**: display user-friendly error messages, log technical details

---

## Test Development

### Coverage Target

**85%+ code coverage** minimum. Aim for meaningful coverage, not just line execution.

### Required Test Scenarios

- **Bulk tests**: test with 200+ records to verify bulkification
- **Permission tests**: test with different user profiles/permission sets to verify CRUD/FLS enforcement
- **Boundary tests**: test edge cases, null values, empty collections, maximum field lengths
- **Negative tests**: test invalid inputs, missing required fields, duplicate detection
- **Mock callouts**: use `HttpCalloutMock` and `Test.setMock()` for external integrations

### Test Data

- Use **TestDataFactory** pattern for centralized test data creation methods
- Never rely on existing org data. All test data must be created in the test context.
- Use `@TestSetup` methods for shared test data across test methods in a class

---

## Key Reminders

- **Use Context7.** Verify current API signatures and patterns before writing code.
- **Governor limits are non-negotiable.** Design for the largest data volumes the client expects.
- **CRUD/FLS enforcement in every Apex class.** Security is not optional.
- **Declarative first.** Evaluate if a Flow, validation rule, or formula field can solve the problem before writing Apex.
