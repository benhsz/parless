The full functionality involves two fundamental layers. The first is something like a code-in-progress layer that renders imbalanced parentheses as alternative characters. The second layer is something of an indentation layer to show atypical indentation. It's assumed code is indented as normal but this second layer is needed for the cleanest view that can be achieved with parless.

Some kind of user configuration will also be needed eventually, the same way editors let you choose a color scheme for syntax highlighting.

Given that the first layer is always used it seems look a good place to start there.

# Step 1

Visualize imbalanced parentheses as alternative characters.

Decide on an appropriate shape, such as `◖` and `◗`. (see: https://en.wikipedia.org/wiki/Geometric_Shapes)

From

    (define X

to

    ◖define X

When the expression is finished, the shape will turn into `(`.

    (define X 5)
    
This means that `(` will only visualize when there's a matching closing parens. Same goes for ')'.

It may not be necessary to have distinct shapes for opening and closing parens, so you could one shape for both opening and closing parens.

So that `(...` would look like `●...` and `(...))` would look like `(...)●`.

So it may not even be necessary to distinguish whether that imbalanced parens is opening or closing, as long as it stands out.

Note: an exception may have to be made for parens when they are part of a string i.e. `"("` should show `"("` not `"◖"`.

# Step 3

Provide visual cues to point out atypical indentation.
It could be as simple as just having arrows to point it out, such as →

For example

    (define X 1)
      (define Y 2)

could appear as

    (define X 1)
    → (define Y 2)
    
The arrow would then say something like 'this has been indented to there when it would normally be here'.

And this

    (define X 1)
     define Y 2

could appear as

    (define X 1)
    →define Y 2


# Step 4

Make parentheses that satisfy the right conditions appear as ` ` (whitespace character). If these conditions are not satisfied, `()` will remain as `()`.

For expressions formatted on a single-line, the conditions are:
1. `(` and `)` are respectively the first and last characters on the line

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
          
The last example in step 2 was this:

    (define X 5)
    ←define Y 4

After having completed step 3, it would look like this:

     define X 5 
    ←define Y 4

# Step 5

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

# Step 6

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

# Step 7

The next step could be to provide customization options for the user, such as being able to:
- Choose the shape for unfinished opening parens
- Adjust what the editor should consider 'unexpected indentation' and what the cues should look like
- Decide whether parentheses in commented-out code should be affected ('no' as default?)

# Step 8

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
