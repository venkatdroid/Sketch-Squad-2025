# IAS Data Management Strategy (Final Revision)

## 1. Introduction & Principles

This document outlines the data management strategy for the Interview Accelerator System (IAS), guided by Domain-Driven Design (DDD - [ADR-01](/ADR/ADR-01-Use%20Domain%20Driven%20Design.md)), an Event-Driven Architecture (EDA) using microservices ([ADR-04](/ADR/ADR-04-Adopting%20a%20Three-System%20Architecture%20with%20EDA%20and%20Microservices.md)), and integration with existing COTS systems ([ADR-02](/ADR/ADR-02-Analyzing%20COTS%20for%20ATS.md)). The strategy prioritizes supporting the key architectural characteristics:

- Performance
- Maintainability
- Extensibility

alongside other important NFRs like Security and Resilience ([ADR-03](/ADR/ADR-03-Selecting%20Core%20Architectural%20Characteristics.md)).

**DDD Alignment**: Data ownership aligns with Bounded Contexts (BCs).

**Source of Truth**: External systems (InterviewLogger, MindComputeScheduler) are the SoT for their respective data; IAS references or caches this data via APIs protected by Anti-Corruption Layers (ACLs - [ADR-01](/ADR/ADR-01-Use%20Domain%20Driven%20Design.md)).

**Event-Driven Data Flow**: Domain events (identified during Event Storming, e.g., Interview meeting scheduled, Interviewer declined) are the primary means of propagating state changes between BCs via the Centralized Streaming Platform ([ADR-04](/ADR/ADR-04-Adopting%20a%20Three-System%20Architecture%20with%20EDA%20and%20Microservices.md)), supporting Extensibility (new consumers) and Performance (asynchronous processing).

**Data Segregation**: Each microservice owns and manages its data independently, enhancing Maintainability and Modularity.

**Eventual Consistency**: Accepted across distributed services to optimize Performance and availability over strong consistency.

## 2. Data Ownership & Boundaries (Per ADR-01 & Event Storming)

| Bounded Context (BC)              | Data Owned                                                                                                                                                                                                                                                                                    | Data Referenced / Cached                                                                                                                                                                            |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Interview Management System (IMS) | Core scheduling workflow state (e.g., aggregates like Interview Schedule, Panelist Assignment Status); IAS-specific configurations; Lifecycle events managed (e.g., Interviewers identified, Availability slots shared, Interview meeting scheduled, Interviewer declined scheduled meeting). | Candidate/Job data (from InterviewLogger via ACL); Panelist tags/availability (from MindComputeScheduler via ACL); Calendar data (via ACL).                                                                          |
| Recruitment Analytics             | Aggregated, transformed data optimized for reporting (e.g., interviewer load, scheduling efficiency, time-to-schedule metrics).                                                                                                                                                               | Consumes domain events (e.g., Interview completion, interviewer load updates) from the Centralized Streaming Platform.                                                                              |
| Chatbot                           | Conversation state: User interaction history related to chatbot queries (e.g., Interviewer recommendation requests, scheduling inquiries - ADR-01).                                                                                                                                          | Data via APIs from IMS (e.g., schedule status); Potentially read-only data from external systems (e.g., interviewer search).                                                                        |
| External Systems                  | N/A (External to IAS)                                                                                                                                                                                                                                                                         | InterviewLogger (Candidate, Job Profiles, Application Status); MindComputeScheduler (Panelist Tags, Availability, Scheduling Actions); Calendars (Availability); MyMindComputeProfile (Org Structure). These are Sources of Truth. |

## 3. Data Storage (Characteristic-Based)

The choice of storage technology type per service balances functional needs with Performance, Maintainability, and Extensibility:

### IMS:

**Primary State Store**: Requires transactional consistency. A relational database technology provides this reliability. Data modeling should consider query Performance for scheduling logic.

**Event Log (Recommended)**: Using event sourcing patterns or a transactional outbox pattern ensures reliable publishing of domain events to the streaming platform. This enhances Maintainability (auditability, clear state change history) and Extensibility (easy addition of new event consumers).

**Cache**: An in-memory data grid/cache technology is crucial for Performance, reducing latency by caching frequently accessed external data (e.g., availability, tags) and potentially internal read-models.

### Recruitment analytics:

**Primary Store**: Requires optimization for complex analytical queries. A Data Warehouse or analytical database technology provides the necessary query Performance and supports Extensibility for new types of reports. Schema design should balance query needs with Maintainability.

### Chatbot:

**State Store**: Needs fast read/write access for session data. A key-value or document database technology offers simplicity and Performance for this use case. Flexible schemas can aid Extensibility.

## 4. Data Integration & Consistency

**Internal (IAS Services)**: Asynchronous communication via the Centralized Streaming Platform using domain events identified in Event Storming. This decoupling enhances Maintainability and Extensibility.

**External Systems**: Interactions via APIs/Webhooks managed through Anti-Corruption Layers (ACLs). ACLs are vital for Maintainability and Extensibility by isolating the IAS domain from external system changes and complexities. Caching (see Storage) is critical for Performance and resilience when dealing with external APIs.

**Consistency Management**: Eventual consistency is the default. For critical workflows spanning services/systems (identified via Event Storming), the Saga pattern will be used to manage logical transactions and ensure eventual consistency or compensation, balancing consistency needs with Performance. Idempotency in handlers supports resilience and data integrity.

## 5. Data Quality & Governance

**Validation**: Implement validation at BC boundaries (APIs, event consumers) for data integrity.

**Monitoring**: Monitor data synchronization, API health, event flows, and data consistency metrics to ensure reliability and aid Maintainability.

**Ownership & Documentation**: Clear documentation of data ownership, sources of truth, and schemas enhances Maintainability. Define processes for managing shared reference data (e.g., skill tags - Assumptions A.9, A.10).

## 6. Data Security & Privacy (ADR-03 Priority)

Implement appropriate security controls (access control, encryption) for data stores and APIs to protect sensitive information.

## 7. Analytics Data Flow (Aligning with ADR-04 & Event Storming)

IMS (and potentially other services) publishes domain events (e.g., Interview meeting scheduled, Application moved to actionable stage) to the Centralized Streaming Platform.

An ETL/ELT process within the Analytics BC consumes these events asynchronously.

Data is transformed, aggregated, and loaded into the dedicated Analytics Data Store. This decoupled flow supports Maintainability and Extensibility of the analytics component.

Analytics dashboards (via SPA) and APIs query this optimized store for reporting Performance.
