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

<details>
<summary>

### Get Useful Feedback in 400 Milliseconds

</summary>

- tests
- types
- dev environment
- linters
- easier said than done. how do you make tests that fast?
    - you may need to write your own test framework (for now)
    - mock everything? nope.
    - you will need to change how you design.

</details>

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

### How to choose a test subject

- choosing a test subject is designing an interface
- adding tests stabilizes the interface (makes it harder to change). plan accordingly.
- design several collaborating parts at the same time?

### Ensuring integration with types

- you can also write a small number of integration tests. Often, just one is enough.

### Separate Computation from Effects

- a.k.a. "separate decisions from dependencies"

### Represent Effects as Values

### Use machines to interleave computation with effects

### Use fakes when the order / number of effects isn't critical

### Types and tests serve orthogonal purposes. Use both.

- when I say "types" I mean algebraic types.

### Use linters to find dead code

- avoid line length limits and other superficial style checks that force code to be asymmetrical.

### Write tests that catch your mistakes

- quote Kent Beck

> Program testing can be quite effective for showing the presence of bugs, but is hopelessly inadequate for showing their absence.
>
> —Edsger Dijkstra

### Ensure that everyone can run the tests

### As you work, run the tests several times per minute

### Remove all obstacles to getting continuous feedback

- obstacles can be psychological or technical

### There are four testing methods that are useful in different situations

- formal vs. informal
- manual vs. automated

### There are two main types of formal automated test

- system vs. unit

### System tests can serve many purposes. Know what yours are for.

- functional
- perf
- load
- stress
- recovery
- ...

### Testability is Worth Designing For

- geepaw calls this "the driven premise"

### There are five kinds of test doubles

- dummy, stub, spy, mock, fake

### There are two schools of thought about test driven development

- london vs. detroit

### There are two approaches to writing assertions

- state-based vs. message-based

### [Design] Separate Computation from Effects

### There are three kinds of code

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

### Judge code by consilience

- does it harmonize with the user's needs and expectations?
- can the programmers who will maintain it understand how it works? Can they add features to it without adding bugs?
- does it work well with the machine?
  - e.g. memory locality in row-oriented vs. column oriented layouts, arrays vs. linked lists. Arrays (copied on write) may be faster even when immutability is to be maintained. Profile before optimizing!

### A program is a theory about how to solve a problem

### Design interfaces, refactor implementations

### Review process, not just code

### Preserve immutability by copying on write

### [Testing] Keep test suites flat

### [Design] Keep the call graph flat

### [Testing] Start with the edges

- a.k.a. thorns around the gold

### [Testing] Control your experiments

- a.k.a. set up for success

### [Testing] Distrust test coverage measurements

### [Style] Make similar things look the same so important differences stand out

- your linter may fight you

### [Refactoring] Use the Flocking Rules to refactor toward symmetry

### When to use exceptions vs. error returns

### Algebraic properties of contracts

### Shallow Hierarchies

### Representing effects as data

### [Testing] Use tests to learn about your testing tools

 - use tests to discover what you don’t know by first writing the thing you do know.

### [Types] Make Illegal States Unrepresentable

### [Types] Make Equivalent States Identical

### [Types] Parse, Don't Validate

</details>

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

</details>

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

</details>
