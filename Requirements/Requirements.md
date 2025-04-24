# **Requirements**

## **1. Introduction**

This document outlines the functional and non-functional requirements for the Interview Accelerator System (IAS), designed to automate and optimize the interview scheduling process for MindCompute recruiters.

## **2. Functional Requirements (FR)**

**FR.1: Interview Scheduling Automation**

* **FR.1.1:** The system shall automatically match candidates with suitable interviewers based on job profile, required skills, and interviewer expertise.
* **FR.1.2:** The system shall consider interviewer availability to prevent scheduling conflicts.
* **FR.1.3:** Upon identifying interviewers availability, the system shall generate link for the candidate to select available slot and send it to the candidate.
* **FR.1.4:** Upon selection of availability slot by the candidate, the system shall send seperate invites to the candidate and interviewers for the interview.
* **FR.1.5:** The system shall monitor the invites to the interviewers and attempt reschedules when one of the interviewers declines.

**FR.2: Resource Allocation**

* **FR.2.1:** The system shall identify and assign interviewers based on specified skill sets, roles, and availability.
* **FR.2.2:** The system shall prevent double-booking of interviewers.
* **FR.2.3:** The system shall automatically suggest alternate interviewers when scheduling conflicts arise.

**FR.3: Intelligent Chatbot for Rapid Interviewer Search**

* **FR.3.1:** The system shall provide a chatbot interface accessible through Messenger.
* **FR.3.2:** The chatbot shall allow recruiters to query for interviewers using natural language.
* **FR.3.3:** The chatbot shall return a short list (e.g., top 3) of recommended interviewers based on skill set and availability.
* **FR.3.4:** The chatbot shall provide recommendations and scheduling support.

**FR.4: Notifications & Reminders**

* **FR.4.1:** The system shall send automated reminders to candidates, interviewers, and recruiters.
* **FR.4.2:** The system shall handle declined calendar invites and automatically suggest alternate interviewers and time slots.
* **FR.4.3:** The system shall send confirmation notifications upon interview scheduling.

**FR.5: Integration with Existing Systems**

* **FR.5.1:** The system shall integrate with MindComputeScheduler (to generate interview link, to get the tags, to get availability).
* **FR.5.2:** The system shall integrate with MyMindComputeProfile (to get interviewers role and skills - as a fallback mechanism).
* **FR.5.3:** The system shall integrate with InterviewLogger (to get candidates information and scorecard).
* **FR.5.4:** The system shall provide visibility into interviewer availability and workload.
* **FR.5.5:** The system shall ensure seamless data synchronization across integrated platforms.
* **FR.5.6:** The system shall integrate with Company calendars.

**FR.6: Candidate Interaction**

* **FR.6.1:** The system shall allow candidates to select interview slots based on pre-approved availability.
* **FR.6.3:** The system shall send confirmation notifications upon interview scheduling.

**FR.7: Analytics & Reporting**

* **FR.7.1:** The system shall generate comprehensive reports on interviewer activity, interview completion rates, and other key metrics.
* **FR.7.2:** The system shall provide customizable reporting options to analyze interview data.
* **FR.7.3:** The system shall provide a centralized dashboard for recruiters to view interviewer availability, interview schedules, and interview progress.

## **3. Non-Functional Requirements (NFR)**

**NFR.1: Efficiency & Automation**

* **NFR.1.1: Process Optimization:** The application shall significantly improve the efficiency of the interview scheduling process for recruiters.
* **NFR.1.2: Reduced Manual Intervention:** The system shall automate tasks to minimize manual effort by recruiters, freeing them for higher-value activities.

**NFR.2: Accuracy & Reliability**

* **NFR.2.1: Error Reduction:** The system shall minimize errors such as double-bookings, scheduling conflicts, and missed interviews.
* **NFR.2.2: Data Integrity:** The system shall maintain data consistency and accuracy across all integrated platforms.

**NFR.3: Visibility & Transparency**

* **NFR.3.1: Interviewer Availability:** Recruiters shall have visibility into interviewer availability.
* **NFR.3.2: Interview Load Monitoring:** Recruiters shall have visibility into interviewer workload.
* **NFR.3.3: Progress Tracking:** Recruiters shall have visibility into overall interview progress.

**NFR.4: Interoperability & Integration**

* **NFR.4.1: Seamless Data Exchange:** The application shall effectively interact and exchange data with MyMindComputeProfile, MindComputeScheduler, and InterviewLogger.
* **NFR.4.2: System Compatibility:** The system must integrate smoothly with the existing infrastructure and platforms.

## **4. Assumptions**

* **A.1:** Interviewer availability data is accurately maintained in the integrated systems (InterviewLogger, MindComputeScheduler, Company calendars).
* **A.2:** Candidate and interviewer contact information is accurate and up-to-date.
* **A.3:** The integrated systems (InterviewLogger, MindComputeScheduler, MyMindComputeProfile) provide webhooks and APIs for data exchange.
* **A.4:** Skill sets and job profiles are accurately defined and categorized within the system.
* **A.5:** The security protocols used by InterviewLogger, MindComputeScheduler, and MyMindComputeProfile are sufficient for secure data exchange.
* **A.6:** InterviewLogger APIs supply all necessary candidate and application details.
* **A.7:** Recruiters also manage configuration changes in InterviewLogger and MindComputeScheduler.
* **A.8:** Interviewers leave schedules are available via their calendars.
* **A.9:** Tags related to skills, location, designation and role are maintained in MindComputeScheduler by TalOps and are updated regularly. This also matches the candidate information fed from InterviewLogger for easy matching.
* **A.10:** Recruiters update new skill set tags in MindComputeScheduler.
* **A.11:** The system is designed only for MindCompute Recruiting Team in India.
