# Common Lisp and Coalton Style

This document contains various style rules to be followed for Common
Lisp and Coalton code. These rules were accumulated as a part of
working on teams of programmers writing Common Lisp and Coalton in a
uniform style. The rules are not complete.

The things documented here are the generally more peculiar rules, as
opposed to ordinary software engineering best practices.

## Both languages

### IF with side-effects

If the branches of `if` have side effects, use `cond` instead. In
general, one should not see `progn` in an `if` branch unless one is
debugging.

### WHEN and UNLESS

Both `when` and `unless` are for effect or for nonlocal control. Don't
depend on their return values ever. If you care about the return
value, use `cond` with an explicit `nil` branch.

### Chained IF

Do not serially chain `if`. Use `cond`.

### Exported symbols should have docstrings

Anything exported should be documented with an appropriate docstring.


## Common Lisp

### LAMBDA and FUNCTION

Do not write `lambda` functions with `function` or `#'`.

### Macro libraries for "better" Common Lisp

Do not use any libraries to effectively replace any of the iteration,
conditionals, binding, function definitions, type declarations,
etc. These libraries make Common Lisp code more alien and less
standard. Common Lisp's operators may not be perfect, but they're
often good enough, even if somewhat verbose or clumsy.

### Keywords as data

If a keyword like `:xyz` is being used as data, always quote it as
`':xyz`. Keyword arguments, keywords-as-syntax, etc. may remain
unquoted.

### LOOP keywords

Keywords of `loop` should be spelled as Common Lisp keywords,
including `:=`. For example, `(loop :for i :below x :collect z)`.

### Prefer DOLIST/DOTIMES over LOOP

Prefer `dolist` or `dotimes` over `loop` when the code is simple
enough.

### CASE keys should always be parenthesized

No bare case keys. Prefer `otherwise` to `t`.

```
;; yes!
(case x
  ((:a)      1)
  (otherwise 2))

;; no!
(case x
  (:a        1)
  (otherwise 2))
```



## Coalton

### Prefer Seq and HashMap for collections and associations

When possible, use `Seq` and `HashMap` as the default type for
collections and associations respectively. These are efficient and
immutable. Both are compatible with `[...]` syntax.

### Prefer functions with explicitly named arguments

Prefer functions with explicit arguments over point-free style. Only
use combinators when it's obvious or sensible in context.

### Write type declarations for all exported functions

Exported functions should have type declarations.

### Define Coalton in .ct files

Use `.ct` files for Coalton code.

### Void for uninteresting functions

Functions that have no interesting value to receive or return should
use `Void`.

### Loop preference

Prefer `for` to `rec` to other solutions. Make exceptions when the
code is less complex using a particular method.

### Prefer writing in Coalton over Common Lisp

If the purpose for a project is to be in Coalton, prefer to write as
much as possible in Coalton, and only use thin wrappers to Common Lisp
as necessary. Try to avoid `lisp` forms if possible, and when they're
needed, try to factor them out into their own functions.

### One package per file

Follow the "one package per file" convention in Coalton. It's not
perfect, but works reasonably well. Use package-local nicknames
pervasively.
