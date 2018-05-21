# Requirements Notes 

## Reading requirements

#### A1 

* By storage do we mean persistent storage, or just during application runtime? 


#### A2 Uniqueness of identifers

* Somewhat tricky to test. The implementation could have been written to create a random numeric identifier for every new client and that would highly reduce the likelihood of having clashes. With manual testing or just looking at it we may think it has been coded properly, but really ensuring this requires extensive testing. 

* Let's try to check that the client's ID dies with them when they die too. 

#### A3 Updating cusotmer details

* Check that we can change client details after they have been created. 
* What happens when we try to update the details of an entry that hasn't been created (not found)? 


#### A4 Print lists of customers

* Check that users can select a bunch of clients' info to be printed 
* Check that the printed info is correct, that is, one client's info does not mix with another one.

#### A5 

* Check that the application 'can' calculate the amount of tax to be paid
* Check that the application 'can' display the amount of tax to be paid 
* Check that the amount of tax to be paid is indeed dependent on their tax code and their salary: 
	* changing the tax code for the same salary gives different results. 
	* changing the salary for the same tax code gives different results.

##### A6

* Check that the system has a help command 
* Check that the help command will list all possible commands 

#### A7 

* Check robustness of the system against different user inputs - it **should not crash**.
	* upper/lower case
	* different whitespace values
	* weird characters ; - ',./`
	
#### A8 Tax rules

<!-- There's no option for someone who doesn't have children! ( I guess u don't use any of the letters for that.. ok I get it now).  --> 

The rules are vague on whether a negative number is allowed. I understand, there was no mention of '-' characters, which would make using negatives much harder, but it would be good to be more precise. 

##### A8.1 Tax to be paid dependent on tax code and salary

##### A8.2 _About the tax code format._

We can either have: 

* numeric part then alphabetic part 
* alphabetic part then numeric part

Test-wise: 

This sounds like something that would be great to test with regular expressions 

(_check slides for mentions of this_). 

* we cannot see alphabetic only 
* we cannot only see numeric only 
* if we've seen alphabetic then numeric, then we cannot see alphabetic again 
* if we've seen numeric then alphabetic, then we cannot see numeric again

##### A8.3

_Do not quite understand this yet_

##### A8.4 Letters in the tax code

* Only the outlined letters can be present in the alphabetic part of the tax code. 
* Check Q2


##### A8.5 Tax for Married people

* Check Q3. 

##### A8.6 Tax for Single people

* Not sure all salary brackets are covered by the given requirements. 
* The last two bullet points are contradictory

##### A8.7

* The last two bullet points are contradictory

##### A8.8 

* Nothing mentioned about those with salary of exactly £10,400
* We need to test that the final tax value is calculated based on:
	1. marital status first 
	2. single child or not

##### A8.9

* The exact £9900 is not decided. 

_Same as A8.8_
 

-

## Issues with requirements
* no mention of negative numbers being allowed. Are negative salaries a thing? Does the system guard against them? What about taxableAmounts, can we manage to squeeze them in to the taxCode string? 
* can the letters in the tax code be lowercase? 
<!-- * there is no mention of whether some of the codes in the tax code are mutually exclusive (e.g. D and S and M, or T and U, or C and E and F).  -->
<!-- * there is not mention of it not being acceptable for duplicate letters to be found in the taxcode.  -->
<!-- * We assume that earning less than zero (Paying?) person, pays no tax. i.e. min range = 0.  -->
<!-- * Non-functional requirements have not been detailed: do we expect the system to run on any platform? are there any speed / performance requirements that need to be catered for? For example, the requirements don't mention how many clients can be persisted on the app, which may imply (unlimited), but in such a case, it would be useful to reconsider the array design, as this does not provide enough flexibility going forward. --> 
<!-- * Are there no security requirements? Perhaps the ability to allow locking the app with an app is desirable? After all, the software does hold people's tax codes, rates... --> 
<!-- * There is no mention of non-functional requirements. How many clients does the system need to support? --> 
<!-- * They are wrong with regards to the tax brackets, we don't know what to do if someone earns exactly 27,000 etc.etc. -->
<!-- * There is no mention of the tax year in which the tax code applies. If an accountant runs the program after a tax year has ended, or they forget to clear the system from all entries, and use it in the new year, it will be highly misleading.  -->
<!-- * Can we be more specific with the word storage in the first requirement. A1. It doesn't specify if this is persistent storage or only for the lifetime of the application. The implementation has assumed this will only be accross the lifetime of the application.  -->
<!-- * Q2: 8.4 Can we have both 'M' and 'S' at the same time? Are they not mutually exclusive? Same for 'T' and 'U' ? The other ones with the children? -->
<!-- * Q3: 8.5 What about people with exactly £27,000? Given that this detail has been overlooked, can we please get some clarification on what exactly is the intended behaviour for people earning £12,000. Understand that from the given requirements, they belong to group of people earning less than £27,000. But can we please get confirmation on this?  -->
