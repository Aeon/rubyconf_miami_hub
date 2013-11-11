---
author: Anton Stroganov (@Aeon)
---

[@lsegal](http://twitter.com/lsegal)

Are rubyists writing good code just because they're good testers? No... it's a question of tools.

- Visualization tools
	- help you know what your code is doing...
	- Examples: 
		- Visual studio diagnostics hub
		- Visual studio debugger can show the call graph of the program as a tree.
		- XCode instruments shows what UI element was activated in Instruments...
		- VisualVM shows profiling info as the program runs for Java
	- great for discoverability, finding out things you didn't even know you should know
	- Eclipse - show list of Implementors - what classes implement this method, what will break if I refactor it?
	- Eclipse - call references - what is the code that is calling this method?
	- These tools don't have to be part of an IDE... example - Firebug! (or chrome web inspector these days).
		- before firebug, it was much more horrible, because of lack of visibility.
	- [Ember inspector](https://chrome.google.com/webstore/detail/ember-inspector/bmdblncegkenkacieihfhpjfppoconhi?hl=en), chrome extension
	- [Angular Batarang](https://chrome.google.com/webstore/detail/angularjs-batarang/ighdmehidhipcmcojjgiloacoafjmpfk/related?hl=en)
	- Smalltalk screenshot... we are barely getting close to what we had in the old days.
	- Visualization tools in ruby?
		- memprof (out of date...)
		- perftools.rb
		- JRuby - can use NetBeans and other java tools for JVM profiling tools

- Linting tools - basic checking for common problems...
	- Reek, Flog, Flay
		- detect code smells, but not common errors.
		- CodeClimate uses them for grading the code...
		- But they check code beauty, not code correctness, so they cannot replace testing.
	- ruby-lint

- Static analysis
	- more powerful than linting...
	- Defect finding,
	- brakemanscanner.org - looks for common flaws in rails code 
	- "static detection of security vulnerabilities in scripting languages" (2006? paper) - targeted php, but could (should) be implemented for ruby
	- Fuzz testing, 
	- Microsoft Research - "Automated Whitebox Fuzz Testing" paper - SAGE (2008)
	- ruby has Heckle and Mutant github/mbj/mutant, but neither is real fuzz testing, they are more of a code mutator
	- FuzzBert?

- Symbolic Execution...
	- Run your code with no immediate values...
	- Automatic testcase generation... magic...
	- uses a bunch of intelligent heuristics for input values that will make your algorithm break.
	- KLEE (LLVM)
	- Kudzu (JS)
	- Kiasan (Java, Spark)
	- there is nothing for Ruby... :(

- Extended Static Checking

- Ruby doesn't really have a scientific community compared to Python or Java... 
	- Ruby has so many web developers because it has great tools for them.
	- It needs more tools for other audiences (science, engineering, math) to attract those audiences
	- Having a broader audience always improves the language...

- Take responsibility... 
	- great tool ideas are just waiting to be implemented
	- scholar.google.com
	- some of his favorite papers: https://gist.github.com/lsegal/7375509