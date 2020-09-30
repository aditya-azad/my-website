---
title: "Concurrency, Parallelism and Related Concepts"
date: 2020-09-30
tags:
  - concurrency
  - parallelism
  - threads
layout: layouts/post.njk
---

One of the most confusing things that I have tackled recently are the difference between all the terms that seem to fall under this parallel/concurrent processing concepts which are often considered the same thing by a lot of people (but are actually different).

This post is an attempt to clarify all the confusion surrounding those terms.

## Process

A process is an instance of a computer program that can have multiple threads within it. It has it's own share of memory space and the communications between different processes is often minimized by the operating system for security reasons.

An example might be when you run an application the operating system spawns a process which can be seen in the task manager.

It takes more time to create and terminate a process

## Thread

Thread is the fundamental quantity that is executed by the CPU. A process can have multiple threads within it. Threads belonging to same process share resources among them. They are usually run one after the other in time slices giving the illusion of being run at the same time. In some cases they might be running on a different processor core in parallel.

An example of this might be when a web browser is downloading a file while you are navigating the web pages and doing your usual stuff on the web. On the back end, these processes might be time sliced.

It takes less time to create and terminate a thread within a process.

Deductively, multi-threading means that many threads can exist within the process at once.

## Coroutine

A coroutine is like the function call stack. In a function you can call other functions, once the called function is finished executing, the control is reverted back to the caller function.

But in the case of coroutine, the called function can revert back to the caller function so that it can run some of its code and hand back the control to the called function from where it left off. This back and forth happens continuously.

An example of this is `yield` construct of python. The function containing the yield returns the iterator when it encounters a `yield`. The caller function does some work and then calls `next(it)` where the called function runs till the next yield and returns the new value as so on. That is one of the reasons why is it better to use an iterator than a simple for loop in a few cases.

## Concurrency

Concurrency is when a few tasks are giving the illusion of running at the same time but are usually running in time sliced environment one after the other. They are not actually running at the same time. It like when you are reading something and writing notes along with it. You are not literally writing as you are reading but doing more of switching behavior between the two from time to time.

Concurrency can be implemented as time slicing. Node.js more or less falls in this category instead of parallelism.

## Parallelism

On the other hand parallelism means that the tasks are running literally the exact same moment. It would be when you are listening to music while coding. You are performing both the tasks at the same time and not listening to music for a few minutes, pausing the music and then doing some work and so on.

One way to implement parallelism can be by running processes on different cores altogether.

## Native Threads

Native threads or non-green threads are the threads are created by the operating system, it requires the CPU/OS to actually support such feature, therefore there is a limited amount of these you can create depending on the CPU and OS you are using. They are generally more costly and difficult to create and maintain than green threads, but they can be run on different CPU cores.

## Green Threads

Green threads, on other hand, are implemented by an application or virtual machine virtually. It does not require any support from the OS to run. They are usually cheaper to create than native thread but they cannot run on multiple cores of a CPU. Resources are easier to share between these threads as they are created in the user space.
