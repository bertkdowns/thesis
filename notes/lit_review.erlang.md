---
id: 81u0q3glh8ld6dq8yo2hyjw
title: Erlang
desc: ''
updated: 1743117524977
created: 1743117524977
---
# Erlang

[phd dissertation](https://erlang.org/download/armstrong_thesis_2003.pdf)


Goal of reading this: 
- To figure out how what principles and practices might be useful from a software engineering perspective
- Note general helpful things about a software engineering phd dissertation
- look at how things are measured, and what is used to back up arguments


# focuses

How can we make a fault-tolerant sodware system which behaves in a reasonable manner in the presence of software errors?
(I need one question to focus on, this is more broad than a research question though.)


# Interesting things

Error isolation - fault tolerance is fault isolation

A lot of it is just presenting your argument, what you have done, what the architecture, framework, or key characteristics are.

all unreliable message passing.

distributed systems theory - causual ordering


# argument evidences

A lot of it is not on argument evidences, but on the framework, what has been built, what principles are behind it, and why

chapter 8 and 10 focus more on the evidences - using case studies of it actually working. (I might have to do research on similar software and what resources they had and do)

You can prove stuff from axioms. you define what the essential characteristics are of the system, and you can argue other things from there. E.g becuase processes are isolated, adding new processes doesn't affect the existing ones. Time and care must be taken to define axioms and attributes.





# cool tidbits

result is actually used in companies

early identification of key outcome

decide what not to include - maybe some things could be a paper not a thesis?

this is a summary of 20 years of work

design using erlang was much faster

code upgrade


1:1 mappings are good

fault-detection and fail-fast - testing in production/defensive programming - auto-done by erlang. stopping if something doesn't work. How does this work as in comparison to stopping error propogation?

[Wikipedia: software design patterns](https://en.wikipedia.org/wiki/Software_design_pattern)