# Sequencing Plan - CS 581

**Student:** Sindi Banda

**W1 System:** OT-SIEM - OT Security Information & Event Mgmt

## Workshop-System Map

| Workshop | Theme | System(s) | Justification |
|---|---|---|---|
| W1 (Sep 8) | Attack Surface Mapping & System Selection | OT-SIEM | OT-SIEM sits at Purdue Level 3 and receives security information from several plant systems, so mapping its connections gives me a starting view of how monitoring data moves through the plant OT environment. |
| W2 (Sep 21) | Adversary Profiling & Threat Modeling | PPC | The historian stores operational records that an adversary may want to read, alter, or suppress, making PPC useful for comparing espionage and sabotage objectives. |
| W2 (Sep 21) | Adversary Profiling & Threat Modeling | PSI | PSI handles physical security information such as access and security events, making it useful for examining insider threats and adversaries who may already have some level of authorized access. |
| W3 (Oct 5) | Protocol Security & Access Control | DCS | The DCS uses several industrial communication protocols and connects supervisory functions with lower-level control systems, making it a strong standalone system for studying protocol weaknesses and access boundaries. |
| W4 (Oct 12) | ICS/SCADA & Digital I&C Security | RPS | RPS is a Level 1 safety-critical digital system, making it useful for studying how access control and digital I&C security change when a cyber event could affect a reactor protection function. |
| W5 (Oct 26) | Physical Protection & Regulatory Frameworks | SFPCM | Spent fuel pool cooling and monitoring provides a useful system for examining how cybersecurity, physical protection, safety consequences, and regulatory requirements interact around critical plant equipment. |
| W6 (Nov 16) | Monitoring Strategy & Supply Chain Security | TCS | TCS depends on specialized control hardware, software, and vendor support, making it useful for studying supply chain trust and how monitoring should account for risks introduced through external products and services. |
| W6 (Nov 16) | Monitoring Strategy & Supply Chain Security | EDGC | EDGC is availability-critical backup power equipment, making it useful for studying how monitoring can distinguish equipment degradation from possible adversarial activity and how maintenance, vendor, and component dependencies affect supply chain risk. |
| W7 (Nov 30) | Incident Response in Nuclear Environments | RMS | Radiation monitoring data can influence plant response decisions, so an integrity or availability problem in RMS provides a useful case for examining incident response when the information used to assess an event may not be trustworthy. |
| W8 (Dec 7) | Side Channels, AI & Emerging Threats | SMR I&C | SMR I&C combines advanced digital control, automation, and embedded systems, making it useful for examining side-channel risks and newer cyber threats in an emerging nuclear architecture. |
| W9 (Dec 14) | Ethics, Policy & Portfolio Synthesis | ICT-SIEM | ICT-SIEM returns the sequence to security monitoring from the enterprise side, allowing me to compare it with the OT-SIEM system I started with and examine how plant security information is handled across the IT and OT boundary. |

All 11 systems appear exactly once across W1 through W9, which meets the coverage requirement.

## Notes and Open Items

These are things to verify or watch, not changes to the plan above.

**W2 PSI.** PSI is Physical Security Integration and is placed at Level 3 in the Plant Architecture Diagram. I kept it in W2 because its access and security-event data provides a useful way to examine insider-threat scenarios. I should still compare this choice with its workshop affinity rating when developing the W2 artifact.

**W3 DCS.** DCS now stands alone in W3. This gives me a focused protocol-security analysis without adding a second system that could take attention away from the DCS multi-protocol environment.

**W6 TCS and EDGC.** W6 now uses two systems. TCS gives me a supply chain case involving specialized control hardware, software, and vendor support. EDGC adds the monitoring side because its availability is important during plant events and its condition can involve both normal equipment degradation and possible adversarial activity.

**W9 ICT-SIEM.** The relationship between OT-SIEM and ICT-SIEM gives the semester a useful beginning and ending. W1 studies monitoring from the OT side, while W9 returns to monitoring from the enterprise side. The W9 artifact should make this comparison clear so the connection to the W1 work is not lost.

**Suggested pairings not used.** The course materials suggest some natural system pairings, including DCS with PPC and RPS with EDGC. This plan separates them because PPC is used as an adversary target in W2, DCS is the main protocol system in W3, RPS is the W4 digital I&C system, and EDGC is placed in W6 where the Plant Architecture Diagram identifies Monitoring and Supply Chain as a primary fit.

**Workshop label discrepancy.** The repo materials use slightly different descriptions for W4. One describes it as ICS/SCADA and Digital I&C Security, while another emphasizes Access Control and Identity. RPS can support either approach, but I should follow the current workshop instructions when W4 begins.

## Semester Arc

The thread is data: where it is produced, how it moves, who can alter it, and what decisions are made from it. W1 starts with OT-SIEM because monitoring brings information from several plant systems into one place. W2 moves into PPC and PSI to examine operational records and physical security information from an adversary perspective, while W3 moves deeper into DCS to study the industrial protocols and access boundaries behind that information. W4 moves into the safety-critical RPS environment, and W5 shifts to SFPCM to examine the connection between cyber risk, physical protection, safety, and regulation. W6 combines TCS and EDGC to examine supply chain trust and monitoring of availability-critical equipment, while W7 and W8 move into incident response and emerging technology. W9 returns to ICT-SIEM so the sequence ends by comparing enterprise monitoring with the OT monitoring system studied in W1.

My comfort zone is strongest around data, monitoring, and integrity, so the sequence becomes harder as it moves closer to physical plant processes. RPS and SFPCM will require me to understand how cyber events can affect safety functions and how nuclear regulatory requirements change the analysis. EDGC will also require me to think about equipment availability and how monitoring separates normal degradation from possible malicious activity. W8 will be challenging because side-channel analysis requires more understanding of hardware and physical behavior than I currently have. W6 and W7 should connect more closely to my existing background because monitoring, supply chain data, incident evidence, and making decisions from information are concepts I already understand.

## Revision Log

| Date | Change | Reason |
|---|---|---|
| 2026-09-03 | Initial plan | - |
| 2026-09-03 | Reviewed against curriculum.md and workshop themes | Confirmed that all 11 systems could be covered across W1 through W9. |
| 2026-09-08 | Corrected the W7 RMS justification from 73.71 to 73.77 | Cyber security event notifications are governed by 10 CFR 73.77 rather than the physical security reporting provisions originally referenced. |
| 2026-09-08 | Added regulatory context to the W6 TCS justification and corrected the PSI level note | The review identified additional context for TCS and confirmed that the course architecture places PSI at Level 3. |
| 2026-09-08 | Removed the second RPS assignment from W5 | RPS is already covered in W4, and all 11 systems are covered without repeating it. |
| 2026-09-08 | Moved EDGC from W3 to W6 | DCS already provides a strong standalone target for W3 protocol analysis. The Plant Architecture Diagram identifies EDGC as a primary fit for W6 Monitoring and Supply Chain, and W6 allows a second system. |