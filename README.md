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



## 2. Practicing Evaluation
 *Whenever you give an editing command* to Emacs Lisp, such as the
 command to move the cursor or to scroll the screen, *you are
 evaluating an expression*, the first element of which is a
 function. *This is how Emacs works*. (Super cool. :smirk:)

 The functions you evaluate by typing keystrokes are called
 **interactive functions**, or **commands**.

### 2.1. Buffer Names

``` emacs-lisp
(buffer-name)      ; the name of the buffer
(buffer-file-name) ; the full path-name of the file
```

 + A **file** is information recorded permanently in the computer.
 + A **buffer** is information inside of Emacs that will vanish at the
   end of the editing session or when you kill the buffer.
 + Usually, a buffer contains information taht you have copied from a
   file; we say the buffer is **visiting** that file.
 + Changes to the buffer do not change the file, until you save the
   buffer.
+ The symbol `nil` is from the Latin word for "nothing". In Lisp,
  `nil` is also used to mean "false" and is a synonym for the empty
  list `()`.

 In spite of the distinction between files and buffers, you will often
 find that people refer to a file whey they mean a buffer and vice
 verse. .... When dealing with computer programs, it is important to
 keep the distinction in mind.

 :thinking: The word "buffer" comes from the meaning of the word as a
 *cushion* that deadens the force of a collision. In early computers,
 a buffer cushoined the interaction between files and the computer's
 CPU. The drums or tapes that held a file and the CPU were pieces of
 equipment that were very different from each other, owrking at their
 own speeds, in spurts. The buffer made it possible for them to work
 together effectively. Eventually, the buffer grew from being an
 intermediary, a temporary holding place, to being the place where
 work is done. This tranformation is rather like that of a small
 seaport that grew into a great city: once it was merely the place
 where cargo was warehoused temporarily before being loaded onto
 ships; then it became a business and cultural center in its own
 right.

 + `C-u C-x C-e`: causes the value returned to appear after the
   expression.

### 2.2. Getting Buffers

``` emacs-lisp
(current-buffer)  ; get the buffer itself, not the name of the buffer
(other-buffer)    ; get the most recently selected buffer other than the one you are in currently.
```

 A name and the object or entity to which the name refers are
 different from each other. You are not your name. To get a buffer
 itself, you need to use a function such as `current-buffer`.

 What you see is a *printed representation of the name of the buffer*
 without the contents of the buffer. Emacs works this way for two
 reasons: the buffer may be thousands of lines long - too long to be
 conveniently displayed; and, another buffer may have the same
 contents but a different name, and it is important to distinguish
 between them.

### 2.3. Switching Buffers
 + The keystrokes `C-x b` cause the Lisp interpreter to evaluate the
 interactive function `switch-to-buffer`.
   + For example, `C-f` calls `forward-char`, `M-e` calls
     `forward-sentence`, and so on.
 + `switch-to-bufffer` is designed for humans and does two different
   things: it switches the buffer to which Emacs's attention is
   directed; and it switches the buffer displayed in the window to the
   new buffer.
 + `set-buffer` does only one thing: it switches the attention of the
   computer program to a different buffer. The buffer on the screen
   remains unchanged.

### 2.4. Buffer Size and the Location of Point

``` emacs-lisp
(buffer-size)  ; tells you the size of the current buffer; a count of the number of characters in the buffer.
(point)        ; tells you where the cursor is located as a count of the number of characters from the beginning of the buffer up to point.
```

 + In Emacs, the current position of the cursor is called point.



## 3. How to Write Function Definitions
 All functions are defined in terms of other functions, except for a
 few **primitive functions** that are written in the C programming
 language. The primitive functions are used exactly like those written
 in Emacs Lisp and behave like them. When you write code in Emacs
 Lisp, you do not distinguish between the use of functions written in
 C and the use of functions written in Emacs Lisp. **The difference is
 irrelevant**.

### 3.1. The `defun` Macro
 Function definition is created by evaluating a Lisp expression that
 starts with the symbol `defun`.

 A function definition has up to five parts following the word
 `defun`:
 1. The name of the symbol to which the function definition should be attached.
 2. A list of the **arguments** that will be passed to the
    function. If no arguments will be passed to the function, this is
    an empty list, `()`.
 3. **Documentation** describing the function. Technically optional,
    but strongly recommended.
 4. Optionally, an expression to make the function **interactive** so
    you can use it by typing `M-x` and then the name of the function;
    or by typing an appropriate key or keychord.
 5. The code that instructs the computer what to do: the **body** of
    the function definition.

``` emacs-lisp
(defun function-name (arguments ...)
  "optional-documentation..."
  (interactive argument-passing-info) ; optional
  body ... )

;; Example
(defun multiply-by-seven (number)
  "Multiply NUMBER by seven."
  (* 7 number))

(multiply-by-seven 3) ;; 21
```

 + The choice of name is up to the programmer and should be chosen to
   make the meaning of the function clear.
 + The name you use in an argument list is private to that particular
   definition.
 + You can see he documentation string that describes the function
   when you type `C-h f` and the name of a function.
   + You should make the first line a complete sentence.
   + You should not indent the second line of a documentation string.

### 3.2. Install a Function Definition
 By evaluating `defun`, you can install the function definition in
 Emacs.

 + You can read the documentation for the installed function by typing
   `C-h f` and then the name of the function.
 + If you want to change the code in the function definition, just
   **rewrite it**.

### 3.3. Make a Function Interactive
 You can make a function interactive by placing a list that begins
 with the special form `interactive` immediately after the
 documentation.

 + An interactive function can be invoked by typing `M-x` and then the
   name of the function.
 + Or, by typing the keys to which it is bound.

``` emacs-lisp
;; Interactive Example
(defun multiply-by-seven (number)
  "Multiply NUMBER by seven."
  (interactive "p") ;; make this interactiv
  (message "The result is %d" (* 7 number)))
```

 + `C-u 7 M-x multiply-by-seven` -> `The result is 49`
 + A *prefix argument* is passed to an interactive function by typing
   <META> key followed by a number, e.g. `M-3 M-e`, or by typing `C-u`
   and then a number, e.g. `C-u 7 M-e`

 + `"p"` tells Emacs to pass the prefix argument to the function and
   use its value for the argument of the function.


### 3.4. Different Options for `interactive`
 A function with two or more arguments can have information passed to
 each argument by adding parts to the string that follows
 `interactive`. When you do this, the information is passed to each
 argument in the same order it is specified in the `interactive`
 list. In the string, each part is separated from the next part by a
 `\n`, which is a newline.

``` emacs-lisp
(defun name-of-function (arg char)
  "documentation ..."
  (interactive "p\ncZap to char: ")
  body-of-function ...)
```

 + When a function does not take arguments, `interactive` does not
   require any. Such a function contains the simple expression
   `(interactive)`.


### 3.6. `let`
 `let` is used to attach or bind a symbol to a value in such a way
 that the Lisp interpreter will not confuse the variable with a
 variable of the same name that is not part of the function. (local
 binding, local variable)

 Local variable created by a `let` expression retain their value
 *only* within the `let` expression itself; the local variables have
 no effect outside the `let` expression.

 + In Emacs Lisp, scoping is dynamic, not lexical.
 + `let` can create more than one variable at once.
 + `let` gives each variable it creates an initial value, either a
   value specified by you, or `nil`.
 + After `let` has created and bound the variables, it executes the
   code in the body of the `let`, and returns the value of the last
   expression in the body, as the value of the whole `let` expression.

#### 3.6.1. The Parts of a `let` Expression

``` emacs-lisp
(let varlist body...)

;; Example
(let ((variable value)
      (variable value)
      ...)
 body...)
```

 + `varlist`: each element of which is either **a symbol** by itself
   or **a two-element list**, the first element of which is a symbol.
   + The symbols in `varlist` are the variables that are given initial
     values by the `let` special form.
   + A symbol by itself: initial value of `nil`
   + First element of a two-element list: bound to the value that is
     returned when the second element is evaluated.
 + `body`: usually consists of one or more lists.

#### 3.6.2. Sample `let` Expression

``` emacs-lisp
(let ((zebra "stripes")
      (tiger "fierce"))
 (message "One kind of animal has %s and another is %s."
          zebra tiger))
```

> According to Jared Diamond in Guns, Germs, and Steel, "...zebras
> become impossibly dangerous as they grow older" but the claim here
> is that they do not become fierce like a tiger.

#### 3.6.3. Uninitialized Variables in a `let` Statement

``` emacs-lisp
(let ((birch 3)
      pine
      fir
      (oak 'some))
 (message
   "Here are %d variables with %s, %s, and %s value."
   birch pine fir oak))

;;  "Here are 3 variables with nil, nil, and some value."
```


### 3.7. The `if` Special Form

``` emacs-lisp
(if true-or-false-test
    action-to-carry-out-if-test-is-true)

;; Example
(if (> 5 4)
    (message "5 is greater than 4!"))
```

 + The test part of an `if` expression is often called the *if-part*,
   and the second argument is often called the *then-part*.
 + When an `if` expression is written, the true-or-false-test is
   usually written on the same line as the symbol `if`.
 + The then-part is written on the second and subsequent lines to make
   it easier to read.

#### 3.7.1 The `type-of-animal` Function in Detail

``` emacs-lisp
(defun type-of-animal (characteristic)
  "Print message in each area depending to CHARACTERISTIC.
If the CHARACTERISTIC is the string \"fierce\",
then warn of a tiger."
  (if (equal characteristic "fierce")
      (message "It is a tiger!")))

(type-of-animal "fierce") ; "It is a tiger!"
(type-of-animal "what") ; nil
```

 + `equal` function determines whether its first argument is equal to
   its second argument.
