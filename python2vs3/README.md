# Python 2 versus Python 3

The most prominent differences between Python 2 and Python 3 are presented here. Thankfully, they're relatively easy to get used to. If you're curious about the other differences, [Sebastian Raschka's article](http://sebastianraschka.com/Articles/2014_python_2_3_key_diff.html) on the topic lays them out nicely.

## Using `print`

In Python 2, `print` is a statement, while in Python 3, `print` is a function. Practically, this means that in Python 2 you can do things like:

```python
# using Python 2
print "Hello, World!"
print "LaunchCode", "is great"
```
Output:
```
Hello, World!
LaunchCode is great
```
In Python 3, the equivalent code would be
```python
# using Python 3
print("Hello, World!")
print("LaunchCode", "is great")
```
For most intents and purposes, you can see that Python 2 print statements don't use parentheses, but Python 3 print statements do. That said, you can do the following in Python 2:
```python
# using Python 2
print("Hello, World!")
print("LaunchCode", "is great")
```
with output:
```
Hello, World!
(LaunchCode, is great)
```
Hence, Python 2 will accept print statements with parentheses, but with potentially unexpected output. Conversely, if you try to use `print` without parentheses in Python 3, you'll get a syntax error.

```python
# using Python 3
print "please print me"
```
Output:
```
  File "<stdin>", line 1
    print "please print me"
                          ^
SyntaxError: Missing parentheses in call to 'print'
```

## Prompting for input

In Python 3, you've used the `input` function to prompt for user input.

```python
# using Python 3
name = input("What's your name? ") # name is a string
age = input("How old are you? ") # age is a string
```

In Python 2, to get the same behavior we have to use the `raw_input` function.

```python
# using Python 2
name = raw_input("What's your name? ") # name is a string
age = raw_input("How old are you? ") # age is a string
```

`input` is available as a function in Python 2, but it behaves differently than in Python 3. For more details, see the article linked above.
