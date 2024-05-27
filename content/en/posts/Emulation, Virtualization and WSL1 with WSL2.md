---
title: "Emulation, Virtualization and WSL with WSL2"
date: 2023-08-29T22:58:00+03:00
draft: false
---
Imagine, you want linux on Windows. You want to run some linux apps, have linux-based manage of groups, users, and so on. How to do that? You have two ways:
1. **Emulate Linux**. You create something from scratch, that looks like Linux. It has the same (but not all, otherwise you'll recreate original Linux) core api methods for most popular Linux apps. You will not have original core, but you'll have something, which looks like Linux at first sight.
2. **Insert original Linux to virtual environment** and working with original linux core. You'll have a real original Linux at Windows. Yes, you can make it seamlessly as much as possible. But be ready to get overheads.

So **WSL1** is the first way, and **WSL2** is the second. The main problem of first version of **WSL1** is because... it is not real Linux. And when you want to run Docker container on Windows with WSL1, Windows starts virtual machine with original Linux. Because what is docker container? It consist of cgroups with namespaces based on linux core + some HDD space with root directory. And now you already have two Linux - WSL1 and original Linux. Therefore Microsoft decides to create WSL2, real Linux onboard :) And contaners with WSL2 runs native.

Ok, got that sorted out. So now,  what is **Emulation** and **Virtualization** at all?

Emulation is more general word. You can emulate anything: for example, when you say 'mew', you emulate cat! Therefore I can use this word when I say that WSL1 emulates Linux. But at the same time, we have more narrow meaning of this word, if we considering it within emulation of something, which applies to emulate environment. And on this case we can use two different words:
1. you can **emulate** environment for run inside some program(OS)
2. you can **virtualize** environment for run inside some program(OS)

And meaning/difference between 1 and 2 is a matter of agreement. Emulation is when to simulate environment you need to simulate all from scratch, even processor calls. Therefore emulator is really slow. But it can emulate any environment you want (theoretically).

Virtualizing is when you simulate just couple of things. For Linux, to virtualize environment for another Linux you need to use another cgroups + namespaces + root dir. It is built in functional in Linux. So, virtualizing linux on linux is very simple and ther is no (almost no) any overhead.