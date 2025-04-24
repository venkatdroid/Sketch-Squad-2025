# **ADR-04: Adopting a Three-System Architecture with EDA and Microservices**

## Date:

2025-03-26

## **Status**

Accepted

## **Context**

The current interview scheduling process at MindCompute is inefficient, requiring manual intervention. To streamline this, we are designing an event-driven system that integrates with external tools (InterviewLogger, MindComputeScheduler, MyMindComputeProfile) and consists of three core services:

1. **Interview Management System** – Manages interviews, schedules, and updates.

2. **Chatbot System** – Provides an interactive interface for recruiters.

3. **Recruitment Analytics** – Generates dashboards and reports for decision-making.

We prioritize the following architectural characteristics:

- **Security**: Ensuring data privacy and access control.

- **Performance**: Optimizing event processing and reducing latency.

- **Extensibility**: Allowing easy integration of future features and external systems.

## **Decision**

We will implement the Interview Accelerator System(IAS) with **three distinct microservices**, communicating asynchronously via **a Centralized streaming platform for event-driven processing** while enforcing **strict security controls** at each layer.

- **Interview Management System**: Handles scheduling, interviewer selection, and calendar integration.

- **Chatbot System**: Facilitates recruiter interactions and interview scheduling assistance.

- **Recruitment Analytics**: Aggregates and visualizes interview trends while ensuring controlled access to sensitive data.

Each service will publish/subscribe to **streaming topics** to ensure real-time event propagation while maintaining independence.

## **Rationale**

We chose a three-system architecture with EDA and microservices for the following reasons:

1. **Separation of Concerns & Domain Specialization**

- Each system has a unique operational model: scheduling, chatbot interactions, and analytics. A unified platform would introduce unnecessary dependencies, making it harder to maintain and evolve.

- Dividing into separate microservices aligns with **Domain-Driven Design (DDD)** by keeping domains distinct and manageable.

2. **Enhanced Maintainability & Extensibility**

- By isolating concerns, teams can develop, deploy, and update services independently.

- New features (e.g., advanced AI for chatbots, predictive analytics) can be added without affecting core interview scheduling.

3. **Performance Optimization**

- The chatbot and analytics services require different scaling strategies compared to the interview service.

- Chatbot interactions are real-time and demand low latency, while analytics involves heavy computations that can be processed asynchronously.

4. **Event-Driven & Loose Coupling**

- EDA ensures minimal dependencies between services. Events such as **InterviewScheduled**, **CandidateContacted**, and **InsightsGenerated** allow services to interact asynchronously.

- Ensures resilience, as a failure in one service does not impact others.

## **Alternatives Considered**

| Approach                                         | Security                                       | Performance                               | Extensibility                               | Trade-offs                                           |
| ------------------------------------------------ | ---------------------------------------------- | ----------------------------------------- | ------------------------------------------- | ---------------------------------------------------- |
| **Monolithic System**                            | ❌ Weak (single failure point)                 | ✅ High (direct DB calls)                 | ❌ Low (hard to modify)                     | Harder to scale and evolve                           |
| **API-driven Microservices**                     | ✅ Strong (RBAC, OAuth)                        | ⚡ Medium (sync API calls)                | ✅ High (new APIs can be added)             | Direct API calls create tight coupling               |
| **Event-Driven Microservices (Chosen Approach)** | ✅✅ Strong (secure messaging, encrypted data) | ⚡⚡ High (async processing, low latency) | ✅✅ High (easy to extend, loosely coupled) | Requires message schema design, eventual consistency |

## **Consequences & Trade-offs**

✔ High **security** with RBAC, encryption, and authentication.
✔ Optimized **performance** with Centralized streaming platform, Caching system, and ow latency comm. protocol.
✔ Future-proof **extensibility** with modular design.

### 🔗 **Integration Points (Key Technologies)**

| System                          | Integration                                                                        | Event Communication                                                                                  |
| ------------------------------- | ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **Interview Management System** | MindComputeScheduler (Interview Scheduling), InterviewLogger (Candidate Info)                       | Centralized Streaming Platform (application.created, application.updated, interview.scheduled. etc.) |
| **Chatbot System**              | Natural Language Processing (NLP), Internal APIs (MindComputeScheduler, InterviewLogger and MyMindComputeProfile) | Centralized Streaming Platform                                                                       |
| **Recruitment Analytics**       | IMS,MindComputeScheduler and InterviewLogger, Use of charts for interview status                    | Centralized Streaming Platform                                                                       |

## **Final Decision**

We will **adopt a three-system architecture using EDA and microservices**, prioritizing **security, performance, and extensibility**. Centralized Streaming Platform will serve as the backbone for event-driven communication, while **RBAC, encryption, and OAuth2** ensure security.
