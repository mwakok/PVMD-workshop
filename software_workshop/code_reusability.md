---
marp: true
title: "Code Reusability"
paginate: false
theme: custom
backgroundColor: #fff

math: mathjax

---

# II. Code reusability

### Some questions
- What does "modular code development" mean for you?
- What best practices can you recommend to arrive at well structured, reusable code in your favourite programming language?
- What do you know now about programming that you wish somebody told you earlier?

---

### Additional questions
- Do you design a new code project on paper before coding? Discuss pros and cons.
- Do you build your code top-down or bottom-up? Discuss pros and cons.
- Would you prefer your code to be 2x slower if it was easier to read it?

![bg left:40% width:400](img/complex-machine.jpg)

<!-- --- -->

<!-- ![bg left: width:400](https://imgs.xkcd.com/comics/good_code.png) -->

---

# [The tar pit](https://github.com/papers-we-love/papers-we-love/blob/master/design/out-of-the-tar-pit.pdf)

- Over time, software tends to become harder and harder to reason about
- Small changes become harder to implement
- Bugs start appearing in unexpected places
- More time is spent debugging than developing
- Complexity strangles development because it does not scale well
  
_Do you recognize this? Can you give an example?_ 

---

![width:650](img/development-speed.svg)
_Do you agree and what do you think "properly" means?_

---

# Modularity and composition

- Build complex behaviour from simple components
- We can reason about the components and the composite
- Composition is key to managing complexity
- Modularity does not imply simplicity, but is enabled by it

![width:800](img/knit_vs_lego.jpg)

---

# Code reusability: some guidelines

- Separate code and data: data is specific, code need not be
  - consider using a config file for project-specific (meta)data in a interoperable datatype (`.json`, `yaml`)
  - use relative paths
  - but DO hard-code unchanging variables, e.g. `gravity = 9.80665`, **once**.

---

# Code reusability: some guidelines

- Do One Thing (and do it well)
  - One function for one purpose
  - One class for one purpose
  - One script for one purpose (no copy-pasting to recycle it!)

---

# Code reusability: some guidelines

- Don't Repeat Yourself (DRY): use functions
  - Write routines in functions, i.e., code you reuse often
  - Identify potential functions by action: functions perform tasks (e.g. sorting, plotting, saving a file, transform data...)
  - Single source of truth reduces maintenance and makes debugging easier

_Note, in MATLAB a function can only be called from a script if it bears it's name_

---

# Purity
- Pure functions have no notion of state, i.e. they take input values and return values. 
- Given the same input, a pure function _always_ returns the same value

### Examples
- Mathematical functions
  $f(x, y) = x - x^2 + x^3 + y^2 + xy$

- Unix shell
  ```bash
    cat somefile | grep somestring | sort | uniq | ..
  ```

---

# <!-- fit --> Pure functions are easy to
- Test (_we will address testing later_)
- Understand
- Reuse
- Simplify
- Parallelize
- Optimize
- Compose

![bg left:50% width:600](img/Pure-functions.png)

---

# Example: pure vs. stateful

### Stateful code
```python
f_to_c_offset = 32.0
f_to_c_factor = 0.555555555
temp_c = 0.0

def fahrenheit_to_celsius_bad(temp_f):
    global temp_c
    temp_c = (temp_f - f_to_c_offset) * f_to_c_factor

fahrenheit_to_celsius_bad(temp_f=100.0)
print(temp_c)
```

---

### Pure - no side effects
```python
def fahrenheit_to_celsius(temp_f):
    temp_c = (temp_f - 32.0) * (5.0/9.0)
    return temp_c
temp_c = fahrenheit_to_celsius(temp_f=100.0)
print(temp_c)
```
---

# Recommendations
- I/O is impure (reading/writing)
- Keep I/O on the outside and connected
- Keep the inside of your code pure/stateless

![width:800](img/good-vs-bad.svg)

---

Functions do not necessarily make code shorter (at first)! Compare:

```python
indexATG = [n for n,i in enumerate(myList) if i == 'ATG']
indexAAG = [n for n,i in enumerate(myList) if i == 'AAG']
```
with

```python
def indexString(inputList: List, z: str) -> List:
  """Find index of string in list
  """
	zIndex = [n for n,i in enumerate(li) if i == z]
	return zIndex
	
indexATG = indexString(myList,'ATG')
indexAAG = indexString(myList,'AAG')
```

But, you can now reuse your function elsewhere, made it general (**one instance!**), and have added additional documentation (function name, docstring, typehinting)

---

# Divide and conquer

### How to approach your code
- Split up the code into functional parts
- Construct your program from these parts
    - functions
    - modules
    - packages (Python) or libraries (C or C++ or Fortran)

---

# Your turn: visualize your code!

Choose:
- Make a screenshot, process it in paint, powerpoint, or your favorite editor;
- Copy paste your code to a text editor, and use markers.

**The objective is for you to 'see' your code!**

- Yellow denotes scripted, unstructured code _(basic, sequential lines of instructions)_
- Purple denotes functions or other structured code _(e.g. for-loops, conditionals, etc.)_
- Green denotes comments (or comment blocks)

Again, make notes in your code (`#TODO`!) if you see:
- **Scripted code**: this can be a function
- **Structured code**: this should be re-structured 


---

# <!-- fit --> How does your code look?

![height:440](img/tetris.svg)  ![bg right:50% height:500](img/tetris_help.svg)

---

# Functions, functions, functions
- Build your code from functions
- Break your code down to more functions
    - if you have too many levels of indentation
    - if a function gets too long
    - if a function does more than one thing
    - if you find it hard to name a function
    - if you find it hard to write tests for a function

---

# <!-- fit --> Simplicity and clarity before elegance before efficiency

**Avoid premature optimization**
- Do not optimize
- If you have to optimize, do not optimize
- If you have to optimize, measure, do not guess

**Simple is better than complex**

>Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it. (Brian W. Kernighan)
---

![bg left: width:1000px](img/god_knows.jpg)

---

# Your turn: make a function

You have visualized your code. Use your findings to improve it!

- **Preferably**: take scripted code and turn it into a function, _or_ split an existing function into two or more functions, _or_ generalize multiple similar functions (DRY!)

- If there is no function to work on: try and address the readability of your code.

_However_: for future exercises you will need at least one function, preferably with parameters, in your code! 

---

# Recommendations
- Divide and isolate
- Compose your code out of pure functions
- Keep it simple
- Prefer immutable data structures (_immutable_ means that the data value cannot be modified)
- Think about the people reading your code, do not ["Write Unmaintainable Code"](https://www.doc.ic.ac.uk/~susan/475/unmain.html) 
