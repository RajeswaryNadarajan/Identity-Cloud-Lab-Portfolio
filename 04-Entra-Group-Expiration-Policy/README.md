# Microsoft Entra Group Expiration Policy and Lifecycle Management

## 📌 Project Overview

This lab demonstrates the configuration and validation of a Microsoft Entra
group expiration policy for Microsoft 365 groups.

The objective was to implement lifecycle management for Microsoft 365 groups
so that inactive groups do not remain permanently within the tenant without
review.

---

## 🏢 Business Scenario

An organization has multiple Microsoft 365 groups created for projects,
departments, and collaboration activities.

Over time, some groups may no longer be required but can remain active in the
tenant if they are not reviewed or removed.

To improve group lifecycle management, an expiration policy is configured so
that selected Microsoft 365 groups must be periodically renewed.

If a group is no longer required and is not renewed, the expiration process
can remove the obsolete group from the environment.

---

## 🎯 Lab Objectives

The objectives of this lab were to:

1. Review existing groups in Microsoft Entra ID.
2. Navigate to the Microsoft Entra group expiration settings.
3. Configure the group lifetime.
4. Configure an email contact for groups without an owner.
5. Configure the expiration policy for selected Microsoft 365 groups.
6. Create a Microsoft 365 group for testing.
7. Add the test group to the expiration policy.
8. Validate that the group is included in the expiration configuration.

---

# 🔧 Implementation

## Step 1 — Review Existing Groups

The existing groups in the Microsoft Entra tenant were reviewed before
configuring the expiration policy.

This helped confirm the available group types and identify the Microsoft 365
group that would be used for lifecycle testing.

> Screenshot evidence will be added here.

---

## Step 2 — Open Group Expiration Settings

The Microsoft Entra Admin Center was used to navigate to:

**Groups → Expiration**

The expiration configuration provides centralized lifecycle management for
Microsoft 365 groups.

> Screenshot evidence will be added here.

---

## Step 3 — Configure the Group Expiration Policy

A group lifetime was configured to define how long a Microsoft 365 group can
remain active before renewal is required.

An email contact was also configured so that expiration notifications can be
sent when a group does not have an available owner.

The expiration policy was configured to apply to selected Microsoft 365 groups
rather than automatically applying to every group in the tenant.

> Screenshot evidence will be added here.

---

## Step 4 — Create a Microsoft 365 Test Group

A Microsoft 365 group was created specifically for testing the expiration
policy.

The group provides a controlled object that can be added to the expiration
policy without affecting other groups in the tenant.

> Screenshot evidence will be added here.

---

## Step 5 — Select the Group for Expiration

The expiration policy was configured to apply to selected Microsoft 365 groups.

The newly created test group was selected and added to the policy.

This ensures that the group becomes subject to the configured lifecycle and
renewal requirements.

> Screenshot evidence will be added here.

---

## Step 6 — Validate the Expiration Policy

The Group Expiration configuration was reviewed after the group was added.

Validation confirmed that:

- The group expiration policy was configured
- A group lifetime was defined
- A notification contact was configured
- The policy was scoped to selected Microsoft 365 groups
- The test Microsoft 365 group was successfully included in the expiration policy

> Screenshot evidence will be added here.

---

## ✅ Result

The lab successfully demonstrated Microsoft 365 group lifecycle management
using Microsoft Entra group expiration policies.

The configuration helps organizations reduce unnecessary or abandoned groups
by requiring periodic renewal of groups that remain in use.

---

## 🛠️ Skills Demonstrated

- Microsoft Entra ID
- Microsoft 365 Group Administration
- Group Lifecycle Management
- Group Expiration Policies
- Microsoft 365 Administration
- Identity Governance Concepts
- Administrative Policy Configuration
- Group Renewal and Expiration
- Policy Scoping
- Configuration Validation

---

## 💡 Key Learning

This project strengthened my understanding of how Microsoft Entra can be used
to manage the lifecycle of Microsoft 365 groups.

Rather than allowing groups to remain indefinitely, expiration policies provide
a governance mechanism that requires groups to be periodically reviewed and
renewed when they are still required.

This helps reduce unused groups and improves the overall governance of the
Microsoft 365 environment.

---

## 🔒 Lab Environment

This project was completed in a personal/training lab environment.

All groups, identities, and configurations shown in this project are used for
learning and skills demonstration purposes only.
