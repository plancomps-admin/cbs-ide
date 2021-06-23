---
title: Interface
nav_order: 2
---

# Interface

This page indicates which menus and buttons are used by the CBS IDE.

## Menus

Use the following menus for CBS specifications:

**Project**
: Use **Clean...** if an analysis needs refreshing.
  Keep **Build Automatically** enabled.

**Run**
: Add funcon interpreters as **External Tools**,
  customise their display with **Organize Favorites**,
  and run them by clicking a button.

**Spoofax**
: Use to generate editors, HTML, and LaTeX from CBS specifications,
  and to generate funcons from programs.

**Spoofax (meta)**
: Use to disable static analysis of CBS specifications
  temporarily while editing.[^disable]

## Projects

Use the project explorer to:

- import projects
- inspect which files projects contain
- open language editors

## Console

Follow the progress of Spoofax with generating and building artefacts
in the Spoofax console.

## Annotations

Many of the language specifications in CBS-beta have "info" annotations
(usually indicating incomplete argument checking in funcon terms).
The default Spoofax setting for infos is to show the source with squiggly lines.
You can turn this off in the preferences:
`General: Editors: Text Editors: Annotations: Infos`, uncheck `Text as`.

# Browsing 

You can browse the [CBS-beta] collections of funcons definitions and language specifications
as web pages and PDfs, both locally and online.
Highlighting of keywords and the various kinds of names makes the specifications easier to read.
Moreover, each name reference is hyperlinked to the corresponding declaration,
so you do not need to become familiar with the hierarchical structure of the repository.

You can also browse the source files of CBS specifications locally using Spoofax,
with the same highlighting and hyperlinks.

## Web pages

Use any browser.
[Install the CBS-beta repository] to browse its web pages locally.
Use [Jekyll] to serve the pages; they may require Internet access.

## PDFs

On macOS (Catalina and Big Sur), neither the Preview app nor the Safari web browser 
currently supports scrolling to hyperlink targets inside different PDFs,
which badly affects browsing PDFs of CBS specifications.

In contrast, Firefox supports accurate and seamless browsing of both PDFs and web pages.

## Source files

You can browse the CBS source files in the [CBS-beta] collections online [on GitHub][math branch].

[Install the CBS IDE] to browse CBS specification source files
with highlighting and hyperlinks using Spoofax.

----

[^disable]:
    Static analysis of CBS specifications is currently non-incremental,
    and generally takes at least 30 seconds,
    due to re-analysing unchanged collections of funcons.
    The Spoofax default is to reanalyse every time you stop typing...
    
    
{% include links.md %}
