# Metadata-Driven Architecture

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

The dictionary defines how this table behaves within its layer and domain.

---

# Example: Sellout Data Dictionary

Below is a simplified example of a dictionary used for a table
in the schema.

``` json
{
  "data_dictionary": {
    "<table_name>": {
      "table": {
        "catalog": "main",
        "schema": "<layer>_<domain>",
        "domain": "<domain>",
        "container": "raw | transformation | serving",
        "description": "<table description>",
        "purpose": "raw | standardized | conformed | serving",

        "write_mode": "append | overwrite | merge",
        "merge_schema": true,

        "table_properties": {
          "delta.autoOptimize.optimizeWrite": "true",
          "delta.enableChangeDataFeed": "true"
        }
      },

      "columns": {
        "<source_column_name>": {
          "target_column": "<target_column_name>",
          "data_type": "string | int | bigint | decimal(18,2) | date | timestamp | boolean",
          "nullable": true,
          "transform_expr": "<spark_sql_expression_optional>",
          "key": false,
          "partition": false,
          "z_order": false,
          "delta_column": false
        }
      },

      "quality_rules": {
        "<rule_name>": {
          "type": "expression | not_null | in_list | regex | range | custom",
          "active": true,
          "severity": "ERROR | WARN | QUARANTINE | SKIP",
          "expr": "<spark_sql_boolean_expression_optional>",
          "params": {}
        }
      },

      "constraints": {
        "<constraint_name>": {
          "type": "primary_key | foreign_key | unique | check",
          "active": true,
          "severity": "ERROR | WARN | QUARANTINE | SKIP",
          "columns": ["<col1>", "<col2>"],

          "reference": {
            "catalog": "main",
            "schema": "<ref_schema>",
            "table": "<ref_table>",
            "columns": ["<ref_col1>"]
          },

          "check_expr": "<spark_sql_boolean_expression_optional>"
        }
      }
    }
  }
}
```

## Separation of Concerns

The metadata model separates concerns inside a single, structured
dictionary:

-   Structural definition (`columns`, types, nullability)
-   Execution behavior (`write_mode`, partitioning, table properties)
-   Data quality validation (`quality_rules`)
-   Integrity enforcement (`constraints`)
-   Lifecycle responsibility (`container`)
-   Business alignment (`domain`)

Each concern is explicit, declarative, and versionable.

No execution logic is hard-coded.

The ingestion engine consumes this dictionary and applies behavior
deterministically.

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
