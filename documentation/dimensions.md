---
layout: default
title: Dimensions
parent: Documentation
permalink: /documentation/dimensions

nav_order: 3

---

Dimensions contain the linguistic descriptions, which are composed of constraints. Each XMG dimension comes with a specific set of constraints, which allow to describe different structures (trees, feature structures, etc). This section presents the different dimensions supported by XMG, and their description languages.

The dimensions documented here are:
* [SYN](#syn-a-tree-description-language)
* [IFACE](#iface-connecting-dimensions)
* [FRAME](#frame-describing-semantics-using-typed-feature-structures)
* [SEM](#sem-describing-semantics-using-predicates)
* [MORPH_LP](#morph_lp-describing-morphology-with-ordered-fields)
* [LEMMA](#lemma-describing-lexicons-of-lemmas)
* [MORPHO](#morpho-describing-lexicons-of-inflected-forms)
# SYN: a tree description language

The `<syn>` dimension allows to describe trees, initially to create Tree Adjoining Grammars or Interaction Grammars. To use this language, you can either build a new compiler using the brick **syn** (contribution **treemg**) or use one of the existing compilers including the dimension: **synsem** (contribution **synsemCompiler**, with predicate based semantics) or **synframe** (contribution **synframeCompiler**, with frame based semantics).

A syntactic description is given following the pattern `<syn>{ formulas }`. Now what kind of formulas does a syntactic description contain ? The answer is nodes. These nodes are in relation with each other. In XMG, you may give a name to a node by using a variable, and also associate properties and/or features with it. The classic node definition is `node ?id ( prop1=val1 , ... , propN=valN ) [ feat1=val1 , ... , featN=valN ]` such as in:

    node ?Y (gf=subj)[cat=n]
Here we have a node that we refer to by using the ?Y variable. This node has the property gf (grammatical function) associated with the value subj, and the feature structure [cat=n] (note that associating a variable to a node is optional).

Once you defined the nodes of the tree fragment, you can describe how they are related to each other. To do this, you have the following operators:

* `->` 	strict dominance
* `->+` 	strict large dominance (transitive non-reflexive closure)
* `->*` 	large dominance (transitive reflexive closure)
* `>>` 	strict precedence
* `>>+` 	strict large precedence (transitive non-reflexive closure)
* `>>*` 	large precedence (transitive reflexive closure)
* `=` 	node equation

Each subformula you define can be added conjunctively (using ";") or disjunctively (using "|") to the description. For instance, the fragment:

![alt text]({{ site.url }}/docs/assets/tree.png)


can be represented by the following code in XMG:

    class Example
      declare ?X ?Y ?Z
      {<syn>{
        node ?X [cat=S] ; node ?Y [cat=N] ; node ?Z [cat=V] ;
        ?X -> ?Y ; ?X -> ?Z ; ?Y >> ?Z
      }
    }
XMG also supports an alternative way of specifiyng how the nodes are related to each other. This alternative syntax should allow the user to both define the nodes and give their relations at the same time:

* node { node } 	strict dominance
* node { ...+node } 	strict large dominance (transitive non-reflexive closure)
* node { ...node } 	large dominance (transitive reflexive closure)
* node node 	strict precedence
* node ,,,+node 	strict large precedence (transitive non-reflexive closure)
* node ,,,node 	large precedence (transitive reflexive closure)
* = 	node equation

Thus the tree fragment above could be defined in the XMG syntax the following way:

    class Example
    {<syn>{
       node [cat=S] {
               node [cat=N]
               node [cat=V]
               }
       }
    }

Note that the use of variables to refer to the nodes becomes useless inside the fragment, nonetheless we may want to assign variables to node to reuse them later through inheritance. 

When the properties or features of a node need to be unified with a variables, one can use the keywords 'props' and 'feats'. This is useful for instance when the properties or features need to be exported.

       node NN props ?P feats ?F;
       ?F = [cat=n,top = [idx=?W,num = ?N, pers=?M] ];
       ?P = [color=red,mark=subst] 



# IFACE: connecting dimensions

Interfaces correspond to attribute-value matrices, allowing one to associate a global name to an identifier.
The syntax of the interface is the following (the interface is between square-brackets): 

    class Id
    { ... }*= [Name1=Id1, ... , NameN=IdN]


The *= operator represents unifying extension. 
When a class is valuated, the descriptions (contained in the classes) it refers to are accumulated. At the
same time, the interfaces associated with these descriptions are accumulated. The semantics of their
accumulation may correspond to unification.

Let us see the use of an interface in an example. Considering the tree fragment used so far. Imagine we
want to refer to the N node outside of the class. To do so, we give this node a global name. We can do
this by using the following interface:

    class Example
    declare ?X ?Y ?Z
    {<syn>{
       node ?X [cat=S] {
                        node ?Y [cat=N]
                        node ?Z [cat=V]
                       }
       }*=[subj = ?Y]
    }

In a class A which is combined with Example, you can constraint the identification of a local node X with the subj node of Example by reusing the feature subj in the interface of A:  `*=[subj=?X]`
Note that the interface may also be used to give names to properties or feature values.

The interface can also be accessed as a regular dimension, meaning that the `*=` operator can be replaced as follows:

    class Example
    declare ?X ?Y ?Z
    {<syn>{
       node ?X [cat=S] {
                        node ?Y [cat=N]
                        node ?Z [cat=V]
                       }
       };
   <iface>{[subj = ?Y]}
    } 
    


    
# FRAME: describing semantics using typed feature structures

The `<frame>` dimension can be used in a compiler by using the **frame** brick (contribution **framemg**). A set a pre-assembled compilers use this brick: **synframeCompiler** (with Tree Adjoining Grammars) and **framelpcompiler** (with morphological descriptions).

This dimension allows to describe typed feature structures. These structures use conjunctive types, which means that types are not atomic, but rather sets of elementary types. When two typed feature structures get unified, the type of the resulting structure is determined by a type hierarchy. In the simple case, and if the types are compatible, the resulting type is the union of both types. 

![alt text]({{ site.url }}/docs/assets/hierarchy.png)

Type hierarchies are defined in two steps. First, the declaration of the atomic types:

    frame-types = {t1,t2,...,tn}
    
where t1, t2, ..., tn are constants. In addition to these atomic types, the type + (most generic type, or root of the type hierarchy) is defined by default.

In a second time, the atomic types get organized into a hierarchy by specifying constraints:

    frame-constraints = {c1, c2,..., cn }
    
where c1, c2, ..., cn are type constraints. Several sorts of them are available: 


*  constraints concerning subtyping: ` t1 t2 ... tn -> tt1 tt2 ... ttn `

*  incompatibility constraints: ` t1 t2 -> - `

*  constraints concerning attributes ` t1 t2 ... tn -> c1 ... cn `, with `c1 ... cn` constraints on attributes

Constraints on attributes can be of the following types:


*  existence constraint: `att : +`

*  constraint on the value/type of an attribute: `att : val`

*  path equality `att1 = att2`

Note that all attributes in these constraints can be paths, using dots. For example, `actor.name : +` means that there is an attribute `actor`, and that the value of this attribute has an attribute `name`. 

The following example makes use of all the types of constraints:

    frame-types = {event, motion, activity, causation, locomotion}
    frame-constraints = { 
          causation -> event,
          motion -> event,
          activity -> event,
          motion causation -> -,
          activity causation -> -,
          activity motion -> locomotion,
          activity -> actor:+,
          motion -> mover:+,
          causation -> cause:+, 
          causation -> effect:+
    }
    
The first three constraints are subsumption constraints. `causation -> event` means for example that all frames of type `causation` also have type `event`. The two next constraint express incompatibilities of types, meaning for instance that a frame cannot have both types `motion` and `causation`. 
`activity motion -> locomotion` means that all frames having both type `activity` and `motion` will also have type `locomotion`.
The three last constraints concern attributes. For instance, `causation -> cause:+ effect:+` makes sure that all frames of type causation will have attributes `cause` and `effect`, both with value `+`.

Semantic descriptions with frames can as well include relations between unification variables. The different types of relations are declared in the following way:

    frame-relations = { r1(a11,...,a1y), ..., rn(an1,...,anx) }

where `r1...rn` are relation identifiers,`a11,...,a1y` are the types of the y arguments of `r1`, and `an1...anx` are the types of the x arguments of `rn`.

![alt text]({{ site.url }}/docs/assets/frames.png)


In the frame dimension, frames are described in a very similar way to untyped feature structures. The type of the frame and the attribute-value pairs are given between square brackets, and separated by commas. Pairs are separated by a colon. For example, the  first frame in the example above can be encoded in the following way:

    class example
    declare ?X1 ?X2 
    {
      <frame>{
        [causation,
         actor:?X1,
         theme:?X2,
         cause: [activity,
                 actor: ?X1,
                 theme: ?X2],
         effect:?IN[activity,
                    actor: ?X2]
         ]
       }
    }

Relations are written using parenthesis for the arguments. For example, the statement to express the relation `part-of` between the variables `?V1` and `?V2` would be:

    <frame>{
      part-of(?V1,?V2)
    }
   
##  Exporting the type hierarchy

XMG computes the type hierarchy to handle the unification of typed features structures during the compilation of the metagrammar. However, to be able to reuse this type hierarchy with the generated resource (with a parser for example), the hierarchy needs to be exported. When compiling the metagrammar, the option `--more` activates the export of additional useful resources, which is the hierarchy in our case. For a file called `example.mg`, the complete command is the following:

    xmg compile synframe example.mg --force --more
    
The compiled grammar can then be found in the file `example.xml` and the type hierarchy in the file `more.mac`.

# SEM: describing semantics using predicates

Here we will see how to describe semantic information with predicates. Basically, this dimension allows one to 
describe:

*  predicates with 0, 1 or more arguments and a label, 

*  negation, 

*  a specific relation called "scope_over" for dealing with quantifiers,

*  and semantic identifiers.

So the language of the semantic dimension is:
    Description ::= l:p(E_1,...,E_n) | ~ l:p(E_1,...,E_n) | E_i << E_j | E 
In XMG concrete syntax, one may define a class with a semantic content by: 

    class BinaryRel
    declare !L ?X ?Y ?P
    { 
    <sem>{ !L:?P(?X,?Y) }*=[pred=?P]
    }

That is to say, we define the class `BinaryRel` in which 3 variables and a skolem constant (prefixed
by "!") are declared. This class only contains semantic information (dimension `<sem>`), more
precisely it contains a predicate (whose value is the variable ?P) of arity 2, its arguments are the
variables ?X and ?Y. !L represents the label associated to this predicate. Note that we use the interface
dimension to give the name **pred** to ?P. Further, this variable may be unified with a constant, and the
value of the predicate thus given.
Finally, it is possible to define a class containing both a semantic and syntactic dimension, and these
dimensions may share identifiers. Besides sharing identifiers may also be done by using the interface
dimension. Thus XMG provides efficient devices to define a syntax / semantics interface within the 
metagrammar.

# MORPH_LP: describing morphology with ordered fields

This dimension allows to form words by assembling morphemes. First, fields need to be defined and ordered (using constraints), then information can be added to the fields. The description language offered by dimension consists of only one keyword and two operators.

* `field ?Id [a1=v1, ..., an=vn]`: 	definition of a field, with optional identifier and feature structure
* `f1 >> f2`:   linear precedence constraint between fields `f1` and `f2`	
* `f1 <- val`:	    affectation of the value `val` (typically a string) into the field `f1` 

The following class shows concretely how the dimension can be used:

    class plural_suffix
    {
      <morph>{
        field suffix;
        root >> suffix;
        suffix <- "s"
        }
    }
    
A field `suffix` is created, and placed on the right of another field `root` (defined in another class). The string "s" is added into the new field. This complete example can be found in the `MetaGrammars/lp_morph/example.mg file` of XMG-2 (or on [GitHub](https://github.com/spetitjean/XMG-2/blob/master/MetaGrammars/lp_morph/example.mg)).

Note that in this example, the field `suffix` is a global constant. It is also possible, and recommended, to use variables as field identifiers. In this case, the fields can be accessed (for linear ordering or affecting values) only in the class where it is defined or by using an import mechanism.    

Metagrammars containing contributions to the **morph_lp** dimension can be compiled with the compilers named lp (only morphology) and framelp (with semantic frames):

    xmg compile lp file.mg

# LEMMA: describing lexicons of lemmas

This dimensions is used  when parsing (with a TAG for example). The typical use of these lexicons is to list which TAG families are compatible with the lemmas of the language. The description language basically allows to associate values to different attributes. An example of class using the lemma dimension is as follows:

    class LemmeAller
    {
    <lemma> {
      entry <- "aller";
      cat   <- v;
      fam   <- n0Vloc1
     }
    }
    
where `entry` is the lemma, `fam` a TAG family which can use this lemma as anchor, and `cat` the syntactic category of the anchor.
    
Metagrammars containing contributions to the `<lemma>` dimension (only) must be compiled with the compiler named lex:

    xmg compile lex lemma.mg
    
#  MORPHO: describing lexicons of inflected forms

This dimensions is used when parsing (with a TAG for example). The typical use of these lexicons is to list which lemmas are compatible with the inflected forms of the language. The description language basically allows to associate values to different attributes. An example of class using the morpho dimension is as follows: 

    class a
    {
    <morpho> {
      morph <- "a";
      lemma <- "avoir";
      cat   <- v
     }
    }
    
where `morph` is the inflected form, `lemma` is the lemma associated to this inflected form, and `cat` the syntactic category of the inflected form.
    
Metagrammars containing contributions to the `<morpho>` dimension (only) must be compiled with the compiler named mph:

    xmg compile mph morph.mg
