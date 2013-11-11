---
author: Anton Stroganov (@aeon)
---

## The Polyglot in the Code - An Elixir/Ruby Mashup

Johnny Winn, hashrocket, [@johnny_rugger](http://twitter.com/johnny_rugger)

- has seven kids, homeschooled all of them... so a lot of experience figuring out teaching methodologies...
- why bring this up?
- challenge us to spark our curiousity, we tend to get expertise in a field and stay in the comfort zone after that

- learning process...
	- throwing things together without knowing what you are doing still teaches you things
	- knowledge is constructed not acquired...
	- you don't just read a book and wake up the next day knowing ruby, you have to build up the knowledge by doing things

- scaffolding theory of language acquisition:
	- if you give students something that they already know alongside new material, they progress faster 

- so, use ruby to scaffold around another language to make learning it easier.

- intro to elixir, created by Jose Vallim...
	- use pattern matching
	- use guard clauses
	- functions have to always be in modules
	- mix lets you configure elixir environment 
	- dynamo - sinatra-like web framework for elixir
	- ecto - handles database interactions, pretty different from activerecord/sql approach in modeling the data
- what is missing?
	- spec/BDD testing... elixir has xunit, but that's not enough
	- data migrations...
- well, why not use ruby?
	- just create an elixir project
	- then create a Gemfile
	- testing:
		- disable default servers for cucumber, since we want to test elixir code that's running with mix server
		- add a task to invoke cucumber from `mix` and it works.
		- configure db config in cucumber.rb and establish connection so that you can connect to test db from tests
	- data migrations
		- add builder and activerecord to your gemfile...
		- add a Rakefile that will require the migrations
		- create db namespace and run migration tasks in Rakefile
		- and yep, rake db:migrate works. pretty much same as setting up "standalone migrations" gem really.
- back to elixir...
	- set up db Repo object, Model object, and Query objects.
	- set up router to handle urls we are interested in
	- set up the template to render - uses list comprehension in the template
- run the tests again, and... it works.

- So now what?
	- use existing tools to fill gaps in the new toolkits if we need to
	- learn things by using your existing knowledge and tools as starting point