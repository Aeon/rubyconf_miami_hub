---
author: Anton Stroganov (@aeon)
---

# Secrets of the Standard Library - Jim Gay - [@saturnflyer](http://twitter.com/saturnflyer)

- Recommended: Ruby under a microscope - amazing book to learn how ruby internals actually work

- Anyway, check out /lib directory in ruby source, there's actually a lot of stuff, written in ruby, that people don't know about or use.
- Delegator
	- require 'delegate'
	- wraps a class and passes any method calls it does not have to the wrapped class with method_missing.
	- Keep the User class clean, create a UserDecorator class to wrap it and have methods that don't relate directly to user's base functionality (like user.birth_year... calls User.born.year).
	- define == to compare the object to the wrapped object and self... hmm.
	- but better to just use SimpleDelegator because it handles this stuff for you as well as preventing other problems.
	- __get_obj__ and __set_obj__ are standard, but kind of ugly
	- User < SimpleDelegator - would be nice to call the wrapped object as user, not __get_obj__ 
		- Could :alias it, but that gets repetitive too
		- self.inherited used to define behaviour to run when a class is instantiated by a child class...
		- can use it to figure out the child class name, and use klass.send() to create the aliases automatically.
	- Delegator removes methods from Kernel module so that the missing methods get passed on to the wrapped object... neat.
	- drapergem/draper is interesting too
	- So this is very useful when you try to redefine methods or use method_missing
		- def DelegateClass - 
			- klass = Class.new(Delegator) - create a new class with same methods etc. as an existing class
		- you could do something like
			- UserDecorator < DelegateClass(User)
			- but that loses the "User" in ancestry because it makes an anonymous copy of the class, so it can be more confusing...
	- the rabbit hole goes deeper... basically can do lots of your own magic with this
- Forwardable
	- User has an address
	- You could say, User.address.city, but that requires the rest of the system to know that user has an address which has a city... really want to just be able to say User.city
	- you can write a module, Forward
		- and use class_eval to define a method that will be defined in the context of the current class
		- example use:

			class Person
				extend Forward
				forward :address, :city
			end

		- which will pass call to Person.city to address.city.
	- but standard library already has a tool to do it:

		require 'forwardable'
		class Person
			extend Forwardable
			delegate [:city, :number, :postal_code] => :address
		end

	- ActiveSupport adds a couple of extra features:
		- allow_nil
		- prefix to define a prefix for the methods to be delegated
	- could use this to make third party code hot-swappable... delegate "save_exception" method to a class that could be changed later...
	- SingleForwardable is for class level methods, Forwardable is for defining instance level methods 
- SimpleDelegator - run time behaviour
- Forwardable - load time behaviour

- similar projects
	- saturnflyer/casting
	- myobie/rep
	- stevenharman/dumb_delegator
	- elight/modest_presenter

- clean-ruby.com