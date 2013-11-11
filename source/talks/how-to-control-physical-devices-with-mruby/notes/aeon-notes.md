---
author: Anton Stroganov (@aeon)
---

Controlling physical devices with mruby - team yamanekko

- they work for tatsu-zine.com, japanese ebook company

- goal is to make embedded programming more accessible
- embedded is a bit more difficult than web or desktop... memory limitations, no os
	- mruby is not an app, it's a library that can be embedded into an application
	- mruby is embedded in any c/c++ app and your ruby code is embedded as compiled bytecode in the app.
	- mrbgems are compiled in alongside mruby
	- very configurable, so it can fit hardware requirements
- 7 steps to use mruby
	- choose target board
		- need at least 64Kb, but 1MB+ is better
		- STM32F4 is pretty nice
		- Raspberry is nice too, but power hog, can run normal OS and use CRuby
	- mruby is raw metal, no OS... 
	- development environment
		- dwelch67/raspberrypi has good info about running things on raw metal pi without OS
		- cross compiler - launchpad.net/gcc-arm-embedded
		- dwelch67/build_tools
		- debugging:
			- JTAG for PI, OpenOCD, armjtag
			- For STM, use STMLink
	- write application
		- write ruby code
			- mruby can include the ruby code as source, or as bytecode
			- mrbc command will generate bytecode from ruby source so it can be embedded
		- write mrbgems
			- statically linked as compile time
			- minirake is like rake for mruby, but works even with ruby 1.8
			- build_config.rb - executed at host machine on build to build final image with mruby and mrbgems in.
		- write C code
	- cross build
		- eclipse plugin mrubytools
		- use openocd, telnet and gdb for debugging on board

- demos... it works... magic :o