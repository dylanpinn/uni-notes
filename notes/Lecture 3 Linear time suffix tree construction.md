---
tags: [FIT3155, Lectures, Notebooks/Computer Science Degree]
title: 'Lecture 3: Linear time suffix tree construction'
created: '2019-08-20T10:24:27.000Z'
modified: '2019-10-28T05:04:07.846Z'
---

# Lecture 3: Linear time suffix tree construction

## What is covered in this lecture?

Linear-time suffix tree construction

* Ukkonen algorithm (suffix tree construction)
 
## Introduction

The substring (matching) problem

Given a reference text `txt[1...n]`, preprocess `txt` such that any given pattern `pat[1...m]` can be searched in linear time proportional to the length of the pattern, $O(m)$.

* Suffix trees (and similarly suffix arrays) permit solving the above (and many other related) problems. They are very versatile.
* Suffix trees unravel the composition of any string, and permit efficient access to them.
 
## String definitions

Given
* A `prefix` of `str[1..n]` is a **substring** `str[1..j]`, $∀1≤j≤n$.
* A `suffix` of `str[1..n]` is a **substring** `str[i..n]`, $∀1≤i≤n$.
* A `substring` of `str[1..n]` is any `str[j..i]`, $∀1≤j≤i≤n$.
* Therefore,
    * A substring is a **prefix of a suffix**
    * (or equivalently) a substring is a **suffix of a prefix**
 
## Efficient Suffix Tree Construction usingUkkonen'salgorithm

### Recall from FIT2004:

#### Path compressed suffix tries = suffix trees

![Suffix Trie of abaaba$ 
Suffix T 
aba ](@attachment/7bb5a489-7b28-456b-a034-046aec3cb43c.png)#### Efficient representation of suffix trees requires O(n) space

![Suffix Tree of 
aba $ 
abaaba $ 
$ aba $ 
Suffix Tree 
(7 
(7,7) 
(4,7) 
(7, ](@attachment/22a93d55-d1e1-4574-86d7-aee74dfbbda2.png)Note, instead of storing the edge labels as substringsexplicitly, we can store themimplicitlyusing(j,i)denoting the substringstr[j..i], where1≤j≤i≤n.

 
### Building a suffix treenaively

![5 
Consider suffixes of str 
3 
C 
4 
a 
6 
SUFFIX 
SUFFIX 
SUFFIX 
SUFFIX 
1: 
2: 
3: 
4: 
str[l.. 6] 
str[2..6] 
str[3..6] 
str[4..6] 
b 
c ](@attachment/c8f462cf-deac-40b6-b696-18be370d433a.png) 
1. Start with the (empty) root node of the suffix tree:r
2. Insert suffix1( 
into an empty tree.

    1. Call the resultant treeT1

3. Insert suffix2intoT1

    1. Note 
is not a prefix of 

    2. So create a new leaf node forstr2…suffix, branching of atr
    3. Call the resultant treeT2

4. Insert suffix3intoT2

    1. Note 
is not a prefix of 
or 

    2. So create a new leaf node for 
suffix, again branching of atr
    3. Call the resultant treeT3

5. Insert suffix4intoT3

    1. Note 
is the longest common prefix shared with the suffix 

    2. So, a new nodeuis inserted

        1. Along the edge betweenrand the leaf node1
        2. With another edge branching offuto the new leaf node4

    3. Call the resultant treeT4

6. Insert suffix5intoT4

    1. Note 
is the longest common prefix shared with the suffix 

    2. So a new nodevis inserted

        1. Along the edge betweenrand the leaf node2
        2. With another edge branching out fromvto the new leaf node5

    3. Call the resultant treeT5

7. Insert suffix6intoT5

    1. Node the suffixstr6..6denotes the sepecial terminal character $
    2. This creates a new isolated edge branching off the rootr

![oeq$ ](@attachment/52308b45-25d6-40b7-9b73-7459000092ce.png) 
### Ukkonen's linear-time algorithm -- introduction

Ukkonen's algorithm uses the following main ideas:

1. Construct and use an 'implicit suffix tree' data structure

    1. The actual suffix tree is computed after iteratively constructing (over many phases) an implicit suffix tree.

2. Enhance this implicit suffix tree using 'suffix links'.

    1. This helps make the traversals on the implicit tree much faster.

3. Gain from a set of implementational 'tricks':

    1. These tricks avoid unnecessary computations, thus speeding up the algorithm drastically.

 
### Implicit suffix trees

The relationship between an implicit suffix tree and its regular suffix tree can be understood by the following operations on the regular suffix tree:

* Start with a regular suffix tree (of str = a b c a b).
* Remove all terminal ($) characters in the regular suffix tree.
* Then, remove all edges without edge labels (i.e. substrings)
* Then, path compress the tree by removing all nodes that do not have at least two children.
![Regular suffix tree 
cabs 
Implicit suffix tree 
3 
remove terminal 
'$' symbol 
path compress 
edges ](@attachment/d6d71283-bf70-445a-ae0e-b28dfb965e31.png) 
 
### Ukkonen's algorithm builds implicit suffix trees incrementally in phases

Given a string 
, Ukkonen's algorithm proceeds overn"phases"

* Eachphasei+1(where1≤i+1≤n) builds theimplictsuffix tree (denoted byimplicitSTi+1) for theprefixstr[1..i+1].
* Importantly, eachimplicitSTi+1is incrementally computed using theimplicitSTifrom the previous phase.

    * The construction ofimplicitSTi+1fromimplicitSTi, in turn involves severalsuffix extensionsteps, one for each suffix of the formstr[j..i+1], wherej=1…i+1in that order.

 
### Each phase involves suffix extensions

* In any phasei+1, the suffixes inimplicitSTi(from the previous phasei) undergosuffix extensionsto accommondate theadditional character,stri+1
* Thus, extending any suffixj, where 
, in the current phase 
involves:

    * Finding the path from the root nodercorresponding to the suffix 
, and
    * Extending 
by appending 
to the suffix.

 
### Algorithm at a very high level

 
* ConstructimplicitSTi
* ```
For i from 1 to n - 1
```

    * ```
BEGIN PHASE 

```

        * ```
For j from 1 to i + 1
```

            * Begin SUFFIX EXTENSION j
            * Find end of path from root denoting str[j..i] in the current state of the suffix tree.
            * Apply one of the three suffix extension rules

        * End of extension step j

    * End of phasei+ 1 (implicitSTicomputed).

 
### Suffix extension rules

#### Rule 1 of 3

If the pathstrj..iinimplicitSTiends at a leaf, adjsut the label of the edge to that leaft to account for the added characterstr[i+1].

![Before suffix extension 
str[ 
str[j+ 
str[j+ 
str[ 
str[k 
str[k+l 
After suffix 
str[k 
str[k+ ](@attachment/6fdf29e9-29cb-43cd-9564-b9a9c8bec647.png) 
#### Rule 2 of 3

If the pathstr[j..i]inimplicitSTidoes NOTend at a leaf, and the next chracter in the existing path is somex≠str[i+1], then split the ege afterstr[..i]and create a new nodeu, followed by a new leaf numberedj; assign characterstr[i+1]as the edge label betwene the new nodeuand leafj.

![Before suffix extension 
str[ 
str[j+ 
str[j+ 
str[k-l 
str[k] 
str[ k+l] 
str[i] 
After suffix 
cest r 
str[ ](@attachment/13dfb3b3-15d3-40b1-9854-41c664b65200.png)#### Rule 2 of 3 - an alternative scenario that can arise

If the pathstr[j..i]inimplicitSTidoes NOTend at a leaf, and the next chracter in the existing path is somex≠str[i+1], then split the ege afterstr[..i],andstr[i]andxare seperated by an existing nodeu, and create a new nodeu, followed by a new leaf numberedj; assign characterstr[i+1]as the edge label betwene the new nodeuand leafj.

![Before suffix extension 
str[ 
str[j+ 
str[j+ 
st r [i] 
After suffix ](@attachment/e832bcea-7c30-47a1-b7f8-12179569121b.png) 
#### Rule 3 of 3

If the pathstrj..iinimplicitSTidoes NOTend at a leaf, but is within some edge label, and the next character in that pathisstr[i+1], thenstr[i+1]is already in the tree.No further action needed.

![str[ 
str[j+ 
str[j+ 
str[k-l 
str[k] 
st r [ k+l] 
st r [i] ](@attachment/99585795-da9e-475f-9d07-bbfe2dc7365d.png)#### Example

![1234 
For the string str=a b b a , the implicit suffix trees for 
with their corresponding suffix extensions are shown belc 
6 
1 
Phase I 
path 
str[l..l] 
str[1..2] 
str[2..2] 
str[1..3] 
str[2..3 
1 
2 
Phase 2 
2 
1 
Phase 3 
omment 
1 
3 
Phase 4 
phase 
1 
2 
2 
3 
3 
extn 
1 
1 
2 
1 
2 
rule 
rule-2 
rule-I 
rule-2 
rule-I 
rule-I 
branch out of r into a new leaf 1 with 
xtend edge label between nodes r 
branch of r into a new leaf 2 with 
xtend edge label between nodes r 
xtend edge label between nodes r 
an 
lab 
an 
an ](@attachment/dfa1dee9-84ba-4443-a175-8ff2ce101d66.png) 
### Speeding up tree traversal usingsuffix links

Suffix links are simplypointersbetween internal nodes of an (implicit) suffix tree, that speed up traversal time in each phase.

 
#### Definition of a suffix link

* Letuandvbe two internal nodes of an implicit suffix tree.
* Let the traversal from root nodertouyield some substring 

* Let the traversal from root nodertovyield a substring 
.
* Then the pointer fromutovdefines a suffix link between those nodes.
 
#### Example

![12345678 
str=a a b b a b a a ](@attachment/236d11be-c10a-43e5-9f20-599e1edc18d0.png) 
### KEY OBSERVATION:

Every internal node of an implicit suffix tree has a suffix linkfromit.

* If, in some suffix extension 
of phase 
, a new internal nodeuis added to the current state of the implict suffix tree.

    * i.e., rule2of the suffix extension rules is applicable here.

* This means beforeuis added, the path 
is continued by a character (say 
), where 

* This implies, in the next suffix extension 
of the same phase 
:

    * Eitherthe path 
continuesONLY VIAcharacter 
.

        * Which implies, a new internal nodevmust be added, after 
, that branches to the new leaf 
via character 
.

    * Orthe path 
already ends in anexistinginternal nodev

        * With one branch below extending via character 

        * And one (or more) branch(es), via character(s) 



* Thus, new suffix linkutovwill be created in 
extension.
 
### Following the trail of suffix links to buildimplictSTi+1fromimplicitSTi

Recall that in the extension 
of phase 
the algorithm locates suffix 
, and extends it by 
, for each 
increasing from 1 to 
. Suffix links are used to speed these extensions.

 
#### Extension 1, phase 


* This first suffix represents the full prefix 
considered in this phase.
* We have to locate first the suffix 
.
* Note, suffixstr[1…i]is the longest string represented in the implict suffix treeimplicitSTi(from whichimplicitSTi+1is being computed).
* Letptrbe a pointer to the leaf containingstr[1..i]inimplicitSTi.
* Givenptr, the extension from 
to 
can be handled in constant time.
![str[ 
str[2] 
str[3] 
str[k-l 
str[k 
• str[k+ 
str[i ](@attachment/56ec5fba-f301-4dfb-8214-4808194e78e1.png) 
#### Extension 2, phase 


* Consider the edge ending at leaf 1 (after extension 1).
* Letube the internal node forming the edge with leaf 1.
* Let the substring corresponding to that edge be 

* In this extension, to find 
(and extend to 
):

    * Walk up EXACTLY ONE EDGE from leaf 1 tou.
    * Fromu, follows its suffix link tov.
    * Fromv, walk down along the path 

    * Then apply the pertinent suffix extension rule, by walking down fromvand then appending 
.

 
### General extension procedure for phase 


For any extension 
of phase 
repeats the same general idea

1. Fin the first nodeuAT OR ABOVEthe end of 


    * i.e., backtrackingAT MOST one edgefrom the end of 

    * Let the substring 
(possibily empty) denote the edge label betweenuand end of 


2. Ifu≠r, traverse the suffix link fromutov. Walk down fromvalong the path dictated by the substringstrk..i.

    * On the other hand, ifu=r, then no choice but to naively follow traverse the path from 
fromr.

3. Once at the desired point (i.e., end of 
), apply pertinent suffix extension rules.
4. Repeat items 1-3 above until each suffixstr[j..i+1](of the prefixstr[1..i+1]), for1≤j≤i+1is inimplicitSTi+1.
5. During these extensions, any new internal nodeucreated in extension 
, gets its suffix link to it corresponding nodevin the next extensionj.
