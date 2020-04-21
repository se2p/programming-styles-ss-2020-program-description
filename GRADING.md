# Grading and Submission

In each assignment, you are asked to complete programming tasks and possibly answer questions. Programming tasks are evaluated in terms of their correctness, adherence to the various programming styles, general organization, code quality and clarity in the code comments.

Students have the responsibility to identify, reports and possibly fix problems and shortcomings in the assignments' material. Additional **bonus** points can be earned by the students that contribute to the class by implementing additional public tests, improving documentation, task descriptions, and, in general, sharing anything that can be useful to the other students dealing with the assignments. Sharing the solution of the assignments is, of course, not possible.

## Plagiarism
All the submissions undergo a plagiarism check that consists of manual code inspection and possibly the use of standard software for plagiarism check and code clone detection. Depending on the severity of the situation, plagiarism is punishable with failure, loss of all the points of the assignment or the task.

## Build scripts
To enable automatic testing, each of the submitted solutions must come with a make script (i.e., Makefile) that implements the following commands:

- `clean`: to clean up all the generated resources (e.g., compiled classes)
- `build`: to compile the code such that it can be executed by the system tests.

Usually a skeleton of the makefile is provided along with the assignment so students can code their solution around that. Otherwise, changes to the makefile must be committed along with the solution.

The automated testing infrastructure will use the make script to clean up, rebuild and prepare your submission for the execution in the system tests.

## Valid submissions
### Public tests
Be sure that your submissions passes **all** the public tests. Submissions that do not pass **all** the public tests on the automated testing infrastructure will not be considered correct, i.e., they will give you zero points. That is, even if your solution passes **all** the public tests on your local installation, it will still be considered wrong if it does not pass the tests on the automated testing infrastructure.

Public tests can be found at [https://github.com/se2p/programming-styles-ss-2020-public-tests](https://github.com/se2p/programming-styles-ss-2020-public-tests)

### Private tests
In addition to public tests, students submission are executed against a set of private systems tests, i.e., tests that student do not know of. Correct solutions should pass all the private tests; however, solutions that do not pass all the private tests will not be considered completely invalid, hence rejected. Instead, depending on how many private tests fail, student solutions will lose more or less points. 


### Automatic test execution
The automated testing infrastructure is available since the moment the assignments are given to students and will trigger regularly, but *not at every code commit*. So if students commit their code frequently, they will received a feedback from the automated testing infrastructure directly on their git repo. The feedback summarizes which test (both private and public) failed, and possibly the reason why it failed.

## Code comments
Student submission must respect **all** the constraints defined by the target programming styles; however, it might not be always possible to do so. To avoid any confusion and misunderstanding, the students must reasonably comment their code.
Code comments help in understanding the reasoning behind student programs and, more importantly, explain (potential) style violations. Note that the default behavior in the absence of suitable code comments is to consider any solution that violate the programming styles as wrong.