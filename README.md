# Parless
Parless is a DrRacket plugin that implements Adaptive Code Visualization, first described in this [blog post](https://benhsz.github.io/my-answer-to-the-parenthesis-problem/). 

The goal is to improve the user experience of Lisp-like programming languages, such as Racket, Scheme, Common Lisp, etc.

## Work In Progress
No working implementation as of yet, although the [implementation plan](steps-to-implement.md) describes the steps necessary to implement this feature. Steps completed: 0/7

## What Does It Do?
First, the non-features: 

* No significant indentation
* No syntax changes of any kind

That means the programmer will type just as many opening and closing parentheses as before. It should therefore be __compatible with all existing Lisp code__.

As for what it does, it makes code look nicer by having the editor variously visualize parentheses as:

1. Whitespace character
2. Padding character
3. Unclosed expression character

Which shape goes where depends on the code structure and formatting.

For instance, this code:

```racket
(define (factorial n)
  (if (zero? n)
      1
      (* n (factorial (sub1 n)))))
```
Would appear as:

![example code](https://benhsz.github.io/images/parless/parless.png)

But how do you know that `if` expression is parenthesized when it could be falsely indented? 

The same example, but with one closing parenthesis missing:

```racket
▪define (factorial n)
   if ▹zero? n
      1
      ▹* n (factorial (sub1 n)
```
Four missing closing parentheses:

```racket
▪define (factorial n)
  ▪if ▹zero? n
      1
      ▪* n ▪factorial (sub1 n)
```
Or with the right amount of parentheses but false indentation:

```racket
  →   define (factorial n)
    → if ▹zero? n
      1
      ▹* n (factorial (sub1 n)
```

What allows the parentheses to be visually omitted without introducing ambiguity to the code is the availability of these visual cues. These will visualize when something's amiss (missing parentheses for instance). If everything is in order (balanced parentheses, correctly indented) these cues will not visualize, and so the code will look clean and unambiguous.

![example code](https://benhsz.github.io/images/parless/parless.png)

Of course, with a decent amount of parentheses being outright invisible, it could be a bit awkward if the user wishes to manually select and edit code as text with the cursor. For that reason, parentheses will re-appear with the presence of a cursor, be it text or mouse cursor. Seen here in this animated mockup:

![animation](https://benhsz.github.io/images/parless/mouse-over.gif)

Examples are in Racket but this should work for all Lisps. A larger example in Common Lisp (source from http://norvig.com/python-lisp.html)

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
  
More details and examples are in the [implementation plan](steps-to-implement.md) and the above linked blog post.

Another way to think about what this is, is to think of a file explorer. A file explorer shows files and provides different views, such as list or thumbnail view. Changing the view doesn't actually change the files. In this case, you have an editor and code. This plugin is to __provide the editor with another view of the code__, so the code doesn't actually change. It's similar to syntax highlighting in that regard.

## License
[MIT License](LICENSE)
