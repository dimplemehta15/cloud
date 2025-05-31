# AWS IAM Deep Dive

## 📌 Overview

**IAM (Identity and Access Management)** is an **AWS global service** that allows you to securely control access to AWS services and resources. With IAM, you can manage **users**, **groups**, and **permissions (policies)** following the **principle of least privilege**.

---

## 🌍 IAM Is a Global Service

- IAM is **not region-specific**.
- All configurations — users, groups, roles, and policies — apply **globally**.

---

## 👤 IAM Users

- Represents an individual identity (developer, admin, app, etc.).
- Has **long-term credentials** (username/password for console, access keys for CLI/API).
- Created under your AWS account.
- Best practice: **Never use root account for daily tasks.**

---

## 👥 IAM Groups

- **Group = collection of users**.
- Helps manage permissions easily by attaching policies to a group instead of individuals.
- A user can belong to **multiple groups** or none (but that’s not ideal).
- Example: Group `admin` with `AdministratorAccess` policy.

---

## 🔐 IAM Policies

Policies define **permissions**. You attach them to users, groups, or roles.

### 📖 Types of Policies

| Type             | Description                                          | Use Case                       |
|------------------|------------------------------------------------------|--------------------------------|
| **Managed**      | Reusable. AWS-provided or customer-created.         | Shared access control logic.   |
| **Inline**       | Embedded directly in a user/group/role.             | One-off or tightly coupled use.|

### 🧱 Policy Structure (JSON)

\`\`\`json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "*"
    }
  ]
}
\`\`\`

| Field       | Description                                                        |
|-------------|--------------------------------------------------------------------|
| `Version`   | Defines the policy language version. Use `"2012-10-17"` always.    |
| `Effect`    | `"Allow"` or `"Deny"` the action.                                  |
| `Action`    | What action is allowed/denied, e.g., `s3:PutObject`.               |
| `Resource`  | AWS resources affected, e.g., `"arn:aws:s3:::my-bucket/*"` or `"*"`|
| `Condition` | *(Optional)* Add constraints like IP address or MFA requirements.  |

---

## ⚙️ Example: Admin User Setup

1. Create group `admin`.
2. Attach policy `AdministratorAccess`.
3. Create IAM user `stephane`.
4. Add `stephane` to the `admin` group.

➡️ Now `stephane` has full access. Remove him from the group to revoke all admin rights.

---

## 🔍 Permissions Test Scenario

1. Remove `stephane` from `admin` group → ❌ loses all access.
2. Attach `IAMReadOnlyAccess` directly to user `stephane`.
3. ✅ Can view IAM console but cannot create users or groups.
4. Attempting to create group now → ❌ fails (insufficient permissions).

---

## 🔑 Sign-In URL and Alias

- IAM users sign in via account-specific URL.
- AWS allows you to set an **alias** for better readability.

### Example:

Alias: `aws-stephane-v5`  
Login URL:  
\`\`\`
https://aws-stephane-v5.signin.aws.amazon.com/console
\`\`\`

🧪 **Tip**: Use an incognito window to test IAM users while staying logged in as the root/admin in your main browser.

---

## 🛡️ IAM Best Practices

- 🔒 **Enable MFA** for all users, especially root.
- ❌ Avoid using the root account after the initial setup.
- 🧑‍🤝‍🧑 Use **groups** to manage permissions.
- 📜 Prefer **managed policies** over inline ones.
- 🎯 Enforce the **principle of least privilege**.
- 🔍 Regularly audit IAM using **IAM Access Analyzer** and **AWS Config**.
- 🧹 Delete unused credentials and rotate access keys.

---

## 🧭 What’s Next?

- Learn about **IAM Roles** (for EC2, Lambda, cross-account access).
- Explore **Service-linked roles** and **Resource-based policies**.
- Practice writing custom policies with specific conditions and ARNs.
- Set up permission boundaries for delegated user control.

---

## ✅ Summary

| Component  | Purpose                               | Notes                                |
|------------|---------------------------------------|--------------------------------------|
| **User**   | Identity with credentials             | Use for people or apps               |
| **Group**  | Manages permissions collectively      | Assign users to multiple groups      |
| **Policy** | Defines what actions are allowed/denied| Managed = reusable; Inline = unique  |

IAM is the **first step** to a secure AWS environment. Start with strong foundations and grow permissions gradually.

---
"""
