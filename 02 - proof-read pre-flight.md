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
- [ ] Read this entire document first for context -- no skimming -- then return here immediately.
- [ ] If there's any ambiguity in the **Problem**, the **Scope**, or the **Routing and resources** -- anything that would change what gets built, or what "in scope" means -- halt and fill in any clarifying questions you have inside the **Pre-flight Q&A** section of this document -- not in the terminal. Follow the instructions in that section. Then alert the operator that questions are ready for him in this spec. Once all questions have been resolved to your satisfaction, check this box and resume pre-flight check.
- [ ] Confirm every resource named in **Routing and resources** is reachable: the local codebase, the design-system, and the deployed site.
- [ ] Confirm that you have access to either an existing local host server to render the images, or that you have the ability to create a new one
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

`<assistant_may_tick>`
- [ ]
`</assistant_may_tick>`

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

- `/// first entry here`

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

`<assistant_may_tick>`

`</assistant_may_tick>`

<!-- If every assistant-tickable DoD box is ticked and Issues to resolve is clear, fire
     gate 07 (completion) and alert the operator that their final confirmation is ready.
     Gate 07 fires BEFORE the operator's box below — it captures the state handed over for sign-off. -->
### Final DoD Confirmation
This section is for the operator only.

`<operator_may_tick>`
- [ ] Spec is complete
`</operator_may_tick>`
