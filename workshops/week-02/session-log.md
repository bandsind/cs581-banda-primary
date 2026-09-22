# Session Log — Workshop 2

## Sindi Banda | Claude Code | 9/19/2026 to 9/21/2026

**Workshop:** W2, Adversary Profile and Threat Cards

**W1 system:** OT-SIEM (OT Security Information and Event Management)

**W2 systems:** PPC (Plant Process Computer / Historian) and PSI (Physical Security Integration)

**Deep-profile actor:** Russian FSB, Center 16 (Military Unit 71330)

Prompt numbers (P1 to P12) point to the verbatim prompts in the appendix at the end of this log.

---

## Session 1 — 9/19/2026 | "Learning PPC, PSI, and OT-SIEM before profiling anything"

**Duration:** [fill in]

**Outcome:** A beginner explanation of PPC, PSI, and OT-SIEM, with each claim labeled as documented in the course diagram or inferred from general OT practice.

**Prompt (P1, 4:05 PM):** I asked for a 10-part explanation, ending with a diagram and five self-test questions. I told the tool not to invent connections and not to invent a pivot path.

**What I got:** The explanation was useful because it separated diagram facts from general knowledge. Documented: PPC receives OPC-UA historian data from DCS and sends "Historian events" to OT-SIEM. PSI sends "Security events" to OT-SIEM. Both arrows point into OT-SIEM, and the diagram shows no line from OT-SIEM back to either system. The tool also reused a correction I made in W1: the "Access interlocks" line runs from PSI to DCS, not from PPC. That gave me the main difference between my two systems. PSI has a line into the control layer. PPC does not.

**What I did next:** I used the "no drawn return path" point as the basis for my `pivot_path` answer instead of guessing a route.

---

## Session 2 — 9/19/2026 | "FSB profile: first build, then refactor to the schema"

**Duration:** [fill in]

**Outcome:** `adversary-profile.json` with 10 TTPs, confidence and provenance on each claim, 11 gaps, and `system_relevance` left empty for me.

**Prompt (P2, 7:13 PM):** I asked for an FSB profile from public sources only. I warned the tool in advance that the ICS matrix uses both T0NNN and T1NNN.NNN numbering, so it had to look up each ID instead of guessing the matrix from the prefix.

**What I got:** That warning mattered. The tool confirmed that native ICS techniques T1691 to T1695 exist, that T0859 and T0865 are still current, and that T0857 (System Firmware), which CISA AA22-083A still cites, now sits under T1693.001. It also caught a scoping trap I had not seen: AA22-083A covers two different Russian entities. Appendix A Table 4 (TRITON) belongs to TsNIIKhM, a Ministry of Defense research institute, not the FSB. The tool left those techniques out and said why. It also kept Sandworm (GRU) out. Only one Enterprise ID was used, T1187 Forced Authentication, because no ICS technique covers stealing credential hashes.

**Pushback (P3, 7:30 PM):** The first output did not match the assignment schema. I told it: "Do not redo the research, instead, refactor it to match the required schema." It remapped the same content without new searches. It noticed the schema uses two different confidence lists (top level allows `assessed`, TTP entries allow `inferred`), so three TTPs where the ICS mapping was its own (T0859, T0811, T0872) became `inferred`.

**What I did next:** Kept `system_relevance` empty so I could write those entries myself against my own map.

---

## Session 3 — 9/19/2026 | "First check: which W1 map row does each TTP reach?"

**Duration:** [fill in]

**Outcome:** A row-by-row list of where each technique lands on my W1 OT map, and where it does not.

**Prompt (P4, 7:44 PM):** I asked the tool to cite rows by component name, to say plainly when a technique reaches nothing, and not to rewrite the profile.

**What I got:** Two techniques, T0865 (Spearphishing Attachment) and T0817 (Drive-by Compromise), reach no row on my OT map, because the map has no row for a user mailbox or browser. The tool also warned that two versions of my OT map existed: an 11-row draft and a 14-row corrected version with the RMS, PSI, and PPC feed rows added. It mapped against the 14-row version and told me to check which one was in my repo. That warning turned out to matter in Session 7.

---

## Session 4 — 9/19/2026 | "Card artwork attempt that could not run"

**Duration:** [fill in]

**Outcome:** No card produced. The tool stopped instead of guessing.

**Prompt (P5, 8:07 PM):** I asked for one SVG threat card built from `cards.json`, "entry [n] only."

**What went wrong:** Two problems, both mine. `cards.json` did not exist yet, and my repo was not connected, so the tool could not find it. I also left the `[n]` placeholder unfilled. The tool refused to invent the card fields, which was correct given my own rule "do not invent facts not present in the selected entry." It offered to build a card from the FSB profile instead, but warned that it would not match `cards.json` if the card is graded against that file.

**What I learned:** Build the data file first, then the artwork. Fill in placeholders before sending a prompt.

---

## Session 5 — 9/20/2026 | "Threat card shortlist, 2 per actor type"

**Duration:** [fill in]

**Outcome:** A 10-row candidate table and a recommended five spanning all five actor types.

**Prompt (P6, 7:41 PM):** I asked for 2 documented examples per actor type, excluding FSB, with MITRE ATT&CK for ICS as the first source and no Wikipedia or blogs.

**What I got:** A compact table with an uncertainty note on each row. The tool also excluded Dragonfly (G0035), because MITRE lists Berserk Bear and Energetic Bear as its aliases, which overlaps with my FSB actor. It avoided ALLANITE for the same reason. Volt Typhoon came back with "none found" for an ICS ID, because MITRE maps only Enterprise techniques to that group.

**What the tool got wrong:** For Sandworm, the uncertainty note said the 2022 Industroyer2 attempt "was disrupted," which made 2022 sound like a failed year. That is incomplete. The April 2022 Industroyer2 deployment was disrupted, but the separate 2022 attack MITRE tracks as campaign C0034 did cause a power outage. This was corrected when the card was built.

---

## Session 6 — 9/21/2026 | "Building cards.json"

**Duration:** [fill in]

**Outcome:** `threat-cards/cards.json` with my five selections, unchanged, in the exact schema.

**Prompt (P7, 6:32 AM):** I named the final five and told the tool to stop and ask before any substitution. I set rules for each card: keep Sandworm separate from FSB, do not name the Maroochy individual, keep Browns Ferry as an accident, do not claim DarkSide compromised Colonial's OT network, and keep CyberAv3ngers as "hacktivist" with the Iranian link in `assessed_affiliation`.

**What I got:** No substitutions. Two cards had no obvious ICS technique:

- DarkSide / Colonial Pipeline uses T0828 (Loss of Productivity and Revenue). This is MITRE's own mapping, because the T0828 page names Colonial Pipeline. The card says the OT network was not shown to be compromised.

- Browns Ferry uses T0814 (Denial of Service) only because the failure worked the same way: a device overwhelmed by network traffic. The card says no adversary was involved and that the mapping is the tool's inference, not MITRE's or the NRC's.

The tool also added `assessed_affiliation` to the Sandworm card to state that GRU Unit 74455 is not the FSB. It flagged that "assessed" understates Sandworm's attribution, which comes from a US indictment. CyberAv3ngers' alias uses MITRE's spelling, "Soldiers of Soloman."

**What I would check:** Whether showing T0814 on an accident card is better than "none found." A MITRE ID on an accident card can make readers think ATT&CK covers accidents.

---

## Session 7 — 9/21/2026 | "Validating my own system_relevance entries"

**Duration:** [fill in]

**Outcome:** Clear list of what to fix in my `system_relevance` entries. The profile itself was not changed by the tool.

**Prompts (P8, 5:20 PM and 5:26 PM; P9, 6:34 PM and 7:04 PM):** After I filled in the ten `system_relevance` fields, I asked for a strict review: "I want you to challenge what I added, not just agree with it." The first response at 5:20 PM was cut off partway, so I resent the prompt at 5:26 PM. I then revised my entries several times and resent the file for checking.

**What I got:**

- **Wrong map.** Five of my ten entries (T0862, T0865, T0817, T1187, T0859) cited rows that are on my IT map, while `w1_map_reference` points only to the OT map.

- **Wrong protocol.** Havex, the FSB malware in my profile, used OPC Classic (DCOM-based), not OPC-UA. My OT map labels the PPC and RMS feeds as OPC-UA, so my T0846, T0888, and T0861 entries claimed a protocol match that does not exist.

- **Stale row names.** My revised entries were written against the old 11-row draft. Three entries quoted "OT-SIEM central correlation and management server at Level 3," but my repo map drops "at Level 3." One entry said the map has no PPC interface, but the corrected map has a "PPC to OT-SIEM historian event feed" row.

**Pushback and correction:** The tool first had to guess which map version was in my repo. I linked the repo folder at 6:54 PM and again at 7:06 PM ("Here is the repo."), and the tool read the real file. My repo has the corrected 14-row map, identical to the W1 correction pass. After that, the review worked from the actual file instead of a copy. The tool also noticed that some "row not found" results came from commas and periods inside my quotation marks, not wrong names, and rechecked before reporting.

**What I did next:** [fill in which fixes you applied]

---

## Session 8 — 9/21/2026 | "Reviewing my Week 2 role synthesis"

**Duration:** [fill in]

**Outcome:** A requirement-by-requirement review of my synthesis draft. The tool did not edit my text.

**Prompt (P10, 8:05 PM):** I uploaded my Word draft and told the tool not to edit or rewrite anything, only to point out what was missing, weak, or unclear.

**Pushback:** The tool tried to download Anderson's Chapter 3 to check my citations. I declined that fetch and uploaded the synthesis template instead. So the review checked that my Anderson concepts were named and tied to behavior, but it did not verify them against the book.

**What I got:**

- My draft had no section headings, so it did not match the template's four sections (Role A, Role B, Divergence Analysis, Synthesis).

- Role A was missing the role's authority and accountability.

- Role B did not answer the constraints question and never mentioned 10 CFR 73.56.

- Divergence Analysis listed the questions each role asks, not what each role would do.

- Synthesis did not answer any of the three template questions.

- The template in my repo has no Anderson Grounding heading, so I need to check the rubric to see if it wants a separate section.

- Grammar: a missing period, inconsistent spelling, and acronyms (TTP, OPC, PPC, PSI, MFA) not written out. The draft was about 645 words against a one-page limit.

**What I did next:** [fill in]

---

## Session 9 — 9/21/2026 | "Side questions: automation and accountability"

**Duration:** [fill in]

**Outcome:** Background for the Divergence and Synthesis sections, and for Module 9.

**Prompts (P11, 9:40 PM; P12, 9:43 PM and 9:44 PM):** Which plant systems are automated, and who is accountable when automation fails.

**What I got:** The tool sorted the course systems into automatic protection (RPS, EDGC), automatic control with operators supervising (DCS, TCS), and monitoring (PPC, PSI, OT-SIEM). On accountability: the licensee stays responsible even when a contractor did the work (10 CFR 50 Appendix B, Section I). Vendors must report defects (10 CFR Part 21). Individuals are liable only for deliberate misconduct (10 CFR 50.5). The three cases were TMI-2 (1979), Browns Ferry (2006), and Davis-Besse (2002).

**Pushback:** I declined the tool's fetch of 10 CFR 50.73, so it stopped and asked how to continue. Its first answer listed Appendix B as unconfirmed because the eCFR site returned rate-limit errors. A retry succeeded before the final answer. 10 CFR 50.73 is still unverified, and I have not used it as a source.

---

## Session 10 — 9/21/2026 | "Threat Card Image Generation"

**Duration:** [fill in]

**Outcome:** Five AI-generated visual threat cards based on the completed cards.json content.

**My Prompt:** I used an AI image generation tool to create separate visual cards for Sandworm Team, CyberAv3ngers, the Maroochy Shire former SCADA contractor, the Browns Ferry Unit 3 incident, and DarkSide. I required the cards to avoid real-person likenesses and to show the actor name, actor type, signature TTP, MITRE ID, and provenance.

**What I got:** The tool generated separate card images for each actor. The visuals were useful for presentation, but I understood that generated images can make incorrect or incomplete claims look more authoritative than they really are.

**What I did next:** I reviewed each generated card against cards.json and the assignment instructions instead of assuming the text on the image was correct.

## Session 11 — 9/21/2026 | "Final Threat Card Validation"

**Duration:** [fill in]

**Outcome:** A final check of both the card data and the generated card images against the assignment requirements.

**My Prompt:** "Let's double check if the cards were done according to the instructions."

**What the review found:** CyberAv3ngers needed its hacktivist actor type kept separate from its assessed Iranian affiliation. DarkSide had an incident listed as an alias and needed correction. Browns Ferry needed clearer wording because T0814 was being used as a technical analogy, not as evidence that an attacker performed a denial-of-service attack.

**What I did next:** I corrected those issues in the card data and made sure the final generated cards did not present inferred or accidental behavior as if it were directly attributed adversary activity.

## Session 12 — 9/21/2026 | "Modifying and Rechecking the Adversary Profile"

**Duration:** [fill in]

**Outcome:** A corrected adversary profile that matched my Week 1 OT attack-surface map more closely.

**My Prompt:** After modifying my adversary-profile.json, I asked the AI to review the changes without rewriting the file. I specifically told it to challenge my system_relevance, confidence levels, MITRE mappings, pivot path, and source support.

**What the review found:** Some of my earlier entries used interfaces from the wrong Week 1 map or made stronger connections than the architecture supported. The review also helped separate the confirmed PPC to OT-SIEM historian event feed and PSI to OT-SIEM security event feed from interfaces that were only potential or inferred.

**What I did next:** I modified the adversary profile myself, corrected the affected system_relevance entries, made the pivot-path wording more conservative, and kept documented, inferred, and theoretical claims separate. I then used the AI again to check the modified version for consistency rather than having it rewrite the profile.

## Reflection

**What worked:**

- Warning the tool about the T0NNN / T1NNN.NNN numbering before it started. It looked up every ID and found that T0857 had moved under T1693.001.

- Telling it to refactor instead of redo. The research stayed the same and only the shape changed.

- Writing hard rules for each card up front (Maroochy by role only, Browns Ferry as an accident, no OT compromise claim for Colonial). The tool followed all of them and explained the two ID choices it had to make.

- Asking for a strict review ("challenge what I added") instead of asking it to fill the fields for me. It found errors I would have submitted.

- Linking the repo so the tool checked the real files instead of copies.

**What did not work:**

- Sending the artwork prompt before `cards.json` existed and with `[n]` unfilled.

- Writing `system_relevance` against the wrong map. Half my entries used IT-map rows, and my revisions used row names from an outdated draft.

- Assuming Havex's OPC traffic matched my map's OPC-UA label.

- The shortlist's "Industroyer2 was disrupted" note, which left out the separate 2022 attack that succeeded.

- One response was cut off partway (5:20 PM), and I had to resend.

**What I would do differently:**

- Link the repo at the start of the workshop so every check uses the real files.

- Decide which W1 map the profile references before writing any `system_relevance` entry, and copy row names directly from that file.

- Build the data file before asking for artwork.

- Read AA22-083A myself before prompting, so I would have caught the TsNIIKhM split on my own.

**Time spent:**

- Reading:

- AI sessions:

- Writing and editing:

- Total:

---

## Reflection — The Card Exercise

<!--

Required for W2. A few paragraphs at the end of the log.

You just asked a generative tool to produce confident, attractive artifacts about actors whose

attribution is contested, and in some cases disputed by the people accused.

Ground this in Anderson's Psychology and Usability chapter: 3rd ed. Ch. 3, 2nd ed. Ch. 2.

The chapter is about cognitive exploitation, authority compliance,

and why people believe things that are presented well. Your cards are a specimen of exactly that:

a polished card with a stat line and a MITRE ID reads as more authoritative than the same claim in

a sentence, even when the evidence behind it has not changed.

Address at least:

- What did the card format add that the underlying source did not support?

- Which concepts from that chapter explain why a well-made card persuades past its evidence?

- What would you want a reader of your cards to know that the cards themselves do not say?

Module 9 returns to this question about AI in nuclear operations. Write it honestly rather than tidily.

-->

[Write this section in your own words. Evidence from this workshop you can use, delete these notes when done:]

**What the card format added that the source did not support:**

- Browns Ferry shows T0814 as a "signature TTP," but no MITRE or NRC source maps that incident to ATT&CK. It is an inference, and on the card it looks the same as MITRE's own mappings.

- DarkSide's T0828 is MITRE's mapping, but a card showing a MITRE ICS ID can suggest an OT compromise that the sources do not support.

- Sandworm's field is called `assessed_affiliation`, which undersells an attribution backed by a US indictment. CyberAv3ngers' same field carries a government assessment the group itself does not accept. The card shows both the same way.

- The Maroochy card shows a role, not a person, so it looks like a "threat group" card even though it is one insider case from 2000.

- The shortlist first said the 2022 Sandworm attempt was disrupted. A card built from that sentence would have looked just as confident.

**Anderson concepts to check in the chapter before citing:** authority compliance is named in the assignment prompt itself. Confirm section titles and page numbers in your edition before naming any other concept.

**What the cards do not say:** every card has a `what_this_card_cannot_tell_you` field, but it only appears in `cards.json`. Decide whether the PNG versions carry any of that warning.

---

## Appendix: Prompts (verbatim)

### P1. Sat 9/19/2026, 4:05 PM

```

Course: CS 581 Nuclear Cybersecurity, Boise State University, Fall 2026.

I am working on Workshop 2. My Week 1 system was OT Security Information & Event Management (OT-SIEM). My Week 2 systems are:

- PPC, Plant Process Computer / Historian

- PSI, Physical Security Integration

Before I continue with the adversary profile, I need to understand these three systems and how they relate to each other.

Use the CS 581 course materials, especially the plant architecture diagram and my Week 1 OT-SIEM attack-surface map. Do not assume connections that are not shown in the course architecture. Clearly separate what is documented in the course diagram from what is inferred from general OT architecture.

Explain this to me as a beginner, using plain English and concrete examples.

Structure the explanation like this:

1. PPC

- What PPC is

- What its main job is in a nuclear plant

- What type of data it receives

- What type of data it stores or provides

- Whether it controls equipment or mainly records/monitors information

- Why an attacker might care about PPC

- Give a simple real-world analogy

2. PSI

- What PSI is

- What its main job is

- What kinds of physical-security systems or information it may interact with

- Why credentials, badge/access information, alarms, or authorized access matter

- Why an attacker might care about PSI

- Give a simple real-world analogy

3. OT-SIEM

- Briefly remind me what OT-SIEM does

- Explain why it is primarily a monitoring/security system rather than a process-control system

4. PPC and OT-SIEM

- Explain exactly what connection the CS 581 plant architecture shows between PPC and OT-SIEM

- State the direction of the information flow

- Explain what “Historian events” means in plain English

- Give a concrete example of an event PPC could provide to OT-SIEM

- Explain whether the architecture shows OT-SIEM controlling PPC

- Clearly identify anything that is inferred rather than directly shown

5. PSI and OT-SIEM

- Explain exactly what connection the CS 581 plant architecture shows between PSI and OT-SIEM

- State the direction of information flow

- Explain what “Security events” means in plain English

- Give a concrete example of a security event PSI could provide to OT-SIEM

- Explain whether the architecture shows OT-SIEM controlling PSI

- Clearly identify anything that is inferred rather than directly shown

6. Compare PPC and PSI

Make a simple table with:

- Main purpose

- Type of information

- What an attacker may want

- Connection to OT-SIEM

- Main security concern

7. Connect this to Workshop 2

Explain why PPC and PSI make an interesting pair for adversary profiling.

In particular explain:

- PPC as a records/data-integrity target

- PSI as an access/security-information target

- why a nation-state actor's documented TTPs might be relevant to one system but not necessarily the other

8. Pivot path

Explain what the Workshop 2 field "pivot_path" is asking me to determine between:

W1 OT-SIEM

and

W2 PPC + PSI.

Do NOT invent a pivot path. Use the course architecture. If the diagram only shows information flowing into OT-SIEM, explain what that means for whether an attacker could move in the reverse direction.

9. Finish with a simple diagram like:

PPC --------> OT-SIEM

PSI --------> OT-SIEM

              |

             v

         monitoring / alerts

Adjust the arrows to match the actual CS 581 architecture.

10. End with five questions to test whether I actually understand PPC, PSI, OT-SIEM, and their connections.

Writing standard:

- Plain English

- Explain acronyms

- Short paragraphs

- No unnecessary jargon

- No em dashes

- Do not make up plant-specific implementations

- Label claims as documented, inferred, or theoretical when appropriate

- Correct me if I misunderstand the architecture

```

### P2. Sat 9/19/2026, 7:13 PM

```

I am building a structured threat intelligence profile for CS 581, a nuclear cybersecurity course.

Target system: PPC + PSI

My W1 attack surface map: workshops/week-01/attack-surface-map-ot.md

Using ONLY publicly available information and named open-source reporting, produce a JSON

adversary profile for Russian Federal Security Service (FSB) as it relates to the energy and nuclear sector.

Requirements:

- Map 6 to 10 TTPs to MITRE ATT&CK for ICS technique IDs.

The ICS matrix uses both T0NNN and newer T1NNN.NNN numbering, so do not infer the

matrix from the prefix. Look each technique up. Use an Enterprise ID only where no

ICS technique applies, and say so in that entry.

- Every claim carries a confidence value: documented, assessed, or theoretical.

- Every claim carries a provenance string naming a specific report plus section or page.

- Populate gaps_in_public_knowledge with attribution claims that exist but cannot be

verified from open sources. Do not leave it empty.

- Do not speculate past what the sources support. If you are inferring, label it inferred.

Output valid JSON only, matching the schema in the assignment. Leave system_relevance

as an empty string for now; I will fill those in myself against my own map.

```

### P3. Sat 9/19/2026, 7:30 PM

```

Do not redo the research, instead, refactor it to match the required schema :{

"_comment": "W2 deliverable. Replace every ... with your own content. Delete this _comment key before committing. File MUST parse as valid JSON.",

"target_system": ["<from your sequencing plan; array so a pair can name both>"],

"w1_map_reference": "workshops/week-01/attack-surface-map-<it|ot|physical>.md",

"pivot_path": "How would this actor move from your W1 system to your W2 system? Name the interface. If there is no path, say so and say why.",

"actor": "...",

"also_known_as": ["..."],

"attributed_to": "...",

"confidence": "documented | assessed | theoretical",

"provenance": "specific public report, with section or page",

"motivation": {

"primary": "...",

"secondary": ["..."]

},

"actor_type": "nation-state | criminal | hacktivist | insider | accidental",

"nuclear_targeting_evidence": "...",

"primary_ttps": [

{

  "tactic": "...",

  "technique": "...",

  "mitre\_ics\_id": "TNNNN or TNNNN.NNN",

  "nuclear\_relevance": "...",

  "system\_relevance": "which interface on YOUR W1 map this technique would reach",

  "confidence": "documented | inferred | theoretical"

}

],

"historical_precedent": ["..."],

"detection_opportunities": ["..."],

"gaps_in_public_knowledge": ["..."]

}

```

### P4. Sat 9/19/2026, 7:44 PM

```

I am filling in the system_relevance field on each TTP in my adversary profile.

Read my W1 attack surface map at workshops/week-01/attack-surface-map-ot.md.

For each TTP in adversary-profile.json, tell me which specific row or interface in that

map the technique would actually reach. Cite the row by its component name.

If a technique has no reachable interface on my map, say so plainly rather than

inventing a connection. I want to know where the gaps are.

Do not rewrite the rest of the profile.

```

### P5. Sat 9/19/2026, 8:07 PM

```

Open `workshops/week-02/threat-cards/cards.json` and use entry [n] only.

Create one threat actor card as a single self-contained SVG file.

Style:

- anime / cyber poster-card

- bold, high-contrast, futuristic, dramatic

- strong graphic shapes, glowing panels, HUD-style framing, cyber motifs

- no real person, no face, no portrait, no likeness

- use an original mascot, emblem, abstract machine-creature, geometric symbol, reactor-core icon, digital beast, or other non-human visual element

Output requirements:

- Output only the final SVG code

- Make it a complete standalone `<svg>...</svg>` document

- Dimensions should be about 750 x 1050

- Readable at half size

- Self-contained only: no external assets, no linked images, no web fonts, no raster images

Hard constraints:

- Do not invent facts not present in the selected cards.json entry

- If a detail is not in the entry, leave it out

- All text must be real SVG `<text>` elements, not paths

- Use only SVG shapes, gradients, strokes, patterns, filters, and text

Extract only these values from the selected cards.json entry:

- actor

- actor_type

- signature_ttp.technique

- signature_ttp.mitre_ics_id

- provenance

The card must clearly and legibly show:

1. actor name

2. actor type

3. signature TTP with its MITRE ATT&CK for ICS ID

4. a provenance line naming the public source

Layout guidance:

- Top: large dramatic actor name

- Near top: actor type as a badge, chip, or HUD label

- Center: a large anime-style cyber emblem or mascot, non-human, tied to the actor’s theme

- Mid-lower section: a highlighted feature box for the signature TTP

- Lower section: smaller info panel for MITRE ATT&CK for ICS ID

- Bottom: provenance line in smaller but readable text

- Make the composition feel like a collectible cyber threat card or futuristic poster

Visual direction:

- use a layered neon-tech look

- angular panels, circuit-like accents, warning stripes, grid overlays, light flares, or reactor/cyber motifs

- prioritize readability over decoration

- keep typography clean and sharp

- make the actor name and TTP the most visually prominent text after the main artwork

Validation:

- Ensure the SVG is valid XML

- Ensure every visible word is selectable/searchable text

- Return only the SVG and nothing else

```

### P6. Sun 9/20/2026, 7:41 PM

```

Search the public web and build me a SHORT candidate list for CS 581 Workshop 2 threat cards.

I need 2 documented examples for EACH actor type below:

- nation-state

- criminal

- hacktivist

- insider

- accidental

That means 10 total candidates.

Important:

- Do NOT include the Russian FSB / Center 16 because I already used that actor in adversary-profile.json.

- Prefer actors or incidents relevant to ICS, OT, critical infrastructure, energy, nuclear, water, transportation, manufacturing, or physical security systems.

- Start with MITRE ATT&CK for ICS when possible:

https://attack.mitre.org/techniques/ics/

https://attack.mitre.org/groups/

- You may also use authoritative public sources such as:

- CISA

- FBI

- DOJ

- NSA

- DOE

- NIST

- NRC

- other national cybersecurity agencies

- reputable vendor research only when government/MITRE sources are insufficient

Do not use random blogs, Wikipedia, SEO sites, or unsourced summaries as primary evidence.

For each candidate, give me ONLY:

1. Actor or incident name

2. actor_type

3. aliases, if any

4. one-sentence description of why it is relevant to ICS/critical infrastructure

5. one possible signature TTP

6. MITRE ATT&CK for ICS ID, if one clearly applies

7. primary public source

8. confidence: documented | assessed | theoretical

9. one short note about what is uncertain or not proven

If there is no valid ATT&CK for ICS equivalent for the signature TTP:

- set the ICS ID to "none found"

- give the Enterprise ATT&CK ID if applicable

- briefly explain why no ICS technique applies

Special instructions by type:

NATION-STATE:

Find named government-linked cyber groups with documented ICS or critical-infrastructure activity.

CRIMINAL:

Find ransomware or financially motivated actors with documented critical-infrastructure or OT impact.

HACKTIVIST:

Find groups that publicly present themselves as hacktivists and have documented ICS/critical-infrastructure activity.

If government sources assess a state affiliation, keep actor_type as hacktivist and list the state relationship separately.

INSIDER:

Find documented insider cases involving ICS, OT, utilities, industrial facilities, or physical-security systems.

The actor may be described by role, such as former contractor or employee, if naming the real person is unnecessary.

ACCIDENTAL:

Find documented accidental or unintentional human actions that affected ICS/OT or critical infrastructure, such as operator error, contractor mistakes, misconfiguration, accidental shutdown, or unintended network impact.

Do not invent an adversary group where none exists. Describe the actor by role if necessary.

Output as a compact table with 10 rows.

After the table, give me:

- your 5 strongest candidates for the final threat cards

- make sure those 5 span at least 3 actor_type values

- one sentence explaining why each is a strong choice

Do NOT build cards.json yet.

Do NOT generate artwork.

Do NOT write long biographies.

This is only the initial research shortlist. We will verify and expand each selected candidate later.

```

### P7. Mon 9/21/2026, 6:32 AM

```

Create my Workshop 2 threat-cards/cards.json using these five selections: 1. Sandworm Team → nation-state 2. CyberAv3ngers → hacktivist 3. Maroochy Shire former SCADA contractor → insider 4. Browns Ferry Unit 3 broadcast-storm incident → accidental 5. DarkSide / Colonial Pipeline → criminal These are the final five. Do not replace them unless you find a serious factual problem, and if you do, stop and tell me before making a substitution. Research and verify each entry using authoritative public sources. Source priority: 1. MITRE ATT&CK for ICS 2. CISA 3. FBI 4. DOJ 5. DOE 6. NRC 7. Idaho National Laboratory / CyOTE 8. Other authoritative government or national-lab sources only when needed Do not use Wikipedia, random blogs, SEO summaries, or unsourced articles as the primary source. IMPORTANT: - My deep-profile actor is the Russian FSB / Center 16, so do not reintroduce FSB as one of these five. - Sandworm is a separate GRU-linked actor and must not be conflated with FSB. - Do not depict or identify a real individual for the Maroochy case beyond what is necessary for the assignment. Refer to the actor by role, such as "Maroochy Shire former SCADA contractor." - Browns Ferry is an accidental incident, not a malicious threat group. Preserve that distinction. - DarkSide caused major operational consequences at Colonial Pipeline, but public reporting did not establish compromise of the OT network. Do not claim otherwise. - CyberAv3ngers publicly presents as a hacktivist actor, while government reporting assesses an Iranian state affiliation. Keep actor_type as "hacktivist" and use assessed_affiliation for the government-assessed relationship. Use this EXACT JSON structure: [ { "actor": "...", "aka": ["..."], "actor_type": "nation-state | criminal | hacktivist | insider | accidental", "assessed_affiliation": "OPTIONAL. Include only where needed.", "sector_targeting": ["..."], "signature_ttp": { "technique": "...", "mitre_ics_id": "TNNNN or TNNNN.NNN" }, "first_observed": "...", "confidence": "documented | assessed | theoretical", "provenance": "named public source", "what_this_card_cannot_tell_you": "..." } ]

```

### P8. Mon 9/21/2026, 5:20 PM (resent 5:26 PM)

```

Validate the new information I added to fill the previously missing sections. Do not make any changes to the file yet. Your job is to critically review what I added and challenge it against authoritative public sources. For each new or newly completed field: 1. Check whether the claim is supported by the cited source. 2. Check whether the source actually says what I claim it says. 3. Identify any overstatement, unsupported inference, ambiguity, or attribution problem. 4. Check whether the confidence level is appropriate. 5. Check whether any MITRE ATT&CK for ICS technique or ID is correctly mapped. 6. Check whether I am confusing: - documented evidence - inferred analysis - theoretical possibility 7. Check whether any actor, alias, campaign, malware family, or incident has been incorrectly merged with another. 8. Check whether the information is relevant to my assigned systems and Week 1 architecture where applicable. 9. Prefer authoritative sources such as: - MITRE ATT&CK for ICS - CISA - FBI - DOJ - DOE - NRC - NIST - Idaho National Laboratory - other official government or national-lab sources Do not rewrite the JSON. Do not silently fix anything. Return only review comments and suggested corrections. For every issue you find, use this format: Field: Current claim: Problem: Source evidence: Suggested correction: Confidence in your review: Also include a final section: VALID AS WRITTEN - List the newly added claims that are well supported and do not need changes. NEEDS REVIEW - List the claims that are questionable, overstated, weakly sourced, or need better wording. MISSING EVIDENCE - List anything that still lacks enough public evidence. Be strict. I want you to challenge what I added, not just agree with i

```

### P9. Mon 9/21/2026, 6:34 PM (resent 7:04 PM after revisions)

```

I am filling in the system_relevance field on each TTP in my adversary profile.

Read my W1 attack surface map at workshops/week-01/attack-surface-map-ot.md.

For each TTP in adversary-profile.json, tell me which specific row or interface in that

map the technique would actually reach. Cite the row by its component name.

If a technique has no reachable interface on my map, say so plainly rather than

inventing a connection. I want to know where the gaps are.

Do not rewrite the rest of the profile.

```

### P10. Mon 9/21/2026, 8:05 PM

```

Review my uploaded Word document against the exact Module 2 synthesis template below, but do not edit, rewrite, or replace any of my text. Only give specific suggestions on what is missing, weak, unclear, repetitive, unsupported, or out of place, and point out grammar, punctuation, sentence-structure, and organization problems I should fix myself. Also check that I meet every requirement, especially Role A Perspective, Role B Perspective, Divergence Analysis, Synthesis, and Anderson Grounding with at least two specifically named Chapter 3 concepts tied to observable insider-risk behavior. Avoid em dashes, overly complex wording, unnecessary jargon, and vague comments.

```

### P11. Mon 9/21/2026, 9:40 PM

```

Which system are automated in Nuclear Industry?

```

### P12. Mon 9/21/2026, 9:44 PM

```

Who is accountable when an automation fails? please provide sources and 3  cases

```

### What worked:
Validation helped me go through my work carefully and catch mistakes before submitting. Asking the AI to challenge my work instead of just agreeing with it was especially useful.

### What did not work:
Some of my early prompts were not specific enough, so the answers were less useful or missed important details. I also spent time working without realizing the Workshop 2 guide already explained many of the requirements.

### What I would do differently:
If I started this workshop over, I would definitely use the workshop guide from the beginning. I did not know each workshop would have one. I also need to improve my prompts and use AI more effectively because good prompting plus verification tends to produce better work. I spent a lot of time reading and checking sources in this workshop, but that was also a good learning experience, and I am confident I can do better in the next workshops.


**Time spent:**
- Reading: 10
- AI sessions: 5
- Writing and editing: 3
- Total: 18