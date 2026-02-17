# Metadata-Driven Model

## Documentation as Execution Input

Ingestia is built on a simple but transformative idea:

**Metadata does not describe the system. Metadata drives the system.**

Instead of spreading structural definitions across notebooks, pipelines, and implicit conventions, Ingestia centralizes execution parameters inside explicit dictionaries.

These dictionaries are not passive documentation artifacts.

They are consumed directly by the ingestion engine.

---

## Catalog, Schema, and Layering

Ingestia assumes a **single catalog** at the platform level.

Within that catalog, schemas are defined by combining:

```
<layer>_<domain>
```

Examples:

- `raw_sales`
- `transformation_sales`
- `serving_sales`

The layer comes first because lifecycle responsibility precedes business domain.

This structure enforces architectural clarity at the metadata level.

---

## Table-Centric Dictionary

Ingestia does not define generic models.

Each table has its own explicit dictionary.

For example:

- `sellout`

The dictionary defines how this table behaves within its layer and domain.

---

## Example: Sellout Data Dictionary

Below is a simplified example of a dictionary used for a `sellout` table in the `transformation_sales` schema.

```json
{
  "catalog": "main",
  "layer": "transformation",
  "domain": "sales",
  "table": "sellout",
  "purpose": "standardized",
  "write_mode": "merge",
  "enable_control_columns": true,
  "partition_by": ["period_id"],
  "columns": [
    { "name": "data_provider_cd", "type": "string", "nullable": false },
    { "name": "period_id", "type": "int", "nullable": false },
    { "name": "ean_cd", "type": "string", "nullable": false },
    { "name": "store_cd", "type": "string", "nullable": false },
    { "name": "sellout_qt", "type": "decimal(18,4)", "nullable": true },
    { "name": "sellout_vl", "type": "decimal(18,4)", "nullable": true }
  ]
}
```

This dictionary defines:

- Where the table belongs (layer + domain)
- How it should be written (`merge`)
- Whether control columns are enabled
- How it is partitioned
- The structural schema of the table

Execution logic is not hard-coded in notebooks.

It is parameterized.

---

## Separation of Concerns

The metadata model separates:

- Structural definition (columns, types, nullability)
- Execution behavior (write mode, partitioning)
- Lifecycle responsibility (layer)
- Business alignment (domain)

Additional dictionaries (such as integrity definitions) extend this structure without polluting the core definition.

Each concern is explicit and versionable.

---

## Why This Matters

Without a metadata-driven approach:

- Schema evolution becomes inconsistent.
- Naming conventions drift.
- Integrity rules are optional.
- Reproducibility depends on individual developers.
- Governance lives outside execution.

With a metadata-driven model:

- Standards are enforced.
- Behavior is deterministic.
- Architecture becomes executable.
- Delivery speed increases without sacrificing control.

Metadata becomes the contract between architecture and execution.

That contract is what makes Ingestia scalable.
