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

### 1.3. Generate an Error Message
 + Read the `*Backtrace*` buffer from the bottom up.

### 1.4. Symbol Names and Function Definitions
 + A symbol is not itself the set of instructions for the computer to
   carry out. Instead, the symbol is used as a way of locating the
   definition or set of instructions.
 + One set of instructions can be attached to several names.
 + A symbol can have only one function definition attached to it at a
   time.

### 1.5. The Lisp Interpreter
 + What the Lisp interpreter does when we command it to evaluate a
   list:
   + If there is a **quote** before the list, the interpreter just
     gives us the list.
   + If there is no quote, the interpreter looks at the first element
     in the list and sees whether it has a **function definition**. If
     it does, the interpreter carries outt he instructions in the
     function definition.
   + Otherwise, the interpreter prints an **error message**.
#### Complications
 + The Listp interpreter can evaluate a symbol that is not quoted and
   does not have parentheses around it. It will attempt to determine
   the symbol's value as a **variable**.
 + Some functions are unusual and do not work in the usual manner,
   which called **special forms**. e.g. defining a function, ...
 + A **macro** is a construct defined in Lisp, which differs from a
   function in that it translates a Lisp expression into another
   expression that is to be evaluated in place of the original
   expression.
 + If the function that the List interpreter is looking at is not a
   special form, and if it is part of a list, the interpreter looks to
   see whether the list has a *list inside of it*. It always works on
   the **innermost list first**.

### 1.6. Evaluation
 + Most frequently, the interpreter **returns a value**.
 + The other kind of action is called **side effect**.

### 1.7. Variables
 + `fill-column` is evaluated as value.
 + `(fill-column)` generates an error message `(void-function
   fill-column)`
 + `(+ 2 2)` is evaluated as `4`.
 + `+` generates an error message `(void-variable +)`

### 1.8. Arguments
 > According to the Oxford English Dictionary, the word "argument"
 > derives from the Latin for `to make clear, prove`; thus it came to
 > mean "the evidence offered as proof", which is to say, "the
 > information offered", which led to its meaning in Lisp.

 + Lisp counts everything between the two quotation marks as part of
   the string, including the spaces.

#### 1.8.2 An Argument as the Value of a Variable or List
 + An argument can be a symbol that returns a value when it is
   evaluated: `(+ 2 fill-column)`
 + An argument can be a list that returns a value when it is
   evaluated: `(concat "The " (number-to-string (+ 2 fill-column)) "
   foxes.")`

#### 1.8.3 Variable Number of Arguments
 + Some functions take any number of arguments. Just like initial
   default value of `fold_left`.

#### 1.8.4 Using the Wrong Type Object as an Argument
 The `p` for `number-or-marker-p` is the embodiment of a practice
 started in the early days of Lisp programming. The `p` stands for
 "predicate". In the jargon used by the early Lisp researchers, a
 predicate refers to a function to determine whether some property is
 **true or false**. So the `p` tells us that `number-or-marker-p` is
 the name of a function that determines whether it is true or false
 that the argument supplied is a number or a marker. Other Lisp
 symbols that end in `p` include `zerop`, a function that tests
 whether its argument has the value of zero, and `listp`, a function
 that tests whether its argument is a list.

#### 1.8.5 The `message` Function
 + `message` function have format string just like in C/C++.

### 1.9. Setting the Value of a Variable
#### 1.9.1 Using `set`
 + Evaluating the following command sets the value of the symbol
   `flowers` to the list `'(rose violet daisy buttercup)`.

``` emacs-lisp
(set 'flowers '(rose violet daisy buttercup))
```

 + `set` function *returns* the list `(rose violet daisy buttercup)`
   itself.
 + As a **side efect**, the symbol `flowers` is bound to the list.
 + Every Lisp function must return a value if it does not get an
   error.
 + When you use `set`, you need to quote both arguments to `set`,
   unless you want them *evaluated*. Since we do not want either
   argument evaluted, neither the variable `flowers` nor the list
   `(rose violet daisy buttercup)`, both are quoted.

#### 1.9.2. Using `setq`
 + The combination of `set` and a quoted first argument is so common
   that it has its own name: the special form `setq`.

``` emacs-lisp
(setq flowers '(rose violet daisy buttercup))
```

 + Also, as an added convenience, `setq` permits you to set several
   different variables to different values, all in one expression.
   + The first argument is bound to the value of the second argument,
     the third argument is bound to the value of the fourth argument,
     and so on.

``` emacs-lisp
(setq trees '(pine fir oak maple)
      herbivores '(gazelle antelope zebra))
```

 + `set` and `setq` make thet symbole *point* to the list.

#### 1.9.3 Counting

``` emacs-lisp
(setq counter 0)                ; initializer
(setq counter (+ counter 1))    ; incrementer
counter                         ; counter
```

 + When you evaluate the incrementer, the Lisp interpreter first
   evaluates the innermost list; this is the addition.

### 1.10 Summary
 + Lisp programs are made up of expressions, which are lists or single
   atoms.
 + Lists are made up of zero or more atoms or inner lists, separated
   by whitespace and surrounded by parentheses. A list can be empty.
 + Atoms are multi-character symbols, like `forward-paragraph`, single
   character symbols like `+`, strings of characters between double
   quotation marks, or numbers.
 + A number evaluates to itself.
 + A string between double quotes also evjaluates to itself.
 + When you evaluate a symbol by itself, its value is returned.
 + When you evluate a list, the Lisp interpreter looks at the first
   symbol in the list and then at the function definition bound to
   that symbol. Then the instructions in the function definition are
   carried out.
 + A single-quote `'` tells the Lisp interpreter that is should return
   the following expression as written, and not evaluate it as it
   would if the quote were not there.
 + Arguments are the information passed to a function. The arguments
   to a function are computed by evaluating the rest of the elements
   of the list of which the function is the first element.
 + A function always returns a value when it is evaluated (unless it
   gets an error); in addition, it may also carry out some action that
   is a side effect. In many cases, a function's primary purpose is to
   create a side effect.
