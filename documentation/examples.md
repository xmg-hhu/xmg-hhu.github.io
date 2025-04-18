---
layout: default
title: Examples
parent: Documentation
permalink: /documentation/examples

nav_order: 5

---

# Simple TAG example

Now, we will see in details how to write a metagrammar. We will define a metagrammar generating a
small TAG for French. This small TAG will contain 2 trees, namely the ones representing a transitive
verb either with a canonical subject or a subject in relative position.

## Specifying data

 
First thing to do: defining the principles, types, properties and features we will use. For the sake of clarity,
we will only constraint the produced trees to have no duplicate grammatical function. That is to say,
we will only activate the unicity principle with the gf property as parameter:

    use unicity with (gf = subj) dims (syn)
    use unicity with (gf = obj) dims (syn)

We will deal with few types in this example. We only pay attention to grammatical functions and syntactic categories. The first one is a node property and the second one a node feature (ie part of the TAG formalism):

    type CAT = {n,v,s}
    type GF = {subj, obj}
    property gf : GF
    feature cat : CAT

## Defining blocs (tree fragments)

The metagrammatical rule we will use is the following:

    transitive = (CanSubject | RelSubject) ; Active ; Object 

So we will handle 4 tree fragments: Active, CanSubject, Object, and RelSubject. The class transitive will consist of an abstraction on a conjunctive combination including a disjunction on the subject that is used.
The Active class corresponds to the verbal spine: 

![alt text]({{ site.url }}/docs/assets/active.png)

    class Active
    export ?X ?Y
    declare ?X ?Y
    {`<syn>`{
        ?X -> ?Y
        }
    }

The CanSubject class corresponds to the Example class introduced previously:

![alt text]({{ site.url }}/docs/assets/cansubj.png)

    class CanSubject
    export ?X ?Y ?Z
    declare ?X ?Y ?Z
    { `<syn>`{
        node ?X [cat = s]{
                node ?Y (gf=subj)[cat=n]
                node ?Z [cat = v]
                }
        }
    }

The Object class is the symetric class of CanSubject:

![alt text]({{ site.url }}/docs/assets/obj.png)


    class Object
    export ?X ?Y ?Z
    declare ?X ?Y ?Z
    { `<syn>`{
        node ?X [cat = s]{
                node ?Y [cat = v]
                node ?Z (gf=obj)[cat=n]
                }
        }
    }

The RelSubject class and its concrete syntax are given below:

![alt text]({{ site.url }}/docs/assets/relsubj.png)

    class RelSubject
    export ?X ?Y ?Z
    declare ?X ?Y ?Z ?U ?V
    { `<syn>`{
        node ?U [cat = n]{
                node ?V [cat = n]
                node ?X [cat = s]{
                        node ?Y (gf=subj)[cat=n]
                        node ?Z [cat = v]
                        }
                }
        }
    }

At this point, we may wonder why associating variables to nodes ? The answer is that we still have to
merge these fragments, we will use the exported variables to unify specific nodes.

## From tree fragments to trees

 
Once the basic blocs have been defined, we can combine them to produce the expected trees. We define the transitive class:

    class transitive
    declare ?SU ?OB ?AC
    {
        ?SU = {CanSubject[] | RelSubject[]} ; ?OB = Object[]  ; ?AC = Active[] ;
        ?SU.?X = ?OB.?X ; ?SU.?Z = ?OB.?Y ; ?SU.?X = ?AC.?X ;
        ?SU.?Z = ?AC.?Y 
    }

In this class, we use the dot operator to associate a variable to the record of exported identifiers. For
instance, ?OB being the variable representing the Object class, ?OB.?X refers to the ?X variable of this class, provided it has been exported. In the transitive class we combine conjunctively 3 classes (one being either CanSubject or RelSubject, and Object, and Active). We also unify their s and v nodes so that the tree fragments get merged. Note that we may prefer using a color system to semi-automatize this node unification (see Controlling fragment combination semi automatically by coloring nodes).
Eventually, we know that the  transitive class contains all the information needed to build 2 TAG
trees. So we ask for its evaluation by invoking:


    value transitive

As a result we obtain the 2 following trees (the first one represents the relative subject, and the second
one the canonical subject) :

![alt text]({{ site.url }}/docs/assets/solution_1.png)
    
![alt text]({{ site.url }}/docs/assets/solution_2.png)
    
## The whole metagrammar


    use unicity with (gf = subj) dims (syn)
    use unicity with (gf = obj) dims (syn)
    type CAT = {n,v,s}
    type GF = {subj, obj}
    property gf : GF
    feature cat : CAT
    
    class Active
    export ?X ?Y
    declare ?X ?Y
    {`<syn>`{
        ?X -> ?Y
        }
    }
    
    class CanSubject
    export ?X ?Y ?Z
    declare ?X ?Y ?Z
    { `<syn>`{
        node ?X [cat = s]{
                node ?Y (gf=subj)[cat=n]
                node ?Z [cat = v]
                }
        }
    }
    
    class Object
    export ?X ?Y ?Z
    declare ?X ?Y ?Z
    { `<syn>`{
        node ?X [cat = s]{
                node ?Y [cat = v]
                node ?Z (gf=obj)[cat=n]
                }
        }
    }
    
    class RelSubject
    export ?X ?Y ?Z
    declare ?X ?Y ?Z ?U ?V
    { `<syn>`{
        node ?U [cat = n]{
                node ?V [cat = n]
                node ?X [cat = s]{
                        node ?Y (gf=subj)[cat=n]
                        node ?Z [cat = v]
                        }
                }
        }
    }
    
    class transitive
    declare ?SU ?OB ?AC
    {
        { ?SU=CanSubject[] | ?SU=RelSubject[] } ; ?OB = Object[]  ; ?AC = Active[] ;
        ?SU.?X = ?OB.?X ; ?SU.?Z = ?OB.?Y ; ?SU.?X = ?AC.?X ;
        ?SU.?Z = ?AC.?Y 
    }
    
    value transitive
    
#  More examples

More examples can be found in the `Metagrammars` folder of the XMG installation directory. Some grammars are also available on the [resources](/resources) page.

