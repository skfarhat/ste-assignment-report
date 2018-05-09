# Report 

## Pre-Testing activities 

### a) Review System Requirements 

_See requirements-notes.md_ 

### b) Code Review 

_Assess code structure, comment on its testability and make recommendations to improve testability_

* _See code-inspection.md_ 
* Main has too much logic, and lots of methods in there are `private static` the private part is especially bad for testing. 
* Allowing the inputs and outputs of the program to be configurable would make it much more testable. We can still use `System.setOut` and `System.setIn` which is still convenient. 
* When 'q' is typed, replace the exit() call with `running = false` to stop the loop and get us to exit


resources/

* A1.txt: add user 
* A2.txt: add user, then print ranges 0,1
* A3.txt: add user, delete user
* A100.txt: add 200 users and print 


## Testing Steps 

1. Requirements Review 
2. Exploratory Testing (taking notes) 
3. Code Review (taking notes) 
4. Setup gradle, Jenkins
4. Static analysis, Syntax check: Checkstyle, FindBugs with build steps in Jenkins 
5. Refactor for testability (+ check that MD5 hash does not change) 
6. Unit Testing 
7. Acceptance Testing 
8. Exploratory Testing (we may find bugs that the MD5 hash test failed to find) 
8. Code Coverage 


## Defects detected 

Testing for 'M1000': 

1. requirements say 'M' but the implementation says 'm'. If they insist on keeping the 'm' use toLowerCase(). Also the code is self-contradictory. This is not an issue of requirements were not understood, because the regex expression tries to match [A-Z] whereas the finding of chars is on lowercase ones. 


Testing taxAmount(): 

* If input string "strTaxCode" does not match the pattern required, 0 is returned. It has been "observed" (we cannot be sure), that the intention is for this to be identified as an error (something is printed on the console). However, this method of error reporting is very weak. 




