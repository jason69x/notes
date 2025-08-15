If you forget to include `self` in the method parameters, you’ll get an error because Python won’t know where to bind the instance.

```python
p.greet()  # Python internally does: Person.greet(p) thats why we pass self
```


try-except-else-finally

