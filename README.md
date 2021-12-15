# Parsvelte
Parsvelte is the name of the DrRacket plugin that aims to implement Adaptive Code Visualization, first loosely described in this [blog post](https://benhsz.github.io/my-answer-to-the-parenthesis-problem/). 

The purpose of this feature is to improve the user experience of Lisp-like programming languages, such as Racket, Scheme, Common Lisp, etc.

## What Does It Do?
First, what it does *not* do: it makes __absolutely no changes to the syntax.__  

The programmer will type just as many opening and closing parentheses as before.  

As for what it does, it makes code look different. Here's an example:

![example code](https://benhsz.github.io/images/lbp/lbp.png)

More details and examples in the [implementation plan](steps-to-implement.md) and the above linked blog post.

One way to think about what this is, is to think of a file explorer. A file explorer shows files and provides different views, such as list or thumbnail view. Changing the view doesn't actually change the files. In this case, you have an editor and code. This plugin is to provide the editor with a certain view of the code. Changing the view doesn't change the code.

## Work In Progress
Work on this hasn't begun yet, although the [implementation plan](steps-to-implement.md) describes the steps necessary to implement this feature.  

Steps completed: 0/7

## License
[MIT License](LICENSE)
