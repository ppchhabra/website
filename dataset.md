---
layout: page
title: Dataset
subtitle: Posting data and pseudocodes
---

### styles in designs

Product design (or the form of a product) is an important aspect of new product development. This site serves as a repository where I (in collaboration with Jürgen Mihm, and Manuel Sosa) post results, data, and materials for our ongoing research on product design.

The styles dataset (found [here](https://drive.google.com/open?id=1s6iJnyxDbWrNXFv0RCjiLY3ubK2eIxZ7)) came out of this [paper](https://pubsonline.informs.org/doi/10.1287/mnsc.2016.2653): using design patent data granted from 1977-2010, we categorized over 350,000 designs into over 9,000 styles (or categories of designs that are perceived to be visually similar).

The same dataset is also used in a collaboration with Yonghoon Lee (currently working). If you use the data or would propose improvements / alternatives to our approach, please also let us know and we are happy to improve the approach over time, and acknowledge your work.

![Beetle](https://cdn.shopify.com/s/files/1/0101/8547/4107/products/A1dGir4nbIL._SL1500_540x.jpg)

#### Pseudocode to generate styles 
Attached is the pseudocode if you wish to identify styles on a dataset of designs (first you need a measure of similarity between them). 

The approach in turn relies on the Ng-Jordan-Weiss (2002) algorithm (NJW), which approximately partitions a group of objects into two groups by minimizing conductance (a graph measure of heterogeneity). The evaluate step stops the algorithm corresponding to a post-hoc identified Δ value of about 0.002, at which point a sharp increase in conductance is observed. (The NJW algorithm is popular and you can find ready implementations online, e.g., from [MATLAB central](https://www.mathworks.com/matlabcentral/fileexchange/44879-spectral-clustering)).  

~~~
1. Select the group with the lowest conductance ϕ: Calculate e_2 from step (b) in NJW for each group of designs. Label the group with the smallest e_2 as〖 G〗_T. Label the corresponding e_2 as ϕ_T. (Note that there is only one group in first iteration, so〖 G〗_T=G_1).
2. Partition G_T into two groups: Partition G_T into two groups G_T1 and G_T2  using NJW. 
3. Evaluate if partitioning is to continue: measure conductance ϕ over all the groups that created thus far, and label the lowest value identified ϕ_TNext. Stop if ϕ_TNext>Δ+ϕ_T. Else, repeat Steps 1-3. 

NJW:
a.	Compute the normalized graph Laplacian matrix L=I-D^(1/2) SD^(1/2) where I is the identity matrix and D is a diagonal matrix with each entry the degree of the vertex.
b.	Compute the two smallest eigenvalues {e_1,e_2} and their associated eigenvectors {u_1,u_2} 
c.	Let U∈R^(n×2) be the matrix containing {u_1,u_2} as columns.
d.	Normalize the rows of  U to unit lengths (i.e., length = 1).
e.	Treat every row as a point in space, and use K-means (two groups) to group the n data points.
~~~

### measuring "decomposability" of a (utility) patent


1.	For each claim, identify whether it is independent or dependent by using a regular-expression search for the term “claim #”, where # signifies any number.
2.	If the claim is independent:
a.	Break down the claim into individual words.
b.	Tag each word in the claim with its type: noun, adjective, verb, etc.
c.	Identify the root for each word (e.g., reduce the word “surfaces” to “surface”).
d.	Identify the subject matter for the claim based on first-occurring noun phrase with adjectives 
(e.g., “outer free end”, “elastic strip”, “bioprosthetic mitral valve replacement”).
3.	If the claim is dependent:
a.	Take the sentence starting after “claim #”—for example, from the text “A mitral valve replacement as in claim 1, wherein the base is…” use only “wherein the base is…”.
b.	Return to steps a.–d. of Step 2.
4.	Count the number of distinct subject matter in the patent.



