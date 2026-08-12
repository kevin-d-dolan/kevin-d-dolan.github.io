# Intune Runbooks

../index.md

---

Practical troubleshooting and implementation runbooks for Microsoft Intune, Windows Autopilot, endpoint management, and enterprise mobility scenarios.

## Available Runbooks

### Windows Autopilot

#### Autopilot Troubleshooting Decision Tree

A stage-based troubleshooting methodology covering:

- Registration and profile assignment
- Network connectivity
- Entra join
- MDM enrollment
- Enrollment Status Page (ESP)
- Diagnostic tools and log analysis

📖 **Guide:** ./Autopilot-Troubleshooting-Decision-Tree.md

#### Autopilot Troubleshooting Framework

A structured troubleshooting framework designed for:

- Customer workshops
- Incident response
- Root cause analysis
- Escalation preparation

📖 **Guide:** [Autopilot Troubleshooting Framework](./Autopilot-Troubleshooting Methodology

For most endpoint deployment issues, follow this workflow:

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
