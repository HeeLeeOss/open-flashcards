# TASKS — open-flashcards

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

Backlog for the `open-flashcards` good-deed project. Read alongside `PLAN.md` (same directory).

## How these tasks map to Hee-Lee Oss

Every task here becomes a **Task JSON** validated by `packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable, e.g. `open-flashcards-style-001` (the table's ID column).
- `title` — the task title.
- `project` — `"open-flashcards"` for all tasks.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (see each task).
- `lane` — `"donated"` for all tasks (the human runs their own agent; the CLI preps workspace + PR).
  No `funded` tasks in this backlog, so `fundedBudgetUsd` is not required.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["education","open-education-resources","spaced-repetition"]`.
- `riskTier` — `low | medium | high`. Default `low`; subject-accuracy work is `medium`;
  medical/legal/financial/safety/high-stakes-exam decks are `high` (credentialed expert sign-off).
- `urgent` — boolean (default `false`; nothing here is time-critical).
- `deliverable` — `pr | dataset | document | translation`. Card/deck content artifacts → `dataset`;
  published/merged decks, code, and adapters → `pr`; style guide / rubrics / research → `document`;
  localised decks → `translation`.
- `tokenEstimate` — `small | medium | large` (the table's Size column).
- `status` — `open | in-progress | review | delivered | done` (all start `open`).
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — task body fields.
- `requestor` — proposer/maintainer; `verifiedNeed` — **`false`** for delivery-dependent tasks until a
  publishing channel **and** a validating educator are secured (see PLAN "partner gap"); `true` only
  where the artifact is self-evidently useful and channel-independent.
- `outputLicense` — `MIT` for code; `CC-BY-4.0` (or `CC-BY-SA-4.0` / `CC0-1.0` to match source terms)
  for deck content and documents.

At production scale this project fans out: **one deck (or one card batch) = one task**, drawn from a
subject-scoped batch (the `*-deck-*` and `*-batch-*` tasks below are the templates for that fan-out).

---

## Milestone M0 — Foundation & cold-start

Prove one deck can travel from an open/PD source → accuracy-reviewed → published-and-installable.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| open-flashcards-style-001 | Author card-writing & sourcing style guide v1 | writing | small | low | document | — | Maintainer |
| open-flashcards-research-001 | Secure ≥1 publishing channel + ≥1 validating educator; validate AnkiWeb policy | research | medium | low | document | — | Maintainer + Steward |
| open-flashcards-research-002 | Approved-source list + license-verification rules (PD/CC only) | research | small | medium | document | — | Steward |
| open-flashcards-data-001 | Define CrowdAnki-aligned card/deck data model + provenance/attribution + GUID rules | data | small | low | dataset | style-001 | Maintainer |
| open-flashcards-code-001 | Deterministic CrowdAnki JSON → `.apkg` build tool | code | medium | low | pr | data-001 | Maintainer |
| open-flashcards-deck-001 | Author cold-start deck (~50 cards) from one open/PD source (≥2 note types, ≥1 open-media card) | data | medium | low | dataset | research-002, style-001, data-001 | Domain reviewer |
| open-flashcards-pub-001 | Build, publish, confirm installable + measure coverage baseline | maintenance | medium | low | pr | code-001, deck-001, research-001 | Steward |

**Sequencing — channel/partner and accuracy gates are hard.** `research-001` blocks `pub-001`: no deck
is published until ≥1 channel is confirmed (and its policy validated) and ≥1 validating educator is
identified. Accuracy review blocks every merge. If the time-boxed channel/partner search confirms
nothing, the project hits its **kill/pivot criteria** (next candidate channel per PLAN's decision
tree, or stop) — it does not produce undeliverable decks. GitHub release is an always-available
fallback channel, but `verifiedNeed` stays `false` until an educator validates the first subject's need.

**Key task acceptance criteria**

- **open-flashcards-style-001** (style guide v1)
  - [ ] Defines the **atomicity / minimum-information** rule (one fact per card; exactly one defensible
        answer) and prompt-disambiguation guidance.
  - [ ] Specifies cloze conventions and the **standard note types** allowed (Basic, Basic-reversed,
        Cloze) — no custom note types.
  - [ ] Mandates **per-card sourcing & attribution** ("source-or-it-doesn't-ship") and how to record
        license + source URL.
  - [ ] Has explicit **accessibility** (semantic HTML, image alt-text, no color-only cues) and
        **i18n/UTF-8** conventions.
  - [ ] Has a **high-stakes & civic** section ("study aid, not advice" framing; expert escalation;
        non-partisan rule; no proprietary question banks).
  - [ ] Published, versioned, licensed CC-BY-4.0.

- **open-flashcards-research-001** (secure channel + validating educator)
  - [ ] Evaluates candidate channels in priority order (GitHub release → AnkiWeb → OER/CrowdAnki
        partner) with each one's acceptance posture, and applies the PLAN decision tree.
  - [ ] Confirms ≥1 channel will accept clean-provenance, openly-licensed, attributed decks, and
        **validates AnkiWeb shared-deck policy / account terms** if AnkiWeb is chosen.
  - [ ] Identifies ≥1 **validating educator/learner** who confirms the first subject is genuinely
        needed and will check accuracy.
  - [ ] Records the channel's required deck metadata, license display, and any rate/automation limits.
  - [ ] States the time-box and kill/pivot criteria; on success, flips `verifiedNeed=true` for tasks
        targeting that channel/subject.

- **open-flashcards-research-002** (license-verification rules)
  - [ ] Produces an **approved-source list** (PD, CC0, CC-BY, CC-BY-SA, OER, official open syllabi)
        with a per-source license note.
  - [ ] Defines the **"unknown = excluded"** rule and the **categorical exclusion of existing
        third-party decks** as sources.
  - [ ] Specifies how **share-alike / NC** terms constrain the deck's chosen output license.
  - [ ] Specifies separate **media (image/audio) license verification** with its own attribution.

- **open-flashcards-deck-001** (cold-start deck)
  - [ ] All cards sourced from **one clearly open/PD source**; per-card source + license + attribution
        recorded; no third-party-deck content.
  - [ ] Low-risk subject; exercises **≥2 note types** and includes **≥1 card with an openly-licensed
        image** (media license recorded).
  - [ ] Every card conforms to style guide v1 (atomic, unambiguous, accessible HTML) and has a
        **stable, deterministic GUID**.
  - [ ] Every card scored on the 4-dimension accuracy rubric against the source, failures tagged by the
        error taxonomy; nothing below 3/4 on any dimension is kept.
  - [ ] Output is valid CrowdAnki JSON in `decks/`; output license compatible with the source.

- **open-flashcards-pub-001** (build, publish, baseline)
  - [ ] Deck **builds deterministically** to `.apkg` and is confirmed **installable in Anki +
        AnkiDroid** with cards rendering correctly.
  - [ ] Deck **published to the confirmed channel** with license + attribution + changelog; source JSON
        committed; semver assigned.
  - [ ] **Curriculum-coverage baseline** measured for the chosen subject using a reproducible method
        (named standard snapshot + date + in-scope denominator).
  - [ ] No SM-2/FSRS scheduling state baked in; deck is scheduler-agnostic.

**M0 Definition of Done:** style guide v1 + sourcing rules + data model published; build tool working;
≥1 channel confirmed and ≥1 validating educator identified; ≥1 ~50-card deck published, installable,
100% accuracy-reviewed with full attribution; coverage baseline measured; zero license violations.

---

## Milestone M1 — Pipeline & validation

Turn the manual slice into a repeatable, CI-gated pipeline.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| open-flashcards-code-002 | Deck linter/validator + CI gate (schema, cloze, GUIDs, attribution, HTML, note types) | code | large | low | pr | data-001, code-001 | Maintainer |
| open-flashcards-code-003 | License-verification gate / approved-source checker ("unknown = excluded") | code | medium | medium | pr | research-002, data-001 | Steward |
| open-flashcards-code-004 | Stable-GUID + non-destructive update tooling (semver + changelog) | code | medium | low | pr | code-001, data-001 | Maintainer |
| open-flashcards-adapter-001 | Publishing adapter (AnkiWeb and/or GitHub release automation) | code | medium | low | pr | research-001 | Maintainer |
| open-flashcards-doc-001 | Reviewer workflow: 4-dimension accuracy rubric, error taxonomy & expert-escalation checklist | writing | small | low | document | style-001 | Maintainer |
| open-flashcards-batch-001 | Author + publish ≥3 decks (~300+ cards) via the repeatable pipeline | data | large | medium | pr | code-002, code-003, adapter-001, doc-001 | Reviewer rotation |

**Key task acceptance criteria**

- **open-flashcards-code-002** (linter + CI)
  - [ ] Validates CrowdAnki JSON against the data-model schema; rejects empty required fields and
        non-standard note types.
  - [ ] Detects **broken cloze syntax**, **duplicate or missing GUIDs**, **missing attribution**, and
        unsafe/malformed HTML; flags missing image alt-text.
  - [ ] Runs as a **CI gate**: a deck PR cannot merge unless validation passes; emits an auditable
        report (no secrets in logs).

- **open-flashcards-code-003** (license gate)
  - [ ] Rejects any card whose source is not on the approved-source list or whose license is not
        verifiably PD/CC-* ("unknown = excluded").
  - [ ] Records SPDX-style license id + attribution + source URL + retrieval date per accepted card.
  - [ ] Flags share-alike/NC sources that would constrain or block the deck's output license.

- **open-flashcards-code-004** (GUID stability / non-destructive update)
  - [ ] Derives **deterministic, collision-free GUIDs** from a stable key; re-import **updates**, never
        duplicates.
  - [ ] Updating a deck preserves GUIDs so learners keep scheduling history; bumps semver and appends a
        changelog entry.

- **open-flashcards-batch-001** (first repeatable run)
  - [ ] ≥3 decks / ~300+ cards published via license gate → linter/CI → human accuracy review →
        publishing adapter.
  - [ ] Reviewer-corrected rate and error-taxonomy mix measured; medium-risk subjects domain-reviewed.

**M1 Definition of Done:** linter + CI gate and license gate operational; GUID-stable update tooling
working; ≥1 publishing adapter automated; reviewer workflow documented with ≥2 reviewers onboarded;
≥3 decks (~300+ cards) published with measured reviewer-corrected rate; zero license violations.

---

## Milestone M2 — Controlled scale (one subject, deep)

Cover one coherent curriculum subject, with accuracy and provenance held.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| open-flashcards-batch-002 | Build a coherent single-subject deck set (≥6 decks / ≥1,500 cards) | data | large | medium | pr | batch-001 | Reviewer rotation |
| open-flashcards-research-003 | Standards/curriculum mapping + coverage measurement for the subject | research | medium | low | document | batch-001 | Maintainer |
| open-flashcards-research-004 | Educator/learner beneficiary validation of the subject's decks | research | small | low | document | batch-001 | Maintainer |
| open-flashcards-data-002 | Quality + adoption metrics: rubric scores, reviewer-corrected rate, error mix, installs/ratings | data | small | low | dataset | batch-001 | Maintainer |

**Key task acceptance criteria**

- **open-flashcards-batch-002** (subject deck set)
  - [ ] ≥6 decks / ≥1,500 cards published and installable in one coherent subject.
  - [ ] Zero license/provenance violations; zero known factual errors; all high-stakes cards
        expert-signed and carrying "not advice" framing; civic content non-partisan.
  - [ ] All cards carry full attribution; all decks build deterministically and update non-destructively.

- **open-flashcards-research-004** (beneficiary validation)
  - [ ] ≥1 educator/learner reviews the subject's decks and confirms accuracy + usability.
  - [ ] Findings feed back into the style guide / reviewer checklist.

**M2 Definition of Done:** ≥6 decks / ≥1,500 cards published in one subject; zero license violations and
zero known factual errors; standards mapping + coverage delta measured; ≥1 beneficiary validation;
quality + adoption metrics dashboarded.

---

## Backlog / future (sized, unscheduled)

| ID | Title | Type | Size | Risk | Deliverable | Notes |
|---|---|---|---|---|---|---|
| open-flashcards-research-005 | High-stakes / exam-prep deck pathway (expert sign-off + "not advice") | research | small | high | document | Defines credentialed-expert flow; official open syllabi only |
| open-flashcards-translate-001 | Multilingual / localised deck extension | writing | large | medium | translation | i18n; per-language reviewer; UTF-8/script support |
| open-flashcards-deck-audio-001 | Openly-licensed pronunciation audio for language decks | data | large | medium | dataset | Overlaps `open-pronunciation-audio`; sibling-project candidate |
| open-flashcards-code-005 | Outcome-tracking dashboard (published/installable + adoption per deck) | code | medium | low | pr | Sustainability metric |
| open-flashcards-code-006 | Media-sourcing pipeline (open image/audio license verification + attribution) | code | medium | medium | pr | Scales the media path beyond hand-curation |
| open-flashcards-doc-002 | Accessibility audit + guide for card templates | writing | small | low | document | Ties to a11y-alttext-commons conventions |
| open-flashcards-maint-001 | Maintenance vs Anki / genanki / CrowdAnki format drift | maintenance | medium | low | pr | Ongoing post-delivery |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 task. `verifiedNeed` is `true` here: a published,
openly-licensed card-writing & sourcing style guide is a self-evidently useful artifact and does not
depend on a publishing channel or validating educator being secured (unlike the `deck`/`pub` tasks,
which are `false` until M0's channel + educator are confirmed).

```json
{
  "id": "open-flashcards-style-001",
  "title": "Author card-writing & sourcing style guide v1",
  "project": "open-flashcards",
  "type": "writing",
  "lane": "donated",
  "priority": "high",
  "domain": ["education", "open-education-resources", "spaced-repetition"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "small",
  "status": "open",
  "context": "The public Anki deck ecosystem is undermined by decks of unknown provenance and unverified accuracy, and by inconsistent card-writing quality. Before any deck is authored at scale, open-flashcards needs a single, versioned style guide so that cards produced by many parallel AI sessions are atomic, unambiguous, accessible, accuracy-reviewable, and sourced only from openly-licensed material with per-card attribution.",
  "objective": "Write and publish version 1 of the card-writing and sourcing style guide that all deck/batch tasks will follow.",
  "acceptanceCriteria": [
    "Defines the atomicity / minimum-information principle (one fact per card; exactly one defensible answer) and prompt-disambiguation guidance.",
    "Specifies cloze conventions and restricts decks to standard note types (Basic, Basic-and-reversed, Cloze) for cross-app portability; forbids custom note types and baked-in scheduler state.",
    "Mandates per-card sourcing and attribution ('source-or-it-doesn't-ship'): how to record source URL, SPDX-style license id, and attribution, and the categorical exclusion of existing third-party decks as sources.",
    "Includes accessibility conventions (semantic HTML, image alt-text, no color-only cues) and i18n/UTF-8 conventions.",
    "Includes a high-stakes and civic section: 'study aid, not advice' framing, credentialed-expert escalation for medical/legal/financial/safety/exam content, non-partisan rule for civic content, and a ban on copying proprietary question banks.",
    "Published, versioned, and licensed CC-BY-4.0."
  ],
  "resources": [
    "C:\\code\\hee-lee-oss\\planning\\projects\\open-flashcards\\PLAN.md",
    "C:\\code\\hee-lee-oss\\docs\\good-deed-definition.md",
    "Anki manual (note/card model, note types, GUIDs, .apkg package format)",
    "Creative Commons license suite (CC0, CC-BY, CC-BY-SA) and SPDX license identifiers"
  ],
  "output": "A versioned style-guide document (style-guide/card-writing-style-guide.md), CC-BY-4.0, covering atomicity, cloze and note-type conventions, sourcing & attribution, accessibility & i18n, and high-stakes/civic handling.",
  "requestor": "jdev1977",
  "verifiedNeed": true,
  "outputLicense": "CC-BY-4.0"
}
```

---

## Generated task index

> Auto-generated by Hee-Lee Oss task-decomposition agent on 2026-06-29.
> Branch: `hee-lee-oss/task-decomposition` · 23 new task files generated (1 pre-existing seed).
> All 24 files validated PASS against the Hee-Lee Oss task schema.

| File | Title | Milestone | Type | Priority | Deliverable | verifiedNeed |
|---|---|---|---|---|---|---|
| [open-flashcards-style-001.json](tasks/open-flashcards-style-001.json) | Author card-writing & sourcing style guide v1 | M0 | writing | high | document | true |
| [open-flashcards-research-001.json](tasks/open-flashcards-research-001.json) | Secure ≥1 publishing channel + ≥1 validating educator; validate AnkiWeb policy | M0 | research | high | document | true |
| [open-flashcards-research-002.json](tasks/open-flashcards-research-002.json) | Approved-source list + license-verification rules (PD/CC only) | M0 | research | high | document | true |
| [open-flashcards-data-001.json](tasks/open-flashcards-data-001.json) | Define CrowdAnki-aligned card/deck data model + provenance/attribution + GUID rules | M0 | data | high | dataset | true |
| [open-flashcards-code-001.json](tasks/open-flashcards-code-001.json) | Deterministic CrowdAnki JSON → .apkg build tool | M0 | code | high | pr | true |
| [open-flashcards-deck-001.json](tasks/open-flashcards-deck-001.json) | Author cold-start deck (~50 cards) from one open/PD source (≥2 note types, ≥1 open-media card) | M0 | data | high | dataset | false |
| [open-flashcards-pub-001.json](tasks/open-flashcards-pub-001.json) | Build, publish, confirm installable + measure coverage baseline | M0 | maintenance | high | pr | false |
| [open-flashcards-code-002.json](tasks/open-flashcards-code-002.json) | Deck linter/validator + CI gate (schema, cloze, GUIDs, attribution, HTML, note types) | M1 | code | high | pr | true |
| [open-flashcards-code-003.json](tasks/open-flashcards-code-003.json) | License-verification gate / approved-source checker ("unknown = excluded") | M1 | code | high | pr | true |
| [open-flashcards-code-004.json](tasks/open-flashcards-code-004.json) | Stable-GUID + non-destructive update tooling (semver + changelog) | M1 | code | medium | pr | true |
| [open-flashcards-adapter-001.json](tasks/open-flashcards-adapter-001.json) | Publishing adapter (AnkiWeb and/or GitHub release automation) | M1 | code | medium | pr | false |
| [open-flashcards-doc-001.json](tasks/open-flashcards-doc-001.json) | Reviewer workflow: 4-dimension accuracy rubric, error taxonomy & expert-escalation checklist | M1 | writing | medium | document | true |
| [open-flashcards-batch-001.json](tasks/open-flashcards-batch-001.json) | Author + publish ≥3 decks (~300+ cards) via the repeatable pipeline | M1 | data | medium | pr | false |
| [open-flashcards-batch-002.json](tasks/open-flashcards-batch-002.json) | Build a coherent single-subject deck set (≥6 decks / ≥1,500 cards) | M2 | data | medium | pr | false |
| [open-flashcards-research-003.json](tasks/open-flashcards-research-003.json) | Standards/curriculum mapping + coverage measurement for the subject | M2 | research | medium | document | false |
| [open-flashcards-research-004.json](tasks/open-flashcards-research-004.json) | Educator/learner beneficiary validation of the subject's decks | M2 | research | medium | document | false |
| [open-flashcards-data-002.json](tasks/open-flashcards-data-002.json) | Quality + adoption metrics: rubric scores, reviewer-corrected rate, error mix, installs/ratings | M2 | data | medium | dataset | false |
| [open-flashcards-research-005.json](tasks/open-flashcards-research-005.json) | High-stakes / exam-prep deck pathway (expert sign-off + "not advice") | Backlog | research | low | document | true |
| [open-flashcards-translate-001.json](tasks/open-flashcards-translate-001.json) | Multilingual / localised deck extension | Backlog | writing | low | translation | false |
| [open-flashcards-deck-audio-001.json](tasks/open-flashcards-deck-audio-001.json) | Openly-licensed pronunciation audio for language decks | Backlog | data | low | dataset | false |
| [open-flashcards-code-005.json](tasks/open-flashcards-code-005.json) | Outcome-tracking dashboard (published/installable + adoption per deck) | Backlog | code | low | pr | false |
| [open-flashcards-code-006.json](tasks/open-flashcards-code-006.json) | Media-sourcing pipeline (open image/audio license verification + attribution) | Backlog | code | low | pr | true |
| [open-flashcards-doc-002.json](tasks/open-flashcards-doc-002.json) | Accessibility audit + guide for card templates | Backlog | writing | low | document | true |
| [open-flashcards-maint-001.json](tasks/open-flashcards-maint-001.json) | Maintenance vs Anki / genanki / CrowdAnki format drift | Backlog | maintenance | low | pr | false |

**Fan-out note:** No dimensions were fanned out. `open-flashcards-translate-001` covers multilingual extension as a single task because TASKS.md does not explicitly enumerate target languages. `open-flashcards-deck-audio-001` is a single task because TASKS.md does not enumerate specific language decks to receive audio.
