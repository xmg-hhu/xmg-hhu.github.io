---
layout: default
title: Errors and support
parent: Documentation
permalink: /documentation/errors-and-support

nav_order: 9

---

## Common errors

This section lists some errors that can be encountered while developing a resource with XMG.

### Tokenizer errors

*  `unrecognized`: the given symbol is not supported by the tokenizer. You may check the encoding of the file or try to use the `--latin` option.
### Syntax errors

*  `expected`: syntax error. Check the different languages of the section [Dimensions](http://dokufarm.phil.hhu.de/xmg/doku.php?id=start#dimensions). Maybe the used compiler is not the right one?
### Type errors

*  `incompatible types`: the value of an attribute does not have the expected type. Check the type declarations.

*  `unknown constant`: all constants, except for the boolean values `+` and `-` (type **bool**), must be declared (a value for an enumerated type for example). 

*  `multiple definitions of constant (in type definitions)`: the same identifier is used to refer to two constants (a value can only have one type).

*  `variable not declared`: a variable appears in the class, but is not imported, nor declared, nor a class parameter.

*  `property_not_declared`: a node is given a property which was not defined in the headers.

*  `feature not declared`: a node is given a feature which was not defined in the headers.

*  `multiple definitions of feature`: the same identifier is used to refer to two different features.

*  `type not defined`: a structure is given (in the headers) a type which is not defined. 

*  `multiple definitions of type`: the same identifier is used to refer to two types.

*  `incompatible expressions`: the types of two expressions are not compatible.

*  `value not in range`: an integer variable has a value incompatible with its definition (out of the bounds). 




### Unfolder errors

*  `cycle detected with class`: the given class creates a cycle in the class hierarchies (it calls a class which is already one of its ancestors in the hierarchy).  This is forbidden as the resource generated would be infinite.

*  `no class set to be valued`: there should be at least one axiom in a metagrammar (see [Valuations](http://dokufarm.phil.hhu.de/xmg/doku.php?id=start#valuations)).


## Other common problems


*  Uncolored nodes: in a XMG metagrammar using the **syn** dimension and the **colors** principle, a color needs to be given to all nodes appearing in the accumulation. Warnings will be displayed if some nodes do not have colors. Grammars developed with XMG-1 can contain uncolored nodes, but they are ignored by the compiler. With XMG-2, you can simply remove these nodes from the metagrammar to obtain the same result.

## Support

To report any bug concerning XMG, please use the [issue tracker](https///github.com/spetitjean/XMG-2/issues). You can also use the tracker for any question or request for assistance (installing, developing with XMG).

Please also use the [GitHub page](https///github.com/spetitjean/XMG-2) if you would like to request new extensions for XMG, or to share your own extensions or resources.

