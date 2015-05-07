Test Driven Development
=======================

Code a little, test a little, **redo**.

You test a fixture (contract, behaviour)
but *NOT* the implementation.

No happy path testing, you intend to break
your code.

Single responsibility of tested classes
(methods), i.e. a class does too much.

NO code smells, i.e. a constructor
with unreachable objects.

NO multiple assertions, rather having more
test methods.

A test cannot be more complicated than
a code under the test.


comments say only what the code does not say,
i.e. your intentions, technical quirks or so.



There's a funny thing 

If keep failing, you don't know whether it's
that code under testing or the test itself :)


actually my first @Test ever,
more than 2 years old


<script src="https://gist.github.com/paveljurca/57deec705e09ac4070fc.js"></script>

[HraTest.java](https://github.com/paveljurca/adventura/blob/master/test/slepec/hra/HraTest.java)

0. set up a [test fixture](https://github.com/junit-team/junit/wiki/Test-fixtures) first 
1. declare the expected results
2. exercise the unit under a test
3. get the actual results
4. assert that they match the expected results

`assertEquals("Calculation wrong", expResult, result);`


++ pridej Perl findAirport test!!



There's even a [Readme Driven Development](http://tom.preston-werner.com/2010/08/23/readme-driven-development.html) ;)


PARADIGMA
http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html
>>TDD is dead
http://www.rbcs-us.com/documents/Why-Most-Unit-Testing-is-Waste.pdf


> Don't ask for the data that you need to do something;
> ask the object that has the data to do the work for you.
> *Allen Holub*

For more I've found [these lectures](http://d3s.mff.cuni.cz/teaching/programming_practices/lecture12.html) or [Thing I Have Learned About Software Testing
](http://qntm.org/test) simply great!

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

