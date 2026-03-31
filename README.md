# 6G-DALI MLOps Service Layer — DBML Schemas

This repository contains the DBML (Database Markup Language) entity-relationship schemas of the **6G-DALI MLOps Service Management Layer**, developed by Nextworks within the [[6G-DALI](https://6gdali.eu)] project (Grant Agreement No. 101139161, funded by the European Union under the Horizon Europe programme).

The schemas document the data model of the MLOps Service Layer backend, grounding each entity in the **3GPP TS 28.105** information model where applicable and clearly marking DALI-specific extensions and new contributions.

---

## Repository Structure

| File | Domain | Description |
|------|--------|-------------|
| `schema_model_training.dbml` | Model Catalogue + Training | Focused view: `MLModelEntry`, `MLModel`, `MLModelMetadata`, and the TS 28.105 `MLTrainingRequest` / `MLTrainingProcess` / `MLTrainingReport` triple |
| `schema_catalogue.dbml` | Model Catalogue | `MLModelEntry`, `MLModel` (with self-referencing lineage), `MLModelMetadata` |
| `schema_deployment.dbml` | Deployment | `MLDeploymentRequest` / `MLDeploymentProcess` / `MLDeploymentReport` triple + `DeployedMLModel` |
| `schema_admin.dbml` | Administration | `User`, `Project`, `ProjectMembership`, `AccessPolicy` |
| `schema_testbeds.dbml` | Testbed | `Testbed`, `HWCapabilities`, `MLToolsStack`, `MLTool` |
| `schema_full.dbml` | All domains | Complete schema combining all the above |

---

## Field Origin Convention

Every field carries a `[note]` annotation indicating its provenance:

| Tag | Meaning |
|-----|---------|
| `[28.105 Top]` | Top IOC fields inherited from TS 28.105 (grounded in TS 28.622): `id`, `created_at`, `updated_at`, `managed_by` |
| `[28.105]` | Field directly grounded in a TS 28.105 IOC attribute, preserved in name and semantics |
| `[DALI ext]` | DALI-specific extension of an existing TS 28.105 attribute (widened vocabulary, structured payload, or additional enum values) |
| `[DALI new]` | New field introduced from scratch as part of the 6G-DALI contribution, with no analogue in the current standards |

---

## Visualising the Schemas

The `.dbml` files can be imported directly into **[dbdiagram.io](https://dbdiagram.io)**:

1. Open [dbdiagram.io](https://dbdiagram.io)
2. Click **Import** → **Import from DBML**
3. Paste or upload the desired `.dbml` file
4. The E-R diagram is rendered automatically and can be exported as PNG, PDF, or SVG

---

## Standards Baseline

The information model is grounded in the following 3GPP specifications:

- **3GPP TS 28.105** — Management of artificial intelligence/machine learning (AI/ML) model lifecycle for mobile networks
---

## Context

These schemas are part of the initial implementation results of the **MLOps Service Management Layer** described in Deliverable D3.1 of the 6G-DALI project. The Service Layer provides a unified, tool-agnostic management interface for the full ML lifecycle — covering training, validation, deployment, monitoring, hyperparameter optimisation, federated learning, transfer learning, and trustworthiness assessment — exposed as AIaaS REST APIs to experimenters and administrators across the 6G-DALI testbed federation.

---

## Authors
Nextworks S.r.l.  
