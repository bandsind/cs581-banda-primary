# System Selection — Week 1

**Chosen system:** OT Security Information & Event Management

**Purdue Zone:** Level 3 Site Operations

**System type:** Monitoring/Security

---

## Rationale

I selected OT Security Information & Event Management because my background in data and databases gives me a useful starting point for understanding how the system works. 

I already understand data collection, data integrity, and the process of identifying irregular patterns in data. That gives me a mental model for understanding how a monitoring system can receive information from different sources and use that information to identify abnormal or suspicious activity. 

What I do not yet understand is how OT-SIEM gains visibility into physical plant activity, especially how information from lower-level devices such as sensors and actuators moves through controllers, monitoring systems, or historians before reaching OT-SIEM. This is the first knowledge gap I want the attack surface mapping exercise to help me close. 

The plant architecture places OT-SIEM at Level 3 Site Operations. ICT-SIEM sits above it, with OT-SIEM sending information upward to ICT-SIEM. The diagram also shows direct inputs to OT-SIEM from RMS, PSI, and the PPC/Historian. Below OT-SIEM, the DCS connects to the PPC/Historian, creating an indirect path through which operational information can reach the monitoring system. 

These connections make OT-SIEM a useful starting point for understanding how data moves from plant operations into cybersecurity monitoring and how the security of that data path affects the reliability of detection.
