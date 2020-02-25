# :art: Notes on emacs-lisp

 Personal notes on [Programming in Emacs
 Lisp](https://www.gnu.org/software/emacs/manual/html_node/eintr/index.html#Top)

## 1. List Processing
### 1.1. Lisp Lists
 + Both data and programs are represented the same way; that is, they
   are both lists of words, numbers, or other lists, separated by
   *whitespace* and surrounded by *parentheses*.
 + What we have been calling words are called atoms. In a list, atoms
   are separated from each other by whitespace. They can be right next
   to a parenthesis.
   + The amount of whitespace in a list does not matter.
 + `()` is called the empty list. (considered both an atom and a list
   at the same time)
 + The printed representation of both atoms and lists are called
   *symbolic expressions*, *s-expressions*, *sexp*.

### 1.2. Run a Program
 + A list in Lisp - any list - is a program ready to run.
   1. Do nothing except return to you the list itself
   2. Send you an error message
   3. Treat the first symbol in the list as a command to do something
 + The single apostrophe \` is called a quote. When quote precedes a
   list, it tells Lisp to do nothing with the list, other than take it
   as it is written.
 + `C-x C-e`: shortcut for `eval-last-sexp`
