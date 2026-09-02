# Monitoring Strategy — [System Name(s)]
## CS 581 Workshop 6 | [Your Name] | [Date]

**System(s):** <!-- from your sequencing plan; may be a pair -->
**Purdue Zone:**
**Workshop theme:** Monitoring Strategy & Supply Chain Security

---

## What to Monitor

<!--
What events, anomalies, and indicators of compromise should be monitored for this system?
Be specific to its architecture — generic "monitor for unusual traffic" is not sufficient.

Think about:
- Protocol-level anomalies (Modbus write to unexpected register, OPC-UA auth failure)
- Process anomalies (setpoint deviations, unexpected controller state changes)
- Access anomalies (engineering workstation login outside maintenance window)
- Physical anomalies (cabinet door open, USB insertion)
-->

| Event / Indicator | Why It Matters | Data Source | Detection Difficulty | Confidence & Provenance |
|---|---|---|---|---|
| | | | | |

---

## SIEM Architecture

<!--
How does monitoring data from this system reach the OT-SIEM and/or ICT-SIEM?
What crosses the air gap, and how?
What is lost in the crossing (latency, fidelity, context)?
-->

---

## Supply Chain Risks

<!--
What supply chain risks apply to this system?
- Vendor software updates as an attack vector
- Hardware components from adversarial suppliers
- Vendor remote access during maintenance
- Third-party libraries or firmware in controller software

Reference CISA/NSA supply chain guidance and any ICS-CERT advisories on
supply chain compromise of ICS components.
-->

---

## What This Strategy Cannot Tell You
