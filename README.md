# What Works

_Software development as rational inquiry_

Click the headings to expand them.

<details>
<summary>

## Introduction V2

</summary>

## The Task of Programming
- If you are a programmer, you do something incredibly difficult every day. It may not be obvious to you just how difficult and daunting the task of programming really is.
- our job as programmers is to shape a software system's behavior
	- system: a network of interacting parts
		- for convenience of representation, we assume these interactions are discretized—since the computers we're talking about are digital, this is a fine assumption.
	- in a software system, the parts may be people, machines, programs, organizations...
	- behavior: the set of possible interaction-sequences in the system
		- interaction-sequences may generally be arbitrarily long
		- the behavior is an infinitely large set!
		- example: pocket calculator
	- enumerating the interaction-sequences is not an efficient way to implement the system.
	- instead, we write code, which describes the behavior in a compressed format.
	- code can be good (tight compression that uses concepts we recognize) or bad (inefficient, illegible compression)
- code is much more comprehensible than a list of interaction-sequences.
	- but it still doesn't fit in our heads (beyond 10 lines or so)
	- and so we cannot _directly_ control the code.
		- There isn't a straight path that goes from desired behavior -> fully-formed design in our heads -> typing the code out, start-to-finish, with no backtracking.
	- when we write code, we need to use a _process_ which involves _state_: intermediate artifacts (designs, work-in-progress code, tests, etc.) that persist outside our heads.
	- The process tells us what to do next given the current state of our artifacts and the desired behavior of the system.
	- the process includes the tools we use to comprehend and shape the code, the way we interact with teammates, how we deploy...
	- we can _only_ directly control the process. Not the code.
	- We _can_ look at the code and use our judgements about it as inputs to the process. For example, a process may involve refactoring existing code.
	- My friend who was between jobs and working on a personal software project set himself the goal of working on it 8 hours a day for a month. I said, "why not set a goal of what features you want to have implemented by the end of the month?" He replied "I can't control what features are implemented. All I can control is how I work and for how long." At that, I was enlightened.
- processes are so complex and idiosyncratic that it is hard to characterize any _actual_ process exactly. We have theoretical descriptions of processes (XP, waterfall) but these necessarily leave out many details that are specific to the project and the people working on it.
- code constrains our choice of process.
	- when the tests take 4 hours, you can't do TDD. Even when tests take 40 seconds, you can't really do TDD.
- We may choose to constrain the code to accommodate certain processes.
	- if we want to do TDD, it's important to shape the code and tests so the tests don't take more than a couple seconds to run.
	- anecdote about member variable prefixes in C++ being used to accommodate text editors that don't highlight member variables differently
	- if we are choosing a process to work with existing code, the code constrains our choice.
- Code is not the only constraint on the process. There are also fundamental constraints on human cognition and collaboration that necessarily shape the process.
	- the mythical man-month: collaboration has a cost
	- doherty threshold

We can only effectively approach the programming task if we acknowledge its enormous difficulty. What I have described in this chapter is a pile-up of difficulty upon difficulty several layers deep. In the subsequent chapters I will take some steps toward resolving these difficulties.

## Process
- get enough sleep
- starting a system from scratch
	- walking skeleton: something technically deployable that hits all the essential integrations and tools
		- database
		- external services
		- client/server interaction
		- development environment
		- tests
		- build
		- lint/format
		- versioning
		- deploy
	- design verification strategies for sub-doherty feedback
	- many more much smaller steps
		- use tools to reveal a problem: mismatch between desired and actual behavior, or code smell. You should be able to do this quickly enough that your attention doesn't wander.
			- your discernment of code smells is a tool. Code smells should be objective.
		- fix the problem
		- use the same tools to verify that the problem is now fixed.
- evolving an existing system
	- don't demand perfection, and don't settle for "good enough"
    - find out what the team is already doing that's working, and intensify it.
    - find the bugs and fix them
    - make new code exemplary
    - separate fast and slow tests so they can be run at different times
- working on code
	- design interfaces (data crossing a boundary)
	- refactor implementations

## Seeing Code
Motivating example: design of a timer
- separate "tick" action from state, calculations (e.g. displaying the time)
- benefits: testing, persistence
- "how did you know to do that?"
	- it is possible to find this design without even thinking about testing or persistence.
	- you just have to follow the heuristic that actions, state, and calculations should be kept separate

- Capability levels
	- data
	- function
	- state
	- action
- Graphs
	- call graph
	- dependency graph
	- why cycles are usually bad

## Designing Code
- Objectives
	- tight, legible compression
	- fast feedback
	- self-evident correctness (not orthogonal to the other two points)

</details>
  
<details>
<summary>
  
## Introduction

_where did this book come from and why am I writing it?_

</summary>

### The Goal: less confusion, less anxiety, fewer bugs, more comfort, more habitability, more humanity, more effectiveness

- this book is about ways of seeing and working with code that will make your software a more reliable translation of your intent. In practical terms, this means writing fewer bugs, and being able to add features faster and with less risk.
- most of these techniques were invented by other people
- some of this stuff has been known for 50+ years

- these techniques are tools, not rules that you have to always follow. They solve a problem in a context. The meta-technique is to always be adapting the techniques to the exact context you find yourself in. There are many aspects of the context to consider:
  - UX
  - testability
  - readability
  - maintainability
  - performance
  - TODO: refine this list.

- Because the only way to use these techniques is to be constantly adapting them, you have to be and feel free.
  - if you feel that political pressure from your manager, architects, coworkers, or whoever, is constraining you to do things in a particular way—_no matter what that way is_, the techniques in this book will seem idealistic and impractical.
    - I do not believe there is _any_ closed set of rules that will work for all circumstances (at least, no set of rules that a human can comprehend)
  - double-check with yourself that this is not just a defensive reaction to the prospect of having to do an enormous amount of work to improve things. Are other people _really_ that opposed to change, or are _you_ opposed to change because you might have to be the one to lead it?

- Adapting techniques to circumstances
  - Try to write code
  - think "gosh this is hard/confusing"
  - notice "this technique seems like it could be applicable here"
  - "but if we use it, it will harm the system in ways XYZ"
  - "but if we change the technique slightly, the problems with XYZ go away. Even though this modified technique _would cause other problems if used as a general rule_, in this situation it doesn't cause those problems."

Example: writing tests for YAML config
- problem: config is duplicated and hard to maintain. Different parts need to be kept in sync. It's hard to see the crucial details amid all the boilerplate. It's easy to break things.
- insight: writing tests for the config could stop us from breaking things.
- problem: testing guidelines say to make tests very specific examples of behavior. But YAML doesn't have behavior. If we write tests that make assertions about specific YAML nodes, the tests will just duplicate the structure of the config—not helpful.
- insight: when testing static config files, it's okay for tests to assert on generalities. Tests can have loops and complex logic. They are more like validators than traditional unit tests.

- incremental change
  - don't demand perfection, and don't settle for "good enough"
  - identify what the team is already doing that's working, and enhance that.
  - find the bugs and fix them
  - make new code exemplary

### This book is for self-organizing engineering teams

- cite Roy Osherove's XP+Scrum video

### This is all stuff that has actually worked in practice.

### This book can provide the start of a pattern language for your team.

### This is all old news

- TDD + the unix philosophy + XP + Christopher Alexander + the _Tao Te Ching_


### TL;DR: it's the Unix philosophy plus TDD (plus algebraic types, where available)

what isn't it? i.e. what have I tried that _hasn't_ worked?
- a lot of OOP: inheritance, active objects
- naive top-down procedural decomp
- pure functional programming
- just thinking harder or being smarter
- ATDD

what haven't I tried in earnest?
- logic programming

### TDD

### The Unix Philosophy

### Algebraic Types

</details><!-- Introduction -->

<details>
<summary>

## Vision

_I'm advocating change. What is the goal of the change? why is it compelling?_

</summary>

### To what end? Quality.

- a quality system is/does what you expect
- "everything that helps is there, and everything that doesn't is left out" (find C.A. quote; this is a paraphrase)
- helps us feel at home, alive, present, comfortable, ourselves
  - we spend more and more of our time using computers. we need this.

### Two responses to a bug report

- "oh weird, how can that even happen? I'll file it in our issue tracker; we'll probably start investigating it in 2 weeks."
- "d'oh! I should have thought of that case. How embarrassing! I'll fix it right now; it should only take a few minutes."

The first response is typical; the second is desirable.

All programmers make mistakes. I'm not claiming I don't write any bugs. I do. But when they're
discovered, I almost always know why they're happening, because they reflect flaws in my reasoning
that, once exposed, are obvious.

Your goal should not be to try to prevent all bugs, but to be able to respond to bug reports
with simple, immediate fixes rather than protracted, head-scratching investigations. The way
to do this is to understand your code. Test-driven development can help.

### Joy

### Mental Modeling

the opposite: confusion, feeling like a pinball, anxiety

most code does not help us learn about what it does. We need a holistic understanding of the system (what Peter Naur
calls a _theory_ of the program) before the parts make sense.

The best code teaches us how to solve a problem, and can be read in at least two ways: whole-to-parts or parts-to-whole.

### Bugs come from complexity

- Cite "out of the tar pit"

- essential complexity bugs: the domain or application is complicated, so programmers make mistakes
- accidental complexity bugs: there's a mismatch between the user's model and the system's model
  - e.g. integer overflow. To a user, 2^31-1 isn't special, but to the machine it is.

### Push complexity under the microscope

- complicated computations are relatively easy to deal with; because the code and data can be observed and controlled,
  bugs can be reproduced "in the lab" and reliably fixed. Complicated effects on things outside the computational
  process are relatively hard to deal with; they are harder to observe and control.
- we want to make the program do all the interesting, complicated work as pure computation, with no external effects.

### What "Out of the Tar Pit" Got Wrong

- tests can be useful
- a suite of tests can give us pretty good confidence that the program behaves correctly, if we assume that
  the program's code is _simple_: that is, that it makes no unnecessary distinctions between
  different inputs. Test-driven development is a technique for ensuring that we get code that is as simple
  as it can be while still passing all the tests.

- OOP and accidental state can be useful
  - I have found that OOP works well to control accidental complexity.
  - FP is better suited to essential complexity.
  - sometimes stateful algorithms are needed to avoid exorbitant memory usage.

### The means: Fast Feedback and transparency

When we can see more about what's going on in the system, we can mentally model its inner workings.
When we can try things out and observe the result, we can learn what the system does by experimenting with it.
The quicker we can do this, the quicker we reach a level of comfort where we experience quality.


### A vision of the future

- a bit disingenuous to call this a vision of the future. I've lived it.

- you come into work Monday morning. Failing test on your workstation reminds you where you left off the previous Friday. You fix the code, and within milliseconds, the test results turn blue-green indicating that all 1500 tests in your project are passing. You notice that, with your fix, there's now a redundant conditional in the code, and you remove it. The steady green of the test results confirms that this was a good idea.
- Backlog-based team standup
    - PM watched (via telemetry) some users apparently get confused by the new account-switching UX. He's going to collaborate with the team's designer for the next hour to work out a fix. He'd like one engineer to be there to give rough estimates of cost as they spitball solutions.
    - A quick conference between you and your pair- you decide that since your current feature is close to done, you will solo on the finishing touches and deploy it to the staging environment while your partner joins the PM and designer. You finish and deploy the code to staging in about 15 minutes (since deploying is just pushing to the main git branch; automation does the rest). You spend the next 45 minutes cleaning up some tech debt in a file you worked on recently.
    - Your pair returns. At this point, it's time for the weekly planning meeting. The PM describes the upcoming features and asks the engineers if they have any initial questions

When you return from the meeting, you notice the team's CI monitor is red! A quick check of the git history confirms that you were the one who pushed last, so after a quick conference with your team it is decided that you will work on fixing the build initially. By inspecting the test output, you quickly discover the problem: the CI server's timezone is set to UTC, but a test you added assumes that the system time will be in your local timezone. You fix the test so it will work reliably no matter in which timezone it's run. You commit and push your changes. The whole process takes about 5 minutes.

- next story you pick up is the one your pairing partner helped define this morning. (it's at the top of the backlog)

### Reflection on the vision

- the point is not to avoid making mistakes, but to catch them quickly (ideally, before users are affected) and reduce risk.

### What needs to change?

what is the diff between the vision and reality?

</details><!-- Vision -->

<details>
<summary>

## Glossary

_I use some terms to mean very specific things. what are they?_

</summary>

### Ways of Categorizing Tests

### Formal Test

### Informal Test

### Automated Test

### Manual Test

### Exploratory Testing

### Unit Test

### System Test

### Types of system test

- functional
- integration
- perf
- load
- stress
- recovery
- migration
- journey
- acceptance
- ...

### Fast Test

### Slow Test

### Testing metaphors

- red
- green
- flake
- brittle
- against
- drive
    - in the way that an engine _drives_ a machine. tests provide the motive force
      for development
    - TODO: get a copy of _TDD by example_ and confirm that this is what Kent Beck intended.

### Test Doubles

- double
- dummy
- stub
- spy
- mock
- fake

### Test-Driven Development

- Red-green-refactor
- London-school TDD
- Detroit-school TDD
- the crucial difference: state-based vs. messsage-based assertions.
- you need both of these in your toolkit. State-based assertions
  are more often what you want, though.

### Algebraic Type Systems

- important differences from Java or C-style types
  - no null pointers or null object references
  - union types instead
  - can prove desirable properties of programs
  - check for exhaustive handling of different cases
- Bob Martin warns against embedding null-checking in the type system
  - The warning may be wise if your goal is to minimize changes to code
  - Forcing arguments to be present may complicate testing... but the
    existence of params that are sometimes not used may be
    a design smell! Possible solution: bottom type.

### Computational Process

### The Four Levels of Capability

- data
- computation
- machine
  - a Turing machine runs until it is "done". A suspendable machine can be observed and fiddled with as it computes.
  - can change state (within the OS process)
  - no one talks about these for some reason. people talk about state machines, which are a particular kind of suspendable machine.
  - cite Parnas
  - an OS process is a suspendable machine
- effect

### Accidental and Essential Complexity

</details><!-- Glossary -->

<details>
<summary>

## Epistemology

_Understanding—knowledge—is key to quality. What can be known? Where does knowledge come from?_

</summary>

### Inductive and Deductive reasoning

### The scientific method

### Analogy between science and TDD

### Tests as instruments

### Types as theorems

### What types and tests can and can't do

</details><!-- Epistemology -->

<details>
<summary><h2>Beginning a Project</h2></summary>

### Aim to get useful feedback in 400 milliseconds

- tests
- types
- dev environment
- linters
- easier said than done. how do you make tests that fast?
    - you may need to write your own test framework (for now)
    - mock everything? nope.
    - you will need to change how you design.
- if you can't run all your tests in 400ms, it probably means:
  - your test framework is too slow. popular test frameworks often have tons
    of features that make it possible to test poorly-designed code, but slow down
    all of the tests. E.g. Jest runs every test suite in its own process to allow
    entire JS modules to be mocked out.
    - write your own test framework. If you're using JS, use Taste as a starting point.
  - you're wasting time testing other people's (side-effecting) code. Design your
    system so your tests only exercise your code (and fast, purely functional third-party code)
  - you're testing with too much data.

### Make your dev environment independent of production

### Make installation and configuration trivially easy

- Setting up an installation should be one command
- If the software can run in multiple configurations, make it
  easy for devs to install it in every configuration they will need
  to test.

### Allow multiple installations per machine

### Start with a walking skeleton

### Beware of Complicated Frameworks

- What's the difference between a framework and a library?
  - it's a spectrum, not a hard line.
  - a framework is monistic and all-encompassing. Libraries can coexist, even if they provide
    overlapping functionality (e.g. you can have lodash, underscore, and ramda and call them
    all from the same function). You can't use multiple frameworks in the same code. E.g. a
    UI component can't use both Angular and React—that would be nonsensical. Each of these
    frameworks forms a mini-language for writing UIs, and a given piece of the UI can only be
    written in one language.
  - a library has a simple interface; a framework has a complex one. It may have a lot of
    configuration options that essentially form a mini programming language.
  - Frameworks usually have configuration options; libraries usually don't.
  - The major difference for our purposes: _you call libraries, but frameworks call you_.
    (this is an oversimplification; sometimes libraries call you back. But generally, not until
    you call them).
    - This means that while you can often replace a library with a fake in your tests,
      and then run contract tests against the fake to verify that it behaves the same
      as the library, you cannot do that with a framework. There is no way to run contract
      tests on a framework. You have to write integrated tests, which are slow. The more
      complicated your interactions with the framework, the more slow tests you need.
  - Case study: Giant CI pipeline integrating many scripts using an extremely complicated
    YAML-based config language. It wasn't possible to test that everything worked together
    without deploying the pipeline to the cloud and running it. This took 4 hours and cost $20.
    And it wasn't even a complete test. E.g. if the pipeline succeeded, we couldn't be sure that
    error handling was working correctly.
  - Choose frameworks with simple interfaces that you feel confident leaving untested, or
    covering with just a few integrated tests.

### Deploy immediately and often

- deploying to production or a production-like env should be almost
  the first thing you do.
  (assuming no users are actually looking at prod.)
  if you can't deploy the software, you have nothing.
- establish a lightweight release process. I recommend something like:
  - tag a particular commit as a release candidate, e.g. 1.0.0-rc1
  - follow semver unless you have a reason not to.
  - deploy that commit to a staging environment. For bonus points,
    create a special environment for each release whose subdomain matches
    the release version, e.g. https://1.0.0-rc1.frobozz.com
  - validate that the release is okay.
  - deploy to production.
  - tag the commit you deployed with the release version, e.g. 1.0.0.

### Continuous deployment doesn't mean continuous delivery

Some organizations resist continuous deployment processes because
they mistakenly equate them with continuous delivery.
Continuous deployment does _not_ mean that every change is immediately
released to users.
Decouples technical decisions (is this software ready for an internal
audience) from business decisions (is this software ready for users)

### Avoid project templates that have a short shelf-life

- a fast suite of tests obviates most other developer conveniences.

### Practice README-driven development

### Ensure everyone can run the tests

### Start with manual testing

### Types and tests serve orthogonal purposes. Use both.

- when I say "types" I mean algebraic types.
- TypeScript, Flow, Kotlin, Scala, Rust

### Use linters to find dead code

- avoid line length limits and other superficial style checks that force code to be asymmetrical.

### Make status evident

- CI monitor
- you may not need a CI monitor

</details><!-- Beginning -->

<details>
<summary><h2>Designing Interfaces</h2></summary>

### Designing code means dividing it into parts

- the way we draw the boundaries between parts implies certain
  interfaces and interaction between the parts.
- the interactions at these interfaces largely determine whether
  the code is comprehensible or not.

### How to divide solutions into parts

- choose a split. consider:
  - is each part simple?
  - Is the interaction (e.g. the data that passes between them) simple and comprehensible?
    - a different way to phrase this: is each part easy to test? If I make a mistake in implementation,
      will the test failure be easy to understand, or will I have to carefully comb the test output
      or run a debugger to know what behavior I broke?
    - if I change or add behavior later, and a test fails, will it be clear whether I should change
      the code or the test (TODO: this is getting very abstract; I should move this to a section on test UX)
    - is the data input to each part maximally convenient for that part?
  - Are the parts coupled to their context, so I have to understand the whole before
    I can understand any part? Or can I build up from knowledge of the parts to knowledge of the whole?
  - Could the parts be reused to solve new problems (or new variations on this problem)?
- repeat several times with different splits and pick the best one.
- recurse: consider each part and split it.

- decouple parts by relying on their caller to compose them. rather than have each function call the next one in the processing pipeline,
  have it return some data and let its caller decide what to do with that data.

- heuristic: input/computation/output pattern
  - separate computations from their context/effects
- e.g. "print a multiplication table" can be divided into parts:
  - get the size of the table as user input
  - generate the table as a sequence of lines
  - print a sequence of lines

### How to know when you've gotten the split wrong

- lagging indicators:
  - a function immediately translates its arguments into another form and then
    just uses the translated args; not the originals.
    - example: null checks, type conversions
    - deciding what to do with null, or a parse error etc. is policy, not mechanism—should
      be the caller's responsibility.
  - a function does some calculations and at the very end passes the results to another function.

### Use the typechecker to sanity-check your design

- sketch the interface of each part using types
- write the minimum implementation possible to make the typechecker happy, e.g. `return null`.
  Values that work well for this include null, empty string, empty array, noop function, etc.
- then test-drive each part.
- sometimes the types may force you to write the correct implementation, e.g. `function head<T>(NonEmptyList<T>): T`
  if that happens, awesome. One less thing you have to test.


### Interface design drives tests; tests drive implementation

### Designing top-down

- input/processing/output (Eric Roberts: _The Art and Science of C_)
- phrase the processing steps as app-agnostic utilities.
- hiding "implementation details" at this level is often detrimental.
- the goal should be to state what the program needs to do as straightforwardly as possible.
  Imagine reading the code aloud to another programmer. Would it help them understand
  how the problem gets solved? Or is it too vague, too high-level to actually explain anything?
  Or too low-level and grubby, so the solution is obscured?

### Limiting call depth

- too many layers make the code hard to follow
- a clear separation between mechanism and policy helps greatly.
  The mechanism code can have several layers of calls without causing readability
  problems, since it will seldom need to be read.

### Bring the language up to meet the problem

- pure top-down design breaks a problem into smaller and smaller pieces until the pieces
  are the fundamental operations of the programming language.
- in practice, I've found that designing _only_ top-down results in the creation of a lot
  of hard-to-understand ad-hoc functions, and, ultimately, duplicative code.
- instead, raise the level of abstraction of the "fundamental operations" so the solution
  is easy to express. In other words, design better mechanisms with which to express your
  policy.

### Model-View-Controller

### Adding Viewmodels

### Eliminate accidental state

### How to choose a test subject

- choosing a test subject is designing an interface
- adding tests stabilizes the interface (makes it harder to change). plan accordingly.
- design several collaborating parts at the same time?

### Ensuring integration with types

- you can also write a small number of integration tests. Often, just one is enough.

### Separate Computation from Effects

- a.k.a. "separate decisions from dependencies"

### Focus on the data first

- Fred Brooks quote

### Represent Effects as Values

### Represent the passage of time as a sequence of calls

### Name things after what they are, not what they're used for

- when a function's name refers to its caller, that's often a sign
  of a conceptual dependency cycle.

### Consider the value of reuse across time as well as space

- reused code does not have to be called in multiple places
- it can simply not change while other things change around it

### Use machines to interleave computation with effects

### Domain sandwich

- domain code may call effectful code injected by the entrypoint
  (e.g. main)

### When to use exceptions vs. error returns

</details><!-- Designing Interfaces -->

<details>
<summary><h2>Designing Data</h2></summary>

### Using union types to enumerate possible states

### Clean data before it reaches domain code

### [Types] Make Illegal States Unrepresentable

### [Types] Make Equivalent States Identical

### [Types] Parse, Don't Validate

</details><!-- Designing Data -->

<details>
<summary><h2>Designing to Contracts</h2></summary>

### Use fakes when the order / number of effects isn't critical

### Algebraic properties of contracts

- idempotency
- nilpotency
- associativity
- commutativity
- last-write wins
- first-write wins
- transitivity? how might this apply?

### Contract testing

### Use contract tests as a cross-team collaboration tool

### Reuse ubiquitous interfaces


</details><!-- Designing to Contracts -->

<details>
<summary><h2>Writing Tests</h2></summary>

### Write tests that catch your mistakes

- quote Kent Beck

> Program testing can be quite effective for showing the presence of bugs, but is hopelessly inadequate for showing their absence.
>
> —Edsger Dijkstra

### Calibrate your tests

### As you work, run the tests several times per minute

### Remove all obstacles to getting continuous feedback

- obstacles can be psychological or technical

### Write single-purpose tests

- avoid trying to make tests do double-duty, e.g. test performance
and features. This just makes failures harder to diagnose.
- tests are not the place to document realistic input values.
  Using weird values more clearly demonstrates the ways in which the
  solution is generalized.

### [Testing] Keep test suites flat

### [Testing] Start with the edges

- a.k.a. thorns around the gold

### [Testing] Control your experiments

- a.k.a. set up for success

### [Testing] Distrust test coverage measurements


### [Testing] Use tests to learn about your testing tools

 - use tests to discover what you don’t know by first writing the thing you do know.

### Test contravariance

- as the tests get more specific, code gets more generic
- if you are writing concrete code/config/data, write generic tests to validate it.

</details><!-- Writing Tests -->

<details>
<summary><h2>Refactoring</h2></summary>

### [Style] Make similar things look the same so important differences stand out

- your linter may fight you

### [Refactoring] Use the Flocking Rules to refactor toward symmetry

### Don't just abbreviate duplication; eliminate it.

### Shallow Hierarchies

</details><!-- Refactoring -->

<details>
<summary><h2>Other stuff that needs a home</h2></summary>

### Testability is Worth Designing For

- geepaw calls this "the driven premise"

### Judge code by consilience

- does it harmonize with the user's needs and expectations?
- can the programmers who will maintain it understand how it works? Can they add features to it without adding bugs?
- does it work well with the machine?
  - e.g. memory locality in row-oriented vs. column oriented layouts, arrays vs. linked lists. Arrays (copied on write) may be faster even when immutability is to be maintained. Profile before optimizing!
- you know consilience is good when learning about one domain teaches
  you something useful about the other.

### A program is a theory about how to solve a problem

### Design interfaces, refactor implementations

### Review process, not just code

### Preserve immutability by copying on write

</details><!-- Other stuff that needs a home -->

<details>
<summary>

## Epilogue

_The gist._

</summary>

> The ancient Masters were profound and subtle.<br/>
> Their wisdom was unfathomable.<br/>
> There is no way to describe it;<br/>
> all we can describe is their appearance.<br/>
>
> They were careful<br/>
> as someone crossing an iced-over stream.<br/>
> Alert as a warrior in enemy territory.<br/>
> Courteous as a guest.<br/>
> Fluid as melting ice.<br/>
> Shapable as a block of wood.<br/>
> Receptive as a valley.<br/>
> Clear as a glass of water.<br/>
>
> Do you have the patience to wait<br/>
> till your mud settles and the water is clear?<br/>
> Can you remain unmoving<br/>
> till the right action arises by itself?<br/>
>
> The Master doesn't seek fulfillment.<br/>
> Not seeking, not expecting,<br/>
> she is present, and can welcome all things.<br/>

> In pursuit of knowledge,<br/>
> every day something is added.<br/>
> In the practice of the Tao,<br/>
> every day something is dropped.<br/>
> Less and less do you need to force things,<br/>
> until finally you arrive at non-action.<br/>
> When nothing is [being] done,<br/>
> nothing is left undone.<br/>

> Act without doing;<br/>
> work without effort.<br/>
> Think of the small as large<br/>
> and the few as many.<br/>
> Confront the difficult<br/>
> while it is still easy;<br/>
> accomplish the great task<br/>
> by a series of small acts.<br/>
>
> The Master never reaches for the great;<br/>
> thus she achieves greatness.<br/>
> When she runs into a difficulty,<br/>
> she stops and gives herself to it.<br/>
> She doesn't cling to her own comfort;<br/>
> thus problems are no problem for her.<br/>
>
> What is rooted is easy to nourish.<br/>
> What is recent is easy to correct.<br/>
> What is brittle is easy to break.<br/>
> What is small is easy to scatter.<br/>
>
> Prevent trouble before it arises.<br/>
> Put things in order before they exist.<br/>
> The giant pine tree<br/>
> grows from a tiny sprout.<br/>
> The journey of a thousand miles<br/>
> starts from beneath your feet.<br/>
>
> Rushing into action, you fail.<br/>
> Trying to grasp things, you lose them.<br/>
> Forcing a project to completion,<br/>
> you ruin what was almost ripe.<br/>
>
> Therefore the Master takes action<br/>
> by letting things take their course.<br/>
> He remains as calm<br/>
> at the end as at the beginning.<br/>

> When he makes a mistake, he realizes it.<br/>
> Having realized it, he admits it.<br/>
> Having admitted it, he corrects it.<br/>
> He considers those who point out his faults<br/>
> as his most benevolent teachers.<br/>
> He thinks of his enemy<br/>
> as the shadow that he himself casts.<br/>

> The ancient Masters<br/>
> didn't try to educate the people,<br/>
> but kindly taught them to not-know.<br/>
>
> When they think that they know the answers,<br/>
> people are difficult to guide.<br/>
> When they know that they don't know,<br/>
> people can find their own way.<br/>

> If a country is governed wisely,<br/>
> its inhabitants will be content.<br/>
> They enjoy the labor of their hands<br/>
> and don't waste time inventing<br/>
> labor-saving machines.<br/>
> Since they dearly love their homes,<br/>
> they aren't interested in travel.<br/>
> There may be a few wagons and boats,<br/>
> but these don't go anywhere.<br/>
> There may be an arsenal of weapons,<br/>
> but nobody ever uses them.<br/>
> People enjoy their food,<br/>
> take pleasure in being with their families,<br/>
> spend weekends working in their gardens,<br/>
> delight in the doings of the neighborhood.<br/>
> And even though the next country is so close<br/>
> that people can hear its roosters crowing and its dogs barking,<br/>
> they are content to die of old age<br/>
> without ever having gone to see it.<br/>
>
> —_Tao Te Ching_, trans. Stephen Mitchell

</details><!-- Epilogue -->

<details>
<summary>

## References

_Other people have already said all of this. Here is where to find their work._

</summary>

- Eric S. Roberts: _The Art and Science of C_. Addison-Wesley, 1995.
- Alexis King: "Parse, Don't Validate". Personal blog, 2019. https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/
- Matt Parker: “TDD: The Bad Parts” (video)
- Gary Bernhardt: “Boundaries” (video)
- Gary Bernhardt: “Fast Test, Slow Test” (video)
- [Christopher Alexander: The Timeless Way of Building (book)][Alexander1]
- [Christopher Alexander: The Nature of Order (book)][Alexander2]
- [Kent Beck: Extreme Programming Explained, 2nd ed (book)][Beck1]
- [Fred Brooks: The Mythical Man-Month (book)][Brooks1]
- [Robert M. Pirsig: Zen and the Art of Motorcycle Maintenance (book)][Pirsig1]
- [Sandi Metz and Katrina Owen: 99 bottles of OOP (e-book)][Metz1]
- [Eric S. Raymond: The Art of Unix Programming (html book)][Raymond1]
- David Parnas: "On the criteria to be used in decomposing systems into modules" (pdf paper)
- Michael Nygard: “Uncoupling” (video)
- Tony Hoare: "The Emperor's Old Clothes" (pdf)
- Ben Moseley and Peter Marks: “Out of the Tar Pit” (pdf)
- Gary Bernhardt: “Test Isolation Is About Avoiding Mocks” (html blog post)
- Jim Coplien: “Why Most Unit Testing is Waste” (pdf)
- Stephen Mitchell (translator): Tao Te Ching (book)

</details><!-- References -->
