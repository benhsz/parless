# Parless
Parless is a DrRacket plugin that aims to implement Adaptive Code Visualization, first described in this [blog post](https://benhsz.github.io/my-answer-to-the-parenthesis-problem/). 

The goal is to improve the user experience of Lisp-like programming languages, such as Racket, Scheme, Common Lisp, etc.

## Work In Progress
No working implementation as of yet, although the [implementation plan](steps-to-implement.md) describes the steps necessary to implement this feature.

## What Does It Do?

__Parless aims to provide an alternative view of Lisp code__. Or rather, an enhanced view of Lisp code. The idea is that the editor re-dresses parentheses. This would (hopefully) make Lisp code easier to read and write. 

The functionality is similar to syntax highlighting in that it changes the appearance of code but not the actual syntax.

More specifically, the functionality could variously visualize parentheses as:

1. 'Imbalanced parenthesis' characters
2. 'Padding' characters
3. Space characters
4. Dots

How parless goes about re-dressing parentheses depends on the formatting and structure of the code, and user configuration.

For example, visualizing parentheses here as space characters would be ambiguous:

```racket
 cons 1  cons 2  cons 3 '()
 ```
 
 But not if each expression were on its own line and indented:
 
 ```racket
  cons 1
    cons 2
      cons 3 '()
```

Then again, if each expression were on its own line and __not__ indented, parentheses would again be the more appropriate visualization:

```racket
(cons 1
(cons 2
(cons 3 '())))
```

Editors already keep track of how code is formatted and structured. Parless uses that information to dynamically provide the appropriate view.

Note that while some examples and mock-ups below may look very similar to other attemps to change Lisp code (i.e. sweet-expressions) parless does *not* involve any syntax changes such as significant indentation. This mean the programmer will input just as many parentheses as before. The alternative view parless provides should be compatible with all existing Lisp code.

## Examples & Mock-ups

For instance, this code:

```racket
(define (factorial n)
  (if (zero? n)
      1
      (* n (factorial (sub1 n)))))
```
Could be re-dressed as:

![example code](https://benhsz.github.io/images/parless/parless.png)

The same example, but with one closing parenthesis missing:

```racket
◖define (factorial n)
   if ▹zero? n
      1
      ▹* n (factorial (sub1 n)
```
Four missing closing parentheses:

```racket
◖define (factorial n)
  ◖if ▹zero? n
      1
      ◖* n ◖factorial (sub1 n)
```
Or with the right amount of parentheses but false indentation:

```racket
  →   define (factorial n)
    → if ▹zero? n
      1
      ▹* n (factorial (sub1 n)
```

What allows the parentheses to be visually omitted without introducing ambiguity is the availability of these visual cues. They will only visualize when certain conditions are met (e.g. a missing closing parenthesis). Once everything is in order (balanced parentheses, correct indentation) the code will appear clean and unambiguous.

With a decent amount of parentheses being outright invisible, it could be awkward if the user wishes to manually select and edit code as text with the cursor. One possibility is to have hidden parentheses re-appear with the presence of a mouse cursor, as in this animated mockup:

![animation](https://benhsz.github.io/images/parless/mouse-over.gif)

Parless could also provide options to configure the view. 

If the above examples were too minimalist, you could have parentheses turn into dots instead of whitespace.

```racket
·define (factorial n)
  ·if ·zero? n·
      1
      ·* n (factorial (sub1 n)····
```

A larger example in Common Lisp (source from http://norvig.com/python-lisp.html)

```lisp
 defparameter *grammar*
  '(▹sentence -> (noun-phrase verb-phrase)
     noun-phrase -> (Article Noun)
     verb-phrase -> (Verb noun-phrase)
     Article -> the a
     Noun -> man ball woman table
     Verb -> hit took saw liked)
  "A grammar for a trivial subset of English."

 defun generate (phrase)
  "Generate a random sentence or phrase"
   cond (▹listp phrase
          mappend #'generate phrase)
        (▹rewrites phrase
          generate (random-elt (rewrites phrase)
        (t (list phrase)

 defun generate-tree (phrase)
  "Generate a random sentence or phrase,
  with a complete parse tree."
   cond (▹listp phrase
          mapcar #'generate-tree phrase)
        (▹rewrites phrase
          cons phrase
               (generate-tree (random-elt (rewrites phrase)
        (t (list phrase)

 defun mappend (fn list)
  "Append the results of calling fn on each element of list.
  Like mapcon, but uses append instead of nconc."
   apply #'append (mapcar fn list)

 defun rule-rhs (rule)
  "The right hand side of a rule."
   rest (rest rule)

 defun rewrites (category)
  "Return a list of the possible rewrites for this category."
   rule-rhs (assoc category *grammar*)

 defun random-elt (choices)
  "Choose an element from a list at random."
   elt choices (random (length choices)
  ```

## License
[MIT License](LICENSE)
