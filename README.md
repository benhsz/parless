# Parsvelte
Parsvelte is a DrRacket plugin that implements Adaptive Code Visualization, first loosely described in this [blog post](https://benhsz.github.io/my-answer-to-the-parenthesis-problem/). 

The goal is to improve the user experience of Lisp-like programming languages, such as Racket, Scheme, Common Lisp, etc.

## What Does It Do?
First, what it doesn't do: 

It makes __absolutely no changes to the syntax__ and therefore has __no significant indentation__.  

The programmer will type just as many opening and closing parentheses as before.  

As for what it does, it makes code look nicer (svelte parentheses).

An example of code and a mockup below it that shows the result.

```racket
(define (factorial n)
  (if (zero? n)
      1
      (* n (factorial (sub1 n)))))
```

![example code](https://benhsz.github.io/images/parsvelte/parsvelte.png)

An animated mockup, showing some interaction with the cursor:

![animation](https://benhsz.github.io/images/parsvelte/mouse-over.gif)

Another, larger [mockup](https://benhsz.github.io/images/parsvelte/repeat-pasta.png) 
([source](https://benchmarksgame-team.pages.debian.net/benchmarksgame/program/fasta-racket-3.html)).

More details and examples are in the [implementation plan](steps-to-implement.md) and the above linked blog post.

One way to think about what this is, is to think of a file explorer. A file explorer shows files and provides different views, such as list or thumbnail view. Changing the view doesn't actually change the files. In this case, you have an editor and code. This plugin is to provide the editor with a certain view of the code. Changing the view doesn't change the code.

Therefore, it should be compatible with all existing Lisp code.

## Work In Progress
Work on this hasn't begun yet, although the [implementation plan](steps-to-implement.md) describes the steps necessary to implement this feature.  

Steps completed: 0/7

## License
[MIT License](LICENSE)
