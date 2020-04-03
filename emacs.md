# Emacs Manual

 [Reference](https://www.gnu.org/software/emacs/manual/html_node/emacs/index.html#Top)


## 49. Customization

### 49.2. Variables
 Emacs uses many Lisp variables for internal record keeping, but the
 most interesting variables for a non-programmer user are those meant
 for users to change - these are called **customizable variables** or
 just **user options**.

#### 49.2.2. Hooks
 Hooks are an important mechanism for customizing Emacs. A **hook** is
 a Lisp variable which holds a list of functions, to be called on some
 well-defined occasion. (This is called *running the hook*.)

 Most hooks are normal hooks. This means that when Emacs runs the
 hook, it calls each hook function in turn, with no arguments. Every
 variable whose name ends in `-hook` is a normal hook.

 A few hooks are abnormal hooks. Their names end in `-functions`,
 instead of `-hook`. What makes these hooks abnormal is the way its
 functions are called - perhaps they are given arguments, or perhaps
 the values they return are used in some way.

 You can set a hook variable with `setq` like any other Lisp variable,
 but the recommended way to add a function to a hook (either normal or
 abnormal) is to use `add-hook`.

 Most major modes run one or more *mode hooks* as the last step of
 initialization. Mode hooks are a convenient way to customize the
 bevhaviour of individual modes; they are always normal.


#### 49.2.4 Local Variables in Files
 A file can specify local variable values to use when editing the file
 with Emacs. Visiting the file or setting a major mode checks for
 local variable specifications; it automatically makes these variables
 local to the buffer, and sets them to the values specified in the
 file.

##### 49.2.4.1 Specifying File Variables
 There are two ways to specify file local variable values: in the
 first line, or with a local variables list.

 Here's how to specify them in the first line:

```lisp
-*- mode: modename; var: value; ... -*-
```

 You can specify any number of variable/value pairs in this way, each
 pair with a colon and semicolon. The special variable/value pair
 `mode: modename;`, if present, specifies a major mode. The *values*
 are used literally, and not evaluated.

 You can use `M-x add-file-local-variable-prop-line` instead of adding
 entries by hand.

 In shell scripts, the first line is used to identify the script
 interpreter, so you cannot put any local variables there. To
 accomodate this, Emacs looks for local variables specifications in
 the *second* line if the first line specifies an interpreter.

 A local variables list starts with a lone containing the string
 `Local Variables:`, and ends with a line containing the string
 `End:`.

``` c++
/* Local Variables:  */
/* mode: c           */
/* comment-column: 0 */
/* End:              */
```

 In this example, each line starts with the prefix `/*` and ends with
 the suffix `*/`. Emacs recognizes the prefix and suffix by finding
 them surrounding the magic string `Local Variables:`, on the first
 line of the list; it then automatically discards them from the other
 lines of the list. The usual reason for using a prefix and/or suffix
 is to embed the local variables list in a comment, so it won't
 confuse other programs that the file is intended for.
