# Code inspection 

## Main

Some very very dodgy code  

## AccountNumbers

Notes: 

* Singleton pattern is useless and doesn't add anything in the system. 
* Class is meant to be a Singleton. 
* We start account numbers at 1.
* We can have `newAccountNum()` called without an instance. Then why do we need the AccountNumbers class in the first place?

## AllCustomers

Very badly named class. 

1. We create an array of customers with size = MAX_CUSTOMERS = 100. 
  * Do we check that number when adding? Is it possible to overflow this array?
  * some stuff is not specified as private, protected, public (bad practice). 

2. We never return the Customer object, which is fair maybe we don't want to expose it? But anyway, we should be returning something about its ID. --> we have added getIntCurrentCustomerIndex
  
### addCustomer()

Visibility: public
Input: firstName, lastName, taxCode, salary
Output: void 

Details: 
    * if we have not exceeded max number of customers, create a new customer passing in all the params.
    * no checks whatsoever are done on the input params
    * increment currentCustomerIndex
    * if we have exceeded print error. Return.
    
### getCustomer(int customerID)

Visibility: public 

Output: a Customer() instance. Always, even if not found. If not found, we return a default Customer() which has an INVALID_ACCOUNT=-1.
Details: 

* i=0 is redundant, we could just define it in the for loop 
* we're not looping correctly. We traverse in the loop n+1 elements (see `i<= intCurrentCustomerIndex`). 
This probably does not manifest now because we either find the guy we're looking for before reaching the end, or we don't find him 
and reach the last empty entry. 

DEFECT: we check the account number by using == which doesn't work for Strings. we should use equals() - testAddAndGetCustomer() finds that. 


### deleteCustomer(int customerID)

Visibility: public 

### updateCustomerAll

Quite surprising that this method does not call on the other update methods. This would make sure there is no code duplication, 
plus if we later find a bug somewhere, either in updateCustomerAll() or in updateCustomerFirstName() for example, then one change 
in code will fix both. 

**TODO to be continued** 

-- 


## Customer

* there are no getters for field names first name, last name, tax code. 
* not using camel case in the name of setter methods setlasname and setfirstname 
* field parameters have a different name from the setter method. This is justifiable sometimes, but this is just a cock up. 
* the overall desgin of the class is poor and poses some headaches for testers. Creating a customer with default constructor gives them an INVALID_ACCOUNT number. Is this in the requirements? Should we test for it? 


## TaxEngine


```
double dblTaxAmount =0;
		// NOTE: no need for this to be declared here, could be declared where used in the 
		// if block
		String strTaxAmount = new String();
		
		// NOTE: declaration of variable followed by usage. Inconsistent. Rest of code, 
		// declares and assigns. 
		Pattern rgxpTaxCode;
		
		// NOTE: regex pattern used is bonkers. 
		rgxpTaxCode = Pattern.compile("(\\d*)(([A-Z]|[a-z])*)(\\d*)");
		Matcher rgxmTaxCode = rgxpTaxCode.matcher(strTaxCode);
		
		if (rgxmTaxCode.find()) {
 			// NOTE: details here are irrelevant to user. 
			System.out.println("Number of matched groups = " + rgxmTaxCode.groupCount());
			System.out.println("All text = " + rgxmTaxCode.group(0));
			System.out.println("First group = " + rgxmTaxCode.group(1));
			System.out.println("Second group = " + rgxmTaxCode.group(2));
			// NOTE: why oh why, start with non-capital. Are you that lazy? 
			System.out.println("fourth group = " + rgxmTaxCode.group(4));

			// NOTE: what this does is take the number before the alphanumeric (if exists)
			//  and concatenate it with the number (if exists) after the alphabets. 
			// This breaks the requirements. 
			strTaxAmount =1 rgxmTaxCode.group(1).toString()+ rgxmTaxCode.group(4).toString();
			
			// NOTE: maybe the first statement i've seen that isn't totally bonkers - horaay!
			dblTaxAmount = Double.parseDouble(strTaxAmount);
			
			
			// NOTE: FOR ALL top level if, else statements below: 
			// 			1- should be whitespace between bracket and brace
			// 			2- indexOf > 0, should actually be indexOf >= 0 
			// 1- Code formatting 
			// 2- greater than zero? Think they meant greater than -1, or greater or equal than zero. 
			// zero is a valid index for finding in the string. 
			// If the M character was first in the tax code, then this would fail to detect it. 
			
			// NOTE: laziness: the zero is collated with the greater than sign. 

			// MAKE_TEST: taxCode='Mxxxx'. This will fail to detect marriage. 
			// REQUIREMENTS_VAGUE: Don't specify what should be done for someone who earns exactly 27,000.
			
			// NOTE: BUG BUG BUG: HI_DIVORCED_MODIFIER used instead of HIGH_DIVORCED_MODIFIER. 
			// Same for HI_MARRIED_MODIFIER. 
			// NOTE: see this is what happens when variables are very badly named!!!
			// at the very least could have moved them to another Java file, called Legacy, or Complex or something. 
			
			// The code assumes that 27,000 is in the high band.
			if (strTaxCode.indexOf(Constants.MARRIED_CODE) >0 ) {
				
				// NOTE: I'm so happy we finally used if, else if. Something right!
				if (intSalary < Constants.LOW_MARRIED_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.LOW_MARRIED_MODIFIER;
				}
				else if (intSalary < Constants.MED_MARRIED_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.MED_MARRIED_MODIFIER;
				}
				else if (intSalary < Constants.HIGH_MARRIED_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.HI_MARRIED_MODIFIER;
				}
				else {
					dblTaxAmount = dblTaxAmount * Constants.TOP_MARRIED_MODIFIER;
				}
			} 
			// NOTE: we want an else if here instead of if (explain how this is bad). 
			// NOTE: Code assumes that the requirements were sane and wanted 132% 
			// for those earning above 22,000 but we have no confirmation of this. 
			if (strTaxCode.indexOf(Constants.SINGLE_CODE) > 0 ){
			
				if (intSalary < Constants.LOW_SINGLE_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.LOW_SINGLE_MODIFIER;
				}
				else if (intSalary < Constants.MED_SINGLE_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.MED_SINGLE_MODIFIER;
				}
				else if (intSalary < Constants.HIGH_SINGLE_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.HI_SINGLE_MODIFIER;
				}
				else {
					dblTaxAmount = dblTaxAmount * Constants.TOP_SINGLE_MODIFIER;
				}
			}
			// NOTE: we want an else if here instead of if (explain how this is bad). 
			// NOTE: Code assumes that the requirements were sane and wanted 110% 
			if (strTaxCode.indexOf(Constants.DIVORCED_CODE) > 0 ){
				
				if (intSalary < Constants.LOW_DIVORCED_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.LOW_DIVORCED_MODIFIER;
				}
				else if (intSalary < Constants.MED_DIVORCED_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.MED_DIVORCED_MODIFIER;
				}
				else if (intSalary < Constants.HIGH_DIVORCED_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.HI_DIVORCED_MODIFIER;
				}
				else {
					dblTaxAmount = dblTaxAmount * Constants.TOP_DIVORCED_MODIFIER;
				}
			}
			
			// NOTE: code_formatting. bracket should be after parenthesis. 
			// 
			if (strTaxCode.indexOf(Constants.CHILD1_CODE) > 0 ){
				
				if (intSalary < Constants.LOW_CHILD1_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.LOW_CHILD1_MODIFIER;
				}
				else if (intSalary < Constants.HIGH_CHILD1_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.HIGH_CHILD1_MODIFIER;
				}
				else {
					dblTaxAmount = dblTaxAmount * Constants.TOP_CHILD1_MODIFIER;
				}
			}
		
			// NOTE: TOP_CHILD2_MODIFIER is set to 10.1, it should be 1.01
			if (strTaxCode.indexOf(Constants.CHILD2_CODE) > 0 ){
				
				if (intSalary < Constants.LOW_CHILD2_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.LOW_CHILD2_MODIFIER;
				}
				else if (intSalary < Constants.HIGH_CHILD2_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.HIGH_CHILD2_MODIFIER;
				}
				else {
					dblTaxAmount = dblTaxAmount * Constants.TOP_CHILD2_MODIFIER; 
				}
				
			}
	
			if (strTaxCode.indexOf(Constants.CHILDm_CODE) > 0 ){
				
				if (intSalary < Constants.LOW_CHILDm_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.LOW_CHILDm_MODIFIER;
				}
				else if (intSalary < Constants.HIGH_CHILDm_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.HIGH_CHILDm_MODIFIER;
				}
				else {
					dblTaxAmount = dblTaxAmount * Constants.TOP_CHILDm_MODIFIER;
				}
			}

			if (strTaxCode.indexOf(Constants.STUDENT1_CODE) > 0 ){
				
				if (intSalary < Constants.LOW_STUDENT_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.LOW_STUDENT1_MODIFIER;
				}
				else if (intSalary < Constants.HIGH_STUDENT_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.HIGH_CHILDm_MODIFIER;
				}
				else {
					dblTaxAmount = dblTaxAmount * Constants.TOP_STUDENT1_MODIFIER;
				}
			}

			if (strTaxCode.indexOf(Constants.STUDENT2_CODE) > 0 ){
				
				if (intSalary < Constants.LOW_STUDENT_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.LOW_STUDENT2_MODIFIER;
				}
				else if (intSalary < Constants.HIGH_STUDENT_BAND) {
					dblTaxAmount = dblTaxAmount * Constants.HI_STUDENT2_MODIFIER;
				}
				else {
					dblTaxAmount = dblTaxAmount * Constants.TOP_STUDENT2_MODIFIER;
				}
			}

		} else { 
			//  NOTE: returning 0 is not necessarily an indicative tax return
			// TEST:  find some test, I can't think clearly now...
			System.out.println("Badly formatted tax code");
			dblTaxAmount = 0;
		}	

		return (dblTaxAmount);
	}
```

Should be static.

Code: 

They're using regex. The regex already looks wrong: 
    * FYI: they can use non-capturing groups instead of ignoring the third group

They're doing the tax amount as the combination of first and fourth group, somewhat assuming that the user will obey their requests
and put the numeric part of the tax code either before or after the non-numeric part. 


123SAMI123 matches 

Bugs: 

* regex is wrong
	* it is not enforced that there be any numbers. They use the 0 or more sign "\*" instead of "+"
    * all alphanumerics are allowed in the first place.
* indexOf, strictly greater than zero (>0)
* since supposedly one cannot be both divorced and single and married, we should have "else if" instead of "if" in the code checker. 
* that would ensure only one of those branches gets executed. Although, in reality the whole format of the function is not ideal.
* failure to parse the tax code should result in something more alarming than a printout and returning a zero. Zero is a valid tax output. 
 
## Constants 

```
package tax;

public class Constants {

	public static final int MAX_CUSTOMERS = 100;
	public static final int INVALID_ACCOUNT = -1;
	
	// Tax codes
	public static final char MARRIED_CODE = 'm';
	public static final char SINGLE_CODE = 's';
	public static final char DIVORCED_CODE = 'd';
	public static final char CHILD1_CODE = 'c';
	public static final char CHILD2_CODE = 'e';
	public static final char CHILDm_CODE = 'f';
	public static final char STUDENT1_CODE = 't';
	public static final char STUDENT2_CODE = 'u';

	
	// Tax Bands
	public static final int LOW_MARRIED_BAND = 5000;
	public static final int MED_MARRIED_BAND = 12000;
	public static final int HIGH_MARRIED_BAND = 27000; 

	public static final int LOW_SINGLE_BAND = 6500;
	public static final int MED_SINGLE_BAND = 11000;
	public static final int HIGH_SINGLE_BAND = 22000; 
	
	public static final int LOW_DIVORCED_BAND = 7200;
	public static final int MED_DIVORCED_BAND = 13000;
	public static final int HIGH_DIVORCED_BAND = 24000; 
	
	public static final int LOW_CHILD1_BAND = 8000;
	public static final int HIGH_CHILD1_BAND = 10400;
	
	public static final int LOW_CHILD2_BAND = 7400;
	public static final int HIGH_CHILD2_BAND = 9900;

	public static final int LOW_CHILDm_BAND = 7000;
	public static final int HIGH_CHILDm_BAND = 9000;

	public static final int LOW_STUDENT_BAND = 2000;
	public static final int HIGH_STUDENT_BAND = 3000;
	
	
	// Modifiers
	// Modifiers - marriage
	public static final double LOW_MARRIED_MODIFIER = 0.7;
	public static final double MED_MARRIED_MODIFIER = 0.9;
	public static final double HIGH_MARRIED_MODIFIER = 1.2;
	public static final double TOP_MARRIED_MODIFIER = 1.3;
		

	public static final double LOW_SINGLE_MODIFIER = 0.75;
	public static final double MED_SINGLE_MODIFIER = 0.95;
	public static final double HI_SINGLE_MODIFIER = 1.25;
	public static final double TOP_SINGLE_MODIFIER = 1.32;

	public static final double LOW_DIVORCED_MODIFIER = 0.6;
	public static final double MED_DIVORCED_MODIFIER = 0.8;
	public static final double HIGH_DIVORCED_MODIFIER = 0.95;
	public static final double TOP_DIVORCED_MODIFIER = 1.1;
	
	public static final double LOW_CHILD1_MODIFIER = 0.8;
	public static final double LOW_CHILD2_MODIFIER = 0.9;
	public static final double LOW_CHILDm_MODIFIER = 1.2;
	
	public static final double HIGH_CHILD1_MODIFIER = 0.85;
	public static final double HIGH_CHILD2_MODIFIER = 0.95;
	public static final double HIGH_CHILDm_MODIFIER = 1.25;
	
	
	// NOTE: dude this is so wrong! 10.1--> 1.01
	public static final double TOP_CHILD1_MODIFIER = 0.95;
	public static final double TOP_CHILD2_MODIFIER = 10.1;
	public static final double TOP_CHILDm_MODIFIER = 1.3;
	
	
	public static final double HI_STUDENT1_MODIFIER = 1.07;
	public static final double HI_STUDENT2_MODIFIER = 1.09;
	
	public static final double LOW_STUDENT1_MODIFIER = 1.13;
	public static final double LOW_STUDENT2_MODIFIER = 1.15;
	
	public static final double TOP_STUDENT1_MODIFIER = 1.23;
	public static final double TOP_STUDENT2_MODIFIER = 1.25;
	

	
	// Modifiers - other complex / Legacy
	public static final double HI_MARRIED_MODIFIER = 1.5;
	public static final double HIGH_SINGLE_MODIFIER = 1.4;
	public static final double HI_DIVORCED_MODIFIER = 1.18;
	public static final double HIGH_STUDENT1_MODIFIER = 1.07;
	public static final double HIGH_STUDENT2_MODIFIER = 1.90;
	public static final double TOP_STUDENTm_MODIFIER = 1.95;
}


```

## Bugs to test for 

* string handling can be improved by using stringbuffer (improvement)
* Code enforces a 100 client limit. The requirements make no mention of this. 