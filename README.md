# Next-neighbours
An open problem concerning next neighbours in a planar graph

Next neighbours - introduction

"Next neighbours" is a project related to graphs and plane geometry. The aim of the project is to discuss an open problem which will be named "Next Neighbour Problem".

The Next Neighbour Problem has a simple explanation in real world terms. Consider a village where, on a certain day, everyone visits her or his next neighbour.

To make things easier we will think of n houses with pairwise different distances.

For most arrangements of houses there will be some villagers who do not receive visitors.

What about the number m of visited houses? There is an infinite number of possible arrangements of the n houses. So (for n>3) different m will appear in these arrangements. It is easy to establish the maximum for m and not so easy to establish the minimum for m. In fact, the latter is an open problem (as far as the author knows).

![neighbours_figure_01](https://user-images.githubusercontent.com/88709288/129047431-3f63be39-9f12-48c1-9bfa-b7570456d4eb.png)

Figure 1  -  n=10, m=5

Here is Mathematica code for random arrangement of houses:

***********************************************************************************
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
 ***********************************************************************************
