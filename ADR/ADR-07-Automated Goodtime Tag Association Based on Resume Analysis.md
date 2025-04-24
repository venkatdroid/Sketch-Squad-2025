# ADR-07: Automated MindComputeScheduler Tag Association Based on Resume Analysis

## Date:
2025-04-03

## Status:
Proposed

## Context:
Our recruitment process involves uploading resumes to InterviewLogger, where MindComputeScheduler interviewer tags are currently identified and manually entered by recruiters. This manual effort, based on resume content, is inefficient and can be time consuming for recruiters. To streamline this, we plan to automate the suggestion of relevant MindComputeScheduler tags by leveraging an NLP  inside IMS to analyze parsed resume from InterviewLogger and map the extracted skills to MindComputeScheduler's existing tag vocabulary. Recruiters will then verify and adjust these auto-generated tag suggestions within InterviewLogger before they are finalized for use in MindComputeScheduler. This aims to reduce manual work and improve the precision of interviewer selection.

## Decision:
We will implement an integration leveraging the InterviewLogger API and a mechanism to interact with MindComputeScheduler's tag vocabulary (either directly via an API, or through a pre-defined mapping within our IMS). The workflow will be as follows:

- When a new candidate profile is created or a resume is uploaded in InterviewLogger:
    - A InterviewLogger webhook will trigger our internal system (IMS).
    - IMS will use the InterviewLogger API to retrieve the raw resume content.
    - IMS NLP pipeline will extract skills and experiences from the resume.
    - IMS will then map these extracted skills and experiences to the existing tag vocabulary defined in MindComputeScheduler. This mapping logic will reside within IMS.
    - IMS will write the **identified MindComputeScheduler tags** back to **dedicated custom fields** on the candidate's InterviewLogger profile via the InterviewLogger API. These custom fields will be designed for easy recruiter review and editing (e.g., a multi-select dropdown populated with potential MindComputeScheduler tags, or a text field displaying the suggested tags).
- Recruiters will review the auto-generated MindComputeScheduler tags within the candidate's InterviewLogger profile in these custom fields.
- Recruiters can confirm, modify, or remove the suggested MindComputeScheduler tags directly within these InterviewLogger custom fields.
- Once the recruiter has verified and finalized the MindComputeScheduler tags in InterviewLogger, this information will be used by IMS to ensure the correct tags are associated with the candidate in MindComputeScheduler (this step might involve a separate API call from IMS to MindComputeScheduler, depending on MindComputeScheduler's integration capabilities).

**Alternatives Considered:**

| Alternative                                              | Rationale for Rejection                                                                                                                                                                                                                                                            |
| :------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| External Integration with Third-Party Resume Parsing Solutions (e.g., Vettd) | Requires additional integration complexity and costs; less control over tag mapping; reliance on an external vendor.                                                                                                                                           |
| Using Separate Analytics UI for Tag Verification       | Forces recruiters to switch contexts between systems, leading to inefficiencies and potential user errors.                                                                                                                                                                           |
| Direct Integration with MindComputeScheduler API for Resume Parsing (if available) | Might require significant development effort on the MindComputeScheduler integration side; less control over the NLP process.                                                                                                                                                     |
| Manual Tagging Based on MindComputeScheduler Vocabulary             | Highly inefficient, prone to errors, and doesn't scale.                                                                                                                                                                                                                              |

**Consequences:**

| Type     | Consequence                                                                                                                                 | Alignment with Characteristics |
| :------- | :------------------------------------------------------------------------------------------------------------------------------------------ | :----------------------------- |
| Positive | Automates the suggestion of relevant MindComputeScheduler tags based on resume analysis within InterviewLogger, facilitating efficient interviewer matching.   | (Efficiency, Accuracy)         |
| Positive | Reduces recruiter effort in manually identifying and entering MindComputeScheduler tags, increasing efficiency and reducing errors.                      | (Efficiency, Reduced Errors)   |
| Positive | Leverages our IMS NLP capabilities for intelligent tag suggestion based on the MindComputeScheduler vocabulary.                                       | (Intelligent Automation)       |
| Positive | Keeps the tag verification and editing process primarily within the InterviewLogger interface, minimizing context switching.                     | (User Experience, Efficiency)  |
| Positive | Improves the accuracy of interviewer matching by leveraging NLP on resume content.                                                         | (Accuracy)                     |
| Negative | Requires custom development and integration effort with the InterviewLogger API and potentially the MindComputeScheduler API (or a mapping mechanism).        | (Complexity, Cost)             |
| Negative | Requires careful design and maintenance of the mapping logic between extracted skills and MindComputeScheduler tags.                                     | (Maintainability)              |
| Negative | May increase maintenance complexity and require ongoing updates as InterviewLogger and MindComputeScheduler evolve, or as the tag vocabulary in MindComputeScheduler changes. | (Maintainability)              |
| Negative | Needs robust error handling and security measures to protect candidate data during processing and data transfer between systems.            | (Reliability, Security)        |
| Negative | Requires clear understanding of MindComputeScheduler's API capabilities for tag association with candidates.                                           | (Dependency, Integration Risk) |
