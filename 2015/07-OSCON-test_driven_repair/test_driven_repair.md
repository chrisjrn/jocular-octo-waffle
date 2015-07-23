# Test-Driven Repair
## A discussion of test patterns for code that was written without tests.

Christopher Neugebauer <br />
mail<b>@chrisjrn</b>.com

##### Notes
This is a talk about what happens when you forget to write tests, and you suddenly discover that you actually need tests.


## Australian
<div class="stretch">
![Australian](images/bruce.jpg)
</div>

##### Notes
I'm Australian, I'll speak Australian. Sorry.


## Home
<div class="stretch">
![Hobart](images/spectra.jpg)
</div>

##### Notes
I'm from Hobart.

It's a lot like Portland, except that we appear to have an actual port.

It's also very cold there at the moment, so I'm rather glad to be here instead.


## What I knew about testing when I wrote the abstract?
<div class="stretch">
![I Have No Idea What I'm Doing](images/science-dog.jpg)
</div>

##### Notes
I wrote the abstract after spending two weeks fixing a 3000-line piece of enterprise Java code at work. It was barely tested.


## The abstract was wrong.

##### Notes
What I've learned in the six months between writing the abstract for this talk, and actually producing the talk is that everything I thought was some generally useful knowledge actually wasn't.

Hopefully this talk is still useful.



# Testing Done Right
## What? Where? Why?

##### Notes
We start this talk with a discussion of testing. Specifically test-driven development.
It's pretty popular at the moment, and it usually results in pretty good code.


## Test-Driven Development
* Step 1: Write a failing test
* Step 2: Write some code that makes the test pass
* Step 3: Repeat until all your code is done

##### Notes
Test-Driven development is all about doing the bare minimum amount of work to make all of your tests pass.
If your entire requirements are specced by tests, then a passing test suite indicates you've met your requirements.


## Not Quite TDD
* Testing during review
* Testing as bug reports emerge

##### Notes
Other approaches can be used to write tests early in the process -- such as making it a mandatory part of your code review process. Or making it policy to produce a test case whenever a bug report comes in.


## Structured Testing
* *Unit Testing*: One element of functionality works.
* *Integration Testing*: Unit-tested elements work together

##### Notes
When talking about testing in this talk, I'm using these definitions --
* unit tests are tests for small and isolated parts; you have lots of them.
* integration tests test that these parts work together; ideally you have very few, because they're slow.


## Structured Testing
* Write unit tests first
* Write units
* Write integration tests

##### Notes
Structured testing usually results in units and unit tests being written first.


## Why write tests early?
* Separated interfaces
* Clear separation of concerns (mocks, fakes, etc)

##### Notes
Code where you've thought about testing from an early point in the process generally has well thought out interfaces that are easy to test.


## Why write tests early?
* I/O (if not all state) separate from business logic
  * Lots of unit tests (fast)
  * Relatively few integration tests (slow)

##### Notes
Also, considering tests early often leads to state and I/O being separated from business logic -- this is because state leads to isolation issues in tests, and I/O makes tests slow.


## Why TDD?
You get tests from the moment you get code.

##### Notes
People like TDD because it's a way of making sure that you have tests planned out as soon as your actual code comes into existence.


## TDD Zealotry
> “it is impossible BY DEFINITION to do Test-Driven Development after the code is written.”

— [Tim Ottinger](https://twitter.com/tottinge/status/544632253205475329), author of ‘Clean Code’, on Twitter

##### Notes
You get statements like this, which are clearly strongly-held opinions.

You can tell by the capital letters.


## LIES.

##### Notes
Statements like Tim's aren't actually that useful. Firstly because it suggests that testing is a lost cause on old codebases.


## People have old code that should be tested.
> The unfortunate thing is that much design and process advice assumes that your project is a blank page. In actuality, greenfield projects are noticeably rare. Most projects carry some amount of legacy code.

– [Michael Feathers](http://www.objectmentor.com/resources/articles/WorkingEffectivelyWithLegacyCode.pdf), 2002.

##### Notes
Michael Feathers, in his paper (and subsequent book), observed that very few people get to start on a greenfields project.

More than that, he notes that old code without tests often gets left untested, and becomes a huge maintenance burden into the future.


## You *can* adopt early-stage testing on future releases.

##### Notes
Once you realise you have a lack of tests, continuing with a lack of tests seems like a bad idea.

The aim should be to transform your codebase into one where you have a shot at early-stage testing is possible, and easy.



# So you have code that needs tests?

##### Notes
Not everyone has the freedom to start their codebase from scratch. Adopting a test-driven development approach isn't immediately possible.


## Untested Code
* Poor Interfaces
* Poor separation of concerns
* Dependencies tied together

##### Notes
Code that's left without testing for long enough tends to develop undesirable features for implementing unit tests later on:


## Poor Interfaces
Lots of setup required to test one function


## Poor Interfaces
The only way to execute code in one function may involve burrowing through other functions.


## Poor Separation of Concerns
Impossible to test results of operations without altering state or performing I/O.


## Tied Dependencies
You need to invoke code in other subsystems to run code in the subsystem under test.

##### Notes
* dependency injection
  * makes it very easy to pile up objects that will need to be mocked to be able to be tested. Use with caution


## When your code is coupled,
### "units" for testing do not exist.

##### Notes
The "units" that you unit test only come about if you have well-defined interfaces.


## If you don't have units,
### integration isn't well defined.

##### Notes
And integration tests only make sense if you have unit tested parts to put together.

The standard structured approach to testing doesn't really apply here.


## How Do You Test?
<div class="stretch">
![Monty Python Meat Grinder](images/gilliam-meatgrinder.jpg)
</div>

##### Notes
And when they realise that it's hard to easily write a test, most people don't. This is a bad idea.

So what does a testing strategy look like if you don't adopt the unit/integration behaviour that TDD recommends?

When we're repairing old code, we need something different.



# Test Coverings

##### Notes
Michael Feathers coined the idea of "test coverings", which is a different way to think about structuring your tests.


## Test Coverings
Tests that cover all of the subsystem under investigation.

##### Notes
Test coverings are a suite of tests that seek to cover all of the code in the part of your system that you're trying to fix.

There's not much more to define than that.


## Interface Testing
Testing everything that is reachable, with the interface you already have.

##### Notes
The most basic type of testing you can do to code is interface testing: take whatever interface you have available, put it under a test harness, and write test cases.

Often such tests will not give particularly specific information about specific bugs.


## What is an Interface?
Anything that gives OK isolation to the code you want to test.

##### Notes
OK islation, with minimal side effects introduced by other parts of your system. You probably won't be able to avoid isolating other parts of your system completely.


## Interfaces for testing
*GOOD*: A custom test harness

##### Notes
The ideal for an interface test is being able to extract just the code you want to test and wrap it in a custom testing harness.

Indeed, the Feathers essay thought that making custom test harnesses was the most important thing you can do.

If your code is easy enough to wrap in another product, then maybe so! If not…


## Interfaces for testing
*OK*: Web APIs running in-place in your server

##### Notes
Sometimes extracing code and putting it into a custom harness may be too hard. You need to be pragmatic.

If your system has a web service or a REST API that exercises the right parts of your code. *this is a perfectly fine test harness*.


## Interfaces for testing
*MEH*: A web front-end running in-place on your server

##### Notes
If you don't have a web API, but you do have a browser interface, then maybe wrapping an interface with webdriver may help? Aim for isolation.


## Any interface that lets you get tests in early is a good interface.

##### Notes
Find whatever technology you can get that will *let* you write tests early.


## Units or Integrations?
Interface tests are neither.
Do not expect unit-level precision.

##### Notes
In particular, an interface test will not give unit-level granularity.

Unit tests can usually be run in isolation to test the function you're repairing. If you're testing a much bigger system, looking at a single test may not make sense.


## What do you get then?
<div class="stretch">
![Confuse a Cat](images/confuse-a-cat.jpg)
</div>

##### Notes
Well?


## Invariants
Tests induce an _invariant_ in the code under test.

##### Notes
What a covering _does_ get you is to allow you to observe invariants in your code.
Invariants are the fixed, observed properties of your system. A single passing test case is an invariant.


## Invariants
Invariants do not change after refactoring.

##### Notes
When you have invariants, you can refactor code around that invariant, because you can tell if the behaviour has changed.

Establishing invariants mean you can start refactoring with some impunity.


## Invariants
Invariants only exist on code under test.

##### Notes
OK.


## Unit tests give invariants with better precision
* You can tell what failed
* You can tell how you made it fail


## Interface tests need lots of coarse-grained invariants
* You can tell that a case failed
* You can't (easily) tell what made it fail

##### Notes
Making a test at the interface fail gives you useful information -- a test has failed.

What it can't tell you is with any precision *where* in the code path the tests failed.

Analysing _multiple_ failing tests that touch similar code paths are far more likely to reveal causes of failures.


## Interface test invariants
* Relatively few invocation paths
* Lots of different inputs
* Classify into valid inputs and invalid inputs

##### Notes
This suggests that it's often a good idea to adopt an approach where you have lots of tests where the lines of code that comprise the test don't change, but the input data does.


## It doesn't quite test what you want
### but at least you *get* tests

##### Notes
So even if you don't get tests that tell you direct, fine-grained results, you still get tests. That's a good thing, you shouldn't be unhappy with that.


## Test Coverage
How you know what your tests actually test, and what your next test should be.

##### Notes
It's also vital to know what code is actually covered by the invariants that you create. This is how you can justifiably refactor.


## Lines of Code Covered
```python
def cook_spam():
  print("1. Preheat the oven.")
  print("2. Put spam into oven")
  print("3. Sing the spam song")
  print("4. Tell customer their spam will be ready")
  print("5. Fight off horde of vikings")
  print("6. Serve the spam")
```

##### Notes
Simple line coverage is good for seeing broadly which bits of code need work on tests. Getting big gains on lines covered is easy though -- big functions with minimal branching can get you gains of tens or hundreds of lines with minimal effort.

This 6-line function needs only 1 test to achieve full coverage.


## Branches Covered

```python
def cook_thing(thing):   # Needs four tests for full coverage
  if thing == "spam":
    cook_spam()
  elif thing == "eggs":
    cook_eggs()
  elif thing == "bacon":
    cook_bacon()

def cook_spam():         # Needs ony one test
  print("1. Preheat the oven.")
  print("2. Put spam into oven")
  print("3. Sing the spam song")
  print("4. Tell customer their spam will be ready")
  print("5. Fight off horde of vikings")
  print("6. Serve the spam")
```

##### Notes
Checking for branch coverage is often a better bet -- you can back-track from the branch points to figure out what data you need to put in to make a branch light up.

Basically, unlit branches should give you clues about *what data* you should offer to your tests.

In this case, a 6-line function needs three tests to achieve full branch coverage.


## Use metrics that indicate effort.
### Good interface tests will test every *branch*

##### Notes



# Making Interface Tests Bearable

##### Notes
Interface tests are terrible -- they're complicated to write, they're imprecise and slow. Let's talk about how to make these slightly more bearable.


## I/O and State
If it's embedded in your code, don't expect to remove it before you write tests.

##### Notes
If I/O and statefulness is embedded in the interface you're testing, it's difficult to get rid of it. Usual techniques like mocking and faking only apply if you can extract an interface from the service-level you're trying to write.

Do that refactor *after* you have tests.


## Speed
Testing at the interface is slooooooow.

##### Notes
The problem here is that I/O makes tests very slow. While you're working to refactor a system, you want the tests that you've written to be accessible.


## Correctness vs Speed
* A correct suite runs after every test case in a sanitised environment.
* A fast suite rarely sanitises.

##### Notes
If your tests require you to do IO, then it's likely a large amount of your test time is spent setting up and tearing down.

This is necessary to maintain complete test isolation, but it comes at the expense of making your suite slow.


## Side-Effects of Isolation
* A slow suite will never get run
* A fast, but incorrect suite will waste developers' time.

##### Notes
Slow test suites are an inconvenience, and may well get neglected. Incorrect suites, are likely to create further issues down the track, from chasing down non-existent bugs.


## Measure Every Test Case
* Setup time
* Execution time
* Teardown time

##### Notes
measure everything -- know where time is being wasted, and where you can save time in exchange for absolute correctness.


## Cluster Unrelated Tests
Find tests that alter different parts of the environment.

Run them under the same test fixtures.

##### Notes
Clustering can, to a certain extent, obviate the need for isolation and sanitisation of tests.

Local isolation means you can run multiple tests in the same environment without hurting correctness.


## Cluster Unrelated Tests
Run each cluster, multiple times, in random orders.

##### Notes
If the execution time for cluster is fast compared with running each test with proper isolation, you can run the cluster two or three times in random orders to verify that the tests aren't dependent on each other.


## Run Thoroughly Nightly

##### Notes
Verify that there's no actual dependency between tests by running each test with full sanitisation on a nightly basis.


## Don't make this your long-term testing strategy.
### Refactor immediately.

##### Notes
This approach yields tests that are slow, non-specific, and cumbersome to write in quantity. These should not be used as a testing strategy.

Broken interface tests tell you that overall behaviour has changed, not *where* it has changed, so use these tests as a way to check if overall behaviour has changed as you change your structure.



# A Refactoring Strategy

##### Notes
With that in mind, let's turn to refactoring. The big advantage of having your code covered by tests is that you *can* refactor with impunity.

Let's look at a refactoring strategy that I've used once to stop relying on these cumbersome tests.


## Uncle Bob's Clean Architecture

![The Clean Architecture](images/clean-architecture.jpg)

– [Uncle Bob](https://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html)

##### Notes
Uncle Bob's *Clean Architecture* describes code structure where frameworks, external I/O, and user interface sit on the outside, and core business logic sits on the inside.

This is enforced by the dependency rule, which says that an inner layer isn't allowed to know anything about code that exists in an outer layer.


## Unclean Architecture

```python
def find_definition(word):
    q = 'define ' + word
    url = 'http://api.duckduckgo.com/?'
    url += urlencode({'q': q, 'format': 'json'})
    data = call_json_api(url)
    definition = data[u'Definition']
    if definition == u'':
        raise ValueError('that is not a word')
    return definition

def call_json_api(url):
    response = requests.get(url)     # I/O
    data = response.json()           # I/O
    return data
```
-[Brandon Rhodes, *Clean Architecture in Python*](http://rhodesmill.org/brandon/slides/2014-07-pyohio/clean-architecture/)

##### Notes
Brandon Rhodes' PyOhio talk on the Clean Architecture gives a nice simple example of how this works in practice.

This example shows the wrong way of refactoring -- pulling I/O out into a separate function, and leaving the processing code at the top.

This means you need to do mocking or dependency injection in order to test.


## Clean Architecture

```python
def find_definition(word):
    url = build_url(word)
    data = requests.get(url).json()  # I/O
    return pluck_definition(data)

def build_url(word):
    q = 'define ' + word
    url = 'http://api.duckduckgo.com/?'
    url += urlencode({'q': q, 'format': 'json'})
    return url

def pluck_definition(data):
    definition = data[u'Definition']
    if definition == u'':
        raise ValueError('that is not a word')
    return definition
```
-[Brandon Rhodes, *Clean Architecture in Python*](http://rhodesmill.org/brandon/slides/2014-07-pyohio/clean-architecture/)

##### Notes
The alternative is to leave the I/O-bound functions at the top, and pull out the processing code into functions.

These processing functions, provided you can set up the data they expect, are easy to test in isolation.


## Functional Core--Imperative Shell
–Gary Bernhardt, *Boundaries (2012, 2013)*
* Write processing logic as pure functions
* Isolate I/O as close to the top as you can.

##### Notes
Gary Bernhardt's *Boundaries*, as well as giving the Clean Architecture a cooler name, talked about using this approach to think about structuring code so that you minimise the amount that you need to mock/fake when testing.


## Testing with FCIS
Unit tests are quick because pure functions are fast.

##### Notes
The idea is that if your core processing logic is expressed as pure functions, you can unit test them thoroughly, and those tests will run quickly.


## Testing with FCIS
Integration tests are easy because you can write lots of unit tests.

##### Notes
And the integration tests are just the tests that execute at the top level of your system -- where the I/O potentially happens. You have relatively few of these tests, and you only need to fake, or test under I/O at a relatively central point.


## This is not just an architectural decision.
### It's a refactoring strategy too

##### Notes
The core observation here is that the Functional Core-Imperative Shell approach also works at a local level. Any function that calls I/O is integration tested; any function that doesn't can get unit tests.

So increasing your unit test coverage is just a matter of factoring out small, unit testable functions.


## FCIS at Smaller Scales
* Find processing routines inside bigger functions
* Factor out testable functions
* Factor inner loops out of those too!


## FCIS at Smaller Scales
* Keep I/O at the top
* Factor fast code into their own functions.


## Test After You Refactor
These emergent functions are unit testable, so write some tests.


## Once you refactor into units and test them,
Your interface tests *become* integration tests.

##### Notes
The tests you wrote originally are now integration tests for unit tested components! Magic!


## Taking unstructured testing seriously means you'll start doing structured testing.

##### Notes
Starting with unstructured testing can still eventually lead to good code coverage.



# Tracking Down Bugs

##### Notes
We end the talk with a short discussion of actually tracking down bugs in code.


## Test Harnesses
Make it easy to translate a bug report into a test case.

##### Notes
Consider how your code might be invoked in production by a user. Can you reproduce an invocation of some of your system in a test harness?

If so, then you can capture every bug report at the interface as a test, and spot regressions as they arise.


## Data Scales, Code Doesn't
Express cases as parameters to a function rather than writing new procedures.

##### Notes
Writing new code to get an interface into a suitable way for testing is hard. Write tests where you can expose data-level cases rather than actual tests. Data is faster to write than code.


## Retain your cases
Keep these test cases, and run them regularly.

##### Notes
Whilst interface tests can be slow and cumbersome, and you clearly don't need as many once you have lots of unit-level tests…

Keeping test cases around is sensible. Just don't run them as frequently.




# Not the talk I thought I'd give

##### Notes
Parting thoughts --
I wrote the abstract for this talk six months ago whilst undertaking a substantial refactor of some very important code.

One thing that I *have* learned since then is that specific advice doesn't always apply. Everyone's code has its own mix of issues that make testing it difficult. Not every technique I've described here might work in your situation.


## Test-Driven Development
* Write Unit Tests
* Write Units
* Test end-to-end functionality with integration tests


## Test-Driven Repair
* Write Integration Tests
* Refactor into units
* Write unit tests

##### Notes
Take-home message: It doesn't hurt to define integration tests before unit tests. Having any tests _at all_ means you can fix your code.


## Start with Test-Driven Repair
### Enable Test-Driven Development

##### Notes
It is not *impossible* to practice test-driven development once the code is already written… you just need to get to a point where you can write tests.


# The End
## Test-Driven Repair

Christopher Neugebauer <br />
mail<b>@chrisjrn</b>.com
