# Project 10 – Microsoft Entra Lifecycle Workflows: Finance Pre-Hire Automation

## Project Overview

This project demonstrates a hands-on **Microsoft Entra Lifecycle Workflows** implementation for a Finance pre-hire onboarding scenario.

The lab focuses on preparing a test identity, configuring a **Joiner Lifecycle Workflow**, using `employeeHireDate` as a time-based trigger, restricting the workflow scope to the Finance department, configuring group-assignment tasks, and validating the workflow using **Preview / What If**, **Run on demand**, workflow history, and task summaries.

The project demonstrates how Microsoft Entra ID Governance can help automate repetitive identity lifecycle activities during employee onboarding.

> **Lab Accuracy Note:** The workflow was configured for **3 days before `employeeHireDate`**. During the hands-on testing, **Run on demand** and workflow validation features were used rather than waiting for the scheduled hire-date event.

---

# Business Scenario

Contoso needs a structured way to prepare Finance employees before their first working day.

Without lifecycle automation, administrators may need to:

- Monitor employee start dates manually
- Check employee department information
- Assign required groups manually
- Prepare access before the employee starts
- Troubleshoot missing onboarding access
- Repeat the same onboarding process for multiple employees

Microsoft Entra Lifecycle Workflows can help automate these activities based on identity attributes.

For this lab, the requirement was to build a pre-hire Joiner workflow that:

- Targets users in the **Finance** department
- Uses **employeeHireDate** as the lifecycle event attribute
- Is designed to process eligible users **3 days before** their hire date
- Includes an **Add user to groups** onboarding task
- Can be validated using **Preview / What If**
- Can be manually tested using **Run on demand**

---

# Project Objectives

The objectives of this project were to:

- Prepare a Finance pre-hire test identity
- Configure the required employee attributes
- Create a Microsoft Entra Joiner Lifecycle Workflow
- Configure a time-based workflow trigger
- Use `employeeHireDate` as the event attribute
- Configure a Finance department scope
- Create a Finance security group
- Configure automated group assignment
- Validate the workflow using Preview / What If
- Test the workflow using Run on demand
- Review workflow history and execution results
- Understand how Lifecycle Workflows support Joiner automation

---

# Technologies and Skills Used

- Microsoft Entra ID
- Microsoft Entra Admin Center
- Microsoft Entra ID Governance
- Microsoft Entra Lifecycle Workflows
- Microsoft Entra Groups
- Microsoft Entra User Attributes
- Microsoft Authenticator
- Identity Lifecycle Management
- Joiner Process
- Pre-Hire Automation
- Rule-Based Workflow Scope
- Time-Based Workflow Trigger
- Preview / What If
- Run on Demand
- Workflow History
- Workflow Task Validation

---

# Implementation

## Step 1 – Review the Joiner Lab Requirements

The lab began by defining the identity and employee information required for the Finance pre-hire onboarding scenario.

The Joiner identity required attributes such as:

- Department
- Job Title
- Employee Type
- Employee ID
- Employee Hire Date

These attributes are important because Lifecycle Workflows can use identity information to determine which users should be processed.

![Joiner preparation requirements](screenshots/40-lab-joiner-preparation-requirements.png)

---

## Step 2 – Create the Finance Test Identity

A test user named **Anjali Rao** was created in Microsoft Entra ID.

The account represents a new employee joining the Finance department.

Navigate to:

**Microsoft Entra Admin Center → Identity → Users → All users → New user → Create new user**

The initial test-user information was entered.

![Create test user basic details](screenshots/01-create-test-user-basic-details.png)

Additional employee and job information was configured during user creation.

![Create Anjali user with job details](screenshots/33-create-anjali-user-with-job-details.png)

The employee job information was also reviewed during the creation process.

![Create test user job information](screenshots/38-create-test-user-job-information.png)

---

## Step 3 – Verify the New User in Microsoft Entra ID

After creating the account, the Microsoft Entra users list was checked.

Anjali Rao appeared as an available user in the tenant.

![Microsoft Entra users list](screenshots/09-entra-users-list.png)

A further validation of the users list was performed during the lab.

![Microsoft Entra users validation list](screenshots/24-entra-users-validation-list.png)

The Anjali Rao user account was then opened.

![Anjali Rao user overview](screenshots/02-anjali-rao-user-overview.png)

The user overview was checked again during validation.

![Anjali Rao overview validation](screenshots/22-anjali-rao-overview-validation.png)

A further view of the Finance pre-hire test identity was captured.

![Finance pre-hire test user overview](screenshots/26-finance-prehire-test-user-overview.png)

---

## Step 4 – Configure Anjali Rao's Finance Employee Attributes

The test user's job information was configured to represent a Finance employee.

Navigate to:

**Microsoft Entra Admin Center → Identity → Users → All users → Anjali Rao → Properties → Job Information**

The following attributes were configured:

| Attribute | Value |
|---|---|
| User | Anjali Rao |
| Job Title | Finance Analyst |
| Company | Contoso |
| Department | Finance |
| Employee ID | LW010 |
| Employee Type | Employee |
| Employee Hire Date | 30 September 2026 |

The user's properties and job information were reviewed.

![Anjali Rao properties and job information](screenshots/21-anjali-rao-properties-job-information.png)

The employee job information was edited and validated.

![Edit Anjali Rao job information](screenshots/36-anjali-rao-job-information-edit.png)

The Finance-related identity attributes were then confirmed.

![Validate Anjali Finance attributes](screenshots/44-anjali-finance-attributes-validation.png)

### Why These Attributes Matter

Two attributes are especially important in this project:

```text
department
employeeHireDate
```

The `department` attribute is used to determine whether the user belongs to the Finance workflow scope.

The `employeeHireDate` attribute is used as the time-based lifecycle event.

The workflow logic therefore begins with:

```text
Department = Finance
        +
Employee Hire Date = 30 September 2026
```

---

## Step 5 – Prepare Authentication for the Test User

The test user's authentication methods were reviewed before continuing with the workflow testing.

![Anjali authentication methods](screenshots/10-anjali-authentication-methods.png)

During sign-in, Anjali was prompted to secure the account.

![Anjali security registration prompt](screenshots/16-anjali-security-registration-prompt.png)

Microsoft Authenticator registration was performed.

The QR code was displayed during the registration process.

![Microsoft Authenticator QR code registration](screenshots/30-authenticator-qr-code-registration.png)

Authenticator registration completed successfully.

![Authenticator added successfully](screenshots/43-authenticator-added-successfully.png)

The sign-in process was then continued.

![Anjali stay signed in](screenshots/08-anjali-sign-in-stay-signed-in.png)

After completing the authentication setup, the Anjali Rao account was reviewed again.

![Anjali overview after authentication setup](screenshots/17-anjali-rao-overview-after-setup.png)

---

## Step 6 – Review Microsoft Entra Lifecycle Workflow Templates

Navigate to:

**Microsoft Entra Admin Center → Identity Governance → Lifecycle Workflows → Workflows → Create Workflow**

The available Lifecycle Workflow templates were reviewed.

For this project, the:

**Onboard pre-hire employee**

template was selected.

![Lifecycle Workflow template selection](screenshots/05-lifecycle-workflow-template-selection.png)

This template belongs to the **Joiner** lifecycle category.

---

## Step 7 – Configure the Finance Pre-Hire Workflow

The workflow was configured with the following settings:

| Setting | Configuration |
|---|---|
| Workflow Name | Project10-Finance-PreHire |
| Category | Joiner |
| Trigger Type | Time-based attribute |
| Event Timing | Before |
| Days from Event | 3 |
| Event User Attribute | employeeHireDate |

The pre-hire workflow trigger configuration was reviewed.

![Pre-hire workflow trigger details](screenshots/31-prehire-workflow-trigger-details.png)

The workflow properties were also checked.

![Project 10 workflow properties](screenshots/27-project10-workflow-properties.png)

### Trigger Logic

The workflow was designed around the following lifecycle logic:

```text
Employee Hire Date
        ↓
employeeHireDate
        ↓
3 days BEFORE
        ↓
Pre-Hire Joiner Workflow
```

For the test user:

```text
Employee Hire Date = 30 September 2026
```

The workflow design therefore represents pre-hire processing before that date.

> The lab does not claim that scheduled execution actually occurred three days before the hire date. Manual validation was performed using Run on demand.

---

## Step 8 – Configure the Finance Department Scope

The workflow should not process every user in the organization.

A **rule-based scope** was therefore configured.

The rule used was:

```text
(department eq 'Finance')
```

![Finance department scope rule](screenshots/45-finance-department-scope-rule.png)

The rule means that the workflow is scoped to users whose department attribute matches:

```text
Finance
```

Anjali Rao was configured with:

```text
Department = Finance
```

Therefore, she matches the department condition configured for this workflow.

### Scope Logic

```text
User
 ↓
Check department attribute
 ↓
department = Finance?
 ↓
YES
 ↓
User matches workflow scope
```

---

## Step 9 – Review the Available Lifecycle Workflow Tasks

The available tasks for the pre-hire workflow were reviewed.

![Select Lifecycle Workflow tasks](screenshots/42-select-lifecycle-workflow-tasks.png)

For this project, the main onboarding task was:

**Add user to groups**

The group-assignment task was configured.

![Add user to groups task configuration](screenshots/06-add-user-to-groups-task-configuration.png)

The pre-hire workflow task configuration was reviewed.

![Pre-hire workflow task configuration](screenshots/07-prehire-workflow-task-configuration.png)

The enabled workflow tasks were then checked.

![Pre-hire enabled tasks review](screenshots/15-prehire-enabled-tasks-review.png)

A workflow task summary was reviewed.

![Workflow task summary](screenshots/23-workflow-task-summary.png)

The task summary was validated again during the implementation.

![Workflow task summary validation](screenshots/41-workflow-task-summary-validation.png)

---

## Step 10 – Create the Finance Security Group

A Microsoft Entra security group was prepared for the Finance onboarding scenario.

Navigate to:

**Microsoft Entra Admin Center → Identity → Groups → All groups → New group**

The Finance security group was created.

![Create Finance security group](screenshots/29-create-finance-security-group.png)

This group represents access that can be assigned automatically as part of the employee onboarding process.

---

## Step 11 – Add the Finance Group to the Workflow Task

The Finance group was selected within the **Add user to groups** Lifecycle Workflow task.

![Add Finance group to workflow task](screenshots/39-add-finance-group-to-workflow-task.png)

This creates the following automation design:

```text
Eligible Finance Employee
        ↓
Lifecycle Workflow
        ↓
Add user to groups task
        ↓
Finance Group
```

Instead of an administrator manually assigning the group to every new Finance employee, the workflow task can perform the group-membership action when the lifecycle process is executed.

---

## Step 12 – Review the Complete Workflow Configuration

Before creating the workflow, the configuration was reviewed.

![Finance pre-hire workflow review and create](screenshots/12-prehire-workflow-review-and-create.png)

The final configuration was reviewed again before creation.

![Final workflow review and create](screenshots/34-final-workflow-review-and-create.png)

The review included the important workflow components:

```text
Category = Joiner

Trigger Type = Time-based attribute

Event Attribute = employeeHireDate

Event Timing = Before

Days from Event = 3

Scope = Rule based

Department Rule = Finance

Task = Add user to groups
```

---

## Step 13 – Create the Lifecycle Workflow

After reviewing the configuration, the workflow creation process was completed.

![Pre-hire workflow creation confirmation](screenshots/13-prehire-workflow-review-creation-confirmation.png)

Microsoft Entra Lifecycle Workflows then confirmed that the workflow was successfully created.

![Lifecycle workflow created successfully](screenshots/37-lifecycle-workflows-created-successfully.png)

The new workflow was now available within:

**Identity Governance → Lifecycle Workflows → Workflows**

---

# Validation and Testing

## Step 14 – Preview the Workflow Using What If

Before relying on normal workflow processing, the workflow was reviewed using the **Preview / What If** functionality.

An initial What If preview was performed.

![Initial workflow What If preview](screenshots/03-workflow-preview-what-if-initial.png)

The workflow preview was reviewed again after the identity attributes and workflow scope were prepared.

![Workflow What If result](screenshots/19-workflow-preview-what-if-result.png)

Anjali Rao appeared in the What If result details during the validation.

![Anjali What If result details](screenshots/25-what-if-preview-anjali-result-details.png)

### Why What If Is Useful

Preview / What If can help an administrator understand which identities are expected to match a workflow configuration.

It is useful when validating:

- User attributes
- Workflow scope
- Lifecycle conditions
- Workflow targeting

This can help identify configuration issues before relying on scheduled lifecycle processing.

---

## Step 15 – Test the Workflow Using Run on Demand

Instead of waiting for the normal hire-date processing window, the workflow was tested manually.

Navigate to:

**Identity Governance → Lifecycle Workflows → Workflows → Project10-Finance-PreHire → Run on demand**

The test user:

**Anjali Rao**

was selected.

![Run on demand select Anjali Rao](screenshots/28-run-on-demand-select-anjali-rao.png)

The workflow tasks available during the Run-on-Demand process were reviewed.

![Run on demand workflow tasks](screenshots/32-run-on-demand-workflow-tasks.png)

### Why Run on Demand Was Used

Run on demand allows an administrator to initiate workflow processing for a selected identity during testing.

This avoids waiting for the normal scheduled lifecycle event.

It is particularly useful for:

- Lab testing
- Workflow validation
- Troubleshooting
- Task verification
- Administrator testing

---

## Step 16 – Review Workflow History

After workflow testing, Lifecycle Workflow history was reviewed.

![Workflow history users summary](screenshots/04-workflow-history-users-summary.png)

Additional execution information was checked in the workflow history.

![Workflow history run details](screenshots/11-workflow-history-run-details.png)

A completed workflow-history view was also captured during the lab.

![Workflow history completed run](screenshots/18-workflow-history-completed-run.png)

Workflow history is important when troubleshooting because it provides visibility into workflow execution and processing.

---

## Step 17 – Review Workflow Run Results

The workflow Runs summary was reviewed.

![Workflow runs summary](screenshots/14-workflow-runs-summary.png)

A completed run summary was also captured.

![Workflow run summary completed](screenshots/20-workflow-run-summary-completed.png)

These views provide useful evidence when validating workflow execution.

---

## Step 18 – Additional Test Environment Validation

During the hands-on lab, the Deleted Users area was also checked while validating the test environment.

![Deleted users validation](screenshots/35-deleted-users-validation.png)

This screenshot is retained in the project because it forms part of the complete identity troubleshooting and validation process performed during the lab.

---

# Complete Workflow Logic

```text
Create Test User
        ↓
Anjali Rao
        ↓
Configure Job Information
        ↓
Job Title = Finance Analyst
Company = Contoso
Department = Finance
Employee ID = LW010
Employee Type = Employee
Employee Hire Date = 30 September 2026
        ↓
Configure Authentication
        ↓
Microsoft Authenticator Registered
        ↓
Create Lifecycle Workflow
        ↓
Category = Joiner
        ↓
Trigger = Time-based attribute
        ↓
Attribute = employeeHireDate
        ↓
Timing = 3 days BEFORE
        ↓
Configure Scope
        ↓
(department eq 'Finance')
        ↓
Create Finance Security Group
        ↓
Configure Add user to groups Task
        ↓
Review + Create Workflow
        ↓
Workflow Successfully Created
        ↓
Preview / What If
        ↓
Validate Anjali Rao
        ↓
Run on Demand
        ↓
Review Workflow History
        ↓
Review Run and Task Results
```

---

# Identity Attribute Logic

One of the main concepts demonstrated in this project is the relationship between identity attributes and lifecycle automation.

For example:

```text
Anjali Rao
        ↓
Department = Finance
        ↓
Matches:
(department eq 'Finance')
```

The lifecycle event is then based on:

```text
employeeHireDate
```

Therefore:

```text
WHO?
Finance employees

WHEN?
Before employeeHireDate

WHAT?
Configured onboarding workflow tasks
```

---

# Scheduled Execution vs Run on Demand

Understanding the difference between scheduled workflow processing and Run on demand was an important part of this project.

## Scheduled Processing

The workflow was designed around:

```text
employeeHireDate
+
3 days before
+
Finance department scope
```

This represents the intended lifecycle automation design.

## Run on Demand

During this hands-on lab, manual testing was used.

```text
Administrator
        ↓
Run on demand
        ↓
Select Anjali Rao
        ↓
Execute workflow for testing
        ↓
Review history/results
```

This allows administrators to validate workflow behaviour without waiting for the normal lifecycle event.

> **Important:** This portfolio does not claim that the scheduled workflow automatically executed three days before Anjali's hire date. The workflow was configured with that design, while the lab validation used manual testing features.

---

# Troubleshooting Approach

If a Lifecycle Workflow does not process an expected employee, I would investigate the following areas.

## 1. Verify the User Account

Confirm that the user exists and that the correct identity is being evaluated.

Check:

```text
Microsoft Entra ID
→ Users
→ User account
```

---

## 2. Verify Employee Attributes

Check whether the required identity attributes contain the expected values.

For this project:

```text
Department = Finance

Employee Hire Date = 30 September 2026
```

Incorrect or missing identity attributes can affect workflow evaluation.

---

## 3. Verify the Workflow Scope

Check the rule:

```text
(department eq 'Finance')
```

Confirm that the employee's actual department attribute matches the workflow rule.

---

## 4. Verify the Time-Based Trigger

Check:

```text
Trigger Type = Time-based attribute

Event User Attribute = employeeHireDate

Event Timing = Before

Days from Event = 3
```

---

## 5. Verify Workflow Tasks

Confirm that the required onboarding task is enabled.

For this project:

```text
Add user to groups
```

---

## 6. Verify the Target Group

Check whether the correct Microsoft Entra group has been selected in the workflow task.

---

## 7. Use Preview / What If

Use Preview / What If to check whether the expected identity appears in the workflow evaluation.

This can help determine whether the issue is related to:

- Identity attributes
- Workflow scope
- Workflow targeting

---

## 8. Use Run on Demand

Use Run on demand for controlled testing.

This helps test the workflow without waiting for the scheduled lifecycle event.

---

## 9. Review Workflow History

Check Lifecycle Workflow history for execution information.

Review:

- Workflow run
- User processing
- Task processing
- Execution status

---

## 10. Verify the Workflow Schedule

If automatic processing is expected, confirm that the workflow schedule is enabled according to the organization's implementation requirements.

---

# Security and Operational Considerations

Lifecycle automation can automatically change identity access, so workflows should be implemented carefully.

Administrators should:

- Use precise workflow scopes
- Validate identity attributes
- Test workflows before production deployment
- Avoid targeting unintended users
- Validate target groups
- Review workflow history
- Review failed tasks
- Follow least-privilege administration practices
- Regularly review lifecycle configurations
- Ensure HR/identity-source attributes are accurate

---

# Key Learning Outcomes

Through this project, I gained hands-on experience with:

- Microsoft Entra Lifecycle Workflows
- Microsoft Entra ID Governance
- Joiner lifecycle processes
- Pre-hire onboarding
- Identity attribute configuration
- `employeeHireDate`
- Time-based workflow triggers
- Department-based workflow scopes
- Rule-based scoping
- Microsoft Entra security groups
- Automated group-assignment concepts
- Microsoft Authenticator registration
- Preview / What If
- Run on demand
- Workflow history
- Workflow Runs summary
- Task validation
- Identity lifecycle troubleshooting

---

# Key Concept Learned

The main lifecycle automation relationship demonstrated in this project is:

```text
Identity Attributes
        +
Lifecycle Event
        +
Workflow Scope
        +
Workflow Tasks
        +
Validation
        =
Automated Identity Lifecycle Management
```

---

# Real-World Application

In a production organization, employee information may originate from an HR or identity source.

For example:

```text
HR / Identity Source
        ↓
New Employee Information
        ↓
Microsoft Entra ID
        ↓
Department = Finance
Employee Hire Date populated
        ↓
Microsoft Entra Lifecycle Workflow
        ↓
Evaluate employee attributes
        ↓
Finance employee matches scope
        ↓
Joiner workflow
        ↓
Configured onboarding tasks
```

This approach can help reduce repetitive manual administration and provide a more standardized onboarding process.

---

# Interview Explanation

A concise way I would explain this project during an interview:

> I built a Microsoft Entra Lifecycle Workflow for a Finance pre-hire Joiner scenario. I prepared a Finance test identity with department and employee hire-date attributes, configured a time-based workflow using employeeHireDate for three days before the hire date, and scoped the workflow using the rule `(department eq 'Finance')`. I created a Finance security group and configured the Add user to groups onboarding task. I then validated the workflow using Preview/What If, tested it with Run on demand, and reviewed workflow history and run results. This project helped me understand how identity attributes, lifecycle triggers, workflow scopes and automated tasks work together in Microsoft Entra ID Governance.

---

# Skills Demonstrated

- Microsoft Entra ID
- Microsoft Entra ID Governance
- Microsoft Entra Lifecycle Workflows
- Identity and Access Management
- Identity Lifecycle Management
- Joiner Process
- Pre-Hire Automation
- Microsoft Entra Groups
- Identity Attributes
- Time-Based Triggers
- Rule-Based Scoping
- Workflow Task Configuration
- Group Assignment Automation
- Microsoft Authenticator
- Workflow Testing
- Workflow Validation
- Workflow Troubleshooting

---

# Screenshot Evidence

This project contains **45 screenshots** documenting the hands-on implementation.

```text
screenshots/
├── 01-create-test-user-basic-details.png
├── 02-anjali-rao-user-overview.png
├── 03-workflow-preview-what-if-initial.png
├── 04-workflow-history-users-summary.png
├── 05-lifecycle-workflow-template-selection.png
├── 06-add-user-to-groups-task-configuration.png
├── 07-prehire-workflow-task-configuration.png
├── 08-anjali-sign-in-stay-signed-in.png
├── 09-entra-users-list.png
├── 10-anjali-authentication-methods.png
├── 11-workflow-history-run-details.png
├── 12-prehire-workflow-review-and-create.png
├── 13-prehire-workflow-review-creation-confirmation.png
├── 14-workflow-runs-summary.png
├── 15-prehire-enabled-tasks-review.png
├── 16-anjali-security-registration-prompt.png
├── 17-anjali-rao-overview-after-setup.png
├── 18-workflow-history-completed-run.png
├── 19-workflow-preview-what-if-result.png
├── 20-workflow-run-summary-completed.png
├── 21-anjali-rao-properties-job-information.png
├── 22-anjali-rao-overview-validation.png
├── 23-workflow-task-summary.png
├── 24-entra-users-validation-list.png
├── 25-what-if-preview-anjali-result-details.png
├── 26-finance-prehire-test-user-overview.png
├── 27-project10-workflow-properties.png
├── 28-run-on-demand-select-anjali-rao.png
├── 29-create-finance-security-group.png
├── 30-authenticator-qr-code-registration.png
├── 31-prehire-workflow-trigger-details.png
├── 32-run-on-demand-workflow-tasks.png
├── 33-create-anjali-user-with-job-details.png
├── 34-final-workflow-review-and-create.png
├── 35-deleted-users-validation.png
├── 36-anjali-job-information-edit.png
├── 37-lifecycle-workflows-created-successfully.png
├── 38-create-test-user-job-information.png
├── 39-add-finance-group-to-workflow-task.png
├── 40-lab-joiner-preparation-requirements.png
├── 41-workflow-task-summary-validation.png
├── 42-select-lifecycle-workflow-tasks.png
├── 43-authenticator-added-successfully.png
├── 44-anjali-finance-attributes-validation.png
└── 45-finance-department-scope-rule.png
```

---

# Project Status

**Project 10 – Completed Hands-On Training Lab**

The project documents the complete Finance pre-hire Lifecycle Workflow implementation performed in a Microsoft Entra training environment.

The portfolio evidence covers:

- Test-user preparation
- Employee attributes
- Authentication setup
- Lifecycle Workflow template selection
- Joiner trigger configuration
- Finance department scope
- Security-group creation
- Workflow task configuration
- Workflow creation
- Preview / What If
- Run-on-Demand testing
- Workflow history
- Run-result validation
- Troubleshooting methodology

---

## Repository

**Identity & Cloud Lab Portfolio**

Project:

`10-Entra-Lifecycle-Workflows-Finance-PreHire`

This project is part of my hands-on Microsoft 365, Microsoft Entra ID, Identity Governance and IAM learning portfolio.
