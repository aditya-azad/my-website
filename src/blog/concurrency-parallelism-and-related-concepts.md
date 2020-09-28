---
title: "Concurrency, Parallelism and Related Concepts"
date: 2019-09-10
tags:
  - concurrency
  - parallelism
  - go
  - python
  - javascript
layout: layouts/post.njk
---

One of the most confusing things that I have tackled recently are the difference between all the terms that seem to fall under this parallel/concurrent concepts which are often considered the same thing by a lot of people (but are actually different).

This post is an attempt to clarify all the confusion surrounding those terms.

## A few definitions

Before I get into the specifics there are a few things that are needed to be clarified so that we are all on the same page.

### Process

A process is an instance of a computer program that can have multiple threads within it. It has it's own share of memory space and the communications between different processes is often minimized by the operating system for security reasons.

An example might be when you run an application the operating system spawns a process which can be seen in the task manager.

It takes more time to create and terminate a process

### Thread

Thread is the fundamental quantity that is executed by the CPU. A process can have multiple threads within it. Threads belonging to same process share resources among them. They are usually run one after the other in time slices (multitasking) giving the illusion of being run at the same time. In some cases they might be running on a different processor core altogether.

An example of this might be when a web browser is downloading a file while you are navigating the web pages and doing your usual stuff on the web.

It takes less time to create and terminate a thread within a process

### Coroutine

A coroutine is like the function call stack. In a function you can call other functions, once the called function is finished executing, the control is reverted back to the caller function. But in the case of coroutine the called function can revert back to the caller function so that it can run some of its code and hand back the control to the called function from where it left off.

An example of this is `yield` construct of python. The function containing the yield returns the iterator when it encounters a `yield`. The caller function does some work and then calls `next(it)` where the called function runs till the next yield and returns the new value as so on. That is one of the reasons why is it better to use an iterator than a simple for loop.

## Concurrency and Parallelism

Concurrency is when a few tasks are giving the illusion of running at the same time but are usually running in time sliced environment one after the other. They are not actually running at the same time. It like when you are reading something and writing notes along with it. You are not literally writing as you are reading but doing more of switching behavior between the two from time to time.

Concurrency can be implemented as time slicing or something like non-blocking architecture of JavaScript

On the other hand parallelism means that the tasks are running literally the exact same moment like in a multi-core processor. It would be when you are listening to music while coding. You are performing both the tasks at the same time and not listening to music for a few minutes, pausing the music and then doing some work and so on.

One way to parallelism can be by running processes on different cores altogether.

##

```
parallelism
concurrency
processes
coroutines
threads
multi-threading
green thread
js asyc event model
goroutines
rust or c++
```
