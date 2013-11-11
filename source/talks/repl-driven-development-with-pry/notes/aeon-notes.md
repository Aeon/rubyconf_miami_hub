---
author: Anton Stroganov (@aeon)
---

REPL-driven development with PRY - [Conrad Irwin](http://cirw.in)

- starts with basically... just 
	`loop { puts eval $stdin.readling }`
- pry catches exceptions in your code without crashing the whole eval loop....
- easy to use... just run binding.pry
- adds all kinds of introspection tools...

- REPL-driven development says...
	- don't write code first
	- don't write tests first
	- experiment in REPL to figure out what you're doing...

- probability of a bug is multiplied by the amount of code... github seems to have about one in a billion chance of a crash considering the size of their codebase.
	- how do we reach that kind of reliability?
		- run the code...
		- read the code... 
		- understand the code...
	- Pry makes it easy and fast to do all three....
		- run pry
		- type variable name to print out the information about it
		- ? (show-doc) shows documentation about object or class
		- $ (show-source) shows source of object or class
		- ls shows methods on object
		- syntax highlighting helps debug the issues...
		- super cool!
	- Downsides of REPL
		- doesn't keep a record
		- you have to retest lots
		- hard to re-test
	- Solution: tests!
		- Find solution in REPL
		- Record findings in tests
		- Write code that works the first time
	- Tests help more and more
		- when code-base is large
		- with api design
		- documentation of assumptions
	- Downsides of tests?
		- slow to write
		- slow to run
		- become outdated...
	- Pry can help!
		- gem install pry-plus
		- require 'pry-rescue/rspec' (or /minitest)...
		- when your tests fail... you just get dumped into the pry console! (OMG!)
			- you can use "try-again" to re-try the test inline...
			- add a breakpoint, with "break", then use try-again to step through from beginning of the test...
			- step / next
			- play -l 6 will execute line 6 and leave the current line where it is.
			- edit -m allows you to edit the method in place IN THE ORIGINAL TEST FILE and reload it... more MAGIC
			- save, remove breakpoint, run "try-again"
		- test passes, boom, we're done
	- Even with testing
		- bugs slip through...
		- so you probably want some kind of exception tracking in production... like bugsnag (his company)...
		- so how do you track down a weird problem?
		- binding is a magic ruby object that encapsulates the state of the currently running code (stack snapshot?)... 
		- example: a param is was passed as nil unexpectedly... 
			- use binding.pry
			- use up to go up the stack to the caller of the current function
			- use cd to move sideways in the stack to investigate sibling objects
			- use wtf to get the first 5 lines of latest backtrace....
			- use whereami to show current context
			- use down to move down the stack into the method

- overall... super cool stuff!!!