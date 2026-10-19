# Wish-Type Programming and the Small Local Loop

English | [简体中文](README.zh-CN.md)

## 1. How we built software a year ago

- About a year ago, agents could only handle short tasks
- Coding happened mostly inside IDEs like VS Code and Cursor
- You dragged files into the chat and selected specific lines to give the agent precise context
- A human described one local piece of functionality, and tested each small segment right after it was written
- Work moved forward smoothly, and the code that came out was high quality

## 2. What changed

- Agents now handle tasks that run from 2 hours to several days, using subagents, multiple sessions and goal tools
- Expectations rose with them: people want 2x to 100x productivity gains
- Content creators use the latest models for "wish-type" demos: write one sentence of requirements, wait for the agent to deliver a finished product

## 3. The problem with demos

- The demos look stunning, but they are one-time works: nobody knows how they run inside, and they can't be modified, maintained or iterated on afterwards
- Companies promote these demos as highlights of what their models can do
- Model companies have close to unlimited internal tokens, so they can run many agents in parallel to develop, test and verify
- Model companies are new companies, and most of what they build is new features from scratch, with no legacy code to carry

## 4. Where wish-type programming fits

- With enough agents, wish-type programming can reach good engineering quality: edge-case tests, unit tests, integration tests and end-to-end tests built from scratch
- The catch is that nobody knows the implementation details
- For traditional companies maintaining complex software that is 10+ years old, this approach is hard to apply:
  - Changing one feature breaks others
  - There are no docs, no regression tests, no unit tests
  - The agent can't tell whether new code is correct, or whether old features got broken
- How well wish-type programming applies differs a lot between building new features and maintaining legacy systems

## 5. Conclusion

- Keep using the latest models and agents
- Model the workflow on the local develop-and-verify loop from a year ago: write a small segment, a human tests it, move to the next one
- Let agents run that loop

## 6. How it worked in the copilot era

- Back then the agent was a copilot, and the human and agent paired
- The human had the agent write one segment, then reviewed the code, ran it, and fed problems back to the agent
- The paradigm today still moves segment by segment: write a segment, test it, verify it
- Each stage must pass verification before the next one starts, and tasks are split small
- The method follows the old way, the difference is that the agent runs it end to end
- What the human used to do (review, testing, feedback) now has to be handed to the agent too, and how to hand it over is the core problem this method has to solve

## 7. Two agents pairing

- In the old pairing, the human and the agent were two different minds
- Now the developing agent needs a partner agent that takes the human's old role: review, test, verify, and control the overall pace
- The structure becomes: an agent that simulates the human, paired with the agent that writes the code

## 8. Verifying wishes

- The agent making the wishes can also wish wrong, so wishes have to be verified, never assumed correct
- Verification has two layers:
  - First, a wish must be machine-checkable: a vague wish like "add a login button" can't be written as executable acceptance criteria, so the wish-making agent rewrites it until it is verifiable
  - Second, only a human can judge whether a wish is right: before each batch of wishes is submitted, the human scans them to confirm the direction hasn't drifted
- The human role remains, and its work shifts from reviewing code line by line to reviewing wishes: checking whether the decomposition is right and whether the acceptance criteria are strict enough
- Reviewing wishes takes less time than reviewing code, since wishes are natural language and a batch can be read in a few minutes

## 9. Orchestrating wishes

- A wish starts out vague, so the first step is to set the overall direction by chatting with the bot and working out the concrete wish together
- Once that is set, only the first few wishes are clear, and later ones are just a rough direction, or a feeling
- Later wishes emerge step by step: as earlier wishes get implemented and tested, I understand the system better, and the later wishes get clearer
- Sometimes the new understanding overturns the earlier feeling, and the result runs against intuition
- This is the main difference between this method and today's mainstream wish-type programming and some open-source workflows: they decompose and orchestrate the whole wish at the start, while here only the clear part is orchestrated, and later wishes emerge as implementation and testing go on
- So you can't orchestrate every wish at the start, wishes become clear gradually as the work moves forward
- Orchestration has two parts:
  - The first few steps that are already clear get split into sub-wishes, arranged as serial or parallel by their dependencies, the same way tasks are orchestrated
  - The vague part later on only records a direction, and after each batch of wishes is done, it gets filled in, adjusted or overturned based on what was learned
- After orchestration, move forward by current state:
  - Which step runs first
  - The status of the current step
  - Which steps run after this one finishes, and whether they can run in parallel
  - The status of each of those serial or parallel tasks
- Every wish needs acceptance criteria, and the result is checked against them

## 10. Test every step

- This method drops the old habit of writing all the code first and testing everything at the end
- In that old process many cases never get covered, a pile of problems shows up, and since all the code is already written before fixing starts, it is hard to change
- So go step by step: implement one step, test it, accept it, and move on only when this step is fine
- One more case: some tasks do A first, then B, where A and B are independent, and the last step integrates A and B
  - Integration may reveal problems in the separate designs of A and B that need temporary changes
  - Such a change does not count as breaking A's original verification standard
  - When writing A on its own, follow A's own standard and don't worry about integration yet
  - When integration reveals problems, go back and change A's implementation and its acceptance criteria

## 11. Record acceptance-criteria changes in commits

- When integration reveals problems and A is changed, A's acceptance criteria change too
- That change has to be recorded, and the place to record it is the commit, not comments in the docs
- Each time an acceptance standard changes, the commit message states why, for example: "integration revealed a conflict between A and B's interface conventions, standard changed from X to Y"
- Without that, the git history later only shows that the tests changed, not why: whether A was written wrong, or the standard itself turned out to be wrong during integration
- Recording it in the commit lets anyone reading the code later trace the whole decision process

## 12. Comments

- Write comments only at function declarations and key logic, covering two kinds of content:
  - First, the key design: how this code is designed
  - Second, things outside the code: external dependencies of the overall feature, reference docs or papers needed to understand the background of the logic, the issue doc behind a bug fix, and anything else that can only be explained through comments
- What to keep out of comments as much as possible: some code was written poorly, with specific workarounds made for reasons at the time, and that practice isn't worth keeping, so write as little of it as possible, because agents may pick up these bad habits later
- Leave only good practices in comments, never bad habits

## 13. The Explore-and-Verify Loop

- Once the clear first steps are done, the next steps show up as ideas: some are certain, some are not
- I tell the agent the certain ones directly
- The uncertain ones go to the agent, which keeps searching, exploring, and actually running them to verify
- The agent reports which fuzzy ideas it verified and what the actual effect was, then the agent and I turn those fuzzy ideas into concrete, decided wishes and keep pushing forward
- This repeats, and some ideas go from fuzzy to specific step by step
- Each round records two things:
  - what has been implemented
  - the explore-and-verify process itself: which ideas were tried and what the actual effect was
- The far-future goal stays as the reference point: the more gets explored and verified, the more concrete new things emerge, step by step closer to the far-future goal
