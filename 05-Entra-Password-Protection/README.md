# Microsoft Entra Password Protection and Custom Banned Password Policy

## 📌 Project Overview

This lab demonstrates the configuration of Microsoft Entra Password Protection
using a custom banned password policy.

The objective was to prevent users from using passwords that contain variations
of the word "Falcon" by adding it to the custom banned password list.

---

## 🏢 Business Scenario

An organization wants to strengthen password security by preventing users from
choosing passwords that contain specific organization-related or commonly used
terms.

The word "Falcon" must therefore be prohibited from being used as part of user
passwords.

Microsoft Entra Password Protection was configured with a custom banned
password list to enforce this requirement.

---

## 🎯 Lab Objectives

The objectives of this lab were to:

1. Access Microsoft Entra Password Protection settings.
2. Enable custom banned passwords.
3. Add "Falcon" to the custom banned password list.
4. Save the password protection policy.
5. Validate that the configuration was successfully saved.

---

# 🔧 Implementation

## Step 1 — Access Password Protection Settings

The Microsoft Entra administration interface was used to access the Password
Protection configuration under Authentication methods.

> Screenshot evidence will be added here.

---

## Step 2 — Configure the Custom Banned Password Policy

Custom banned passwords were enabled and the word "Falcon" was added to the
custom banned password list.

This configuration prevents users from selecting passwords containing banned
variations of the configured term.

> 01-configure-custom-banned-password.png

---

## Step 3 — Save and Validate the Policy

The Password Protection policy was saved after completing the configuration.

The successful save notification was used to confirm that the password
protection policy had been updated.

>02-save-and-validate-password-protection-policy.png

---

## ✅ Result

Microsoft Entra Password Protection was successfully configured with a custom
banned password policy containing the word "Falcon".

This demonstrates how custom password restrictions can be used to strengthen
identity security and reduce the use of predictable password terms.

---

## 🛠️ Skills Demonstrated

- Microsoft Entra ID
- Microsoft Entra Password Protection
- Custom Banned Passwords
- Authentication Security
- Identity and Access Management (IAM)
- Password Policy Administration
- Security Policy Configuration
- Configuration Validation

---

## 💡 Key Learning

This lab strengthened my understanding of how Microsoft Entra Password
Protection can supplement password security by preventing users from selecting
passwords based on organization-specific or commonly used terms.

---

## 🔒 Lab Environment

This project was completed in a personal/training lab environment.

All identities, configurations, and information shown are used for learning
and skills demonstration purposes only.
