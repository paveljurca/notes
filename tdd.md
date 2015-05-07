Test Driven Development
=======================

First, I should confess I'm no
pro on TDD by any means.
It's just I'd like to list
a few (obvious) notes I've get
to know alongside.

My TDD's incentive is that
I feel less pressure knowing
the tests sit there, so they
will tell me if my last added
block of code broke everything.
Did it passed? Then move on :)

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">[Without unit tests] You&#39;re not refactoring, you&#39;re just changing shit. — Hamlet D&#39;Arcy</p>&mdash; Alexander Gugel (@alexanderGugel) <a href="https://twitter.com/alexanderGugel/status/566656504422752257">February 14, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

The only tricky (or funny)
part of this might be that
if your test fails you may
not know (at first) whether
there's a bug in your code
or in the test itself.

Of course TDD is an extra effort
but it pays off in terms of thinking
about a new code you're gonna write.
To put it simply: if you can't test it,
you're probably doing it wrong.

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">If you can&#39;t figure out how to write the tests for something, how will you figure out how to write the something?</p>&mdash; Evan Phoenix (@evanphx) <a href="https://twitter.com/evanphx/status/504735308932333568">August 27, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

- Test a fixture (contract, behaviour)
but *NOT* the implementation

- No happy path testing, you intend
to break that code

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
3. get real results
4. assert that they match the expected results

`assertEquals("Calculation wrong", expResult, result);`

I've got some example tests in Perl or Java
(but especially that Java code's not good
for it was my first test ever). Anyway,
here they are:

<script src="https://gist.github.com/paveljurca/57deec705e09ac4070fc.js"></script>

There's even a [Readme Driven Development](http://tom.preston-werner.com/2010/08/23/readme-driven-development.html)
or some [bad](http://www.rbcs-us.com/documents/Why-Most-Unit-Testing-is-Waste.pdf)
[points](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html)
about TDD.
I've also found [these lectures](http://d3s.mff.cuni.cz/teaching/programming_practices/lecture12.html)
and [Thing I Have Learned About Software Testing](http://qntm.org/test)
simply great.

(pj)

* __shownotes__
* [http://sscce.org/]
* [http://en.wikipedia.org/wiki/Action_at_a_distance_%28computer_programming%29]
* [http://en.wikipedia.org/wiki/Single_responsibility_principle]
* [http://en.wikipedia.org/wiki/God_object]
* [http://martinfowler.com/articles/injection.html]
* [framework VS. library](http://stackoverflow.com/questions/148747/what-is-the-difference-between-a-framework-and-a-library/148788#148788)
* [http://www.oracle.com/technetwork/java/codeconventions-150003.pdf]
* [http://www.javaworld.com/javaworld/jw-01-2004/jw-0102-toolbox.html]
* [http://www.oracle.com/technetwork/articles/javase/index-142890.html]
* JUnit [testcase or @test](http://stackoverflow.com/questions/2635839/junit-confusion-use-extend-testcase-or-test)

