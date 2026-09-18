# knowledge-work-executable-spec-research
Most frontier executable spec research is based on code, which is highly benefited by deterministic verification methods. My research is on doing the same with knowledge work. I.e. tinkering on methodology to agentically author and execute orchestration documents for even quite complex ouputs.

# Start Here
This a a snapshot of around ~mid august 2026 of how I set up medium-complex -- i.e. all tasks semantically related enough to be orchestrated by a single session between gates 6 and 7 with heavy delegation to subagents.

No one wakes up one day and says "let's build a 7-stage life cycle for an executable spec." Stage/Gate/Phase 6 is roughly where I'd consider a spec finished for execution, and everything before that is my way of automating what I used to do my hand. All the context engineering, research, etc used to be something I would do as a separate step before preparing this kind of document to try and "one-shot." It used to be about proper handover, state save, etc. to basically spend several session prepping for a no-HITL execution phase.

Because I used to be a copywriter, it felt oddly normal to spend 3-6 hours manually writing a document on how to execute something. But because I'm self-respecting adult who wants to see his kids sometimes, I started looking for ways to automate this, and this is a rough snapshot of where I landed. The goal is to move up one level of abstraction, and that means building a system who knows how to decompile complex tasks into nested, yet bounded, sub compoenents.

Super rough benchmark: Works best when I know ~50-75% of the "how" at start. When I know less than 50, my micromanagement makes things worse, and it's much more of a token-inefficient research task than an execution task. When I know more than 75, I usually can do it faster in other ways (or, if I'm being truly honest, I can accidentally make the project take 10x more time by being overconfident, choosing NOT to take the time for the systematic approach, and then being stubborn 😀)

## Initial contents
One group of my **7-stage Spec lifecycle**.

(Sept. 2026 edit: My controlled vocab for these types of artifacts are now Executable Guiding Documents (EGDs).

# Info System

## Quick taxonomy
There's a Child-Parent relationship chain between:
Task > Spec > Project > Charter

### Rough boundaries

Note: these are a perspective of a 1-person "operation" when it comes to balancing human time and token spend. If you have a 7-8 figure token budget you should be way more efficient in some places because of scale and way less efficient in others because of time.  

- A task can be given to a single session **with almost zero additional context.**
- Bounded by operator time. If you have high confidence you can get a >>verifiably<< correct output in 1-3 tries within 20 mins, then don't spend hours writing a document to micromanage that.
- 
- "build me a python script that does x, and then prove to me that it actually works"

- A ready-to-execute spec (in this repo: 06 coherency check) can be delivered end-to-end **by a single, orchestration session with heavy delegation** (concretely, in aug 2026, that meant a Fable 5 orchestrator, and Opus 5 subagents)
- Bounded by the effective context managemment by a single session using a flagship model. This almost never about token limits, this is about semantic drift. Basically, if it delegates most tasks to subagenets, how much semantic range those it have in being able to integrate all of the outputs of the subagents. (sept 2026 edit: I'm now really interested on when it's worth it to strategically fork orchestration sessions at specific gates, which would expand the bounds of this phase)

- a project is an executable document focused purely on decomping even more complex tasks down to the Spec level, while also trying to surface all needed HITL activities to be done initially, so that all of specs are ready to run end-to-end without me (I call this async HITL -- I'm still "in the loop" as in my judgement is still being applied, it's just happening separately from the execution phase). The boundary between setting up a project document vs just executing a spec-sized chunk of it at a time, is roughly the percentage of token use is spent in execution vs verification. If 85% of your token use is just drift-prevention verificaiton steps, it's probably something that (outside of a research context) should probably be done at a lower level of abstraction

- a charter would theoretically one level of abstraction higher, breaking work too semantically diverse to even decomp -- i.e. you can't reliably verify pre-run that all of the decopm tasks together, once executed, would actually achieve dod -- into chunks that COULD be deocomped. I don't think we ever need this level of abstraction. Because:

### 2 forces that rapidly shift these boundaries
- Model capacity. I still have weird superstitions/scars left from 2023-2025 about LLMs and a lot of my mental model of what fits inside a context window -- as well as what context in their training data is already good enough to not micromanage -- is probably quite outdated as of Sept 2026.
- Harness capacity. The boundary of "when can I just ask without giving any additional context" very much depends on how much context your session boots up with. Does each session have a lazy loaded reading list to understand how the work its doing fits into the broader picture of your harness and previous work? Is your system legible? Grep does not equal ontology. And this shift is happening on both your and anthropic's (et al) side. In the next six months we can all probably just /ultracode all of this stuff if we're being clear eyed about the trajectory. But on the other hand, maybe token usage becomes 100x more expensive and we need to do things on open weight models.... 
