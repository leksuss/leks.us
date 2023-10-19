---
title: "OR and AND in Python"
date: 2023-04-24T23:34:00+03:00
draft: false
---

Logical operators in Python is not just "logical". You will have powerful tool If you understand it's mechanics. In Python operators `AND` and `OR` doesn't return boolean value. It **always** return one of the operand. So you can use logical operators not only in if/else statement. For example:

```python
print(1 or 2). # return 1
```

## How it works?

#### OR operator

In case of `OR` returned first value evaluated as `True`. If there is no `True` values, it returned last operand. This behavior very useful if you need to return value by default when evaluated value is `False` or `None` or zero:
```python
count_items = count(items) or 'No items'
```

Another example if some evaluated code return value or `False`, but you need `None` instead of `False`. You can write:
```python
count_items = count(items) or None
```
In this case if `count(items)` return `False`,  `count_items` will be `None`. Remember? If all of the operands are `False`, `OR` returns last operand.

In next example `OR` looks like dummy plug, but it works perfect and very clean:
```python
avg_salary = sum_salary / (count_emplyers or 1)
```
This code is for case when we iterate through some array of departments and calculate average salary of each department. And one department doesn't have any employers and salaries. Without `OR` we will receive `ZeroDivisionError` exception. But we add just `OR 1` and everything works. `avg_salary` now is `0`. 

But why we add round brackets? It's because logic operators has lowest priority and we need to evaluate `(count_emplyers or 1)` statement at first.


#### AND operator

In case of `AND`, conversely, returned first value evaluated as `False`. If there is no `False` values, it returned last operand (if it even `False`).  `AND` operator in this behavior is using quite rarely.  Here is classic example:
```python
def digital_root(num):
	return num % 9 or num and 9
```
WTF? Okay, okay, maybe it's not a classic :) This func calculates [digital root of a number](https://en.wikipedia.org/wiki/Digital_root). 
Expression `num % 9` almost always return right answer. Almost, because in one case is not:
if num don't have remainder with division on `9`. But digital root of any number which divided without remainder on `9` is.. `9`! So, we should return `9` if there is no remainder:
```python
return num % 9 or 9
```
Right? Right! Or no? No! We forget about zero! If number is `0` we should return `0`, not `9`. In this case `OR` plays a cruel joke. And now `AND`  takes the stage. `AND` have priority and evaluates first. Expression `num and 9` return `0` if num is `0`. So, we cover `num = 0` case with `AND` operator.


####  AND and OR are lazy

What does that mean? `OR` don't execute next values if it found truthly value before. And similarly `AND` operator don't execute next values if it found `False` before. It is useful in many cases, like this:
```python
some_list = ['one']
if some_list and some_list[0] == 'one':  # if some_list == [] there is no list index out of range error
	pass
```

`AND` and `OR` operators are really powerful. But do not get carried away with it. Simple and easy-to-read code is more important than shortness.