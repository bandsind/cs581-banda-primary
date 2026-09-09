# Sequencing Plan — CS 581

**Student:** Sindi Banda
**W1 System:** OT-SIEM — OT Security Information & Event Mgmt

## Workshop-System Map

| Workshop | Theme | System(s) | Justification |
|---|---|---|---|
| W1 (Sep 8) | Attack Surface Mapping & System Selection | OT-SIEM | OT-SIEM sits at Purdue Level 3 and ingests from the zones below it through collectors, forwarders, and taps, so mapping its ingest paths produces a first inventory of the plant OT attack surface as a byproduct. |
| W2 (Sep 21) | Adversary Profiling & Threat Modeling | PPC | The historian carries no safety function but stores the operational record an adversary would want to read or falsify, which makes it the right system for separating espionage-motivated actors from sabotage-motivated ones. |
| W2 (Sep 21) | Adversary Profiling & Threat Modeling | PSI | PSI holds the badge, access authorization, and entry log records that 10 CFR 73.56 governs, so it is the concrete surface for the insider threat half of this module where the adversary is already inside the protected area. |
| W3 (Oct 5) | Protocol Security & Access Control | DCS | The DCS carries Modbus and DNP3 traffic between operator consoles and field controllers, giving concrete CVE-grounded targets for the protocol analysis and a real zone boundary for the access control half. |
| W3 (Oct 5) | Protocol Security & Access Control | EDGC | EDG control logic sits idle until it must accept an automatic start command, so authentication and replay questions apply to a command path that is rarely exercised and rarely monitored. |
| W4 (Oct 12) | ICS/SCADA & Digital I&C Security | RPS | RPS is the Level 1 safety-critical CDA governed by NUREG-0800 Chapter 7, and it is where the emergency access problem is sharpest, since a locked-out operator during a transient is a worse outcome than an unauthorized one. |
| W5 (Oct 26) | Physical Protection & Regulatory Frameworks | RPS | RPS is the one system whose qualification basis runs through Safety (10 CFR 50 Appendix B, IEEE 603) and Security (10 CFR 73.54) at the same time, so tracing a single system across both authorities is the cleanest way into the regulatory triad. |
| W5 (Oct 26) | Physical Protection & Regulatory Frameworks | SFPCM | Spent fuel pool cooling and monitoring sits inside the protected area and is governed by 10 CFR 73.54 alongside post-Fukushima instrumentation requirements, so physical barriers are its primary compensating control. |
| W6 (Nov 16) | Monitoring Strategy & Supply Chain Security | TCS | The turbine control system is vendor-maintained balance-of-plant equipment with remote diagnostic access and vendor-delivered firmware, which is where supply chain trust and monitoring coverage have to be designed against each other. |
| W7 (Nov 30) | Incident Response in Nuclear Environments | RMS | Radiation monitoring readings drive emergency action level classification, so an integrity attack on RMS forces the response team to make 10 CFR 50.72 and 73.71 notification decisions on instrument data it cannot trust. |
| W8 (Dec 7) | Side Channels, AI & Emerging Threats | SMR I&C | SMR I&C combines shared multi-module control, heavy automation, and small embedded controllers, which puts power-analysis exposure and adversarial ML risk on the same platform with the thinnest existing regulatory coverage. |
| W9 (Dec 14) | Ethics, Policy & Portfolio Synthesis | ICT-SIEM | The Level 4 corporate SIEM holds employee monitoring and investigative data, which turns the final workshop into the disclosure and export control questions about who sees plant security data and under what obligation. |

All 11 systems appear at least once, which meets the coverage requirement. RPS appears twice, at W4 and W5.

## Notes and Open Items

These are things to verify or watch, not changes to the plan above.

**W5 RPS revisit.** The second RPS appearance is the weakest slot in the plan and needs a deliverable that is visibly different from W4. W4 is a technical digital I&C and access control analysis; W5 has to be a regulatory analysis that traces RPS through Safety, Security, and Safeguards authorities. If the W5 artifact starts restating W4 findings, move the revisit out. W6 and W8 are both two-week workshops currently holding one system each, so there is capacity to relocate it without breaking coverage.

**W2 PSI.** PSI is Physical Security Integration, a Security CDA at Purdue Level 2-3, and its strongest theme affinity is W5 rather than W2. The insider threat half of Module 2 supports the W2 placement, but check PSI's W2 affinity rating on the Plant Architecture Diagram before committing. If it rates only Applicable rather than Strong, the cleanest fix is to swap PSI into W5 and drop the RPS revisit.

**W9 ICT-SIEM.** curriculum.md explicitly suggests pairing OT-SIEM and ICT-SIEM in one workshop because the two are most meaningful in contrast with each other. W1 is locked to OT-SIEM alone, so that contrast is stretched across the full semester instead. Make the W9 artifact explicitly comparative and carry the W1 attack surface map forward into it, otherwise the pairing rationale is lost.

**W6 TCS.** W6 has two deliverables, a CCE monitoring strategy and a supply chain assessment. TCS anchors the supply chain half well through vendor firmware and remote diagnostics. It is a weaker anchor for the consequence-first monitoring half, since balance-of-plant consequences rank below core damage and spent fuel outcomes. Reference the higher-consequence systems already analyzed in W5 when building the consequence-prioritized monitoring targets.

**Suggested pairings not used.** curriculum.md suggests DCS with PPC and RPS with EDGC. This plan splits both. That is allowed, but expect to justify it, so state the reasoning in the W2 and W3 artifacts: PPC is treated as an adversary objective rather than a data feed, and EDGC is placed where its dormant command path is the finding.

**Workshop label discrepancy.** The repo README lists M4 (Week 7) as ICS/SCADA & Digital I&C Security, while curriculum.md describes Module 4 as Access Control & Identity with an RBAC and Purdue zone workshop. Confirm which framing governs the W4 deliverable. RPS works for either, but the artifact format differs.

## Semester Arc

The thread is data: where it is produced, how it moves, who can alter it, and what decisions get made on top of it. W1 starts at the aggregation point, because OT-SIEM is the one system whose interfaces touch nearly every other system in the plant, and mapping its ingest paths forces an early inventory of the whole architecture. W2 stays with record-keeping systems and splits them by adversary motive, using PPC as the operational record an external actor would want to read or falsify and PSI as the access record an insider would want to control. W3 drops to the protocols that generate those records, with DCS as the busy path and EDGC as the rarely exercised one, so I am examining authentication gaps in traffic I have so far only seen after it was parsed and indexed. W4 and W5 stay on RPS long enough to see one system from two directions, first as digital I&C engineering and then as a regulatory object, with SFPCM added to put physical protection assumptions next to it. W6 through W8 move outward to vendor trust, incident conditions, and emerging platforms, and W9 returns to a monitoring platform so the portfolio closes on the ICT and OT boundary it opened on.

The comfort zone ends at W4. RPS is deterministic, safety-qualified logic with one-way communication and no patch window, and my instinct to solve integrity problems with logging, replication, and after-the-fact reconciliation does not transfer to a system where the control either holds in real time or does not. W5 is the harder of the two multi-week blocks, because SFPCM and the RPS regulatory analysis both require holding Safety, Security, and Safeguards as three separate authorities with three separate definitions of resolved, and because the physical protection assumptions underneath the cyber controls are ones I have never had to reason about. W8 is the other hard one, since side-channel analysis is a hardware and physics argument rather than a data argument, and I expect to spend most of that workshop verifying claims I cannot evaluate from experience. W6 and W7 should be the strongest weeks, because consequence-driven monitoring design and incident triage under untrusted data are closest to what I already do.

## Revision Log
| Date | Change | Reason |
|---|---|---|
| 2026-09-03 | Initial plan | — |
| 2026-09-03 | Reviewed against curriculum.md and workshop themes; no system assignments changed | All 11 systems are covered and no workshop is empty, so there is no requirement issue. Weaker placements are recorded in Notes and Open Items rather than resolved by substitution. |
