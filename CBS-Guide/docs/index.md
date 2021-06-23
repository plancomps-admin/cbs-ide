---
title: Overview
nav_order: 0
---

# Overview

A prototype of a CBS IDE is available [on request](mailto:plancomps@gmail.com).
This website provides guidance on using the prototype, assuming familiarity with CBS.
See the
[CBS-beta website][CBS-beta]
for information about the CBS framework.

[Installation]
: Software requirements, with links to download sites

[Interface]
: Using Spoofax menu actions and browsing specifications

[Specification]
: Creating and editing language specifications

[Validation]
: Generating language editors to validate specifications

[Formatting]
: Generating LaTeX documents and web pages from specifications

# Development

Although the prototype CBS IDE is not yet fully developed, and has not been released,
it has been successfully used in the PLanCompS project for the following purposes:

- Checking the funcon definitions and language specifications in the [CBS-beta] collections
  for name resolution and arity consistency;

- Generating editors, parsers, and translators for the specified languages;

- Editing and parsing test programs in specified languages;
  
- Validating language specifications by translating test programs to funcons,
  and checking that executing the funcons gives the expected outcomes;

- Generating HTML and LaTeX documents from CBS source files.

However, users should be aware of some [issues] with the current version of the prototype.

{% include links.md %}
