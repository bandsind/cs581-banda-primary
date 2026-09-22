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

Two or three paragraphs. Reconstruct the incident from the talk in your own words: what the agent was supposed to be doing, what it actually did, how the boundary was crossed, and how it was discovered.

Be specific about the sequence. If the speakers are uncertain about a step, or if they say something is reconstruction rather than observation, carry that uncertainty into your write-up rather than resolving it.

---

## Where the Adversary Taxonomy Breaks

This is the section to spend real effort on.

Your W2 `actor_type` field has five values: nation-state, criminal, hacktivist, insider, accidental. Place the agent in this incident into one of them.

You will find it does not fit cleanly. Say why. Then answer:

- Is this an adversary at all? It had no intent in any sense the taxonomy assumes.
- Is it an insider? It held legitimate credentials and was operating inside a boundary it was authorized to be in.
- Is it accidental? Its actions were goal-directed and instrumentally sensible, which is not what that category usually means.
- What would you have to add to the taxonomy to hold it, and what would that addition break elsewhere?

A threat model that cannot name an actor cannot defend against it. Note that MITRE ATT&CK has the same problem.

---

## Connection to Your System

One or two paragraphs. Take your W2 system specifically.

If an autonomous agent with legitimate access to some part of your plant's digital environment behaved the way the agent in this talk behaved, what would it reach? Use your own W1 attack surface map and name rows from it, the same way you did for `system_relevance`.

Then the harder half: which of your detection opportunities would fire? Most detection assumes either an intruder without credentials or a human insider with a motive. An authorized non-human process pursuing an objective its operators did not anticipate is neither.

---

## Nuclear Implications

Two or three paragraphs. The talk is about AI infrastructure, not a plant. Do the translation carefully and mark where it stops holding.

Consider at least two of these, and say which you chose:

- **10 CFR 73.54 and RG 5.71 Rev. 1** assume a cyber attack has an attacker. Does an autonomous agent exceeding its boundary constitute a cyber attack under the rule? If a licensee cannot answer that, what does the reporting decision under 10 CFR 73.77 look like?
- **Vendor and supply chain.** Plant vendors are adopting AI-assisted tooling in engineering, diagnostics, and support. What does the boundary of a vendor's agent have to do with your plant's boundary?
- **Access authorization.** 10 CFR 73.56 governs personnel. It has nothing to say about a non-human process holding credentials. Who authorizes an agent, and who revokes it?
- **The safety case.** Nuclear safety analysis rests on bounding worst-case behavior. What does bounding mean for a system whose behavior is not specified in advance?

Where the analogy fails, say so. An AI research lab and a licensed nuclear facility differ in regulation, network architecture, and consequence, and a write-up that ignores those differences is not doing the work.

---

## Knowledge Gap Identified

One paragraph. What did this talk reveal that you do not yet understand well enough? Name it specifically. Then describe how you would use an AI tool to close that gap: what you would ask, what you would verify it against, and how you would know when you understood it.

There is an obvious irony in using an AI assistant to research an incident about an AI assistant exceeding its boundary. Say something about that if it is worth saying.

---

## Question for Charlie

Two or three sentences.

Our live session with Charlie Nickerson of INL is Wednesday Sep 23, 5:00 to 6:30 PM MT. Write the question you would put to a working practitioner about this incident, as you would actually ask it, and explain why the answer would matter for nuclear cybersecurity work.

Bring it to the session.

---

## AI Tool Reflection

Did you use an AI tool to prepare, process what you heard, or follow up? What did you ask, what did it get right, and where did you correct it?

---

<!-- Writing standard: see Practitioner Session Write-up Guide in Module 1.
     Use the LLM Reset and Wikipedia Signs of AI Writing references before drafting.
     These standards apply to this course. Other courses follow their own rules. -->
