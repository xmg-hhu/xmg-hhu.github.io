---
layout: default
title: Documentation
permalink: /documentation/

nav_order: 2

---

The XMG system corresponds to what is usually called a "metagrammar compiler" (see below). More precisely it is a tool for designing large scaled grammars for natural language. Provided a compact representation of grammatical information, XMG combines elementary fragments of information to produce a fully redundant strongly lexicalised grammar. It is worth noticing that by XMG, we refer to both

*  a formalism allowing one to describe the linguistic information contained in a grammar,

*  a device computing grammar rules from a description based on this formalism. 

## What is a metagrammar?

This term has been introduced at the end of the 1990s by MH Candito. During her PhD, she proposed a new process to generate semi-automatically a Tree Adjoining Grammar (TAG) from a reduced description that captures the linguistic generalizations appearing among the trees of the grammar. This reduced description is the metagrammar.

## What is a metagrammar compiler?

Once we have described the grammar rules by specifying the way structure is shared, i.e. by defining reusable fragments, we use a specific tool to combine these. Such a tool is called a metagrammar compiler. 

## What is XMG-2?

A distinction has to be made between XMG and XMG-2 (sometimes called XMG-NG).
[XMG](https://sourcesup.cru.fr/xmg/) is a metagrammar compiler dedicated to the generation of Tree Adjoining Grammars and Interaction Grammars. XMG-2 is a whole new project which has been developed at the [LIFO](http://www.univ-orleans.fr/lifo/?lang=en) (University of Orléans) and the [SFB 991](http://www.sfb991.uni-duesseldorf.de/) (University of Düsseldorf). XMG-2 makes it possible to create new compilers, adapted to other generation tasks. Its modularity allows to simply assemble Domain Specific Languages, and automatically generate the processing chain for these languages.

In other words, XMG-2 is a tool which allows to generate compilers such as XMG: a metacompiler (or compiler compiler).

This user documentation of XMG-2 is based on the documentation for XMG, and includes the new features provided by the recent extensions.



