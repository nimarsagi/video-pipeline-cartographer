# collisions.md — words that mean two things in content-editor

Confirmed by the owner on 2026-08-24. Nothing is on this page that he did not
agree to.

Where a rename fixed the word in the repo, the entry says so and the new
name is what the repo now uses. Where nothing was renamed, the entry is the only
thing standing between a reader and the wrong object.

The repo keeps its own copy of these, in
`content-editor/reference/terminology.md`, under *Words that mean two things*.

---

### script

| | |
|---|---|
| **Means here** | The words he says in a take. This is the default and the only meaning that needs no qualifier. |
| **Also names** | A Python file under `stages/*/scripts/` — eleven of them. |
| **Rule** | The second is a **python script**, said with the word *python* in front. Without it, the sentence is about the spoken words. |
| **Prevents** | A model told "the script is wrong" going to edit a program when a transcript was meant. |
| **Renamed** | No. The folders would cost paths in five files, and the owner's own speech carries the distinction. |

### hook

| | |
|---|---|
| **Means here** | The thing in `.claude/hooks/after-edit.py` that runs `tools/sync.py` after every edit. Wired at `.claude/settings.json:9`. |
| **Also names** | The large text panel at the top of a reel — Inter 800 at 85–110 px, `HOOK_SIZE = 96`, a Poppins variant. `content-editor/reference/source-specs/reel-typography-spec.md:28,40,45,64`, and `reel-edit-spec-askcatgpt.md:103` beside it. |
| **Renamed** | Yes — the second is now the **opener**, in `content-editor/reference/source-specs/EXCLUSIONS.md:37-38`. The two spec files still say *hook*: they are a record of someone else's reels analysed frame by frame, and rewriting them would falsify the analysis. |
| **Prevents** | Two mistakes. Editing the edit-hook when the reel's opening panel was meant. And, worse, *building* the opener — `EXCLUSIONS.md:37` marks it a deferred phase, do not build. |

### clip

| | |
|---|---|
| **Means here** | One file off the phone, a few seconds long. `content-editor/reference/terminology.md`, *Session · clip · export*. Invisible by the time the export arrives — the joins are baked in. |
| **Also named** | A stretch that `01b_calibrate` found for itself by measuring. |
| **Renamed** | Yes — that is a **segment**. `between_clip_correction` and `within_clip_correction` are now `between_segment_correction` and `within_segment_correction` (`_config/edit-defaults.yaml:234,270`), which is the word `segment.py` already used. |
| **Prevents** | Turning one of those settings up expecting it to act on your phone clips. `governance.md` rule 4: the boundaries "never have to agree with the real cuts" — the stage may find three segments in a video assembled from nine clips. Also stops `kept 3 of 14` being read as *found 3 of my 9 clips*. |

### render

| | |
|---|---|
| **Three meanings** | The stage, `stages/03_render`, which burns the subtitles on. The finished file you post, written to `render_output` (`_config/paths.yaml:5`). And the `render:` block in `_config/edit-defaults.yaml:485-489`. |
| **Renamed** | No — kept as it stands, the owner's call. |
| **Prevents** | Two. "The render is wrong" almost always means the video came out wrong, and the fault is upstream — in the transcript, the subtitles, or the levelling — so it is a symptom, not an address. And the `render:` block reads like the place you would change render behaviour; nothing reads it. See the `caption_motion` write-up in this folder's `examples.md`. |

### rule

| | |
|---|---|
| **Three meanings** | A value under `rules:` in `_config/edit-defaults.yaml`. One of the four in `governance.md` that no component may break. And what the person used to write by hand in `memory/preferences.md`. |
| **Renamed** | The third only. It is now an **override**, written `**Override:**` — `memory/preferences.md`, read at `pipeline_lib.py:99`. |
| **Why only the third** | It is the one that gets mixed up, because writing it changes a *rule* in the config file. Renaming `rules:` would cost every `lib.rules()` call plus the guard at `pipeline_lib.py:59` that stops a run when the rules/diagnostics split goes missing. The four in `governance.md` describe what code may never do; nobody edits those to change how a caption looks. |
| **Prevents** | "Add a rule" landing in the wrong file, and the reader not knowing that the override beats the config value while the governance rule beats everything. |

### animation

| | |
|---|---|
| **Means here** | Nothing. The word is not used in this workspace any more, on purpose. |
| **Was** | `animation: none` in `_config/edit-defaults.yaml` — subtitles do not pop, highlight word by word, or type themselves on. |
| **Also names** | The owner's separate, unstarted project of drawing visuals into videos. That one keeps the word. |
| **Renamed** | Yes — the setting is now `caption_motion: none` (`_config/edit-defaults.yaml:33`). |
| **Prevents** | Exactly the near-miss on record: "delete everything about animation" nearly took the caption setting with it, which would have let a later build put popping captions back. The rename makes the two impossible to confuse. |
| **Note** | This is the one entry whose second meaning lives outside the repo. It is here because the mistake it prevents happens inside it. |
