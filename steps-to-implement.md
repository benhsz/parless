# Step 1

Give opening parentheses with no closing counterparts a different appearance.

Decide on an appropriate shape, such as `●, ▯, ▮, ◖` or `┃` (see: https://en.wikipedia.org/wiki/Geometric_Shapes)

The result is to have that shape show up when you type `(` in the editor.

Code would go from looking like this

    (define X

to

    ●define X

When the expression is finished, the shape will turn into `(`.

    (define X 5)
    
This means that `(` will only visualize when there's a matching closing parens. 

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

Make parentheses that satisfy the right conditions appear as ` ` (blank). Those conditions would be: if it starts at the very beginning of a line and (b) is __not__ part of a 'list-like format' and (c) is __not__ preceded by another character, such as `'`, another `(`.

For the code below that would be the ones that start `define` and `if`, but not `*` because it doesn't satisfy (b).

    (define (factorial n)
      (if (zero? n)
          1
          (* n (factorial (sub1 n)))))

Closing parentheses that pile-up at the end should also be made blank.
For the above example that would be all the closing parentheses after `(sub1 n)`

If selected, these parentheses would no longer be faded-out, including their counterparts.
For example if the very first opening parenthesis is selected from the above code, it would also cause the very last closing parenthesis to be no longer blank as well.

# Step 4

If a cursor is present on a line (either text cursor or mouse cursor), the parentheses made blank in step 3 should re-appear on that line.

So without a cursor anywhere you should see this:

     define (factorial n)
       if (zero? n)
          1
          (* n (factorial (sub1 n)
          
But then with a cursor present, for instance on the first line, it would show

    (define (factorial n)

And so on.

# Step 5

Experiment with the list-like formatting: have `(` appear as `▹` (or similar shape) and fade out its closing counterpart.
The if expression below has such a format.

So this

     define (factorial n)
       if (zero? n)
          1
          (* n (factorial (sub1 n)

would appear as

     define (factorial n)
       if ▹zero? n
          1
          ▹* n (factorial (sub1 n)

# Step 6

The next step could be to provide customization options for the user, such as being able to:
- Choose the shape for unfinished opening parens
- Adjust what the editor should consider 'unexpected indentation'
- Decide whether parentheses in commented-out code should be affected ('no' as default?)

# Step 7

Experiment further.
