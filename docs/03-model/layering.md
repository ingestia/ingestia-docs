# Layering Strategy

## The Medallion Concept — and Its Adaptation

The Medallion Architecture (Bronze, Silver, Gold) was popularized in the Lakehouse ecosystem as a structured way to organize data refinement.

Ingestia adopts this concept but applies it with explicit operational boundaries and governance rules.

Instead of blindly replicating the pattern, the framework defines clear responsibilities for each layer — both physical and logical.

Ingestia operates with three physical layers:

- **Raw (Bronze)**
- **Transformation (Silver)**
- **Serving (Gold)**

Within the Transformation layer, two logical stages are defined:

- **Standardized**
- **Conformed**

This distinction is essential for maintaining clarity and scalability.

---

## Raw (Bronze)

Raw is not a transformation layer.

It is a storage layer.

In this layer:

- Data is stored exactly as received.
- No business transformation is applied.
- No Spark-managed tables are created.
- Ingestion writes directly to the lake storage.
- Partitioning is controlled by ingestion metadata.

Key principles:

- Raw data is never modified.
- Raw data is never deduplicated.
- Raw data is never merged.
- Raw preserves ingestion history.

Raw exists to guarantee traceability and reproducibility.

It is the source of truth for ingestion — not for analytics.

---

## Transformation (Silver)

From this layer forward, data is treated as structured tables.

Transformation is divided logically into two stages:

### Standardized

The Standardized stage focuses on structural alignment.

Here we:

- Apply type casting.
- Normalize column names.
- Enforce naming conventions.
- Introduce control columns.
- Apply structural integrity validations.
- Preserve provider-specific grain.

Important:

- No cross-provider merging occurs here.
- No aggregation is performed.
- The objective is structural consistency, not canonical modeling.

Standardized prepares data for integration without altering its semantic origin.

---

### Conformed

The Conformed stage unifies data across sources and domains.

Here we:

- Align canonical keys.
- Merge multiple providers.
- Harmonize domain logic.
- Define cross-source models.
- Maintain controlled duplication when necessary.

Data may be reorganized, but its transformation remains governed and traceable.

Conformed models represent enterprise-aligned structures — not consumption-optimized artifacts.

---

## Serving (Gold)

Serving is the consumption layer.

Here, the focus shifts from structural integrity to purpose optimization.

In this layer:

- Redundancy is allowed and intentional.
- Data may be aggregated.
- Models may be reshaped for BI, reporting, or data science.
- Performance and access patterns are prioritized.

Examples include:

- Power BI data marts
- Feature tables for machine learning
- Domain-specific analytical views

All redundancy in Serving must be governed by the controlled origin defined in Transformation.

Serving is optimized.  
Transformation is disciplined.

---

## Physical vs Logical Separation

Ingestia separates layers physically:

- Raw, Transformation, and Serving are isolated at storage level.

Within Transformation, Standardized and Conformed are logically separated to preserve clarity of responsibility.

This approach:

- Prevents accidental cross-layer leakage.
- Improves governance.
- Simplifies troubleshooting.
- Supports scalable domain growth.

---

## Why This Matters

Without clear layering boundaries:

- Raw becomes polluted.
- Standardized becomes business-driven.
- Conformed becomes uncontrolled.
- Serving becomes a shadow warehouse.

Layering is not about aesthetics.

It is about enforceable discipline.

---

## References

The Medallion Architecture was popularized in the Lakehouse paradigm, particularly within the Databricks ecosystem.

Ingestia adopts the layered refinement principle while introducing explicit governance, metadata-driven execution, and operational enforcement across layers.