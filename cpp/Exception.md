##   Exception :
> run time error that can be handled to avoid crashing the program.


| Keyword | Purpose                                       |
| ------- | --------------------------------------------- |
| Try     | Wraps the code that might throw an exception. |
| Catch   | Used to handle the execption.                 |
| Throw   | Raise/Throw an exception.                     |
```c++
try {
	//code that might throw an exception
	throw exception_type;
}

catch (exception_type e) {
	// code to handle the exception
}

```

| term     | means                                              |
| -------- | -------------------------------------------------- |
| what( )  | method to get the error message from an exception. |
| noexcept | It promise not to throw other exception            |
