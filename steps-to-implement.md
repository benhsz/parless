# Step 1

Give opening parentheses with no closing counterparts a different appearance.

Decide on an appropriate shape, such as `▯, ▮, ◖ or ┃` (see: https://en.wikipedia.org/wiki/Geometric_Shapes)

The result is to have that shape show up when you type `(` in the editor.

Code would go from looking like this

    (define X

to

    ▯define X

When the expression is finished, the shape will turn into `(`.

    (define X 5)

# Step 2

Provide visual cues to point out unexpected indentation.
Decide what 'unexpected' indentation is, and decide on the kind of cues.
Initial step could be to just use one arrow shape, such as →

It might also be best to start out with a 'strict' version of indentation, i.e. any code that deviates
from what DrRacket considers proper indentation would show an arrow.

For example

    (define X 5)
      (define Y 4)

would appear as

    (define X 5)
    → (define Y 4)

and

    (define X 5)
     define Y 4

would appear as

    (define X 5)
    →define Y 4


# Step 3

Fade out (to a certain degree) parentheses that (a) start at the very beginning of a line and (b) are not part of a 'list-like format'.

For the code below that would be the ones that start `define` and `if`, but not `*` because it's part of that format.

    (define (factorial n)
      (if (zero? n)
          1
          (* n (factorial (sub1 n)))))

Closing parentheses that pile-up should also be faded out.
For the above example that would be all the closing parentheses after `(sub1 n)`

If selected, these parentheses would no longer be faded-out, including their counterparts.
For example if the very first opening parenthesis is selected from the above code, it would also cause the very last closing parenthesis to be no longer faded-out as well.

# Step 4

If no cursor is present (either text cursor or mouse cursor), fade out the appropriate parentheses *completely*, thus rendering them invisible. 

That means with no cursor present on any of the following lines of code, the running example would look like this

     define (factorial n)
       if (zero? n)
          1
          (* n (factorial (sub1 n)

If a cursor is present on the last line above, it would cause every closing parentheses to reappear on that line. If a cursor were present on the very first line, it would show the opening parenthesis of the define expression, etc.

# Step 5

Experiment with the list-like formatting: have `(` appear as `|` and fade out its closing counterpart.
The if expression below has such a format.

So this

     define (factorial n)
       if (zero? n)
          1
          (* n (factorial (sub1 n)

would appear as

     define (factorial n)
       if |zero? n
          1
          |* n (factorial (sub1 n)

Consider an alternative shape if showing a `|` bar would be confusing.

# Step 6

The next step could be to provide customization options for the user, such as being able to:
- Adjust the shape for unfinished opening parens
- Modify what the editor should consider 'unexpected indentation'
- Decide whether parentheses in commented-out code should be affected ('no' as default?)

# Step 7

Experiment further.
