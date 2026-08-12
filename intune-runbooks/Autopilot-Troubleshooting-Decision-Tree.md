# Stage-Based Autopilot Troubleshooting Methodology

## My Favorite Autopilot Troubleshooting Flow

---

# Phase 1: Registration & Profile Assignment

### Verify

- Device exists in **Windows Autopilot Devices**
- Hardware hash imported successfully
- Deployment profile assigned
- Assignment status = **Assigned** (not Pending)
- Device appears as **Corporate**

### Common Failures

- No profile assigned
- Hardware hash registered to another tenant
- Incorrect dynamic group targeting

### Check

```text
Intune Admin Center
└─ Devices
   └─ Windows
      └─ Windows Enrollment
         └─ Devices
```

---

# Phase 2: Network Connectivity

Autopilot cannot start if it cannot reach Microsoft endpoints.

### Quick Tests

- Try a mobile hotspot
- Remove SSL inspection
- Review proxy/firewall configuration

### Common Symptoms

- "Something went wrong"
- Device receives normal OOBE instead of Autopilot
- Profile never downloads

---

# Phase 3: Entra Join

This is where many customers get stuck.

### User Device Join Permissions

Check:

```text
Entra Admin Center
└─ Identity
   └─ Devices
      └─ Device Settings
```

Verify:

- Users may join devices = All (or appropriate group)
- Device limit not exceeded

### Conditional Access

Most common blockers:

- Require MFA
- Require compliant device
- Block all cloud apps

Exclude:

- Microsoft Intune
- Microsoft Intune Enrollment
- Device Registration Service

---

# Phase 4: MDM Enrollment

If Autopilot gets past sign-in but fails during enrollment.

## MDM User Scope

Check:

```text
Entra Admin Center
└─ Mobility (MDM and MAM)
```

Verify:

- MDM user scope includes the enrolling user

## Licensing

Verify the user has one of:

- Intune Plan 1
- EMS E3/E5
- Microsoft 365 E3/E5
- Business Premium

## Enrollment Restrictions

Check:

```text
Intune Admin Center
└─ Devices
   └─ Enrollment
      └─ Enrollment Restrictions
```

Verify:

- Windows enrollment allowed
- Enrollment restrictions not blocking enrollment

---

# Phase 5: Enrollment Status Page (ESP)

This is where most customer incidents arrive.

## Typical Symptoms

- Stuck on "Identifying"
- Stuck on "Installing Apps"
- ESP timeout

## Common Root Causes

- Failed Win32 app
- Detection rule issue
- Security baseline requiring reboot
- AppLocker CSP conflicts
- Device lock policies
- Kiosk configuration issues

---

# Best Diagnostic Tools

## Ctrl + Shift + D

During OOBE:

```text
Ctrl + Shift + D
```

Provides Autopilot diagnostics.

---

## Get-AutopilotDiagnostics.ps1

```powershell
Get-AutopilotDiagnostics.ps1
```

or

```powershell
Get-AutopilotDiagnostics.ps1 -CabFile <file>
```

Useful for:

- Failed app identification
- ESP stage analysis
- Policy processing failures

---

## IntuneManagementExtension.log

Location:

```text
C:\ProgramData\Microsoft\IntuneManagementExtension\Logs
```

Search for:

- Failed app ID
- Detection rule failures
- Installation return codes

---

## Event Viewer

```text
Applications and Services Logs
└─ Microsoft
   └─ Windows
      └─ DeviceManagement-Enterprise-Diagnostics-Provider
```

Commonly used for:

- Enrollment errors
- CSP failures
- Policy processing failures

---

# Top 10 Autopilot Failure Causes

1. MDM User Scope not configured
2. Intune license missing
3. Conditional Access blocking enrollment
4. User can't join devices in Entra
5. Enrollment restrictions blocking device
6. Device limit exceeded
7. Required Win32 app failure
8. Security Baseline requiring reboot during ESP
9. Network or SSL inspection issues
10. Device not assigned to an Autopilot profile

---

# CSA Runbook

When a customer says:

> Autopilot failed

Immediately ask:

1. What exact screen?
2. What exact error code?
3. User-driven, Self-deploying, Pre-provisioning, or Hybrid?
4. Where did it stop?

### Before Login

- OOBE
- Network
- Registration

### During Login

- Entra Join
- Device Registration

### After Login

- ESP Device Setup
- ESP Account Setup
- Desktop

---

# 5-Workstream Troubleshooting Model

```text
Registration
    ↓
Network
    ↓
Entra Join
    ↓
MDM Enrollment
    ↓
ESP
```

**Key Principle:** Once you identify the stage where Autopilot failed, you've typically eliminated 80% of the possible root causes.
