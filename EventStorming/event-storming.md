# Event Storming Workshop

## Introduction

In our recent workshop, we used Event Storming decided in [ADR-01](/ADR/ADR-01-Use%20Domain%20Driven%20Design.md) to explore and map out various domains within our business processes. The session focused on capturing and documenting the flow of events, actors, and interactions between systems. This document summarizes our outcomes and serves as a guide to understanding our approach.

## Workshop Goals

- **Discover key domains**: Identify key business domains within IAS (e.g., communication, application tracking, recruitment tools and analytics)
- **Document workflows**: Capture the flow of domain events, commands, actors, and systems.
- **System Design and Challenges**: Identify integration challenges, data consistency, and failure handling.
- **Actor and System Interaction**: Map interactions between recruiters, candidates, interviewers, and systems (MindComputeScheduler, InterviewLogger, MyMindComputeProfile).

## Workshop Outcomes

### 1. **Actors**:
![Actor](/EventStorming/assets/ES-Actors.png)
- We identified the main actors involved in each process. Actors are the individuals or systems that initiate commands/actions and trigger domain events.
- Examples include Recruiter, Interviewer, Candidate, etc.

### 2. **Domain Events**:
![Domain Events](/EventStorming/assets/ES-DomainEvents.png)
- Domain events describe significant occurrences in the business process, such as Interviewer meeting scheduled, Candidate Identified, User logged in , etc.

### 3. **Systems**:
![Systems](/EventStorming/assets/ES-Systems.png)
- We noted the systems that interact with our business processes, such as MindComputeScheduler, InterviewLogger, MyMindComputeProfile, etc.

### 4. **Modelling and Grouping**:
![Modelling and Grouping](/EventStorming/assets/ES-ModellingAndGrouping.png)
- Based on the domain events , actors and the systems , we started to bring them all together to make logical connection to understand the flow between an actor , their command(s) , the usage of systems and the concerned domain events. Thus we grouped such interactions and modelled the domain contexts.

### 5. **Workflow**:
![Workflow](/EventStorming/assets/ES-Iteration-Workflow.png)
- We went over iterations to identify the flow between sequence of events and came up with the workflow diagram as depicted.


## Workshop Process

1. **Domain Event Mapping**:
   We started by mapping the domain events, capturing them on orange sticky notes in mural and placing them in chronological order.

2. **Systems and Actors**:
   We then identified the commands that trigger each event and linked them to the relevant actor responsible where possible and the systems that are involved in triggering the event.

3. **Grouping**:
   We grouped the actors, their commands and usage of systems to trigger a specific event. Such domain event mappings were then grouped to a acheive a common goal or aggregation.

4. **Workkflow Creation**:
   We iterated through the grouped aggregations to identify the sequence of events that consitute a specific bounded context, thereby resulting in a workflow.

## Conclusion

The Event Storming workshop helped us gain a clear understanding of our business processes and uncover important insights. We documented actors, commands, domain events, external systems, and clarified open issues.

For more information on the event storming process, please visit our [Mural](https://app.mural.co/t/twma7655/m/twma7655/1741584722800/06b3ef97467cc9d67e498699352bec078b41188e?sender=ua3f63264dd15bb4402cd0018).