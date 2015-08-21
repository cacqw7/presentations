## Presentation Ideas
* Make people write terrible examples of code
	* Refactor, reviewing solid Ruby design principles



## Reading Notes


#### Style

* Good code doesn't need an explanation
* CamelCase classes and snake_case everything else
* CONSTANTS_OR ConstantsOr
* Blocks
	* Single Statement `10.times { |n| puts "The number is #{n}"}`
	* Multiple
		```
		10.times do |n|
			puts n
			puts n*2
		end
		```

* _for_ vs _each_
	* _each_ loop introduces a new scope, variables in the loop are not accessible outside

* Boolean Logic
	* Only `false` or `nil` evaluate to _false_
	* Everything else (including `0` and `"false"`) evaluate to true
	* Page 24 explains why not to test if something is true
	* If you're looking for nil and there is any possibility of `false` turning up, then look for `nil` explicitly
* `@first_name ||= '' ` is the same as `@first_name = @first_name || ' ' ` _pg 26_

## Arrays and Hashes
* Mapping returns an array with the calculated values

		arr = [1,2,3]
		sum = arr.map do | num |
			num + 1
		end

	returns `sum [2,3,4]`

## [Strings](http://ruby-doc.org/core-2.2.2/String.html)
* `%q( Enter your string here )`
* `%Q( This is treated like double quotes)`
* Trailing `\` escapes new line characters on multi line strings
* `str.strip` takes off whitespace
	* `str.lstrip`
	* `str.rstrip`
* `sub` and `gsub` pg 48
* Blocks
	* `.each_char`
	* `.each_line`
* `first_name[first_name.length - 1]` == `first_name[-1]` or ranges `first_name[1..2]`

#### Many of these methods have ones with `!` appended, and those change the actual string vs giving you back a copy

## REGEX
* pg 54
* Escape special characters with `\` _ex:_ `3\.14` == `3.14`
* **Sets** match any one of a bunch of characters
	* [aeiou] will match any *single* character that has one of those letters
	* Range
		* [0123456789abcdef] == [0-9a-f]
		* `\d` any digit
		* `\w` any word character (letter, number, underscore
		* `\s` any whitespace
	* `|` match anything before or after `AM|PM`
	* `*` matches zero or more of what came before it
		* `AB*` matches:
			* `AB`
			* `ABBBBBBBBB`
			* `A`
	* `.*` matches **anything**
	* `i` at the end turns off case sensitivity `/AM/i`
	* `\A` beginning of string, `\z` end
	* `\^` beginning of string or line, `\$` end of string or line
	* `^Subdomain:\s([^,]+),\sPromotion\sDate:\s(\d{1,2}\/\d{1,2})`
		* `^Subdomain:\s` - matches string and space (\s)
		* `([^,]+)` matches everything up to the `,`, `+` means multiple characters and `()` makes it the $1 variable
		* Promotion\sDate:\s - matches string plus

## Symbols
* Symbols are like strings
	* Some strings represent data (book titles, authors, text)
	* Others are just used to represent other parts of code
		* you don't need all the string methods (first 10 characters, up-case etc) if you are just using them to represent other parts of 		your code
	* `a = :all` , `b = a` and `c = :all` all point to the same `:all`
	* every time you say "all", you make a brand new string
	* Symbols are great for hash keys because the comparisons happen at lightening speeds, and symbols are immutable (never change)

## Everything's an object

Class is a container for methods
Everything in ruby is an object include -3, "hello" and false

Everything is dependent of the `Object` class

`Object.public_methods` shows all available methods

## Dynamic Typing

#### Advantages
* Compact code
	* Duck typing (if it walks like a duck, quacks like a duck, it must be a duck)
	* Get rid of base classes if they aren't providing any value
* Flexibility
	* Any two classes that CAN work together WILL work together
* Dynamic typing lacks the "Documentation" of strong typed languages
	* Use nice full words for class, method and variable names ex:

			def is_longer_than?( number_of_characters )
				@content.length > number_of_characters
			end
* Clear and concise code == Fewer mistakes
	* There is a difference between *concise* and *cryptic*
* **Ruby is a language for grown-ups; it gives you the tools for writing clear and concise code. It's up to you to use them**


## TESTS!!!

Mocks vs Stubs
	* A mock is a stub with an attitude. Along with knowing what canned responses to return, a mock also knows which methods should be called and with what arguments (pg 107)

# Classes, Modules, and Blocks

## Constructing Classes

**Breaking up your code into many short, single purpose methods not only plays to the strengths of Ruby but also makes your application more testable**

* Composed Method Technique
	* Advocates dividing your class up into methods that have three characteristics
		* Each method should do a single thing
			* Easier to write and *understand*
		* Each method needs to operate at a single conceptual level
			* Don't mix high level logic with nitty gritty details
		* Each method needs to have a name that reflects it's purpose.
**The computer doesn't care about the names of your methods etc, follow this principal for humans, they help *you* get the details right**

* Cost of defining a new method is very low. (just add a `def` and an `end`)
* Methods should be **short** and **coherent**
* Have to tote the line between adding value vs adding **clutter**
* `ActiveRecord::Base` is existence proof for the postulate that you can get something done with short, focused methods

## Defining Operators

Operators are methods
` sum = first + second` is really `sum = first.+(second)`

`&&, `and`, `||` , `or` are all built into ruby and can't be changed

If you change operator methods, make sure that each class is aware of the other classes and knows what to do.

Remember the "cloud" of ideas that operators conjure up for anyone using your operators.

`c = a + b` no one would expect the value of b to change in this expression.

#### Equals

`equal?` is for Object Equality, references to exactly the same object.
Ruby's built in `equal?` is so good, there's no reason to override it, ever. pg 143

`==`

`===` is for case statements with regexp

`eql?`

*Hash Buckets*
Create different _buckets_ to store your hash values. Give each object a hash key, then to decide which bucket it goes in do
`has_key % number_of_buckets`

#### Singleton and class methods

*Singleton Method* - a method that is defined for exactly one object instance.

Instead of `def method_name` you do `def instance.method_name` ex:

		hand_built_stub_printer = Object.new

		def hand_built_stub_printer.available?
			true
		end


*Singleton methods are just like regular methods, except they are stuck to a single instance of an object*
They override any regular class-defined methods, except Numeric classes and symbols
Short hand way to write them is to wrap all your method definitions in

		class << hand_built_stub_printer

		end

The singleton class sits between the object and it's regular class (pg 161)
Since the singleton class is before the actual class, it has first say in where the method is getting called.

Singleton class AKA (meta classes or eigenclasses)

Class methods are singleton methods. Because all methods inherit from `Class`, when we make a method on something like `Document` we only want those methods to be on an instance of `Document` not on all `Class` objects

#### Use class instance variables
Two ways to store *class level* data, class variable and the class instance variable

##### Class Variables
* start with `@@`
* associated with a class instead of an ordinary instance.
When Ruby sees a class variable, it has to check the class to see if it has `@@class_variable`, if it doesn't it works its way up the inheritance tree looking for the class variable. If it finds one, it sets that variable, if it doesn't it sets it on the current class

They aren't so much variables attached to a specific class, but a global variable with a slightly restricted realm.

##### Class Instance Variables
* Start with `@`
* Only for the instance of the object
* Can put in an initialize method

No need to write getter and setter methods, just use `attr_accessor`

#### use modules as name spaces
Wrap your classes in
`module ModuleName`

Then call them by `ModuleName::ClassName`
Or include the module `include ModuleName`

Helps organize code logically, and reduce name collision.

Modules can be nested

Modules are great for helper methods or other methods we don't know what to do with.

Can access them like classes.

	module Wordprocessor
		def self.points_to_inches(points)
	end

	Wordprocessor.points_to_inches(points)
Nice example for using modules pg 187

When to create Classes and when a module

If you find yourself creating a lot of names that alls tart with the same word, you may want a module.

### Use Modules as Mixins
You can insert or "mix in" modules into the inheritance tree of your classes
Mixins can be put into multiple, otherwise unrelated, classes

first you put your method into a module

	module WritingQuality
		def method
		end
	end
Then in any class that needs it, you include it.

	class Document
		include WritingQuality
	end

If you want them to be _class_ methods

	class << self
		include Finders
	end
or

	extend Finders

Modules are inserted essentially, as super classes of the current class
So you can override module methods by declaring them in your class file.

### Blocks to Iterate

fire off a block if it's given in your method

	yield if block_given?
an iterator method calls its block once for each element in some collection, passing the element into the block as a parameter
*Enumerable* Your iterator on Steroids


##### Execute around with a block

logging is the best debugging tool

pg 220 and 221

	with_logging('load') { @doc = Document.load( 'resume.txt' )

	def with_logging(description)
	  begin
	    @logger.debug("Starting #{description}" )
	    return_value = yield
	    @logger.debug( "Completed #{description}" )
	    return_value
	  rescue
	    @logger.error( "#{description} failed!!" }
	    raise
	  end
	end

Block scope

*All of the variables that are visible just before the opening `do` or `{` are still visible inside the code block. Code blocks drag along the scope in which they were created

#### Save blocks to execute later

Explicit blocks

Pass an argument with an `&` in front of it to your method and it'll be taken as a block

	def run_that_block( &that_block )
		puts "About to run the block"
		that_block.call
		puts "Done running the block"
	end
Check if it's a block `that_block.call if that_block`j

If you use explicit block variables, you can hold onto the block and store a reference to it like any other object

Banking blocks
Saving blocks for callbacks

Lambda - create a default `Proc` object
Pass it a block and the method will pass the corresponding `Proc` object right back at you

Keep in mind the things you might be unconsciously dragging along with your blocks
If a block creates a variable and is in scope in the block, it'll drag that around

#### Method Missing for flexible error handling

If there's no method, it calls `method_missing` on the object class.

Constants also go missing
`const_missing` to deal with AWOL constants
needs to be a class method, `self.const_missing`

`whiny nil`

be careful with method calls inside of `method_missing`, if the method doesn't exist, ruby will start all over again looking for the missing method

#### Method missing for delegation

Delegation
	* You have an object that needs to do something
	* you know you have another object that does exactly the same thing
	* Sending work to another object
Problem with traditional delegation is just like in real life. The Wrapper object just follows around the delegated object, standing over it's shoulder

	def method_missing(name, *args)
		check_for_expiration
		@original_document.send(name, *args)
	end

Or you can use an array of delegated methods to send to the original class
Problem, if there isn't a method missing like `.to_s`

`BasicObject` is a great way to get around this. They only inherit a handful of methods.

### Method_missing for flexible apis

Use method missing to see if you can make sense of what the user wants to do, if not you can always throw a `NameException` error
**Magic methods**

Ruby Culture
	The look of the code matters. It matters because the people who use the code, read the code, and maintain the code matter. pg 289

#### Update Existing Classes with Monkey Patching

Ruby has **open classes**
You can change the behavior of any class at any time.

If you define a class, with some methods
And then define it AGAIN you are *modifying*

*Last `def` wins*

**monkey patch** - Modifying existing classes on the fly

guerrilla patching, gorilla patching, monkey patching

`alias_method` - copies a method's implementation, giving it a new name

pg 298, using `alias_method` to reference the older version of the class method we are overwriting.

Can do anything, make class methods private, public or remove them.

`ActiveSupport`

#### Create Self-Modifying Classes

Ruby class definitions are executable.

You can have a `puts` method use `self` to see when classes are defined.

Ruby methods are defined one step, or method, at a time

*You can put programming logic in your classes* And define methods based on constants. PG. 309

You can define a method inside of another method Pg. 310

Meta Programming calls out for tests!!

## Create Classes that modify their subclasses
You can use `define_method` and `class_eval` to define class methods on the fly.
Some examples `attr_accessor`, `attr_reader`, `attr_writer`, `has_many`

Modifying classes, using a method to modify those methods, and then moving those up to a super class.

**Inclination with metaprogramming is for dev to use them all the time or never use them**
Need to strike a balance

#Pulling it all together
#### Making internal DSL's
Domain Specific Language

**External DSL** Building your own language from scratch
**Internal DSL** Built within an existing language
Use instance_eval to stop from having to call `.new`
`instance_eval(&block) if block

#### External DSL's
Can use Treetop to build parsers

#### Package Programs as gems

#### Know your ruby implementation

## Premature optimization is the root of all evil
