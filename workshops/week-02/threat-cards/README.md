# Threat Cards — Workshop 2

Five actors beyond the one in `adversary-profile.json`. Stat-line depth, not full depth.

## Spread the types

The five must span **at least three different `actor_type` values**. Five nation-state groups is not a
landscape, it is one type five times.

The course adversary taxonomy is nation-state, insider, hacktivist, criminal, and accidental. Independent
operators and small collectives belong on these cards. They have different motivations, different
resources, different time horizons, and they fail in different ways than a state programme does.

`actor_type` describes **what kind of adversary this is**, not how capable it is. Those are different
questions and one field cannot answer both. If a group's public presentation and its assessed attribution
disagree, put the presentation in `actor_type` and the attribution in `assessed_affiliation`, and say so
on the card. That disagreement is usually the most interesting thing about the actor. A set
that only contains tier-1 actors misses most of what actually reaches a plant.

## What goes here

- `cards.json` — the data behind the five cards. Must parse as valid JSON.
- `card-01` through `card-05` — one file per actor, any extension (`.svg`, `.html`, `.png`, `.jpg`).

## Style

Yours. Baseball card, Pokemon card, tarot, trading card, anime, whatever you find fun.

Every card must legibly show:
- actor name
- actor type
- signature TTP with its MITRE ATT&CK for ICS ID

The ICS matrix uses both `T0NNN` and newer `T1NNN.NNN` sub-technique numbering, so the prefix does not
tell you which matrix an ID belongs to. Look the technique up rather than inferring from the number.

If a card's signature technique genuinely has no ICS equivalent, set `mitre_ics_id` to `null`, put the
Enterprise ID in `mitre_enterprise_id`, and explain in `id_note` why the ICS matrix has nothing for it.
That case is rare and it needs a reason, not just a different field.
- a provenance line naming a public source

## How to make them

Either approach is fine. Pick whichever you will actually enjoy.

**Option A: have your coding assistant emit SVG or HTML.** Costs nothing, keeps the files small and
diffable, works with the tools you already have for this course, and getting an agent to produce clean
SVG is a useful exercise in its own right.

**Option B: use an image generator.** You are welcome to, and several produce very good card art:

- ChatGPT (OpenAI)
- Nano Banana, in Google Gemini
- Microsoft Copilot / Designer
- Adobe Firefly
- Midjourney

Use a provider with clear data handling practices. Consistent with the course policy on AI tools, stick
to US-based providers for this course and do not route coursework through platforms outside that scope.

**Do not buy a subscription for this assignment.** The free tier of any of the above is more than enough,
and Option A costs nothing at all. Nobody gets a better grade for having paid for a tool.

You can also mix the two: generate the artwork as an image, then have your coding assistant lay out the
stat line and provenance text around it in HTML or SVG. That usually produces the most readable card,
since generated images tend to mangle small text.

## One hard rule

**No card may depict a real person.** Original characters, mascots, creatures, emblems, and abstract art
are all fine. Art that presents itself as showing what a real operator looks like is not.

Several actors you will look at are tied to named individuals under federal indictment, including in
CISA advisory AA22-083A on this module's reading list. Inventing faces for them is the one thing this
assignment will not accept.

## Grading

Cards are scored on whether the stat lines are accurate and sourced, not on artistic quality.
A hand-drawn card photographed with your phone scores the same as a polished render if the data is right.
