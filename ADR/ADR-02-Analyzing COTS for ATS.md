# **ADR 02: Analyzing COTs for ATS**

## Date:

2025-03-24

## **Status**

Accepted

## **Context:**

Given that there is no centralized system for matching, scheduling, tracking and reporting, we would need to assess the capabilities of InterviewLogger with MindComputeScheduler and check if there are available COTs that could replace these two systems as a single unit

## **Decision:**

We choose **InterviewLogger** and **MindComputeScheduler** for integration over COTs.Given the recruiters are using MindComputeScheduler and InterviewLogger and are much familiar + new enhancements that make these integrations easy to automate, we would recommend using and extending the existing integration

## **Rationale:**

- ✅ InterviewLogger and MindComputeScheduler is **already in use** at MindCompute.
- ✅ Both provide **well-documented APIs** as well as webhook integrations.
- ✅ InterviewLogger **strong support for integrations** (with MindComputeScheduler & other internal tools).
- ✅ InterviewLogger can **auto-fetch candidate skills and job roles**, reducing manual input. (This can be integrated in future. Currently recruiters manually provide the candidate's information. IMS would use Harvest API of InterviewLogger to fetch **candidates + job postings**.)

## **Alternatives Considered and Consequences:**

| Feature Category                                 | Workable                                                                                                                                              | BambooHR                                                                                                              | InterviewLogger                                                                                                          | Lever                                                                                                              |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| A. Applicant Tracking System (ATS) Functionality | Comprehensive candidate profile management, customizable workflows, robust reporting and analytics, strong collaboration tools, supports GDPR/CCPA 1. | Good for SMBs, integrated with HRIS, basic ATS features, applicant funnel reporting, mobile app 1.                    | Excellent candidate experience, structured hiring, strong reporting, collaborative hiring features, focus on DEI 1. | Combines ATS and CRM, strong candidate relationship management, visual pipelines, good for collaborative hiring 2. |
| B. Automated Interview Scheduling                | Self-scheduling, calendar integrations (Google, Outlook), integrates with video conferencing (Zoom, Meet, Teams) 1.                                   | Requires Cronofy integration for robust scheduling, offers self-scheduling links, calendar sync 12.                   | Native scheduling capabilities, integrates with external scheduling tools (MindComputeScheduler, ModernLoop), calendar sync 40. | Native scheduling features, calendar integration, time zone support, buffer time settings 77.                      |
| C. RSVP Monitoring and Management                | Automated alerts and email reminders, real-time updates 14.                                                                                           | Monitors status via Cronofy integration, allows rescheduling and cancellation 12.                                     | Tracks candidate RSVP with calendar integration, allows rescheduling and cancellation 15.                           | Notifications and reminders, reschedule option available 77.                                                       |
| D. Integration Capabilities                      | Open API, 270+ integrations, including job boards, assessment tools, and HR systems 14.                                                               | Open API, integrates with various HR and productivity tools, partners with other ATS like InterviewLogger and Workable 10. | Open API, 500+ pre-built integrations across various categories (scheduling, HRIS, video interviewing) 43.          | API available, integrates with HR systems, CRM, and other tools 78.                                                |
| E. Additional Relevant Features                  | AI-powered candidate sourcing, salary estimator, built-in texting, onboarding capabilities 1.                                                         | Mobile hiring app, automated offer letters, onboarding tools, reporting and analytics 29.                             | Advanced features for onboarding, referrals, and DEI recruiting, talent filtering 7.                                | Built-in CRM, recruitment marketing tools, AI interview intelligence, recruitment analytics 79.                    |
| F. Pricing Models                                | Mid-market range, per employee per month (PEPM) 1.                                                                                                    | Typically per employee per month (PEPM), varies based on features and number of employees 3.                          | Higher end of mid-market, per employee per month (PEPM), tiered plans 50.                                           | Mid-market range, per employee per month (PEPM), potential additional costs for API access 20.                     |
| G. User Reviews and Ratings                      | Intuitive, ease of use, good customization, responsive customer support 1.                                                                            | User-friendly, good customer support, efficient data export 1.                                                        | Generally positive feedback, intuitive platform, some reports of customer support challenges 1.                     | User-friendly interface, good for collaboration, some reports of customer service turnover 2.                      |
