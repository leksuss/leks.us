---
title: "Closures in Python"
date: 2023-06-18T11:48:00+03:00
draft: false
---

Not everyone programmer knows what is closures, but it is quite interesting, so I'm going to try explain it in this article.

### What is closure?

Let's take a look in this example:
```python
def say_name(name):
    def say_goodbye():
        print(f"Don't say me goodbye, {name}!")
    return say_goodbye


f = say_name('Andrei')
f()  # Don't say me goodbye, Andrei!
```

We define one func in another func and return internal func as object. Then we call external func, and then call internal. Looks OK, right? Or no? When you think about this code, you'll fount some strange thing: how the `f()`  calling knows the `name` variable? `f` is pointed to internal func, which doesn't accept any params. In this call we don't set any names.

Okay, let's took another example, more strange:
```python
def counter(value=0):
    def plus_one():
        nonlocal value
        value += 1
        return value
    return plus_one


c = counter()

print(c())  # 1
print(c())  # 2
```

Here we add `nonlocal` construction to be able to modify `value` in enclosing scope from local scope. So, what happens here? When I first time found this example I was really surprised. Somehow `value` remember its value (yes, it was bad idea to set name variable as `value`) between `c()` calls. There is no any global variable which stores `value`. Where is it stores? And what a hell is going on here? :)

Okay, okay :) Let me explain. Wen we call internal function in global scope, interpreter create snapshot of the used variables in enclosing scope (regardless it's nesting level). Python interpreter collect all links to all objects are used in internal function and save it in tuple named `closure`. You can se this tuple when ask dunder method `__closure__` of internal function. You can see how variable is _closures_ in our example:

```python
def counter(value=0):
    def plus_one():
        nonlocal value
        value += 1
        return value
    return plus_one


c = counter()
print(c.__closure__)  # (<cell at 0x1017da4c0: int object at 0x10156b910>,)
print(c.__closure__[0].cell_contents)  # 0
c()
print(c.__closure__[0].cell_contents)  # 1
```

So, the answer on question "where is `value` stores?" is: in magic method `__closure__` of internal function. Note,  you can't change this variables, because there is no direct mapping name - value. And you can't change order of the stored variables, because `__closure__` is tuple.