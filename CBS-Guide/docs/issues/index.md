---
title: Issues
nav_order: 7
---

# Issues

The known issues with the current prototype CBS IDE include the following.

## Responsiveness of name analysis

Name analysis of CBS specifications is currently *non-incremental*,
and generally takes at least 30 seconds,
due to re-analysing unchanged collections of funcons.
Every time you stop typing, Spoofax starts to reanalyse the current file,
together with all the files that its name analysis depends on --
and a CBS language specification always depends on Funcons-beta.

As a work-around, you can disable name analysis while editing a CBS specification,
and re-enable it when you want to check for well-formedness.
For regression testing, you can use scripts to run the CBS IDE non-interactively.

## Implementation of name analysis

Name analysis in currently implemented in the *NaBL2* meta-language.
Spoofax has stopped development of NaBL2, and has released the Statix meta-language
for future use when implementing name analysis.

Re-implementation of CBS name analysis in Statix may take some time,
due to significant differences between NaBL2 and Statix.

## Rebuilding generated editor projects

Rebuilding generated editors and translators after editing their CBS specifications
is currently *non-incremental*,
and generally takes between one and five minutes.
When you add or modify a translation rule for a specified language,
Spoofax recompiles not only the Stratego code generated from all the CBS rules,
but also numerous Stratego libraries.

And when you extend or modify the grammar of a specified language,
or add a new translation function,
Spoofax also regenerates all the parse tables.

Spoofax already has experimental implementations of incremental Stratego compilation
and parse table generation.
When these are sufficiently reliable, the CBS IDE will be reconfigured to use them,
and rebuilding generated editors and translators should become much quicker.

An aggravating factor is that the CBS IDE currently requires Spoofax to parse
rewriting rules that use *concrete* program syntax, which involves extra steps.
Changing the CBS IDE to generate abstract syntax rewriting rules should reduce
build times, and simplify the generated parsers.

## Dependency of hyperlinks on paths

The hyperlinks from names to their declarations in generated HTML and LaTeX documents
currently use (relative) paths.
CBS specifications are independent of paths to the files where declarations are stored.
However, changing the path to a declaration (or moving a declaration between files)
invalidates all hyperlinks to it.

To address this issue, hyperlinks to name declarations should never include paths --
merely the name (and what sort of name it is) and the name of the funcon collection
or language specification where the name is declared.
The CBS-beta website should map URLs in such hyperlinks to the corresponding paths,
based on the current multi-file name analysis.

## Generation of funcons indexes

The source file for [Funcons-Index] was created manually;
the CBS IDE should generate it from the definitions in [Funcons-beta].
(The CBS IDE already generates index files for funcons used by specified languages.)

## Literate CBS

The current CBS grammar requires explanatory comments to be enclosed in `/*...*/`.
For literate specification (as illustrated in [Unstable-Funcons-beta]),
it would be better for CBS specifications to be Markdown documents,
with formal definitions embedded in informal text using code fences.

For compatibility with the current CBS tools,
files with literate CBS specifications should have a different extension,
e.g., `cbs-md`.
The CBS IDE can then extract the contents of the code fences to produce a plain `cbs` file.
It can also replace the formal CBS by HTML or LaTeX code to produce Markdown files,
from which existing formatting tools can generate web pages and PDFs
with highlighting and hyperlinks.

The current CBS notation for section headings (and links to numbered sections)
should be replaced by standard Markdown notation in literate CBS.

{% include links.md %}
