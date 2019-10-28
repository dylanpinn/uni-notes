---
tags: [FIT3155, Lectures, Notebooks/Computer Science Degree]
title: 'Lecture 1 & 2: Linear-time string pattern matching'
created: '2019-08-19T08:35:56.000Z'
modified: '2019-10-28T04:58:50.905Z'
---

# Lecture 1 & 2: Linear-time string pattern matching

## Outline

Linear-time pattern approaches to exact pattern matching problem on strings

* Gusfield's Z-algorithm
* Boyer-Moore's algorithm
* Knuth-Morris-Pratt's algorithm
## Gusfield'sZ-algorithm

* This is actually a linear-time algorithm topre-processa given string.
* After pre-processing, the output from this algorithm (Zi-values) can be used to address a versatile set of problems that arise in strings.
### Definingzi

For a stringstr[1..n], defineZi(for each positioni>1instr) as thelengthof thelongest substring starting at positioniofstrthatmatches its prefix(i.e.,str[i...i+Zi−1]=str[1...Zi]).

### Defining Zi-box

For a stringstr[1..n],and for anyi>1such thatZi>0, aZi-box is defined as the interval[i...i+Zi−1]ofstr.

### Definingri

For a stringstr[1..n], and for alli>1,riis theright-most endpointfor all Z-boxes that begin at or before positioni.

Alternately,riis the largest value ofj+Zj−1over all1<j≤i, such thatZj>0.

### Definingli

For a stringstr[1..n], and for alli>1,liis theleft endof the Z-box that ends atri.

* In the case there is more than on Z-box ending atri, thenlican be chosen to be the left end ofanyof those Z-boxes.
### Main point of Gusfield's Z-algorithm

* We have shown how to defineZi,Zi−box,li,ri
* The fundamentalpre-processing taskof Gusfield's Z-algorithm relies on computing these values, given some string, inlineartime.
* That is, for a stringstr[1..n], we would like to computeZi,Zi−box,li,rifor each positioni>1instrin O(n)-time.
### Overview of the linear-time pre-processing

* In this pre-processing phase, we computeZi,Zi−box,li,rivaluesfor each successive positioni, starting fromi=2.
* All successively computedZivalues are remembered.

    * Note: EachZi−boxvalues can be computed from its correspondingZivalue inO(1)time.

* At each iteration, to compute(li,ri), this pre-processing only needs values of(lj,rj)values are needed.

    * Note: no earlier(lj,rj)values are needed…
    * …so, temporary variables(l,r)can be used to keep track of the most recently computed(li−1,rr−1)values to update(li,ri).

### Base-Case

* To begin, computeZ2by explicitleft-to-rightcomparision of charactersstr[2…]withstr[1…]until amismatchis found.
* IfZ2>0

    * Setri.e.,r2=Z2+1
    * Setli.e.,l=2

* Else (i.e., ifZ2==0)

    * Setri.e.,r2=0
    * Setli.e.,l=0

### Inductive assumption and general cases

Assume inductively…

* …we have correctly computed the values ofZ2through toZk−1.
* ...thatrcurrently holdsrk−1,
* …thatlcurrently holdslk−1.
For computingZkat positionk, these two scenarios arise:

* Case 1: If 

* Case 2:Else 

### Case 1: 


* ComputeZkby explicitly comparing charactersstr[k…q−1]withstr[1…q−k]untilmismatchis found atsomeq≥k.
* IfZk>0:

    * Setri.e.,rk=q−1
    * Setli.e.,lk=k

* Otherwise they retain the samel,rvalues as before.
### Case 2: 


* The characterstr[k]lies in the substringstr[…r](i.e., withinZl−box).
* By the definition ofZl−box,strl…r=str[1…Zl].
* This implies the characterstr[k]is identical tostr[k−l+1].
* By extending this logic, it also implies that the substringstr[k…r]is identical tostr[k−1+1…Zl].
* But, in previous iterations, we already have computedKk−l+1value.
Case 2a, ifKk−l+1<r−k+1:

* SetZktoZk−l+1.
* randlremainunchanged.
Case 2b, ifZk−l+1≥r−k+1:

* Zkmust also be≥r−k+1
* So, start explicitly comparingstr[r+1]withstr[r−k+2]and so on until mismatch occurs.
* Say the mismatch occurred at position 
, then:

    * SetZktoq−k,
    * Set 
to 
.
    * Set 
to 
.

### Pre-processing (ofZivalues) forstr[1…n]takesO(n)time

* Total time is directly proportion to the SUM of:

    1. The number of iterations
    2. The number of character comparisons (matchesormismatches)

* This pre-processing has 
iterations.
* The number ofmatchesandmismatchesare both at most 
, because :

    * rk≥rk−1(for all iterations)
    * Update torkis of the formrk=rk−1+𝛿(where𝛿≥0).
    * Butrk≤n.
    * Thus, there are at most 
matchesor mismatches.

### UsingZ−algorithmfor linear-time exact pattern matching.

#### Recall the exact pattern matching problem

Given a reference texttxt[1…n]and a patternpat[1…m], findALLoccurrences, if any, ofpatintxt.

#### Realising a linear-time solution using Gusfield'sZ−algorithm/pre-processing

* Construct a new string 
by concatenation as follows:

    * str=pat1…m+$+txt[1…n].
    * Note,str=m+1+n.

* Pre-processZivalues corresponding tostr, for1<i≤m+n+1.
* For anyi>m+1, allZi=midentifies an occurrence ofpat[1…m]at positioniinstr, and hence at positioni−(m+1)intxt. That is,pat1…m=(str1…i+m−1≡txti−m+1…i−2).
* We already, showed that the computation ofZivalues for any stringstrtakesO(str)time).
* Thus, this pattern matching algorithm takes 
time. 

 
## Boyer-Moore Algorithm

Boyer-Moore algorithm incorporate three clever ideas:

1. Right-to-left scanning
2. Bad character shift rule
3. Good suffix shift rule
Caveat: An additional rule, termed in the field as theGalil's optimisation, ensures linear runtime across all possible scenarios oftxtandpat.

 
### Right-to-left scan

For any comparison ofpat[1..m]againsttxt[j…j+m-1], the Boyer-Moore algorithm checks/scans for matched charactersrightto left (instead of the normal left to right scan, as in the naïve algorithm).

![Example: right to left scanning (in some arbitrary ite 
123456789 10 11 12 13 14 15 16 17 
txt: x p b ct b x a b p q x c t b p q 
1234567 
tpabxab 
pat: 
Scanning from right to left in the above example: 
Compare pat[7] b with txt[9] b: match. 
Compare pat[6] E- a with txt[8] 
E- a: match. ](@attachment/fee9f3ad-3d23-4740-9771-ad2ccaf10ac0.png)So, after amismatchduring right-to-left scanning, to avoid naively shiftingpatrightwards by1 position, BM algorithm employs two additional ideas/tricks discussed below.

 
### Bad character shift rule

* Scanning right-to-left, we found a mismatch comparingpat3≡awithtxt5≡t.
* But the rightmost occurrence in the entirepatof the mismatched character intxt(i.e.txt5≡t) is at position 1 ofpat(i.e.,pat1≡t).
* So, in this case, once case safely shiftpatbytwo placesto the right as to match characterspat1≡tandtxt5≡t(instead of naively shifting by only one place).
 
#### Formalising 'Bad character' shift rule

* Letpat[1..m]andtxt[1..n]be strings from the alphabet𝒩.
* Pre-processpat,and store for each character 
, the rightmost position of occurrence of characer 
inpat.

    * Call this position, 
.
    * Note,Rx=0, whenxdoes not occur inpat.

* These pre-processed 
values will be used (for > 1 position shifts ofpatundertxt, where possible) as we will see in the slides to follow.
Note: storingR(x)values forpatrequires at mostO(𝒩)space, and one lookup per mismatch.

![There is still one problem, 
The current rule will only 
1 
txt 
when R(x) is between k+l and m. 
allow shift by 1 position: max(l 
mismatched 
characters 
x 
This scenaric ](@attachment/8c8dd449-0eb6-4baa-892d-c6b8b591cd19.png)* In some iteration of this algorithm, let's say 
and 
are being compared via right-to-left scan.
* Let the 
th character oftxtcounting from position 
, i.e. 
, bemismatchedwith the 
th character of the pattern 
.
* Shift/Jump RULE:Then, the bad-character shift rule asks us to shift rightwards thepatalong thetxtbymax⁡{1,k−Rx}positions.
* This implies, ifxdoes not occur inpat1…m(Rx=0), then the entirepatcan be shifted one position past the point ofmismatchintxt.
 
#### Extended Bad-Character Rule

When amismatchoccurs at some positionkinpat[1…m], and the correspondingmismatchedcharacter is 
, thenshift 
to the right so thatthe closest x in pat is to the left of position kis now below the (previouslymismatched)xintxt.

* To achieve this, pre-processpat[1…m]so that, for each position 
inpat, and for each character 
, the position of the closest occurrence of 
to the left of each position 
can be efficiently looked up.
* A 2D (shift/jump table)of size 
can store this information.
 
### Good Suffix Rule

* In some iteration, say 
and 
are being compared via right-to-left scan.
* Let thekth character oftxt, i.e.x=txt[j+k−1], be mismatched with thekth chracter of the patterny=patk.
* If we knew that 
is the rightmost position inpatwhere the longest substring (of length 
ending at position 
matches its suffix, that is:

    * patp−m+k+1…p≡patk+1…m
    * patp−m+k≠pat[k].

* Then,patcan be safely shifted right by 
positions,
* And a new iteration can be restarted.
 
patp−m+k+1…p≡patk+1…m 
#### Ideas to implement the 'good suffix' rule more efficiently

To efficiently implement the 'good suffix' rule, we take 'inspiration' from the computation ofZivalues in Gusfield's algorithm, and defineZisuffix(specifically onpat) as follows:

Definition ofZisuffix:Givenapat[1…m],defineZisuffixfor (each positioni<m) as thelengthof thelongest substring ending at positioniofpatthat matches itssuffix(i.e.,pati−Zisuffix+1…i=pat[m−Zisuffix+1…m]).

* Note, computation ofZisuffixvalues onpatcorresponds to the computation ofZivalues onreverse(pat).
* Thus,Zisuffixvalues can be computed inO(m)time forpat[1…m].
In fact, for eachsuffixstarting at position 
inpat, we want to score the rightmost position 
inpatsuch that:

patj..m≡patp−Zisuffix+1..p* patj..m≡patp−Zisuffix+1..p
* patj−1≠pat[p−Zisuffix].
Store these rightmost positions asgoodsuffixj=p.These can be computed as:

m := |pat|

forjfrom1to m+1do

goodsuffix(j) :=0

endfor

forpfrom1to m-1do

j := m - Zˆ{suffix}_p +1

goodsuffix(j) := p

endfor

#### Using 'good suffix' rule during search

In any iteration, to use the 'good suffix' rule, the following cases have to handled:

![la: 
1 
txt 
pat 
Case 
1 
goodsuffix(k+l) > O 
mismatched 
characters 
x 
goodsuffix(k+l) = p 
m ](@attachment/c83b7580-53e1-4d7e-a226-4ba26b5b54d5.png) 
Case 1a:if a mismatch occurs at some 
,and 
then

* Shiftpatby 
places.
Case 1b:if a mismatch occurs at somepat[k], andgoodsuffixk+1=0then

* Shiftpatby 
places

    * matchedprefix(k+1)denotes the length of thelargest suffix ofpat[k+1..m]that is identical to theprefix ofpat1..m−k.
    * matchedprefix(.)values forpatcan be precomputed using Z-algorithm inO(m)time.

Case 2:when 
fully matches 


* Shiftpatby 
places.
 
### Bringing all pieces together

Pre-processing step

* Pre-processpat…

    * … for jump tables (e.g. 
values) needed for 'bad-character' shifts
    * … for 
and 
values needed for 'good suffix' shifts.

Algorithm

* Starting with 
vs. 
, in each iteration, scan'right-to-left'.
* Use (extended)bad-characterrule to find how many places to the rightpatshould be shifted undertxt. Call this amountnbadcharacter.
* Usegood-suffixrule to find how many places to the rightpatshould be shifted undertxt. Call this amountngoodsuffix.
* Shiftpatto the right undertxtbymax⁡(nbadcharacter,ngoodsuffix)places.
 
### Galil's optimisation to ensure linear runtime always

* Suppose, in some iteration, we are comparing 
with 
, viaright-to-leftscanning.
* Saypatk≠txt[j+k−1](or even say the entirepatmatches intxt)…
* … in the next iteration (after applying some appropriate shift)…
* … if the left end ofpat[1]lies between 
…
* … then there is definitely a prefixpat[1…]thatmatches 
, which need not be explicitly compared, after this shift.
* Thus, in the next iteration, the right-to-left scanning can stop prematurely if positiontxt[j+m−1]is reached, to conclude there tis an occurrence ofpatintxt.
* Employing Galil's optimisation during shifting between iterations, the Boyer-Moore algorithm guaranteesworst-case time-complexityof 
.
 
## Knuth-Morris-Pratt (KMP) Algorithm

### DefiningSPivalues forpat[1…m]

Definition ofSPi: Given a patternpat[1…m], defineSPi(for each positioniinpat) as thelengthof thelongest proper suffixofpat[1…i]thatmatches a prefixofpat, such thatpati+1≠pat[SPi+1].

![Example 
1 2 3 4 5 6 7 8 9 19 11 12 
Pat= b b c ca e b bc abd 
O SPF 1 SPF O 
SPI 
SPI ](@attachment/bd11c284-3671-49f6-936f-c48c101b2e57.png)m := |pat|

forifrom1to mdo

SP_i :=0

endfor

forjfromm down to2do

i := j + Z_j -1

SP_i := Z_j

endfor

 
### KMP Algorithm overview

* KMP algorithm is described in terms of theSPivalues.
* The general procedure/iteration of KMP involves:

    * Compare 
against any region of 
in thenaturalleft-to-right direction.
    * In the firstmismatch, while scanning left-to-right, occurs at posi+i: that is,pat1…i≡txt[j…j+i−1]

        * Shiftpatto the right (relative totxt) so that …
        * pat1…SPiis now aligned withtxt[j+i−SPi…j+i−1]
        * KMPshift rulein other words, shiftpatby exactlyi−SPiplaces to the right.

    * Else, in the case of occurrence ofpatis found intxt(i.e., no mismatch), then shiftpatbym−SPmplaces.

## Summary

* Naïve algorithm takes 
-time
* Gusfield's Z-algorithm guaranteed in 
-time, worst case.
* Boyer-Moore's algorithm takes

    * O(n+m)-time worst case …
    * … butO(mn)-time (sublinear) in most 'realworld' usage.

* Knuth-Morris-Pratt algorithm also takes 
-time worst-case, but inferior in performance to Boyer-Moore in practice.
 
 
