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


Test for pairwise different distances (should be 0):  0

Visited  12  {{0.0429076,0.304972},{0.503513,0.536901},{0.437182,0.465738},{0.7299,0.931306},{0.141308,0.909238},{0.362938,0.962599},{0.995759,0.582091},{0.975956,0.546681},{0.0593653,0.485206},{0.677594,0.281822},{0.497433,0.387906},{0.634686,0.97685}}

Not visited  4  {{0.298363,0.264859},{0.561932,0.0972997},{0.936393,0.096885},{0.998866,0.690008}}

\!\(\*
GraphicsBox[{{{}, 
{GrayLevel[0], PointSize[Large], 
      PointBox[{{0.2983625488820537, 0.26485857088834647`}, {
       0.5619321802753807, 0.09729971949628732}, {0.9363934221770109, 
       0.09688502327743508}, {0.9988656592233294, 
       0.6900078999914248}}]}, {}}, {{}, 
{RGBColor[0, 1, 0], PointSize[Large], 
      PointBox[{{0.042907637866073066`, 0.3049721597862793}, {
       0.5035127166889447, 0.5369006549274477}, {0.43718199257873636`,
        0.46573829301635467`}, {0.7299000617050462, 
       0.9313060675822221}, {0.14130818863696876`, 
       0.9092384788328058}, {0.36293794043512, 0.962599371614135}, {
       0.9957587603181495, 0.5820907827457411}, {0.9759564481316755, 
       0.5466809725180141}, {0.05936533814113854, 
       0.485205759468306}, {0.6775938992496218, 
       0.28182240162966765`}, {0.49743315786575787`, 
       0.38790603997201867`}, {0.6346862613835487, 
       0.9768502418433884}}]}, {}}},
AspectRatio->1,
Axes->True,
AxesLabel->{None, None},
AxesOrigin->{0, 0},
Method->{},
PlotRange->{{0, 1}, {0, 1}},
PlotRangeClipping->True,
PlotRangePadding->{{0.014010062206825514`, 0.014010062206825514`}, {
    0.013800157999828495`, 0.013800157999828495`}}]\)
