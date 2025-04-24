
# ADR-05-Chatbot Natural Language Processing Strategy

## Date:

2025-04-03

## Status:

Proposed

## Context:

The Interview Accelerator System (IAS) includes a Chatbot service intended to help Recruiters find interviewers and get information using natural language queries (Requirement FR.3.2). This requires a Natural Language Processing (NLP) capability within the Chatbot Service Layer to interpret user requests and extract necessary information (intent, entities). A decision is needed on the required level of NLP sophistication, balancing capabilities against complexity, cost, and alignment with architectural characteristics (Performance, Maintainability, Extensibility).

## Decision:

We will implement the initial version of the Chatbot Service using LLM-based NLP primarily for Natural Language Understanding (NLU).

1. NLU Focus: An LLM will be integrated within or called by the Chatbot Service Layer to interpret the recruiter's free-text queries, identify intent (e.g., search panelists, check status), and extract key entities (e.g., skills, dates, names).

2. Action Mapping: The Chatbot Service logic will map the understood intent and entities to specific, relatively fixed actions, primarily involving calls to the IMS API (via the API Gateway) to fetch data or trigger processes.

3. NLG (Optional/Simple): Natural Language Generation (NLG) for responses may use the LLM for more conversational replies or start with simpler template-based responses.

4. Agentic AI as Future Evolution: Full "Agentic AI" capabilities (autonomous multi-step planning, complex tool/API orchestration based on high-level goals) are deferred as a future enhancement, supporting the Extensibility characteristic. The initial focus is on robust NLU and reliable action execution.


This approach leverages the power of LLMs for understanding natural language effectively, meeting FR.3.2, while managing initial implementation complexity.

## Alternatives Considered:


| Alternative                                | Rationale for Rejection                                                                                                                                                                                                                               |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Simpler NLP Techniques (Non-LLM)           | Less robust for handling variations in natural language (FR.3.2); potentially poor user experience; limits future Extensibility towards more complex conversational interactions. Might require significant rule/model maintenance (Maintainability). |
| Full Agentic AI (Immediate Implementation) | Significantly higher initial complexity, cost, and development time; increased risk associated with autonomous multi-step execution; better addressed as an evolution (Extensibility) once core functionality is stable and validated.                |
| No NLP (Structured Input Only)             | Does not meet the explicit requirement (FR.3.2) for natural language interaction.                                                                                                                                                                     |

## Consequences:


| Type     | Consequence                                                                                                                           | Alignment with Characteristics (Perf/Maint/Ext) |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| Positive | Provides robust Natural Language Understanding (NLU) to meet core requirements (FR.3.2).                                              | (Functionality)                                 |
| Positive | Leverages LLM capabilities for a better, more flexible user experience compared to simpler NLP.                                       | (Usability)                                     |
| Positive | Establishes a technical foundation (LLM integration) that supports future evolution towards Agentic AI capabilities.                  | Extensibility                                   |
| Positive | Keeps the initial Chatbot Service logic relatively focused on mapping understood requests to actions, aiding initial Maintainability. | Maintainability                                 |
| Negative | Requires integration with an LLM (via API or self-hosted model), adding technical complexity.                                         | Maintainability                                 |
| Negative | Potential operational costs associated with LLM usage (API calls or hosting).                                                         | (Cost)                                          |
| Negative | Requires effort in prompt engineering or fine-tuning to ensure accurate NLU for the specific domain (skills, names, dates etc.).      | Maintainability                                 |
| Negative | Latency of LLM calls for NLU could impact the perceived Performance of the chatbot's responsiveness.                                  | Performance                                     |
| Negative | Defers more advanced autonomous capabilities (Agentic AI) to a later phase.                                                           | (Scope)                                         |


**