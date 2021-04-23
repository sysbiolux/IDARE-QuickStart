# IDARE Quickstart guide

This Repository contains basic sample datasets to use with the IDARE cytoscape application.
The data and models used in the examples are in the [Data folder](https://github.com/sysbiolux/IDARE-QuickStart/tree/master/Data).
 
The Tutorial contains two parts.

Part 1 is an example with the E. coli metabolic model overlayed with metabolomic, proteomic and gene expression data.

Part 2 is an example with the Zebrafish metabolic model and gene expression data from 3 pancreatic cell types.


In each case, the following 3 steps are recommended:
1) setting up the network, which is necessary for image matching.  
2) adding images based on the data to the network.  
3) split the network into parts based on compartments and pathways.  

For more detailed descriptions on the tool, please check the [USER MANUAL](https://github.com/sysbiolux/IDARE/blob/master/UserManual.pdf).

# Example 1: overlaying multiple omics data onto the E. coli metabolic model

You can get the model and sample data in the [*E.coli* Data folder](https://github.com/sysbiolux/IDARE-QuickStart/tree/master/Data/ecoli)
You can also get the model by right-clicking on this [Link](https://github.com/sysbiolux/IDARE-QuickStart/tree/master/Data/ecoli/ecoli_core_model.xml) and selecting "save as".

Sample data:

-"Fold Change of Gene Expression ethanol vs no ethanol.xls" - Data from Horinouchi et al.[1] used to calculate Gene expression fold changes between untreated and ethanol treated samples  
-"Metabolomics fold change treatment vs Control.xls" - Data from Wang et al[2] for Metabolomics fold changes of treatment vs control samples  
-"Total relative C13 label.xls" - Data from Goodarzi et al.[3] for C13 labeling upon feeding with ethanol for a wild type and a ethanol resistant starin. The data is computed to present the total amount of labeled carbons in each metabolite.  
-"Proteomics Time course of ethanol treated wild type.xls" - Data from Soufi et al.[4] epresenting the fold change to time 0 of different protein products associated with the genes in the network.  


## Part 1: Setting up the Network for IDARE in Cytoscape
In this example, we will set up the *E.coli* core network for use with IDARE and add some additional annotations to the network.

### 1.1 Loading the E.coli core model
After installing the IDARE app (either from the Cytoscape app store or from [here](https://github.com/sysbiolux/IDARE/releases/download/2.0/IDARE-2.0.jar)), load the *E.coli* core network by selecting:

File --> Import --> Network --> Network from File and select the *E.coli* core file you downloaded.

### 1.2 Generating an initial Layout
From the Layout menu, select a layout you want to apply (for this tutorial we assume, that the **y-Files organic layout** was choosen, or no additional layout if you have cy3sbml installed).

### 1.3 Setting up the Network for IDARE

Right click on some empty space in the network view.

Select 'Apps' --> IDARE --> 'Setup Network For IDARE'  
A popup will appear that lets you select the properties to be used in IDARE.  
On the left you can select the columns you want to use for the setup. On the right, depending on your choice of columns, you can select the values to be used for compound and interaction.  
Select the 'sbml type' column as column to determine node types and the 'sbml id' column as column to determine the node names.  
As compound node value select species, and as interaction node value select reaction.  
Check the 'Overwrite existing values' checkbox.  
If you are using cy3sbml please select the Base network view (it should have 187 nodes and 380 edges).

### 1.4 Changing to the IDARE Visual Style
From the 'Style' tab in the 'Control Panel' select the 'IDARE Visual Style' and apply it to the network. 

Note, that the IDARE images cannot be removed from this style. 

If you want to use the images with another style, you can add and remove them by right-clicking in the display area, and select 
Apps --> Add/Remove IDARE Images

### 1.5 Adding SBML Annotations and Gene Nodes
Right click on some empty space in the network view.  
Select 'Apps' --> 'Add SBML Annotations'

Select the *E.coli* core network file again. 
If you have cy3sbml this step is not necessary, as IDARE will automatically use the SBML structure provided by cy3sbml for the network.  
You will be asked, whether you want to add Gene Nodes. Tick the box (if not done already) and click Ok.

You will notice, that additional nodes have been created, which represent the protein and gene nodes that were created. Proteins are created for all conjunctive GPR clauses (i.e. all possible and combinations that fulfill a GPR).  
If the SBML contains enzyme species (annotated by the "isEncodedBy" bio qualifier), you would be asked which labeling pattern (i.e. database) to use for the genes and proteins.


## Part2: Automated image generation and loading images to the Network.
In this example you will generate a few images based on artificial data and map it to the previously created and initialized network.
This example assumes that you have at loaded the E.coli core network and set up the network for IDARE (Steps 1 and 3 of the previous example).

### Load Data into IDARE
In the 'Control Panel', select the IDARE tab. 

Click on 'Add Dataset'

In the dialog, click on 'Choose File'  

Select "Fold Change of Gene Expression ethanol vs no ethanol.xls" and click 'open' 

The 'DataSet Description' field will be updated with the file name  

As 'DataSet Type', select 'Array Dataset' (since this dataset contains only one sheet). 

Check the 'Use two column headers' option, since the dataset provides id and label columns.  
Click ok.  

Repeat the process with "Metabolomics fold change treatment vs Control.xls"

Now, load the "Total relative C13 label.xls" dataset which should both be loaded as 'Multiarray Dataset'. 

This dataset only has a single header column (i.e. only IDs not labels), so uncheck the two-column header box. 

Finally, load the "Proteomics Time course of ethanol treated wild type.xls" dataset, which again provides two header columns and can be loaded as both a 'Multiarray Dataset' or an 'Array dataset' as it contains only one sheet in the xls. 

Loading it as both types allows the selection of additional layouts (those specific to multiarray datasets and those specific to array datasets).  

Example descriptions would be "External Metabolite amount information" and "Reaction Activity during the experiment" and "Internal Metabolite Amounts at specific stages".

### 2.2 Create the Visualisation
You can now select individual datasets you want to visualize. 
To do so, choose a selection of datasets from the table by ticking the "selected" boxes.  
In the lower left corner, (below the 'Create' and 'Preview' buttons) are indicators, how many nodes the datasets represent in total and how many shared nodes the selection contains.  
When creating visualisations for multiple datasets at once, there should be an overlap of the sets, otherwise parts of the generated image will always stay blank since no data is available.
However, you can of course create multiple different nodes by selecting different sets of data.
Images will only be created for nodes which are part of a selected dataset and old images will be kept unless a new image is created for the node.

### 2.3 Adding IDARE images to other Styles
If you did not select the IDARE visual style, but want to use a different style for your network, you can do so by right-clicking anywhere in a network view using the style you want to add your nodes to.
Select 'Apps' --> 'Add IDARE Images'. Now the images will be shown for your choosen style.


## Part3: Subnetwork Generation

The first step of this part is independent of the first 2 parts.
For the second step, it is necessary to have the SBML annotations added to the network (step 1.5), as otherwise the corresponding table column does not exist.
You can add the sbml annotation to the Cytosol network (C_c) after step 1 of this example, but this would lead to gene nodes not being added in the external compartment subnetwork.

### 3.1 Create Networks for the External compartment and the cytosol
NOTE: If you want to recreate Figure 4 of the Paper, skip the creation of the Compartment subnetworks, as they were not created for that figure.  
We first want to split the network at the transporters which translocate metabolites from the external compartment to the cytosol, and vice versa.  
To do so:  
Select 'Apps' --> 'IDARE' --> 'Create Subnetworks'  
or  
Right Click in an empty space in the network you want to create subnetworks in --> 'Apps' --> 'Create Subnetworks'  
In the resulting dialog, you can select the column to determine the node types as well as the node names.  
If the network is set up for IDARE (see Part 1), appropriate columns are chosen automatically (i.e. if you have set up the network for idare - Step 1.3 - you can just accept the selection) .  
You further have to choose a value for nodes which form the branching points between subnetworks, and a value for nodes that form the "base" of the subnetwork.
Since the aim is to branch at the transporters between compartments reactions will be used as branching nodes and species as subnetwork nodes.   
Click accept, when you finished the selection.  
In the next dialog, you can select the column that contains the subnetwork identifiers as well as the type of layout that should be used for the subnetwork (or the "keep layout" option, if the current layout shoudl be kept)
The table allows you to select nodes, which should not branch and those which should be removed entirely  (e.g. nodes with too many connections).   
Depending on the network size some nodes are already suggested by the tool.  
Since we want to create the networks for the cytosol and external compartment, we select "sbml compartment" as column to determine sub-networks.  
We leave the 'Keep Layout' option as it is and select remove for the Biomass reaction (as this reaction mainly leads to a hairball structure).  
We also select C\_c, the cytosol, and C\_e, the external medium, in the sub-networks to be generated table.   
If we now click on the "network" tab in the 'Control Panel' we will see two new networks that were generated.   

### 3.2 Create pathway networks in the Cytosol
There is not much to see in the external compartment, as it only contains the transporters and the cytosol is still a hairball. 
To get a better overview, we would like to generate pathway networks in the cytosol.  
To do so, again, select 'Apps' -> 'SubNetworkGenerator'  
However, this time, since reactions belong to pathways and metabolites can be shared between different pathways, we select species as values for branching nodes and reaction as value for subnetwork nodes.  
By default, the "SUBSYSTEM" column generated by the SBMLAnnotator is selected, if available, so this selection is already fine.  
From the metabolites we would like to exclude currency metabolites and most cofactors.  
For larger networks, the pre-selection is better, but on small networks (like the e.coli core model), the relative amounts of retained nodes tends to be higher, so we have to select additional nodes to remove during the generation process.  
Select M\_h\_c, M\_h2o\_c, M\_atp\_c, M\_pi\_c, M\_pi\_c, M\_nadh\_c, M\_nad\_c, M\_adp\_c, M\_nadph\_c, M\_co2\_c, M\_nadp\_c and M\_coa\_c, as metabolites to remove.  

As pathways, we would like to create 'Glycolysis', the 'Citric Acid Cycle', 'Glyoxylate Shunt' and the 'Pentose phosphate pathway'.  
Click ok.  

You can now navigate through the different pathways. If you wish to create additional pathways, select the source network again and repeat the steps.
New links will be added to the existing networks.

You will notice, that links from parental networks will be included in the newly created networks, but no links are generated in these parental networks. 
In general, links will be generated for all networks built on the same column, that share linking nodes. 
If you create additional subnetworks at a later stage, links to and from old networks will be created for all newly generated networks.

You can see, that the labelling for ketoglutarate is much smaller than the labelling of any other measured metabolite. One potential reason is an increase in the glyoxylate shunt (visible in both the proteomics and gene expression of the enzymes involved in that pathway). 

# Example 2: visualizing RNA-seq data from pancreatic cell types onto the Zebrafish metabolic model

You can get the model and sample data in the [Zebrafish Data folder](https://github.com/sysbiolux/IDARE-QuickStart/tree/master/Data/zebrafish)
You can also get the model by right-clicking on this link [zebraGEM2_model.xml](https://github.com/sysbiolux/IDARE-QuickStart/tree/master/Data/zebrafish/zebraGEM2_model.xml) and selecting "save as".
For more details on the zebrafish metabolic model, check the paper by van Steijn et al. 2019[5]. We thank the authors for their kindness.

Sample data:

-"TableS2_PublicData_12915_2017_362_MOESM3_ESM_4_IDARE2_NCBIds_DESEQ2_NORMcountsLog2.xlsx" - Data from Tarifeño-Salvidia et al.[6] from the transcriptome analysis of zebrafish endocrine, acinar and duct cells. TableS2 was normalized using DESeq2[7], log2 transformed and ENSIDs converted to NCBI IDs to match the model.


## Part 1: Setting up the Network for IDARE in Cytoscape
In this example, we will set up the *zebrafish* network for use with IDARE and add some additional annotations to the network.

### 1.1 Loading the zebrafish metabolic model
Load the zebrafish network by selecting:

File --> Import --> Network --> Network from File and select the zebrafish file you downloaded.
Select the Base network view (it should have 8678 nodes and 21059 edges).

The zebrafish model does not contain info on the subsystems (pathways) reactions belongs to.
This info is needed to generate subnetworks based on pathways.
We use a csv file "ZFmodel_subsys_check.csv" (available in the [Zebrafish Data folder](https://github.com/sysbiolux/IDARE-QuickStart/tree/master/Data/zebrafish)), which contains reaction IDs in the first column (matching those used in the model) and the pathways they belong to in the second column.

Load the data into Cytoscape: File --> Import --> Table from File.
Choose 'sbml id' as 'Key Column for Network' and click 'OK'.

### 1.2 Setting up the Network for IDARE

Right click on some empty space in the network view.

Select 'Apps' --> IDARE --> 'Setup Network For IDARE'  
A popup will appear that lets you select the properties to be used in IDARE.  
On the left you can select the columns you want to use for the setup. On the right, depending on your choice of columns, you can select the values to be used for compound and interaction.  
Select 'sbml type' as column to determine node types and the 'sbml id' as column to determine the node names.  
As compound node value select 'species', and as interaction node value select 'reaction'.  
Check the 'Overwrite existing values' checkbox.  

### 1.3 Changing to the IDARE Visual Style
From the 'Style' tab in the 'Control Panel' select the 'IDARE Visual Style' and apply it to the network. 

Note, that the IDARE images cannot be removed from this style. 

If you want to use the images with another style, you can add and remove them by right-clicking in the display area, and select 
Apps --> Add/Remove IDARE Images

### 1.4 Adding SBML Annotations and Gene Nodes
Right click on some empty space in the network view.  
Select 'Apps' --> 'Add SBML Annotations'

Tick the boxes for "Should FBC nodes and edges be removed" and "Do you want to generate gene and Protein nodes?" and click 'OK'.
You will be asked, whether you want to add Gene Nodes. Tick the box (if not done already) and click Ok.

You will notice that additional nodes and edges have been created (total of 10331 nodes and 30032 edges), which represent the protein and gene nodes that were created. Proteins are created for all conjunctive GPR clauses (i.e. all possible and combinations that fulfill a GPR).  
If the SBML contains enzyme species (annotated by the "isEncodedBy" bio qualifier), you would be asked which labeling pattern (i.e. database) to use for the genes and proteins.


## Part2: Automated image generation and loading images to the Network.
In this part you will generate images based on RNA-seq data and map them to the previously created and initialized network.
This part assumes you have loaded the zebrafish model and set up the network for IDARE (see Part 1 before).

### Load Data into IDARE
In the 'Control Panel', select the IDARE tab. 

Click on 'Add Dataset'

In the dialog, click on 'Choose File'  

Select "TableS2_PublicData_12915_2017_362_MOESM3_ESM_4_IDARE2_NCBIds_DESEQ2_NORMcountsLog2.xlsx" and click 'open' 

Adjust the 'DataSet Description' to something you like, for example, "zebrafish pancreatic cell type gene expression", or the field will be updated with the file name.

As 'DataSet Type', select 'Array Dataset' (this dataset contains only one sheet). 

Check the 'Use two column headers' option, since the dataset provides id and label columns.  
Click ok.  


### 2.2 Create the Visualisation

Tick the box to 'Select' the dataset. 
In the lower left corner, (below the 'Create' and 'Preview' buttons) are indicators, how many nodes the datasets represent in total and how many shared nodes the selection contains.  

Click on 'Preview Visualization' to see an example node and legend and adjust the 'Visualization Type' to the most suitable.
Once done, click on 'Create Visualization' to display the images onto the network. You may need to zoom in several times to see the images on the gene nodes.


### 2.3 Adding IDARE images to other Styles
If you did not select the IDARE visual style, but want to use a different style for your network, you can do so by right-clicking anywhere in a network view using the style you want to add your nodes to.
Select 'Apps' --> 'Add IDARE Images'. Now the images will be shown for your choosen style.


## Part3: Subnetwork Generation

It is necessary to have the SBML annotations added to the network (step 1.4), as otherwise the corresponding table column does not exist.

### 3.1 Create subnetworks

Select 'Apps' --> 'IDARE' --> 'Create Subnetworks'  
or  
Right Click in an empty space in the network you want to create subnetworks in --> 'Apps' --> 'Create Subnetworks'  

In the resulting dialog, you can select the column to determine the node types as well as the node names.  
If the network is set up for IDARE (see Part 1), appropriate columns are chosen automatically (i.e. if you have set up the network for IDARE - Step 1.3 - you can just accept the selection).  
You further have to choose a value for nodes which form the branching points between subnetworks, and a value for nodes that form the "base" of the subnetwork.

Select reactions as branching nodes and species as subnetwork nodes and click 'OK'.   

In the next window, you can select the column that contains the subnetwork identifiers (select "subsystem") as well as the type of layout that should be used for the subnetwork (select "Keep Layout" / you can also try others).

The table allows you to select nodes, which should not branch and those which should be removed entirely  (e.g. nodes with too many connections).   
Depending on the network size some nodes are already suggested by the tool. 

Next, select nodes that should be removed as they are highly connected (and secondary). Leave as is or tick additional boxes to remove more metabolites.

Finally, in the bottom part, select which subnetworks to generate (all for a thorough exploration or just a few cases). You can scroll down and you will see a button for "Select All". Make your selection(s) and click 'OK', the different subnetworks will be generated.

We have manually laid out the Citric acid cycle subnetwork, which looks like this: ![Alt text](Data/zebrafish/ZF_CitricAcidCycle.svg?raw=true "pancreatic cell-types TCA cycle")

Another nice functionality from IDARE2 is the ability to generate the legend and metanode images, which are saved as png.
This can be useful to further select or highlight specific metabolic genes with interesting data patterns.
In our example above of the TCA cycle, although we do not see striking differences in the image, there are actually interesting differences in the expression of genes between endocrine, acinar and duct cells.


# Citations
[1]Horinouchi T, Tamaoka K, Furusawa C, Ono N, Suzuki S, Hirasawa T, Yomo T, Shimizu H (2010) Transcriptome analysis of parallel-evolved escherichia coli strains under ethanol stress. BMC Genomics. 11(1):579, , URL http://dx.doi.org/10.1186/1471-2164-11-579  
[2]Wang J, Chen L, Tian X, Gao L, Niu X, Shi M, Zhang W (2013) Global metabolomic and network analysis of escherichia coli responses to exogenous biofuels. Journal of Proteome Research. 12(11):5302–5312, , URL http://dx.doi.org/10.1021/pr400640u, pMID: 24016299  
[3] Goodarzi H, Bennett BD, Amini S, Reaves ML, Hottes AK, Rabinowitz JD, Tavazoie S (2010) Regulatory and metabolic rewiring during laboratory evolution of ethanol tolerance in e. coli. Molecular Systems Biology. 6(1):378–n/a, , URL http://dx.doi.org/10.1038/msb.2010.33, 378  
[4]Soufi B, Krug K, Harst A, Macek B (2015) Characterization of the e. coli proteome and its modifications during growth and ethanol stress. Frontiers in Microbiology. 6:103, , URL http://journal.frontiersin.org/article/10.3389/fmicb.2015.00103  
[5] van Steijn L, Verbeek FJ, Spaink HP, Merks RMH (2019) Predicting Metabolism from Gene Expression in an Improved Whole-Genome Metabolic Network Model of Danio rerio. Zebrafish. Aug;16(4):348-362. doi: 10.1089/zeb.2018.1712. PMID: 31216234; PMCID: PMC6822484.  
[6] Tarifeño-Saldivia, E., Lavergne, A., Bernard, A. et al. (2017) Transcriptome analysis of pancreatic cells across distant species highlights novel important regulator genes. BMC Biol. 15, 21. https://doi.org/10.1186/s12915-017-0362-x  
[7] Love, M.I., Huber, W. & Anders, S. (2014) Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome Biol 15, 550. https://doi.org/10.1186/s13059-014-0550-8  



