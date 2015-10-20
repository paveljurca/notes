Test
====

First, I don't know much on [TDD](http://www.ibm.com/developerworks/library/j-test/j-test-pdf.pdf).
It's just I'd like to write down a few lines to keep a future reference for myself.

My __Test__ incentive is
I feel less pressure.
Did it pass? Move on :)
Did I wrong? I'll know then.

> [Without unit tests] You're not refactoring,
> you're just changing shit. — Hamlet D'Arcy
> *[Alexander Gugel](https://twitter.com/alexanderGugel/status/566656504422752257)*

The only tricky (or funny)
part of this might be that
if your test fails you may
not know (at first) whether
there's a bug in your code
or in the test itself.

> Testing can be used very effectively to show
> the presence of bugs but never to show their absence.
> *Edsger W. Dijkstra*

Of course __Test__ is an extra effort
but it pays off in terms of thinking
about the new code you write.
To put it simply: If you can't test it,
you're likely to be wrong.

> If you can't figure out how to write the tests for something,
> how will you figure out how to write the something?
> *[Evan Phoenix](https://twitter.com/evanphx/status/504735308932333568)*

- *Test* fixture (contract, behaviour) but **not** the implementation

- No *happy-path* testing; destroy that code

- The very single responsibility, i.e. a class or method does too much

> Don't ask for the data that you need to do something;
> ask the object that has the data to do the work for you.
> *[Allen Holub](https://youtu.be/HZyRQ8Uhhmk)*

- Beware of [code smells](http://c2.com/cgi/wiki?CodeSmell), i.e. a constructor with unreachable objects

- Comments are there to clarify intentions, quirks, i.e. say what the code does not

*Code a little, test a little, redo*. Considering the __Test__ itself..

- **No** multiple (unrelated) assertions, i.e. do more @Test methods

- Test **cannot** be more complicated than the code you test

So to get us going..

0. set up a [test fixture](https://github.com/junit-team/junit/wiki/Test-fixtures)
1. declare `_this` results
2. run
3. store `_that` results
4. assert *_this* matches *_that*

`assertEquals("Size wrong", _this, _that);`

<script src="https://gist.github.com/paveljurca/57deec705e09ac4070fc.js"></script>

And [Perl Testing: A Developer's Notebook](http://shop.oreilly.com/product/9780596100926.do)
which I came across a week ago. It's The book.

[Unit tests](http://martinfowler.com/bliki/TestDrivenDevelopment.html) might have
[some](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html)
[dark sides](http://www.rbcs-us.com/documents/Why-Most-Unit-Testing-is-Waste.pdf).
And there's even a [Readme Driven Development](http://tom.preston-werner.com/2010/08/23/readme-driven-development.html)
or many other [test levels](https://en.wikipedia.org/wiki/Software_testing#Testing_levels).
To conclude, look at [these lectures](http://d3s.mff.cuni.cz/teaching/programming_practices/lecture12.html)
or [Thing I Have Learned About Software Testing](http://qntm.org/test).


* __shownotes__
* [http://sscce.org/]
* [The Testability Explorer Blog](http://misko.hevery.com/2008/11/04/clean-code-talks-unit-testing/)
* [Code Review Best Practices](http://kevinlondon.com/2015/05/05/code-review-best-practices.html)
* [http://en.wikipedia.org/wiki/Action_at_a_distance_%28computer_programming%29]
* [http://en.wikipedia.org/wiki/Single_responsibility_principle]
* [http://en.wikipedia.org/wiki/God_object]
* [http://martinfowler.com/articles/injection.html]
* [framework X library](http://stackoverflow.com/questions/148747/what-is-the-difference-between-a-framework-and-a-library/148788#148788)
* [http://www.oracle.com/technetwork/java/codeconventions-150003.pdf]
* [http://www.javaworld.com/javaworld/jw-01-2004/jw-0102-toolbox.html]
* [http://www.oracle.com/technetwork/articles/javase/index-142890.html]
* [Testcase or @Test](http://stackoverflow.com/questions/2635839/junit-confusion-use-extend-testcase-or-test)


