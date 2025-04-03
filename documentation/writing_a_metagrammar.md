---
layout: default
title: Writing a Metagrammar
parent: Documentation
permalink: /documentation/writing_a_metagrammar

nav_order: 2

---

This section gives the general shape of a Metagrammar. The resource itself is described with domain specific languages (depending on the type of resource) which are provided by XMG dimensions. The different description languages available will be presented in the section [Dimensions](/documentation/dimensions).

# Choosing a compiler

The first decision which needs to be made is the choice of the compiler. This decision depends on the type of linguistic resource to describe. Each compiler was created for a specific grammar engineering task, and features a set of dimensions. This means that each compiler comes with its own language. The list of available dimensions is given in the next section ([Dimensions](/documentation/dimensions)),  and a list of existing compilers using these dimensions is given in the section [Bricks, contributions and commands](/documentation/bricks-contributions-and-commands). 
# Getting started

A metagrammar is composed of one or several text files, which are usually using the prefix ''.mg'' or ''.xmg''. Any text editor can be used to write XMG code, although [Emacs](https://www.gnu.org/software/emacs/) is recommended because of the different XMG modes created for it:
* the [emacs](https://sourcesup.cru.fr/xmg/xmg.el) and [vim](https://sourcesup.cru.fr/xmg/xmg.vim) modes for XMG-1 (only tree descriptions and predicate semantics).
*  new emacs modes inspired from this one, which are automatically generated when a compiler is built (in the file .install/yap/xmg/compiler/X/generated/emacs_mode, where X is the name of the compiler).
* a more advanced emacs mode for tree descriptions and frame semantics: [https://github.com/xmg-hhu/xmg-mode](https://github.com/xmg-hhu/xmg-mode)

# Including data from other files

To ease their development and reuse Metagrammars can be written in separated files. For example, all type declarations can be isolated in a file. To include the code of a file into another:

    include file_to_include.mg

# Principles and Constants

## Principles

The first piece of information one has to give in a metagrammar is the principles that will be needed to compute the grammar structures. The instruction used to do this is the `use principle with (constraints) dims (dimensions)` statement. For instance, one may decide to force the syntactic structures of the output grammar to have the grammatical function `gf` with the value `subj` only once. This is told by:

    use unicity with (gf = subj) dims (syn)

In the syn dimension, we use the unicity principle on the attribute-value ''gf = subj''. The description of the unicity principle, together with all information about principles and how to use/create them, can be found in the section [Principles and plugins](principles-and-plugins).

Note that principles use as parameters pieces of information that are associated to nodes with the status property (see below). 

## Types and Constants

Every piece of information in a XMG metagrammar is typed. This is of course the case for values in feature structures, but also for syntactic nodes, dimensions, classes, etc. There are 4 ways of defining types:

* as an enumerated type, using the syntax `type Id = {Val1,...,ValN}` such as in: 

        type CAT={n,v,p}     

Note that the values associated to a type are constants.

* as an integer interval, using the syntax `type Id = [I1 .. I2]` such as in: 

        type PERS=[1 .. 3]

* as a structured definition (T1 ... Tn represent types) `type Id = [ id1 : T1 , id2 : T2 , ..., idn : Tn ]`, such as in: 

        type ATOMIC=[
           mode : MODE,
           num : NUMBER,
           gen : GENDER,
           pers : PERS]`

* as an unspecified type `type Id !`, such as in: 

        type LABEL !

(this is useful when one wants to avoid having to define acceptable values for every single piece of information). Note that XMG integrates 3 predefined types: `int`, `bool` (whose associated values are + and -) and `string`.

## Properties

Once types have been defined, we can define typed properties that will be associated to the nodes used in the tree descriptions. The role of these properties is either   
 1.   to provide specific information to the compiler so that additional treatments can be done on the output structures to ensure their well-formedness or 
 2.   to decorate nodes with a label that is linked to the target formalism and that will appear in the output (see XMG's graphical output). The syntax used to define properties is ''property Id : Type'', such as in:

    property extraction : bool
    
A set of properties is specific to principles: it is the case for the properties **color** and **rank**. This means that when using these principles, these properties must be declared. See the section [Principles and plugins](principles-and-plugins) for more information about how to use these properties.

Properties can also be used to give a "global" name to a node, thanks to the **name** property. To perform interfacing with the lexicon, one may want to give global names to some specific nodes, in order to be able to refer to these nodes in the lexicon. Such an interfacing can be used for instance
to manage semantic information. To associate global names that will appear in the semi-automatically
produced grammar, you have to:

*  declare an enumerate type containing all the names you will use:

    `type NAME = {subjNode, objNode, anchor}`


*  declare a property ''name'' of this type:

    `property name  : NAME`


*  associate to the specific nodes the predefined names:

    `node (mark=subst,name=objNode)[cat=n]`

N.B.: make sure these name properties will not cause node unification failures, ie. do not give different
names to nodes that will be merged. At the end, the node names are visible in the output file (as an attribute of the node element):

    <node type="subst" name="objNode">

##  Features

 
Eventually we have to define typed features that are associated to nodes in several syntactic formalisms such as Feature-Based Tree Adjoining Grammars (FBTAG) or Interaction Grammars (IG). The definition of a feature is done by writing `feature Id : Type`, such as in:

    feature num : NUMBER

Up to now, we have seen the declarations that are needed by the compiler to perform different tasks (syntax checking, output processing, etc). Next we will see the heart of the metagrammar: the definition of the clauses, ie the classes. 

# Classes

Here we will see how to define classes (i.e. the abstractions in the XMG formalism). Note that in TAG these classes refer to tree fragments. A class always begins with `class Id`, such as in:

    class CanonicalSubj

N.B. A class may be parametrized, in that case the parameters are between square brackets and separated by a colon. Parameters should be identifiers which do not appear in the namespace of the class. The values for the parameters are given when a class is instantiated. Values can be constants, variables, or other class instances.

## Import

To reach a better factorization, a class can inherit from another one. This is done by invoking `import Id` (where Id is a class name), such as in:

    import TopClass[]

That is to say, the metagrammar corresponds to an inheritance hierarchy. But what does inherit mean here? In fact, the content of the imported class is made available to the daughter class. More precisely, a class uses identifiers to refer to specific pieces of information. When a class inherits from another one, it can reuse the identifiers of its mother class (provided they have been exported, see below). Thus, some node can be specialized by adding new features and so on.

Note that XMG allows multiple inheritance, and besides it offers an extended control of the scope of the inherited identifiers, since one can restrict the import to specific identifiers, and also rename imported identifiers. Restriction is done by using the keyword `as`:

    import Class[] as [?V1 ... ?Vn]
    
will only import the variables listed (`?V1 ... ?Vn`) to the scope of the current class. Renaming is also made possible by the keyword `as`, by using the `=` sign:

    import Class[] as [?V1 ... ?Vi=?X ... ?Vn] 

will do the same as the previous example, except that the variable initially named `?Vi` will be known in this namespace as `?X`. This is especially useful to avoid name conflicts.


## Export

As we just saw, we use identifiers in each class. One important point when defining a class is the scope we want these identifiers to have. More precisely we can give (or not) an extern visibility to each identifier by using the export declaration. Only exported identifiers will be available when inheriting or calling (ie instantiating) a class. Identifiers are exported using `export id1 id2 ... idn` such as in:

    export X Y

## Identifiers

In XMG, identifiers can refer either to a node, the value of a node property, or the value of a node feature. But whatever an identifier refers to, it must have been declared before by typing `declare id1 id2 ... idn`, such as in:

    declare ?X ?Y ?Z

Note that in the declare section the prefix `?` (for variables) and `!` (for skolem constants) are mandatory. 



## Content

Once the identifiers have been declared and their scope defined, we can start describing the content of the class. Basically this content is given between curly-brackets. This content can either be:

* a statement
* a conjunction of statements represented by `S1 ; S2` in the XMG formalism
* a disjunction of statements represented by `S1 | S2`
* a statement associated to an interface (see [Interface](dimensions#iface-connecting-dimensions)) 

By statement we mean:
* an expression: E (that is a variable, a constant, an attribute-value matrix, a reference (by using a dot operator, see the example below), a disjunction of expressions, or an atomic disjunction of constant values such as `@{n,v,s}`),
* a unification equation: `E1=E2`,
* a class instanciation: `ClassId[]` (note that the square-brackets after the class id are mandatory even if the instantiated class has no parameter),
* a description belonging to a dimension: this is where the main description task takes place (see section [Dimensions](/documentation/dimensions))

# Mutexes

Mutexes are the way provided by XMG to specify which classes are incompatible. To specify which classes are mutually exclusive, you first have to define a mutual exclusion set by typing `mutex Id` such as in:

    mutex SUBJ-INV

Then classes need to be added to this set by invoking `mutex Id += ClassId` such as in:

    mutex SUBJ-INV += CanonicalObject
    mutex SUBJ-INV += InvertedNominalSubject

Here we specify that we cannot use in the same description both the `CanonicalObject` and the `InvertedNominalSubject` classes.

Note that in the metagrammar file, the mutex definitions have to be placed after the type, property and feature declarations and before the valuations. This means that they can appear just before class definitions, between them, or right after.

# Valuations

Once all the classes have been defined, we can ask for the evaluation of the classes that will trigger the
combination of the fragments (ie classes calling classes that contain disjunction and/or conjunction of
fragments). For each of these specific classes, we will obtain an accumulated tree description that may
lead to the building of 0, 1 or more TAG trees. The syntax of the evaluation instruction in XMG is `value Id`, such as in:

    value n0Vn1 
