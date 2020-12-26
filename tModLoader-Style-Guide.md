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
	