# Parless
Parless is a DrRacket plugin that implements Adaptive Code Visualization, first described in this [blog post](https://benhsz.github.io/my-answer-to-the-parenthesis-problem/). 

The goal is to improve the user experience of Lisp-like programming languages, such as Racket, Scheme, Common Lisp, etc.

## Work In Progress
No working implementation as of yet, although the [implementation plan](steps-to-implement.md) describes the steps necessary to implement this feature. Steps completed: 0/7

## What Does It Do?
First, the non-features: 

* No change to the syntax
* No significant indentation

That means the programmer will type just as many opening and closing parentheses as before. It should therefore be __compatible with all existing Lisp code__.

As for what it does, it makes code look nicer by having the editor variously visualize `()` as ` ` (blank), `●`, `▹`,  and `()`. Which shape goes where depends on the formatting. (The shapes can be adjusted according to the user's preferences, but these are the ones used in the examples.)

For instance, this code:

```racket
(define (factorial n)
  (if (zero? n)
      1
      (* n (factorial (sub1 n)))))
```
Would appear as:

![example code](https://benhsz.github.io/images/parless/parless.png)

An animated mockup showing interaction with the cursor:

![animation](https://benhsz.github.io/images/parless/mouse-over.gif)

If the running example is missing one closing parenthesis:

```racket
●define (factorial n)
   if ▹zero? n
      1
      ▹* n (factorial (sub1 n)
```
Four missing closing parentheses:

```racket
●define (factorial n)
  ●if ▹zero? n
      1
      ●* n ●factorial (sub1 n)
```
Or has the right amount of parentheses but is not indented correctly:

```racket
 define (factorial n)
    ← if ▹zero? n
      1
      ▹* n (factorial (sub1 n)
```
Larger example in Common Lisp (source from http://norvig.com/python-lisp.html)

```lisp
 defparameter *grammar*
  '(▹sentence -> (noun-phrase verb-phrase)
    ▹noun-phrase -> (Article Noun)
    ▹verb-phrase -> (Verb noun-phrase)
    ▹Article -> the a
    ▹Noun -> man ball woman table
    ▹Verb -> hit took saw liked)
  "A grammar for a trivial subset of English."

 defun generate (phrase)
  "Generate a random sentence or phrase"
   cond (▹listp phrase
         ▹mappend #'generate phrase)
        (▹rewrites phrase
         ▹generate (random-elt (rewrites phrase)
        (t (list phrase)

 defun generate-tree (phrase)
  "Generate a random sentence or phrase,
  with a complete parse tree."
   cond (▹listp phrase
         ▹mapcar #'generate-tree phrase)
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
  
More details and examples are in the [implementation plan](steps-to-implement.md) and the above linked blog post.

Another way to think about what this is, is to think of a file explorer. A file explorer shows files and provides different views, such as list or thumbnail view. Changing the view doesn't actually change the files. In this case, you have an editor and code. This plugin is to __provide the editor with another view of the code__. Changing the view doesn't change the code. Ultimately, this alternative view is nothing too radical, it's something of an overlay that hopefully makes the code more understandable.

## License
[MIT License](LICENSE)
