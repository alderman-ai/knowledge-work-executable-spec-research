# What this document is

This is an executable spec designed to be executed end-to-end in a single session, with the main session delegating to subagents when specified or otherwise appropriate according to SOP.

# Pre-flight check
<!-- Start execution here. Lifecycle: if gate 01 (draft) has not fired yet, fire it before editing
     anything. Then read this document end to end, repair mechanical defects only, and fire
     gate 02 (proof-read pre-flight). Gates are `./Working/spec-gate.sh <NN>`; the protocol is
     CLAUDE.md > Spec lifecycle. -->

**At this point the Methodology and the DoD are both empty — they have not been written yet.** Pre-flight
tests only what already exists: the Problem, the Scope, and the resources this spec depends on. Ambiguity
about *how* the work will be done is not a pre-flight failure; that is settled later, at the coherency
check. Check off the boxes below as the tasks are completed.

`<assistant_may_tick>`
- [x] Read this entire document first for context -- no skimming -- then return here immediately.
      Read end to end on 2026-08-24 at the `02 - proof-read pre-flight` state, all 166 lines.
- [x] If there's any ambiguity in the **Problem**, the **Scope**, or the **Routing and resources** -- anything that would change what gets built, or what "in scope" means -- halt and fill in any clarifying questions you have inside the **Pre-flight Q&A** section of this document -- not in the terminal. Follow the instructions in that section. Then alert the operator that questions are ready for him in this spec. Once all questions have been resolved to your satisfaction, check this box and resume pre-flight check.
      Four questions raised 2026-08-24; all four answered by the operator the same day and each judged to fully address its concern, so no `<N-a>` follow-ups were needed. **A1** — the prohibition is read strictly: new pages compose the four existing primitives, with a carve-out that the colour rule be written as an actual function. **A2** — "deployed" means a Vercel preview deployment, not production. **A3** — canon is what is visibly rendered on alderman.ai; the codebase is evidence, not authority; contradictions are recorded for a later out-of-scope cleanup. **A4** — the five brand pages only. **Q1 was regenerated after the fact** on operator instruction: it had been asked on a wrong premise (that no reusable paper-card component existed, when `PaperApp` is exactly that), and the corrected version carries the evidence that made A1 answerable. Rulings from A3 and A4 that needed judgement to apply are recorded as **D1–D4** under ASSISTANT DECISIONS.
- [x] Confirm every resource named in **Routing and resources** is reachable: the local codebase, the design-system, and the deployed site.
      Codebase `…\alderman-ai` present -- Next.js 15.5.10 / React 19 / Tailwind 3, 15 components, 9 routes, `node_modules` installed. Design-system `…\design-system` present -- `DESIGN.md`, 8 component cards, 3 foundation cards, `social-drafts/`. Deployed site https://alderman.ai -- HTTP 200 in 0.54s. The `Resources/Code-to-Human_Homepage_Example.md` sample read and audited against `app/page.tsx`.
- [x] Confirm that you have access to either an existing local host server to render the images, or that you have the ability to create a new one
      Created one and verified it: `npm run dev` in the codebase, up in ~3s, `http://localhost:3000` served HTTP 200 and rendered the homepage. Stopped cleanly afterwards by port (not by process name -- killing all `node` would take out the session). Node v24.15.0, npm 11.12.1. **No writes were made to the codebase.**
`</assistant_may_tick>`

<!-- Once every box in this section is ticked and Pre-flight Q&A is fully resolved, fire
     gate 03 (post-qna) and alert the operator. Then author the DoD (gate 04), and only then the
     Methodology (gate 05). Do NOT begin executing anything yet. -->

# Pre-flight Q&A

The assistant will record all clarifying questions by the assistant and answers by the operator verbatim. Use the list entry template inside the fence below for all list entries in this section.

`<list_entry_template>`
```
## Q<N>. <Brief summary of question for operator>?

"<full context of question such that the operator does not need to look up anything in another document or in the terminal to understand>"

### A<N>
`<operator_response>`
**Response:**

- [ ] Answer submitted
`</operator_response>`
```
`</list_entry_template>`

If you reload the SPEC and any Answer submitted boxes are ticked, judge whether:
- The answer fully addresses your concerns -- in that case consider that question complete
- Does not fully address your concern -- in that case add an additional list entry template block using `<N-a>` (followed by `<N-b>`, etc., if needed)

**Once all questions here are answered to your satisfaction, resume the pre-flight check, ticking the appropriate box**
<!-- list goes directly below here -->

## Q1. Does "no new web components" forbid anything this spec actually needs?

"The Problem's third bullet -- one shot from MD to a full working page -- needs a vocabulary of blocks that the renderer can turn into real code. The question is whether that vocabulary can be built from components that already exist, because 'Creating any new web components' is out of scope.

**What the five in-scope brand pages are actually built from** (chrome every page carries -- `PageFrame`, `FloatingNav`, `SideNav`, `Footer` -- omitted):

| Page | Lines | Components used |
|---|---|---|
| `/` | 130 | HeroSection, WhatYouGetSection, TrialCTASection, SectionTile x2, TerminalLine x2, Image |
| `/about` | 280 | SectionTile x3, PaperApp x3, TerminalLine x2, StackedLogo, Image |
| `/contact` | 197 | SectionTile x3, PaperApp x2, Postit, Image |
| `/faq` | 498 | SectionTile x4, TerminalLine x3, PaperApp x3, Postit x2, FaqChat |
| `/faq-download` | 244 | SectionTile x3, TerminalLine x2, Postit x2, PaperApp x2, Image |

**Only the homepage uses `components/sections/*`.** The other four -- over 1,200 lines of live, shipping brand pages -- are composed entirely from four parameterised primitives: `PaperApp`, `Postit`, `TerminalLine`, `SectionTile`. That is the existence proof that the primitives already suffice to build a whole page.

**The sample translation's vocabulary maps 1:1 onto those primitives' real prop APIs:**

- `PAPER CARD -- center, narrow` / `wide` maps to `PaperApp.width: 'narrow' | 'medium' | 'wide' | 'fit'`
- `POST-IT -- hanging off its bottom-right corner` maps to `Postit.overhang: 'br' | 'bl' | 'none'`
- `CTA TILE -- dark, purple accent` maps to `SectionTile.accent: 'purple' | 'orange' | 'green'` and `variant: 'ide' | 'app'`

So `Resources/Code-to-Human_Homepage_Example.md` is a faithful surface over components that exist, not an aspiration. Whoever wrote it read the component APIs.

**The one genuine gap** is the homepage's three legacy sections -- the Job Benefits card, the onboarding-cycle animation, the 20-Years card. All three take zero props; their copy is written directly into the JSX. The sample MD already handles this: it writes them as `### + ANIMATION: onboarding-cycle (code-owned)` with 'Editable text only' -- a named, page-specific block whose text is editable but whose structure is not. No new component required, and the homepage still round-trips.

**The one thing that genuinely does not exist** is the colour rule. The sample states colours are automatic -- 'type COMPANY in caps and it comes out orange' -- and I verified the rule against the real segment data: it is exactly right, including its escape hatch. (`{g: anxiety}` is marked explicitly in the sample precisely because 'anxiety' is neither `ai` nor ALL-CAPS, so the automatic rule would not produce green there.) But no auto-colouring helper exists anywhere in the codebase: `TerminalLine` takes a hand-written `segments` array, and a human applied the rule by eye. Determinism requires writing that rule as a function.

Readings of the prohibition:

(a) **Strict** -- take it literally. New pages compose the four existing primitives; the homepage's legacy blocks stay code-owned and page-specific.
(b) **Permissive** -- extract and parameterise new components wherever it would reduce duplication.
(c) **Props-only** -- add props to existing components, but never add new component files.

**My recommendation: (a).** The strict reading turns out to be free -- four of the five in-scope pages already work exactly this way -- and it keeps the homepage's legacy bespoke markup from leaking into the vocabulary, which is the same mess flagged in A3. The tradeoff being accepted: code-owned blocks are page-specific and cannot be reused when authoring a *new* page. New pages get the primitives only, which is desirable anyway.

**One carve-out to confirm:** the colour rule must be written as an actual function (text to coloured segments). It is a text utility rather than a web component, so I read it as permitted under all three readings -- but it is new code in the codebase, so I would rather have it confirmed than assume it."

### A1
`<operator_response>`
**Response:**
A, with a carve out that the colour rule needs to be written as an actual function (text → coloured segments), since today a human applies it by hand.
- [x] Answer submitted
`</operator_response>`

## Q2. Does "deployed" mean live on alderman.ai, or built and previewed?

"The Problem says 'Convert a completed one of these intake templates into a full working **deployed** web page in a single shot', and the Scope repeats 'completes a one shot from MD to deployed web page'. The codebase is a Next.js 15 app with a `.vercel` directory present, so 'deploy' plausibly means publishing to production at https://alderman.ai -- which I confirmed is live and serving (HTTP 200).

The tension: a genuine 'single shot' with no human gate would publish to a live public website automatically. That is outward-facing and hard to undo. The project `CLAUDE.md` currently gates production deploys on your explicit confirmation, which contradicts 'single shot' if single shot means live.

Readings:

(a) **Production, no gate** -- the MD lands the page on the live site in one shot.
(b) **Production, gated at the last step** -- everything up to the deploy is one shot; the deploy itself pauses for your yes.
(c) **Preview deployment** -- a real, shareable Vercel URL that is not production.
(d) **Local only** -- build and render on localhost; deploying is out of scope for this spec.

**My recommendation: (c), with (b) as the fallback.** A preview deployment gives you a genuinely deployed page to judge -- which is what proves the translator works -- without an unreviewed page appearing on your live site. If you want (a), say so explicitly and I will remove the deploy gate from `CLAUDE.md` rather than leave two documents contradicting each other."

### A2
`<operator_response>`
**Response:**
C
- [x] Answer submitted
`</operator_response>`

## Q3. Is the lexicon's source of truth the codebase, or the design-system's DESIGN.md?

"Two candidate sources disagree, and the Problem requires 'an agreed upon schema and lexicon', so which one is authoritative changes what gets built.

`design-system/DESIGN.md` is a named component reference paired with eight HTML preview cards: faq-chat, floating-nav, footer, page-frame, paper-app, postit, section-tile, terminal-line. The codebase has fifteen components, and its three **page-composition** sections -- HeroSection, WhatYouGetSection, TrialCTASection -- have no cards and no entry at all. So the design system carries no vocabulary for the level at which pages are actually assembled.

DESIGN.md also carries its own staleness banner -- 'SYNC STATUS (2026-08-14) ... the cards were never re-audited' -- while declaring, in the same file, 'Source of truth: the LIVE site at alderman-ai/'.

If the lexicon is generated from DESIGN.md it will be missing the section-level vocabulary the intake template needs. If it is generated from the codebase, it will contain names the design system never blessed, and the two drift further apart.

Related, and part of the same ruling: 'Making changes to the brand or design system' is out of scope. Does updating DESIGN.md so that it matches the codebase count as a forbidden change to the design system, or as documentation maintenance?

**My recommendation: the codebase is the source of truth for the lexicon; DESIGN.md is a consumer of it, and bringing it back into sync is documentation maintenance rather than a design-system change.** DESIGN.md says as much about itself. I would rather have this as an explicit ruling than assume it, because it decides whether the lexicon may name anything that has no design-system card."

### A3
`<operator_response>`
**Response:**
Unfortunately, the code base is a bit messy with superseded artifacts not labeled as such. Please highlight individual contradictions visually rendered on a local host server and I can verify which is live and which is superseded. During this process, make a list of these so I can take it to clean the code base (the cleaning is out of scope for this project). The only true canon is what's visibly present on alderman.ai. But please don't take 30 screenshots to verify everythign ...
- [x] Answer submitted
`</operator_response>`

## Q4. Which URLs are in scope for the import direction?

"The first bullet of the Problem is 'Take a URL of a web page, and convert it into an easy to read and/or edit MD file.' Two things are unspecified.

**Which pages of the site.** There are nine routes. Five are brand pages: `/`, `/about`, `/contact`, `/faq`, `/faq-download`. Four are internal dev tools under `/dev`: `/dev/getsales/carousel`, `/dev/getsales/carousel/export/[id]`, `/dev/paper-template`, `/dev/social`. The dev tools are built from a different vocabulary -- they have their own local `PaperCard` inside `_lib/` folders that the brand site does not use -- so including them would roughly double the lexicon and pull in components that are not part of the brand.

**Whose pages.** 'A URL of a web page' could mean any URL on the internet -- importing an arbitrary third-party page and re-rendering it in your design system -- or only pages of alderman.ai. These are very different problems: the second is a faithful round-trip of code you own; the first is an interpretation of markup you do not control, and cannot be deterministic in the way the Problem demands.

**My recommendation: the five alderman.ai brand pages only, with `/dev/*` and third-party URLs both out of scope.** If you do want arbitrary URLs, that is a substantially larger piece of work, and the '100% deterministic' requirement would need rewording -- determinism cannot be guaranteed against markup you do not own."

### A4
`<operator_response>`
**Response:**
Omg what a mess. Just the first 5
- [x] Answer submitted
`</operator_response>`

# Problem

We need to make a suite of skills connected to my deployed website codebase to import and export human-legible MD files connected to new or existing web pages. For example:

- Take a URL of a web page, and convert it into an easy to read and/or edit MD file
- Create an intake template for a new page
	- with a default set of sections and components
	- and a separate library of possible sections and components that I can copy and paste from
- Convert a completed one of these intake templates into a full working deployed web page in a single shot

For this to be possible, we need to create an agreed upon schema and lexicon such that:
- It's perfectly human legible during editing in Obsidian UI
- AND when uploaded is 100% unambiguous of the exact, deterministic frontend code needed to deploy it
	- The results should be 100% deterministic

## Scope

The scope is making sure all suites of skills work as intended, and the middle layer "translator" from human-legible writing surface to machine-legible deterministic code completes a one shot from MD to deployed web page.

### Not in scope

- Creating any new web components
- Making changes to the brand or design system


## Routing and resources

- We have a working, published, custom-coded website:
	- deployed at https://alderman.ai
	- Local code base: `C:\Users\alder\Desktop\Claude Code Website\alderman-ai`
- A sample of the deployed homepage translated "code-to-human"
	- Treat adversarially to see if this "translation" is sufficient
- There's a Claude design-sync available here: `C:\Users\alder\Desktop\Claude Code Website\design-system`
- Gates of this Spec can be found here: `Projects/PROJ_Custom Web Page Template/Spec Lifecycle/README.md`

## Methodology
<!-- WRITE THIS SECOND — after the DoD below is written and gate 04 has fired. The Methodology is
     authored to satisfy a DoD that is already fixed, never the other way round. Authoring it is
     gate 05.
     EXECUTE IT AFTER GATE 06 — the coherency check sits between writing this and running it, and
     asks one question: if we execute this Methodology, will that DoD be satisfied? Do not begin
     execution before that gate passes, and never before the pre-flight check is complete. -->

### M0 · Shape of the run

**One session, one orchestrator, no human in the loop.** The whole Methodology executes in a single
Claude Code session on the operator's desktop (`DESKTOP-91IG6RH`), with the main session running as
**Fable / high** and acting as **orchestrator only**: it sequences the work packages below, dispatches
Opus subagents to do the reading, building and verifying, validates what comes back, and integrates
receipts. The orchestrator never reads the codebase into its own context beyond what a receipt tells it.
Once gate 06 has fired, execution runs to gate 07 or to a halt with **no planned pause for the operator**;
anything that genuinely cannot continue without the operator goes to **Issues to resolve** as a last
resort, never as a checkpoint.

**The translator is a program, not a prompt.** DoD 5.1 (byte-identical reruns) and 5.4 (ambiguity
impossible) cannot be met by a model re-deriving JSX each time. Subagents *write* the translator; the
translator itself is a Node package that runs without a model in the loop. Every skill this spec produces
invokes that program.

**Two working areas outside the vault, both inside the fence exception:**

- **`<codebase>`** = `C:\Users\alder\Desktop\Claude Code Website\alderman-ai`, branch `main`. Read
  throughout; **`main` is never written to** by this run (5.3, 4.3, 1.3 are all diffs of `main`).
- **`<worktree>`** = `<codebase>\.rt\` — a `git worktree` of `<codebase>` on branch `rt/<yyyy-mm-dd>`,
  excluded from `main`'s status via `<codebase>\.git\info\exclude` (no tracked file edited to hide it).
  **Every generated page lands here, at its canonical route**, because `FloatingNav` and `SideNav` filter
  the current route out of the menu by `usePathname()` — a round-tripped `/about` served from any other
  path would show one nav item more than the live page and fail 5.2 on sight. Preview deployments are
  made from `<worktree>` with the project link copied in from `<codebase>\.vercel\project.json`.

**Vault-side paths this Methodology writes** (all registered in the artifact table at M7):

| Path | Holds |
|---|---|
| `Working/translator/` | the translator package: `export`, `import`, `validate`, `colour`, `compare`, `capture` commands, tests, `code-owned/` registry, its own `.gitignore` for `node_modules/` |
| `Working/receipts/WP<n>.json` | one receipt per completed work package — the resumption unit (M5) |
| `Working/inventory/`, `Working/reconciliation/`, `Working/roundtrip/`, `Working/captures/`, `Working/skill-runs/`, `Working/determinism/` | run records, one file per page or per run, named in the step that writes them |
| `Working/contradictions.md`, `Working/contradictions-compare.html` | the A3 list and the D3 single comparison page |
| `Working/run-log.md`, `Working/evidence-index.md` | appended during the run; the DoD-box → artifact map |
| `Deliverables/Schema and Lexicon.md`, `Deliverables/Intake Template.md`, `Deliverables/Block Library.md`, `Deliverables/Pages/<page>.md` ×5 | the contract and the exports |
| `Skills/export-page-to-md/SKILL.md`, `Skills/new-page-intake/SKILL.md`, `Skills/import-md-to-page/SKILL.md` | the three skills (6.1) |

### M1 · Fan-out contract

**Subagents read no context they were not handed.** Everything a subagent needs is in its dispatch prompt
or in the files that prompt names by absolute path. A subagent that finds it needs something the prompt
did not give it returns a `BLOCKED` receipt naming the gap; the orchestrator re-dispatches with the gap
filled. No subagent asks the operator anything.

- **Definitions, created before gate 06 fires** (the harness registers `.claude/agents/*.md` at the vault
  root only at process start, so a definition written during execution cannot be dispatched in the same
  session — FOLDER DIRECTORY, `Skills/`). Four — the first three **`model: opus`, `effort: high`**, the
  fourth Sonnet by D10:
  - **`webpage-builder`** — writes code and documents: the translator, the schema, the template and
    library, the skills. Receives the contract it builds against verbatim.
  - **`webpage-auditor`** — reads and judges, never writes product: inventories, reconciliations,
    contradiction verdicts, comparison verdicts, adversarial review of the schema. Receives the artifact
    under test and the standard it is tested against, and returns a structured verdict.
  - **`webpage-skill-runner`** — the fresh session DoD 6.2 asks for. Receives **only** the absolute path
    of one `SKILL.md` and that skill's runtime parameters. It did not author the skill and inherits no
    context from the session that did; that is the evidence 6.2 wants.
  - **`webpage-viewer`** — **`model: sonnet`, `effort: high`** — the only definition that drives
    **Claude in Chrome**. Operator ruling 2026-08-24 (D10): whenever a visual judgement needs eyes rather
    than a pixel diff, the page under test is served from a **local dev server** and inspected through
    Claude in Chrome, and as much of that Chrome work as possible is delegated to a Sonnet subagent.
    Receives a localhost URL (and, for comparisons, the baseline URL), what to look at, and the path to
    write its observation to; returns observations, never verdicts — the `webpage-auditor` still judges.
- **Fallback if a definition is missing at dispatch time:** the `Agent` tool with `model: "opus"` and
  the definition's body pasted as the prompt preamble. Record the fallback in the run log; do not stop.
- **Isolation granularity:** one work package per builder dispatch; **one page per auditor dispatch**
  for anything that judges a page (inventory, reconciliation, round-trip comparison) — five pages judged
  in one context would anchor on each other. Reason: measurement integrity, not cost.
- **Shared material travels in the prompt, uncompressed:** the schema document (once it exists), the
  relevant DoD boxes verbatim, the prohibitions from M2 verbatim, and the exact paths to read and write.
- **Output shape, fixed:** every dispatch returns a JSON receipt `{wp, status: DONE|BLOCKED|FAILED,
  wrote: [paths], summary, blockers: []}`; the orchestrator validates the receipt and checks every
  `wrote` path exists before it counts the package done. A malformed receipt is a re-dispatch, not a
  repair by hand.
- **Concurrency:** builders run one at a time (they share the translator); auditors may run five in
  parallel (one per page). Nothing runs in parallel against `<worktree>`.

### M2 · Prohibitions and halts — restated so they are salient at the moment of action

**Prohibitions** (verbatim from Not in scope, `CLAUDE.md`, and the rulings; a subagent receives these in
every dispatch that touches the codebase):

- Creating any new web components. **No file is added under `<codebase>/components/` in `main` or in
  `<worktree>`.** The colour rule is a function inside the translator, resolved at generation time into
  static `<span>`s and `segments` arrays — nothing new runs in the browser (A1 carve-out, D6).
- Making changes to the brand or design system. **`<design-system>` is read-only, without exception**
  (D2). Discrepancies are recorded to `Working/contradictions.md`, never fixed.
- **Deploying to production is operator-gated.** Every `vercel deploy` in this Methodology is a
  **preview** deployment (`vercel deploy` with no `--prod`; A2). If a command would promote to production
  it is not run.
- **`main` of `<codebase>` is not written to.** Generated pages go to `<worktree>` only.
- **Never kill the dev server by process name** (`taskkill //IM node.exe` would kill the session). Kill
  by port.
- **Only the five in-scope pages** (A4). Nothing else on the site is inventoried, exported or compared.

**Halts — write the Issue, stop, alert.** These are the only planned exits to the operator:

- **H1 · A block visible on a live in-scope page cannot be expressed by the four primitives plus a
  code-owned fragment.** That is the one outcome A1's strict reading cannot absorb; the Issue names the
  block, the page, and the smallest reading of A1 that would admit it.
- **H2 · `vercel` authentication or the live site is unavailable** after one retry.
- **H3 · A round-trip page still shows a named visual difference after the M4 loop cap** (three repair
  iterations). The Issue carries the comparison record and the pixel-diff image.
- **H4 · The codebase's `main` changed during the run** (HEAD differs from the SHA recorded at WP0).
  The operator is cleaning cruft in parallel; a moving baseline invalidates every diff-based box, so
  the run halts rather than silently re-baselining.

**Not halts** — handle, record in the run log, continue:

| Looks like | Actually is | Do this |
|---|---|---|
| Production deployment SHA ≠ local HEAD at WP0 | Operator's cleanup not yet deployed | Use the baseline-preview rule in WP6; record in Observed state |
| `TerminalLine` typing animation mid-capture | Timing, not content | Capture only after the settle wait in WP6 |
| Obsidian window partly off the primary monitor | Window geometry | Capture the window rectangle, not the screen (the 2026-08-24 spike showed this) |
| `next build` warning that also appears on an untouched `main` build | Pre-existing | Record it; only *new* warnings count against 4.2 |
| A subagent returns `BLOCKED` | A gap in the dispatch, not in the work | Fill the gap, re-dispatch once; a second `BLOCKED` on the same gap is FAILED |

### M3 · Work packages — executed in order, each leaving a receipt

Each package names the DoD boxes it produces evidence for. Tick a package's box only when its receipt is
validated and every path it names exists.

`<assistant_may_tick>`

- [ ] **WP0 · Baseline and tooling.** *(orchestrator, then builder)* Record `<codebase>` HEAD SHA and a
      clean `git status` to `Working/receipts/WP0.json`; if not clean, halt (H4 applies from here).
      Confirm `vercel whoami` and the project link. Create `<worktree>` on branch `rt/<yyyy-mm-dd>` and
      add `.rt/` to `<codebase>/.git/info/exclude`. Builder scaffolds `Working/translator/` (Node,
      `package.json`, `playwright` + `pixelmatch` + `typescript` as dev dependencies, `npx playwright
      install chromium`, `node --test` wired) and proves it with a smoke test that launches Chromium
      against https://alderman.ai and saves one capture. Prove the Obsidian capture path: open one existing
      vault file by `obsidian://open?vault=Alderman%20OS&file=…`, capture the **Obsidian window
      rectangle** via .NET `CopyFromScreen`, save to `Working/captures/obsidian/_smoke.png`. Start
      `run-log.md`. → *preconditions for everything below.*

- [ ] **WP1 · Canon inventory and contradiction sweep.** *(five auditors in parallel, then one auditor)*
      Per page, an auditor produces `Working/inventory/<page>.json`: an ordered list of every block
      **visible on the live page** (Playwright DOM of https://alderman.ai/<route>, viewport 1440×900 and
      390×844), each with its rendering component as found in `<codebase>/app/<route>/page.tsx`, the
      props set, and the source line range — and, for anything not attributable to one of the four
      primitives or the chrome, a `code-owned` entry with the exact source slice. Then one auditor reads
      `<design-system>/DESIGN.md`, the cards, and `<codebase>/components/` against the five inventories
      and writes `Working/contradictions.md`: one entry per disagreement, citing the artifacts, with a
      canon verdict under D1 (**visible on a live page → canon; not visible → not canon**). Entries the
      live site cannot settle are rendered side by side on **one** local HTML page,
      `Working/contradictions-compare.html`, captured once to `Working/captures/contradictions.png`; if
      none, the list says so explicitly. → **7.2, 7.3**; inputs to 1.2, 1.5, 2.3.

- [ ] **WP2 · Schema and lexicon.** *(builder, then auditor adversarially, then builder repairs)* Builder
      writes `Deliverables/Schema and Lexicon.md` from the five inventories: the block grammar (a `##`
      block heading with position/style knobs, `---` separators, `+` attachment — the sample's surface,
      kept where the inventories support it); the **coverage table** with one row per block name → component
      → props → the page and location where it is visible (1.1, 1.2, 1.3); the **colour rule as a total
      function** — input any body string, output exactly one segmentation, with the `{g:}` `{o:}` `{p:}`
      `{x:}` markers as the only escape hatches, plus a worked-example table that includes the sample's
      `{g: anxiety}` case (1.4); the **code-owned inventory** naming every fragment per page and stating
      that its body lives in `Working/translator/code-owned/<page>/<name>.tsx` under a content hash (1.5);
      and the **rejection rules** — every construct the grammar does not admit and the named reason the
      validator gives (5.4). Auditor then reads the schema **adversarially against
      `Resources/Code-to-Human_Homepage_Example.md`** as the Routing section directs: every place the
      sample's vocabulary is insufficient, ambiguous, or names something not canon is a finding; builder
      repairs the schema until the auditor's finding list is empty (cap: three rounds; residue becomes an
      Issue). → **1.1–1.5, 5.4 (specification half).**

- [ ] **WP3 · The translator.** *(builder; one dispatch per command)* In `Working/translator/`:
      **`export <route>`** — reads `<codebase>/app/<route>/page.tsx` with the TypeScript compiler API,
      matches the primitive call patterns the schema names, emits the page's MD in schema form and writes
      every unmatched fragment to `code-owned/<page>/` by hash; **`validate <md>`** — accepts or rejects
      with the schema's named reason, never guesses; **`import <md> --into <worktree> --route <r>`** —
      emits `page.tsx` for that route from a fixed per-block template, colour resolved statically by the
      `colour` function, code-owned fragments inlined verbatim by name, output byte-stable (no
      timestamps, sorted props, fixed formatting); **`colour <string>`** — the function itself, exposed
      for the worked examples; **`capture <url> <out>`** and **`compare <a.png> <b.png> <out>`** — the
      Playwright capture with the settle rule (wait for `TerminalLine` to reach its done phase or 8 s,
      whichever first; animations disabled via `prefers-reduced-motion` where the page honours it) and
      the pixelmatch diff. Tests under `node --test`: the colour worked-example table, one accepted and
      every rejected-input case from the schema (**this test output is the 5.4 evidence**), and an
      export→import identity test on a fixture. → **5.4 (evidence half); tooling for 2, 4, 5.**

- [ ] **WP4 · Export the five pages.** *(orchestrator runs the program; five auditors reconcile in
      parallel)* `export` each route to `Deliverables/Pages/<page>.md` (2.1). Open each in Obsidian via
      `obsidian://` and capture the window to `Working/captures/obsidian/<page>.png` (2.2). Each auditor
      receives one page's MD, its `Working/inventory/<page>.json`, and the schema, and writes
      `Working/reconciliation/<page>.md`: block by block, present/missing/invented, ending in a count of
      each; any non-zero missing or invented count sends the page back to WP3 for one repair pass (cap:
      two; residue is an Issue). → **2.1, 2.2, 2.3.**

- [ ] **WP5 · Intake template and block library.** *(builder, then auditor)* Builder writes
      `Deliverables/Intake Template.md` — the default section set for a new page, every slot carrying an
      inline instruction and a schema-valid placeholder — and `Deliverables/Block Library.md` — every
      lexicon block once, copy-pasteable unmodified, each with a one-paragraph *what it looks like / when
      to reach for it*. Auditor runs the **two-way check**: every block in the coverage table appears in
      the library and every library block is in the coverage table, and both files pass `validate`; the
      check's output is written to `Working/reconciliation/library-two-way.md`. Obsidian captures of both
      files to `Working/captures/obsidian/`. → **3.1, 3.2, 3.3.**

- [ ] **WP6 · Round trip.** *(orchestrator runs the program; five auditors judge in parallel)* For each
      page, `import Deliverables/Pages/<page>.md --into <worktree> --route <r>` — onto its **canonical**
      route in `<worktree>`. Run `npm run check && npm run build` in `<worktree>`. `vercel deploy` from
      `<worktree>` → one preview URL serving all five. **Baseline rule:** if the production deployment's
      commit SHA equals the WP0 HEAD SHA, the baseline is https://alderman.ai; otherwise it is a second
      preview deployment of an **unmodified** checkout of the WP0 SHA, and the mismatch is recorded in
      Observed state — either way the comparison isolates the translator from the operator's parallel
      cleanup. `capture` baseline and round-trip at both viewports, `compare` them, and hand each auditor
      one page's four images plus the diff images; the auditor writes `Working/roundtrip/<page>.md`
      naming every visible difference region or stating the list is empty. **Where a diff region needs
      eyes** (the auditor cannot classify it from the images alone, or a page has no pixel diff but the
      auditor wants to confirm behaviour such as the terminal typing or a post-it overhang), serve
      `<worktree>` locally (`npm run dev` on a port other than 3000, killed by port afterwards) and
      dispatch a `webpage-viewer` (Sonnet) to inspect that page via Claude in Chrome against the baseline
      URL, writing its observations to `Working/roundtrip/<page>-viewer.md`; the auditor's verdict then
      cites those observations (D10 — the operator approved WP6 on these terms 2026-08-24). A non-empty
      list is the M4 loop: repair the translator, re-import, redeploy, re-judge. Finally `git -C <codebase> diff
      --stat main -- app/page.tsx app/about app/contact app/faq app/faq-download` must be empty and is
      saved to `Working/roundtrip/_main-untouched.diff` (5.3). → **5.2, 5.3.**

- [ ] **WP7 · New page, one shot, and determinism.** *(builder fills the template; skill-runner
      performs the import)* Builder completes a copy of the intake template as
      `Working/skill-runs/new-page-input.md` for route `/template-demo` — **placeholder copy that is
      obviously a demonstration**, no brand claims, so a preview URL that leaks reads as a test (D9). The
      `import-md-to-page` skill (from WP8, so WP8's authoring precedes this step's run — see ordering
      note) is run by a **`webpage-skill-runner`** given only the skill path and that input; the skill
      itself performs validate → import into `<worktree>` → `npm run check && npm run build` → `vercel
      deploy` → returns the preview URL, and writes `Working/skill-runs/import-md-to-page.md` as the run
      record: one invocation, the commands it ran, no manual step (4.1, 4.2, 6.2 for this skill). `git
      -C <worktree> diff --stat main` saved to `Working/skill-runs/new-page.diff` must show additions only
      under `app/template-demo/` (4.3). `capture` the preview URL and save alongside the input MD as
      `Working/captures/template-demo.png` (4.4). **Determinism:** run `import` on the same input twice
      more into two fresh temporary directories, hash every generated file with SHA-256, and write the
      matching-hash table to `Working/determinism/template-demo.md`; do the same once for one exported
      page. → **4.1–4.4, 5.1.**

- [ ] **WP8 · The three skills.** *(builder; authored right after WP3, run at WP7 and WP9)* Under the
      skill authoring contract: runtime parameters not baked values (route, input path, target worktree,
      output path); undeployed Markdown, read and followed; stated input and output shape; a worked
      example per mode; failure behaviour is halt with the validator's reason. `export-page-to-md`
      (route → MD + Obsidian capture), `new-page-intake` (→ a fresh copy of the template plus the
      library, at a caller-named path), `import-md-to-page` (MD → preview URL, as WP7 describes). Each
      names `Working/translator/` by absolute path and `Deliverables/Schema and Lexicon.md` as the
      contract. → **6.1.**

- [ ] **WP9 · Fresh-session runs of the remaining two skills.** *(two skill-runners, sequential)* Each
      receives only its skill path and parameters: `export-page-to-md` on `/contact` to
      `Working/skill-runs/export-contact.md`, then diffed against `Deliverables/Pages/contact.md` (must be
      identical — a second determinism datum); `new-page-intake` to `Working/skill-runs/intake-out/`.
      Each writes its run record to `Working/skill-runs/<skill>.md`. → **6.2.**

- [ ] **WP10 · Close.** *(orchestrator)* Diffs: `git -C <codebase> diff --stat <WP0 SHA> -- components/`
      (must be empty → 1.3, 4.3 for `main`); `git -C <design-system> status --short` and `diff --stat`
      against its state at WP0 (must be empty → 7.1). Write `Working/evidence-index.md`: one row per DoD
      box → the artifact(s) that evidence it. Walk the DoD and tick every box whose evidence is present;
      any box that cannot be ticked becomes an Issue and the run halts there. Fill **Observed state**
      (M6). If every box is ticked and Issues is empty, fire gate 07 and alert the operator that Final
      DoD Confirmation is ready, with the preview URLs and `Working/contradictions.md` named in the
      alert.

`</assistant_may_tick>`

**Ordering note.** Dependencies are WP0 → WP1 → WP2 → WP3 → WP8 → {WP4, WP5} → WP6 → WP7 → WP9 → WP10.
WP4 and WP5 are independent of each other; nothing else may be reordered.

### M4 · Loop control

- **Loops:** the schema adversarial review (WP2), the export reconciliation (WP4), the round-trip
  comparison (WP6). Exit condition for each: the auditor's finding list is **empty** (conjunctive across
  all five pages for WP4 and WP6).
- **Hard caps:** WP2 three rounds; WP4 two repair passes per page; WP6 three repair iterations per page.
  On reaching a cap the residual findings go to **Issues to resolve** (H3 for WP6) and the run halts —
  the cap is not extended and the loop is not restarted under another name.
- **Recorded per iteration:** the finding list, the repair made, and the re-judgement, appended to the
  page's record file as each iteration completes.
- **Carried between iterations:** only the translator's code and the schema. Auditors are re-dispatched
  fresh each time and never see a prior verdict.

### M5 · Resumption

- **Unit of resumption:** `Working/receipts/WP<n>.json`, written the moment a package's receipt is
  validated and before the next package starts. A resumed session reads the receipts, confirms every
  `wrote` path still exists, confirms `<codebase>` HEAD still equals the WP0 SHA (else H4), and restarts
  at the first package without a receipt.
- **Idempotency:** `export`, `import`, `capture` and `compare` overwrite their outputs deterministically;
  `vercel deploy` produces a new URL, so a resumed WP6/WP7 redeploys and records the new URL rather than
  trusting the old one. `<worktree>` is recreated from the branch if missing.
- **A resumed session is a new session** and appends its own entry to `run-log.md`; it does not fire any
  gate to mark the resumption.

### M6 · Observed state

Filled **during** the run, not reconstructed after. Every row is a place where the live system disagreed
with a project document; the two known candidates are pre-registered so their absence means *checked and
clean*, not *not looked at*.

| Observed | Where | Document that says otherwise | Recorded / actioned |
|---|---|---|---|
| Production SHA vs WP0 HEAD SHA | Vercel production deployment | `CLAUDE.md` treats the codebase as "the local codebase of the deployed site" | *(fill at WP6)* |
| Route and component counts | `<codebase>` | `Handover/HANDOVER-2026-08-24.md` §3 says 9 routes / 15 components; `/dev/*` was deleted in `2860a97` | recorded at authoring, 2026-08-24 |

### M7 · Artifacts produced and teardown

| Artifact | Path | Consumed by |
|---|---|---|
| Schema and Lexicon | `Deliverables/Schema and Lexicon.md` | every skill; operator sign-off |
| Page exports ×5 | `Deliverables/Pages/<page>.md` | operator editing; WP6 |
| Intake Template, Block Library | `Deliverables/Intake Template.md`, `Deliverables/Block Library.md` | operator authoring; `new-page-intake` |
| Three skills | `Skills/<name>/SKILL.md` | future sessions |
| Translator | `Working/translator/` | the skills |
| Contradiction list and comparison page | `Working/contradictions.md`, `Working/contradictions-compare.html` | operator's later cleanup |
| Run records, captures, receipts, evidence index | `Working/…` as named in M0 | gate 07; the operator's confirmation |
| Agent definitions | `<vault root>/.claude/agents/webpage-{builder,auditor,skill-runner,viewer}.md` | this Methodology's dispatches |

**Teardown, written before execution.** Everything the run creates outside the vault, and how to remove
it — the vault-side artifacts are the point and are not torn down:

| Residue | Where | Removal |
|---|---|---|
| Round-trip worktree and branch | `<codebase>\.rt\`, branch `rt/<date>` | `git -C <codebase> worktree remove .rt --force; git -C <codebase> branch -D rt/<date>`; delete the `.rt/` line from `.git/info/exclude` |
| Preview deployments | Vercel project `alderman-ai` | `vercel remove <url> --yes` per URL listed in `Working/run-log.md`; they also expire on their own and are never promoted |
| Playwright Chromium | `%LOCALAPPDATA%\ms-playwright\` | `npx playwright uninstall` from `Working/translator/` |
| Translator `node_modules/` | `Working/translator/node_modules/` | gitignored; `Remove-Item -Recurse -Force` |

No file in `<codebase>`'s `main` and no file in `<design-system>` is created or edited by this run, so
nothing there needs reverting.

<!-- Full order, for reference: 04 DoD written -> 05 Methodology written -> 06 coherency check ->
     EXECUTE the Methodology -> 07 completion. Anything that blocks completion during execution goes
     in the section below. Protocol: CLAUDE.md > Spec lifecycle. -->
# Issues to resolve

The assistant will record all issues arising from the methodology execution that prevent completion. Use the list entry template inside the fence below for all list entries in this section.

`<list_entry_template>`
```
## I<N>. <Brief summary of issue to resolve>

"<full context of issue such that the operator does not need to look up anything in another document or in the terminal to understand and includes recommendation.>"

### R<N>
`<operator_response>`
**Response:**

- [ ] Response submitted
`</operator_response>`
```
`</list_entry_template>`

If you reload the SPEC and any Response submitted boxes are ticked, judge whether:
- The response fully unblocks the continuation of execution, if possible -- in that case consider that question complete
- Does not fully address your concern -- in that case add an additional list entry template block using `<N-a>` (followed by `<N-b>`, etc., if needed)

**Once all issues here are resolved to your satisfaction, resume execution of the Methodology at the point it halted** -- not the pre-flight check, which is long finished by the time this section is ever used. While any issue here is unresolved, gate 07 (completion) does not fire.
<!-- list goes directly below here -->


`<assistant_authored>`
## ASSISTANT DECISIONS

*!<-- The assistant will list here all judgement calls it made that it would normally have asked the operator for clarification if the operator was in the loop. Any of these judgement calls will be summarized in a list below, including the trade-offs and logic, as well as any dependencies that would need to change if the operator decides to reverse any of these decisions -->!*

**D1 — Canon is the live site; the codebase is evidence, not authority.** *(from A3.)* Where
`DESIGN.md`, the design-system cards and the codebase disagree, the tiebreaker is what is visibly
rendered on https://alderman.ai. Anything that is *not* visible on one of the five in-scope brand pages
is not canon and does not enter the lexicon -- which resolves most contradictions without needing the
operator at all. **Reverse this and** the lexicon would have to name unused and possibly superseded
components, and every contradiction would need adjudicating by hand.

**D2 — `DESIGN.md` and the design-system cards are read-only for this project.** *(from A3, read with
Not-in-scope.)* They are demonstrably out of sync with the codebase and `DESIGN.md` says so itself, but
repairing them is a change to the design system and the operator has placed cleanup out of scope.
Discrepancies are therefore **recorded, not fixed** -- written to a list in `Working/` for the operator's
later cleanup pass, as A3 requests. **Reverse this and** the sync repair becomes a deliverable of this
spec, which enlarges it considerably.

**D3 — Contradictions are rendered in one batched harness, not one screenshot each.** *(from A3's "please
don't take 30 screenshots".)* Where a contradiction genuinely cannot be settled by looking at the live
site, the disputed items are rendered **side by side on a single local page** so one or two screenshots
cover all of them, rather than a screenshot per item. Items the live site already settles are resolved
under D1 and never reach the operator. **Reverse this and** verification cost scales with the number of
contradictions instead of staying flat.

**D4 — The contradiction list is Methodology work, not pre-flight work.** *(sequencing.)* A3 asks for a
rendered comparison and a written list. Producing either means executing, and execution happens after
gate 06 -- so the contradiction sweep is authored into the Methodology and the list is a named artifact
the DoD can test for. Pre-flight is not held open for it. **Reverse this and** gate 03 blocks until the
sweep is done, which inverts the lifecycle order the operator ruled on 2026-08-24.

**D5 — A fresh-context Opus subagent is "a session that did not author it".** *(DoD 6.2, read with
the operator's 2026-08-24 direction: one Fable/high orchestrator session, Opus subagents, no HITL after
gate 06.)* The `webpage-skill-runner` definition receives only a skill path and its parameters and
inherits nothing from the session that wrote the skill; that isolation is what 6.2 is testing.
**Reverse this and** 6.2 needs a second human-launched session, which reintroduces the HITL the operator
ruled out and splits the spec in two.

**D6 — The colour rule lives in the translator and is resolved at generation time.** *(A1 carve-out.)*
The function is real code with tests, but it emits static `<span>`s and `segments` arrays into the
generated page; no new module runs in the browser, so `components/` stays untouched and 1.3 / 4.3 are
trivially clean. **Reverse this and** a runtime helper lands in `<codebase>/lib/`, which is defensible
under A1 but means every diff-based box has to carve out an exception.

**D7 — Generated pages go to a git worktree on a throwaway branch, at their canonical routes.**
*(5.2 and 5.3 together.)* The nav components hide the current route by pathname, so a round-tripped page
served from any other path is visibly different by one nav item. A worktree at `<codebase>\.rt\`
lets `/about` be regenerated *as* `/about` for a preview deployment while `main` is never written to.
**Reverse this and** either 5.3 or 5.2 fails by construction: writing to `main` breaks 5.3, and a
prefixed route breaks 5.2.

**D8 — The fidelity baseline is alderman.ai only when production is at the WP0 commit; otherwise a
preview of the unmodified WP0 commit.** *(The operator is cleaning the codebase in parallel; a
production deployment that lags `main` would attribute the operator's changes to the translator.)*
Canon for the *lexicon* (D1) is still what alderman.ai shows. **Reverse this and** a false 5.2 failure
can halt the run for a difference the translator did not cause.

**D9 — The one-shot demonstration page carries obviously-placeholder copy at `/template-demo`.**
*(4.1 needs a real preview URL; preview URLs are shareable.)* No brand claims, no operator voice, so an
accidental share reads as a test. **Reverse this and** the builder writes brand copy without the
operator's review, which is a content-voice decision the operator has reserved.

**D10 — Visual approval runs on a local server through Claude in Chrome, delegated to a Sonnet
subagent.** *(Operator ruling, 2026-08-24, on approving WP6, verbatim: "If you need to approve visually,
run it on a local server to check via claude in chrome. Use a sonnet subagent for as much of the claude
in chrome work as possible.")* Applied as: pixel diffs stay the mechanical evidence; wherever a
judgement needs eyes, `<worktree>` is served locally and a `webpage-viewer` (Sonnet/high) inspects it
via Claude in Chrome and writes observations, which the Opus auditor then cites in its verdict. The
viewer observes, the auditor judges. **Reverse this and** either the Opus auditor drives Chrome itself
(more expensive per look) or visual judgement is confined to pixel diffs (blind to behaviour such as
animation and overhang).

`</assistant_authored>`
## DoD
<!-- WRITE THIS FIRST — before the Methodology above, despite sitting below it in the document.
     Authoring the DoD is gate 04. It fixes what "done" means so the Methodology can then be written
     to satisfy a target that is already settled.
     TICK IT LAST — the boxes below are ticked only after the Methodology has been executed, which
     happens after gate 06. Written early, ticked late: a box ticked before execution is a lie. -->

**Writing this section (gate 04).** Define what *done* means for this spec, before any decision about
how to get there has been made. Each box states an outcome and names the evidence that proves it -- an
artifact, a file, a check that can be run -- so that ticking it is a matter of fact rather than opinion.
Write it against the **Problem** and **Scope** only; the Methodology does not exist yet, and the DoD must
not be shaped around a method nobody has chosen.

**Ticking this section (after execution).** Consider if each section of the DoD is complete. If you cannot check off the box for any reason, add the problem to Issues to resolve, halt, and alert the operator.

**The five in-scope pages** (A4) are `/`, `/about`, `/contact`, `/faq`, `/faq-download`. Every box below
that says "each in-scope page" means all five.

`<assistant_may_tick>`

**1 · The contract — schema and lexicon**

- [ ] **1.1 A written schema and lexicon exists** at a registered path in `Deliverables/`, and it can express every block appearing on the five in-scope pages. *Evidence:* the document itself, plus a coverage table with one row per block name giving the component it renders to and the props it sets.
- [ ] **1.2 The lexicon names nothing that is not canon.** Every block name traces to something visibly rendered on alderman.ai; no name exists for a component that is unused, superseded, or `/dev`-only (D1). *Evidence:* the coverage table's canon column, each row citing the in-scope page and location where that block is visible.
- [ ] **1.3 The lexicon introduces no new web components** (A1). *Evidence:* every component named in the coverage table already exists in `components/` at the commit the work started from, shown by a diff of that directory containing no additions.
- [ ] **1.4 The colour rule is specified as a total function** — given any body string it yields exactly one segmentation, with its escape hatches fully specified so no input is undefined. *Evidence:* the rule stated in the schema, plus a worked-example table that includes at least one case the automatic rule cannot produce and which therefore requires an explicit marker.
- [ ] **1.5 Every part of a page the schema does *not* represent is named explicitly** as code-owned, rather than left silent. *Evidence:* a code-owned inventory in the schema listing each such item and the page it belongs to.

**2 · Export — a URL becomes a legible MD file**

- [ ] **2.1 Each in-scope page exports to an MD file** conforming to the schema. *Evidence:* five files at a registered path, one per page.
- [ ] **2.2 Each exported file is legible in the Obsidian editing UI** — headings render as headings, lists as lists, no raw schema machinery showing as noise, no broken markup. *Evidence:* a rendered capture of each of the five files as Obsidian displays them.
- [ ] **2.3 Each exported file carries the whole page.** No block visible on the live page is missing from its MD, and the MD invents nothing the live page does not show. *Evidence:* a per-page block-by-block reconciliation against the live page.

**3 · The intake template and the block library**

- [ ] **3.1 An intake template for a new page exists**, carrying a default set of sections and components ready to fill in. *Evidence:* the template file at a registered path.
- [ ] **3.2 A separate block library exists**, holding every block the lexicon defines, each one copy-pasteable into the template unmodified. *Evidence:* the library file, plus a two-way check that no lexicon block is missing from the library and no library block is absent from the lexicon.
- [ ] **3.3 Template and library are legible in Obsidian** on the same terms as 2.2, and the library states for each block what it looks like and when to reach for it. *Evidence:* rendered captures of both files.

**4 · Import — a completed template becomes a deployed page in one shot**

- [ ] **4.1 A completed intake template becomes a working page at a Vercel preview URL** (A2), from a single invocation with no human intervention between starting it and the URL existing. *Evidence:* the preview URL, plus a run record showing one invocation and no intervening manual step.
- [ ] **4.2 The generated page passes the codebase's own checks.** *Evidence:* clean output from `npm run typecheck`, `npm run lint` and `npm run build` in the run record.
- [ ] **4.3 The generated page adds no new components and changes no existing one** (A1). *Evidence:* a codebase diff for the run showing additions confined to the new page's own route directory.
- [ ] **4.4 The generated page renders correctly in the browser** — every block present, in template order, in the right register. *Evidence:* a capture of the preview URL alongside the template it came from.

**5 · Determinism**

- [ ] **5.1 The same MD produces byte-identical code on every run.** *Evidence:* two independent runs over the same input, with matching hashes for every generated file.
- [ ] **5.2 The round trip is faithful.** For each in-scope page, exporting it and then re-importing that MD produces a page visually indistinguishable from the live page. *Evidence:* a per-page comparison record naming any difference found, with an empty difference list for all five.
- [ ] **5.3 No verification step modified a live page.** The round trip renders to preview only; the source of the five in-scope pages is untouched. *Evidence:* a clean diff of the five live page files across the whole run.
- [ ] **5.4 Ambiguity is impossible rather than merely absent.** No schema construct has two valid code outputs, and any MD the schema cannot represent is rejected with a named reason instead of being guessed at. *Evidence:* a record of rejected-input cases showing the reason given for each.

**6 · The skills**

- [ ] **6.1 One skill exists per capability named in the Problem** — export a URL to MD, produce the intake template and library, and convert a completed template into a deployed page. *Evidence:* the skill files in `Skills/`.
- [ ] **6.2 Each skill runs end to end from a fresh session** with nothing but its own instructions and the artifacts it names — no reliance on context from the session that wrote it. *Evidence:* a run record per skill from a session that did not author it.

**7 · Prohibitions and the record the operator asked for**

- [ ] **7.1 The brand and design system are unchanged.** *Evidence:* a clean diff of `design-system/` across the whole project (D2).
- [ ] **7.2 The contradiction list exists** (A3), naming each place the codebase, `DESIGN.md` and the cards disagree, and which side canon supports. *Evidence:* the list at a registered path in `Working/`, each entry citing the disagreeing artifacts and its canon verdict.
- [ ] **7.3 Contradictions the live site could not settle were put to the operator in one batch** (D3), not one at a time. *Evidence:* the single rendered comparison page, or an explicit note that no contradiction required the operator.

`</assistant_may_tick>`

<!-- If every assistant-tickable DoD box is ticked and Issues to resolve is clear, fire
     gate 07 (completion) and alert the operator that their final confirmation is ready.
     Gate 07 fires BEFORE the operator's box below — it captures the state handed over for sign-off. -->
### Final DoD Confirmation
This section is for the operator only.

`<operator_may_tick>`
- [ ] Spec is complete
`</operator_may_tick>`
