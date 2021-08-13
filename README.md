# Next-neighbours
An open problem concerning next neighbours in a planar graph

## Introduction

"Next neighbours" is a project related to graphs and plane geometry. The aim of the project is to discuss an open problem which will be named "Next Neighbour Problem".

The Next Neighbour Problem has a simple explanation in real world terms. Consider a village where, on a certain day, everyone visits her or his next neighbour.

To make things easier we will think of `n` houses with pairwise different distances.

For most arrangements of houses there will be some villagers who do not receive visitors.

What about the number `m` of visited houses? There is an infinite number of possible arrangements of the `n` houses. So (for `n>3`) different `m` will appear in these arrangements. It is easy to establish the maximum for `m` and not so easy to establish the minimum for `m`. In fact, the latter is an open problem (as far as the author knows).

![neighbours_figure_01](https://user-images.githubusercontent.com/88709288/129047431-3f63be39-9f12-48c1-9bfa-b7570456d4eb.png)

Figure 1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;n=10, m=5

Here is Mathematica code for a random arrangement of houses:

```
(* Neighbours random *)

t = Table[{Random[], Random[]}, {i, 16}];

n = Length[t];

td = Table[
   ReplacePart[Table[EuclideanDistance[t[[i]], t[[j]]], {j, n}], 
    i -> 2.], {i, n}];
    
Print["Test for pairwise different distances (should be 0):  ", 
 Length[Union[Drop[Sort[Flatten[td]], -n]]] - Binomial[n, 2]]
 
tdmin = Table[
   Min[ReplacePart[Table[EuclideanDistance[t[[i]], t[[j]]], {j, n}], 
     i -> 2.]], {i, n}];
     
tposition = 
  Union[Flatten[Table[Position[td[[i]], tdmin[[i]]], {i, n}]]];

t2 = Table[t[[tposition[[m]]]], {m, Length[tposition]}];

t1 = Complement[t, t2];

Print["Visited  ", Length[t2], "  ", t2]

Print["Not visited  ", Length[t1], "  ", t1]

Show[ListPlot[t1, PlotStyle -> {Black, PointSize[Large]}], 
 ListPlot[t2, PlotStyle -> {Green, PointSize[Large]}], 
 PlotRange -> {{0, 1}, {0, 1}}, AspectRatio -> 1, AxesOrigin -> {0, 0}]
 ``` 
 
Here is the output:

```
Test for pairwise different distances (should be 0):  0

Visited  10  {{0.136873,0.662183},{0.77065,0.908998},{0.099012,0.644006},{0.0747849,0.711588},{0.417849,0.475834},{0.466013,0.146717},{0.494827,0.421237},{0.418441,0.00420624},{0.914883,0.921726},{0.18339,0.548094}}

Not visited  6  {{0.0452213,0.368721},{0.0981768,0.793929},{0.144233,0.0127273},{0.281567,0.342023},{0.522212,0.630813},{0.850147,0.131827}}
```
 
Here is the Mathematica plot:
 
![neighbours_figure_02](https://user-images.githubusercontent.com/88709288/129056422-5dc3d0be-65c3-4498-8d13-d67dffa7c151.png)

Figure 2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;n=16, m=10


## Mathematical description of the problem and some proofs

Let `G` be a simple digraph; the vertices of `G` are `n` arbitrary placed points in `R^2` with pairwise different distances; the edges of `G` are arrows joining each point (tail end) to its nearest neighbour (head end).

Only `n>1` makes sense. We call a point *visited* if it is a head end in `G`, else *unvisited*. Every point is a *visitor* as it *visits* one head end. We call `(n,m)` a **_working pair_** if we can present an arrangement of `n` points with `m` points unvisited. We get a first result for working pairs:

**(1)&nbsp;&nbsp;&nbsp;`m>1`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;** and **&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`n` odd&nbsp;&nbsp;&rArr;&nbsp;&nbsp;`m<n`**

This can easily been proved. `m=0` and `m=1` are discarded because the two points with minimal distance visit each other.  —  What about `m=n`?  This is obviously possible for even `n` as the vertices of `G` could be an arrangement of `n/2` pairs of close-by points.  —  For `n` odd we give an inductive proof. For `n=3` one point will not be visited. For bigger `n` the two points `A` and `B` with the minimal distance visit each other; the other`n-2` points fall in one of the following two cases:  
&nbsp;&nbsp;&nbsp;*First case*: At least one of the remaining `n-2` points visits `A` or `B`; then there are at most `n-3` possible visitors left for `n-2` points; thus one point at least has no visitor.  
&nbsp;&nbsp;&nbsp;*Second case*: The remaining `n-2` points do not visit `A` or `B`; then there are `n-2` visitors for `n-2` houses which settles the claim by induction: At least one house gets no visit.

#### Anchor pairs `(n,m)`

For any `m` we want to find the maximum `n` for which we can establish `(n,m)` as a working pair. This pair will be called an *anchor pair*. 
&nbsp;&nbsp;&nbsp;Example: It is obvious that `(4,2)` is a working pair but not an anchor pair because `(5,2)` is a working pair. We shall see that `(5,2)` is not an anchor pair either.

#### Anchor pair `(9,2)`

No graph `G` has as yet been found with `(n,2), n>9` as working pair. But `(9,2)` is a working pair. The main idea is the graph in figure 3 where all distances marked by a line are equal.
