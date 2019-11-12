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

***

1. Select the group with the lowest conductance ϕ: Calculate e<sub>2</sub> from step (b) in NJW for each group of designs. Label the group with the smallest e<sub>2</sub> as **G**<sub>T</sub>. Label the corresponding e<sub>2</sub> as ϕ<sub>T</sub>. (Note that there is only one group in first iteration, so G<sub>T</sub>=G<sub>1</sub>).
2. Partition G<sub>T</sub> into two groups: Partition G<sub>T</sub> into two groups G<sub>T1</sub> and G<sub>T2</sub>  using NJW. 
3. Evaluate if partitioning is to continue: measure conductance ϕ over all the groups that created thus far, and label the lowest value identified ϕ<sub>TNext</sub>. Stop if ϕ<sub>TNext</sub>>Δ+ϕ<sub>T</sub>. Else, repeat Steps 1-3. 

NJW:

a.	Compute the normalized graph Laplacian matrix L=I-D<sup>1/2</sup> SD<sup>1/2</sup> where I is the identity matrix and D is a diagonal matrix with each entry the degree of the vertex.

b.	Compute the two smallest eigenvalues {e<sub>1</sub>,e<sub>2</sub>} and their associated eigenvectors {u<sub>1</sub>,u<sub>2</sub>} 

c.	Let U∈R<sup>n×2</sup> be the matrix containing {u<sub>1</sub>,u<sub>2</sub>} as columns.

d.	Normalize the rows of U to unit lengths (i.e., length = 1).

e.	Treat every row as a point in space, and use K-means (two groups) to group the n data points.

***


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



