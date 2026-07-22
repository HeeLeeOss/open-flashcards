# PLAN — open-flashcards

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## Executive summary

Spaced repetition is one of the few learning techniques with strong, replicated evidence behind
it — and Anki is its most widely used open-source vehicle. Yet the public deck ecosystem has a
quiet integrity problem: the most popular shared decks are of **unknown or laundered provenance**
(textbook content, proprietary question banks, and copyrighted images re-uploaded without rights),
their **factual accuracy is unverified**, and their card-writing quality is inconsistent. A learner
who drills a deck for 200 hours and memorises a wrong fact is *worse off* than one who never studied
it — memorised errors are sticky and expensive to unlearn.

`open-flashcards` produces **clean-provenance, accuracy-reviewed, Anki-compatible spaced-repetition
decks for core curricula** — built only from public-domain and openly-licensed sources, with
per-card attribution, a published card-writing standard, and a mandatory human accuracy-review gate.
The canonical source of each deck is **version-controlled CrowdAnki JSON** (diffable, reviewable in a
PR), from which a deterministic build produces the distributable `.apkg`. Success is not "cards
generated" — it is **decks published to a channel where learners can actually install them, that a
domain reviewer (and, where stakes require, a credentialed expert) has confirmed are accurate.**

Positioning, in one line: *the trustworthy, openly-licensed alternative to the provenance-murky deck
ecosystem — sourced, reviewed, attributed, and free.*

The work is **low risk by default**, with explicit escalation. The two real hazards are (1) **factual
accuracy** — a wrong card teaches a wrong fact durably — and (2) **licensing/provenance** — flashcard
content is unusually easy to copy from copyrighted material. Both are handled by hard gates (a license
gate and a human accuracy gate), not by trusting the model. A third, narrower hazard is **high-stakes
content**: decks for medical, legal, financial, safety, or high-stakes-exam topics escalate to
**medium/high risk**, require **credentialed expert sign-off**, and carry an explicit **"study aid,
not advice"** framing; civic/citizenship content additionally must be **non-partisan and sourced to
official material.**

This document is honest about what is not yet in place: **no educator, school, or curriculum
organisation partner is secured**, and no specific learner cohort has prioritised a first subject.
Until a channel and at least one validating educator are confirmed, delivery-dependent tasks carry
`verifiedNeed = false`. M0 is a thin, manual, end-to-end slice — author one small deck from a
clearly-open source and get it published and confirmed accurate — to earn the right to scale.

## Problem & beneficiaries

**Who is helped (primary beneficiaries):**

- **Self-directed learners** — students, language learners, adult learners, and people without
  access to paid courseware — who want effective, free study material they can trust.
- **Learners in under-resourced settings** who rely on Anki/AnkiDroid precisely because it is free,
  offline-capable, and cross-platform, but who currently must gamble on deck quality and legality.
- **Teachers and tutors** who want to assign or recommend spaced-repetition practice aligned to a
  curriculum without shipping their students copyright-infringing or error-ridden material.

**Secondary beneficiaries:**

- The **open-education commons** — OER projects, Wikidata/Wiktionary, and public-domain reference
  works become more usable when their facts are turned into well-formed, attributed study material
  that points learners back to the source.

**The verified need (what is and isn't established).** The *effectiveness* of spaced repetition is
well established in the learning-science literature (the spacing and testing effects are among the
most replicated findings in cognitive psychology), and Anki's free, open, cross-platform reach is not
in dispute. What is **not** yet established for this project is a *specific* beneficiary cohort and
subject: which curriculum, for which learners, validated by which educator. The general need is real;
the *targeted* need is **TO BE SECURED**.

**The partner gap (honest).** No educator, school, NGO, or curriculum body has yet agreed to
**prioritise a subject, review for accuracy, and confirm the decks meet a real learner need**, and no
publishing channel's policy has been validated for our use. **Partner org / requestor: TO BE
SECURED.** Until at least one validating educator and one publishing channel are confirmed,
task-level `verifiedNeed` is set conservatively to `false` for delivery-dependent tasks. Securing
both is an M0 exit criterion.

## Goals and non-goals

**Goals**

- Produce **accurate, well-formed, Anki-compatible** spaced-repetition decks for core curricula.
- Source content **only** from public-domain and openly-licensed material, with per-card attribution
  and provenance recorded.
- Make decks **trustworthy**: every published card passes a human accuracy review against its source;
  high-stakes content passes credentialed expert sign-off.
- Keep decks **portable and durable**: scheduler-agnostic content (works with Anki's FSRS default and
  legacy SM-2), standard note types, stable note GUIDs so learners can update a deck *without losing
  their own scheduling progress*.
- Publish to channels where learners can actually install the decks, and **measurably move coverage**
  of a chosen curriculum.

**Non-goals (constraints that define the project)**

A deck project that will "import any popular deck and clean it up" would inherit exactly the
provenance problem it exists to solve. On principle, `open-flashcards` will **not**:

- **Not** ingest, scrape, "clean up", or re-publish existing decks of unknown or copyrighted
  provenance (e.g. popular AnkiWeb decks derived from textbooks or proprietary question banks).
- **Not** copy proprietary or exam-board question banks. Exam-prep decks are derived only from
  **official open syllabi / officially released sample materials**, never from copyrighted item banks.
- **Not** generate facts the model "knows" without a citable open source — **every card is
  source-attributable** or it does not ship.
- **Not** build a hosted SaaS, an account system, a study app, or a scheduler — we ship **content**;
  the open Anki ecosystem (Anki, AnkiDroid, AnkiMobile) does the scheduling and review.
- **Not** collect learner data, telemetry, or PII; decks contain no tracking and phone home to nothing.
- **Not** provide medical, legal, financial, or safety **advice**. Such decks, where in scope at all,
  are **study aids only**, expert-reviewed, and carry a "not advice" notice.
- **Not** ship partisan, persuasive, or one-sided civic content; civic/citizenship decks are neutral
  and sourced to official material.
- **Not** bake in proprietary scheduling state or non-standard note types that break portability.

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Vanity counts ("cards generated") are explicitly **not** the
headline; **published-installable-and-confirmed-accurate** is.

| Metric | Baseline | Target (first 2 quarters post-M0) |
|---|---|---|
| Decks **published and installable** by learners (across ≥1 confirmed channel) | 0 | ≥ 6 decks covering ≥ 1 coherent subject |
| Cards **published with a domain-reviewer accuracy sign-off** | 0 | ≥ 1,500 cards |
| **Known factual errors in published cards** | n/a | **0** (hard gate; any confirmed error is a stop-the-line fix-and-republish) |
| **Mean accuracy-rubric score** (4-dimension rubric below) | n/a | ≥ 3.5 / 4 mean; nothing merged below 3/4 on any dimension |
| **Reviewer-corrected rate** (cards edited before merge) | n/a | tracked; trending down as the style guide tightens |
| **License/provenance violations** (non-open source used) | 0 | **0** (hard gate; stop-the-line incident) |
| **Cards with complete attribution** (source + license recorded) | n/a | **100%** of published cards |
| **High-stakes cards shipped without expert sign-off** | 0 | **0** (medium/high decks gated on credentialed review) |
| **Curriculum coverage** of a chosen subject (cards ÷ in-scope topics in a named standard) | measured at M0 | +X percentage points (set once baseline known) |
| Beneficiary validation (an educator or learner confirms a deck is accurate and usable) | none | ≥ 1 qualitative validation per published subject |
| Adoption signal (host-reported installs / rating), *tracked not targeted* | 0 | tracked per deck; quality, not raw installs, is the bar |

**Accuracy rubric (replaces binary "review passed").** Every card is scored 1–4 on four independent
dimensions, not a single flag:

1. **Factual fidelity** — the answer is correct and matches the cited open source (no model invention).
2. **Source-attributability** — the fact is traceable to a recorded open/PD/CC source.
3. **Atomicity & unambiguity** — one fact per card; the prompt has exactly one defensible answer
   (the *minimum information principle*).
4. **Curriculum fit** — the card teaches something the target curriculum/topic actually requires.

**Error taxonomy.** When a reviewer corrects or rejects a card, the failure is tagged with one or
more classes so we can see *how* authoring fails and tune the style guide: **wrong fact**,
**ambiguous prompt** (multiple defensible answers), **multiple-facts-per-card** (not atomic),
**unsourced claim** (no traceable open source), **formatting/cloze break** (malformed card/HTML),
**license issue** (source not verifiably open). The taxonomy mix is reported with the
reviewer-corrected rate.

**Counting "curriculum coverage" (reproducible).** Coverage is `cards-or-topics-covered ÷ in-scope
topics` for a **named curriculum/standard snapshot** at a recorded date (e.g. a published topic list
for a subject). The denominator is the count of in-scope topics in that snapshot; "covered" means ≥1
published, accuracy-signed-off card mapped to that topic via its standards tag. Baseline and every
delta use the same snapshot definition so they are comparable over time.

Percentage-point coverage targets are deferred until M0 measures a real baseline against a chosen
standard — inventing one now would be dishonest.

## Scope

**In scope**

- Authoring **atomic, well-formed cards** (Basic, Basic-reversed, and Cloze note types) for core
  curricula: e.g. vocabulary/language, math facts, science terminology, geography, history dates and
  events, and similar fact-dense subjects well suited to spaced repetition.
- Per-card **license + attribution + provenance** capture.
- A published, versioned **card-writing & sourcing style guide**.
- **CrowdAnki JSON** as the canonical, version-controlled deck source; a deterministic **`.apkg`
  build**; openly-licensed media (images/audio) where used, separately license-verified.
- **Validation** (schema + linter: cloze integrity, empty fields, duplicate/missing GUIDs, missing
  attribution, malformed HTML) and **publishing adapters** for ≥1 channel.
- **Standards/curriculum tagging** for discoverability and coverage measurement.

**Out of scope**

- Ingesting/scraping/re-publishing existing decks of unknown or copyrighted provenance (hard exclusion).
- Copying proprietary or exam-board question banks (hard exclusion).
- Building a study app, scheduler, account system, sync server, or hosted SaaS.
- Collecting learner data, telemetry, analytics, or any PII.
- Medical/legal/financial/safety **advice** (only expert-reviewed *study aids*, with "not advice"
  framing, if in scope at all).
- Partisan or persuasive civic content.
- Non-standard/custom note types or scheduler state that breaks cross-app portability.
- Audio/TTS generation as a primary deliverable (a future extension; only openly-licensed audio if used).

## Solution approach & architecture

This is a **content/data pipeline** project with supporting **build + adapter code**, run in the
**donated lane**: a human runs their own agent interactively to author and review cards; Hee-Lee Oss
prepares the per-deck task workspace and opens the PR. The CLI never runs an agent headless and never
authenticates a coding agent.

**Pipeline (per deck / per card batch)**

1. **Select & verify source.** Pull facts from an approved open/PD/CC source. Confirm the license is
   PD or CC-* (or otherwise open and reuse-permitting). Record canonical URL, license (SPDX-style id),
   author/attribution, and source identifier. *If license is unclear → drop; do not ingest.* Existing
   third-party decks are **never** a source.
2. **Author cards.** The agent converts source facts into **atomic** cards following the style guide
   (minimum information principle; one defensible answer per prompt; cloze conventions; no leading or
   ambiguous prompts). Output is structured CrowdAnki JSON, never free-floating text. **Every card
   carries a source/attribution field.**
3. **Assign stable GUIDs.** Each note gets a **deterministic, stable GUID** so that re-imports
   *update* rather than *duplicate*, and learners can pull deck updates without losing their own
   scheduling history. GUIDs are derived from a stable key (source id + card key), not regenerated per
   build.
4. **Validate (automated linter + schema).** Reject malformed cloze, empty required fields, duplicate
   or missing GUIDs, missing attribution, broken/unsafe HTML, and non-standard note types. Validation
   is a CI gate.
5. **Human accuracy review.** A reviewer scores every card on the 4-dimension accuracy rubric against
   the cited source and tags failures with the error taxonomy. **Domain reviewer for medium**;
   **credentialed expert sign-off for high** (medical/legal/financial/safety, high-stakes exams);
   **non-partisan check for civic.** Nothing below 3/4 on any dimension ships.
6. **Build deterministically.** A reproducible build converts the reviewed CrowdAnki JSON to a
   distributable `.apkg` (deterministic output for the same input). Scheduler-agnostic: **no SM-2/FSRS
   scheduling state is baked in** — content only.
7. **Publish (additive, versioned).** A channel adapter publishes the deck (GitHub release of the
   `.apkg` + JSON source in-repo; AnkiWeb shared-deck listing where policy permits). Each deck is
   **semantically versioned** with a changelog; updates preserve GUIDs so learners upgrade
   non-destructively.
8. **Track to adoption.** Record publish status and host-reported adoption/rating; a deck is "done"
   only when published, installable, and accuracy-signed-off — and validated by a beneficiary where
   possible.

**Components**

- `style-guide/` — the published card-writing & sourcing style guide (atomicity, cloze conventions,
  prompt-disambiguation rules, sourcing & attribution rules, sensitive/high-stakes handling,
  accessibility & i18n conventions).
- `decks/` — **canonical CrowdAnki JSON** per deck (the version-controlled source of truth), plus
  per-deck `README` (source list, license, standards mapping, changelog).
- `pipeline/` — card/deck data model + schema, the **linter/validator**, license-verification rules,
  GUID assignment, and standards-tagging.
- `build/` — deterministic CrowdAnki JSON → `.apkg` builder.
- `adapters/` (Hee-Lee Oss-conformant; all channel/host-specific logic lives here) —
  - `adapters/ankiweb/` — AnkiWeb shared-deck publishing (policy-compliant).
  - `adapters/release/` — GitHub release packaging of `.apkg` + source.
- `records/` — provenance/attribution log (audit artifact, not a re-publication of sources).

**Tech stack & conventions.** TypeScript, ESM, pnpm workspaces. `.apkg` build via a maintained
open-source library (e.g. a TS `genanki`-style/`anki-apkg-export` builder, MIT) over CrowdAnki JSON
interchange; if a non-TS tool (e.g. Python `genanki`) is used it is wrapped behind a clean interface
in `build/`. Code is **MIT**; deck **content** is **CC-BY-4.0** (or CC-BY-SA / CC0 to match a source's
share-alike terms — see Data, licensing & compliance). DCO sign-off (`git commit -s`) on all code and
PRs. Changesets for user-facing changes.

**Per-card data model (CrowdAnki-aligned, draft)**

```
noteGuid          stable, deterministic GUID (re-import updates, never duplicates)
noteType          "Basic" | "Basic (and reversed card)" | "Cloze"   (standard types only)
deckPath          e.g. "Geography::Capitals::Europe"
fields            { Front, Back } or { Text, Extra } (cloze) — UTF-8, accessible semantic HTML
tags              standards/topic tags, e.g. "cc-math::3.OA", "subject::geography"
sourceUrl         canonical URL of the open/PD/CC source for the fact
license           SPDX-style id, e.g. "CC-BY-4.0" | "CC0-1.0" | "PD"
attribution       required attribution string (author/creator + source title)
mediaRefs         [ openly-licensed image/audio refs, each with its own license+attribution ]
language          BCP-47 language tag of the card content
reviewStatus      drafted | needs-expert | reviewed | rejected
rubricScores      { fidelity, attributability, atomicity, fit }  (each 1–4)
riskTier          low | medium | high
provenance        tool, model, date, reviewer (+ expert credential where high)
deckVersion       semver of the deck this card belongs to
```

**Key decisions (locked)**

- *JSON is the source of truth, `.apkg` is a build artifact.* CrowdAnki JSON is diffable and
  reviewable in a PR; the binary `.apkg` is generated, never hand-edited.
- *Stable GUIDs are mandatory.* Non-destructive learner updates depend on it; regenerated GUIDs would
  duplicate cards and destroy scheduling history.
- *Scheduler-agnostic content.* We never ship SM-2/FSRS state; the deck works with Anki's FSRS default
  and legacy schedulers, and in AnkiDroid/AnkiMobile/other readers.
- *Standard note types only.* Portability beats cleverness; custom note types break other apps.
- *Source-or-it-doesn't-ship.* Every card is attributable to an open source; no unsourced model facts.
- *Human accuracy review is non-optional.* It is the line between "helpful" and "confidently wrong",
  which for flashcards is especially corrosive.

## Data, licensing & compliance

**This is the critical, conservative section.**

**Allowed sources (only these classes).**

- **Public-domain** reference works and data (e.g. PD texts, government works that are PD, expired-
  copyright material).
- **Creative Commons** sources under CC0, CC-BY, or CC-BY-SA (share-alike obligations honoured).
- **Structured open data** with reuse-permitting licenses (e.g. Wikidata CC0; Wiktionary/Wikipedia
  CC-BY-SA with attribution and share-alike), **OER** under CC-BY/CC-BY-SA, and **official open
  syllabi / officially released sample materials** for exam-prep derivation.

**Hard license gate.** Before *any* card is authored from a source:

1. The source's license must be verified as **PD or CC-*** (or otherwise open and explicitly
   reuse/derivative-permitting). **Unknown, all-rights-reserved, non-commercial-only where it
   conflicts with our open output, or unclear → excluded.** No ingestion, no caching, no authoring.
2. The license id and required **attribution** string are recorded **per card**.
3. **Provenance** is recorded: source URL, retrieval date, tool/model used, reviewer (and expert
   credential for high-risk).
4. **Existing third-party decks are categorically excluded as sources** — most popular shared decks
   have laundered provenance; we never import, clean up, or re-publish them.
5. **Media** (images/audio) must independently be PD/CC-* with its own recorded license + attribution.

**Output licensing.** Deck **content** is **CC-BY-4.0** by default, escalating to **CC-BY-SA-4.0**
when any source carries a share-alike obligation, or **CC0-1.0** where appropriate; the chosen output
license must be **compatible with every source** in the deck (no mixing that violates share-alike or
non-derivative terms). Pipeline/build/adapter **code** is **MIT**. Each deck's `README` states its
output license and full source/attribution list.

**Privacy / PII stance.** Curricular content carries essentially no personal data — but the project
still **collects no learner data, embeds no telemetry, and stores no PII**. Decks must not phone home.
Where a card's subject involves real people, content stays factual and sourced (no private-attribute
inference); biographical cards use only published, openly-licensed material.

**Accuracy & "not advice" framing.** Cards state facts, not advice. Any deck touching medical, legal,
financial, or safety topics carries an explicit **"educational study aid, not professional advice"**
notice on the deck and is **expert-reviewed** (high risk). Civic/citizenship decks are **non-partisan**
and sourced to official material.

**Host/channel policy.** Publishing honours each channel's terms (AnkiWeb shared-deck policy, GitHub
release terms) and any rate/automation limits. We do not work around access controls.

## Quality, review & risk gates

**Risk tier: low by default, with explicit escalation** (per the good-deed definition).

- **low** — general fact-dense curricular content (vocabulary, math facts, geography, science terms,
  history dates) from clearly-open sources; standard domain review.
- **medium** — content where domain accuracy is subtle or error-prone (language translation pairs,
  scientific specifics, anything where a non-specialist could merge a plausible-but-wrong card) →
  reviewer with the relevant subject skill.
- **high** — medical, legal, financial, safety, and **high-stakes exam** content → **credentialed
  expert sign-off before merge**, plus the **"study aid, not advice"** framing. Civic content adds a
  **non-partisan** review. If no qualified expert is available, the deck is **declined**, not shipped.

**Required review before a deck/card is "done":**

1. **License & provenance check passed** (per-card source + license + attribution recorded; source is
   verifiably open; no third-party-deck provenance).
2. **Automated validation passed** (schema + linter: cloze integrity, no empty fields, unique stable
   GUIDs, attribution present, safe HTML, standard note types) — a CI gate.
3. **Human accuracy review** on the 4-dimension rubric with error-taxonomy tagging; nothing below 3/4
   on any dimension merges. Domain reviewer for medium.
4. **Expert sign-off (high risk)** — credentialed expert clears medical/legal/financial/safety/exam
   content before merge; credential recorded in `provenance`; "not advice" framing present.
5. **Non-partisan clearance (civic)** — civic/citizenship content reviewed for neutrality and sourced
   to official material.

**Definition of Shipped (project-level).** A deck is **published to a confirmed channel and
installable by learners**, every card is **accuracy-signed-off** (and expert-signed-off where high
risk) with **complete attribution + provenance**, the deck builds **deterministically** from
version-controlled JSON, and — where possible — a **beneficiary (educator/learner) has validated** it.
Generated-but-not-published, or published-but-unreviewed, ≠ shipped.

## Roadmap & milestones

Phased; each milestone has measurable exit criteria. M0 is a deliberately thin, manual, end-to-end
slice — it must prove one deck can travel from open source → reviewed → published-and-installable
before anything scales.

**M0 — Foundation & cold-start (prove one end-to-end deck).**
Goal: publish the style guide + sourcing policy, fix the data model and build, secure ≥1 publishing
channel and ≥1 validating educator, and ship one small, accuracy-reviewed deck end-to-end.

**Sequencing — channel + accuracy gates are hard.** The channel/partner-securing research task is a
**blocking prerequisite** for publishing; the accuracy review is a **blocking prerequisite** for any
merge. No deck is published until ≥1 channel is confirmed and its policy validated; no card is merged
until accuracy-reviewed. **Kill/pivot criteria:** if the time-boxed channel search confirms no channel
will accept clean-provenance, openly-licensed decks, the project **pivots** (next candidate channel)
or **stops** — it does not generate undeliverable decks.

**Candidate publishing channels (priority order, with acceptance posture):**
1. **GitHub release of `.apkg` + JSON source** — fully under our control; acceptance posture: *always
   available; the canonical home of the version-controlled source.*
2. **AnkiWeb shared decks** — largest learner reach; acceptance posture: *validate shared-deck policy
   and account terms before listing; confirm openly-licensed, attributed decks are welcome.*
3. **OER repositories / CrowdAnki import** — high-value distribution to educators; acceptance posture:
   *requires a named educator/repo partner — TO BE SECURED.*

**M0 deck-selection criteria.** The cold-start deck (~50 cards) must (a) come from a **single, clearly
open/PD source**, (b) be a **low-risk** subject (no medical/legal/financial/exam stakes), (c) exercise
**≥2 note types** (e.g. Basic + Cloze), and (d) include **≥1 card with an openly-licensed image** to
prove the media-license path. A trivial 50-card single-type deck is disallowed — it would not prove
the pipeline.

Exit criteria:
- Card-writing & sourcing **style guide v1** published (CC-BY-4.0).
- **Approved-source list + license-verification rules** finalised and applied to a sample.
- **Card/deck data model** (CrowdAnki-aligned, with provenance/attribution fields + GUID rules)
  finalised.
- Deterministic **`.apkg` build** working from version-controlled JSON.
- **≥1 publishing channel confirmed** and its policy validated; **≥1 validating educator/learner**
  identified (closes the partner gap for the first subject) — gates publishing.
- **≥1 deck (~50 cards) published and installable**, 100% accuracy-reviewed with full attribution,
  builds deterministically, and confirmed installable in Anki + AnkiDroid.
- Baseline **curriculum coverage** measured for one chosen subject per the counting method.

**M1 — Pipeline & validation (make it repeatable).**
Goal: turn the manual slice into a repeatable, CI-gated pipeline.
Exit criteria:
- **Linter/validator + CI gate** operational (schema, cloze, GUIDs, attribution, HTML, note types).
- **License-verification gate** (approved-source checker; "unknown = excluded") operational.
- **GUID-stable, non-destructive update** tooling + per-deck semver/changelog working.
- **Reviewer workflow** + accuracy rubric + error taxonomy + expert-escalation checklist documented;
  ≥2 reviewers onboarded.
- ≥1 **publishing adapter** (AnkiWeb and/or release) automated and policy-compliant.
- **≥3 decks (~300+ cards)** published via the repeatable pipeline; reviewer-corrected rate measured.

**M2 — Controlled scale (one subject, deep).**
Goal: meaningfully cover one coherent curriculum subject, with quality and provenance held.
Exit criteria:
- **≥6 decks / ≥1,500 cards** published and installable in one coherent subject.
- Zero license/provenance violations; zero known factual errors; all high-stakes cards expert-signed.
- **≥1 educator/learner beneficiary validation** of the subject's decks.
- **Standards/curriculum mapping** in place; coverage delta measured against the M0 baseline.
- Quality metrics (rubric scores, reviewer-corrected rate, error-taxonomy mix, adoption) dashboarded.

**M3 — Broaden & sustain (more subjects/languages, durable ops).**
Goal: add subjects/languages and a maintenance model.
Exit criteria:
- ≥2 additional subjects (or language localisations) onboarded with confirmed sourcing + reviewers.
- Outcome-tracking dashboard (published/installable counts, adoption, accuracy) maintained.
- Documented maintainer + reviewer rotation; format-drift maintenance plan (Anki/genanki/CrowdAnki)
  in effect.

## Work breakdown

The itemised, schema-mapped backlog lives in **`TASKS.md`** (same directory). It is organised by the
milestones above, with stable IDs (`open-flashcards-<area>-NNN`), a size/risk/reviewer table per
milestone, acceptance criteria for the key tasks, and a complete example Task JSON for the first M0
task. The defining characteristic of the production backlog is fan-out: at scale, **one deck (or one
card batch) = one task**, drawn from a milestone-scoped subject.

## Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — owns the style guide, data model, build/linter, adapters, and the
  overall accuracy + provenance bar.
- **Reviewers / rotation:** a pool of accuracy reviewers, including ≥1 per active subject with the
  relevant domain skill; rotation documented in M1.
- **Steward (last-mile owner):** owns the relationship with each publishing channel and validating
  educator, and shepherds decks to *published-installable-and-validated* — accountable for "delivered,
  not merged."
- **Partner / requestor:** TO BE SECURED — the validating educator(s)/curriculum body and the
  publishing channel(s) that prioritise and receive the decks.
- **Expert reviewers (risk tiers):** subject specialist for medium; **credentialed expert** required
  for any high-risk deck (medical/legal/financial/safety/high-stakes exam) before merge; civic decks
  add a non-partisan reviewer.
- **Beneficiary validators:** educators/learners who confirm a published deck is accurate and usable.

## Dependencies & integrations

- **Open/PD/CC content sources** — PD reference works, Wikidata (CC0), Wiktionary/Wikipedia (CC-BY-SA),
  OER (CC-BY/CC-BY-SA), official open syllabi. Each subject-license verified before use.
- **Anki ecosystem & formats** — the Anki note/card model, **CrowdAnki JSON** interchange, the `.apkg`
  package format, and Anki's **FSRS** (default) / SM-2 schedulers (content stays scheduler-agnostic).
- **Build/library dependency** — a maintained open-source `.apkg` builder (TS `genanki`-style /
  `anki-apkg-export`, MIT; or wrapped Python `genanki`). Format-drift risk tracked (see Risks).
- **Publishing channels** — **AnkiWeb** shared decks (policy-validated), **GitHub releases**, and OER
  repositories / CrowdAnki import (partner-dependent).
- **License metadata** — SPDX identifiers; Creative Commons license deeds; source-provided license fields.
- **Hee-Lee Oss platform pieces** — `packages/cli` (task workspace + PR prep, donated lane), the Task schema
  (`packages/schema`), and `adapters/` for all channel-specific code. The CLI never runs an agent
  headless and never authenticates a coding agent.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Inaccurate card teaches a wrong fact (durably memorised) | Medium | High | Mandatory human accuracy review scored on the 4-dimension rubric + error taxonomy; "source-or-it-doesn't-ship"; domain reviewer for medium, credentialed expert for high; nothing below 3/4 merges | Maintainer / Reviewers |
| Copyrighted / unknown-provenance content used (incl. ripped textbooks, existing decks) | Medium | High | Hard license gate ("unknown = excluded"); per-card source+license+attribution; **existing third-party decks categorically excluded as sources**; stop-the-line on any violation | Steward |
| No channel / validating educator secured → nothing ships or it isn't really needed | Medium | High | M0 exit requires ≥1 confirmed channel + ≥1 validating educator; `verifiedNeed=false` until secured; GitHub release is always-available fallback; kill/pivot criteria | Maintainer / Steward |
| High-stakes content (medical/legal/financial/exam) shipped without expertise | Low | High | Risk-tier escalation; credentialed expert sign-off before merge or the deck is declined; "study aid, not advice" framing; civic non-partisan review | Expert / Maintainer |
| Model invents a plausible-but-wrong fact | Medium | High | Every card must cite an open source; reviewer verifies against the source and tags *unsourced claim* / *wrong fact* in the taxonomy | Reviewers |
| Re-import duplicates cards / destroys learner scheduling history | Medium | High | Mandatory **stable, deterministic note GUIDs**; non-destructive update tooling; per-deck semver + changelog; linter rejects duplicate/missing GUIDs | Maintainer |
| Output license incompatible with a source (share-alike / NC) | Low | High | Per-card license capture; output license chosen to be compatible with *every* source (CC-BY-SA when any source is share-alike); deck README lists all sources | Steward |
| Exam-prep deck copies proprietary question bank | Low | High | Exam decks derived only from official open syllabi / released samples; copying item banks is a hard exclusion | Maintainer / Expert |
| Cards inaccessible (poor contrast, color-only, image without alt) | Medium | Medium | Style-guide accessibility rules; semantic HTML, alt-text on images, no color-only cues; linter checks | Maintainer |
| Anki / genanki / CrowdAnki format drift breaks the build | Medium | Medium | Pin + wrap the build library behind a clean interface; CI build-and-validate; maintenance task tracks format changes | Maintainer |
| Channel rejects automated/bulk publishing | Low | Medium | Honour channel policy + rate limits; validate policy before listing; metric decoupled from a single channel (GitHub release fallback) | Steward |
| Reviewer capacity becomes the bottleneck | High | Medium | Reviewer rotation; subject-scoped batches; only scale fan-out to review capacity | Maintainer |
| Duplicate / redundant authoring across parallel sessions | Medium | Low | Deterministic GUIDs + subject-scoped batches + per-deck claim so two sessions can't author the same cards | Maintainer |

## Security & privacy

- **Threat surface.** Primarily content-integrity and licensing rather than classic security: wrong or
  fabricated facts, copyright/provenance violations, and incompatible licensing. Publishing adapters
  also touch external write surfaces (AnkiWeb account, GitHub releases).
- **Secrets handling.** Any channel credentials (AnkiWeb login, GitHub token) are supplied via the
  human's own environment, **never** written into logs, receipts, records, the deck, or committed
  files (per CLAUDE.md). The donated lane means the human authenticates with their own account.
- **PII / privacy.** No learner data, telemetry, or analytics are collected; decks contain no tracking
  and phone home to nothing. Biographical content uses only published, openly-licensed material with no
  private-attribute inference.
- **Abuse / misuse prevention.** No mass unreviewed publishing; honour channel policy and rate limits;
  every deck is human-reviewed. Refuse and flag (per Hee-Lee Oss guardrails) any request to ingest
  non-open/third-party decks, copy proprietary question banks, ship high-stakes content without expert
  review, or produce partisan/deceptive content.

## Sustainability & maintenance

- **Post-delivery ownership.** Published decks live in the open Anki ecosystem and the project repo;
  the **CrowdAnki JSON is the durable, forkable artifact**, so decks outlive any one maintainer.
- **Non-destructive updates.** Stable GUIDs + semver let maintainers (and the community) correct or
  extend a deck while learners upgrade without losing scheduling progress; corrections are republished,
  with the changelog recording what changed and why.
- **Format-drift maintenance.** The maintainer keeps the build library, CrowdAnki, and `.apkg`
  compatibility current; CI build-and-validate catches breakage early.
- **Outcome tracking.** A lightweight dashboard tracks published/installable counts, host-reported
  adoption/ratings, and accuracy metrics per deck/subject over time, so impact is visible after the
  active push ends.
- **Wind-down.** If a channel closes, decks remain available via the GitHub release + JSON source; the
  provenance log records final disposition.

## Open questions

- Which **subject and learner cohort** go first, and which **educator/curriculum body** validates the
  need and accuracy? (Blocks `verifiedNeed=true` and M0 exit.)
- Which **publishing channel** is confirmed first — GitHub release only, AnkiWeb, or an OER/CrowdAnki
  partner — and what does AnkiWeb's shared-deck policy require for openly-licensed, attributed decks?
- What is the right **build toolchain** — a TS `.apkg` builder vs wrapping Python `genanki` — given
  the TypeScript/ESM convention and CrowdAnki JSON as source of truth?
- What **standards/curriculum taxonomy** anchors coverage measurement for the first subject (e.g. a
  national curriculum, Common Core, or a published topic list)?
- What **credential bar** clears the high-risk gate for each high-stakes domain, and where will those
  experts come from?
- What **per-card GUID derivation** is both stable across edits and collision-free across decks?
- Should **audio/TTS pronunciation** (openly licensed) be in scope for language decks, or a sibling
  project (overlaps `open-pronunciation-audio`)?
- What is the right **batch/deck size** to keep reviewer capacity from becoming the bottleneck?

## References

- `C:\code\hee-lee-oss\CLAUDE.md` — Hee-Lee Oss work rules, lanes, quality bar, refusal guardrails.
- `C:\code\hee-lee-oss\docs\good-deed-definition.md` — good-deed criteria and risk tiers.
- `C:\code\hee-lee-oss\packages\schema\src\schemas.ts` — Task JSON schema.
- `C:\code\hee-lee-oss\planning\ROADMAP.md` — portfolio roadmap (open-flashcards entry, Track 3).
- Anki manual — note/card model, note types, GUIDs, importing/updating, `.apkg` package format.
- FSRS (Free Spaced Repetition Scheduler) — Anki's default open-source scheduler; SM-2 legacy.
- CrowdAnki — JSON deck import/export for version control.
- `genanki` / `anki-apkg-export` — open-source `.apkg` builders.
- Creative Commons license suite (CC0, CC-BY, CC-BY-SA) and Public Domain; SPDX license identifiers.
- Wikidata (CC0) and Wiktionary/Wikipedia (CC-BY-SA) as openly-licensed structured sources.
- Spacing effect & testing effect — cognitive-psychology evidence base for spaced repetition.

---

## Appendix A — Improvements applied

Twenty-five specific improvements over a naive first draft of this plan, each **already applied** in
the body above (with a pointer to where). These exist because a "generate flashcards with an LLM"
project fails in predictable, specific ways; this is the difference between a deck dump and a
trustworthy public good.

1. **CrowdAnki JSON as the source of truth, `.apkg` as a build artifact** — diffable, PR-reviewable
   content; binaries are generated, never hand-edited. *(Solution approach; key decisions.)*
2. **Mandatory stable, deterministic note GUIDs** so re-import *updates* instead of *duplicating*, and
   learners upgrade without losing scheduling history. *(Pipeline step 3; key decisions; code-004.)*
3. **Scheduler-agnostic content (no SM-2/FSRS state baked in)** — works with Anki's FSRS default,
   legacy SM-2, AnkiDroid/AnkiMobile. *(Goals; pipeline step 6; key decisions.)*
4. **Standard note types only (Basic, Basic-reversed, Cloze)** for maximum cross-app portability;
   custom note types explicitly banned. *(Data model; non-goals; style-001.)*
5. **Per-card provenance/attribution fields in the data model** so source + license travel *inside*
   the card, not in a side file. *(Data model; "100% attribution" metric.)*
6. **A card-writing style guide enforcing atomicity / minimum-information principle** and prompt
   disambiguation — the single biggest driver of flashcard quality. *(Style guide; rubric dim. 3.)*
7. **Automated linter + schema validator as a CI gate** (cloze integrity, empty fields, duplicate/
   missing GUIDs, missing attribution, unsafe HTML, note-type check). *(code-002; review gate 2.)*
8. **A 4-dimension accuracy rubric** (fidelity, attributability, atomicity, fit) replacing a binary
   pass/fail. *(Success metrics; review gate 3.)*
9. **An error taxonomy for card review** (wrong fact, ambiguous prompt, multiple-facts, unsourced,
   formatting/cloze break, license issue) to tune the style guide from observed failures. *(Metrics.)*
10. **Hard license gate ("unknown = excluded")** before any authoring, with per-card license capture.
    *(Data/licensing; code-003; risks.)*
11. **Categorical exclusion of existing third-party decks as sources** — directly confronts the
    ecosystem's biggest provenance hazard (laundered popular decks). *(Non-goals; data/licensing.)*
12. **Separate media (image/audio) license verification** with its own attribution. *(Data model;
    licensing; deck-001 requires an open-media card.)*
13. **High-stakes escalation with credentialed expert sign-off + "study aid, not advice" framing**
    for medical/legal/financial/safety/exam content. *(Risk gates; review gate 4; research-005.)*
14. **Non-partisan gate for civic/citizenship content**, sourced to official material. *(Risk gates;
    non-goals.)*
15. **Exam-prep derived only from official open syllabi / released samples**, never proprietary item
    banks. *(Non-goals; data/licensing; risks.)*
16. **Standards/curriculum tagging** for discoverability and reproducible coverage measurement.
    *(Solution; success metrics; research-003.)*
17. **Deterministic, reproducible `.apkg` build** (same input → same output) wrapped behind a clean
    interface. *(code-001; key decisions; format-drift risk.)*
18. **CI gate: schema + linter + build must pass before merge** — quality is enforced, not hoped for.
    *(Review gates; code-002.)*
19. **Per-deck semantic versioning + changelog** enabling non-destructive corrections. *(Pipeline
    step 7; sustainability; code-004.)*
20. **Channel strategy decoupled from a single host** (GitHub release + AnkiWeb + OER/CrowdAnki) with
    an always-available fallback, so one rejection isn't fatal. *(Roadmap channels; risks.)*
21. **AnkiWeb / host policy validated before publishing** (acceptance posture, like channel-securing).
    *(research-001; dependencies.)*
22. **Accessibility built into the card standard** (semantic HTML, image alt-text, no color-only cues)
    — flashcards should not exclude screen-reader / low-vision learners. *(Style guide; linter; doc-002.)*
23. **Internationalisation-ready** (UTF-8, BCP-47 language tag, non-Latin scripts, openly-licensed
    audio path). *(Data model; style guide; translate-001.)*
24. **Privacy-respecting outcome tracking** — rely on host-reported aggregate adoption; no telemetry,
    no PII, decks phone home to nothing. *(Security & privacy; sustainability; metrics caveat.)*
25. **Kill/pivot criteria + `verifiedNeed=false` until a validating educator confirms the targeted
    need** — honest about the partner gap; we don't generate undeliverable or unneeded decks.
    *(Roadmap sequencing; partner gap; TASKS sequencing.)*

## Review sign-off

A completeness + correctness pass over both documents against the PLAN_SPEC, the Task schema, and the
Hee-Lee Oss guardrails. Issues found were fixed in place; this records the state at sign-off.

**Completeness.**
- All **17 required PLAN.md H2 sections** are present, in spec order. ✓
- TASKS.md has the required parts: schema-mapping section, one section per milestone with
  ID/Title/Type/Size/Risk/Deliverable/Depends-on/Reviewer tables, per-milestone key acceptance
  criteria + Definition of Done, a Backlog/future section, and a complete example Task JSON. ✓
- **Task count: 17 scheduled** (M0 ×7, M1 ×6, M2 ×4) + **7 backlog** = 24 well-formed tasks across
  **4 milestones** (M0–M3 in PLAN; M3 work seeds from the backlog). Within the 12–20 scheduled target
  with quality retained. ✓

**Schema correctness (validated against `packages/schema/src/schemas.ts`).**
- Example Task JSON includes every `required` field and uses only enum-valid values
  (`type=writing`, `lane=donated`, `priority=high`, `riskTier=low`, `deliverable=document`,
  `tokenEstimate=small`, `status=open`); `acceptanceCriteria` has ≥1 item; `output` is non-empty;
  `additionalProperties:false` respected (no stray fields). ✓
- No `funded` tasks, so the conditional `fundedBudgetUsd` requirement is not triggered. ✓
- Every table deliverable maps to the enum (`pr|dataset|document|translation`) — fixed one earlier
  ambiguity by routing **deck content → `dataset`** and **publish/merge → `pr`** (mirroring the
  a11y-alttext-commons convention). ✓

**Guardrail correctness.**
- License/provenance: hard PD/CC-only gate, "unknown = excluded", third-party decks categorically
  excluded, share-alike compatibility enforced, separate media verification. ✓
- Privacy/PII: no learner data/telemetry; no phone-home; no private-attribute inference. ✓
- High-stakes: medium/high escalation with credentialed expert sign-off + "not advice"; civic
  non-partisan; no proprietary question banks. ✓
- For-profit benefit: none — output is openly licensed (MIT code, CC-BY content). ✓
- Lane: donated; CLI never runs an agent headless or authenticates one (stated in Solution +
  Security). ✓

**Honesty/`verifiedNeed`.** Partner + validating-educator gap is stated plainly as **TO BE SECURED**;
delivery-dependent tasks are `verifiedNeed=false`; only channel-independent, self-evident artifacts
(e.g. the style guide) are `true`, and the example JSON reflects this with a written justification. ✓

**Corrections applied during review.**
- Disambiguated the `deck`-vs-`pub` task split so the content artifact and its publication map to
  distinct schema deliverables.
- Made the M0 channel/partner gate explicitly *blocking* in both PLAN sequencing and the TASKS M0
  note, and named GitHub release as the always-available fallback while keeping `verifiedNeed=false`
  until an educator validates need (avoids a "we can always self-publish, so need is verified" fallacy).
- Aligned milestone exit criteria in PLAN with each milestone's Definition of Done in TASKS (counts
  and gates now match: M0 one ~50-card deck; M1 ≥3 decks/~300+ cards; M2 ≥6 decks/≥1,500 cards).

**Residual open items (tracked in Open questions, not blockers to planning):** first subject + cohort;
confirmed channel + AnkiWeb policy specifics; TS-vs-Python build toolchain; standards taxonomy;
high-risk credential bar; GUID-derivation scheme. These require **human decisions** before M0 exit.

**Status:** Plan and backlog are internally consistent, schema-valid, and guardrail-compliant.
Ready for maintainer review. — *Senior staff engineer + TPM, 2026-06-28.*
