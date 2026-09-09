# System Selection, Workshop 1

**Student:** Sindi Banda
**Tool:** Claude Code
**Date:** 2026-09-02

---

## System

OT Security Information & Event Management (OT-SIEM)

**Purdue Zone:** Level 3 Site Operations

**System type:** Monitoring/Security

---

## Purdue Zone and Safety Classification

Sitting at Level 3 with a security monitoring role means OT-SIEM is attacked through the feeds it trusts rather than through a control interface it does not have, and it is defended by protecting those feeds and its own supporting network at a level equal to or greater than the systems it watches, because an attacker who quietly degrades it removes the plant's ability to detect everything else.

---

## Rationale

I selected OT Security Information & Event Management because my background in data and databases gives me a useful starting point for understanding how the system works.

I already understand data collection, data integrity, and the process of identifying irregular patterns in data. That gives me a mental model for understanding how a monitoring system can receive information from different sources and use that information to identify abnormal or suspicious activity.

What I do not yet understand is how OT-SIEM gains visibility into physical plant activity, especially how information from lower-level devices such as sensors and actuators moves through controllers, monitoring systems, or historians before reaching OT-SIEM. This is the first knowledge gap I want the attack surface mapping exercise to help me close.

The plant architecture places OT-SIEM at Level 3 Site Operations. ICT-SIEM sits above it, with OT-SIEM sending information upward to ICT-SIEM through a one-way export across the separation boundary. The diagram also shows direct inputs to OT-SIEM from RMS, PSI, and the PPC/Historian. Below OT-SIEM, the DCS connects to the PPC/Historian, creating an indirect path through which operational information can reach the monitoring system. The diagram shows no direct DCS to OT-SIEM connection, which means all of OT-SIEM's process visibility arrives through PPC.

These connections make OT-SIEM a useful starting point for understanding how data moves from plant operations into cybersecurity monitoring and how the security of that data path affects the reliability of detection.

---

## Workshop Fit Analysis

**W1 Attack Surface Mapping & System Selection: strong.** OT-SIEM's interfaces reach into several other systems, so mapping its four architecture connections produces a partial inventory of the plant's monitoring paths as a byproduct.

**W2 Adversary Profiling & Threat Modeling: moderate.** OT-SIEM is a target an adversary attacks to stay hidden rather than to cause an effect, which is a useful case but a narrower one than a system with direct process consequences.

**W3 Protocol Security & Access Control: moderate.** The RMS feed is labeled Syslog / OPC-UA, which gives one concrete protocol to analyze, but OT-SIEM does not carry the Modbus and DNP3 control traffic that this module centers on.

**W4 ICS/SCADA & Digital I&C Security: weak.** OT-SIEM is not a control system, has no safety classification, and issues no actuation commands, so it cannot anchor a digital I&C analysis.

**W5 Physical Protection & Regulatory Frameworks: moderate.** OT-SIEM has real physical exposure through its console ports and its dependence on power, cooling, and fire suppression, but the regulatory triad is better traced through a safety system.

**W6 Monitoring Strategy & Supply Chain Security: strong.** This is the module OT-SIEM belongs to. RG 5.71 Rev. 1 Section C.4.1 names SIEM directly as a tool supporting anomaly detection under continuous monitoring.

**W7 Incident Response in Nuclear Environments: strong.** OT-SIEM is what the response team relies on to know an incident is happening, and the case where OT-SIEM itself is compromised is the hardest version of that problem.

**W8 Side Channels, AI & Emerging Threats: moderate.** Behavioral anomaly detection puts OT-SIEM in reach of baseline poisoning and adversarial ML questions, but side-channel analysis targets embedded hardware rather than a monitoring server.

**W9 Ethics, Policy & Portfolio Synthesis: strong.** OT-SIEM holds security and monitoring data about people and plant activity, which raises the disclosure and access questions this module asks.

**Where the fit is weakest and how I address it.** W4 is the weakest fit, and W5 is the next weakest. My sequencing plan puts RPS in W4 so the digital I&C analysis has a safety-critical system to work on, and puts RPS and SFPCM in W5 so the regulatory and physical protection work sits on systems with real physical consequence. That leaves OT-SIEM in W1 where it is strongest, and lets the W6, W7, and W9 modules return to monitoring questions using the systems that feed it.
