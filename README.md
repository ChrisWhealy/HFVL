# HFVL

Hash Function Visualization Language

**Created:** Andrew Chabot<br>
**Advisor:** Dr. Hans-Peter Bischof

**Adapted:** Chris Whealy

A simple to use language called the Hash Function Visualization Language or HFVL.

This repository contains all necessary files for creating and running files defined in this language.

Example visualizations have also been provided for the SHA-1, SHA-2 256 and SHA-3 256 algorithms, called`SHA1`, `SHA2`, and `SHA3` respectively.
Within these files, there are link to sub-visualization files that allow you to see the internal processing steps with each algorithm.

## Usage

Within a Python 3.9 virtual environment, run `python HFVL.py`

You will see a message requesting the name of an input file.

> Enter HFVL filename to run:

Enter the name of an HFVL file and you should get a message that looks like

> file found, loading visualization for: *SHA1*


# HFVL Language Syntax

The various SHA visualizations serve as examples of how to use HFVL.
The language syntax is somewhat simplistic, but provides sufficient tools to visualise the internal workings of the different SHA algorithms.

---

## Frames

All the visual transitions occur within a basic processing unit known as a `Frame`.
A visualisation must contain at least one frame.

Every frame starts with a `Frame <frame_number>:` command and ends with the `Frame End` command.
A sample frame statement is below.

```
Frame 1:

    Frame Changes

Frame End
```

---

## Visual Functions

All mandatory argument values are positional and must be provided without specifying the argument name, but all optional argument values must be prefixed by the argument name.

### `$drawBox`

Creates a box that can contain text and can be connected to other boxes by arrows.

`x + width` must be less than the overall window width.<br>
`y + height` must be less than the overall window height.

| | Name | Value/Units | Description
|---|---|---|---|
| Command Name | `$drawBox` |
| Abbreviation | `$db` |
| Mandatory Arguments | `box_id` | text | Unique identifier for this box. This `id` will be referenced when using functions such as `$modifyBox` or `$drawArrow`
| | `x` | Int, Pixels | Distance from the left window edge to the bottom left box corner
| | `y` | Int, Pixels | Distance from the bottom window edge to the bottom left box corner
| | `width` | Int, Pixels | Box width
| | `height` | Int, Pixels | Box height
| Optional Arguments | `color=` | `blue`, `green`, `red`, `black` | `black` is the default
| | `bold=` | `True`, `False`
| | `text=` | | Either some hardcoded, quote delimited text, or a box id prefixed with an asterisk (E.G. `text=*some_box`).<br> Hardcoded text must not contain a comma or an equals sign.<br>Use a backslash character `\` for a carriage return.
| | `link=` | | Name of sub-visualisation HFVL file to open when this box is clicked on
| | `input=` | | Semicolon-separated list of argument values to pass to the sub-visualisation window

For example
```
$drawBox(box1, 100, 100, 100, 100, color=blue, bold=True, text='this is box1', link='linkfile', input=box2;box3)
```

### `$modifyBox`

Modifies a box's optional argument values.
This function cannot move or resize a box.

| | Name | Value/Units | Description
|---|---|---|---|
| Command Name | `$modifyBox` |
| Abbreviation | `$mb` |
| Mandatory Arguments | `box_id` | text | Unique identifier of the box being modified
| Optional Arguments | `color=` | `blue`, `green`, `red`, `black` | `black` is the default
| | `bold=` | `True`, `False`
| | `text=` | | Either some quote delimited text or a box id prefixed with an asterisk (E.G. `text=*some_box`).<br> Must not contain a comma or an equals sign.<br>Use a backslash character `\` for a carriage return.
| | `link=` | | Name of sub-visualisation HFVL file to open when this box is clicked on
| | `input=` | | Semicolon-separated list of argument values to pass to the sub-visualisation window

For example
```
$modifyBox(box1, color=red, bold=False, link='linkfile2', input=box2)
```

### `$resetBox`

Reverts a box to its default state.
That is:

* Links and inputs are removed (`link=, input=`)
* `bold=False`
* `color=black`
* Text will ***not*** be modified!

For example
```
$resetBox(box_id)
```

### `$drawArrow`

Draws an arrow between two boxes using a pathfind algorithm

| | Name | Value/Units | Description
|---|---|---|---|
| Command Name | `$drawArrow` |
| Abbreviation | `$da` |
| Mandatory Arguments | `start_box_id` | | Unique identifier of the start box
| | `start_box_id` | | Unique identifier of the end box
| Optional Arguments | `color=` | `blue`, `green`, `red`, `black` | `black` is the default
| | `bold=` | `True`, `False` |

For example
```
$drawArrow(start_box_id, end_box_id, color=red, bold=True)
```

### `$modifyArrow`

Modifies a arrow's optional argument values.
This function cannot relocate an arrow to point to different boxes.

| | Name | Value/Units | Description
|---|---|---|---|
| Command Name | `$modifyArrow` |
| Abbreviation | `$ma` |
| Mandatory Arguments | `start_box_id` | | Unique identifier of the start box
| | `start_box_id` | | Unique identifier of the end box
| Optional Arguments | `color=` | `blue`, `green`, `red`, `black` | `black` is the default
| | `bold=` | `True`, `False`

### `$resetArrow`

Reverts an arrow to its default state of `bold=False, color=black`

For example
```
$resetArrow(arrow_id)
```

### `$modifyTitle`

Changes the window's title.
For example:

```
$modifyTitle(text='A New Title', color='red')
```

---

## Variables

String or integer values can be stored in variables.

All variable names must begin with an underscore character `_` and must be given an initial value at declaration.

String variables should not use quotation marks.

For example:
```
_stringValue = sample text
_intValue = 0
```

---

## Calculation Functions

A variety of predefined functions are available that implement the various internal steps within the secure hash algorithms.

### Syntax Rules

* All function names start with an `@` character.
* Arguments must be seperated with a semicolon `;`

### Predifined Function

| Function Name | Description
|---|---
| `@add(bits1;bits2)`         | Returns the sum of the two input bit blocks
| `@and(bits1;bits2)`         | Returns the logical `AND` of the two input bit blocks
| `@bitbyte(bits)`            | Returns input bit string into a byte string
| `@bytebit(bytes)`           | Returns input byte string as a bit string
| `@concat(string1;string2)`  | returns the string concatenation of the input strings
| `@first272(bytes)`          | Returns the first 272 bytes of the input bytes
| `@groupAsI64(bytes)`        | Returns the input bytes as a space-delimited list of hexdecimal byte values with an extra space every 8 bytes
| `@last128(bytes)`           | Returns the last 128 bytes of the input bytes
| `@last384(bytes)`           | Returns the last 384 bytes of the input bytes
| `@lbitshift5(bits)`         | Shifts input bits left by 5
| `@lbitshift30(bits)`        | Shifts input bits left by 30
| `@lt(num1;num2)`            | Returns `True` if `num1` is less than `num2`, else returns `False`
| `@mod(num1;num2)`           | Returns the first input number modulo the second input number
| `@mod32(bits)`              | Returns the input bits modulo `2^32`
| `@not(bits)`                | Returns the logical `NOT` of the input bits
| `@or(bits1;bits2)`          | Returns the logical `OR` of the two input bit blocks
| `@rbitshift(bits;shift_by)` | Shifts input bits right by input amount
| `@rbitshift2(bits)`         | Shifts input bits right by 2
| `@rbitshift6(bits)`         | Shifts input bits right by 6
| `@rbitshift11(bits)`        | Shifts input bits right by 11
| `@rbitshift13(bits)`        | Shifts input bits right by 13
| `@rbitshift22(bits)`        | Shifts input bits right by 22
| `@rbitshift25(bits)`        | Shifts input bits right by 25
| `@trunc32(bytes)`           | Returns the first 32 bytes of the input bytes
| `@xor(bits1;bits2)`         | Returns the logical `XOR` of the two input bit blocks
| `@xorloop(a_block;d_block)` | Returns the output of the `XORLoop` part of the `Keccak-f[1600]` `Theta` function
| `@kf1600(capacity;rate)`    | Returns the output of the `Keccak-f[1600]` function against the input capacity and rate
| `@cfunc(bytes)`             | Returns the output of the `C` function part of the `Keccak-f[1600]` `Theta` function
| `@dfunc(bytes)`             | Returns the output of the `D` function part of the `Keccak-f[1600]` `Theta` function
| `@theta(bytes)`             | Returns the output of the `Keccak-f[1600]` `Theta` function
| `@rho(bytes)`               | Returns the output of the `Keccak-f[1600]` `Rho` function
| `@pi(bytes)`                | Returns the output of the `Keccak-f[1600]` `Pi` function
| `@chi(bytes)`               | Returns the output of the `Keccak-f[1600]` `Chi` function
| `@iota(bytes)`              | Returns the output of the `Keccak-f[1600]` `Chi` function
| `@indexarr(array;index)`    | Converts the input string to an array and returns the value at the given index
| `@indexmat(matrix;row;col)` | Converts the input string to an matrix and returns the value at the given row and column of the matrix
| `@+(num1;num2)`             | Returns the sum of the two input numbers
| `@-(num1;num2)`             | Returns the difference of the two input numbers
| `@*(num1;num2)`             | Returns the product of the two input numbers

These functions can be nested within each other, an example of them being nested in SHA-1 is below for an F function:

```
$modifyBox(f,text=@bitbyte(@or(@or(@bytebit(*w2);@bytebit(*w3));@and(@not(@bytebit(*w2));@bytebit(*w4)))))
```


## Flow Control

### Branching

The `if` keyword must be followed by a function that returns a boolean.
Currently, only the `@lt` function is available for this purpose.

`elif` can be used to chain additional `if` statements and `else` can also be used.

All if statements must end with an `if end` line.

```
if @lt(_roundCount;4):
    $mb(box1,color=blue)
elif @lt(_roundCount;8):
    $mb(box1,color=green)
else:
    $mb(box1,color=red)
if end
```

### Looping

The `while` keyword starts a loop and must be followed by a function that returns a boolean.
The loop will execute zero or more times and will repeat until the function returns `False`.
Currently, only the `@lt` function is available this purpose.

While loops must end with a `while end` line.

```
while @lt(_roundNum;25):
    _roundNum = @+(_roundNum;1)
while end
```
