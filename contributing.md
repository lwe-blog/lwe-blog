---
layout: page
title: "contributing"
permalink: "/contributing/"
---
# Contributing

This is a collaborative blog, so everyone is welcome (and encouraged) to
contribute.

**The easiest way to contribute is to simply write a LaTeX
or Markup document, and email it to me (preetum [at] berkeley).**
(Include the .bib file if you use a bibliography, and put all macros into the
main tex).

The entire source code of this blog is public on
[Github](https://github.com/lwe-blog/lwe-blog.github.io), so in theory you can
also author a new post by compiling and pushing the appropriate files in the
appropriate places. In practice it's rather messy, but details are
[here](https://github.com/lwe-blog/lwe-blog.github.io).

We use a LateX to HTML compiler which should work on anything that's not too fancy.
(details of what works is below, but almost everything in math mode should
work). Markdown is rendered "natively".

## Writing LaTeX: What works
We use [this](https://github.com/lwe-blog/latex3html) LaTeX to HTML compiler,
which is a modified version of Luca's
[LaTeX2WP](https://lucatrevisan.wordpress.com/latex-to-wordpress/).

It has support for:

- Theorem-like environments: "theorem","definition","lemma","proposition","corollary","claim", "remark","example","exercise"

- section, subsection, section\*, subsection\*

- proof environment, and \qed

- font formatting: \em, \bf, \it, \emph, \textbf, \textit

- Arbitrary **math-mode** macros. (There are processed by MathJax, so they only
  expand in math mode).

- The following math environments: <http://docs.mathjax.org/en/latest/tex.html#environments>
(including align*, array, bmatrix, cases).

- Footnotes (which render inline, collapsible).

- Bibliography (input .bbl)

- \cite, \ref, \eqref, \label


**Working Examples**

- FastJL post: [TeX](https://github.com/lwe-blog/latex3html/blob/master/example-fastjl/fastjl.tex),
[HTML](http://learningwitherrors.org/files/fastjl.html),
[PDF](http://learningwitherrors.org/files/fastjl.pdf)

- Small-bias post: 
[TeX](https://github.com/lwe-blog/lwe-blog.github.io/blob/master/sources/june2016-small-bias/small-bias.tex),
[HTML](http://learningwitherrors.org/sources/june2016-small-bias/small-bias.html),
[PDF](http://learningwitherrors.org/sources/june2016-small-bias/small-bias.pdf)

