---
cover: https://cdn.pixabay.com/photo/2022/06/12/22/48/gradient-7258997_960_720.png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Recursion

## Introduction <a href="#introduction" id="introduction"></a>

Recursion is one of those things that some of you may or may not have heard of. This section of the notes will introduce to you what it is if you do not already know and how it works.

It is important to know at least a little recursion because some algorithms are most easily written recursively.

## The Run-time Stack <a href="#the-run-time-stack" id="the-run-time-stack"></a>

The run-time stack is basically the way your programs store and handle your local non-static variables. Think of the run-time stack as a stack of plates.&#x20;

With each function call, a new "plate" is placed onto the stacks. local variables and parameters are placed onto this plate. Variables from the calling function are no longer accessible (because they are on the plate below).&#x20;

When a function terminates, the local variables and parameters are removed from the stack. Control is then given back to the calling function. Understanding the workings of the run-time stack is key to understanding how and why recursion works.
