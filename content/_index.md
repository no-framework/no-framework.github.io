+++
title = "index"
insert_anchor_links = "right"
+++

## When you don't want to use a framework, but you want to have conventions

No-Framework is a concept library which you can use as you wish. The aim is to
offer pre-packaed architecture conventions, not running software packages.o

In general when a software team wants to use a framework it's usually for one of three reasons:

1. They want pre-packaged conventions. The framework tells them where to put complexity.
2. They use it because there's wide-spread support, and this allows them to
   easily get help and move quickly through common pitfalls.
3. They want pre-built common infrastructure. The framework ships simple APIs
   to do complicated stuff, and rewards them with a community effort to do it
   better than they could on their own.

No-Framewok is not openly opposed to the use of frameworks and not part of any
"you don't need a framework" mindsets (the author of this document actually
likes frameworks, a lot). Indeed, you don't need a framework, but often it
makes sense to have one. No-Framework is a collection of "codeless frameworks"
(Distros), made up of codeless concepts ("Packages"). 

## Key concepts

- Distros are a currated set of packages that "just make sense together", like
  you would expect from a framework.
- Packages are ideas for how to solve common problems. These are either very
  concrete design patterns, or strict rules on how to approach something
  (maybe something you would enforce at Code Review or thorugh a linter rule). 
No framework packages are ideas that stand alone. This means there are no
supporting scripts or helper functions or third party vendors neede to make the
idea work. Sometimes a helper function does make the idea better, in this case,
it is easy to write an actual software package with lines of code, but base it
on a corresponding no-framework package. Similarly, no-framework packages can
be based on ideas from real software packages, and paired down to a simpler
concept, or just rule expected to be enforced 

## What is "not a framework" and what is "no framework"?

A framework is often used to enforce rules. "No framework" brings rules but no
code. "Not a framework" is the reason most people dislike using a framework:
it's just a mess of any idea without rules. No-framework is code that you write
without a third party package to enforce concepts. It might be that a framework
or tool is based on a specific idea from the no-framework catalogue of ideas.
That's fine, but at that moment the software package is a framework, the
no-framework concept remains "not a framework", they do not merge, but the
concrete code can form a dependency on the idea.


### Where's the line between no-framework and frameworks?
In general, things that are well-supported by many languages can belong to
no-framework. For example: Functions, Classes, variables... but not things like
the exact functools package in python. The concept of regex is fine, but not
requiring a specific function in PHP that does string manipulation. If a
library is available or cloned/ported to many languages, it is not valid in
No-framework as it heavily depends on real code. This is a fuzzy boundary and
we will need to improve the explanation as cases come up.

## Why No-framework?

One of the reasons people like frameworks is because they provide rules, but
the rules are often enforced via lines-of-code that are a hard dependency both
in API design, software behaviour, dev-dependencies, or some other "code"
dependency. 

Frameworks are awesome for getting started with a project, but after some years
there is a non-trivial tax paid for the software dependencies (for your code
depending on a framework, but also a framework depending on libraries). Interestingly,
there is a more hidden dependency which is easy to break at any time: the ideas
behind the framework and libraries. Often, projects have an "invisible"
conceptual dependency on core ideas that might not be well stated in abstract
and only discoverable through the specific concretion.

No-framework offers one layer of abstraction between pure idea-space (the space
of all possible ideas) and code.

There are other reasons consumers of No-Framework might enjoy it, but they are
secondary benefits and not the primary goal. The biggest of those benefits is usually:
The team has higher ownership over their code because they wrote it, if they
need to get out of some situation they can modify their own code. Not all
software packages make this task very easy, many require hard forking to fix
and offer complex undocumented software architectures.
