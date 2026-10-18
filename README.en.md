# Open Banking Account Information Sharing Platform
### Business Analysis Portfolio Project | Banking and Digital Solutions

[![Domain](https://img.shields.io/badge/domain-Open%20Banking-blue)]()
[![Focus](https://img.shields.io/badge/focus-Business%20Analysis-orange)]()

---

[**Turkish version**](README.md)

## About the Project

This repository presents a business analysis case study based on a fictional bank's need to securely 
share customers' account information with authorized third-party providers (TPPs) within the scope of Open Banking.

The study covers business problem definition, current-state and target-state process analysis, 
stakeholder and risk assessment, requirements documentation, solution approach, API specification, and test case design.
It is not a production system of a real bank; the scenarios, endpoints, and measurements are project assumptions.

## Business Problem

Managing account information sharing through manual and file-based processes can lead to operational delays, 
traceability issues, and compliance risks. This project explores how these processes could be transformed 
into a target operating model supported by standardized APIs, digital consent management, and audit logs.

## Business Goals

- Define a standardized API approach to support Open Banking requirements.
- Manage the creation, viewing, and revocation of customer consent.
- Structure the registration and integration processes for third-party providers (TPPs).
- Document access, security, and audit requirements.
- Design an operating model aimed at reducing manual processes.

## Scope

**In scope:** Account Information Service (AIS), Consent Management, TPP onboarding, access and authorization, audit, and compliance reporting.

**Out of scope:** Payment initiation services, actual core banking development, and live integration in a production environment.

## Key Deliverables

| Area | Deliverables |
|---|---|
| Discovery and business case | Business case, stakeholder analysis, risk register |
| Process analysis | As-Is analysis, root cause analysis, gap analysis, To-Be design |
| Solution approach | High-level solution architecture |
| Requirements | BRD, business rules, epics/user stories, acceptance criteria, SRS |
| Prioritization | Requirements prioritization matrix |
| Test design | Functional, security, negative, integration, and UAT scenarios |
| API documentation | OpenAPI 3.0.3 / Swagger YAML |
| Modeling | UML and process modeling package |

## Folder Structure

```text
01-discovery/          Business case, stakeholder analysis, and risks
02-process-analysis/   As-Is, root cause, gap, and To-Be analysis
03-architecture/       Solution architecture overview
04-requirements/       BRD, user stories, acceptance criteria, SRS, and prioritization
05-testing/            Test case design
06-api-specification/  OpenAPI / Swagger documentation
07-process-modeling/   UML and process modeling
```

## How to Review the Project

The following sequence is recommended for reviewing the project:

1. [Business Case](01-discovery/business-case.md)
2. [As-Is Process Analysis](02-process-analysis/as-is-process-analysis.md)
3. [Root Cause Analysis](02-process-analysis/root-cause-analysis.md) and [Gap Analysis](02-process-analysis/gap-analysis.md)
4. [To-Be Process Design](02-process-analysis/to-be-process-design.md)
5. [Business Requirements Document (BRD)](04-requirements/brd-business-requirements-document.md)
6. [Epic and User Story Model](04-requirements/epic-and-user-story-model.md) and [Acceptance Criteria](04-requirements/acceptance-criteria.md)
7. [Software Requirements Specification (SRS)](04-requirements/srs-software-requirements-specification.md)
8. [Test Case Design](05-testing/test-case-design.md)
9. [OpenAPI / Swagger Specification](06-api-specification/swagger.yaml)

## Approaches Used

- Business analysis approach inspired by BABOK
- Stakeholder analysis and risk assessment
- As-Is / To-Be and gap analysis
- BRD, SRS, business rules, and requirements traceability
- Agile user stories and Given/When/Then acceptance criteria
- MoSCoW prioritization
- UML and process modeling
- Risk-based manual test case design
- API contract documentation using OpenAPI / Swagger

## Project Limitations

This is a fictional case study created for portfolio purposes. It does not connect to real banking systems; example API endpoints and data are representative only. 
The target KPIs are assumptions proposed for evaluation, not measured results. 
The test documents describe test design and should not be interpreted as test results executed against a live system.
