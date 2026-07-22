# Competitive & Improvement Analysis — `open-flashcards`

> Scope: rigorous review of `PLAN.md` (v0.1.0, 2026-06-28) and `TASKS.md` for the Hee-Lee Oss good-deed
> project producing open, openly-licensed, quality-reviewed, Anki-compatible spaced-repetition decks.
> Guardrails: open license; factual accuracy (SRS amplifies errors); attribution; not duplicating
> low-quality shared decks. Competitor claims are web-grounded and cited inline.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong for its class. It already internalizes the two failure modes that kill
"LLM-generates-flashcards" projects — durable memorization of wrong facts, and laundered-provenance
content — and gates both with hard, non-model checks. Below: what is correct, what is missing or
under-specified, and what is wrong.

**Strongly correct (keep):**

- **JSON-as-source-of-truth, `.apkg`-as-artifact** (CrowdAnki) is the right call: diffable,
  PR-reviewable content, reproducible builds. This is exactly how serious community decks now version
  (AnkiHub/CrowdAnki), so it is ecosystem-aligned, not invented.
- **Stable, deterministic note GUIDs** for non-destructive updates is correct and load-bearing: Anki
  keys note identity on GUID, so regenerated GUIDs duplicate cards and destroy a learner's scheduling
  history. The plan treats this as mandatory (pipeline step 3, code-004) — right.
- **Scheduler-agnostic content (no SM-2/FSRS state baked in)** is correct and forward-proof. Anki made
  **FSRS the default in v23.10 (Nov 2023)**, and FSRS needs ~20–30% fewer reviews than SM-2 at equal
  retention ([Anki FAQ](https://faqs.ankiweb.net/what-spaced-repetition-algorithm),
  [fsrs4anki](https://github.com/open-spaced-repetition/fsrs4anki/blob/main/docs/tutorial.md)).
  Shipping content only (never scheduling state) means decks work under FSRS *and* legacy SM-2 and in
  AnkiDroid/AnkiMobile — the durability claim holds.
- **The 4-dimension accuracy rubric** (fidelity, attributability, atomicity, fit) + **error taxonomy**
  is materially better than the binary "review passed" that every competitor effectively uses. Tying
  atomicity to the **minimum information principle** is textbook-correct (SuperMemo Rule 4,
  [supermemo.com](https://www.supermemo.com/en/blog/twenty-rules-of-formulating-knowledge)).
- **Hard license gate + categorical exclusion of third-party decks** is the single most important
  guardrail and is correctly framed as "unknown = excluded." This directly addresses AnkiWeb's reality:
  copyright enforcement is **user-attestation at upload + reactive takedown**, not proactive vetting
  ([AnkiWeb terms](https://ankiweb.net/account/terms),
  [forum](https://forums.ankiweb.net/t/how-is-my-shared-deck-checked-by-copyright-holders/66227)).

**Gaps / under-specified (should fix before M0 exit):**

1. **The 20 rules are under-exploited.** The plan names atomicity and the minimum information principle
   but the style guide should explicitly encode the *broader* SuperMemo rule set the rubric implies:
   avoid sets/enumerations (convert to cloze or multiple cards), avoid interference between similar
   items, optimize wording, and **Rule 18 (provide sources) / Rule 19 (date-stamp)** — which the data
   model already half-implements via `sourceUrl`/`provenance`. Recommend the style guide cite the rules
   one-to-one so reviewers and the model share a vocabulary.
   ([supermemo.guru](https://supermemo.guru/wiki/20_rules_of_knowledge_formulation))
2. **"Atomic" is asserted but not measurably defined / linted.** The linter (code-002) checks cloze
   integrity, GUIDs, attribution, HTML — but atomicity and ambiguity are left entirely to human review.
   A *card-design linter* can catch a large fraction mechanically (answer length > N tokens, multiple
   sentences/clauses in the answer, enumerations/lists in the back, "and"/"or" in a single-answer
   prompt, two-sided cloze on the same line). This is a concrete leverage point (see §5, §6, §7).
3. **Interoperability is narrower than the brief.** The brief mentions **Mnemosyne and CSV**; PLAN
   commits only to `.apkg` + CrowdAnki JSON. Since CrowdAnki JSON is the canonical source, **CSV/TSV
   and Mnemosyne XML are cheap, deterministic export targets** and materially widen reach (CSV imports
   into Anki, Quizlet, RemNote, Brainscape, Knowt, Cram). This should be an explicit (even if deferred)
   build target, not silent.
4. **FSRS is named but not leveraged as a quality signal.** Beyond "stay scheduler-agnostic," FSRS
   optimization data (from real users) is the best available *empirical* signal of card quality — cards
   with chronically low retrievability/high difficulty are often badly written. The plan could note a
   future feedback loop (host-reported, privacy-respecting) where FSRS-flagged "leech-prone" cards are
   candidates for re-authoring. Currently absent.
5. **"Avoiding low-quality AI cards" needs a positive control, not just gates.** The plan forbids
   unsourced model facts and reviews everything, but does not specify a **golden-set / regression
   harness**: a held-out set of source→ideal-card pairs the authoring prompt is measured against, so
   style-guide changes can be A/B'd. Without it, "the style guide tightens" (a stated metric driver) is
   unfalsifiable.
6. **Subject coverage / first-subject choice is deferred (correctly) but the candidate analysis is
   thin.** The plan lists subject *types* (vocab, math facts, geography, history dates) but does not
   rank them by (a) source openness, (b) error-cost, (c) reviewer availability, (d) differentiation vs
   existing free decks. A short scored shortlist would de-risk M0 (see §6 #1). Geography/astronomy/
   public-domain-language (Wiktionary, CC-BY-SA) and **Wikidata (CC0)** structured facts are the
   obvious low-risk, high-openness starting points and should be named as the leading candidates.
7. **Duplication-avoidance guardrail is internal-only.** The plan prevents *parallel-session* dup
   authoring (GUIDs + claims) but the brief's "not duplicating low-quality shared decks" also implies
   an **external** check: before authoring deck N, confirm a high-quality, license-clean equivalent
   does *not* already exist (e.g. an existing CC0 deck), so effort goes to genuine gaps. Not currently a
   gate.
8. **Minor:** "Basic (and reversed card)" auto-reverse can *break* the minimum-information principle for
   asymmetric facts (a definition that is not reversible). The style guide should caution that reversed
   cards are only for genuinely bidirectional pairs (term↔translation), not for one-directional facts.

**Correctness verdict:** No correctness *defects* found in the guardrail logic; the schema/guardrail
self-review in Appendix A is credible. The shortfalls are *completeness* gaps — chiefly (a) atomicity
left unlinted and (b) narrower-than-briefed interoperability (Mnemosyne/CSV) — both fixable cheaply.

---

## 2. Competitive landscape (web-grounded)

| Player | What it is | Strengths | Weaknesses (our wedge) |
|---|---|---|---|
| **AnkiWeb shared decks** | Free deck-sharing host for Anki | Largest open reach; free; FSRS-ready; the install target we want | Copyright = upload attestation + reactive takedown, **no proactive quality/provenance vetting**; quality wildly inconsistent; popular decks often textbook-derived ([terms](https://ankiweb.net/account/terms), [forum](https://forums.ankiweb.net/t/how-is-my-shared-deck-checked-by-copyright-holders/66227)) |
| **The AnKing (medical)** | Community-maintained USMLE megadeck | Gold standard for curation: ~30k+ cards, real-time updates via AnkiHub, 100k+ students, tag discipline ([ankihub](https://www.ankihub.net/step-deck), [studycardsai](https://studycardsai.com/blog/best-anki-decks-step-1)) | A **consolidation of Zanki/Brosencephalon/Lolnotacop with First Aid-derived provenance** — exactly the laundered-source pattern we exclude; single high-stakes domain; not openly licensed |
| **Quizlet** | Mass-market web flashcards | Huge content library; low friction; AI generator | Weak SRS (74% day-14 retention vs Anki 89%); **aggressive paywalling** of formerly-free modes drove boycotts and a **1.4/5 Trustpilot** ([laxuai](https://laxuai.com/blog/best-spaced-repetition-apps-2026), [pawebpage](https://pawebpage.com/3269/opinions/quizlets-paywall-proves-that-students-are-its-last-priority/)) |
| **Memrise** | Language-learning app | Polished official courses, video/immersion | **Killed user-generated/community courses** in the app (moved off-site Mar 2024, sunset through 2025) — abandoned the open-content commons ([memrise](https://www.memrise.com/blog/changes-to-the-memrise-app), [eurolinguiste](https://eurolinguiste.com/memrise-is-doing-away-with-user-generated-content-now-what/)) |
| **Brainscape** | Confidence-based flashcards | **Expert-curated, high-quality pre-made decks**; clean UX | Proprietary confidence loop (not true SRS); paid; closed content ([aiflowreview](https://aiflowreview.com/quizlet-vs-anki-vs-brainscape/)) |
| **RemNote** | Notes + SRS | SM-2 *and* FSRS; cards-from-notes; strong for self-authoring ([notigo](https://notigo.ai/blog/best-flashcard-apps-students-anki-remnote-quizlet-2025)) | Closed platform; quality is whatever the user writes; not a curated-content source |
| **Knowt** | Free AI study tool (Quizlet alt) | Free; AI flashcards from PDFs/notes; positioned as the anti-paywall Quizlet | **~90% AI accuracy → ~10% wrong**, "good but not flawless," misses nuance, weak SRS — the precise error-propagation hazard we gate against ([tutorai](https://tutorai.me/blog/knowt-ai-review/), [fritz](https://fritz.ai/knowt-ai-review/)) |
| **Cram.com** | Free web flashcards | Free; large user library | UGC of unknown quality/provenance; basic SRS; ad-supported |

**Synthesis.** Two structural openings: (1) **the open end is being abandoned** — Memrise killed
community content, Quizlet paywalled, leaving Anki/AnkiWeb as the open commons but with *no quality or
provenance layer*; (2) **the quality-curation playbook is proven but closed/laundered** — AnKing and
Brainscape prove learners value expert-curated decks, but neither is license-clean *and* open. No
incumbent occupies "open license + verified provenance + audited card design + accuracy sign-off."

---

## 3. Gaps we can fill

1. **Provenance-clean decks in an ecosystem where provenance is unenforced.** AnkiWeb relies on
   self-attestation; AnKing's lineage traces to textbook-derived community decks. A deck with
   **per-card source + SPDX license + attribution** is genuinely novel.
2. **Audited card *design*, not just facts.** Every competitor reviews (at best) correctness; none
   publishes a card-design standard with a 4-dimension rubric and a machine linter for atomicity.
3. **License-clean structured-knowledge decks.** Wikidata (CC0) and Wiktionary (CC-BY-SA) are vast,
   openly-licensed, machine-readable fact sources that almost no curated deck systematically exploits
   with attribution.
4. **The "anti-laundering" niche.** By *refusing* to ingest popular decks, we produce the only decks an
   institution (school, NGO, library) can adopt without legal risk — a real procurement gap.
5. **Durable, forkable public good.** CrowdAnki JSON + CC-BY content outlives any host; competitors'
   value evaporates if the company paywalls (Quizlet) or pivots (Memrise).
6. **Accuracy floor for AI-assisted cards.** Knowt ships ~10% wrong by its own framing; our hard human
   accuracy gate + "source-or-it-doesn't-ship" is a category the AI-flashcard wave structurally lacks.
7. **Accessibility & i18n as first-class.** Alt-text, semantic HTML, no color-only cues, BCP-47 tags —
   absent from essentially all shared-deck ecosystems.

---

## 4. Differentiators to win

1. **Verifiable provenance per card** — source URL + SPDX license + attribution travel *inside* the
   note. Nobody else does this; it is the brand.
2. **Published card-design standard + a card-quality linter** — quality is enforced mechanically and
   transparently, not asserted.
3. **License-clean by construction** — institution-adoptable; zero textbook/question-bank lineage.
4. **Scheduler-agnostic + GUID-stable** — install once, upgrade forever without losing progress; works
   under FSRS and SM-2 and across Anki/AnkiDroid/AnkiMobile.
5. **Outcome-defined "done"** — published, installable, accuracy-signed-off, beneficiary-validated;
   not "cards generated."
6. **Open & free in a paywalling market** — directly counter-positions Quizlet's backlash and Memrise's
   retreat.

The **single strongest differentiator**: *per-card verifiable provenance + license cleanliness* — it is
defensible (laborious to fake), institutionally valuable, and structurally impossible for the
laundered-provenance incumbents to copy without rebuilding their libraries.

---

## 5. Claude API leverage — and the hard limits

**Where Claude is genuinely strong (assistive, human-gated):**

- **Drafting atomic cards from a verified source.** Given a license-cleared passage/Wikidata record,
  Claude is excellent at decomposing it into minimum-information cards (the existing **Anki MCP server**
  already does exactly this with a "twenty_rules" prompt that splits e.g. photosynthesis into atomic
  cards — [ankimcp.ai](https://ankimcp.ai/blog/v0.6.0-twenty-rules-prompt/)). This validates the
  approach and gives us prior art to exceed.
- **Card-design linting (style critic).** Claude can flag non-atomic answers, ambiguous prompts,
  enumerations, interference between near-duplicate cards, missing alt-text, and bad cloze — as a
  *reviewer aid* that pre-sorts the queue, raising human-reviewer throughput.
- **Format conversion & normalization.** CrowdAnki JSON ↔ CSV/TSV ↔ Mnemosyne XML, HTML cleanup,
  consistent field structure — deterministic, low-risk, high-value.
- **Provenance/attribution drafting** — proposing the SPDX id + attribution string from a source page
  for *human verification*.
- **Curriculum/standards tagging suggestions** mapped to a named standard snapshot.

**Where Claude must NOT decide (hard gates — humans/experts own these):**

- **Factual accuracy.** No card ships on the model's say-so. A human reviewer verifies every fact
  *against the cited open source*; high-stakes content requires **credentialed expert sign-off**. The
  Knowt ~10%-wrong precedent is the cautionary baseline.
- **License/permission verdicts.** Whether a source is reusable is a human/legal determination
  ("unknown = excluded"); Claude may *surface* a candidate license but never *clear* one.
- **"Is this card well-designed enough to merge."** The linter and Claude pre-sort; the rubric
  sign-off (nothing <3/4 on any dimension) is human.
- **No fabricated facts, ever.** Every card must trace to a citable open source; an unsourced model
  "fact" is a taxonomy failure (`unsourced claim`), not a card.
- **Expert/non-partisan/"not advice" judgments** for medical/legal/financial/safety/civic content.

**Top 3 Claude-API ideas:** (1) source→atomic-card drafter constrained to verified sources and emitting
CrowdAnki JSON with provenance fields; (2) a card-design/atomicity linter run as a CI pre-review and as
a standalone reusable tool; (3) bidirectional format converter (CrowdAnki↔CSV↔Mnemosyne) plus
attribution/standards-tag drafting.

---

## 6. Ten concrete optimizations

1. **Score and name the first subject now.** Rank candidate subjects by source-openness × low
   error-cost × reviewer-availability × differentiation. Lead candidates: Wikidata-CC0 structured facts
   (geography/astronomy/elements), and PD/CC-BY-SA language vocab (Wiktionary). De-risks M0.
2. **Ship a card-design linter (atomicity/ambiguity), not just a schema linter.** Mechanize: answer
   length caps, enumeration/list detection in backs, multi-clause prompts, interference between
   near-duplicate notes, reversed-card sanity for asymmetric facts. (New code-002 sub-scope.)
3. **Add CSV/TSV + Mnemosyne XML export targets.** Cheap deterministic builds off the canonical JSON;
   widens reach to Quizlet/RemNote/Brainscape/Knowt/Cram refugees — directly serves paywall-fleeing
   learners.
4. **Build a golden-set regression harness.** Held-out source→ideal-card pairs to A/B the authoring
   prompt and *prove* the style guide is tightening (makes a stated metric falsifiable).
5. **Encode the full 20 rules in the style guide, mapped 1:1 to the rubric & linter**, so model,
   linter, and reviewer share one vocabulary.
6. **Add an external-duplication pre-check.** Before authoring a deck, confirm no high-quality,
   license-clean equivalent already exists — spend effort on real gaps (honors "don't duplicate").
7. **Pilot the AnkiHub/CrowdAnki collaborative-update path** as a candidate distribution channel; it is
   how the best decks (AnKing) now sustain real-time, versioned, community maintenance.
8. **Define the GUID derivation explicitly** (e.g. `sha1(sourceId + "::" + cardKey)` namespaced per
   deck) and lint for collisions — currently an open question that blocks code-004.
9. **Capture a privacy-respecting FSRS/host quality signal.** Use host-reported aggregate
   leech/retention data (no PII) to nominate chronically-failing cards for re-authoring.
10. **Publish a one-page "trust label" per deck** (sources, licenses, reviewer, rubric mean,
    last-verified date) — turns provenance into a visible, marketable artifact and a procurement asset.

---

## 7. Parallel & perpendicular spin-offs

**Reusable infrastructure (perpendicular — serves the whole Hee-Lee Oss portfolio):**

- **`card-quality-linter`** — a standalone, open library/CLI implementing the atomicity/ambiguity/
  provenance/accessibility checks. Usable by *any* deck project and publishable as its own public good.
- **An Anki/flashcard MCP server** — exposing source→card drafting, card-design linting, and
  format conversion as MCP tools (extending the prior-art [Anki MCP](https://ankimcp.ai/) with
  *provenance enforcement* and the 4-dimension rubric, which it lacks). Lets any agent author
  guardrailed decks.
- **A provenance/attribution metadata schema** (per-card SPDX + source + license) reusable by any
  Hee-Lee Oss open-content project (alt-text commons, datasets).

**Parallel good-deed ties (shared pipeline, different subjects/cohorts):**

- **civics-exam-prep** — civic/citizenship decks from *official, non-partisan* open sources; reuses the
  high-stakes + non-partisan gates verbatim. Strong natural fit.
- **numeracy-from-zero** — math-fact decks (atomic, low-risk) for foundational numeracy; ideal early
  fan-out subject.
- **open-coding-curriculum** — syntax/API/concept cards from openly-licensed docs (CC-BY); pairs with a
  coding curriculum's spaced-recall layer.
- **oncology-data-literacy** — *high-risk* study-aid decks (terminology/data-literacy, "not advice,"
  credentialed-expert sign-off); the canonical test of the high-stakes pathway (research-005).
- **open-pronunciation-audio** — supplies openly-licensed audio for language decks (already flagged as a
  sibling in PLAN open questions).

---

## 8. Open questions

The plan's own open questions (first subject/cohort; confirmed channel + AnkiWeb policy specifics;
TS-vs-Python build toolchain; standards taxonomy; high-risk credential bar; GUID derivation; audio
scope; batch size) all stand. Additional ones surfaced by this analysis:

1. **Atomicity enforcement boundary** — how much card-design quality can be linted mechanically vs must
   stay human? Where is the precision/recall line that keeps the linter from blocking good cards?
2. **Interoperability commitment** — are CSV and Mnemosyne in scope (brief implies yes; PLAN omits)? If
   not, why not, given they are near-free off the canonical JSON?
3. **AnkiHub as a channel** — does its collaborative-update model fit our provenance discipline, or does
   its lineage to laundered decks make it unsuitable as anything but a distribution endpoint?
4. **External-duplication policy** — what counts as "an adequate license-clean deck already exists" such
   that we *should not* author a competing one?
5. **FSRS feedback loop** — is a privacy-respecting host-reported quality signal feasible, or does it
   risk implying telemetry the plan forbids?
6. **Golden-set ownership** — who curates and updates the regression set, and does it become its own
   reviewed artifact?

---

### Sources

- AnkiWeb terms / copyright process: https://ankiweb.net/account/terms ·
  https://forums.ankiweb.net/t/how-is-my-shared-deck-checked-by-copyright-holders/66227
- FSRS vs SM-2 / Anki default: https://faqs.ankiweb.net/what-spaced-repetition-algorithm ·
  https://github.com/open-spaced-repetition/fsrs4anki/blob/main/docs/tutorial.md
- 20 rules / minimum information principle: https://www.supermemo.com/en/blog/twenty-rules-of-formulating-knowledge ·
  https://supermemo.guru/wiki/20_rules_of_knowledge_formulation
- AnKing lineage & scale: https://www.ankihub.net/step-deck · https://studycardsai.com/blog/best-anki-decks-step-1
- Quizlet SRS/retention & paywall backlash: https://laxuai.com/blog/best-spaced-repetition-apps-2026 ·
  https://pawebpage.com/3269/opinions/quizlets-paywall-proves-that-students-are-its-last-priority/
- Memrise dropping community content: https://www.memrise.com/blog/changes-to-the-memrise-app ·
  https://eurolinguiste.com/memrise-is-doing-away-with-user-generated-content-now-what/
- Brainscape/RemNote comparison: https://aiflowreview.com/quizlet-vs-anki-vs-brainscape/ ·
  https://notigo.ai/blog/best-flashcard-apps-students-anki-remnote-quizlet-2025
- Knowt AI accuracy (~90%): https://tutorai.me/blog/knowt-ai-review/ · https://fritz.ai/knowt-ai-review/
- Anki MCP server (twenty_rules + Claude): https://ankimcp.ai/blog/v0.6.0-twenty-rules-prompt/
