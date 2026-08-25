# walk-order.md — where to start, and what follows what

Every path on this page is inside `content-editor/`.

**This is not a tour.** It is the answer to "I have no particular question,
where do I start?" A reader who arrives with a question should go to the catalog
in this folder's `examples.md` and open the one write-up they need instead.

---

## The front door

**`caption.py`** — the run.

Everything else in this folder is one of four things: a step inside that run, a
file the run passes from one step to the next, a value a step reads, or a piece
of prose about what the run may not do. One command over one video, six steps,
`caption.py:45-52`. Read that list and the shape of the repo is settled.

Why here and not `CLAUDE.md`: the entry file tells you how to *operate* the tool,
which is the right front door for someone with a video to caption. A reader who
is going to **change** something needs the order the steps actually run in, and
that order lives in the list of steps, not in the writing about it.

## What follows what

Follow the material, not the folder listing. The order the steps run in is the
order that answers "what did the last one hand me":

```
caption.py                       the run
  -> 01_ingest                   the video is read, then transcribed
     01-video.md                 path, length, frame rate — hand-editable
     01-transcript.json          the words and their timings — the edit surface
     01-transcript-raw.json      the same, written once, never again
  -> 01b_calibrate               the levels are evened out
  -> 02_assemble                 the words become subtitles, plus an .srt
  -> 03_render                   the subtitles are burned on
```

Off to the side, and not part of a run: **`04_learn`**, which is what you run
afterwards if a word came out wrong.

Underneath all of it: **`_config/edit-defaults.yaml`**, which every stage reads,
and the contracts, which no stage reads and which you open only before changing
something.

## The rule that beats this order

If you have a question, do not walk. Read the catalog, open one write-up, stop.
This page exists for the reader who has no question yet.
