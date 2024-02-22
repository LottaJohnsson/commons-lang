## Project

Name: Apache Commons Lang

URL: https://github.com/apache/commons-lang

A package of Java utility classes for classes in the java.lang hierarchy.

## Onboarding experience

We initially tried the project Karate but were unable to build it according to the instructions in the documentation. So, we decided to abandon it and try a different project. Apache Commons Lang was our second attempt and the one we ended up proceeding with. This one was straightforward to build with Maven.

## Complexity

1. What are your results for five complex functions?
   * Did all methods (tools vs. manual count) get the same result?
   * Are the results clear?
2. Are the functions just complex, or also long?
3. What is the purpose of the functions?
4. Are exceptions taken into account in the given measurements?
5. Is the documentation clear w.r.t. all the possible outcomes?

parsePattern:
1. Cyclomatic complexity 39 by manual count and 40 by lizard count.
2. Function is both long and complex.
3. The function is called at FastDatePrinter initialization. It parses a given date pattern such as "YYYY-MM-DD" and returns a list of Rule objects.
4. Exceptions are accounted for in the manual count but perhaps not in lizard since this would account for the discrepancy.
5. The main source of complexity is a large switch statement for different tokens such as 'y' and 'h' which represent different date units. Different cases are explained through brief comments but could be further elaborated.

isCreatable: ...

modify: ...

substitute: ...

createNumber: ...

## Refactoring

Plan for refactoring complex code and estimated impact (lower CC, but other drawbacks?):

parsePattern:
The main source of complexity is a large switch statement with 24 cases + default, many of which have additional if statements inside. The cases have common functionality in the sense that they create a new Rule object based on the character and token length.
One way to refactor would be to exploit this common structure and replace the switch statement with a HashMap mapping each character to a function which takes token length as parameter and returns an appropriate rule. This part could also be moved to a separate helper function.
This refactoring decreases cyclomatic complexity significantly (85%) but may add some overhead and reduce readability slightly.

isCreatable: ...

modify: ...

substitute: ...

createNumber: ...

Carried out refactoring (optional, P+): yes

git diff refactoring master

## Coverage

### Tools

Document your experience in using a "new"/different coverage tool.

How well was the tool documented? Was it possible/easy/difficult to
integrate it with your build environment?

We tried three different coverage tools for Java: OpenClover, Cobertura, and JaCoCo. Our attempts with OpenClover and Cobertura were unsuccessful, and it was difficult to find helpful troubleshooting. JaCoCo was well-documented and quite easy to use, though, and we were able to run it through Maven by adding a plug-in to our POM file.

### Your own coverage tool

Show a patch (or link to a branch) that shows the instrumented code to
gather coverage measurements.

The patch is probably too long to be copied here, so please add
the git command that is used to obtain the patch instead:

git diff manualBranchCoverage master

What kinds of constructs does your tool support, and how accurate is
its output?

...

### Evaluation

1. How detailed is your coverage measurement?

Our coverage measurement outputs the IDs of the branches which are not covered by the class-specific unit tests as well as the percentage of branches covered.

2. What are the limitations of your own tool?

Our tool does not account for all ternary operators.

3. Are the results of your tool consistent with existing coverage tools?

There are some minor discrepancies between our tools results and JaCoCo's report because our tool only checks the coverage of each function in its class-specific test file. Some functions, however, have some additional branch coverage from other tests.

## Coverage improvement

Show the comments that describe the requirements for the coverage.

[Report of old coverage](https://htmlpreview.github.io/?https://github.com/LottaJohnsson/commons-lang/blob/presentation/oldCoverage/jacoco/index.html)

[Report of new coverage](https://htmlpreview.github.io/?https://github.com/LottaJohnsson/commons-lang/blob/presentation/newCoverage/jacoco/index.html)

Test cases added:

git diff tests master

Number of test cases added: four per team member (P+).

## Self-assessment: Way of working

Current state according to the Essence standard: ...

Was the self-assessment unanimous? Any doubts about certain items?

...

How have you improved so far?

...

Where is potential for improvement?

...

## Overall experience

What are your main take-aways from this project? What did you learn?

...

Is there something special you want to mention here?

...