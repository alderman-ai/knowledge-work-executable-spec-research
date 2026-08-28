
# What this document is

This is an executable spec designed to be executed end-to-end in a single session, with the main session delegating to subagents when specified or otherwise appropriate according to SOP. 

# Pre-flight check
<!-- start execution here, first by commit "pre-flight" ONLY on first time encountering this -->

First, , then assistant should check off boxes below in this section as the tasks are completed.

`<assitant_may_tick>`
- [ ] Make sure to read entire document first for context -- no skimming -- but return here immediately afterwards.
- [ ] If there's any ambiguity in the problem, the methodology, or how the methodology connects to the DoD, halt and fill in any clarifying questions you have inside the **Pre-flight Q&A** section of this document -- not in the terminal. Follow the instructions in that section. Then alert the operator that questions are ready for him in this spec. Once all questions have been resolved to your satisfaction, check this box and resume pre-flight check.
- [ ] Confirm that you have access to either an existing local host server to render the images, or that you have the ability to create a new one
`<assitant_may_tick>`

<!-- once all tasks in this section are checked off commit "post-flight", alert operator, and start executing Methodology -->

# Pre-flight Q&A

The assistant will record all clarifying questions by the assistant and answers by the operator verbatim. Use the list entry template inside the fence below for all list entries in this section.

`<list_entry_template>`
```
## Q<N>. <Brief summary of question for operator>?

"<full context of question such that the operator does not need to look up anything in another document or in the terminal to understand>"

### A<N>
`<operator_response>`
**Response:**

-[ ] Answer submitted  
`</operator_response>`

```

If you reload the SPEC and any Answer submitted boxes are ticked, judge whether:
- The answer fully addresses your concerns -- in that case consider that question complete
- Does not fully address your concern -- in that case add an additional list entry template block using `<N-a> (followed by <N-b>, etc, if needed`)

**Once all questions here are answered to your satisfaction, resume the pre-flight check, ticking the appropriate box**

`</list_entry_template>`
<!-- list goes directly below here -->

# Problem

We need to make a suite of skills connected to my deployed website codebase to import and export human-legibility MD files connected to new or existing web pages. For example:

- Take a URL of a web page, and convert it into an easy to read and/or edit MD file
- Create an intake template for a new page
	- with a default set of sections and components
	- and a separate library of possible sections and components that I can copy and paste from
-  Convert a completed one off these intake templates into a full working deployed web page in a single shot

For this to be possible, we need to create an agreed upon schema and lexicon such that:
- It's perfectly human legible during editing in Obsidian UI
- AND when uploaded is 100% unambiguous of the exact, deterministic frontend code needed to deploy it
	- The results should be 100% deterministic

## Scope

The scope is making sure all suites of skills work as intended, and the middle layer "translator" from human-legible writing surface to machine-legible deterministic code completes a one shot from MD to deployed web page.

### Not in scope

Creating any new web components
Making changes to the brand or design system


## Routing and resources

- We have a working, published, custom-coded website:
	- deployed at https://alderman.ai
	- Local code base: "C:\Users\alder\Desktop\Claude Code Website\alderman-ai"
-  A sample of the deployed homepage translated "code-to-human"
	- Treat adversarially to see if this "translation" is sufficient
- There's a medium fidelity 

## Methodology
<!-- do not begin execution until after pre-flight check section is completed -->

`<assitant_may_tick>`
- [ ]  
`</assitant_may_tick>`

<!-- if no issues to resolve, or if all resolved to satisfaction, commit "pre-dod" and proceed to DoD -->
# Issues to resolve

The assistant will record all issues arising from the methodology execution that prevents completion. Use the list entry template inside the fence below for all list entries in this section.

`<list_entry_template>`
```
## I<N>. <Brief summary of issue to resolve>

"<full context of issue such that the operator does not need to look up anything in another document or in the terminal to understand and includes recommendation.>"

### R<N>
`<operator_response>`
**Response:**

-[ ] Response submitted  
`</operator_response>`

```

If you reload the SPEC and any Response submitted boxes are ticked, judge whether:
- The response fully unblocks the continuation of execution, if possible -- in that case consider that question complete
- Does not fully address your concern -- in that case add an additional list entry template block using `<N-a> (followed by <N-b>, etc, if needed`)

**Once all questions here are answered to your satisfaction, resume the pre-flight check, ticking the appropriate box**

`</list_entry_template>`
<!-- list goes directly below here -->


`<assitant_authored>`
## ASSISTANT DECISIONS 

*!<-- The assistant will list here all judgement calls it made that it would normally have asked the operator for clarification is the operator was in the loop. Any of these judgement calls will be summarized in a list below, including the trade-offs and logic, as well as any dependencies that would need to change if the operator decides to reverse any of these decisions -->!*

- `/// first entry here`

`</assitant_authored>`
## DoD
<!-- don't proceed here until all of methodology section is complete -->

Consider if each section of the DoD is complete. If you cannot check off the box for any reason, add the problem to Issues to resolve, halt, and alert the operator.

`<assitant_may_tick>`

`</assitant_may_tick>`

<!-- if all DoD check boxes are ticked, alert operator that their final confirmation is ready -->
### Final DoD Confirmation
This section is for the operator only.

`<operator_may_tick>`
- [ ] Spec is complete
`</operator_may_tick>`