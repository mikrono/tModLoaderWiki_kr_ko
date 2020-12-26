Match the existing formatting!
Don't reformat other people's code!
Particularly for vanilla classes


## Names
Use camelCase for:
* local variables
* method arguments
* instance fields
* non-public fields in static classes
		
Use PascalCase for everything else
	
Don't mix public properties and public fields. If you write a public property, convert all public fields to properties.
	
Use _ for 'backing fields' on properties.
	
Names of interfaces start with I
	
## braces
same line for statements, new line for declarations.

braces may be ommitted on single line `if/else` statements, but it's all or nothing. No if() ... else {...}

## line comments
Simple comments about a single line: on the line

Comments about multiple lines: before the line

Comments giving alternative options: end of line with a space

## In-file organisation/spacing:
Use spaces to split things _by function_.

Generally, put fields, then properties, then methods.	
	
## reducing patch size
	

## The var keyword
Use of var is encouraged if it aids readability by avoiding type names that are noisy, obvious, or unimportant.
### Encouraged:

* When the type is obvious - e.g. `var apple = new Apple();`, or `var request = Factory.Create<HttpRequest>();`
* For transient variables that are only passed directly to other methods - e.g. `var item = GetItem(); ProcessItem(item);`
### Discouraged:

* When working with basic types - e.g. `int i = 0`;
* When working with compiler-resolved built-in numeric types - e.g. `var number = 12 * ReturnsFloat();`
* When users would clearly benefit from knowing the type - e.g. `var listOfItems = GetList();`
