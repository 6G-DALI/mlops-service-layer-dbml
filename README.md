# 6G-DALI MLOps Service Layer — DBML Schemas

This repository contains the DBML (Database Markup Language) entity-relationship schemas of the **6G-DALI MLOps Service Management Layer**, developed by Nextworks within the [6G-DALI](https://6gdali.eu) project (Grant Agreement No. 101192750, funded by the European Union under the Horizon Europe programme).

The schemas document the data model of the MLOps Service Layer backend, grounding each entity in the **3GPP TS 28.105** information model where applicable and clearly marking DALI-specific extensions and new contributions.

---

## Repository Structure

`schema_full.dbml` is the canonical source; the per-domain files are focused views generated from it.

| File | Domain | Description |
|------|--------|-------------|
| `schema_full.dbml` | All domains | Complete schema — 27 tables across seven domains |
| `schema_admin.dbml` | Administration | `User`, `Project`, `ProjectMembership`, `AccessPolicy` |
| `schema_testbeds.dbml` | Testbed | `Testbed`, `TestbedProjectAssociation`, `HWCapabilities`, `MLToolsStack`, `MLTool` |
| `schema_catalogue.dbml` | Model Catalogue | `MLModelEntry`, `MLModel` (self-referencing version lineage), `MLModelArtifact`, `MLModelMetadata`, `Dataset` |
| `schema_model_training.dbml` | Model Catalogue + Training | Focused view: the catalogue tables plus the TS 28.105 `MLTrainingRequest` / `MLTrainingProcess` / `MLTrainingReport` triple |
| `schema_deployment.dbml` | Deployment | `MLDeploymentRequest` / `MLDeploymentProcess` / `MLDeploymentReport` triple + `DeployedMLModel` |
| `schema_hpo.dbml` | HPO | `HPOCampaign` registration layer + the `HPORequest` / `HPOProcess` / `HPOReport` triple |
| `schema_orchestrator.dbml` | Orchestrator | `DispatchQueueItem`, `PipelineRegistration` — internal dispatch and pipeline-sync plumbing |

Each focused view is standalone: tables referenced from another domain appear as slim **context stubs** (primary key plus identifying fields) so the file renders on its own, with the authoritative definition named in the stub's note.

---

## Field Origin Convention

Every field carries a `[note]` annotation indicating its provenance:

| Tag | Meaning |
|-----|---------|
| `[28.105 Top]` | Top IOC fields inherited from TS 28.105 (grounded in TS 28.622): `id`, `created_at`, `updated_at`, `managed_by` |
| `[28.105]` | Field directly grounded in a TS 28.105 IOC attribute, preserved in name and semantics |
| `[DALI ext]` | DALI-specific extension of an existing TS 28.105 attribute (widened vocabulary, structured payload, or additional enum values) |
| `[DALI new]` | New field introduced from scratch as part of the 6G-DALI contribution, with no analogue in the current standards |
| `[DALI-MAP]` | Field from the **6G-DALI-MAP** metadata profile, a curated subset of MLDCAT-AP 2.0 with DALI-specific additions |

---

## Model Lifecycle and Versioning

A catalogue entry (`ml_model_entries`) is the stable identity for a model family and holds **one or more** `ml_models` rows — one per version. The first version is the EMPTY shell registered before any training; every training run appends a child version linked through `parent_model_id`, so the full lineage is preserved rather than overwritten.

`lifecycle_state` on a version takes one of:

```
empty → trained → validated → archived
```

Binary artefacts are **not** columns on `ml_models`. They live in `ml_model_artifacts`, scoped to a single version and typed by `kind` (`weights`, `requirements`, `base_image`, `training_pipeline`, `hpo_train_script`, …), each stored either as an S3 object (`s3_key`) or as an external reference (`uri`, e.g. a Docker image).

---

## Visualising the Schemas

The `.dbml` files can be imported directly into **[dbdiagram.io](https://dbdiagram.io)**:

1. Open [dbdiagram.io](https://dbdiagram.io)
2. Click **Import** → **Import from DBML**
3. Paste or upload the desired `.dbml` file
4. The E-R diagram is rendered automatically and can be exported as PNG, PDF, or SVG

All files are validated against the reference [`@dbml/cli`](https://www.npmjs.com/package/@dbml/cli) parser. To check them locally, or to generate DDL:

```bash
npm install -g @dbml/cli
dbml2sql --postgres schema_full.dbml
```

---

## Context

These schemas are part of the initial implementation results of the **MLOps Service Management Layer** described in Deliverable D3.1 of the 6G-DALI project. The Service Layer provides a unified, tool-agnostic management interface for the full ML lifecycle — covering training, validation, deployment, monitoring, hyperparameter optimisation, federated learning, transfer learning, and trustworthiness assessment — exposed as AIaaS REST APIs to experimenters and administrators across the 6G-DALI testbed federation.

The companion OpenAPI specification of the northbound REST API is published in [`mlops-service-layer-openapi`](https://github.com/6G-DALI/mlops-service-layer-openapi).

---

## Authors
Nextworks S.r.l.
