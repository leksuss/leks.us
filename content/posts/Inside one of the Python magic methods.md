---
title: "Inside one of the Python magic methods"
date: 2023-06-28T23:41:10+03:00
draft: false
---
Of course you know this Python syntactic sugar like:
```python
num += 1
```
That means:
```python
num = num + 1
```

But is it strictly equal? Actually, no. Because for the first exression is responsible one magic method (`__iadd__`) and for the second another:  `__add__`. And therefore Python can change behaviour in this expressions. For different types this behaviour manifests via different side effects. Easiest example which you can find in any Python book with hits&tricks is in Lists:
```python
lst = [1, 2 ,3]
print(id(list))  # 4548312912
lst += [4, 5, 6]
print(id(list))  # 4548312912
```
In example above you can see that `+=` is equal `extend` list method, because resulted list is the same. But if we use another expression:
```python
lst = [1, 2 ,3]
print(id(list))  # 4548312912
lst = lst + [4, 5, 6]
print(id(list))  # 4543366528
```
The `id` is changed, that mean we have a brand new list. In most cases it's a bad code, because you create another list and eats memory (yes, GC - garbage collector - will remve extra list from memory, but not immediately).

And how do you think, whats the difference between my first example with integers? Do you think it's equal? Really? No! It's also has difference. Look at the code below:
```python
res = 1
k = 0
res += k or 1  # expr 1
```
How do you think, what will be inside `res`? Here we use `OR` operator [I wrote early](https://leks.us/posts/or-and-and-in-python/)  Looks like this expression equal of this:
```python
res = res + k or 1  # expr 2
```
But it is not! In expression 2 you will firstly evaluate `res + k` and then check if it's `True` or `False` and then `OR`  will run. In expression 1 firstly will run `OR` statement, and then result will be added to the `res`. So, difference between expr 1 and expr 2 is order of operations. 

Why this happens? I think, core Python Developers decided that operator `+=` firstly is an assignment operator. And assignment operator evaluates at the last moment, afer all other operators. 

It is usefull sometimes to check the difference between syntactic sugar and original expression.