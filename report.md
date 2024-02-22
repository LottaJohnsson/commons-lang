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
The cyclomatic complexity 39 by manual count, 40 by lizard count, and 39 by jacoco. The function is both long and complex. It is called at FastDatePrinter initialization. It parses a given date pattern such as "YYYY-MM-DD" and returns a list of Rule objects. Exceptions are accounted for in the manual count but perhaps not in lizard since this would account for the discrepancy. The main source of complexity is a large switch statement for different tokens such as 'y' and 'h' which represent different date units. Different cases are explained through brief comments but could be further elaborated.

isCreatable:
Manual count (55) gives the same complexity as lizard (55) but different from jacoco (57). The function is very complex for the NLOC (95). Its purpose is to check if a string could be parsed as a number. It supports hex, octal, decimal, scientific notation, and numbers with descriptor (eg. '123l' for long). There are no exceptions. The documentation doesn't clearly explain exactly what numbers are accepted. One example is that it says that numbers beginning in '0.' are treated as decimal, but that's also true of '01.2'.

modify:
Both manual count and the lizard tool get cyclomatic complexity 32. The function is quite long at almost 200 lines. It is an internal calendar calculation function for truncate, round, or ceiling. Exceptions are not included in the manual count. Each branch is fully commented on.

translate: 
The manual counting resulted in a cyclomatic complexity of 23, with 23 decisions and 2 exit points (when counting an exception as an exit point). This is the same result as Lizard got. The function is not long compared to other functions in the project (49 NLOC), the high complexity comes from the large while-loop on row 119. The purpose of this function is to translate a set of code points into another set of code points using a CharSequence. The manual count includes exceptions. The documentation could be better, but this class is now Deprecated

createNumber: 
Lizard found the cyclomatic complexity to be 65 but manually, only 60 was found. The reason for this is that lizard excluded thrown exceptions as exit points. If the thrown exceptions were disregarded the complexity was the same. The CreateNumber function is both long with 144 loc and complex although very well documented with inline comments. The purpose of the function is to create an instance of the class Number given a string input.

## Refactoring

Plan for refactoring complex code and estimated impact (lower CC, but other drawbacks?):

parsePattern:
The main source of complexity is a large switch statement with 24 cases + default, many of which have additional if statements inside. The cases have common functionality in the sense that they create a new Rule object based on the character and token length.
One way to refactor would be to exploit this common structure and replace the switch statement with a HashMap mapping each character to a function which takes token length as parameter and returns an appropriate rule. This part could also be moved to a separate helper function.
This refactoring decreases cyclomatic complexity significantly (85%) but may add some overhead and reduce readability slightly.

isCreatable:
There is some unnecessary logic that increase the complexity. The worst offender being `if (i < chars.length){}` which in the context of this function can't be false. Resulting in unreachable code and increased complexity. Also plan on moving checking hex and octal numbers to their own functions.
The first change reduces CC, improves readability, and doesn't have any drawbacks. When refactoring the hex and octal numbers the CC of isCreatabe will be reduced but of course that complexity is really just moved elsewhere. There is a benefit to doing that though because it's easier to understand as a developer and easier to test separate parts.

modify:
Because the high complexity mainly comes from if-else within "switch" and "for", these parts can be extracted out to single helper functions. A drawback of this plan is that the switch part is used three times in different helper functions, which can be viewed as redundant.

translate: 
In order to refactor this function to lower the complexity, one would probably have to move the while-loop on row 119 to an outside helper function, since its condition contributes with 7 whole branches to the cyclomatic complexity. Furthermore, one could move the contents of the first if statement with a helper function that checks whether the input is valid. With this, you lower the complexity further by 3. 
All in all, implementing this would reduce the complexity from 24 to 14 (decreased by ca. 40%).

createNumber:
To refactor the method three parts can be broken out into smaller utility functions called: createFloatDoubleOrBigDecimal, createNumberFromRequest, createNumberFromHex. This will decrease the loc in createNumber from 144 to 61 making it more readable and easier to debug. This will also decrease the cyclomatic complexity from 65 to 22. The possible drawbacks from this refactoring is maybe that there are more functions to debug.

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

The tool covers basic branches such as if, for, and while statements. It also covers the multiple branches of statements with multiple conditions, such as if statements with an OR or AND. See next section for accuracy and limitations.

### Evaluation

1. How detailed is your coverage measurement?

Our coverage measurement outputs the IDs of the branches which are not covered by the class-specific unit tests as well as the percentage of branches covered.

2. What are the limitations of your own tool?

Our tool does not account for all ternary operators. It also requires manual implementation to extend to new functions.

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

Current state according to the Essence standard: In Place

Was the self-assessment unanimous? Any doubts about certain items?

The self-assessment was unanimous. We have established practices, support each other and use the same tools.

How have you improved so far?

We have improved since the beginning of the course by becoming more proficient in using Git as well as communicating among ourselves and structuring our work.

Where is potential for improvement?

We could improve to the next stage (Working Well) by becoming more proficient in the tools we are working with, since this time for example, we introduced two new tools (Lizard and Jacoco). We are also still learning when it comes to certain Git, Java, and Maven features.

## Overall experience

What are your main take-aways from this project? What did you learn?

Even projects that have been worked on by a lot of highly skilled and knowledgeable people can have flaws.
Managing large code projects is hard. It requires a lot of testing and attention to detail.

Is there something special you want to mention here?

Our project already had very high branch coverage, especially functions with high cyclomatic complexity. Also, many of the branches which were not covered were logically unreachable, making it difficult to identify new potential tests.