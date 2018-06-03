# Exploratory testing 
-
## Failing test cases

### Fail 1 

REASON: salary had letters in it. that's why. 

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

REASON: it is not detecting the Single because of the first letter missed bug.

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