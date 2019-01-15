# Python Turtles

This cheatsheet is a condensed and more accessible version of the official documentation for [Python's `turtle` module](https://docs.python.org/3.5/library/turtle.html) (for Python 3).

## The `turtle` Module

### Importing the Module

The import must always be the first line of your program.

```python
import turtle
```

### Creating a Turtle

Create a new turtle and store it in a variable (using whichever name you choose):

```python
import turtle

jim = turtle.Turtle()
```

It is also possible to create multiple turtles within the same program:

```python
import turtle

jim = turtle.Turtle()
jane = turtle.Turtle()
```

> NOTE: The rest of the examples shown below assume that you have imported and created a turtle named `speedy`

## Moving a Turtle

Here are common commands for moving a turtle around a screen. While the examples give use specific values, you can use any other value that makes sense.

Command | Value/Parameter | Example | Description
--- | --- | --- | ---
`forward` | # of units | `speedy.forward(50)` | Move forward 50 units
`backward` | # of units to move | `speedy.backward(50)` | Move backward 50 units
`left` | # degrees to turn | `speedy.left(45)` | Turn left 45 degrees
`right` | # degrees to turn | `speedy.right(45)` | Turn right 45 degrees
`goto` | coordinates of destination | `speedy.goto(25, 50)` | Move to the point (25, 50)
`speed` | `1...10` | `speedy.speed(5)` | Sets the speed of the turtle's motion to 5. The speed can be 1 through 10, with 1 being the slowest and 10 the fastest. 

## Styling a Turtle Drawing

Here are common commands for controlling how a turtles uses its pen to draw images. While the examples give use specific values, you can use any other value that makes sense.

Command | Value/Parameter | Example | Description
--- | --- | --- | ---
`up` | (none) | `speedy.up()` | Pick up the turtle's pen, do it doesn't draw a line as the turtle moves
`down` | (none) | `speedy.down()` | Put down the turtle's pen, so it draws a line as the turtle moves
`pencolor` | color name | `speedy.pencolor("blue")` | Use a blue pen. See below for valid colors.
`fillcolor` | color name | `speedy.fillcolor("red")` | Fill the shape with the color red. See below for valid colors.
`begin_fill` | (none) | `speedy.begin_fill()` | Marks the current position as the beginning of a shape that is to be filled with the fill color.
`end_fill` | (none) | `speedy.end_fill()` | Marks the current position as the end of a shape that is to be filled with the fill color. When you use this command, the shape will actually be filled.
`dot` | (none) | `speedy.dot()` | Marks a dot shape at the current position, in the pen's color.
`shape` | shape name | `speedy.shape("turtle")` | Change the shape of the turtle on the screen to the given shape. Valid values are "arrow", "turtle", "circle", "square", "triangle", "classic".
`stamp` | (none) | `speedy.stamp()` | Mark a shaped "stamp" at the current position. This works like `speedy.dot()` except that it uses the shape of the turtle set using the `shape` command.

> Valid color names include those named on [this page](https://www.tcl.tk/man/tcl8.4/TkCmd/colors.htm). Note that the color names ending in a number (e.g. "azure1") do *not* work. If you use an invalid color, you will either receive an error message, or the color will be set to black, depending on the situation.
