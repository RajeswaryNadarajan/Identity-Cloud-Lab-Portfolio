# Project 10 – Microsoft Entra Lifecycle Workflows: Finance Pre-Hire Automation

## Project Overview

This project demonstrates the implementation of a **Microsoft Entra Lifecycle Workflow** to automate part of the pre-hire onboarding process for Finance employees.

The lab uses Microsoft Entra ID Governance Lifecycle Workflows to identify eligible employees based on identity attributes and execute onboarding tasks before their official hire date.

A test user named **Anjali Rao** was configured as a Finance employee with an employee hire date.

The workflow was designed to:

- Identify users belonging to the **Finance department**
- Use `employeeHireDate` as the time-based trigger attribute
- Process the user **3 days before the employee hire date**
- Automatically perform the configured onboarding task
- Allow administrators to test the workflow using **Run on demand**

This project demonstrates a practical **Joiner lifecycle management scenario** using Microsoft Entra ID Governance.

---

## Business Scenario

Contoso is onboarding new employees into its Finance department.

The IT and Identity teams currently perform several onboarding activities manually before an employee's first working day.

Manual onboarding can create several challenges:

- Administrators must monitor employee start dates manually
- Access may not be prepared before the employee joins
- Different employees may receive inconsistent onboarding
- Repetitive administrative tasks consume IT support time
- Delayed provisioning can affect employee productivity on the first day

The organization wants to automate part of the Joiner process using **Microsoft Entra Lifecycle Workflows**.

The requirement for this lab is:

> When an employee belongs to the Finance department, prepare the configured onboarding task before the employee's hire date.

For this implementation, the workflow is configured to evaluate the employee **3 days before `employeeHireDate`**.

---

## Objective

The objectives of this project are to:

- Create a Finance pre-hire Lifecycle Workflow
- Configure a Joiner workflow
- Use `employeeHireDate` as a time-based trigger
- Configure a Finance department scope
- Configure an onboarding task
- Test the workflow using Run on demand
- Understand how identity attributes control lifecycle automation
- Validate the workflow configuration without waiting for the scheduled execution date

---

## Technologies Used

- Microsoft Entra ID
- Microsoft Entra ID Governance
- Microsoft Entra Lifecycle Workflows
- Microsoft Entra Admin Center
- Microsoft Entra User Attributes
- Microsoft Entra Groups
- Identity Lifecycle Management
- Joiner Workflow Automation

---

## Test User

The following test identity was used:

| Attribute | Value |
|---|---|
| User | Anjali Rao |
| Job Title | Finance Analyst |
| Company | Contoso |
| Department | Finance |
| Employee ID | LW010 |
| Employee Type | Employee |
| Employee Hire Date | 30 September 2026 |

Two attributes are particularly important to the workflow:

`department`

and

`employeeHireDate`

The department attribute determines whether the user matches the workflow scope.

The employee hire date provides the time-based event used by the Joiner workflow.

---

# Implementation

## Step 1 – Prepare the Finance Pre-Hire Test User

A test user named **Anjali Rao** was prepared in Microsoft Entra ID to simulate a new employee joining the Finance department.

Navigate to:

**Microsoft Entra Admin Center → Identity → Users → All users → Anjali Rao → Properties → Job Information**

The following information was configured:

- **Job Title:** Finance Analyst
- **Company Name:** Contoso
- **Department:** Finance
- **Employee ID:** LW010
- **Employee Type:** Employee
- **Employee Hire Date:** 30 September 2026

The `Department` and `Employee Hire Date` attributes are important because Lifecycle Workflows can use identity attributes to determine:

1. **Who should enter the workflow**
2. **When the workflow should process the user**

For this project:

```text
Department = Finance
Employee Hire Date = 30 September 2026
```

### Screenshot – Finance Employee Job Information

![Anjali Rao Finance job information](screenshots/01-anjali-finance-job-information.png)

---

## Step 2 – Create the Finance Pre-Hire Lifecycle Workflow

Navigate to:

**Microsoft Entra Admin Center → Identity Governance → Lifecycle Workflows → Workflows → Create Workflow**

The **Onboard pre-hire employee** template was selected.

The workflow was configured with the following information:

| Setting | Configuration |
|---|---|
| Workflow Name | Project10-Finance-PreHire |
| Category | Joiner |
| Trigger Type | Time-based attribute |
| Event Timing | Before |
| Days from Event | 3 |
| Event User Attribute | employeeHireDate |

The purpose of the configuration is to create a Joiner workflow that can process eligible users before their employment start date.

For Anjali:

```text
Employee Hire Date
        ↓
30 September 2026
        ↓
Workflow configured for 3 days before
        ↓
Pre-hire processing window
```

---

## Step 3 – Configure the Finance Department Scope

The workflow must not apply to every employee in the organization.

A **rule-based scope** was therefore configured.

The following rule was used:

```text
(department eq 'Finance')
```

This means the workflow evaluates users whose department attribute is set to:

```text
Finance
```

Anjali's user account contains:

```text
Department = Finance
```

Therefore, she matches the workflow scope.

### Scope Logic

```text
User
  ↓
Check department attribute
  ↓
Is Department = Finance?
  ↓
YES
  ↓
User matches workflow scope
```

If the user's department does not match Finance, the user would not satisfy this particular scope rule.

---

## Step 4 – Configure the Pre-Hire Tasks

The workflow tasks were reviewed before the workflow was created.

The lab configuration shows:

| Workflow Task | Status |
|---|---|
| Generate TAP and Send Email | Disabled |
| Add user to groups | Enabled |

The **Add user to groups** task was enabled for this project.

This represents an onboarding scenario where an eligible Finance employee can automatically receive required group membership as part of the Joiner lifecycle process.

The complete configuration was then reviewed under **Review + create**.

### Screenshot – Review Workflow Configuration

![Review Finance pre-hire workflow configuration](screenshots/02-finance-prehire-review-create.png)

The screenshot confirms the important workflow settings:

```text
Category: Joiner

Trigger Type:
Time based attribute

Days from event:
3

Event timing:
Before

Event user attribute:
employeeHireDate

Scope:
Rule based

Rule:
(department eq 'Finance')

Add user to groups:
Enabled
```

---

## Step 5 – Create the Lifecycle Workflow

After reviewing the configuration, the workflow was created.

Navigate to:

**Identity Governance → Lifecycle Workflows → Workflows**

The portal displayed the confirmation:

> **Workflow was successfully created.**

### Screenshot – Workflow Created Successfully

![Lifecycle workflow successfully created](screenshots/03-workflow-created-successfully.png)

This confirmed that the Finance pre-hire workflow had been successfully created in Microsoft Entra Lifecycle Workflows.

### Important Lab Note

During the initial lab configuration, the workflow schedule was not enabled because the workflow was first being validated manually using **Run on demand**.

This is useful in a lab environment because it allows the administrator to test workflow tasks without waiting for the normal scheduled processing window.

For automatic scheduled lifecycle processing, the workflow schedule would need to be enabled according to the organization's implementation requirements.

---

## Step 6 – Test Using Run on Demand

The next stage was to validate the workflow.

Instead of waiting for the employee hire-date trigger, the **Run on demand** feature was used.

Navigate to:

**Identity Governance → Lifecycle Workflows → Workflows → Project10-Finance-PreHire → Run on demand**

The test user:

**Anjali Rao**

was selected.

### Screenshot – Run Workflow on Demand

![Run Finance pre-hire workflow on demand](screenshots/04-run-on-demand-anjali-rao.png)

The portal explains that Run on demand allows the workflow to run immediately for selected users and bypass the current workflow schedule and execution conditions.

This makes the feature particularly useful for:

- Lab validation
- Administrator testing
- Workflow troubleshooting
- Task verification

The **Run workflow** option can then be used to initiate the selected workflow tasks for the test user.

---

# Workflow Logic

The complete workflow logic for this project can be represented as:

```text
New Employee
      ↓
Anjali Rao
      ↓
Department = Finance
      ↓
Employee Hire Date = 30 September 2026
      ↓
Microsoft Entra Lifecycle Workflow
      ↓
Rule evaluates:
(department eq 'Finance')
      ↓
User matches Finance scope
      ↓
Time-based attribute:
employeeHireDate
      ↓
Configured timing:
3 days BEFORE hire date
      ↓
Joiner Workflow
      ↓
Configured Task:
Add user to groups
```

For the lab validation, **Run on demand** was used rather than waiting for scheduled execution.

---

# Scheduled Execution vs Run on Demand

Understanding the difference between these two execution methods was an important part of this project.

### Scheduled Workflow

The normal workflow design uses:

```text
employeeHireDate
+
3 days before
+
Finance department scope
```

Microsoft Entra Lifecycle Workflows can use these conditions to identify eligible users during scheduled processing.

### Run on Demand

For testing, an administrator can manually select a user and execute workflow tasks immediately.

```text
Administrator
      ↓
Run on demand
      ↓
Select Anjali Rao
      ↓
Run workflow
      ↓
Workflow tasks executed for selected user
```

This avoids having to wait until the actual scheduled lifecycle event during testing.

---

# Troubleshooting Considerations

If a user is not processed by a Lifecycle Workflow, the following areas should be investigated:

1. Verify the user's required attributes.

```text
Department
Employee Hire Date
Employee Type
```

2. Verify that the user matches the workflow scope.

For this project:

```text
(department eq 'Finance')
```

3. Verify the workflow trigger configuration.

```text
Trigger Type = Time-based attribute
Attribute = employeeHireDate
Timing = Before
Days = 3
```

4. Verify that the required workflow task is enabled.

5. Verify the workflow schedule when automatic execution is expected.

6. Review Lifecycle Workflow execution history and task results when troubleshooting workflow processing.

7. Use **Run on demand** during testing to validate workflow tasks independently from the normal scheduled trigger.

---

# Security and Operational Considerations

Lifecycle automation should be implemented carefully because workflow tasks can automatically change user access.

Administrators should:

- Define precise workflow scopes
- Validate employee attributes
- Test workflows before production deployment
- Avoid targeting unintended users
- Review workflow execution results
- Apply least-privilege administrative practices
- Validate group memberships granted during onboarding
- Regularly review lifecycle automation configurations

---

# Key Learning Outcomes

Through this project, I gained hands-on experience with:

- Microsoft Entra Lifecycle Workflows
- Joiner lifecycle processes
- Pre-hire onboarding automation
- Time-based workflow triggers
- `employeeHireDate`
- Rule-based workflow scopes
- Department-based targeting
- Workflow task configuration
- Group assignment automation concepts
- Run-on-demand testing
- Workflow validation
- Identity lifecycle troubleshooting

One of the main lessons from this lab was understanding the relationship between:

```text
Identity Attributes
       +
Workflow Scope
       +
Lifecycle Event
       +
Workflow Tasks
       =
Automated Identity Lifecycle Process
```

---

# Real-World Application

In a production organization, employee information may originate from an HR process or identity source.

For example:

```text
HR / Identity Source
        ↓
Employee identity information
        ↓
Microsoft Entra ID
        ↓
Department = Finance
Employee Hire Date populated
        ↓
Lifecycle Workflow evaluates employee
        ↓
Joiner workflow processes eligible user
        ↓
Required onboarding tasks performed
```

This can reduce repetitive manual identity administration and help organizations standardize employee onboarding.

---

# Interview Explanation

A simple way I would explain this project during an interview:

> I created a Microsoft Entra Lifecycle Workflow for a Finance pre-hire onboarding scenario. I configured a Joiner workflow using employeeHireDate as the time-based attribute and set it to process users three days before their hire date. I used a rule-based scope targeting users whose department equals Finance and enabled the Add user to groups task. I created a Finance test user with the required job attributes and used Run on demand to validate the workflow without waiting for the scheduled execution window. This lab helped me understand how identity attributes, workflow scopes, lifecycle triggers and automated tasks work together in Microsoft Entra ID Governance.

---

# Skills Demonstrated

- Microsoft Entra ID
- Microsoft Entra ID Governance
- Lifecycle Workflows
- Identity Lifecycle Management
- Joiner Process
- Pre-Hire Automation
- Rule-Based Scoping
- Identity Attribute Management
- Group Assignment Automation
- Workflow Testing
- Workflow Troubleshooting
- Microsoft 365 Identity Administration

---

## Project Status

**Project 10 – Completed Lab Implementation**

The workflow configuration and Run-on-Demand test demonstrate the core Finance pre-hire Lifecycle Workflow implementation.

The lab was performed in a training Microsoft Entra environment and is documented as a hands-on learning project.
