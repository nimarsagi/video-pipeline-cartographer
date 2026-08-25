# Video Pipeline Cartographer

An instrument for mapping a body of work someone will change. What comes back
is a map: what the things in it are, what else moves when you touch one, and
which of them are actually in use, which are dead weight, and which are names
with nothing behind them. The map is the product; this folder is the
instrument that makes one.

It has mapped one territory so far — `content-editor`, the video pipeline that
adds subtitles and evens out audio — and that run is already done. So in
practice, today, this folder is used for three things:

1. **Answering a specific question about content-editor** before you change
   something there. The written map is `examples.md` plus `reference/`; a
   reader gets one answer in two hops — catalog, one write-up, stop. This is
   the everyday use.
2. **Browsing the whole map as a page.** `output/map-content-editor.html`,
   rendered from the written map — open it in a browser. Saying
   **map content-editor** — that exact phrase — regenerates it.
3. **Charting a new folder** when another body of work needs the same
   treatment. Until that happens, everything here is about content-editor.

---

## What is in here

```
CLAUDE.md      what to read and when — the file Claude opens first
identity.md    who the cartographer is, what it walks, who reads the map
rules.md       the vocabulary, and the two things a map must never do
examples.md    the worked map: the catalog, four write-ups, one change
reference/     what a reader consults, fitted to that one repo
output/        rendered HTML pages — map-content-editor.html is the worked one
README.md      this
```

**`reference/`** holds three things, kept apart because a reader arrives with
one question and should not have to load the other two to answer it:

```
card-types.md   the kinds of thing that repo contains, and what changing each moves
walk-order.md   where to start if you have no question yet, and the front door
collisions.md   the words that mean two things there, and the mistake each prevents
```

## The one rule

**Read the catalog. Then one write-up. Then stop.**

Never the whole folder. The catalog exists so that nobody has to read the shelves.
It is small on purpose, and a reader who opens every write-up has paid the exact
cost this was built to remove.

If you ever find yourself writing "add every file to the project", the map has
failed no matter how good the write-ups are.

## Using it on a new body of work

Drop this folder into a Claude project alongside the thing you want mapped, then
say **chart** and name it:

> chart the invoicing folder

**Why the word is "chart" and not "map":** a globally installed skill
(icm-architect, in `~/.claude/skills/`) also answers to *map this repo*,
*audit this folder*, and *what would a change hit*. Say those and the skill,
not this contract, takes over — and what comes back is a workspace redesign,
not a map. *Chart* belongs to this folder alone.

**One phrase has its own job here:** **map content-editor**, said exactly,
renders the finished written map as a browsable HTML page in `output/` —
the vocabulary up front, then the filterable catalog, the cards, the types,
and the collisions, in one self-contained file. Any other use of "map" starts
nothing. Charting walks and writes; this renders what was written.

In a terminal, start one level up, where both this folder and the target are
visible, and point at the contract in the same breath — naming the contract
beats any skill trigger:

```
Read video-pipeline-cartographer/CLAUDE.md and follow it: chart the invoicing folder.
```

Start Claude *inside* this folder and `CLAUDE.md` loads by itself, but the thing
you want mapped is then outside what it can read.

**What to feed it:** the folder itself, and permission to read it. Not a
description of the folder, not a directory listing you typed out — the real
thing. Every write-up points at real files with line numbers, and a pointer you
cannot open is worth nothing.

**What it needs from you:** two answers it cannot get from the files.

1. **What the folder is for, in a sentence.** Anything it does that serves
   neither that sentence nor any other job you name is where the dead weight
   collects, along with the names that have nothing behind them. That sentence
   is the first place to look.
2. **The words that mean two things.** It proposes; you decide. Nothing goes
   into `reference/collisions.md` until you have confirmed, corrected, or
   rejected each one, because you are the only person who knows which meaning
   wins.

**What you get back:** a catalog, and a write-up for each thing that earned one.
Not one per file. Three real write-ups that point at source beat a fake city.

## Reading a map it made — including by a model with no memory

Don't say "walk me through" or "map" — ask the specific question you have, and
name where the answer lives:

> What else moves if I change `chunk_captions.py`? Answer from the map in
> `video-pipeline-cartographer/` — catalog first, then one write-up, then stop.

> Is `render.resolution` actually read by anything? Check the catalog in
> `video-pipeline-cartographer/examples.md` before opening the config.

The path a reader (human or model) follows:

1. Open **`examples.md`** and read the catalog. Nothing else on the page.
2. Find the thing you care about. Read its type, whether anything uses it, and
   where its write-up is.
3. Open **that one write-up**. It tells you what the thing is, why it is built
   that way, what else changes if you change it, and the neighbour that looks
   like it changes too and doesn't.
4. **Stop.**

Two hops. If you needed three, the map has a fault worth reporting.

No question yet? Start at `reference/walk-order.md`, which names the front door
and the order the material actually flows in. That page is the exception, not
the pattern. It exists for the reader who has no question yet, and it is still
not a thing to read end to end.

Met a word you are unsure of? `reference/collisions.md`, before you act on it.

## What it will not do

It will not tell you why something broke, list everything wrong with the folder,
narrate how the work goes, or write a second specification. If a write-up and
the real file disagree, **the file is right and the write-up is wrong.**
