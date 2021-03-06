# Next-neighbours
An open problem concerning next neighbours in a planar graph

## Introduction

"Next neighbours" is a project related to graphs and plane geometry. The aim of the project is to discuss an open problem which will be named "Next Neighbour Problem".

The Next Neighbour Problem has a simple explanation in real world terms. Consider a village where, on a certain day, everyone visits her or his next neighbour.

To make things easier we will think of `n` houses with pairwise different distances.

For most arrangements of houses there will be some villagers who do not receive visitors.

What about the number `m` of visited houses? There is an infinite number of possible arrangements of the `n` houses. So (for `n>3`) different `m` will appear in these arrangements. It is easy to establish the maximum for `m` and not so easy to establish the minimum for `m`. In fact, the latter is an open problem (as far as the author knows).

![neighbours_figure_01](https://user-images.githubusercontent.com/88709288/129047431-3f63be39-9f12-48c1-9bfa-b7570456d4eb.png)

Figure 1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`n = 10, m = 5`

Here is Mathematica code for a random arrangement of houses:

```
(* Neighbours random *)
(* file: next-neighbour_01.nb *)

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

Visited  10  {{0.136873,0.662183}, {0.77065,0.908998}, {0.099012,0.644006},\
{0.0747849,0.711588}, {0.417849,0.475834}, {0.466013,0.146717}, {0.494827,0.421237},\ 
{0.418441,0.00420624}, {0.914883,0.921726}, {0.18339,0.548094}}

Not visited  6  {{0.0452213,0.368721}, {0.0981768,0.793929}, {0.144233,0.0127273},\
{0.281567,0.342023}, {0.522212,0.630813}, {0.850147,0.131827}}
```
 
Here is the Mathematica plot:
 
![neighbours_figure_02](https://user-images.githubusercontent.com/88709288/129056422-5dc3d0be-65c3-4498-8d13-d67dffa7c151.png)

Figure 2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`n = 16, m = 10`


## Mathematical description of the problem and some proofs

Let `G` be a simple digraph; the vertices of `G` are `n` arbitrary placed points in `R^2` with pairwise different distances; the edges of `G` are arrows joining each point (tail end) to its nearest neighbour (head end).

Only `n>1` makes sense. We call a point *visited* if it is a head end in `G`, else *unvisited*. Every point is a *visitor* as it *visits* one head end. We call `(n,m)` a **_working pair_** if we can present an arrangement of `n` points with exactly `m` points visited. We get a first result for working pairs:

**(1)&nbsp;&nbsp;&nbsp;`m > 1`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;** and **&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`n odd`&nbsp;&nbsp;&nbsp;&nbsp;&rArr;&nbsp;&nbsp;&nbsp;`m < n`**

This can easily been proved. `m=0` and `m=1` are discarded because the two points with minimal distance visit each other.  ???  What about `m = n`?  This is obviously possible for even `n` as the vertices of `G` could be an arrangement of `n/2` pairs of close-by points.  ???  For `n` odd we give an inductive proof. For `n=3` one point will not be visited. For bigger `n` the two points `A` and `B` with the minimal distance visit each other; the other`n-2` points fall in one of the following two cases:  
&nbsp;&nbsp;&nbsp;*First case*: At least one of the remaining `n-2` points visits `A` or `B`; then there are at most `n-3` possible visitors left for `n-2` points; thus one point at least has no visitor.  
&nbsp;&nbsp;&nbsp;*Second case*: The remaining `n-2` points do not visit `A` or `B`; then there are `n-2` visitors for `n-2` houses which settles the claim by induction: At least one house gets no visit.

#### Anchor pairs `(n,m)`

For any `m` we want to find the maximum `n` for which we can establish `(n,m)` as a working pair. This pair will be called an *anchor pair*.  
&nbsp;&nbsp;&nbsp;Example: It is obvious that `(4,2)` is a working pair but not an anchor pair because `(5,2)` is a working pair. We shall see that `(5,2)` is not an anchor pair either.

#### Anchor pair `(9,2)`

No graph `G` has as yet been found with `(n,2), n > 9` as working pair. But `(9,2)` is a working pair. The main idea is the graph in figure 3 where all distances marked by a line are equal.

![neighbours_figure_03](https://user-images.githubusercontent.com/88709288/129348759-ecd18f04-6eea-4138-991c-22da53d249d8.png)

Figure 3

If we move the points in figure 3 a little bit we can yield pairwise different distances and leave the two green points as the only visited points. To prove this we present a Mathematica program (which is a slight modification of the program in the introduction):

```
(* Neighbours (9,2) *)
(* file: next-neighbour_02.nb *)

t = {{.3, .5}, {.6, .5}, {.955, .5}, {.355, .8}, {.35, .2}, {.045, .67}, {.0455, .33}, {.7, .8}, {.7, .195}};

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
Print["Visited  ", t2]
Print["Not visited  ", t1]
Show[ListPlot[t1, PlotStyle -> {Black, PointSize[Large]}], 
 ListPlot[t2, PlotStyle -> {Green, PointSize[Large]}], 
 PlotRange -> {{0, 1}, {0, 1}}, AspectRatio -> 1, 
 AxesOrigin -> {0, 0}]
 ```

Here is the output:

```
Test for pairwise different distances (should be 0):  0

Visited  {{0.3, 0.5}, {0.6, 0.5}}

Not visited  {{0.045, 0.67}, {0.0455, 0.33}, {0.35, 0.2}, {0.355, 0.8}, {0.7, 0.195}, {0.7, 0.8}, {0.955, 0.5}}
```
 
Here is the Mathematica plot:

![neighbours_figure_04](https://user-images.githubusercontent.com/88709288/129362232-333a6107-5814-4e8c-bf13-a54b4f4af610.png)

Figure 4&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`n = 9, m = 2`

#### Anchor pair `(12,3)`

No graph `G` has as yet been found with `(n,3), n > 12` as working pair. But `(12,3)` is a working pair. The main idea is the graph in figure 4 where all distances marked by a line are equal.

![neighbours_figure_05](https://user-images.githubusercontent.com/88709288/129366475-7ebb237e-a40a-4b9c-b7bf-94ef692de115.png)

Figure 5

If we move the points in figure 5 a little bit we can yield pairwise different distances and leave the three green points as the only visited points. To prove this we present a Mathematica program (which is a slight modification of the program in the introduction):

```
(* Neighbours (12,3) *)
(* file: next-neighbour_03.nb *)

t = {{.3, .5}, {.6, .5}, {.895, .5}, {1.25, .5}, {.35, .8}, {.345, .2}, {.045, .67},\
{.0455, .33}, {1.05, .8}, {1.05, .199}, {.7, .78}, {.7, .221}}/1.3;

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
Print["Visited  ", t2]
Print["Not visited  ", t1]
Show[ListPlot[t1, PlotStyle -> {Black, PointSize[Large]}], 
 ListPlot[t2, PlotStyle -> {Green, PointSize[Large]}], 
 PlotRange -> {{0, 1}, {0, 1}}, AspectRatio -> 1, 
 AxesOrigin -> {0, 0}]
 ```

Here is the output:

```
Test for pairwise different distances (should be 0):  0

Visited  {{0.230769, 0.384615}, {0.461538, 0.384615}, {0.688462, 0.384615}}

Not visited  {{0.0346154, 0.515385}, {0.035, 0.253846}, {0.265385, 0.153846}, {0.269231, 0.615385},\ 
{0.538462, 0.17}, {0.538462, 0.6}, {0.807692, 0.153077}, {0.807692, 0.615385}, {0.961538, 0.384615}}
```
 
Here is the Mathematica plot:

![neighbours_figure_06](https://user-images.githubusercontent.com/88709288/129367183-2311882a-bb78-45da-b7c8-534c4e0e58d2.png)

Figure 6&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`n = 12, m = 3`

#### Anchor pairs for bigger `m`

We will suppose that for `m=2` resp. `m=3` the maximum `n=9` resp. `n=12` have been established. One could be tempted to proceed to `m = 4, 5 ...` in a similar manner. Take `m=4` as an example. ???Similar manner??? means putting `4` points in the center which are visited and putting around these `4` points as many points as possible which are not visited. But this seems to be suboptimal because one can also replicate and combine the arrangements in figures 4 and 6: As we want to establish a big as possible `n` for each `m` it seems to be a good idea to build several "islands" of type `(9,2)` if `m` is even. For odd `m` one adds a single "island" of type `(12,3)`. Figure 7 shows the islands for the anchor pair `(n,m) = (57,13)`:

![neighbours_figure_07](https://user-images.githubusercontent.com/88709288/129550589-0f5c5a75-8de1-4bd7-ab0f-222bda3f15b5.png)

Figure 7&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`n = 57, m = 13`\
&nbsp;\
&nbsp;\
**(2)**&nbsp;&nbsp;&nbsp;This yields the **anchor pairs `(9m/2,m)` for `m even`** and **`((9m-3)/2,m)` for `m odd`**.\
&nbsp;
#### All pairs `(n,m)` proven to be working pairs

**(3)**&nbsp;&nbsp;&nbsp;**`(n,m)`** is an **anchor pair** as in (2)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**&rArr;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`(n',m)`** is a **working pair for `n' = m ... n, n even`** and **`n' = m+1 ... n, n odd`**.

**Proof**

Look at figure 4. One can remove any number of the unvisited points (black dots) hereby making `n` smaller while leaving `m` unchanged.

Now look at figure 6. One can remove any number of the unvisited points (black dots) hereby making `n` smaller. When all unvisited points are removed one of the visited points has become an unvisited point (in figure 6, this will be the left one); this reflects the second part of (1).

So (3) is proven for `(n,m) = (9,2)` and `(n,m) = (12,3)`. But the bigger anchor pairs are built from these ???starting??? anchor points. This completes the proof. - In figure 7 one can remove all black dots, one after the other, of the five left ???islands???; then one can remove all black dots, one after the other, of the right ???island???, thus changing one green dot to black. QED.

(3) is shown graphically in figure 8:

![neighbours_figure_08](https://user-images.githubusercontent.com/88709288/129554116-80b2775d-9259-484a-9915-70bc1d029d36.png)

Figure 8\
&nbsp;&nbsp;&nbsp;&nbsp;x: working pair\
&nbsp;&nbsp;&nbsp;&nbsp;green: anchor pair\
&nbsp;&nbsp;&nbsp;&nbsp;orange: no working pair (proven)\
&nbsp;&nbsp;&nbsp;&nbsp;white: no working pair (unproven conjecture) 

Regarding figure 8, the following proposition (4) seems obvious but will be proven. In (3) we ???filled??? the columns of the matrix in figure 8, in (4) we ???fill??? the rows.

**(4)**&nbsp;&nbsp;&nbsp;**`(n,m)`** is a **working pair** as in figure 8&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**&rArr;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`(n,m')`** is a **working pair for `m' = m ... n, m even`** and **`m' = m ... n-1, m odd`**.

**Proof**

We take a pair `(n,m')` with `m' < n` for `m even` and `m' < n-1` for `m odd`. It suffices to show that `(n,m'+1)` is a working pair (by induction, beginning with `m'=m`). The corresponding anchor pair to `(n,m')` is `(n0,m')` and the corresponding anchor pair to `(n,m'+1)` is `(n1,m'+1)`. (3) states that `(n,m'+1)` is a working pair because `n <= n0 < n1` (see (3) for the first inequality and (2) for the second one). QED.

## Results

We can now define four sequences to describe our results.

The sequences `a` and `b` deal with the question ???`n` points given, how many must at least be visited????\
The sequences `c` and `d` deal with the question ???`m` points visited, how many points can at most exist????

In the sections above we got results for `c`. The formulae given below for `a` are directly derived from (2).

**`d(m)  = max n`, maximum taken over working pairs `(n,m)`**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`d(m)=` maximal number of vertices, maximum taken over all possible `G` with exactly `m` vertices with `indegree > 0`

**`c(m) = max n`, maximum taken over _proven_ working pairs `(n,m)`**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`(c(m),m)` are the anchor pairs defined in (2), cf. figure 8.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`c(m) = 9m/2, m even`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`c(m) = (9m-3)/2, m odd`\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`c` is strictly increasing.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`c = (9,12,18,21,27,30, ...)`, beginning at `m=2`\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The sequence `c` is the ???inverse??? of `a`.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`c(m)` is the (best presently known) lower bound for `d(m)`.

**`b(n)  = min m`, minimum taken over working pairs `(n,m)`**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`b(n)=` minimal number of vertices with `indegree > 0`, minimum taken over all possible `G` with `n` vertices

**`a(n) = min m`, minimum taken over _proven_ working pairs `(n,m)`**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`a(2) = a(3) = 2`\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`a(n) = 2j for n = 9j-5 ... 9j, j >= 1`\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`a(n) = 2j+1 for n = 9j+1 ... 9j+3, j >= 1`\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;or with `h=(n+5)/9` for `n>3`:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`a(n) = 2 floor(h) if h-floor(h)<2/3`\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`a(n) = 2 floor(h)+1 else`\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`a` is (not strictly) increasing.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`a = (2,2,2,2,2,2,2,2,3,3,3,4,4,4,4,4,4,5,5,5,6,6,6,6,6,6,7,7,7,8, ...)`, beginning at `n=2`\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The sequence `a` is the ???inverse??? of `c`.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`a(n)` is the (best presently known) upper bound for `b(n)`.

#### OEIS

`a(n)` is the sequence [A347941](http://oeis.org/A347941) in OEIS.\
`c(m)` is the sequence [A261953](http://oeis.org/A261953) for `m > 1` in OEIS.

#### The open problem

Is `a(n) = b(n)` and `c(m) = d(m)`? If "yes" the _Next Neighbour Problem_ would be solved.

Under the following assumptions the answer is ???yes???:

`d(2)=9`\
`d(3)=12`\
For `m>3` the arrangements as in figure 7 are optimal.

So the challenge is this: Can one do better than in figures 4, 6, 7? Or can it be proved that the arrangements in these figures are optimal?
