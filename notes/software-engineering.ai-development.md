---
id: cn84w4kojyd4t3cuemx8pjg
title: Ai Development
desc: ''
updated: 1783581498690
created: 1783580964654
---

We have had some teething problems with getting used to AI development. It is crazy to have so much happening at once. Some hard things have been when we have really big features coming in, with not that much feedback in big PRs.

We had a meeting on what to do, here is a summary of what we discussed.

### Action Items

* Review and improve the test suite - make sure we are testing functionality, not implementation.
* Refactor the core architecture - we are going to have a lot more code and we need to be ready for it.
* Treat initial development as a prototype; rewrite or refactor heavily (explaining that the initial version was a prototype) afterwards. (grey-red-green-gold strategy; prototype-tests-implementation-refactor, an extension of test driven development)
* Merge changes in small, frequent increments. 
* Discuss new features before implementation. As the AI is working we can be talking. (personally I find this hard sometimes, but I don't tend to use AI for massive tasks at once.)
* Clearly define public vs. private interfaces. We care about the public interfaces more than anything else.

### Code Review Focus

Code review gets much harder complete when we have 10,000 lines per pull request. You can't go line by line, so what do we do?
We pretty much will have to review the higher level architecture and rely AI agents to review the individual lines. This can include:

* Does this code affect anything outside itself?
* What does the interface to the code look like?
* Is the AI unnecessarily re-implementing existing functionality?

### Development Process

* Consider whether to:

  * Build a rough draft first, then re-implement, or
  * Use TDD to guide implementation.
* Spend more time discussing high-level architecture.
* Better modularisation and well-defined external interfaces.
* Merge branches more frequently.
* Improve testing without creating a combinatorial explosion of test cases.

### AI-Assisted Development

There's gonna be a lot of this, we can continue moving quite fast as we get better at using ai and as the tools for using it get better too.

* Keep AI-generated code consistent with the current code.
* Avoid duplicate code and unnecessary wrappers (which AI likes to write). We gotta make sure to cut back code sometimes too.
* Define what good AI-generated tests look like.
* Encourage questioning and iteration rather than accepting the first AI output.

### General Themes

* Improve architecture and modularity.
* Increase discussion and review before implementation.
* Balance rapid prototyping with deliberate refactoring and testing.
