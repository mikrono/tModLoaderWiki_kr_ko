## Introduction
Before we begin, it's important that you understand what a config is and how a 'ModConfig' is handled. First of all, config stands for 'configuration' and with that we often mean a 'configuration file'. There are many formats for configuration files, in tModLoader we use a language called JSON for ModConfigs. JSON stands for JavaScript-Object-Notation and we will delve a bit deeper into the meaning and heritance of this name.

## What is JSON?
JSON is a language used to store data in a specific format and it is used for data interchanging. JSON is often preferred over other languages because it is **lightweight** and has a simple format. Although certain other languages like XML or YAML provide more features they are not as lightweight. JSON was derived from JavaScript, although it has adapted into many languages by now. In fact, all JSON really is, is JavaScript objects. JSON is the official internet media type (`application/json`) and files use the `.json` extension.

## How does JSON work?
JSON is comprised of data objects consisting of attribute-value pairs and array data types. If you know how a [Javascript object](https://www.w3schools.com/js/js_objects.asp) is built, JSON will be very simple. To move out of textual example and more into contextual example, here is a JSON sample showcasing showing it's basic data types:
```json
{
	"key:": "value",
	"number": 420.7335,
	"string": "this is a string!",
	"boolean": true,
	
	"array": [
		10.20,
		"an array can hold many data types",
		false, 
		[
			"even arrays inside arrays!",
			42
		]
	],
	
	"object": {
		"name": "another object",
		"description": "we can also make objects within objects, the sky is the limit",
		"remarks": "note the JSON notation: key: value",
		"anotherObject": {
			"array": [
				"arrays inside an object inside an object? this is madness!",
				null
			]
		}
	},
	
	"arrayWithObjects": [
		{
			"name": "01001000 01100101 01101100",
			"description": "01101100 01101111" 
		},
		{
			"name": "01110111 01101111 01110010",
			"description": "01101100 01100100 00100001"
		}
	]
}
```
As you can see, the sky is the limit (yeah.. there is a limit)
In JSON you'll be storing basic data types: strings, numbers, booleans, arrays, booleans and also null values. The above example showcases all basic data types and also in ways they can be combined. Next we will go over important remarks.

### Important remarks
Take a good look at the above example. The most important aspect of JSON is its key-value (or: name-value) pairs. It boils down to: a specific key matches a certain value. (notation: `"key": value`) Unlike a language such as C# (which uses a compiler), you do not specify the data type, it is parsed automatically. What is also important is that each element should be separated with a comma, apart from every last element. You are free to include a comma for last elements, but it is not required. To familiarize yourself with JSON it is recommended to fiddle a bit with it using [JSONLint](https://jsonlint.com/) which is a JSON validator. JSONLint will tell you if your JSON is valid, and if not it will tell you where it went wrong and why it went wrong. 

## How do modders utilize JSON for Terraria modding?
With tModLoader mods, modders can use JSON to store our own configuration files for their mods. These will commonly be stored in your `\Terraria\ModLoader` (Windows: `%userprofile%\Doucments\My Games\Terraria\ModLoader`) folder as a `<ModName>.json` file. Such a file often contains values which are meant to be modified by the player to (most often) alter certain behavior in that mod. For example, some configs will include a boolean value that will enable a debugging mode if set to true. This can be useful for troubleshooting specific mod bugs. The configs are often loaded upon mod loading, so a game restart or mod reload is usually required. Upon reading the configuration file the values inside the mod's code will adjust accordingly.