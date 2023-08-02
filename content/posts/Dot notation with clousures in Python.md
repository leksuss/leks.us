---
title: "Dot notation with clousures in Python"
date: 2023-07-03T01:19:00+03:00
draft: false
---

Let's dive some deeply in [closures in Python](https://leks.us/posts/closures-in-python/). Look at this example:
```python
def func_as_object():
    def echo_foo():
        print('run echo_foo() func')

    def replace():
        pass

    replace.echo_bar = echo_foo
    return replace


f = func_as_object()

f.echo_bar()  # prints 'run echo_foo() func'
```

What happens in this code? What is the construction - `replace.echo_bar` ? 

Okay, okay, let's start from another side. Here is another code:
```python
import types

mod = types.ModuleType('my_module')

def my_func():
    print('Hello from my_func in my_module')

mod.my_func = my_func

mod.my_func()  # prints 'Hello from my_func in my_module'
```

They are looks similar, and that is! `types.ModuleType` - it is an class which represent python  module object. Wheny you import some module in python script,  python create instance of this class like in example above. So, expression `mod = types.ModuleType('my_module')`  is equal to `import my_module as mod`. This is the first step of explanation. The second - what happens in this string? `mod.my_func = my_func`. `types.ModuleType` class allows you to create methods of it's instences by assign any function. To create new method for `mod` we can simply write:
```python
mod.new_method = any_func
```
This is becomes possible because `types.ModuleType` allows you to use methods `getattr()` and `setattr()`.

Now then going to our first example with closure. When python sees construction in enclosing or local scope like:
```python
def foo():
	def something():
		print('something')
	def replace():
		pass

	replace.to = something
```
he automatically create instance of `types.ModuleType` named `replace`, and then it has the same behaviour like we talking before.

Note again, this is working only in local or enclosing scope, not global.

Why is it needed? Actually, I don't quite understand. If you can give me any useful examples of this feature, please, feel free to [contact me](https://leks.us/about/)
