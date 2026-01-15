
euclidean `%` - take floor, towards $-\infty$
truncated `%` - take trunc, towards 0
(just do the above)

python - remainder takes sign on divisor
c      - takes sign of dividend
(just observation nothing todo)

`is` - checks if two variables refer to the same object.
`==` - checks if objects have the same value.

*procedural programming* is the issuing of instructions to a computer in an ordered sequence.

range maintains a small memory footprint, even over extended sequences, as it only stores the start, stop, & step values. The range function can iterate through long sequences of numbers without performance constraints.

Exceptions are a type of error causing your program to crash if not handled (caught).Catching them with a try-except block allows the program to continue. These blocks are created by indenting the block in which the exception might be raised, putting a try statement before it and an except statement after it, followed by a code block that should run when the error occurs:

![[python_exception.png|400]]

```python
if (n := len(data)) > 10:  # avoids multiple func calls
    print(n)
    
results = [y for x in nums if (y := expensive(x)) > 0]
```


*lazy evaluation* is the idea that, when dealing with large amounts of data, you do not want to process all data before using results.

*generators* perform operations on data in chunks as requested. they pause their state in between calls.

to write a generator use *yeild* instead of return.
every time generator is called, it returns the value specified by yeild and then pauses its state until its next call.

![[generators_python.png|300]]

