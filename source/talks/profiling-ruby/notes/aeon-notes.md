---
author: Anton Stroganov (@aeon)
---

Profiling ruby from top to bottom - [@xshay](http://twitter.com/xshay), engineer at Square

- Mindset is important.... Believe you can find something... usually, you will.

Three steps...
- Hypothesize
- Isolate
- Instrument

- Ok... so let's say Rails boot is slow...
	- what does it do?
	- isolate, run ruby commands with 'time' to check how much various stuff takes
	- add a global timer from startup and add timestamp printouts at each major step...
	- "caller" method will give you the stacktrace... so you can see what was called at any given point... and go and change it....
	- added a stamp in the rails engine.rb :load_config_initializer to time each init... sometimes you find surprisingly stupid things.
	- https://github.com/rails/rails/pull/12745

- Sometimes there is opposite problem - needing to prove something is fast
	- benchmark a base and degenerate case...
	- graph the benchmark... so you can actually interpret the data. gnuplot works...
	- Suddenly you may find a low-level problem... uhoh...
		- But C code is just code, even if it looks terrifying at first glance :) (this guy is funny as hell)
		- The computer only does what you tell it to...
		- Start by simply messing with the code... comment out branches and see what happens... just so you can get an idea of what is going on.
		- rewrite the code as you read it in a language you are more familiar with - that actually makes sure that you understand what it does... even if it's ugly.
		- eventually figure out the optimization which sometimes is surprisingly simple

- Ok, my unit tests are slow... what's going on.
	- benchmark... you don't need to know how the code works or why it exists to benchmark it and find the bottlenecks.
	- sometimes you find out that the code is using features it doesn't even need...

- other tools:
	- perftools
	- dtrace (with Flame graphs)
	- strace

Also, pretty much everything at Square is either unit or integration tested... very very heavy coverage.