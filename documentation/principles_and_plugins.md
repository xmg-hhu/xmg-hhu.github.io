---
layout: default
title: Principles and plugins
parent: Documentation
permalink: /documentation/principles-and-plugins

nav_order: 4

---

This section contains descriptions of existing XMG principles, and a method for the user to create their own principles .
# Solvers and principles

XMG's most complex task is to compute all possible models for the descriptions of the metagrammar. These descriptions are sets of constraints, therefore extracting the models is a constraint satisfaction problem. Every dimension comes with its own solver (sometimes identity), which builds only structures with the right properties. For example, the **syn** dimension (which allows to describe trees) accumulates tree descriptions: nodes, dominance and precedence constraints. The **syn** dimension comes with a solver called **tree**, makes sure that all solutions of the constraint satisfaction problem will be well-formed tree (one root, etc) which also takes into account the constraints given in the metagrammar. 

Principles are sets of constraints which come in addition to a solver. They are useful to describe constraints which cannot be described in classes. The metagrammar makes it possible to express constraint between two objects (two nodes for instance) when they can be referred to with variables, but it is sometimes needed to express constraints between one structure from a class and a structure that might appear in another class, or several classes, or not appear at all. In other words, the XMG constraints can be considered as local (to the class) constraints, whereas principles allow to express global constraints.     

Three "historical" principles are provided by XMG, for the dimension **syn**, namely:

* **unicity**: 	uniqueness on a specific attribute-value
* **rank**: 	ordering of clitics by means of associating the rank property to nodes
* **color**: 	automatization of the node merging by assigning color to nodes
but more principles can also be added as **plugins**. This is what led to the creation of the new principles:

* **precedes**: two properties are given as parameters. A node with the first property must precede one with the second property
* **requires**: two properties are given as parameters. If a node with the first property exists, then a node with the second property must also exist.
* **excludes**: two properties are given as parameters. If a node with the first property exists, then no node with the second property can exist.

## Colors

The **colors** principle consists in the use of a color language to semi-automatize node unification during tree description solving. This idea has been proposed by B. Crabbé (see [[https://link.springer.com/chapter/10.1007/11424574_3|[Crabbé and
Duchier, 04]]]). The process is the following: 
 1.  we decorate nodes with colors (red, black or white),
 2.   the description solving is extended so that the nodes are unified according to specific color combination rules:
 
![alt text]({{ site.url }}/docs/assets/colors.png)

That is to say: 
a black node may be unified with 0, 1 or more white nodes and thus produces a black node, 
a white node has to be unified with a black one producing a black node, 
and eventually a red node cannot be merged with any other node. 
As a result, a satisfying model is a model where all the nodes are either black or red.

The important advantage of this color labelling process is that we do not need to explicitly specify all
the node unifications that have to be performed. Actually the saturation of colors will trigger these
unifications. In other words we can think of nodes in terms of "relative addresses". This means that we
do not have to manage node variables (which correspond to "absolute addresses") as the colors give a
way to refer to "mergeable" nodes, ie black nodes that can be unified (thus that can receive a fragment).
By lessening the use of variables, we prevent name conflicts and thus we can for instance easily reuse
the same tree fragment within the same tree description (this happens in TAG for trees with double
prepositional phrase).

To use the **colors** principle, the metagrammar must include the following declarations:

    use color with () dims (syn)
    type COLOR ={red,black,white}
    property color : COLOR

When the principle is used, every node needs to be affected a color (or to be unified with a node having one). As a reminder, giving such a property to a node is done as follows:

    node ?X (color=red)
    
## Rank

The **rank** principle is used to express linear orders between nodes which do not appear in the same classes. For example, in languages where there are strict constraints on the order of clitic pronouns, this principle makes the description task easier. The idea is to give a **rank** to every node representing a clitic, and this rank will make sure that if other clitics are added to the description (with their own ranks), they will be placed on the right side of this clitic. In a description where two nodes have respectively ranks 3 and 4, the only valid solutions will be the ones where the node with rank 3 precedes (not necessarily immediately) the one with rank 4.

Warning: the **rank** principle only applies to sister nodes. If two ranked nodes have different mother nodes, no linear precedence constraint will apply on them.

To use the **rank** principle, the metagrammar must include the following declarations:

    use rank with () dims (syn)
    type RANK = [X..Y]
    property rank : RANK

where `X` and `Y` and integers indicating the lowest and highest values for a rank. When the principle is used, nodes can be affected a rank. As a reminder, giving such a property to a node is done as follows:

    node ?X (rank=3)


## Unicity

To use the **unicity** principle, the metagrammar must include the following declaration:

    use unicity with (attribute=value) dims (syn)

where the pair of parameters (`attribute` and `value`) should only be seen in one node (at most) in every valid model. The principle `unicity` can be used either for features or properties. For example, with the following instance of the principle:

    use unicity with (rank=1) dims (syn)

models will be able to have only zero or one node of rank 1.

## Requires

To use the **requires** principle, the metagrammar must include the following declaration:

    use requires with ( attribute1= value1, attribute2=value2 ) dims (syn)


## Excludes

To use the **excludes** principle, the metagrammar must include the following declaration:

    use excludes with ( attribute1= value1, attribute2=value2 ) dims (syn)


## Precedes

To use the **precedes** principle, the metagrammar must include the following declaration:

    use precedes with ( attribute1= value1, attribute2=value2 ) dims (syn)


# Plugins

XMG-2 makes it possible for a user to create their own principles, without involving too much programming efforts. The solution provided to create these new principles is to use **plugins** for solvers. More documentation coming soon.

