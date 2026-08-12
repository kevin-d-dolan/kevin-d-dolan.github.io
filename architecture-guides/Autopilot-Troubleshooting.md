# Windows Autopilot Troubleshooting Framework

> **CSA Methodology:** Troubleshoot by provisioning stage rather than by error code.

## Autopilot Troubleshooting Flow

```text
Registration & Profile Assignment
                ↓
      Network Connectivity
                ↓
           Entra Join
                ↓
         MDM Enrollment
                ↓
      Enrollment Status Page
```

---

# Phase 1: Registration & Profile Assignment

## Verify

- Device exists in **Windows Autopilot Devices**
- Hardware hash imported successfully
- Deployment profile assigned
- Assignment Status = **Assigned**
- Device appears as **Corporate**

## Common Failures

- No profile assigned
- Hardware hash registered to another tenant
- Incorrect dynamic group targeting

## Check

```text
Intune Admin Center
→ Devices
→ Windows
→ Windows Enrollment
→ Devices
```

### References

- Windows Autopilot Troubleshooting FAQ
- Manual Registration Documentation

---

# Phase 2: Network Connectivity

Autopilot cannot start if it cannot reach Microsoft endpoints.

## Quick Tests

- Test using a mobile hotspot
- Temporarily bypass SSL inspection
- Review proxy configuration
- Review firewall configuration

## Common Symptoms

- Generic "Something went wrong" error
- Standard OOBE appears instead of Autopilot
- Deployment profile never downloads

### Key Point

Network connectivity is one of the most common root causes of Autopilot failures.

---

# Phase 3: Entra Join

This is one of the most common failure points.

## Verify Device Join Permissions

```text
Entra Admin Center
→ Identity
→ Devices
→ Device Settings
```

### Check

- Users may join devices = All (or appropriate group)
- Device limit not exceeded

## Conditional Access

### Common Blockers

- Require MFA
- Require Compliant Device
- Block access to cloud apps

### Exclude These Cloud Apps

- Microsoft Intune
- Microsoft Intune Enrollment
- Device Registration Service

### Common Outcome

Autopilot often fails when Conditional Access policies block registration or enrollment services.

---

# Phase 4: MDM Enrollment

If Autopilot gets past sign-in but fails during enrollment, focus here.

## MDM User Scope

```text
Entra Admin Center
→ Mobility (MDM and MAM)
```

### Verify

- User is included in MDM User Scope

---

## Licensing

Verify the user has one of the following:

| License | Supported |
|----------|----------|
| Intune Plan 1 | ✅ |
| EMS E3 | ✅ |
| EMS E5 | ✅ |
| Microsoft 365 E3 | ✅ |
| Microsoft 365 E5 | ✅ |
| Business Premium | ✅ |

### Common Symptom

Missing licensing can cause silent enrollment failures.

---

## Device Enrollment Restrictions

```text
Intune Admin Center
→ Devices
→ Enrollment
→ Enrollment Restrictions
```

### Verify

- Windows enrollment allowed
- Platform restrictions not blocking enrollment
- Personal/corporate restrictions configured correctly

---

# Phase 5: Enrollment Status Page (ESP)

This is where most customer incidents occur.

## Typical Symptoms

- Stuck on **Identifying**
- Stuck on **
