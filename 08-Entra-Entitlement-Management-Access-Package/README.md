# Project 8: Microsoft Entra Entitlement Management – Access Package & Multi-Stage Approval

## Project Overview

This project demonstrates the design, implementation, and end-to-end testing of a **Microsoft Entra Entitlement Management Access Package** for an internal IDAM team onboarding scenario.

The objective was to simplify and govern access provisioning for new team members who require multiple groups, enterprise applications, SharePoint permissions, and privileged group access.

Instead of manually assigning every resource to each new employee, the required access is bundled into a single access package and controlled through:

- Requestor restrictions
- Multi-stage approval
- Alternate/fallback approvers
- Privileged Identity Management (PIM)
- Time-bound assignments
- Extension controls
- Automated resource delivery

The business scenario represents onboarding **20 new IDAM team members** with the same access requirements.

For lab validation, one test account was used to complete the entire request-to-access workflow.

---

# Business Scenario

Assume 20 new employees are joining the **IDAM department**.

Each employee requires access to:

### Microsoft Entra Administrative Roles

- Intune Administrator
- Helpdesk Administrator

### SharePoint Online

- 1 SharePoint site as Owner
- 1 SharePoint site as Member
- 1 SharePoint site as Visitor/Viewer

### Enterprise Applications

- 5 Enterprise Applications

### Security Groups

Eight groups are required:

- 2 Active Member assignments
- 2 Active Owner assignments
- 2 Eligible Member assignments
- 2 Eligible Owner assignments

The privileged/eligible assignments are managed using **Microsoft Entra Privileged Identity Management (PIM) for Groups**.

---

# Governance Requirements

The access process must meet the following requirements:

| Requirement | Configuration |
|---|---|
| Requestors | Internal IDAM department users only |
| Request method | Microsoft My Access |
| Approval stages | 2 |
| Stage 1 | Designated first-level approver |
| Stage 2 | Manager as approver |
| Alternate/Fallback | Dileep |
| Approval deadline | 4 days |
| Access duration | 45 days |
| Extension allowed | Yes |
| Extension approval | No |
| Privileged access | PIM Eligible Member/Owner |

---

# Technologies Used

- Microsoft Entra ID
- Microsoft Entra Identity Governance
- Entitlement Management
- Access Packages
- Privileged Identity Management (PIM)
- PIM for Groups
- Microsoft 365
- SharePoint Online
- Enterprise Applications
- Microsoft My Access

---

# Implementation Steps

## Step 1 – Create Identity Governance Catalog

Navigate to:

**Microsoft Entra Admin Center → Identity Governance → Entitlement Management → Catalogs**

Select:

**+ New catalog**

Configure:

**Name**

`CAT-IDAM-Governance-Lab`

**Description**

`Catalog for IDAM team access package and identity governance lab.`

Configure:

- Enabled for users to request: **Yes**
- Enabled for external users to request: **No**

Select:

**Create**

The dedicated Identity Governance catalog is now available for the IDAM access package.

---

## Step 2 – Assign Catalog Owner

The administrator initially did not have sufficient catalog-level permissions to manage all required resources.

Navigate to:

**Identity Governance → Catalogs → CAT-IDAM-Governance-Lab → Roles and administrators**

Assign the administrator account as:

**Catalog Owner**

This allows the administrator to manage resources and access packages within the catalog.

---

## Step 3 – Create the Access Package

Navigate to:

**Identity Governance → Entitlement Management → Access packages**

Select:

**+ New access package**

Configure:

**Name**

`AP-IDAM-Team-Access`

**Description**

`Access package for IDAM team members to request governed access to required roles, groups, applications and SharePoint resources.`

Select catalog:

`CAT-IDAM-Governance-Lab`

The access package will act as the central entitlement bundle for IDAM team onboarding.

---

# Step 4 – Create Eight Security Groups

Create eight security groups for the different access requirements.

Navigate to:

**Microsoft Entra ID → Groups → All groups → New group**

Create:

| Group | Required Access |
|---|---|
| LAB-IDAM-Group-01-Member | Member |
| LAB-IDAM-Group-02-Member | Member |
| LAB-IDAM-Group-03-Owner | Owner |
| LAB-IDAM-Group-04-Owner | Owner |
| LAB-IDAM-Group-05-Eligible-Member | Eligible Member |
| LAB-IDAM-Group-06-Eligible-Member | Eligible Member |
| LAB-IDAM-Group-07-Eligible-Owner | Eligible Owner |
| LAB-IDAM-Group-08-Eligible-Owner | Eligible Owner |

Group type:

**Security**

Membership type:

**Assigned**

---

# Step 5 – Configure PIM for Eligible Groups

Groups 05–08 require privileged eligible assignments rather than permanent active access.

Navigate to:

**Identity Governance → Privileged Identity Management → Groups**

Discover/manage the required groups through PIM.

Configure:

### Eligible Member Groups

`LAB-IDAM-Group-05-Eligible-Member`

`LAB-IDAM-Group-06-Eligible-Member`

### Eligible Owner Groups

`LAB-IDAM-Group-07-Eligible-Owner`

`LAB-IDAM-Group-08-Eligible-Owner`

Once the groups are PIM-managed, the access package can use:

- Eligible Member
- Eligible Owner

as resource roles.

This provides **Just-In-Time privileged group access** instead of permanently active privileged access.

---

# Step 6 – Add Group Resources to the Access Package

Navigate to:

**Identity Governance → Access packages → AP-IDAM-Team-Access → Resource roles**

Select:

**Add resource roles → Groups and Teams**

Add the eight groups.

Configure the resource roles as:

| Resource | Role |
|---|---|
| LAB-IDAM-Group-01-Member | Member |
| LAB-IDAM-Group-02-Member | Member |
| LAB-IDAM-Group-03-Owner | Owner |
| LAB-IDAM-Group-04-Owner | Owner |
| LAB-IDAM-Group-05-Eligible-Member | Eligible Member |
| LAB-IDAM-Group-06-Eligible-Member | Eligible Member |
| LAB-IDAM-Group-07-Eligible-Owner | Eligible Owner |
| LAB-IDAM-Group-08-Eligible-Owner | Eligible Owner |

This allows one access package to deliver both normal and privileged group entitlements.

---

# Step 7 – Create Enterprise Applications

Five lab enterprise applications were created:

`LAB-IDAM-App-01`

`LAB-IDAM-App-02`

`LAB-IDAM-App-03`

`LAB-IDAM-App-04`

`LAB-IDAM-App-05`

Navigate to:

**Microsoft Entra ID → Enterprise applications**

Create/configure the required lab applications.

---

# Step 8 – Add Applications to the Access Package

Navigate to:

**Identity Governance → Access packages → AP-IDAM-Team-Access → Resource roles**

Select:

**Add resource roles → Applications**

Add:

- LAB-IDAM-App-01
- LAB-IDAM-App-02
- LAB-IDAM-App-03
- LAB-IDAM-App-04
- LAB-IDAM-App-05

Assign the application role:

**User**

The applications are now part of the access package.

---

# Step 9 – Create SharePoint Sites

Three SharePoint sites were created for the lab:

`LAB-IDAM-SP-01`

`LAB-IDAM-SP-02`

`LAB-IDAM-SP-03`

The sites represent different permission requirements.

---

# Step 10 – Add SharePoint Resources

Navigate to:

**Identity Governance → Access packages → AP-IDAM-Team-Access → Resource roles**

Select:

**Add resource roles → SharePoint sites**

Configure:

| SharePoint Resource | Permission |
|---|---|
| LAB-IDAM-SP-01 | Owners |
| LAB-IDAM-SP-02 | Members |
| LAB-IDAM-SP-03 | Visitors |

This demonstrates how different SharePoint permission levels can be delivered from one governed access package.

---

# Step 11 – Test Microsoft Entra Administrative Role Resources

The original business requirement also included:

- Intune Administrator
- Helpdesk Administrator

From the access package, select:

**Add resource roles → Microsoft Entra role (Preview)**

The following roles were searched:

`Intune Administrator`

`Helpdesk Administrator`

However, the shared lab tenant returned:

**No resource found**

This continued even after the administrator had the required administrative and Catalog Owner permissions.

Therefore, the Microsoft Entra role portion could not be implemented in this shared lab tenant.

This limitation is documented rather than representing the roles as successfully configured.

The intended production design would include these administrative role resources when supported by the tenant and licensing/Preview availability.

---

# Step 12 – Create IDAM Requestor Group

To prevent every tenant user from requesting the package, create a dedicated requestor group.

Navigate to:

**Microsoft Entra ID → Groups → New group**

Configure:

**Group name**

`IDAM-Department-Users`

Group type:

**Security**

Membership:

**Assigned**

Add the test user:

`Rajeswary_test1`

This group represents authorized internal IDAM department employees.

---

# Step 13 – Restrict Access Package Requestors

Navigate to:

**Identity Governance → Access packages → AP-IDAM-Team-Access → Policies**

Configure requestor scope:

**Specific users and groups**

Select:

`IDAM-Department-Users`

Under who can request access:

**Self → Enabled**

This restricts the access package to authorized IDAM department users.

---

# Step 14 – Configure First-Level Approval

Enable approval for the access package.

Configure:

**Require approval → Yes**

Configure first approval stage:

**Approver**

Designated specific approver

Lab approver:

`Rajeswari-E`

Configure:

**Decision must be made in**

`4 days`

Enable:

**Require approver justification → Yes**

Enable:

**Forward to alternate approver if no action → Yes**

Configure alternate approver:

`Dileep`

Configure forwarding after:

`3 days`

This ensures the request does not remain unattended if the primary approver is unavailable.

---

# Step 15 – Configure Second-Level Approval

Add a second approval stage.

Configure:

**Second approver**

`Manager as approver`

Decision deadline:

`4 days`

Enable:

**Require approver justification → Yes**

Configure fallback/alternate approver:

`Dileep`

This creates the intended workflow:

**User Request → First-Level Approval → Manager Approval**

with Dileep available as the fallback/alternate approver.

---

# Step 16 – Configure Access Lifecycle

Configure the assignment lifecycle.

Set:

**Access package assignments expire**

`Number of days`

Duration:

`45`

Configure:

**Users can request specific timeline**

`No`

Configure:

**Allow users to extend access**

`Yes`

Configure:

**Require approval to grant extension**

`No`

This provides fixed, time-bound access while allowing the user to request an extension without another approval workflow.

---

# Step 17 – Create the Access Package Policy

Review all configuration.

Confirm:

- Requestor scope = IDAM department group
- Two-stage approval = Enabled
- First-level approver = Configured
- Manager approval = Configured
- Alternate/fallback approver = Dileep
- Approval deadline = 4 days
- Assignment duration = 45 days
- Extension = Allowed
- Extension approval = Not required

Create the policy/access package.

The final package contained:

- **8 Groups and Teams**
- **5 Applications**
- **3 SharePoint resources**
- **1 Enabled policy**

Microsoft Entra roles remained at **0** due to the documented shared-tenant/Preview limitation.

---

# End-to-End Testing

## Step 18 – Sign In as the Test User

Open a separate browser/InPrivate session.

Sign in as:

`Rajeswary_test1`

Open:

**My Access → Access packages**

The user can see:

`AP-IDAM-Team-Access`

This confirms that the `IDAM-Department-Users` requestor scope is working.

---

# Step 19 – Submit Access Request

Select:

**AP-IDAM-Team-Access → Request**

Requesting for:

**Yourself**

Select:

**Continue**

Enter a business justification.

Example used during testing:

`I want to access the SharePoint resources at IDAM department`

Select:

**Submit request**

My Access displays confirmation that the request is being processed.

---

# Step 20 – Verify Pending Approval

Navigate to:

**My Access → Request history**

The request displays:

**Status: Pending approval**

This confirms that access was not automatically provisioned and that the configured governance approval workflow was triggered.

---

# Step 21 – Complete First-Level Approval

Sign in as the designated first-level approver.

Navigate to:

**My Access → Approvals**

Locate the request from:

`Rajeswary_test1`

Requested package:

`AP-IDAM-Team-Access`

Approve the request and provide justification.

The portal confirms:

**Successfully approved Rajeswary_test1**

The request then proceeds to the next approval stage.

---

# Step 22 – Validate Second/Fallback Approval

The second stage was configured to use the requestor's manager, with Dileep as fallback/alternate approver.

During lab testing, the pending action became available to:

`Dileep`

Sign in as Dileep.

Navigate to:

**My Access → Approvals**

The request for:

`Rajeswary_test1`

appears as pending.

Review the request.

Select:

**Approve**

Provide justification.

Example:

`Due to absence of the manager, approval is provided for the newly joined IDAM department user.`

Select:

**Submit**

The portal confirms:

**Successfully approved Rajeswary_test1**

This validates the fallback/alternate approval path.

---

# Step 23 – Verify Resource Delivery

Return to the test user's My Access portal.

Navigate to:

**Request history**

The request initially displays:

`Delivering`

This indicates that the approval workflow has completed and Microsoft Entra Entitlement Management is provisioning the package resources.

After provisioning completes, verify from the administrator portal:

**Identity Governance → Access packages → AP-IDAM-Team-Access → Requests**

Status:

`Delivered`

Sub-status:

`Delivered`

This confirms successful end-to-end access package processing.

---

# Step 24 – Validate Active Group Membership

Sign in as:

`Rajeswary_test1`

Navigate to:

**My Groups → Groups I am in**

Verify:

`LAB-IDAM-Group-01-Member`

`LAB-IDAM-Group-02-Member`

The user is now an active member of the required groups.

The existing:

`IDAM-Department-Users`

group remains visible because it was used to authorize the user to request the access package.

---

# Step 25 – Validate Active Group Ownership

Navigate to:

**My Groups → Groups I own**

Verify:

`LAB-IDAM-Group-03-Owner`

`LAB-IDAM-Group-04-Owner`

This confirms that Entitlement Management successfully provisioned the **Owner** resource roles.

---

# Step 26 – Validate PIM Eligible Assignment

Navigate to:

**Identity Governance → Privileged Identity Management → Groups**

Open one of the eligible groups.

Example:

`LAB-IDAM-Group-06-Eligible-Member`

Navigate to:

**Assignments → Eligible assignments**

Verify:

`Rajeswary_test1`

The user appears under:

**Eligible assignments**

This confirms that the access package successfully granted PIM eligibility rather than permanent active privileged membership.

Eligible access can subsequently be activated through PIM when required.

---

# Validation Results

| Control | Result |
|---|---|
| Access package created | Successful |
| Dedicated governance catalog | Successful |
| IDAM-only request scope | Successful |
| 2 Active Member groups | Successful |
| 2 Active Owner groups | Successful |
| 2 Eligible Member groups | Successful |
| 2 Eligible Owner groups | Successful |
| PIM eligible assignment | Successful |
| 5 enterprise applications | Configured |
| 3 SharePoint resources | Configured |
| Stage 1 approval | Successful |
| Stage 2/fallback workflow | Successful |
| Dileep fallback approval | Successful |
| 4-day approval deadline | Configured |
| 45-day assignment lifecycle | Configured |
| User extension | Enabled |
| Extension approval | Not required |
| Request delivery | Delivered |
| Entra administrative roles | Lab tenant/Preview limitation |

---

# Troubleshooting / Lab Limitation

## Microsoft Entra Role Resource – No Resource Found

The intended solution included:

- Intune Administrator
- Helpdesk Administrator

However, when adding:

**Microsoft Entra role (Preview)**

the lab tenant returned:

**No resource found**

The issue persisted after validating administrative access and assigning Catalog Owner permissions.

Because the roles could not be surfaced by the shared lab tenant, they were **not falsely represented as successfully implemented**.

The production design still includes these two administrative roles where the required tenant capability, licensing, and Preview feature are available.

This troubleshooting scenario was retained as part of the project evidence.

---

# Final Architecture

The implemented workflow can be summarized as:

**IDAM Department User**

↓

**Microsoft My Access**

↓

**AP-IDAM-Team-Access**

↓

**Stage 1 – First-Level Approver**

↓

**Stage 2 – Manager / Dileep Fallback**

↓

**Microsoft Entra Entitlement Management**

↓

**Automated Access Provisioning**

↓

**Groups + PIM Eligibility + Enterprise Applications + SharePoint**

↓

**45-Day Time-Bound Assignment**

↓

**Extension Available Without Additional Approval**

---

# Security & Governance Benefits

This design provides several advantages compared with manually assigning access.

### Least Privilege

Privileged group access can be provided as **Eligible** rather than permanently active access.

### Standardized Onboarding

Users performing the same IDAM job function can receive a consistent access bundle.

### Approval Governance

Access is not granted until the required approval workflow is completed.

### Business Scope Restriction

Only users belonging to the authorized IDAM requestor group can request the package.

### Resilient Approval Workflow

Alternate/fallback approvers reduce the risk of requests being blocked when a primary approver or manager is unavailable.

### Time-Bound Access

Assignments automatically follow a defined 45-day lifecycle rather than remaining indefinitely.

### Self-Service

Authorized users can request access through My Access instead of relying entirely on administrators to manually provision each resource.

### Reduced Administrative Effort

Instead of manually assigning multiple entitlements for every new team member, the access package centralizes the required resources.

---

# Key Learning Outcomes

This project provided hands-on experience with:

- Microsoft Entra Entitlement Management
- Identity Governance catalogs
- Access package design
- Requestor scoping
- Multi-stage approval workflows
- Manager-based approval
- Alternate and fallback approvers
- PIM for Groups
- Active vs Eligible access
- Member vs Owner permissions
- Enterprise application assignment
- SharePoint resource roles
- Time-bound access
- Assignment lifecycle management
- Access extension
- Microsoft My Access
- End-to-end entitlement testing
- Access delivery validation
- Identity Governance troubleshooting

---

# Project Result

The project successfully demonstrated an end-to-end **Identity Governance onboarding workflow**.

An authorized IDAM user was able to:

1. Discover the access package through My Access.
2. Request the package with business justification.
3. Enter a controlled multi-stage approval process.
4. Pass first-level approval.
5. Use the configured fallback approval path.
6. Receive the approved entitlement package.
7. Obtain active group membership.
8. Obtain group ownership.
9. Receive PIM eligible privileged group access.
10. Reach a final **Delivered** request state.

Although one user was used for lab testing, the same access-package design represents the onboarding requirement for **20 new IDAM team members**, reducing repetitive manual access assignments while improving governance and consistency.

---

# Screenshots / Evidence

Complete implementation and testing evidence is available in the:

`/screenshots`

folder.

The screenshots cover:

- Catalog creation
- Catalog Owner configuration
- Access package creation
- Group creation
- PIM configuration
- Eligible Member/Owner roles
- Enterprise applications
- SharePoint resources
- Microsoft Entra role Preview troubleshooting
- Requestor scope
- Multi-stage approval configuration
- Alternate/fallback approver
- 45-day lifecycle configuration
- End-user My Access request
- Pending approval
- First-stage approval
- Dileep fallback approval
- Delivering status
- Delivered status
- Active group membership
- Active group ownership
- PIM eligible assignment validation

---

## Skills Demonstrated

`Microsoft Entra ID` `Identity Governance` `Entitlement Management` `Access Packages` `PIM` `PIM for Groups` `IAM` `RBAC` `Microsoft 365` `SharePoint Online` `Enterprise Applications` `Access Lifecycle Management` `Least Privilege`
