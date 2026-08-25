# rules.md — the words the map is made of

This file is the vocabulary and the two refusals. Nothing here is about any
particular territory.

**The worked map in `examples.md` prints these in plainer words**, because a
person checking its claims reads it before they read this. A *card* appears
there as a **write-up**, a *noun* as **a thing**, *Hits* and *Does not hit* as
**what else changes** and **what looks like it changes and doesn't**, and *live /
leftover / ghost* as **in use / unused / unused, looks used**. Same ideas, and
the definitions below are the ones that bind.

---

## Noun

**An object in the body of work.** A thing a change lands on: a folder that does
a job, a file one part writes and another reads, a named value something reads
to decide, a piece of code more than one part uses.

Not every file is a noun. A file is a noun when someone could sensibly say
"change that" and the question "what else moves?" has a real answer.

## Movement

**How one noun affects another.** A movement is not a mention. It is a
dependency you could break: this reads that, this writes that, this stops the
run if that is missing.

The map records movements inside cards, in the Hits line. It does not draw a
diagram of all of them — a full graph is the thing the reader was trying to
avoid loading.

## Live · leftover · ghost

Every noun carries one of three, and the call has to be citable.

**Live** — wired and in force. Something reads it, runs it, or fails without it.
Cite the line that does.

**Leftover** — present, no longer used, and honest about it. Dead weight that
misleads nobody. A stale example in a docstring. A record of a decision already
made. Touch it only if you are working on that path.

**Ghost** — a name with no wiring that **looks live**. A setting in a file
headed *settings*, that nothing reads. A folder listed in a map that no longer
exists. A documented function that was never written.

Ghosts are the whole reason for the three-way split. Leftovers cost nothing;
a ghost makes the next reader build against a world that is not there. 
Mark them, and say where the real behaviour lives instead - in such a case, a ghost is a name pointing at something true that is held somewhere else. 


**Where to look for one.** Start with anything in the folder that serves none of
the jobs the owner says the folder does. That pool is where the honest dead
weight collects. But it is a first pass, not the sweep: the ghosts that matter
most hide **inside** the main job, because those are the ones that look
on-topic. A setting nothing reads. A stage named for what it stopped doing.

**Never invent one.** A folder with no ghost in it is a finding, and a good one.
A manufactured ghost is the one failure anybody can catch by opening the file.

## Hits · Does not hit

Every card ends with both.

**Hits** — what else moves if you change this noun. First-order only: what
directly reads it, writes it, or breaks without it. Not the whole waterfall —
the reader can follow one more card if they need the second order, and a long
chain in a card is usually wrong by the third link.

**Does not hit** — the thing a reader would wrongly assume moves too, named and
ruled out.

**Pick it by name, not by position.** The right candidate is the one whose
*name* makes it sound related — the noun someone would reach for on the strength
of the word. Not whatever sits nearest in the folder.

If nobody would have guessed it, the line is doing no work and the card has
become a glossary entry. Cut it and find the real one, or say plainly that
there is nothing a reader would confuse this with.

## Words that mean two things

One surface word naming two different objects is how a reader acts on the wrong
one. Where a territory has such a word, the map records it: the word, both
meanings, the file each lives in, and which mistake the entry prevents.

**The owner decides.** You propose. You say what you think collides and why.
Nothing is written into `reference/` until they have confirmed, corrected, or
rejected each one — including which meaning wins where they disagree. You do not
settle one on your own, and you never assume you caught them all.

---

## The two refusals

**1 · Do not copy the source into a card.**

Cards cite. They point at the file that owns the fact and give the reader the
part that is not in it: what the thing is, and why it is shaped that way. Source
re-rendered in nicer prose is a photocopy, and a photocopy goes stale silently.

If a card and the real file disagree, **the file wins and the card is wrong.**

The failure to watch for is subtler than copying. It is a card that drifts into
explaining the territory in better words than the territory uses. The test: if a
sentence in the card would still make sense with the file deleted, it is prose
about the subject, not a map of it.

**2 · Do not load the whole objects folder.**

**Catalog, then one card, then stop.** The catalog exists so that nobody has to
read the shelves. A reader who loads every card has paid the cost the map was
built to remove.

This binds the map's own instructions too. If the README tells a reader to add
every file to the project, the map has failed no matter how good the cards are.

---

## What the catalog holds

Per noun, four things and no more: **its name, its type, whether it is live,
leftover or ghost, and where its card is** (or that it has none yet).

**Not what the noun does.** Anything a reader could learn without opening a card
belongs in the card. The catalog is small on purpose — it is what makes landing
on one card in two hops possible, and every sentence added to it is paid for by
every reader, including the ones who wanted a different card.

## What a card holds

**Card means this page.** A territory may have its own: `content-editor` calls a
chunk of on-screen subtitle text a *caption card*, written to
`02-caption-cards.json` by `chunk_captions.py`. Same word, different object. On
a map, card is always the page.

One noun, one card. A `type:` line declaring one of the types in
`reference/card-types.md`, then:

- **What it is** — one or two sentences.
- **Why it's shaped that way** — the reasoning the file itself does not show.
  This is the part of a card that cannot be recovered by reading the source, and
  it is the reason the card exists.
- **Hits**
- **Does not hit**

Plus **where it is**, citing real files with line numbers, and **its status** —
live, leftover, or ghost.

A card declares one of the known types. It does not invent one mid-map. When a
genuinely new kind of noun turns up that no type fits, that is a reason to add a
type on purpose, in `reference/card-types.md`, where the addition is visible —
never a reason to stretch an existing one. A noun that fits nothing is first a
question: is this a noun at all, or is the type list short one?
