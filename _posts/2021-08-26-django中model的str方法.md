---
title: django的model中 unicode 与 str 
date: 2021-08-26 15:50:00 +0800
categories: [Django, model]
tags: [Django]
pin: false
---

### **django的model中 `def __unicode__(self):` 与 `def __str__(self):` 做什么用的**

在Django中，Python3只能用__str__方法，Python2就使用__unicode__方法。因为更安全一些。  

这个__str__的作用是美化打印出来的结果，使人类更方便查看。  
看下面例子，如果没有__str__方法，打印的结果是\<\_\_main__.Test object at 0x0000022D6D1387B8>格式，  
有了__str__方法后，打印时会按照__str__定义的格式来打印，打印结果为Name:xiaoming
```python
class Test:

    def __init__(self, name, job):
        self.name = name
        self.job = job

    def __str__(self):
        return 'Name:' + self.name
instance = Test('xiaoming', 'Teacher')
print(instance)
```


### **`__str__()`**

`Model.``__str__`()

The `__str__()` method is called whenever you call `str()` on an object. Django uses `str(obj)` in a number of places. Most notably, to display an object in the Django admin site and as the value inserted into a template when it displays an object. Thus, you should always return a nice, human-readable representation of the model from the `__str__()` method.

For example:
```python
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)

    def __str__(self):
        return '%s %s' % (self.first_name, self.last_name)
```


### **`__str__() and __unicode__() methods`**
In Python 2, the object model specifies `__str__()` and `__unicode__()`_ methods. If these methods exist, they must return str (bytes) and unicode (text) respectively.

The print statement and the str built-in call `__str__()` to determine the human-readable representation of an object. The unicode built-in calls `__unicode__()`_ if it exists, and otherwise falls back to `__str__()` and decodes the result with the system encoding. Conversely, the Model base class automatically derives `__str__()` from `__unicode__()`_ by encoding to UTF-8.

In Python 3, there’s simply `__str__()`, which must return str (text).

(It is also possible to define `__bytes__()`, but Django applications have little use for that method, because they hardly ever deal with bytes.)

Django provides a simple way to define `__str__()` and `__unicode__()`_ methods that work on Python 2 and 3: you must define a `__str__()` method returning text and to apply the python_2_unicode_compatible() decorator.

On Python 3, the decorator is a no-op. On Python 2, it defines appropriate `__unicode__()`_ and `__str__()` methods (replacing the original `__str__()` method in the process). Here’s an example:
```python
from __future__ import unicode_literals
from django.utils.encoding import python_2_unicode_compatible

@python_2_unicode_compatible
class MyClass(object):
    def __str__(self):
        return "Instance of my class"
```

This technique is the best match for Django’s porting philosophy.

For forwards compatibility, this decorator is available as of Django 1.4.2.

Finally, note that `__repr__()` must return a str on all versions of Python.
