---
title: CD-1
parent: Specification
---

# CD-1 evolution

This extension adds grouping disambiguation to the `CD` language,
and allows you to write applications without parentheses.

CBS currently uses the SDF3 meta-language for disambiguation,
enclosing references to embedded grammar sorts and productions in back-ticks.

## Changes

1.  Replace:

    ```
    | exp '(' exp ')'
    ```

    by:

    ```
    | exp  exp
    ```

2.  Replace:

    ```
    Rule eval[[ E1 '(' E2 ')' ]] =
    ```

    by:

    ```
    Rule eval[[ E1 E2 ]] =
    ```

## Additions

```
Syntax SDF
/*
context-free syntax
``exp ::= 'lambda' id '.' exp`` {longest-match}
``exp ::= exp exp`` {left}
``exp ::= 'let' id '=' exp 'in' exp`` {longest-match}

context-free priorities
``exp ::= exp exp``
> {
``exp ::= 'lambda' id '.' exp``
``exp ::= 'let' id '=' exp 'in' exp``
}
*/

Lexis SDF
/*
lexical syntax
  ``id`` = ``keyword`` {reject}

lexical restrictions
  ``id``  -/- [a-z0-9]
  ``int`` -/- [0-9]
*/
```

{:.note}
> You could put disambiguation specifications in a separate file,
> e.g., named `CD-Disambiguation.cbs`
