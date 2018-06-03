# Exploratory testing 

* **TODO** Tax code where the letters do not belong to the list??

## Output formatting 

1. The table is not perfect, the rigth vertical line is not a straight line.
2. Inconsistency in showing function names (some are all CAPS, other lower caps). 
3. "FIRST name" and "SURNAME" "details", "TAX CODE" all kinds of inconsistency 
4. Much more aesthetically pleasing to not have the user input cursor shift from left to write for every command. 
	That is, use tabs or some other mechanism to allow for enough info to be output by the program and the user input cursor not move horizontally. 

## Listing 

1. If first > last nothing happens. We should at least display some warning to the user. 
2. Start index of account range. How is the user meant to know what range is currently being in use? Seems like the range references the indices in the data structure in the code and not the IDs that the user can himself see of the customers. 
3. Not clear what each field in the output of listing references, would be better to have a header row that explains each comma separated field. It seems that the numeric bit of the tax code is being printed. Why is that? 


## Adding customer 

1. First and Last name can be just numbers 
2. First and Last name can be empty strings 
3. First and Last name can be weird characters: " , '
4. Do not understand "First, Second and Fourth group" extracted from the tax-code
5. After an exception is thrown (from null) input, it seems the records disappear. 
6. If invalid command, we get an exception but this exception does not cause removal of the records as observed in 
7. After a new customer is added, the menu doesn't show again. This is inconsistent with other behaviour (such as after listing, or after help). 
8. Nothing prevents us from creating two users with exactly the same name and details. The systems seems to give each of them a different ID, so that seems fine. 
9a. "fourth group" doesn't start with capital letter, whereas the others do. 
9b. Fourth group corresponds to the regex group which is not meaningful to users. 
10. After an invalid input (wrong format of the tax code) - all entries disappear.

## Deleting customer 

1. If input is does not match an ID present, then nothing gets deleted, but also nothing is output to tell the user that was an invalid ID. 
2. If an entry is deleted, nothing is shown to the user, so there is no differentiation between a successful deletion and a non-successful one. 
3. Try deleting a user that was already deleted: Success. No panicking by the app. 


## Updating customer 

1. When updating a customer entry via 'u' option, the printout is actually "Customer deleted" 
2. When updating customer, the tax code shows after the salary which is not consistent with the adding behaviour (confusing). 
3. If updating customer details fails, then the entry is deleted. Well. This actually goes back to the issue of when there is an exception we lose all entries. 


## Updating last name (-s) 

1. Initial test shows it to be working nicely 
2. If we try to change the last name and set it to the same previous last name, then it claims that the entry was modified. This is not critical but is also not ideal. There could be a better user experience. 

## Updating first name 

## Updating tax code 

1. Seems to work from the outside 


## Quitting program 

1. Quitting program actually quits the program: Success
2. try quitting as the first command: Success
3. try quitting after an exception: Success

Looks solid from the outside. As we don't actually expect any state to remain after doing this, and we can't worry about state being corrupt, there is little that can go wrong (or maybe the devs can surprise us?), we'll check this in unit testing. 



-
## Test Ideas

* Try creating more than INTEGER_MAX candidates and see if it overflows (or just try to stress test it). 

-
## Failing test cases

### Fail 1 
```
l
List Account Range

Enter start index of account range >0
Enter end index of account range >10
10: farhat, sami, tab123, 123, 123.0
Done ..


===========
| MENU    |
==================================================
| Input   |         Function                     |
--------------------------------------------------
|   h               HELP                         |
|   l               LIST customers               |
|   a               ADD new customer             |
|   d               DELETE customer              |
|   u               UPDATE customer's details    |
|   f               update customer's FIRST name |
|   s               update customer's SURNAME    |
|   t               update customer's TAX CODE   |
|   q               QUIT program                |
==================================================


u
Enter customer ID >10
Enter customer first name >joe
Enter customer last name >farhat
Enter customer tax code >123
Enter customer salary (pounds only) >tab123
null
```

### Fail 2 

I seem to be paying 100% of my base amount. Is this intentional? 

```
a
Enter customer first name >Sami
Enter customer last name >Farhat
Enter customer salary (pounds only) >45000
Enter customer tax code >S10000
Number of matched groups = 4
All text = S10000
First group = 
Second group = S
fourth group = 10000
New customer added ..


l
List Account Range

Enter start index of account range >1
Enter end index of account range >10
2: Farhat, Sami, ABXYZ82, 9404, 82.0
3: josephine\n, moe, TAB49, 293, 49.0
4: mathers, marshall\n, TAB49, 123, 49.0
5: Farhat, Sami, S10000, 45000, 10000.0
Done ..

```