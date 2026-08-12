# Conditional Access and Compliance Architecture

## Overview

Conditional Access and Compliance work together to ensure that only trusted users on trusted devices can access organizational resources.

This architecture combines:

- Microsoft Entra ID (Identity Provider)
- Conditional Access Policies
- Microsoft Intune Compliance Policies
- Managed Devices
- Microsoft 365 Applications

The result is a Zero Trust approach where access decisions are based on:

- User identity
- Device compliance state
- Risk signals
- Location
- Application
- Authentication strength

Microsoft's recommended model uses Intune compliance status as a signal that Conditional Access can enforce during sign-in. Set up device-based Conditional Access policies with Intune - Microsoft Intune | Microsoft Learn states that device-based Conditional Access uses compliance status signals from Intune to enforce access controls in Microsoft Entra ID. 【1-fca55d】

---

# Business Problem

Organizations need to answer:

> How do we ensure only secure devices can access company data?

### Without Conditional Access

- Personal devices can access data
- Unpatched devices remain connected
- Lost devices can still be used
- Security teams have limited enforcement capabilities

### With Conditional Access and Compliance

- Devices must meet security requirements
- Non-compliant devices lose access
- Security becomes automated
- Risk is reduced

---

# High-Level Architecture

```text
┌─────────────────────────┐
│         User            │
└────────────┬────────────┘
             │
             ▼

┌─────────────────────────┐
│    Microsoft Entra      │
│        Sign-In          │
└────────────┬────────────┘
             │
             ▼

┌─────────────────────────┐
│  Conditional Access     │
│     Policy Engine       │
└────────────┬────────────┘
             │
             ▼

     Is Device Compliant?

             │
             ▼

┌─────────────────────────┐
│         Intune          │
│ Compliance Evaluation   │
└────────────┬────────────┘
             │
             ▼

       Compliant?
         Yes / No

             │
             ▼

┌─────────────────────────┐
│ Access Granted/Denied   │
└─────────────────────────┘
```

---

# Authentication Flow

## Step 1

User launches:

- Outlook
- Teams
- SharePoint
- OneDrive
- Custom Enterprise App

## Step 2

User authenticates against:

```text
Microsoft Entra ID
```

Identity validation occurs.

## Step 3

Conditional Access evaluates:

```text
Who is the user?

What application?

What device?

What location?

What risk?

What authentication method?
```

## Step 4

Policy requires:

```text
Require device to be marked as compliant
```

Conditional Access requests compliance status.

## Step 5

Entra retrieves compliance data from Intune.

```text
Compliant
Not Compliant
Unknown
```

Intune acts as the authoritative source for device compliance. Device-based Conditional Access uses compliance status signals from Intune to enforce access controls in Microsoft Entra ID. 【1-fca55d】

## Step 6

Conditional Access determines outcome.

### Compliant

```text
Access Allowed
```

### Non-Compliant

```text
Access Blocked
```

Or:

```text
Prompt user to remediate
```

---

# Compliance Policy Architecture

## Windows Compliance

Typical controls:

- BitLocker Enabled
- Secure Boot Enabled
- Antivirus Active
- Firewall Enabled
- Minimum OS Version
- TPM Available

## iOS Compliance

Typical controls:

- Passcode Required
- Minimum iOS Version
- No Jailbreak Detected
- Threat Level Approved

## Android Compliance

Typical controls:

- PIN Required
- Encryption Enabled
- Minimum Android Version
- Root Detection

## macOS Compliance

Typical controls:

- FileVault Enabled
- OS Version Compliant
- Firewall Enabled
- Password Policy Met

---

# Example Conditional Access Design

## Policy 1: Require MFA

```text
Users:
All Users

Apps:
Exchange
SharePoint
Teams

Grant:
Require MFA
```

## Policy 2: Require Device Compliance

```text
Users:
Employees

Apps:
All Cloud Apps

Grant:
Require Compliant Device
```

## Policy 3: Block Legacy Authentication

```text
Users:
All Users

Condition:
Legacy Authentication

Result:
Block
```

## Policy 4: Admin Protection

```text
Users:
Administrative Roles

Grant:
Require MFA
Require Compliant Device
Require Authentication Strength
```

---

# Recommended Rollout Strategy

## Phase 1

Create compliance policies.

```text
No Conditional Access Enforcement
```

**Goal:**

- Measure compliance rates

## Phase 2

Enable Report-Only Mode.

```text
Conditional Access = Report Only
```

Identify:

- Who would be blocked
- Which devices are unmanaged
- Potential business impact

Microsoft recommends testing policies with smaller groups before broad deployment and defaults new policies to Report-only. 【1-fca55d】

## Phase 3

### Pilot Group

- IT Team
- Security Team
- Technical Champions

Validate:

- Enrollment
- Compliance
- User experience

## Phase 4

### Department Rollout

- HR
- Finance
- Operations
- Sales

Monitor impact.

## Phase 5

### Full Production

```text
All Users
```

Maintain exclusions for:

- Emergency Access Accounts
- Break Glass Accounts

Microsoft recommends using exclusions and cautions administrators against locking themselves out when protecting all cloud apps. 【1-fca55d】

---

# Common Pitfalls

## No Compliance Policy Exists

### Issue

```text
CA Policy Requires Compliance
+
No Compliance Policy
=
Access Problems
```

Microsoft states that device compliance policies must be configured before Conditional Access can evaluate device compliance. 【1-fca55d】

## Missing Break Glass Account

### Risk

```text
Tenant Lockout
```

Always maintain emergency access accounts and exclusions.

## Enforcing Too Early

### Risk

```text
Large User Impact
```

Use:

```text
Report Only
```

before enforcement.

## Unmanaged BYOD Devices

### Possible Outcomes

- Access Denied
- User Frustration
- Support Tickets

Plan enrollment and management strategy first.

---

# Troubleshooting Flow

```text
User Blocked
      │
      ▼

Check Sign-In Logs
      │
      ▼

Conditional Access Failure?
      │
      ├── No → Investigate Authentication
      │
      └── Yes
               │
               ▼

Check Device Status
               │
               ▼

Compliant?
      │
      ├── No → Fix Compliance
      │
      └── Yes
               │
               ▼

Check Policy Assignment
               │
               ▼

Validate Access
```

---

# CSA Design Recommendations

When I deploy Conditional Access with customers, I typically recommend:

1. MFA before Compliance
2. Compliance before Risk-Based Policies
3. Pilot before Production
4. Report-Only before Enforcement
5. Break Glass Accounts before All Other Policies
6. Regular Sign-In Log Reviews
7. Separate Admin and User Policies

This creates a secure, scalable, and supportable architecture aligned with Zero Trust principles.

---

# Key Takeaways

- ✅ Conditional Access makes access decisions
- ✅ Intune determines device compliance
- ✅ Entra ID performs authentication
- ✅ Compliance status becomes an access signal
- ✅ Report-Only reduces deployment risk
- ✅ Break Glass accounts protect against lockouts
- ✅ Together they form a foundational Zero Trust security architecture

---

# Related Guides

- Autopilot Architecture
- Intune Enrollment Architecture
- Compliance Policy Design Guide
- Zero Trust for Endpoint Management
- Privileged Access and Administrative Protection
- Mobile Application Management (MAM) Architecture

---

## Author

Kevin Dolan

## Focus Areas

Microsoft Intune | Conditional Access | Microsoft Entra ID | Zero Trust | Endpoint Security | Enterprise Mobility
