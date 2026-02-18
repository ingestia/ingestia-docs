# Architecture

## Architecture Is Contextual

There is no universal architecture.

A good architecture is not the most sophisticated one.  
It is the one that is consistently applied, understood, and maintained.

Every client operates within a specific context:

- organizational structure
- data maturity
- regulatory requirements
- platform constraints
- team skillset

Ingestia does not prescribe a rigid architectural model.  
It provides a reference approach that has proven effective in real-world environments and remains adaptable to context.

Consistency matters more than perfection.

---

## The Problem Modern Architectures Face

Modern data platforms must deal with:

- multiple providers and ingestion patterns  
- cross-domain transformations  
- evolving schemas  
- increasing governance expectations  
- distributed teams working in parallel  

Without architectural discipline, ingestion becomes inconsistent.  
Without operational execution, governance becomes theoretical.

Ingestia addresses this gap by aligning architecture, metadata, and execution into a single operational model.

---

## Reference Architectural Model

The reference implementation of Ingestia follows a Lakehouse-based approach composed of structured layers:

- **Raw** — data stored as received
- **Standardized** — typed, cleaned, structurally aligned
- **Conformed** — cross-source canonical models
- **Serving** — optimized for analytical consumption

This layering strategy is not a theoretical exercise.  
It defines responsibilities, boundaries, and transformation rules.

Ingestia also distinguishes between:

- **Platform-level components** (shared execution logic, metadata handling, enforcement mechanisms)
- **Domain-level models** (sales, marketing, finance, etc.)

This separation enables scalability without sacrificing governance.

The detailed rules for each layer are described in the Layering Strategy section.

---

## Methodology First, Technology Second

Ingestia is a methodology first and a set of libraries second.

To operationalize the methodology, a concrete implementation was necessary.  
The initial implementation was developed using:

- PySpark
- Databricks Lakehouse
- Unity Catalog
- Azure Data Lake Storage (ADLS)
- Azure Data Factory (ADF)

These technologies were chosen because they align with the framework’s principles:

- distributed and deterministic processing
- clear separation of storage and compute
- scalable metadata governance
- structured orchestration

However, the architectural principles described in this documentation are not bound to these tools.

The metadata-driven approach, layering strategy, and integrity enforcement model can be implemented in other ecosystems.

The current technology stack represents a pragmatic starting point — not a limitation.

---

## Starting Somewhere

Architecture must eventually leave theory and enter execution.

Ingestia was implemented in Databricks because it provided the closest alignment with the desired operational model at the time of development.

A methodology only becomes real when it is tested under production pressure.

The framework evolved through practical application — across real domains, real providers, and real governance constraints.

As technology evolves, the implementation may evolve.

The philosophy and architectural discipline remain.