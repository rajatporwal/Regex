# Regular Expression


- [Regex Methods](#regex-methods)
- [Regular Expressions Flags](#regular-expressions-flags)
- [String Methods](#string-methods)
  * [exce() vs match()](#exec-vs-match)
- [Meta Characters](#meta-character)
  * [Control Characters](#control-characters)
  * [Indicating Character Repetition](#indicating-character-repetition)
  * [Greediness and Laziness](#greediness-and-laziness)
  * [Character Set Shorthand](#character-set-shorthand)
  * [Character Set Shorthand For Negation](#character-set-shorthand-for-negation)
  * [Anchored Expressions](#anchored-expressions)



## Two ways of writing Regex

> let regex = new RegExp("Hello")
> let  regex  = /hello/
<!-- toc -->

## Regex Methods

 - **test :**  returns true if pattern is found in the passed string; false if not
 
<details>
  <summary>Example</summary>
  
  ```javascript
	let txt = "Programming courses always starts with a hello world example"
    let  regex1  = /hello/
	let  regex2  = /worlds/
	
	regex1.test(txt) 		// output: true
	regex2.test(txt) 		// output: false
  ```
</details>

 - **exec :** returns an object for single match and array for multiple matches
 
<details>
  <summary>Example</summary>
  
  ```javascript
	let txt = "Programming courses always starts with a hello world example hello"
    let regex1  = /hello/
    let regex2  = /hello/g
    let regex3  = /worlds/g
    	
	- regex1.exec(txt)

	/*
	output: 
		{
			0: "hello",
			groups: undefined,
			index: 41,
			input: "Programming courses always starts with a hello world example",
		}
	*/
	
		- regex2.exec(txt)			// output: ["hello, "hello"]
		- regex3.exec(txt)			// output: null

  ```
</details>

 - **toString :** returns a string of the regular expression syntax

## Regular Expressions Flags

> /pattern/flags
> new RegExp("pattern", "flags")
 - **g :** global, it matches more than one occurrence
 - **i :** case insensitive match
 - **m :** multi line match

## String Methods

 - **match :** returns an array of matches just like exec on RegExp
 - **search :** returns an indexed of the matched String
 <details>
  <summary>Example</summary>
  
  ```javascript
	let txt = "Programming courses always starts with a hello world example hello"
    let regex1  = /hello/g
    let regex2  = /hellos/g
    
	- txt.search(regex1) 		// output: 41, it returns only first matched index

	- txt.search(regex2) 		// output: -1
	
  ```
</details>

 - **replace :** replaces matches with a string
 <details>
  <summary>Example</summary>
  
  ```javascript
	let txt = "Programming courses always starts with a hello world example hello"
    let regex1  = /hello/
    let regex2  = /hello/g
    
	- txt.replace(regex1, "hi")  // output: Programming courses always starts with a hi world example hello

	- txt.replace(regex2, "hi")  // output: Programming courses always starts with a hi world example hi

> Note: replace returns new string.
	
  ```
</details>

 - **split :** splits a string into an array. The division is based on the regular expression pattern

 <details>
  <summary>Example</summary>
  
  ```javascript
	let txt = "Programming courses alwayS starts with a hello world example"
    let  space  = /\s/
    
	txt.split(space);  // output: ["Programming", "courses", "always", "starts", "with", "a", "hello", "world", "example", "hello"]
	
  ```
</details>

### exec() vs match()

- match() will return all the matches
- exec will work as a loop, the more time you call it will return next matched element

 <details>
  <summary>Example</summary>
  
  ```javascript
let regex1  = /s\s/;
let regex2  = /s\s/g; 		// g flag will match all the s
let regex3  = /s\s/gi; 		// i will match all the s (ignores case)

	- txt.match(regex1)		// output: ['s']
	- txt.match(regex2)		// output: ['s', 's']
	- txt.match(regex3)		// output: ['s', 'S', s']

	- txt.exec(regex1)		// output: ["s ", index: 18]
	- txt.exec(regex2)		// output: ["S ", index: 25]
	- txt.exec(regex3)		// output: ["s ", index: 32]
	- txt.exec(regex3)		// output: null, as there is no more match having space after s

  ```
</details>

## Meta Characters

|                |Description    |Regex                          |Example Matches                         |
|----------------|----------------------|--------------------|------------------------------------|
| **^** | works as negation when used inside []			   | `/A[^a-d]T/`       	| it will not match words having a-d after A
| **$** | ... 				   | ... 			|
| **.** | wild card            |`/h.t/g`        | hat, het, h t, h@t etc.   
| **=** |...                   |....            |
| **#** |...                   |....            |
| **!** |...                   |....            |
| **:** |...                   |....            |
| **\|**|...                   |....            |
| **\\**| returns literal value| `/\./`            | match all the . in the string
| **/** |...                   |....            |
| **()** |...                  |....            |
| **[]** |Specify group of characters in [] brackets |`/gr[ae]y/`      | grey or gray
| **{}** |...                  |....            |


### Control Characters

|    Symbol               |Description 
|----------------|----------------------|
| **\\t**  | tab
| **\\v**   | vertical tab
| **\\n**     | new line
| **\\r**     | carriage return


### Indicating Character Repetition


|                |Description    |Regex                          |Example Matches                         |
|----------------|----------------------|--------------------|--------------------|
| **\*** | zero or more         |`/hats?/`       |	hat, hats, hatss, hatsss
| **+** | one or more 		   | `/hats+/`      | hats, hatss, hatsss 
| **?** |zero or one           |`/hats?/`       | hat, hats
| **{min}** |exact equal to minimum          |`/\d{3}/`       | 467, 568, 907
| **{min,}** |min to infinite           |`/\d{3,}/`       | 574, 83434790, 341343
| **{min,max}** |min to max times           |`/\d{2,4}/`       | 574, 8790, 341

###  Greediness and Laziness

By Default Regular expression are greedy.

- **\* :** will grab as much as possible
- **? :** will grab as little as possible, it makes the regex lazy

<details>
<summary>Example</summary>

 ```javascript

	let txt = "\<p>Para 1</p><p>Para 2</p>"
	let regex1  = /<p>.*<\/p>/; 		
	let regex2  = /<p>.*?<\/p>/; 		
	
	- txt.match(regex1)		// output : <p>Para 1</p><p>Para 2</p>
	- txt.match(regex2) 		// output : <p>Para 1</p>
	
  ```
  
</details>

## Character Set Shorthand

|    Symbol            |Regex    |Description 
|----------------|----------------------|--------------------|
| **\\d** | `[0-9]`       | Allow only digits
| **\\w** | `[a-zA-Z0-9_]`	   | Allows alpha numeric characters
| **\\s** | `[ \t\r\n]`      | Allow space of any type

## Character Set Shorthand For Negation

|    Symbol            |Regex    |Description 
|----------------|----------------------|--------------------|
| **\\D** | `[^0-9]`       | Do not allow only digits
| **\\W** | `[^a-zA-Z0-9_]`	   | Do not allow alpha numeric characters
| **\\S** | `[^ \t\r\n]`      | Do not allow space of any type

## Anchored Expressions

- **^ :**   match to the start of the line
- **$ :**  match to the end of line

<details>
<summary>Example</summary>

 ```javascript

	let txt1 = "The dot is the very powerful name."
	let txt2 = "Dot is The very powerful name"
	
	let regex1  = /^The/gi; 	// Matches only string starting with the
	
	- txt1.match(regex1)		// output : ["The"]
	- txt2.match(regex1) 	// output : null
	
	let regex2  = /name.$/gi;	// Matches only string ending with name.	
	
	- txt1.match(regex2)		// output : ["name."]
	- txt2.match(regex2) 	// output : null

	let regex3 = /^The.*name.$/;	// Matches only string starting with The and ending with name. 

	- txt1.match(regex3)		// output: ["The dot is the very powerful name."]
	- txt2.match(regex3)		// output: null
	
  ```
  
</details>


## Meta characters you may need to escape

> <pre><b> -  	^ 	 	\ 		] </b></pre>
<!-- toc -->



