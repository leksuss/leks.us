---
title: Нотация через точку в замыканиях Python
date: 2023-07-03T01:19:00+03:00
draft: true
---

Давайте зайдем немного глубже в [замыкания Python](https://leks.us/posts/closures-in-python/) Посмотрите на этот пример:
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

Что происходит в этом коде? Что это за конструкция `replace.echo_bar` ? И зачем у нас есть внутренняя функция `replace`, которая ничего не делает?

Хорошо, давайте подойдем к этому с другой стороны. Вот еще один пример:

```python
import types

mod = types.ModuleType('my_module')  # create manually instance of imported module

def my_func():
    print('Hello from my_func in my_module')

mod.my_func = my_func

mod.my_func()  # prints 'Hello from my_func in my_module'
```
Эти примеры выглядят похожими, и это так и есть! `types.ModuleType` - это класс, который представляет объект модуля. Когда вы импортируете какой-то модуль в питоновский скрипт, интерпретатор python создает экземпляр этого класса как в примере выше. Только в примере мы это сделали вручную. Таким образом, выражение `mod = types.ModuleType('my_module')` эквивалентно выражению `import my_module as mod`. Это первый шаг. Второй - что же происходит в этой строке: `mod.my_func = my_func`? 
Класс `types.ModuleType` позволяет вам создавать методы своих экземпляров через назначение их полям функций. То есть, чтобы создать новый метод для экземпляра `mod`, мы можем просто написать:
```python
mod.new_method = any_func
```
Это возможно благодаря тому, что `types.ModuleType` позволяет вам использовать методы `getattr()` and `setattr()`.

Теперь, когда мы разобрались со вторым примером, давайте вернемся к первому, с замыканием. Когда интерпретатор python видит в enclosing или локальной области конструкцию вида:
```python
def foo():
	def something():
		print('something')
	def replace():
		pass

	replace.to = something
```
он автоматически создает экземпляр класса `types.ModuleType` с именем `replace`, и далее мы получаем то же поведение, как и у второго примера. Это работает только в локальной или enclosing области.

Зачем это нужно? На самом деле, я и сам не понимаю :) Если вы можете дать мне какие-нибудь полезные примеры использования такой фичи, пожалуйста, [напишите мне](https://leks.us/about/)
