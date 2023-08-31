# Gene comparison to network subgraph pipeline

Summary: This pipeline finds the most correlated target genes between a given sample and the L1000 Phase I and Phase II data. Then uses a selected target gene and comparison geneset to produce a subgraph of the shortest path from target gene to each of the comparison (comp) genes (using https://www.diseaselinks.com/TissueNexus/download.php).

# Pipeline:

## Step 1 - Calculate correlation and Enrichment Score against L1000 data:
Input: CSV with the format of example file <br>
Output: Table sorted by pearson correlation of the target column and each L1000 column


Filter and select genes
<br>Get the targets for the whole comp file
<br>Filter out samples with no targets
<br>Drop duplicates while keeping those with top pearson correlation
<br>Filter the gene expression files for L1000toRNAseq level 5 data
<br>Take the selected gene columns and keep only those genes from the several files
<br>Combine the files together - Note that the dataframes should be processable at this point as long as the genes are less than 500 total
<br>Get the correlation and ES score of each with comp genes<br>

## Step 2 - Get drug targets 
Input: Output of step 1
<br>Output: Step 1 results with targets included sorted by pearson correlation, used to select target genes.

## Step 3 - Create subgraph based on target gene with up and down regulated comp set genes
Input: CSV with the column of up and down regulated comp set genes
<br>Output: subgraph centered on target gene with shortest paths to each of the up and down regulated comp set genes.


Select a target gene to start graphs from
<br>Load in brain data
<br>Create distance column: 10.0x(1.0-weight)
<br>Run Djikstra’s algorithm from selected gene to comp gene set
<br>Check selected gene to comp gene set vs selected gene to random set (no replacement or comp set)
<br>Save out subgraph of comp set genes and shortest paths to comp set genes

## Additional step - Bring the subgraph file into Cytoscape and format:
Load network from table, set the source and target nodes
<br>Change the colors of the nodes to signify up and down genes
<br>Signify the target gene with its own color
<br>Set edge width to weight column on a continuous mapping
<br>Use prefuse force directed layout with the distance column
