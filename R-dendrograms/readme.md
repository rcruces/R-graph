Dendrogram
==========
-   [Definition](#definition)
-   [Dataset](#dataset)
-   [Requirements](#requirements)
-   [Default Dendrogram](#default-dendrogram)
-   [Analyses of Phylogenetics and Evolution: `ape` library](#analyses-of-phylogenetics-and-evolution-ape-library)
-   [Colorful Dendrogram: Dendrogram Object + `dendextend` library](#colorful-dendrogram-dendrogram-object-dendextend-library)
-   [Circular Dendrogram: `circlize` + `dendextend` libraries](#circular-dendrogram-circlize-dendextend-libraries)

Definition
----------

A dendrogram is a graphic representation of the data in a tree shape and aids to visualize the groups produced by a [hierarchial clustering analysis](https://en.wikipedia.org/wiki/Hierarchical_clustering). The organization of the tree branches and distance is proportional to the subsample similarity.

Dataset
-------

We will use the `Iris dataset`, described [here](https://github.com/rcruces/R-plots/tree/master/R-heatmap#iris-dataset).

Requirements
------------

Two R libraries are require for this analysis `dendextend` and `circlize`.
1.  **`dendextend`** has a great tutorial with further detailles on the [cran.r-project page](https://cran.r-project.org/web/packages/dendextend/vignettes/introduction.html).
1.  **`circlize`** is a library that enhaces circular visualization in R. If you are interest in more information the [*Circlize Book*](http://zuguang.de/circlize_book/book/) is fantastic.
1.  **`ape`** R-library also known as Analyses of Phylogenetics and Evolution.
To install the packages you should type on the R-terminal:

``` r
install.packages("dendextend")  
install.packages("circlize")
install.packages("ape")
```

Default Dendrogram
------------------

``` r
# Create a matrix from Iris dataset
# We remove the row 143 because is repeated
data.mtx <- as.matrix(iris[-143,1:4])

# Obtain a Euclidean Distance matrix
D <- dist(data.mtx,method = "euclidean")

# Cluster the data with Ward Method
ward.clust <- hclust(D,method = "ward.D")

# Plot of the Default Dengrogram
plot(ward.clust)
```

![](readme_files/figure-markdown_github/dendrogram-1.png)

Analyses of Phylogenetics and Evolution: `ape` library
------------------------------------------------------

`ape` library plots the dendrograms with different output types.

>  Note: plot as.phylo does not allow horiz=FALSE

``` r
library(ape)

# Manually Defines the cluster of k=3
clus3 <- cutree(ward.clust, 3)

# Colors for each cluster k=3
Color<-c("royalblue1","purple4","forestgreen")

# Types of phylograms from ape library
types <- c("phylogram", "cladogram", "fan", "unrooted", "radial")

# Plots the dendrograms, tip.color= labels color matching the cluster
par(mfrow=c(1,3))
for (i in types) {plot(as.phylo(ward.clust), 
                       type= i, 
                       tip.color = Color[clus3], 
                       edge.color = "gray35", 
                       edge.width = 1.5,
                       main=paste("Type:",i))}
```

![](readme_files/figure-markdown_github/ape-1.png)![](readme_files/figure-markdown_github/ape-2.png)

Colorful Dendrogram: Dendrogram Object + `dendextend` library
-------------------------------------------------------------

``` r
library("dendextend")

# Creates a Dendrogram Object
dend.obj <- as.dendrogram(ward.clust)

# Modify the dendrogram to have cluster-based colors in the branches and labels
dend.obj <- dend.obj %>% 
  color_branches(dend.obj,col=c("royalblue1","forestgreen","purple4"),k=3) %>% 
  color_labels(dend.obj,col=c("royalblue1","forestgreen","purple4"),k=3) %>% 
  set("branches_lwd", 3)

# Plot normal dendrogram
par(mfrow=c(1,3))
plot(dend.obj, cex = 0.6, main="Default: dendextend")

# Triangle type Colored Dendrogram
plot(dend.obj, type = "triangle", horiz = FALSE, main="Triangle Type")

# Horizontal Colored Dendrogram
plot(dend.obj, type = "rectangle", horiz = TRUE, main="Horizontal")
```

![](readme_files/figure-markdown_github/dendextend-1.png)

Circular Dendrogram: `circlize` + `dendextend` libraries
--------------------------------------------------------

Using `circlize` and `dendextend` the dendrogram will have cluster-based colors in a very lavish circular output.

``` r
library(circlize)

# Only one line. Try to change the dend_track_height parameter.
circlize_dendrogram(dend.obj, labels_track_height = NA, dend_track_height = 0.6) 
```

![](readme_files/figure-markdown_github/circlize-1.png)
