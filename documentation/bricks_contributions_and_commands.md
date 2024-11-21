---
layout: page
title: Bricks, contributions and commands
parent: Documentation
permalink: /documentation/bricks-contributions-and-commands

nav_order: 6

---

As stated previously, XMG-2 compilers are built using bricks, which implement the compiling steps for parts of metagrammatical languages. For example, the **avm** brick contain all the support for feature structures, and the **syn** brick for the language of the `<syn>` dimension.
Bricks are distributed in contributions, for instance the **core** contribution, which contains all the basic features of XMG-2. The **treemg** contribution contains all the bricks which, in addition to the ones of the **core** contribution, allow to build the **synsem** compiler (equivalent to XMG-1).

Making bricks available is done by installing them. The **install** command takes as parameter a contribution and installs all the bricks provided by it.

All contributions with names ending with "compiler" are special, as they contain a different type of bricks. These bricks contain description of compilers which need to be created before one can use them. Creating a compiler is done with the command **build**.

Compilers are usually named after the dimensions they feature (**synframe** provides both the `<syn>` and the `<frame>` dimensions). The following compilers can be installed:

**framelpcompiler**: for morphological description with frame semantics.

**lexCompiler**: to create lemma files for a TAG parser (as LexConverter).

**lpcompiler**: for morphological descriptions.

**mphcompiler**: to create files of inflected forms for a TAG parser (as LexConverter).

**synframeCompiler**: for TAG descriptions with frame semantics.

**syn2frameCompiler**: the same with a slightly different tree description language.

**synsemCompiler**: for TAG descriptions with predicate semantics (XMG-1).

**tfcompiler**: for morphological descriptions specified using topological fields.

To learn more about how these compilers were assembled, and how to assemble customized compilers for specific description tasks, see [[Petitjean et al., 2016](https://link.springer.com/chapter/10.1007/978-3-662-53826-5_16)].

The commands provided by XMG can be separated in two categories: some of them will be used by any user writing a linguistic resource, the others will be reserved to developers of XMG extensions. 

User commands:

*  `xmg bootstrap`: installs the basic features of XMG.

*  `xmg install path_to_contribution`: makes a contribution available.

*  `xmg build`: assembles a compiler according to a yaml description.

*  `xmg gui gui_name`: starts a GUI. The only GUI provided up to now is `tag`.

*  `xmg compile compiler_name path_to_metagrammar`: compiles a metagrammar with a given compiler. The options for this command are:  
    * `--force` to generate the grammar even if an XML file already exists
    * `--latin` to manipulate metagrammars written in latin encoding
    * `--debug` to print some useful information about compilation
    * `--notype` to disable the strong type checking (equivalent to XMG1)
    * `--more` to generate additional files (type hierarchy, etc)
    *  `--output` or `-o` allow to specify an output file (the default is the name of the metagrammar file with the xml or json extension)

Developper commands:

*  `xmg startcommand`

*  `xmg startyaplib`

*  `xmg startbrick`

*  `xmg startcompiler`

*  `xmg startpylib`

*  `xmg startcontrib`


