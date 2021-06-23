---
title: Specification
nav_order: 3
---

# Specification

This page illustrates how to create a language specification for use with the CBS IDE.

You should [install the CBS IDE] in Spoofax and 
[install the CBS-beta repository] in your file system
before proceeding.

## Create folders and links in the file system

1.  Create a folder for your own collection of languages, e.g.: `My-Languages`
2.  Choose a *full* name for your language (no spaces), e.g.: `CBS-Demo`
3.  Create the folder `CBS-Demo` in `My-Languages`
4.  Choose a *short* name for your language, e.g.: `CD`
5.  Create the folder `CD-cbs` in `CBS-Demo`
6.  Create the folder `CD` in `CD-cbs`
7.  Create the folder `CD-Start` in `CD`
8.  Create a *symbolic link* in `CD-cbs` to your copy of `CBS-beta/Funcons-beta`

Your resulting folder hierarchy should look something like this:[^editor]

```
My-Languages
- CBS-Demo
  - CD-cbs
    - CD
      - CD-Start
    - Funcons-beta -> YOUR-PATH-TO/CBS-beta/Funcons-beta

CBS-beta
- ...
- Funcons-beta
- ...
```
    
## Create a CBS language specification project in Spoofax

1.  Make `CD-cbs` a *general project*
    1.  Select `File: New: Project...: Wizards: General: Project`, click `Next`
    2.  Project Name
        : `CD-cbs`
        
        Location
        : `YOUR-PATH-TO/My-Languages/CBS-Demo/CD-cbs`, click `Finish`
2.  Add the *Spoofax nature* to the `CD-cbs` project:
    1.  Right-click on the project
    2.  Select `Spoofax: Add Spoofax nature`
3.  Spoofax should now analyse your project (about 30 seconds), otherwise
    1.  Check that the `CBS` language project has already been built
    2.  Check that the `Spoofax (meta)` menu indicates that analysis is enabled
    2.  Try cleaning the `CD-cbs` project
4.  Expand the folder `CD-cbs/Funcons-beta/Funcons-Index` 
    and open an editor on the file `Funcons-Index.cbs`
5.  Check that:
    1.  There are no parse errors
    2.  CBS keywords and names are highlighted
    3.  Command- or Control-hovering over names makes them hyperlinks
    4.  Clicking a hyperlink opens an editor showing the declaration of the name

## Specify your language

1.  Check that the `Spoofax (meta)` menu indicates that analysis is currently enabled
2.  Create a file named `CD-Start.cbs` in the `CD-Start` folder with the following contents:[^tabs]
    ```
    Language "CBS-Demo"
    
    Syntax START:start ::= exp
    
    Semantics start[[ _:start ]] : =>values

    Rule start[[ E ]] =
      initialise-binding finalise-failing eval[[ E ]]
    
    Syntax E:exp ::= int
                   | id
                   | 'lambda' id '.' exp
                   | exp '(' exp ')'
                   | 'let' id '=' exp 'in' exp
                   | '(' exp ')'
    
    Semantics eval[[ _:exp ]] : => values
    
    Rule eval[[ N ]] = int[[ N ]]
    
    Rule eval[[ X ]] = bound id[[ X ]]
    
    Rule eval[[ 'lambda' X '.' E ]] =
      function closure
        scope( bind( id[[ X ]], given ), 
               eval[[ E ]] )
    
    Rule eval[[ E1 '(' E2 ')' ]] =
      apply( eval[[ E1 ]], eval[[ E2 ]] )
    
    Rule eval[[ 'let' X '=' E1 'in' E2 ]] =
        scope( bind( id[[ X ]], eval[[ E1 ]] ), 
               eval[[ E2 ]] )
    
    Rule eval[[ '(' E ')' ]] = eval[[ E ]]
    
    
    Lexis  N:int ::= ('0'-'9')+
    
    Semantics int[[ N:int ]] : => integers
                = decimal \"N\"
    
    
    Lexis  X:id  ::= ('a'-'z') ('a'-'z'|'0'-'9')*
    
    Semantics id[[ X:id ]] : => identifiers
                = \"X\"
    ```
2.  Check that:
    1.  There are no parse errors
    2.  CBS keywords and names are highlighted
    3.  Command- or Control-hovering over names makes them hyperlinks
    4.  Clicking a hyperlink scrolls to the declaration of the name

{:.note}
> You can now [validate][Validation] the above language specification
> by generating a translator from programs to funcons,
> and checking the outcomes of executing translations of test programs

## Evolve a language and its specification

The following pages suggest various extensions of the language specified above.

You can add extensions to the specification of language syntax and semantics
in `CD-Start.cbs` in any order.
However, specification of syntactic grouping disambiguation is less modular;
following the order shown below should ensure correct parsing of the suggested test programs
when validating each extension.

In CBS, a language specification is *not* treated as a reusable component.
You should invasively edit it when extending the specified language.
If you commit each extension to a repository,
you can compare commits to display their differences visually in Spoofax
(the local history feature might also be used).

----

[^tabs]:
    To ensure that indentation is preserved when viewing CBS specifications
    in different editors and browsers, 
    avoid conversion of spaces to tab characters when entering or editing text.

[^editor]:
    `CBS-Demo` will contain also the generated `CD-Editor`,
    and a folder `CD-Tests` of test programs.
    The `CD` folder supports division of a language specification into multiple sections.
    Folders such as `CD-Start` can contain formatted versions of CBS source files.

{% include links.md %}
