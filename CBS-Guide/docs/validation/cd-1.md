---
title: CD-1
parent: Validation
---

# CD-1 evolution

You can now write test programs with fewer parentheses:

- `identity.cd`:
  ```
  let f = lambda x. x in f 42
  ```
- `ski.cd`:
  ```
  let s = lambda x. lambda y. lambda z. x z (y z) in
  let k = lambda x. lambda y. x in
  let i = lambda x. x in
  s (s (k s) k)
  ```

Their ASTs are (essentially) the same as before.
