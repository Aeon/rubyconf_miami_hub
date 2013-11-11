---
author: Anton Stroganov (@aeon)
---

Extending CRuby for fun and profit - Andy Pliszka
- put your high level abstractions in ruby
- put your low-level performance-intensive stuff in C

- get ruby source from github and compile it (need openssl)
- make test, make install...
- install gems

- run with lldb to debug!
> lldb ~/my/ruby upstring.rb

example: bignum.c
- first, look for Init function
- contains all the method definitions for the Integer, Fixnum class...
- investigate the implementations of the methods to see how it all works

How do you define a class in c?
- define the method Init_Array()
- rb_cArray is VALUE, which is basically object
- rb_cArray = rb_define_class("Array", rb_cObject); # defines the array class and puts it in rb_cArray

How to define a method on the class?
- rb_define_method(rb_cArray, "length", rb_ary_length, 0); # arguments are - class to implement method on, method name, c function implementing the method, number of arguments the method takes)

Two worlds...
- C
	- working directly with memory
	- malloc/free
- Ruby
	- working with heap and objects
	- GC handles things for you magically
- switching between the worlds requires to make sure that you convert the values correctly when passing them between ruby methods to c calls and vice versa
- use macros that come with the Ruby codebase (like LONG2NUM)
- converting C functions to CRuby functions mainly requires changing the signature, input, and output to return VALUE (objects) instead of long or int or whatever.

Why would you do this?
- performance...
- in a naive benchmark CRuby implementation of fibonacci formula is ~30x faster than plain ruby
- a lot of methods are already implemented in C - numeric algorithm reference books, whitepapers, etc.
- instead of trying to implement them in ruby, it can be worthwhile to just convert them to CRuby and compile them into the language itself.

CLongArray:
- init unwraps the ruby object to extract the pointer to the C array...
- good reference to see how it accesses the struct with [] method

CLongArray qsort:
- basically a wrapper function that unwraps the ruby objects and....
- passes the unwrapped value list to the dumb C quicksort implementation that has no idea about Ruby at all

Example: Graph representation
- simple Adjacency-List representation
- C data structure containing an array of linked lists representing nodes.
- ... code code BFS code code...
- magic! 19x faster than plain ruby on a 1M node graph.

Pitfalls:
- try to avoid holding references to ruby objects in c code so they don't get garbage collected from under you
- there is options for Data_Make_Struct that let you mark objects as used so garbage collector doesn't kill them 

Do you really need to compile it into ruby? 
- No, just write a ruby extension - you might want the ruby source just for reference to see how things are put together.

- http://www.rubyinside.com/how-to-create-a-ruby-extension-in-c-in-under-5-minutes-100.html