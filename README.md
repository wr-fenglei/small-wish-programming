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

## 4. Concreteness: breaking a big wish into small wishes

- A big wish is vague, and the model delivers a result that feels right overall while the details are random:
  - you can barely trace how it fits together, and you can barely change it
  - the time to understand it plus the time to fix it adds up to more than redoing it yourself in small steps
- So the point of "concrete" is breaking a big wish into small wishes; a small wish is more concrete than a big one, and far cheaper to understand and change
- A big ball of mud and small balls of mud:
  - one big wish produces one big ball of mud that is nearly impossible to understand or change
  - split it into ten small wishes, and each produces a small ball of mud that is small enough to see through and change
  - a small wish, once fixed, becomes the foundation that later wishes iterate on, and complexity stays under control
- How fast you can break a wish down depends on how well you know the domain:
  - the better you know a domain, the faster you break it down and reach something concrete
  - in a domain you don't know well, at first you don't know how to split it, and it takes a long stretch of exploration to find a way
- In a domain you aren't good at or don't know, there are three sources for the split:
  - have the agent split it for you
  - look online for how others split similar work and what they learned
  - ask colleagues who worked on this part before; they know the project background, so their split is more accurate than generic advice online
  - combine these and split the work the way experience or the agent suggests
- To help the agent understand the background faster, some infrastructure is needed: the agent can search past documents and past chats, including the team's past documents and chat history, to get up to speed on the relevant background quickly

## 5. Voice mode: the key to going faster

- Voice mode speeds things up in two ways: input and output speed, and a change in how the agent executes
- First, input and output speed: speaking and listening are much faster than typing and reading, and this alone makes the process two to three times faster
- Second, a change in how the agent executes:
  - in text mode, the agent tends to attempt long tasks, running twenty minutes, fifty minutes, or even several hours at a time, and produces a big ball of mud
  - the prompt is short but the result is large, the user spends a long time understanding it, and then finds many details are random and not what they wanted
  - fixing it also relies on reading and typing, which is slow, and in the end the user often has to give up on that ball of mud
  - in voice mode, the agent has to respond quickly, since nobody waits five minutes for an answer in a voice conversation, so it doesn't make big attempts; it only does quick small tasks and produces at most a small ball of mud
  - the result of each small ball of mud is told to the user by voice right away; anything unclear, the user can have the voice agent help explain and sort out, and anything to change, the user can have it change right away
- This way the user can finish one small ball of mud very quickly in voice mode, move on to the next small wish, produce a new small ball of mud, and finish that one quickly too
- Half an hour of voice conversation and the tasks handled in it can replace hours of back-and-forth in text mode
- Comparison:
  - in the same amount of time, text mode yields one big ball of mud that the user doesn't understand and has to spend even more time understanding and fixing
  - voice mode yields a result built up step by step, very clear and concrete, where the user understands every detail, knows it well, and wanted all of it
  - this is the difference between using voice mode and not using it

## 6. Verifying wishes

- The agent that makes wishes can make wrong ones, so wishes must be verified rather than assumed correct
- Verification has two layers:
  - First, a wish must be machine-checkable: a vague wish like "add a login button" can't become an executable acceptance criterion, so the wishing agent rewrites it until it can be verified
  - Second, only a human can judge whether a wish is right: before each batch of wishes is submitted, a human goes through it to confirm the direction hasn't drifted
- Every wish has acceptance criteria, and results are checked against them
- The human's job shifts from reviewing code line by line to reviewing wishes: checking whether the task split is right and whether the acceptance criteria are strict enough; wishes are natural language, so a batch takes a few minutes to read

## 7. Wishes emerge gradually

- A wish starts out vague, so first I chat with the bot to set the overall direction and the concrete wishes
- Once that's settled, only the first few steps are clear; later ones are just a rough direction, or only a feeling
- As earlier wishes are implemented and tested, I understand the system better and later wishes become clearer, and sometimes the new understanding overturns the earlier feeling, against intuition
- So you can't orchestrate every wish at the start; wishes become clear as the work moves forward
- This is the main difference between this approach and today's mainstream wish-type programming and some open-source workflows: they split and orchestrate the whole wish at the start, while here only the parts that are already clear get orchestrated

## 8. Orchestration and progress

- Orchestration has two parts:
  - The clear first few steps are split into sub-wishes and arranged as serial or parallel by their dependencies
  - The vague later parts only record a direction, and after each batch of wishes is done they are added to, adjusted or overturned based on what was learned
- While moving forward, track the current state:
  - which step runs first, and the state of the current step
  - which steps follow once it's done, and whether they can run in parallel
  - the state of each of those serial or parallel tasks

## 9. The explore-and-verify loop

- Once the clear steps are done, the next steps show up as ideas: some are certain, some are not
- I tell the agent the certain ones directly
- The uncertain ones go to the agent, which keeps searching, exploring and actually running them to verify, then reports which ideas it verified and what the actual effect was
- The agent and I turn the verified fuzzy ideas into concrete, decided wishes and keep pushing forward
- This repeats, and some ideas go from fuzzy to specific step by step
- Each round records two things:
  - what has been implemented
  - the explore-and-verify process itself: which ideas were tried and what the actual effect was
- The far-future goal stays as the reference point: the more gets explored and verified, the more concrete new things emerge, step by step closer to the far-future goal

## 10. Test every step, then integrate

- Drop the old habit of writing all the code first and testing at the end: many cases go untested, problems surface all at once, and finished code is hard to change
- Implement a step, test it, accept it, and only move on once it's fine
- When A and B are independent and the last step integrates them:
  - Build A to its own criteria, without worrying about integration yet
  - If integration shows design problems in A or B, go back and change A's implementation and its acceptance criteria; that change doesn't count as breaking A's original verification standard

## 11. Maintaining old projects with no docs and no tests

- This applies to projects that have been maintained for years, have a large codebase, no documentation, no unit or integration tests, and are hard to test
- Agree on test terms first: there are many test terms and people understand them differently, so first spell out what each kind of test is responsible for and what it covers:
  - Unit tests: test the logic of a single function, class or module, with external dependencies replaced by mocks or stubs; they cover branches, boundary values and bad inputs, and run fast
  - Integration tests: test how several modules work together, or how a module works once connected to dependencies such as a database or an external API; they cover interface conventions, data formats and call order
  - End-to-end tests: start from the user's entry point (the UI or a public API) and walk a real business flow through the whole chain; they cover whether the business flow works end to end, are few in number and run slowly
  - Regression tests: after every change, rerun the existing cases to confirm that behavior that used to work hasn't been broken; the cases can come from unit, integration and end-to-end tests, and also include the behavior baseline and cases added when fixing bugs
  - Unit, integration and end-to-end tests are split by scope, while regression tests are defined by purpose, so the two ways of splitting can overlap
- From now on, when developing a new feature, state which parts need unit tests, which need integration tests, which need regression tests and which need end-to-end tests
- Build a behavior baseline first:
  - Have the agent run the system, capture its interfaces, and record inputs and outputs, which gives an undocumented system a layer of black-box tests
  - After that, every change is regression-checked against this baseline, without relying on manual review
  - Once the baseline exists, keep adding related docs and tests to it
- Using tests as documentation is an idea still to be verified, and I'm not yet sure it works:
  - Docs always go stale, so my current practice is to keep docs as a quick index only: a few files covering what the project does, which businesses it has, which components or modules the main business flows live in, and the key files or types in each module; this kind of index rarely changes
  - Other key design goes in comments: use the quick index to find the key files or types, then read the comments and source to work out the project
  - Edge cases are a kind of documentation too, but many are kept by the test team themselves, aren't documented, and are hard for agents to get at
  - In complex old projects, you don't know which business flows use some modules; these modules suit boundary protection by tests: if a module was touched by flows A, B, C and others over its history, the matching end-to-end or regression tests must exist
  - When you change the module later, reading its tests tells you which business flows it sits in; each test walks through a business flow, so together they roughly cover the existing flows
  - So some places get unit tests and others get integration or regression tests, and regression tests covering the various cases can serve as one form of documentation

## 12. Records: commits and comments

- Acceptance-criteria changes are recorded in commits, and code comments aren't used for this:
  - Every time an acceptance criterion changes, the commit message says why, for example: "integration revealed a conflict between A's and B's interface conventions, criterion changed from X to Y"
  - Without this, git history only shows that a test changed, not whether A was written wrong or the criterion itself was wrong; with it, anyone reading the code later can trace the full decision
- Comments go only at function declarations and key logic, and cover two things:
  - the key design: how this code is designed
  - information outside the code: external dependencies of the overall feature, reference docs or papers needed to understand the logic's background, the issue doc behind a bug fix, and anything else that can only be explained in a comment
- Some code was written poorly, with specific workarounds made for reasons at the time; keep those out of comments as much as possible, because agents may pick up these bad habits later, and leave only good practices in comments
