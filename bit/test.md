@Test
=====

First, I should confess I'm no
pro on [unit tests](http://www.ibm.com/developerworks/library/j-test/j-test-pdf.pdf)
by any means.
It's just I'd like to write
down a few remarks to keep
a future reference for myself.

My `@Test` incentive is that
I feel less pressure knowing
the "magic" sits there, so
it's gonna tell me if my last added
block of code broke everything.
Did it passed? Then move on :)

> [Without unit tests] You're not refactoring,
> you're just changing shit. — Hamlet D'Arcy
> *[Alexander Gugel](https://twitter.com/alexanderGugel/status/566656504422752257)*

The only tricky (or funny)
part of this might be that
if your test fails you may
not know (at first) whether
there's a bug in your code
or in the test itself.

Of course a `@Test` is an extra effort
but it pays off in terms of thinking
about a new code you're gonna write.
To put it simply: if you can't test it,
you're likely to be wrong.

> If you can't figure out how to write the tests for something,
> how will you figure out how to write the something?
> *[Evan Phoenix](https://twitter.com/evanphx/status/504735308932333568)*

- Test a fixture (contract, behaviour)
but *NOT* the implementation

- No happy path testing, you intend
to break (humiliate) your own code

- Single responsibility of tested classes
(methods), i.e. a class does too much

> Don't ask for the data that you need to do something;
> ask the object that has the data to do the work for you
> *Allen Holub*

- Beware of [code smells](http://c2.com/cgi/wiki?CodeSmell),
i.e. no constructor with unreachable objects

- Comments are not yet another layer of code,
use them to clarify your intentions, quirks etc.

- Code a little, test a little, **redo**

And considering the test itself..

- No multiple (unrelated) assertions,
rather have more @Test methods

- A test *cannot* be more complicated than
the actual code we run the test for 

0. set up a [test fixture](https://github.com/junit-team/junit/wiki/Test-fixtures) first 
1. declare expected results
2. run your code
3. get actual results
4. assert that they match the expected results

`assertEquals("Calculation wrong", expResult, result);`

I've got some example tests in Perl or Java
(but especially that Java code's not the best one
for it was my first test ever). Anyway,
here they are:

<script src="https://gist.github.com/paveljurca/57deec705e09ac4070fc.js"></script>

[TDD](http://martinfowler.com/bliki/TestDrivenDevelopment.html) might have
[some](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html)
[dark sides](http://www.rbcs-us.com/documents/Why-Most-Unit-Testing-is-Waste.pdf).
FYI there's even a [Readme Driven Development](http://tom.preston-werner.com/2010/08/23/readme-driven-development.html)
and likewise many other [test levels](https://en.wikipedia.org/wiki/Software_testing#Testing_levels).
I've also found [these lectures](http://d3s.mff.cuni.cz/teaching/programming_practices/lecture12.html)
and [Thing I Have Learned About Software Testing](http://qntm.org/test)
simply great.

(pj)

* __shownotes__
* [http://sscce.org/]
* [The Testability Explorer Blog](http://misko.hevery.com/2008/11/04/clean-code-talks-unit-testing/)
* [Code Review Best Practices](http://kevinlondon.com/2015/05/05/code-review-best-practices.html)
* [http://en.wikipedia.org/wiki/Action_at_a_distance_%28computer_programming%29]
* [http://en.wikipedia.org/wiki/Single_responsibility_principle]
* [http://en.wikipedia.org/wiki/God_object]
* [http://martinfowler.com/articles/injection.html]
* [framework VS. library](http://stackoverflow.com/questions/148747/what-is-the-difference-between-a-framework-and-a-library/148788#148788)
* [http://www.oracle.com/technetwork/java/codeconventions-150003.pdf]
* [http://www.javaworld.com/javaworld/jw-01-2004/jw-0102-toolbox.html]
* [http://www.oracle.com/technetwork/articles/javase/index-142890.html]
* JUnit [testcase or @test](http://stackoverflow.com/questions/2635839/junit-confusion-use-extend-testcase-or-test)

