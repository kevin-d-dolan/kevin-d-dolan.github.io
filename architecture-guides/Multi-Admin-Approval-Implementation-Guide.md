# Microsoft Intune Multi-Admin Approval (MAA)

## Before You Start (Required Prerequisites)

1. **Have at least two administrator accounts** in the tenant.
   - MAA requires a second administrator to approve changes.
   - Reference: Microsoft Intune Multi-Admin Approval documentation

2. **Plan the three MAA roles**:

   | Role | Purpose |
   |--------|---------|
   | Access Policy Manager | Creates and manages access policies |
   | Approver | Approves or rejects requests |
   | Change Requestor | Submits changes and completes them after approval |

3. **Licensing Requirements**
   - By default, administrators participating in MAA must have an **Intune license**.
   - An optional setting allows unlicensed administrators.
   - **Important:** Once enabled, this setting **cannot be reversed**.

---

# Step-by-Step: Create and Enable MAA for a Specific Resource Type

MAA is enabled **per Profile Type (resource type)**. You must create a separate access policy for each area you want protected, such as Apps, Scripts, or Device Actions.

## Step 1: Sign In

1. Sign in to the **Microsoft Intune Admin Center**.

## Step 2: Create an Access Policy

Navigate to:

```text
Tenant Administration
└── Multi Admin Approval
    └── Access Policies
        └── Create
```

## Step 3: Configure Basics

Enter the following:

- **Name**
- **Description** (optional)
- **Profile Type**

Examples of Profile Types:

- Apps
- Scripts
- Device Actions
- Compliance Policies

> **Note:** Each access policy supports only a single Profile Type.

## Step 4: Configure Approvers

1. Select **Add Groups**.
2. Choose the group that will approve changes.

> **Note:** Advanced configurations such as excluding groups are not supported.

## Step 5: Review and Create

1. Review the configuration.
2. Select **Create** / **Save**.

## Step 6: Approve the Access Policy

A separate administrator must approve the policy.

1. Sign in with a different administrative account.
2. Review the pending request.
3. Approve the access policy.

## Step 7: Complete the Policy

Return to the original administrator account.

1. Open the policy.
2. Select **Complete**.
3. Allow Intune to apply the policy.

✅ At this point, MAA is active for the selected Profile Type.

---

# MAA Enablement Workflow

```text
Admin A Creates Policy
          |
          v
Request Submitted
          |
          v
Admin B Reviews
          |
     +----+----+
     |         |
 Approve     Reject
     |
     v
Admin A Selects Complete
     |
     v
Policy Becomes Active
```

---

# What You Can Protect with MAA

The following Intune resource types support Multi-Admin Approval:

| Resource Type | Supported |
|---------------|-----------|
| Apps | ✅ |
| Compliance Policies | ✅ |
| Configuration Policies (Settings Catalog) | ✅ |
| Device Actions (Wipe, Retire, Delete) | ✅ |
| RBAC Changes | ✅ |
| Windows Scripts | ✅ |
| Access Policies | ✅ |
| Tenant Configuration (Device Categories) | ✅ |

---

# Day-to-Day Operations After MAA Is Enabled

## A. Requestor Flow (Submitting Changes)

1. Create or modify the resource as normal.
2. On the final page before saving:
   - Enter a **Business Justification**
   - Submit the request
3. Track requests at:

```text
Tenant Administration
└── Multi Admin Approval
    └── My Requests
```

4. Requests can be canceled before approval.

### Requestor Workflow

```text
Create Change
      |
      v
Enter Business Justification
      |
      v
Submit Request
      |
      v
Track in My Requests
```

---

## B. Approver Flow (Reviewing Requests)

Navigate to:

```text
Tenant Administration
└── Multi Admin Approval
    └── Received Requests
```

Steps:

1. Select the request.
2. Review the **Business Justification**.
3. Enter **Approver Notes**.
4. Choose:
   - Approve Request
   - Reject Request

### Approval Workflow

```text
Received Request
       |
       v
Review Justification
       |
       v
Add Approver Notes
       |
   +---+---+
   |       |
Approve  Reject
```

---

## C. Finalization Flow

After approval:

1. The original requestor must open the request.
2. Select **Complete**.
3. Intune processes the change.
4. Status changes to **Completed**.

### Completion Workflow

```text
Request Approved
        |
        v
Requestor Opens Request
        |
        v
Select Complete
        |
        v
Intune Applies Change
        |
        v
Completed
```

---

# Notes That Save You Pain Later

| Consideration | Details |
|--------------|---------|
| Request Expiration | Requests expire after 3 days if not completed |
| Self-Approval | An administrator cannot approve their own request |
| Unlicensed Admin Setting | Once enabled, it cannot be disabled |

## Common Pitfalls

### Expired Requests

```text
Request Submitted
       |
       v
No Action for 3 Days
       |
       v
Request Expires
       |
       v
Resubmit Required
```

### Self-Approval Attempt

```text
Requestor
    |
    v
Attempts Approval
    |
    v
Blocked
```

### Unlicensed Admin Option

```text
Enable "Allow Access to Unlicensed Admins"
                 |
                 v
Permanent Change
                 |
                 v
Cannot Be Reversed
```

---

# Key Takeaways

- MAA is configured **per resource type**.
- A separate administrator must approve changes.
- The requestor must still **Complete** the request after approval.
- Requests expire after **3 days** if not processed.
- Administrators **cannot approve their own requests**.
- The **Allow Access to Unlicensed Admins** setting is a **one-way change**.
