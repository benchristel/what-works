# What Works

<details>
<summary>
  
## Introduction
  
_where did this book come from and why am I writing it?_

</summary>

- I aim to write software that doesn't have any bugs—that does exactly what I intend it to do.
- this book is a list of things I have tried that work
- mostly invented by other people
- some of this stuff has been known for 50+ years

### Advice about software development is contextual. Stay skeptical.

### This is all stuff that has actually worked in practice.

### This book can provide the start of a pattern language for your team.

### If it hurts, backtrack and try something else

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

</details>

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

### Mental Modeling

the opposite: confusion, feeling like a pinball, anxiety

### The means: Fast Feedback and transparency

When we can see more about what's going on in the system, we can mentally model its inner workings. When we can try things out and observe the result, we can learn what the system does by experimenting with it. The quicker we can do this, the quicker we reach a level of comfort where we experience quality.


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

</details>

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

### Fast Test

### Slow Test

### Testing metaphors

- red
- green
- flake
- brittle
- against

### Test Doubles

- double
- dummy
- stub
- spy
- mock
- fake

### Test-Driven Development

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
- effect

</details>

<details>
<summary>

## Epistemology
  
_Understanding—knowledge—is key to quality. What can be known? Where does knowledge come from?_
  
</summary>

### Inductive and Deductive reasoning

### The scientific method

### Analogy between science and TDD

### Tests as instruments

### Types are Proofs

### What types and tests can and can't do

</details>

<details>
<summary>

## Technique
  
_how?_
  
</summary>

## You are good enough; now fix your tools

## Get Feedback in 400 Milliseconds

## Types and tests serve orthogonal purposes. Use both.

- when I say "types" I mean algebraic types.

## Write tests that catch your mistakes

- quote Kent Beck

> Program testing can be quite effective for showing the presence of bugs, but is hopelessly inadequate for showing their absence.
>
> —Edsger Dijkstra

## Ensure that everyone can run the tests

## As you work, run the tests several times per minute

## Remove all obstacles to getting continuous feedback

- obstacles can be psychological or technical

## There are four testing methods that are useful in different situations

- formal vs. informal
- manual vs. automated

## There are two main types of formal automated test

- system vs. unit

## System tests can serve many purposes. Know what yours are for.

- functional
- perf
- load
- stress
- recovery
- ...

## Testability is Worth Designing For

- geepaw calls this "the driven premise"

## There are five kinds of test doubles

- dummy, stub, spy, mock, fake

## There are two schools of thought about test driven development

- london vs. detroit

## There are two approaches to writing assertions

- state-based vs. message-based

## [Design] Separate Computation from Effects

## There are three kinds of code

- computation / pure functions
  - what a Turing Machine can compute
- suspendable machines
  - a Turing machine runs until it is "done". A suspendable machine can be observed and fiddled with as it computes.
  - can change state (within the OS process)
  - no one talks about these for some reason. people talk about state machines, which are a particular kind of suspendable machine.
  - cite Parnas
  - an OS process is a suspendable machine
- procedures
  - affect process-external state

## Judge code by consilience

- does it harmonize with the user's needs and expectations?
- can the programmers who will maintain it understand how it works? Can they add features to it without adding bugs?
- does it work well with the machine?
  - e.g. memory locality in row-oriented vs. column oriented layouts, arrays vs. linked lists. Arrays (copied on write) may be faster even when immutability is to be maintained. Profile before optimizing!

## A program is a theory about how to solve a problem

## Design interfaces, refactor implementations

## Review process, not just code

## Preserve immutability by copying on write

## [Testing] Keep test suites flat

## [Design] Keep the call graph flat

## [Testing] Start with the edges

- a.k.a. thorns around the gold

## [Testing] Control your experiments

- a.k.a. set up for success

## [Testing] Distrust test coverage measurements

## [Style] Make similar things look the same so important differences stand out

- your linter may fight you

## [Refactoring] Use the Flocking Rules to refactor toward symmetry

## When to use exceptions vs. error returns

## Algebraic properties of contracts

## Shallow Hierarchies

## Representing effects as data

## [Testing] Use tests to learn about your testing tools

 - use tests to discover what you don’t know by first writing the thing you do know.

## [Types] Make Illegal States Unrepresentable

## [Types] Make Equivalent States Identical

## [Types] Parse, Don't Validate

</details>
