# card-types.md — the kinds of thing in content-editor

Every path on this page is inside `content-editor/`.

These were found by walking the folder, not assumed beforehand. **The list is
closed:** a write-up names one of these eight, never a ninth invented on the
spot. If a genuinely new kind of thing turns up, it gets added here on purpose,
where the addition is visible.

Each type answers "what else changes?" differently. That is what makes it a type
and not a label.

| Type | What it is | What changing one moves |
|---|---|---|
| **command** | The one thing that decides what runs and in what order: `caption.py`. | What runs, in what order, and which steps a rebuild skips. |
| **stage** | A numbered folder under `stages/`, holding a `CONTEXT.md` and its scripts. Takes named files, writes named files. | The files either side of it, and the list of steps in `caption.py:45-52`. |
| **handover file** | A file in `output/runs/[slug]/` that one stage writes and another reads, or that leaves the workspace as something you keep. Most are also where a person edits by hand. | Every stage after the one that writes it. |
| **setting** | A named value in `_config/`, or an override in `memory/preferences.md`. | Whatever reads it — **or nothing at all**, which is the thing worth checking before you trust one. |
| **written rule** | Writing that binds behaviour and that no code opens: `content-editor/`'s `CLAUDE.md`, `governance.md`, `identity.md`, `CONTEXT.md`, each stage's `CONTEXT.md`, and `reference/terminology.md`. | Nothing runs differently. People and models behave differently, which is slower to notice and harder to undo. |
| **shared part** | Code more than one stage uses and no stage owns: `pipeline_lib.py`, `tools/measure_audio.py`, `remotion/`. | Every stage that reaches for it — and how far that reaches is not visible from the folder it sits in. |
| **check** | Something that runs by itself and either stops you or reports: `tools/sync.py`, `tools/run_fixture_checks.py`, the hook in `.claude/`, and the self-checks inside a stage. | What is allowed to break quietly. Weakening one changes nothing today. |
| **running tally** | A file that builds up across runs: `memory/caption-fixes.md`, `memory/boundary-tally.md`. | **Nothing.** No run reads one to decide anything. Delete one and the count restarts. That is the whole effect. |

## The two pairs that get confused

**setting** and **running tally** both look like something that persists between
runs. They are opposites. A setting is read in order to decide something; a
tally is written in order to remember something. Before treating a file as
either, find the line that reads it. For a tally there is none, and for a
setting there had better be.

**written rule** and **check** both stop bad changes. A written rule stops them
by being read; a check stops them by failing. Only one of the two works on a
reader who never opened it.

## Why one type has a single member

**command** covers `caption.py` and nothing else. That is not padding out a
list. It is the only thing in the folder that decides *what runs*. Every stage
does work; only this one chooses the work and the order. Folding it into
**stage** would hide the change with the widest reach in the repo.
