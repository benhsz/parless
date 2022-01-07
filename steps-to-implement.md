Completing steps 1 through 4 is likely enough to get most of the benefits of parless.

Finishing the steps beyond that is primarily to tidy things up a bit.  

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
Initial step could be to just use simple arrow shapes, such as →

It might also be best to start out with a 'strict' version of indentation, i.e. any code that deviates
from what DrRacket considers proper indentation would show an arrow.

Also decide what exactly the cues are supposed to point out.

For example

    (define X 5)
      (define Y 4)

would appear as

    (define X 5)
    → (define Y 4)
    
Then it would say something like 'this has been indented too far'.

Where as if it were to show an arrow like this

    (define X 5)
    ← (define Y 4)
    
Then it would say 'this belongs over there', and the presence of the arrow itself is enough to let the programmer know it's been indented correctly. 

The latter case might be preferable for being more informative.

Another example

    (define X 5)
     define Y 4

This should appear as

    (define X 5)
    ←define Y 4


# Step 3

Make parentheses that satisfy the right conditions appear as ` ` (blank). If these conditions are not satisfied, `()` will remain as `()`.

For expressions formatted on a single-line, the conditions are:
1. `(` and `)` are respectively the first and last characters on the line
2. It is not part of another expression

For expressions formatted on more than one line, the conditions are:
1. `(` is the first character on the line
2. Expressions within it are indented

With this code

    (define (factorial n)
      (if (zero? n)
          1
          (* n (factorial (sub1 n)))))
 
 The result would be
 
     define (factorial n)
       if (zero? n)
          1
          (* n (factorial (sub1 n)))))

Any closing parentheses after the last expression should also be made blank.
For the above example that would be all closing parentheses after `(sub1 n)`.

The result would then be

     define (factorial n)
       if (zero? n)
          1
          (* n (factorial (sub1 n)

# Step 4

If a cursor is present on a line (either text cursor or mouse cursor), the parentheses made blank in step 3 should re-appear, slightly faded-out, on that line.

So without a cursor anywhere you should see this:

     define (factorial n)
       if (zero? n)
          1
          (* n (factorial (sub1 n)
          
But then with a cursor present, for instance on the first line, it would show

    (define (factorial n)

And so on.

If either `(` or `)` is selected, that parens as well as its matching parens would become fully visible.
For example if the very first opening parenthesis is selected from the above code, it would also cause the very last closing parenthesis to be no longer blank as well.

# Step 5

Experiment with the list-like formatting: have `(` appear as another shape, such as `▹` or `|` and have its closing paren appear blank. 

The conditions are:
1. It is part of another expression
2. It is formatted on a one line

After that, it should satisfy either the first set of conditions or the second set. 

1. `(` is not the first character on the line
2. The first character immediately before `(` is a space character or an opening parenthesis
3. The first character immediately after `(` is __not__ another opening parenthesis
4. One or more expression below it is indented to match its formatting

Or...

1. `(` is the first character on the line
2. It is formatted below an expression that satisfies the previous conditions

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
- Adjust what the editor should consider 'unexpected indentation' and what the cues should look like
- Decide whether parentheses in commented-out code should be affected ('no' as default?)

# Step 7

Experiment further.

Nested pairs of expressions could be an interesting place to start.

For instance

    (let loop-i ([i 0] [px 0.0] [py 0.0] [pz 0.0])
      ...

Could be rendered as

     let loop-i ( i 0 · px 0.0 · py 0.0 · pz 0.0 )
      ...

Also, if making pile-ups of closing parens at the end of the line blank, it could look a bit odd if it's preceded by an expression that does have a pile-up.

For example

     one (two (three (four))) (two (three (four)
    
If it does look too strange, it might be best to just visually match the closing parens.

     one (two (three (four))) (two (three (four)))

Another possibility to avoid that visual imbalance could be to fade out the first `))` rather than make them blank.

     one (two (three (four).. (two (three (four)
     
With `..` being where the faded-out `))` should be.
