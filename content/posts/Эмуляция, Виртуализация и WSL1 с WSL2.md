---
title: Эмуляция, Виртуализация и WSL1 с WSL2
date: 2023-08-29T22:58:00+03:00
draft: true
---
Представьте, что вы хотите Linux на машине Windows. Вам нужно запускать какие-то приложения, работающие под Linux, или вы хотите управлять группами или пользователями на основе движка linux. Как это сделать? У вас есть два пути:
1. **Эмулировать Linux**. Вы создаёте что-то с нуля, что выглядит как Linux. Это что-то должно иметь такие же, как и у Linux методы api его ядра (во всяком случае, основные методы, но не все, иначе вы просто заново напишете ядро Linux :)). Это нужно для того, чтобы большинство популярных приложений под Linux могли обращаться к ядру Linux и не думали, что их дурят :). У вас не будет оригинального ядра, но у вас будет что-то, что будет похоже на Linux.
2. **Внедрить оригинальный Linux в виртуальное окружение** и работать с оригинальным ядром Linux. В этом случае уже Linux будет думать, что он работает на отдельной физической машине, то есть, мы создаем для Linux виртуальный физический компьютер. То есть, мы эмулируем железо для Linux. И тогда у нас самый настоящий Linux на борту Windows. Да, это создает доп.издержки, так как вычислительные мощности хостовой машины будут тратиться на эмуляцию виртуальной машины для Linux. 

Так вот, **WSL1**  - это первый путь, тогда как **WSL2** - второй. Главная проблема первого варианта в том, что.. ну, это не настоящий Linux. Например, если вам нужно запустить Docker Engine на таком Linux, у вас это не получится. И Docker Engine под капотом запустит на Windows классическую виртуальную машину (на основе `VirtualBox` или чего-то похожего). Почему? Потому что Docker container состоит из `cgroups` и пространства имен (которые основаны на ядре Linux), плюс немного места на диске. 

So **WSL1** is the first way, and **WSL2** is the second. The main problem of first version of **WSL1** is because... it is not real Linux. And when you want to run Docker container on Windows with WSL1, Windows starts virtual machine with original Linux. Because what is docker container? It consist of cgroups with namespaces based on linux core + some HDD space with root directory. And now you already have two Linux - WSL1 and original Linux. Therefore Microsoft decides to create WSL2, real Linux onboard :) And contaners with WSL2 runs native.

Ok, got that sorted out. So now,  what is **Emulation** and **Virtualization** at all?

Emulation is more general word. You can emulate anything: for example, when you say 'mew', you emulate cat! Therefore I can use this word when I say that WSL1 emulates Linux. But at the same time, we have more narrow meaning of this word, if we considering it within emulation of something, which applies to emulate environment. And on this case we can use two different words:
1. you can **emulate** environment for run inside some program(OS)
2. you can **virtualize** environment for run inside some program(OS)

And meaning/difference between 1 and 2 is a matter of agreement. Emulation is when to simulate environment you need to simulate all from scratch, even processor calls. Therefore emulator is really slow. But it can emulate any environment you want (theoretically).

Virtualizing is when you simulate just couple of things. For Linux, to virtualize environment for another Linux you need to use another cgroups + namespaces + root dir. It is built in functional in Linux. So, virtualizing linux on linux is very simple and ther is no (almost no) any overhead.