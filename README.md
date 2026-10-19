# Wish-Type Programming and the Small Local Loop

English | [简体中文](README.zh-CN.md)

## 1. Background: a year ago and now

- A year ago agents could only handle short tasks, and coding happened mostly in IDEs like VS Code and Cursor: you dragged files into the chat and selected exact lines of code to give the agent precise context
- The agent was a copilot, and the human and the agent pair-programmed: the human described a local piece of functionality, had the agent write a piece, then reviewed, ran and tested it, and fed problems back to the agent
- This kept things moving and produced high-quality code
- Now agents handle long tasks lasting from 2 hours to several days, call subagents, run across multiple sessions, and use goal-style tools
- Expectations rose with them: people want 2x to 100x productivity

## 2. The problem with wish-type demos

- Influencers use the latest models for "wish-type" demos: write one line of requirements and wait for the agent to deliver a finished product, and companies promote these demos as capability highlights
- The demos look stunning, but they are one-offs: nobody knows how they work inside, and they can't be changed, maintained or iterated on afterwards
- Model companies work under different conditions from ordinary teams: internal tokens are nearly unlimited, so many agents can develop, test and verify at the same time, and most of their work is new features built from scratch with no legacy code to carry
- With enough agents, wish-type programming can reach good engineering quality, building edge-case, unit, integration and end-to-end tests from scratch, but nobody knows the implementation details
- For traditional companies maintaining complex software that is over 10 years old, this approach is hard to adopt:
  - changing one feature breaks others
  - there are no docs, no regression tests and no unit tests
  - the agent can't tell whether its new code is correct, or whether old features were broken
- Wish-type programming fits building new features and maintaining legacy systems to very different degrees

## 3. The core approach: a small local develop-and-verify loop run by agents

- Keep using the latest models and agents
- The development process follows the local loop from a year ago: keep tasks small, write a piece, test a piece, verify a piece, and only move to the next stage once the current one passes verification
- The difference is that agents run it from start to finish, and the review, testing and feedback humans used to do also go to agents; how to hand that work over to agents is the core problem this approach has to solve
- The human and agent who used to pair were two different thinkers; now the developing agent needs a partner agent playing the human's role: reviewing, testing, verifying, and controlling the overall pace

## 4. Verifying wishes

- The agent that makes wishes can make wrong ones, so wishes must be verified rather than assumed correct
- Verification has two layers:
  - First, a wish must be machine-checkable: a vague wish like "add a login button" can't become an executable acceptance criterion, so the wishing agent rewrites it until it can be verified
  - Second, only a human can judge whether a wish is right: before each batch of wishes is submitted, a human goes through it to confirm the direction hasn't drifted
- Every wish has acceptance criteria, and results are checked against them
- The human's job shifts from reviewing code line by line to reviewing wishes: checking whether the task split is right and whether the acceptance criteria are strict enough; wishes are natural language, so a batch takes a few minutes to read

## 5. Wishes emerge gradually

- A wish starts out vague, so first I chat with the bot to set the overall direction and the concrete wishes
- Once that's settled, only the first few steps are clear; later ones are just a rough direction, or only a feeling
- As earlier wishes are implemented and tested, I understand the system better and later wishes become clearer, and sometimes the new understanding overturns the earlier feeling, against intuition
- So you can't orchestrate every wish at the start; wishes become clear as the work moves forward
- This is the main difference between this approach and today's mainstream wish-type programming and some open-source workflows: they split and orchestrate the whole wish at the start, while here only the parts that are already clear get orchestrated

## 6. Orchestration and progress

- Orchestration has two parts:
  - The clear first few steps are split into sub-wishes and arranged as serial or parallel by their dependencies
  - The vague later parts only record a direction, and after each batch of wishes is done they are added to, adjusted or overturned based on what was learned
- While moving forward, track the current state:
  - which step runs first, and the state of the current step
  - which steps follow once it's done, and whether they can run in parallel
  - the state of each of those serial or parallel tasks

## 7. The explore-and-verify loop

- Once the clear steps are done, the next steps show up as ideas: some are certain, some are not
- I tell the agent the certain ones directly
- The uncertain ones go to the agent, which keeps searching, exploring and actually running them to verify, then reports which ideas it verified and what the actual effect was
- The agent and I turn the verified fuzzy ideas into concrete, decided wishes and keep pushing forward
- This repeats, and some ideas go from fuzzy to specific step by step
- Each round records two things:
  - what has been implemented
  - the explore-and-verify process itself: which ideas were tried and what the actual effect was
- The far-future goal stays as the reference point: the more gets explored and verified, the more concrete new things emerge, step by step closer to the far-future goal

## 8. Test every step, then integrate

- Drop the old habit of writing all the code first and testing at the end: many cases go untested, problems surface all at once, and finished code is hard to change
- Implement a step, test it, accept it, and only move on once it's fine
- When A and B are independent and the last step integrates them:
  - Build A to its own criteria, without worrying about integration yet
  - If integration shows design problems in A or B, go back and change A's implementation and its acceptance criteria; that change doesn't count as breaking A's original verification standard

## 9. Records: commits and comments

- Acceptance-criteria changes are recorded in commits, and code comments aren't used for this:
  - Every time an acceptance criterion changes, the commit message says why, for example: "integration revealed a conflict between A's and B's interface conventions, criterion changed from X to Y"
  - Without this, git history only shows that a test changed, not whether A was written wrong or the criterion itself was wrong; with it, anyone reading the code later can trace the full decision
- Comments go only at function declarations and key logic, and cover two things:
  - the key design: how this code is designed
  - information outside the code: external dependencies of the overall feature, reference docs or papers needed to understand the logic's background, the issue doc behind a bug fix, and anything else that can only be explained in a comment
- Some code was written poorly, with specific workarounds made for reasons at the time; keep those out of comments as much as possible, because agents may pick up these bad habits later, and leave only good practices in comments
