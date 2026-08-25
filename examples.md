# The map of content-editor

`content-editor` takes a finished video and gives back the same video with
subtitles burned on and the audio evened out. That's all it does.

Four parts follow: the catalog, three things explained, one that looks used
and isn't, and one change followed all the way through.

**Every claim here can be checked.** Each one names a file and a line, like
`caption.py:66`. Open it and look — if the file says something different, the
file is right and this page is wrong. All paths below are inside
`content-editor/` — meaning the root of that repo, whatever the folder is
called on disk. A copy or a fresh clone counts the same; the root is the
folder holding `caption.py`.

Checked on 2026-08-24, straight from the files on disk — not the last version
saved to git (`9aa1f4e`, 2026-08-01; 27 files have changed since). One result
of that: `stages/01b_calibrate/` has never been saved to git at all, though it
made the audio for both real runs.

---

# 1 · The catalog

Everything in the repo worth naming. This part doesn't say what a thing
*does* — that's what a write-up is for.

**In use** — something reads it. **Unused** — nothing reads it, and doesn't
pretend to. **Unused, looks used** — nothing reads it, but everything around
it says it's in charge. Those are the dangerous ones.

Eight kinds of thing show up here, listed in this folder's
`reference/card-types.md`. Two need a quick note: a **handover file** is what
one step hands to the next. A **written rule** is a file that only tells a
person or a model what's allowed — no code reads it.

| Thing | Kind | Status | Written up |
|---|---|---|---|
| `caption.py` | command | in use | [§2](#caption-py) — **start here** |
| `stages/01_ingest` | stage | in use | — |
| `stages/01b_calibrate` | stage | in use, never saved into git | — |
| `stages/02_assemble` | stage | in use | — |
| `stages/03_render` | stage | in use | — |
| `stages/04_learn` | stage | you run it by hand | — |
| `01-topic.md` | handover file | in use | — |
| `01-video.md` | handover file | in use | — |
| `01-transcript.json` | handover file | in use | [§2](#01-transcript-json) |
| `01-transcript-raw.json` | handover file | in use | — |
| `02-caption-cards.json` | handover file | in use | — |
| `02-captions.srt` | handover file | the subtitle file you keep; no later step reads it | — |
| `03-remotion-props.json` | handover file | in use | — |
| `04-proposed-words.md` | handover file | never yet written | — |
| `_config/edit-defaults.yaml` | setting | in use | — |
| `caption_motion` | setting | **unused, looks used** | [§3](#caption-motion) |
| twelve more settings nothing reads | setting | **unused, looks used** | [§3](#caption-motion) |
| `_config/paths.yaml` | setting | in use | — |
| `_config/lexicon.txt` | setting | in use | — |
| `memory/preferences.md` | setting | in use, currently empty | — |
| `pipeline_lib.py` | shared part | in use | — |
| `tools/measure_audio.py` | shared part | in use | [§2](#tools-measure-audio-py) |
| `remotion/` | shared part | in use | — |
| `tools/sync.py` + `.claude/hooks/after-edit.py` | check | in use | — |
| `tools/run_fixture_checks.py` | check | in use | — |
| `tools/make_fixture.py`, `make_fixture_video.py` | check | in use | — |
| `tools/render_smoke_test.py` | check | never run | — |
| `memory/caption-fixes.md` | running tally | in use, empty | — |
| `memory/boundary-tally.md` | running tally | in use | — |
| `CLAUDE.md`, `CONTEXT.md`, `identity.md`, `governance.md` | written rule | in use | — |
| each stage's `CONTEXT.md` | written rule | in use | — |
| `reference/terminology.md` | written rule | in use | — |
| `BUILD-NOTES.md` | written rule | its *NOT verified* list is out of date | — |
| `reference/source-specs/` | written rule | unused — a record of where the numbers came from; `CONTEXT.md:59` says no script opens it | — |

A dash means nobody has written it up yet.

---

# 2 · Three things, explained

<a id="caption-py"></a>
## `caption.py` — the one command that runs everything

Runs six steps over one video, in a fixed order, at `:45-52`. Everything else
in the repo is one of those steps, a file passed between them, a value a step
reads, or a rule about what a step may not do.

**Why it's built this way.** `--redo` lets you fix a misheard word without
transcribing the video again — transcribing again would just bring back the
same wrong word. So `--redo` re-runs the list from step 3 on, instead of
working from a second copy of it. But where it restarts isn't automatic: the
number 3 is typed by hand at `:66`. A comment above it, at `:54-65`, shows
someone moved it from 2 to 3 when the audio step was added.

**What else changes.** The order of a run — `:45-52` is the only place that
order lives. And `--redo`, in a way you wouldn't expect: add a step above
position 3 and the 3 stays put, so every step below it shifts down one. The
rebuild then starts in the wrong place, with no warning. Also the checks that
run after an edit — `tools/sync.py:32` names `caption.py` by name.

**What looks like it changes and doesn't.** `CONTEXT.md`. It holds the
diagram of which stage runs when, and `CLAUDE.md:57-61` sends you there
before you change anything — but nothing actually opens it. Change the
steps, and that diagram goes quietly wrong until someone notices.

<a id="01-transcript-json"></a>
## `01-transcript.json` — the words, and the file you edit

Every word that was said, with the time it starts and ends, plus the video's
path, length, and frame rate. Written by
`stages/01_ingest/scripts/transcribe.py:136` to `output/runs/[slug]/`. When a
word comes out wrong, this is the file you fix.

**Why it's built this way.** Two things about it aren't obvious. First, it's
written twice: `01-transcript-raw.json` is a copy made once, at
`transcribe.py:140-147`, and never touched again. Not a backup — it's the
only record of what the transcription originally heard, which is how
`learn_words.py` spots your corrections. Second, its `"video"` line gets
overwritten partway through the run, changed to point at the evened-out
audio copy instead (`calibrate_audio.py:195`). That's why the audio stage
reads `01-video.md` instead — the line it would normally read is the one it
just overwrote (`calibrate_audio.py:12-17`).

**What else changes.** `02-caption-cards.json` and `02-captions.srt` — both
rebuilt from it. `03-remotion-props.json` and the finished video, since the
render step takes the length, frame rate, and path from here instead of
opening the video again (`render_captions.py:44-52`). And the audio stage,
which reads where each word starts and ends, because sound levels are
measured over speech.

**What looks like it changes and doesn't.** `01-transcript-raw.json`. Same
folder, almost the same name, same shape — the obvious file to reach for,
and the wrong one. Overwrite it and every correction stops looking like a
correction: `learn_words.py` counts nothing, and the word list stops
growing. Nothing breaks out loud — it just quietly stops learning
(`governance.md`, rule 3).

<a id="tools-measure-audio-py"></a>
## `tools/measure_audio.py` — needed by every run, filed under "not part of a run"

Measures the sound: how loud it is moment by moment, and where someone is
speaking. It sits in `tools/`, and runs on every video, used at
`stages/01b_calibrate/scripts/measure.py:41`.

**Why it's built this way.** It was first written to produce the numbers in
`reference/source-specs/audio-measurements.md`. When the audio stage was
built, `measure.py` reused this script instead of measuring the same thing a
second way — the reason is at `measure.py:18-23`: two versions of the same
measurement would let the pipeline and the evidence behind it drift apart.
One side effect: the repo's own description of itself is wrong here.
`CLAUDE.md:94-95` lists what's in `tools/` and skips this file.
`BUILD-NOTES.md:122` says of `tools/`, *"None of them is part of a run."*

**What else changes.** Every run — `measure.py` uses it, and both
`segment.py` and `calibrate_audio.py` use `measure.py`. Break it, and a run
stops at the audio step. Also `audio-measurements.md`, since every number in
it came from this script.

**What looks like it changes and doesn't.** The checks in `tools/sync.py`.
You'd expect the check that runs after every edit to cover the folder this
file lives in. It doesn't — it watches `stages`, `_config`, and `memory`,
plus two files by name (`tools/sync.py:31-33`). Edit this file, and nothing
runs. The one file in `tools/` a run actually depends on is the one file the
check was never told about.

---

# 3 · One that looks used and isn't

<a id="caption-motion"></a>
## `caption_motion` — a setting nothing reads

`caption_motion: none` at `_config/edit-defaults.yaml:33` means subtitles
don't pop, light up word by word, or type themselves on. That's genuinely
how they behave — but nothing reads the setting to make it happen.

It's one of thirteen settings in that file, and none of the thirteen are
read by anything:

`gap_between_cards_s` :31 · `lead_in_s` :32 · `caption_motion` :33 ·
`prefer_fewest_cards` :57 · `preserve_quotes` :61 · `anchor_semantics` :133 ·
`allow_serif` :184 · `hierarchy_via` :185 · `background_box` :195 ·
`subtitles_topmost` :197 · `render.resolution` :486 ·
`render.single_export_serves` :487 · `max_words_per_card` :507

Twelve of them sit under a heading called `rules:`, which says at `:9`:
*"rules: enforced. A violation is a bug."*

**Why it's built this way.** The file was built by hand from two written
specs, and every value those specs mentioned got copied in so nothing was
lost. Good instinct — but it left values that record a decision sitting in a
file meant to hold what's enforced. The real behavior lives elsewhere:

| Setting | Where the behavior actually is |
|---|---|
| `caption_motion` | `remotion/src/CaptionedVideo.tsx:14-19` — no `interpolate()` call anywhere in the file, so nothing can move unless someone adds it |
| `subtitles_topmost` | same file, `:23` and `:57-94`. Subtitles are drawn last, which puts them on top |
| `background_box` | same file, `:83`. `background: "none"` is written into the code |
| `gap_between_cards_s` | subtitles are built end to end; `:27-29` depends on it |
| `anchor_semantics` | `render_captions.py:160` and `CaptionedVideo.tsx:64` center the block. The setting describes what they do; it doesn't tell them to |
| `render.resolution` | a duplicate of `geometry.canvas` at `:112`, which **is** read, at `render_captions.py:203` |

`render.resolution` is the clearest case: set it to any size, and the video
still comes out 1080×1920. This setting used to be called `animation`, until
2026-08-24, when "delete everything about animation" nearly took it out too
— along with an unrelated project of the same name (see this folder's
`reference/collisions.md`).

**What else changes.** Nothing. Change it, delete it, flip it — no run
behaves any differently. That's the finding, not a footnote to it. What it
does affect is the next person, who'll believe this setting controls the
behavior, and may go delete the code that actually does.

**What looks like it changes and doesn't.** `remotion/src/CaptionedVideo.tsx`
— the file that draws the subtitles, and so the one place that would
animate them if anything did. The values `render_captions.py` hands it, at
`:210-233`, never include this setting. Also `words_per_card`:
`max_words_per_card`, at `:507`, sits one line below `words_per_card`, at
`:506`, and only the upper one gets read — at `chunk_captions.py:385`, to
count how many subtitles fall outside four to six words and print that
count. Edit the upper one and a number on screen changes, which is enough to
make it look like the pair works. Edit the lower one and nothing happens
anywhere. Neither actually caps anything — a comment at `:504-505` explains
that a 2.5-second limit already stops a subtitle at around eight words.

---

# 4 · One change, and what it hits

**"A word came out wrong. I fixed it in `01-transcript.json`."**

| What changes | Why |
|---|---|
| `02-caption-cards.json` | rebuilt from the corrected words |
| `02-captions.srt` | rewritten from those |
| `03-remotion-props.json` | rebuilt, carrying the new text |
| the finished `.mp4` | rendered again — the slow part |
| `memory/caption-fixes.md` | **only if you then run `learn_words.py`**, which no run calls |

**What looks like it changes and doesn't.** The evened-out audio, in
`output/audio/[slug]/`. Everyone reaches for this one, and for a good
reason: the transcript's own `"video"` line points straight at it, so the
file you just edited looks like it's naming the audio as its subject. It
isn't touched. `caption.py:66` starts the rebuild at step 3, below the audio
step — fixing a spelling can't change the audio, and re-running that slow
step would just produce an identical file (`caption.py:58-65`). Same goes
for `01-transcript-raw.json`, for the reason given in its write-up above.

---

## Where the rest of this folder is

`reference/card-types.md` — the eight kinds of thing this repo contains, and
what changing each one moves.

`reference/walk-order.md` — where to start if you have no question yet, and
the order the material actually flows in.

`reference/collisions.md` — the six words that mean two things here, and the
mistake each one prevents.
