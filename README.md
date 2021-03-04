### ProKlust (“Prokaryotic Clusters”) was written with a focus on taxonomical data. It obtains, filters and visualizes clusters from multiple identity/similarity matrices.

ProKlust could be employed to analyze any identity/similarity matrix, such as ANI or barcoding gene identity. Additionally, it contains useful filter options to deal with taxonomical data. 

### How to install

- Install the devtools package: install.packages("devtools")
- Load devtools: library(devtools)
- Install this package: install_github("camilagazolla/ProKlust")
- Done! You can then load it by: library(ProKlust).

### Package dependencies
- [igraph](https://cran.r-project.org/web/packages/igraph/index.html)
- [networkD3](https://cran.r-project.org/web/packages/networkD3/index.html)

### How to use

- Use function prokluster to obtain the clusters.
- Use function plotc to plot.

#### Inputs
- Obligatory tabbed-delimited pairwise identity/similarity matrix(ces) 
- Optional annotation table text file with a specific format: each line must be "(previous name)<Tab>(new name)<New Line>", where (previous name) and (new name) can be alphanumeric and some special characters.

#### Filters
- filterRemoveIsolated: remove isolated nodes i.g. nodes that do not form groups/clusters; 
- filterRemoveLargerComponent and filterOnlyLargerComponent: remove or retain only the component containing the highest number of nodes;
- filterDifferentNamesConnected: retain groups of connected nodes containing more than one binomial species name;
- filterSameNamesNotConnected: retain groups of unconnected nodes containing the same species names.

#### Outputs

- maxCliques: the maximal clique is the largest subset of nodes in which each node is directly connected to every other node in the subset; 
- components: contains the isolated nodes or groups formed of complete graphs; 
- graph: an igraph object graph, that can be further handled by the user;
- plot: where the final graph could be promptly visualized with forceNetwork function from the networkD3 R package.

A genome/gene that is part of a component does not necessarily share identity/similarity values above the established cut-off with all the other genomes/genes of that component, but it must share an identity/similarity value above the cut-off for at least one other genome/gene. Cliques, instead, are formed by genome/gene that all share identity/similarity values above the chosen criteria. A genome/gene could belong at the same time to different cliques within the same component.

#### Examples
```
#Example 1
basicResult <- prokluster(filenames = "ANIb_percentage_identity.tab", cutoffs = 0.9)

#Example 2
files <- c("ANIb_percentage_identity.tab", "ANIb_alignment_coverage.tab")
thresholds <- c(0.95, 0.70)
renamedResults1 <- prokluster(filenames = files, cutoffs = thresholds, nodesDictionary = "dictionary.tab", filterRemoveIsolated = TRUE)

#Example 4
nodesNames <- read.table(file= "dictionary.tab", sep = "\t", header = F, stringsAsFactors=FALSE)
renamedResults3 <- prokluster(filenames = files, cutoffs = thresholds, nodesPreviousNames = nodesNames$V1, nodesTranslatedNames = nodesNames$V2, filterSameNamesNotConnected = T)

#Example 5
files <- c("ANIb_percentage_identity.tab", "ANIb_alignment_coverage.tab")
thresholds <- c(0.95, 0.70)
results <- prokluster(filenames = files, cutoffs = thresholds, nodesDictionary = "dictionary.tab")
plottedD3 <- plotc(results$graph)
```


### Workflow


<img src="https://user-images.githubusercontent.com/64544051/109963334-0d672180-7ccb-11eb-85f5-238f9cefbe76.JPEG" width="50%" height="50%">


A) The average of each pair from the pairwise input matrix/matrices is/are obtained. A Boolean matrix/matrices is/are obtained according to the cut-off values chosen by the user. If more than one matrix is used as input, the final generated matrix is obtained by multiplying the elements of the matrices. A graph is formed by connecting the nodes which present the positive values. In this example, nodes correspond to genomes and edges correspond to ANI ≥ 95% with coverage alignment ≥ 50%. The data could be filtered to retain components containing more than one species name or unconnected nodes containing the same species names. 

B) Overview of the hierarchical-based clustering approach. These approaches return tree-shaped diagrams with non-overlapping clusters.

