
## @ decorator is used to modify or extend the behaviours of classes or functions in python 

> example 

```python

def decorator(func):
    def wrapper():
        print("Before the function call")
        func()
        print("After the function call")
    return wrapper

@decorator
def say_hello():
    print("Hello!")

say_hello()

```

output 
```
Before the function call
Hello!
After the function call

```


## Built-in decorators

@classmethod
@property

