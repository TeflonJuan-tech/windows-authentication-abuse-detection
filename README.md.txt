# Authentication Abuse Detection with Splunk

## Overview
This project demonstrates detection engineering techniques for identifying Windows authentication abuse using native Security logs ingested into Splunk. The focus is on detecting brute-force attacks, password spray attacks, and account lockouts through validated telemetry, tuned thresholds, and SOC-style visualization.

## Environment
- Windows 10 VM (local users)
- Splunk Enterprise (Linux VM)
- Splunk Universal Forwarder
- Windows Security Logs (`XmlWinEventLog:Security`)

## Telemetry Validation
Before building detections, the lab validated end-to-end telemetry health:
- Confirmed Windows generated failed logon events (Event ID 4625)
- Added and validated `WinEventLog://Security` input
- Verified Security events were ingested into Splunk via XML
- Confirmed account lockout events (Event ID 4740)

## Detection Logic
The following detection patterns were implemented and validated using real telemetry:

### Brute Force Detection
Identifies high-volume failed logons targeting a single user within a short time window.

- Event ID: 4625
- Threshold: ≥ 5 failures in 5 minutes per user

### Password Spray Detection
Identifies low-volume failed logons across multiple distinct users from a single source.

- Event ID: 4625
- Threshold: ≥ 3 users and ≥ 3 failures within 10 minutes

### Account Lockout Detection
Identifies accounts locked due to authentication abuse.

- Event ID: 4740

Detection searches are stored in the `searches/` directory.

## Dashboard
A dedicated dashboard was built to visualize authentication abuse patterns:
- Failed logons over time
- Potential brute-force activity
- Potential password spray activity
- Account lockouts

The dashboard export is available in the `dashboard/` directory.

## Key Takeaways
- Authentication abuse patterns must be differentiated by depth (brute force) vs breadth (spray)
- Detection thresholds should be tuned to real environment behavior
- XML-based Windows logs require intentional parsing strategies
- Visualization is critical for rapid SOC triage

## Future Work
- Endpoint telemetry enrichment using Sysmon
- Correlation of authentication abuse with post-authentication behavior
- Alerting and response automation

