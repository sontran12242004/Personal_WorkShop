---
title: "Week 11 Worklog"
date: 2025-10-07
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---


### 🎯 Week 11 Objectives:

* Complete remaining modules of Human Resource Management system (HRM).
* Integrate **AWS SES** to send automated emails for HR operations.
* Use **AWS CloudWatch** to monitor, log and alert.
* Perform **Integration Testing**, prepare to move to full system testing phase.

---

### 📌 Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Documentation |
| --- | --------- | ------- | ----------- | -------- |
| 2  | - Summarize feedback from mentor week 10 <br> - Update IAM Role & S3 Policy to standard | 15/09/2025 | 15/09/2025 |
| 3  | - Integrate SES into HRM system: <br> &emsp;+ Send leave notification emails <br> &emsp;+ Send onboarding emails for new employees <br> &emsp;+ Send OTP / password reset | 16/09/2025 | 16/09/2025 | AWS SES Docs |
| 4  | - Set up CloudWatch Logs for EC2 application <br> - Create Log Group / Metric Filter <br> - Try creating simple alert (Error > 10 events) | 17/09/2025 | 17/09/2025 | AWS CloudWatch Docs |
| 5  | - Optimize HRM backend: <br> &emsp;+ Query payroll <br> &emsp;+ Fix attendance logic <br> &emsp;+ Improve API response time | 18/09/2025 | 18/09/2025 | |
| 6  | - Write Integration Test (JUnit + Mockito) for: <br> &emsp;+ Payroll Service <br> &emsp;+ Attendance Service <br> &emsp;+ Leave Management | 19/09/2025 | 19/09/2025 | Testing Docs |

---

### ✅ Week 11 Achievements:

#### ✉️ AWS SES – Email Automation
Successfully integrated email sending features from HRM system:
* Leave notification emails for management.
* Account & password emails for new employees.
* OTP / password reset emails.
* Understand and apply:
    * Domain verification
    * Identity management
    * Sandbox mode (test environment)

Created reusable, lightweight and scalable email service.

---

#### 📊 AWS CloudWatch – Logging & Monitoring
* Push logs from EC2 & Spring Boot to CloudWatch.
* Created separate Log Group: `hrm-backend-prod-logs`.
* Set up Metric Filter to detect:
    * ERROR
    * WARN
    * Unauthorized access
* Created CloudWatch Alarm to send email when error count exceeds threshold.

---

#### ⚙️ Backend Optimization (HRM Project)
* Reduced payroll processing time by optimizing SQL + Stream API.
* Fixed attendance tracking logic that was double-checking.
* Improved API performance thanks to local caching.
* Standardized DTO + Response Model.

---

#### 🧪 Integration Testing (Spring Boot)
Wrote tests for main modules:

| Module | Progress |
| ------ | ------- |
| Payroll Service | ✔ Completed |
| Attendance Service | ✔ Completed |
| Leave Management | ✔ Completed |
| Authentication + JWT | ⏳ In progress (next week 12) |

Results:
* Achieved coverage ~ 65%.
* Detected and fixed 3 logic errors due to missing null checks & missing fields.

---

### 📌 Personal Notes:
Week 11 helped me understand the process of completing backend in real cloud environment: from email service, logging, monitoring to integration testing. Although workload was quite heavy, thanks to clear task division and mentor support, I successfully deployed
