---
title: "Finite State Machine as a state control"
date: 2023-05-22T17:22:00+03:00
draft: true
---

I don't have really big experience in programming right now, but I often encounter with such thing as **state**. Some of books says that the STATE is a King, it is most importanit thing you should take care about in programming.

### So, what is the state exactly mean? Why it is so important?

State is all data in your program at a specific runtime period. If you change data, any variable - you change the state. Most of the programs has a very huge count of states. Ideal program should have only 2 states: 
 - at the beginning of its execution
 - at the ending of its execution

Why we should avoid a lot of states? Because most of the errors occurs while transfer state from one function to another function. And now we come to such a concept as FSM (Finite State Machine).

### Finite State Machine 

FSM is a pattern when you have a number of states and rules for each state when it should be in certain state. And thats all. In the each moment your data (your program) should be in one of number states. Next, somethig happens: user click button, program receive message, database send data to script. And FSM checks if this changes is enought to change the state or not. 

You can think about FSM as a graph, the states is vertexes, and you jump between them based on chages made outside. All huge count of states you compress to several states. But FSM is not a silver bullet, your buissness logic should allow to allocate a well-defined set of states and rules to jump between them. A lot of programs has so many states, and you can't reduce it for different reasons. But if you can, FSM is your choice!