---
title: "To annotate or do not to annotate"
date: 2023-05-11T00:26:00+03:00
draft: true
---

Python is not strict annotated language. That means I don't need to set variable type. But since v. 3.5 python support annotating, not for compiler, but for IDE and static code analyzers. 

And there is a question, annotation is it good practice for python? Should I use annotate or not? In which cases I must annotate? 

First of all you should understand that annotation doesn't work without tuned linters, tests and static code analyzers. So, if you don't use it, there is no reason to apply annotating to your python code.

You definitely sould use annotation in a big projects, with CI/CD pipeline with tests and static code analyzers. But in small projects it's like annotation for the sake of annotation.