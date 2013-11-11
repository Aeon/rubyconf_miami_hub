---
author: Anton Stroganov (@aeon)
---

Eliminating Branching, nil, and attributes 

David Copeland, [@davetron5000](http://twitter.com/@davetron5000), works at stitch fix, wrote a couple of books

Top chef - quick fire challenge featuring an ingredient tends to produce the following:
- the chef that is familiar with ingredient likely will produce the same thing he always does with it, which is not good enough
- the chef that has never seen it before, is forced to be creative and invent something, which seems to win more often than not

## null - the billion dollar mistake

- introducing null in 1965 caused innumerable vulnerabilities and pain down the years according to inventor...
- so, what if ruby did not have nil at all?
- Can use types - define a subclass system to eliminate optional values 
	- but that gets quickly complex and confusing for any normal-sized object...
	- and not very rubylike...
- well, what does nil mean?
	- value is not set
	- don't know what it is
	- there is no value
	- value is empty
- all of the above concepts are distinct
	- so let's make four classes to replace it...
		- Unknown, Unassigned, Empty, NoValue
		- but now we need to check all four values to decide whether to try to render the variable or not
		- so we add a couple of helper methods on these objects in NilSentinel class that we use as ancestor them...
		- and add KnownNilSentinel subclass to differentiate between "unset / unknown" vs "no value / empty"
	- this all gives us two things
		- ability to distinguish between the different reasons the value is empty in the code logic
		- better error reporting in stack trace

## attributes

- allowing users to know too much about your object internals makes for fragile and silly code.
	- if person.gender.present? && person.gender.salutation.present? - kind of crap, the code needs to know all kinds of stuff about the internals of the object
- instead, define helper methods like

     person.with_attributes { |attribute_name| ... }
     .else_with_attribtes {|attribute_name| ... }
     .else { ... }

	- the lambda has access to the both argument count and argument names, so they can just be used directly
- ok, but what if we need to change the attribute values...
	- let's define an update method that takes attributes and encapsulates the logic of updating multiple attributes
		- can enforce consistency
		- can enforce transaction around attribute update


## if statements
- replace checking for magic values with helper methods in the object that execute a given block based on the internal state... but that's kind of skirting the issue
- looks like js callbacks

	some_method
		.on_success{ ... }
		.on_failure{ ... }
		.on_exception{ ... }

- the api can encapsulate the error checking and implied assumptions to try to make the end-users code less likely to fail
- however all of this is just moving the "if" statement into the api somewhere, rather than eliminating it
- we could replace if statements with a list of lambdas, and use .detect to test each predicate, and execute the block associated with it in the list
- it would be a very confusing language if that was the primary way for logic checking...
- if we did have that though, we could detect the case where the conditions were not mutually exclusive and throw an exception... which is interesting!

## conclusions:

- removing pieces of the language and trying to figure out how to solve the problems without it can lead you to new and interesting ideas for tooling...
- not really production-ready, but some of these ideas are interesting to think about and investigate further...

- http://bit.ly/weird-ruby
- http://bit.ly/weird-ruby-source