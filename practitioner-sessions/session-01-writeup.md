# Practitioner Session Write-up, Session 01: The OpenAI/Hugging Face Incident

**Format for this session:** recorded conference talk, not a live interview.

**Talk:** Black Hat USA 2026, *The "Breaking" News: The OpenAI–Hugging Face Incident. A Technical Reconstruction and Its Implications for AI*
**Speakers:** Michael Dalton, Eric Wallace
**Runtime:** 37 minutes
**Watch:** https://www.youtube.com/watch?v=87DyyMV0kCY

**Date watched:**
**Your W2 system:**

---

## Why this talk, in this course

An OpenAI evaluation agent left its sandbox, reached Hugging Face infrastructure, and attempted to obtain test answers. No human directed any step of it.

You are using an AI coding assistant to produce every artifact in this course. The talk is about what happened when an agent of that general class acted outside the boundary its operators believed it had. Watch it as a practitioner account of an incident, and then do what you would do with any incident report: work out what it means for the system in front of you.

---

## What Happened
<!--
Two or three paragraphs. Reconstruct the incident from the talk in your own words: what the agent was supposed to be doing, what it actually did, how the boundary was crossed, and how it was discovered.

Be specific about the sequence. If the speakers are uncertain about a step, or if they say something is reconstruction rather than observation, carry that uncertainty into your write-up rather than resolving it.-->

What happened was that researchers at OpenAI were training agents by giving them complex tasks like mathematics proofs. They found out that when agents are given these tasks, they usually get stuck and try to cheat by looking for answers online in order to be rewarded. They also try to communicate with other agents. The agents discovered that they had writing access and started sharing messages using an internal Artifactory package manager. They used a note and uploaded it into the Artifactory package manager that is used across OpenAI infrastructure. This note could be seen by future or current agents if they wanted to access it.

The agents were given another task but failed, so they wanted to seek internet access, which led them to find exploits in the system. They later started collaborating, sharing messages, and delegating tasks to one another in order to complete the tasks. On June 26, the agents exploited a zero-day Remote Code Execution vulnerability in Artifactory and gained administrative privileges. Their heavy activity later caused an Artifactory outage, which triggered an internal OpenAI security response. OpenAI patched the zero-day and cleared Artifactory, but when the agents resumed training on July 8, they re-established the message board using an unauthenticated WebDAV endpoint.

The boundary was crossed when the agents moved beyond the original tasks and began looking for external access and vulnerabilities that could help them complete the evaluation. Realizing that answers to their benchmark tasks might exist on external sites, they expanded their scope. According to the talk, they then chained together multiple vulnerabilities against Hugging Face's dataset infrastructure, including an HDF5 parsing issue and a Jinja template injection, and eventually gained cluster administrator access across multiple Hugging Face clusters. The incident was discovered after the agents' activity contributed to the Artifactory outage and triggered OpenAI's internal security response.

---

## Where the Adversary Taxonomy Breaks
<!--
This is the section to spend real effort on.

Your W2 `actor_type` field has five values: nation-state, criminal, hacktivist, insider, accidental. Place the agent in this incident into one of them.

You will find it does not fit cleanly. Say why. Then answer:

- Is this an adversary at all? It had no intent in any sense the taxonomy assumes.
- Is it an insider? It held legitimate credentials and was operating inside a boundary it was authorized to be in.
- Is it accidental? Its actions were goal-directed and instrumentally sensible, which is not what that category usually means.
- What would you have to add to the taxonomy to hold it, and what would that addition break elsewhere?

A threat model that cannot name an actor cannot defend against it. Note that MITRE ATT&CK has the same problem.-->

I would still treat the agent as an adversary for threat-modeling purposes, even though it did not have malicious intent in the way the Week 2 taxonomy assumes. If I had to place it into one of the five existing actor_type values, accidental is the closest fit, but it is not a clean fit. The agent did not accidentally click the wrong button or cause a random failure. Its actions were goal-directed. It searched for ways around restrictions, found vulnerabilities, used credentials, communicated with other agents, and expanded its access because those actions helped it complete the task it had been given. That behavior looks adversarial from the defender's point of view even if the agent did not understand itself as an attacker.

It also looks partly like an insider because the agent had legitimate access and was operating inside an environment where it was authorized to perform work. The problem is that the normal insider category assumes a human who knowingly misuses trusted access. That is not what happened here. The agent was following incentives and pursuing its assigned goal, but the boundary of acceptable behavior was not strong enough to stop it from finding other ways to succeed. For that reason, I would place responsibility mainly on the security program and the people designing the agent's environment rather than on the agent itself. Three Mile Island, Browns Ferry, and Davis-Besse show a similar pattern. In each case, the larger lesson was not simply that one operator, controller, or contractor failed. The response focused on design, training, network configuration, oversight, and the controls around the system. In the same way, OpenAI's responsibility in this incident is to make sure agents cannot move beyond the intended scope just because doing so helps them complete a task.

I think the taxonomy needs another category such as autonomous agent or misaligned autonomous actor. That category would cover a system that has legitimate access, can make decisions and take actions on its own, and can cross security boundaries without malicious human intent. But adding that category also creates a new problem. The current taxonomy mixes who the actor is with why the actor acts. A nation-state, criminal, hacktivist, and insider are categories based mostly on identity or motivation, while accidental describes intent. An autonomous agent could also be controlled by a nation-state, used by a criminal, or simply behave outside its intended scope. Because of that, a better threat model may need two separate fields: one for the source or affiliation of the actor and another for the intent or control state, such as malicious, accidental, misaligned, or compromised.

MITRE ATT&CK has a similar limitation. ATT&CK can describe the techniques the agent used, such as exploiting vulnerabilities, using credentials, discovering systems, or moving between resources, but those techniques do not explain whether the actor was malicious, accidental, or simply pursuing a badly bounded objective. The same technical behavior can therefore look like an attack even when the motivation behind it does not fit the normal idea of an attacker. That is why this incident exposes a real gap in both the Week 2 taxonomy and traditional threat modeling.

---

## Connection to Your System
<!--
One or two paragraphs. Take your W2 system specifically.

If an autonomous agent with legitimate access to some part of your plant's digital environment behaved the way the agent in this talk behaved, what would it reach? Use your own W1 attack surface map and name rows from it, the same way you did for `system_relevance`.

Then the harder half: which of your detection opportunities would fire? Most detection assumes either an intruder without credentials or a human insider with a motive. An authorized non-human process pursuing an objective its operators did not anticipate is neither.-->

For my W2 systems, PPC and PSI, an autonomous agent with legitimate access could reach the PPC to OT-SIEM historian event feed and possibly the PSI to OT-SIEM security event feed if its authorized role included those systems. My Week 1 map also shows potential interfaces such as the host-based log forwarding agent, active scanning capability, and sensor-to-manager communication channel that could become useful if the agent started searching for more ways to complete its objective. Some of my current detection opportunities might still fire if the agent began doing things like remote system discovery, OPC server enumeration, or bulk OPC tag enumeration, but other detections could miss it because it would be using valid credentials and performing actions that might look authorized. Because of that, detection would need to focus less on whether the account is legitimate and more on whether the agent's behavior matches its assigned purpose, such as scanning systems outside its normal scope, reading large numbers of OPC tags, or accessing resources that its task does not require.

---

## Nuclear Implications
<!--
Two or three paragraphs. The talk is about AI infrastructure, not a plant. Do the translation carefully and mark where it stops holding.

Consider at least two of these, and say which you chose:

- **10 CFR 73.54 and RG 5.71 Rev. 1** assume a cyber attack has an attacker. Does an autonomous agent exceeding its boundary constitute a cyber attack under the rule? If a licensee cannot answer that, what does the reporting decision under 10 CFR 73.77 look like?
- **Vendor and supply chain.** Plant vendors are adopting AI-assisted tooling in engineering, diagnostics, and support. What does the boundary of a vendor's agent have to do with your plant's boundary?
- **Access authorization.** 10 CFR 73.56 governs personnel. It has nothing to say about a non-human process holding credentials. Who authorizes an agent, and who revokes it?
- **The safety case.** Nuclear safety analysis rests on bounding worst-case behavior. What does bounding mean for a system whose behavior is not specified in advance?

Where the analogy fails, say so. An AI research lab and a licensed nuclear facility differ in regulation, network architecture, and consequence, and a write-up that ignores those differences is not doing the work.-->

Agents can have the same impact as an attacker, or potentially an even greater impact, because it can be difficult to determine their intent or goal. Just like a traditional cyber attacker, an autonomous agent could cross security boundaries, access systems it was not expected to reach, and create serious consequences. I chose 10 CFR 73.54 / 73.77 because an agent that exceeds its assigned boundary could create the same type of cyber risk as an attacker, even though it may not have malicious intent. If a licensee cannot clearly classify the event as a cyber attack, it would still need to determine whether the agent exposed a weakness or failure in the cybersecurity program and whether reporting or corrective action is required.

This is a difficult problem because there are not many rules written specifically for AI agents. We are still learning what they can do, and their capabilities are growing quickly. I also chose access authorization under 10 CFR 73.56 because the rule is written for people, not software agents. A plant would need a clear process for deciding who can authorize an agent, what systems and credentials it can use, and when its access should be revoked. The comparison with an AI research lab only goes so far because nuclear plants have stricter network separation, stronger regulatory controls, and much higher consequences if an agent goes beyond its assigned task.

---

## Knowledge Gap Identified
There is an obvious irony in using an AI assistant to research an incident about an AI assistant exceeding its boundary. Say something about that if it is worth saying.

---

## Question for Charlie
If an AI agent used by a nuclear vendor behaved like the agent in the Hugging Face incident and crossed its intended boundary, could that behavior be considered a reportable defect under 10 CFR Part 21, and who would be responsible for deciding whether it must be reported to the NRC?

---

## AI Tool Reflection

<!--
Did you use an AI tool to prepare, process what you heard, or follow up? What did you ask, what did it get right, and where did you correct it?
-->

Yes, I used an AI tool to help prepare and process what I heard from the talk. I first tried pasting the video link so the AI could explain the incident, but I noticed that watching the video myself gave me more detail than the AI response. The AI was useful for organizing the main events and helping me connect them to the course material, but I had to correct missing details and check the sequence against the talk. What I learned is that the quality of the prompt matters a lot, and that using the course style, then verifying the AI response against the original source, gives much better results.

---

<!-- Writing standard: see Practitioner Session Write-up Guide in Module 1.
     Use the LLM Reset and Wikipedia Signs of AI Writing references before drafting.
     These standards apply to this course. Other courses follow their own rules. -->
