# Report 

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


## Report structure

### Intro 

* Any changes to requirements may invalidate currently written tests. 
* Given requirements have been tagged as version 1. The provided tests target version 1 of the requirements only. 
* We will read the code and attempt to identify what can go wrong and what has not been accounted for in the implementation, then write a test to detet that.

We would like to get a wide range of tests and detect as many and different defects as possible. 

#### Exploratory
1. We will start off with Black Box testing
    * Take notes of what is observed, and idea on how to test it
2. View the code
    * Take notes on weaknesses detected and idea of how we can test it. 


#### Test Plan (not required to do this) 
**TODO** Check Fundamental Test Process

1. Initial review
    1.  Requirements review.  
        * Spot vaguenesses in the requirements. 
        * Check Thursday-05 document on "What does a good requirement look like?" 
    2. Code structure review. 
        * Assess code structure and changes required (to make it more testable)
3. Test analysis and design
4. Test planning. 
    * What is our test infrastructure?
        - Tools to use
    * Acceptance Testing
        - Use cucumber. 
        - How do we validate correctness and success of the testing? 
        - What code coverage are we aiming for? 
5. Finishing up 
    1. Exit and Reporting 
    2. Test Closure
        * All deliverables were made 
        * Lessons learnt 
5. Test closure activities 
    * Check test logs against the exit criteria
    * Asses if more tests are needed 
    * Write a test summary report 


#### Test Traceability 
**TODO** Check last slide in 7th document for Monday 

Need to trace each requirement to a test that validates that requirement. We can write a table that outlines that. 

We can have: 

* Traceability to requirements 
* Traceability to process 
* Traceability to code (unit tests, this test checks that bit of code, or that function..) 
* Traceability to use cases 


## TODO list

**Requirements** 

* Check specification based

**Slides**

* Skim through the slides - and give an organisational idea as to what each couple of slides address. 
* Check the slides for mentions of regular expression. 
