# Mathematical sets

#maths #sets #cantor/georg #notation #mathjax

----- 

[[discrete-mathematics-with-applications-epp-]] p.7

The word *set* as a formal mathematical term was first
introduced in 1879 by Georg Cantor (1845-1918)

For most purposes, a set is a collection of elements.

If _C_ is a collection of all counties in the United Kingdom, then
_Lincolnshire_ is an element of _C_.



## Set Notation

To show that *x* is an element of *S*: 

$$x \in S$$



To show that *x* is not an element of *S*:

$$x \notin S$$


### Set-Roster Notation

To denote a set whose elements are *1, 2, and 3*:

$$\lbrace 1,2,3 \rbrace$$


To denote a set whose elements are in a range of *1 to 100*:

$$\lbrace 1,2,3...,100 \rbrace$$


To denote an infinite set of positive integers:

$$\lbrace 1,2,3... \rbrace$$


## Axiom of extension

The **axiom of extension** says that the elements of a set completely
define that set, not the order in which the elements are listed or 
multiple occurrences of the same element.

Therefore the 3 sets denoted below ($A$, $B$ and $C$) are different ways 
to represent the same set:

$A = \lbrace 1,2,3 \rbrace$

$B = \lbrace 2,3,1 \rbrace$ 

$C = \lbrace 1,1,2,3,3,3 \rbrace$


## Set-Builder Notation 

A set can also be specified with set-builder notation.

In this example let _S_ denote a set and _P(x)_ be a property that
may or may not be satisfied by the elements of _S_.

We can then define a _new set_ to be the set of all elements _x_ in _S_ such 
that _P(x)_ is true.

$$\{x \in S | P(x) \}$$

We might instead write the same notation without being specific about 
where _x_ comes from. 

$$\{x | P(x) \}$$

Think of the pipe as meaning "such that" in this context. 

More examples:

| Example | Meaning |
| ------- | ------- |
| $\{x \in R \| -2 < x < 5 \}$ | real numbers between -2 and 5 |
| $\{x \in Z \| -2 < x < 5 \}$ | integers between -2 and 5 |
| $\{x \in Z^+ \| -2 < x < 5 \}$ | positive integers between -2 and -5, i.e. *1, 2, 3, 4* |




## Frequently used sets

Some sets are common enough to have special symbolic names.

| Symbol | Set |
| ------ | --- |
| $R$ | the set of all real numbers |
| $Z$ | the set of all integers |
| $Q$ | the set of all rational numbers, or quotients of integers |


Additional superscript:

| Superscript | Meaning |
| ----------- | ------- |
| $+$ | only the positive elements of the set |
| $-$ | only the negative elements of the set | 
| $\textit{nonneg}$ | only the non-negative elements of the set |

Examples:

| Example | Meaning | 
| ------- | ------- | 
| $R^+$ | all positive real numbers |
| $Z^{\textit{nonneg}}$ | all non-negative integers |
 

## Subsets 

Set $A$ is a subset of set $B$ if, and only if, every element of $A$ is also
an element of $B$.

$$A \subseteq B$$

You can say "A is contained in B" or "B contains A" instead.

Conversely, if set $A$ is not a subset of set $B$ we denote this as:

$$A \subsetneq B$$ 

Which can be taken to mean that there is at least one element *x* such that $x \in A$ and $x \notin B$

If every element of $A$ is in $B$, but there is at least one element of $B$ 
that is not in $A$ this is known as a **proper subset**.



