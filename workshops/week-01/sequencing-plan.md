# Sequencing Plan — CS 581

**Student:** Sindi Banda
**W1 System:** OT-SIEM — OT Security Information & Event Mgmt

## Workshop-System Map

| Workshop | Theme | System(s) | Justification |
|---|---|---|---|
| W1 (Sep 8) | Attack Surface Mapping & System Selection | OT-SIEM | OT-SIEM sits at Purdue Level 3 and ingests from the zones below it through collectors, forwarders, and taps, so mapping its ingest paths produces a first inventory of the plant OT attack surface as a byproduct. |
| W2 (Sep 21) | Adversary Profiling & Threat Modeling | PPC | The historian carries no safety function but stores the operational record an adversary would want to read or falsify, which makes it the right system for separating espionage-motivated actors from sabotage-motivated ones. |
| W3 (Oct 5) | Protocol Security & Access Control | DCS | The DCS carries Modbus and DNP3 traffic between operator consoles and field controllers, giving concrete CVE-grounded targets for the protocol analysis and a real zone boundary for the access control half. |
| W3 (Oct 5) | Protocol Security & Access Control | EDGC | EDG control logic sits idle until it must accept an automatic start command, so authentication and replay questions apply to a command path that is rarely exercised and rarely monitored. |
| W4 (Oct 12) | ICS/SCADA & Digital I&C Security | RPS | RPS is the Level 1 safety-critical CDA governed by NUREG-0800 Chapter 7, and it is where the emergency access problem is sharpest, since a locked-out operator during a transient is a worse outcome than an unauthorized one. |
| W5 (Oct 26) | Physical Protection & Regulatory Frameworks | SFPCM | Spent fuel pool cooling and monitoring sits inside the protected area and is governed by 10 CFR 73.54 alongside post-Fukushima instrumentation requirements, so physical barriers are its primary compensating control. |
| W5 (Oct 26) | Physical Protection & Regulatory Frameworks | PSI | Physical Security Integration is the system where 10 CFR 73 physical protection and 10 CFR 73.54 cybersecurity govern the same badge readers, door controllers, and cameras under two different NRC authorities. |
| W6 (Nov 16) | Monitoring Strategy & Supply Chain Security | TCS | The turbine control system is vendor-maintained balance-of-plant equipment with remote diagnostic access and vendor-delivered firmware, which is where supply chain trust and monitoring coverage have to be designed against each other. |
| W7 (Nov 30) | Incident Response in Nuclear Environments | RMS | Radiation monitoring readings drive emergency action level classification, so an integrity attack on RMS forces the response team to make 10 CFR 50.72 and 73.71 notification decisions on instrument data it cannot trust. |
| W8 (Dec 7) | Side Channels, AI & Emerging Threats | SMR I&C | SMR I&C combines shared multi-module control, heavy automation, and small embedded controllers, which puts power-analysis exposure and adversarial ML risk on the same platform with the thinnest existing regulatory coverage. |
| W9 (Dec 14) | Ethics, Policy & Portfolio Synthesis | ICT-SIEM | The Level 4 corporate SIEM holds employee monitoring and investigative data, which turns the final workshop into the disclosure and export control questions about who sees plant security data and under what obligation. |

All 11 systems appear once. No system repeats, and every workshop carries at least one system.

## Semester Arc

The thread is data: where it is produced, how it moves, who can alter it, and what decisions get made on top of it. W1 starts at the aggregation point, because OT-SIEM is the one system whose interfaces touch nearly every other system in the plant, and mapping its ingest paths forces an early inventory of the whole architecture. W2 moves one layer down to the PPC historian, which is the same problem I already know from database work: a store of operational record with retention rules, integrity requirements, and no safety function of its own. W3 goes down again to the protocols that generate that record, using DCS as the busy path and EDGC as the rarely exercised one, so I am examining authentication gaps in traffic I have so far only seen after it was parsed and indexed. W4 through W7 apply that view to systems where the data has physical consequence: RPS access control, SFPCM and PSI under the regulatory frameworks, TCS supply chain, and RMS during an incident where the readings themselves are suspect. W9 returns to a monitoring platform, which lets the portfolio close on the ICT and OT boundary it opened on, with eight workshops of plant context in between.

The comfort zone ends at W4. RPS is deterministic, safety-qualified logic with one-way communication and no patch window, and my instinct to solve integrity problems with logging, replication, and after-the-fact reconciliation does not transfer to a system where the control either holds in real time or does not. W5 is the harder of the two multi-week blocks: PSI and SFPCM are where cyber controls rest on physical assumptions I have never had to reason about, and the module requires holding safety, security, and safeguards as three separate authorities with three separate definitions of resolved. W8 is the other hard one, since side-channel analysis is a hardware and physics argument rather than a data argument, and I expect to spend most of that workshop verifying claims I cannot evaluate from experience. W6 and W7 should be the strongest weeks, because consequence-driven monitoring design and incident triage under untrusted data are closest to what I already do.

## Revision Log
| Date | Change | Reason |
|---|---|---|
| 2026-09-03 | Initial plan | — |
| 2026-09-03 | Moved PSI from W2 to W5 | curriculum.md defines PSI as Physical Security Integration, a Security CDA at Purdue Level 2-3. W5 is the physical protection and regulatory module, which is a primary-affinity match. The insider threat angle at W2 was a secondary fit. |
| 2026-09-03 | Removed the second RPS appearance from W5 | RPS was listed at both W4 and W5, which produced one duplicate and no coverage benefit. Dropping the W5 repeat gives every workshop distinct systems and covers all 11 with no redundancy. RPS keeps W4, where the digital I&C and emergency access framing is strongest. |