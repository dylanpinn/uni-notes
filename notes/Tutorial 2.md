---
title: "Tutorial 2"
created: '2019-10-02T08:41:27Z'
modified: '2019-10-28T03:21:20Z'
tags: [Notebooks/Computer Science Degree, Tutorials]
---

# Tutorial 2

1. In the above𝒁-algorithm,foranygiveninputstringifwefoundthat𝒁𝟐=𝒒(where𝒒>𝟎),thenvaluesof𝒁𝟑,𝒁𝟒,…,𝒁𝒒+𝟏,𝒁𝒒+𝟐canbeimmediatelyobtainedwithoutadditionalcharactercomparisions.Reasonwhy?
```
IfZ2=q,weknowthatthefirstqcharactersofthestringmatchexactlytheqcharactersstartingfromk=2. That is str[1..q] = str[2..q+1].
```

 
```
This implies:
str[2] = str[1]
str[3] = str[2] (which we know above = str[1])
str[4] = str[3] = str[2] = str[1]
…
str[q+1] = str[q] = str[q-1] = … = str[1].
```

 
```
This implies that the first q+1 characters of the string repeat the same character, with q+2 being a different characters. For example:  "aaaaaaaaaab...".
```

 
```
Therefore, every Z[k] values for 2 < k <= q+1 can be inferred without explicit comparisons, as we know that this will simply be thelengthof the string str[k..q+1]. That is:
Z[3] = q-1
Z[4] = q-2
…
Z[q+1] = 1
```

 
```
Finally, by the definition of Z[2] = q, we know that the str[q+2] is different from (str[q+1] = str[1]). This implies Z[q+2] has to be 0.
```

2. When𝒁𝒌−𝒍+𝟏≥𝒓−𝒌+𝟏,thealgorithmdoesexplictcomparisionsuntilitfindsamismatch.Thisisareasonablewaytoorganisethealgorithm,butinfactCASE2bcanberefinedsoastoeliminateanunneededcharactercomparision.Arguethatwhen𝒁𝒌−𝒍+𝟏≥𝒓−𝒌+𝟏,then𝒁𝒌=𝒓−𝒌+𝟏,andhencenocharactercomparisionsarenecessary.Therefore,explicitcharactercomparisionsareneededonlyinthecasewhen𝒁𝒌−𝒍+𝟏=𝒓−𝒌+𝟏.
 
![zg-ĺ4ĺ-- 
zŕx 
r-k41 
z-bo, 
Z - box 
Q.ngus 
beyonĹl 
and I ](@attachment/395a17d2-7d2b-4ba4-a1f5-808723393980.png)3. Reason a potential linear-time solution for the following problem. Given two equal length strings⍺and β from a fixed alphabet, determine if⍺is a circular (or cyclic) rotation of β. For example, 
is a circular rotation of 
.
If⍺is a cyclic rotation of β, then:

⍺is of the form someprefix + someprefix

β is of the form someprefix + someprefix

 
So, to check if⍺is a cyclic rotation of β, double up the string β to get ββ which will be of the form:

ββ = someprefix + someprefix + someprefix + someprefix

 
This allows us to convert the problem into an exact pattern matching problem since we are searching for⍺of the form:

someprefix + someprefix

 
Thus, preform an exact pattern matching using Z-algorithm over⍺$ββ to find if⍺occurs in ββ. If so,⍺is a circular rotation of β.

4. Similar to the above exercise, give a linear-time algorithm to determine whether a linear string⍺is a SUBstring of a circular string β. Note: a circular string str[1..n] is such that the character str[n] precedes character str[1].
Can be addressed using the Z-box based solution to the previous question. For this problem, a circular string β can be of course represented in the form of ββ. But in the fact, you don't have to double up fully that string, and rather just append the prefix of β that is of the same size as⍺, and run the Z-algorithm as before.

5. Give an algorithm that takes in two strings⍺and β of lengths m and n, and finds the longest suffix of⍺that exactly matches a prefix of β. Reason the run-time of your algorithm.
If you concatenate β$⍺and run the Z-algorithm, the largest Z[k], for valuesk>b+1, such thatZk+k−1=|⍺|gives the longest suffix of⍺that exactly matchese the prefix of β.

 
Z-box computation takesO(⍺+𝛽)time. The subsequent scanning starting from postion𝛽+2to find the largest Z values that satisfied the above condition takesO(|⍺|)-time.

6. Given a string str[1..n], let len(i) denote the length of the largest suffix of str[i..n] that is also a prefix of str. Give an algorithm that computes len(i) values. Reason the run-time of your algorithm.
```
Recall that Z[i] gives the length of the longest substring that start form I, and matches with a prefix.
```

 
```
And compute Z-array over str[1..n].
```

 
```
Now, we are going to scan Z-array from right to left (from n to 1), checking each Z-value to see if the z-box at i reaches the end, we can find each len[i] value (as follows):
```

 
 
```
Time complexity:
Computation of Z-values takes O(n). Filling up len array in the reverse direction is also O(n). Total = O(n) time.
```

 
 
 
 
 
 
 
 
 
 
```
Let us first initialise len array of size n.
len = [0 0 0 … 0]
```

 
 
```
If Z[i]+i-1 = n:
Len[i] = Z[i] // found a new largest suffix matches a prefix
Else:
Len[i] = len[i+1] // otherwise, previously found value is occupied
```

 
