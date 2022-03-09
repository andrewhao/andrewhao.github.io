---
layout: post
title: What's the fuss about formal specifications? (Part 1)
categories:
- Formal Specification
- Engineering
---

<h2 class="intro">What Math ✨ can bring to your daily toolbox of programming tools to write robust, concurrent programs: A light introduction to TLA+</h2>

If you've been writing software for any amount of time, you may be familiar with the many tools we have available to us to ensure correctness, consistency and debuggability of our systems. They range the gamut of unit / acceptance / integration tests, QA plans, CI/CD automation and the like. System or language tools like type systems, interactive debuggers and profilers abound. Practices emerge like DevOps process, TDD/BDD, and even Agile process itself can be argued to be invented toward the goal of writing correct, robust, easy to maintain systems.

Surely these tools are advanced enough in the 70-plus years of computing to help! But no - with the rise of distributed computing, the classes of bugs that start to emerge start to get ornery and complex, are usually nondeterministic, and often beyond the reach of ordinary tools.

But what if I told you there was another option from the world of... *math*?

## Enter formal verification

What if there was a way to guarantee that our systems and algorithms are performant, run correctly, are reliable against race conditions and the like?

Here's how it works:

1. You describe your system (or program) in terms of formal logic statements. You assert specific conditions that must hold throughout the program runtime (invariants). You write this in the form of a proof (that lives outside your actual program).

2. The tool has a "model checker" which is a glorified BFS search algorithm that explores every possible state space of your program proof and lets you know if the invariant conditions hold.

3. If they do - congratulations! You've verified your system. If it doesn't pass - congratulations! You've found a potential bug!

4. Using the results from the model checker, you can fix the proof to fix the model checking error. This will translate into a real world fix that you can then roll back into your program.

It's not magical. It's also a lot of work, and in all fairness, slightly out of the reach of the typical industry programmer. But it's much more in reach than you think!

### A simple example

I'll be using a tool called TLA+, and writing a sample spec from a derivative syntax called PlusCal. I'll walk us through a simple example that can be found on the [Learn TLA web site](https://learntla.com/introduction/example/).

```tla
---- MODULE transfer ----
EXTENDS Naturals, TLC

(* --algorithm transfer
variables alice_account = 10, bob_account = 10, money \in 1..20;

begin
A: alice_account := alice_account - money;
B: bob_account := bob_account + money;
C: assert alice_account >= 0;
end algorithm *)
=====
```

Here, we are specifying the behavior where Alice transfers some amount of credits to Bob, provided they both have 10 in the bank.

You'll notice this feels a lot like writing a program in your favorite language! Variables are set up, then the algorithm is implemented in the `begin..end` block.

Note that `C:` contains the assertion (the invariant).

When we run the model checker, the model checker will explore the space of:

```
alice_account = 10
bob_account = 10
money = 1..20
```

For a total of 20 initial spaces to explore `(10, 10, 1), (10, 10, 2)`, etc.

(By the way, this is much better [explained on the site](https://learntla.com/introduction/example/): please read more there!)

For the sake of time, I will direct you to some great resources:

- [Learn TLA](https://learntla.com) - A beginner-friendly resource from author Hillel Wayne
- [Practical TLA+](https://link.springer.com/book/10.1007/978-1-4842-3829-5) - Hillel Wayne's more comprehensive resource for specification programming in TLA+
- [The TLA+ Home Page](https://lamport.azurewebsites.net/tla/tla.html) - Leslie Lamport (Author of TLA+)'s resources for learning and running specs written in TLA+

## Up next

I'd love to run through a real world example of using TLA+ to specify a distributed system, loosely based on a concurrency bug we saw recently at Lyft. Stay tuned!
