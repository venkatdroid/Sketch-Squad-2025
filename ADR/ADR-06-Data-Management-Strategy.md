# ADR-06: Data Management Strategy

## Date:

2025-04-03

## **Status**:

Proposed

## **Context**:

The Interview Accelerator System (IAS) is designed using Domain-Driven Design (DDD - ADR-01) and an Event-Driven Architecture (EDA) with distinct microservices for Interview Management (IMS), Recruitment Analytics, and Chatbot functionalities (ADR-04). Each service has unique data storage and access requirements. A decision is needed on the appropriate types of data storage technologies for each component to effectively support their function while aligning with the prioritized architectural characteristics: Performance, Maintainability, and Extensibility.

## **Decision**:

We will adopt a polyglot persistence approach, selecting storage technology types based on the specific needs of each service and the prioritized architectural characteristics:

### Interview Management System (IMS):

**Primary State Store**: Utilize a relational database technology for its transactional consistency (ACID properties) required for managing the core scheduling workflow state reliably. Query performance will be a key consideration in data modeling.

**Event Log (Recommended)**: Implement event sourcing patterns or a transactional outbox pattern (potentially leveraging the relational store or a dedicated event store technology) to reliably publish domain events. This supports Maintainability (auditability) and Extensibility (new consumers via the event stream).

**Cache**: Employ an in-memory data grid/cache technology to store frequently accessed external data (e.g., availability, tags) and potentially read-models, significantly boosting read Performance and system resilience.

### Recruitment Analytics Service:

**Primary Store**: Utilize a Data Warehouse or analytical database technology optimized for complex analytical queries and reporting workloads, ensuring data retrieval Performance and supporting Extensibility for future reporting needs.

### Chatbot Service:

**State Store**: Utilize a key-value or document database technology for fast read/write access required for managing conversation state, optimizing for Performance. Its flexible schema can aid Extensibility.

## **Alternatives Considered**:

| **Alternative**                             | **Rationale for Rejection**                                                                                             |
| ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Single Database Type for All Services       | Compromises performance and suitability for specialized needs; doesn't align well with microservice principles.         |
| IMS: Relational Only                        | Simpler, but lacks robust event publishing/auditing capabilities, impacting Maintainability and Extensibility via EDA.  |
| IMS: NoSQL Only                             | Poor fit for the transactional consistency needed for IMS workflow state characteristics.                               |
| Analytics: Querying Operational Replica     | Simpler setup but risks performance impact on the operational IMS database and is not optimized for analytical queries. |
| Analytics: Querying Operational DB Directly | Creates tight coupling, high performance risk, hinders Maintainability and independent scaling.                         |
| Chatbot: Relational DB                      | Considered overkill and potentially less performant for simple key-value state lookup.                                  |

## **Consequences**:

| **Type** | **Consequence**                                                                   | **Alignment with Characteristics (Perf/Maint/Ext)** |
| -------- | --------------------------------------------------------------------------------- | --------------------------------------------------- |
| Positive | Optimizes storage choice for each service's specific needs (Polyglot Persistence) | Performance, Maintainability                        |
| Positive | Directly supports Performance via appropriate technology types                    | Performance                                         |
| Positive | Enhances Maintainability (separation of concerns, ownership, auditability)        | Maintainability                                     |
| Positive | Improves Extensibility (decoupling, events, flexible schemas where needed)        | Extensibility                                       |
| Positive | Reinforces EDA and Microservices architecture (ADR-04)                           | Maintainability, Extensibility                      |
| Negative | Increases operational complexity (managing multiple data storage tech types)      | Maintainability                                     |
| Negative | Requires broader technical expertise across different database paradigms          | Maintainability                                     |
| Negative | Requires careful management of eventual consistency between services              | Maintainability, Performance                        |
| Negative | Introduces data latency for the Analytics service (event-driven pipeline)         | Performance                                         |
| Negative | Implementation complexity (Event Sourcing/Outbox, Sagas if needed)                | Maintainability                                     |
