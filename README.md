# IDARE Quickstart guide

This Repository contains basic sample datasets to use with the IDARE cytoscape application.
The data can be used with the *E.coli* core network sbml file obtainable from the [Data folder](https://github.com/sysbiolux/IDARE-QuickStart/tree/master/Data), 
or by right-clicking on this [Link](https://raw.githubusercontent.com/sysbiolux/IDARE-QuickStart/master/Data/ecoli_core_model.xml) and selecting "save as".
Also obtain the excel files in this repository for some sample data.

The following files can be downloaded via the repository zip file, or from the [Data Folder](https://github.com/sysbiolux/IDARE-QuickStart/tree/master/Data):  
Fold Change of Gene Expression ethanol vs no ethanol.xls - Data from Horinouchi et al.[1] used to calculate Gene expression fold changes between untreated and ethanol treated samples  
Metabolomics fold change treatment vs Control.xls - Data from Wang et al[2] for Metabolomics fold changes of treatment vs control samples  
Total relative C13 label.xls - Data from Goodarzi et al.[3] for C13 labeling upon feeding with ethanol for a wild type and a ethanol resistant starin. The data is computed to present the total amount of labeled carbons in each metabolite.  
Proteomics Time course of ethanol treated wild type.xls - Data from Soufi et al.[4] epresenting the fold change to time 0 of different protein products associated with the genes in the network.  

The Tutorial Contains three parts.  
In Part 1, we set up the network, which is necessary for image matching.  
In Part 2, we add images based on the data to the network.  
In Part 3, we will split the network into parts based on compartments and pathways.  

## Part 1: Setting up the Network for IDARE in Cytoscape
In this example, we will set up the *E.Coli* core network for use with IDARE and add some additional annotations to the network.

#### 1.1 Loading the E.Coli Core model
After installing the IDARE app (either from the Cytoscape app store or from [here](http://idare.uni.lu/IDAREJars/IDARE.jar)), load the *E.coli* core network by selecting
File -> Import -> Network -> File and select the *E.Coli* core file you downloaded.

#### 1.2 Generating an initial Layout
From the Layout menu, select a layout you want to apply (for this tutorial we assume, that the **y-Files organic layout** was choosen, or no additional layout if you have cy3sbml installed).

#### 1.3 Setting up the Network for IDARE

Right click on some empty space in the network view.  
Select 'Apps' -> 'Setup Network For IDARE'  
A popup will appear that lets you select the properties to be used in IDARE.  
On the left you can select the columns you want to use for the setup. On the right, depending on your choice of columns, you can select the values to be used for compound and interaction.  
Select the sbml type column as column to determine node types and the sbml id column as column to determine the node names.  
As compound node value select species, and as interaction node value select reaction.  
Check the 'Overwrite existing values' checkbox.  
If you are using cy3sbml please select the Base network view (it should have 187 nodes and 380 edges).

#### 1.4 Changing to the IDARE Visual Style
From the 'Style' tab in the 'Control Panel' select the 'IDARE Visual Style' and apply it to the network. Note, that the IDARE images cannot be removed from this style. If you want to use the images with another style, you can add and remove them by right-clicking in the display area, and select Apps->Add/Remove IDARE Images

#### 1.5 Adding SBML Annotations and Gene Nodes
Right click on some empty space in the network view.  
Select 'Apps' -> 'Add SBML Annotations'  
Select the *E.Coli* core network file again. If you have cy3sbml this step is not necessary, as IDARE will automatically use the SBML structure provided by cy3sbml for the network.  
You will be asked, whether you want to add Gene Nodes. Tick the box (if not done already) and click Ok.  
You will notice, that additional nodes have been created, which represent the protein and gene nodes that were created. Proteins are created for all conjunctive GPR clauses (i.e. all possible and combinations that fulfil a GPR).  
If the SBML contains enzyme species (annotated by the "isEncodedBy" bio qualifier), you would be asked which labeling pattern (i.e. database) to use for the genes and proteins.


## Part2: Automated image generation and loading images to the Network.
In this example you will generate a few images based on artificial data and map it to the previously created and initialized network.
This example assumes that you have at loaded the E.coli core network and set up the network for IDARE (Steps 1 and 3 of the previous example).

#### Load Data into IDARE
In the 'Control Panel', select the IDARE tab.  
Click on 'Add Dataset'  
In the dialog, click on 'Choose File'  
Select "Fold Change of Gene Expression ethanol vs no ethanol.xls" and click 'open'  
The 'DataSet Description' field will be updated with the file name  
As 'DataSet Type', select 'Array Dataset' (since this dataset contains only one sheet).  
Check the 'Use two column headers' option, as we only have one header.  
Click ok.  

Repeat the process with "Proteomics Time course of ethanol treated wild type.xls" and "Total relative C13 label.xls" which should both be loaded as 'Multiarray Dataset'.
The Proteomics time course only contains one sheet, so it can be added both as multiarray and array dataset. Adding it in both versions allows the selection of additional layouts (those specific to multiarray datasets and those specific to array datasets).  
Finally repeat the process with "Metabolomics fold change treatment vs Control.xls" which should be loaded as 'Array Dataset'.  
The labeling data only has one column for its IDs, thus you should uncheck the box for two column headers.
Example descriptions would be "External Metabolite amount information" and "Reaction Activity during the experiment" and "Internal Metabolite Amounts at specific stages".

#### 2.2 Create the Visualisation
You can now select individual datasets you want to visualize. 
To do so, choose a selection of datasets from the table by ticking the "selected" boxes.  
In the lower left corner, (below the 'Create' and 'Preview' buttons) are indicators, how many nodes the datasets represent in total and how many shared nodes the selection contains.  
When creating visualisations for multiple datasets at once, there should be an overlap of the sets, otherwise parts of the generated image will always stay blank since no data is available.
However, you can of course create multiple different nodes by selecting different sets of data.
Images will only be created for nodes which are part of a selected dataset and old images will be kept unless a new image is created for the node.

#### 2.3 Adding IDARE images to other Styles
If you did not select the IDARE visual style, but want to use a different style for your network, you can do so by right-clicking anywhere in a network view using the style you want to add your nodes to.
Select 'Apps' -> 'Add IDARE Images'. Now the images will be shown for your choosen style.


## Part3: Subnetwork Generation

The first step of this this Example is independent on the first 2 Examples.
For the second step, it is necessary to have the SBML annotations added to the network (step 1.5), as otherwise the corresponding table column does not exist.
You can add the sbml annotation to the Cytosol network (C_c) after step 1 of this example, but this would lead to gene nodes not being added in the external compartment subnetwork.

#### 3.1 Create Networks for the External compartment and the cytosol
NOTE: If you want to recreate Figure 4 of the Paper, skip the creation of the Compartment subnetworks, as they were not created for that figure.  
We first want to split the network at the transporters which translocate metabolites from the external compartment to the cytosol, and vice versa.  
To do so:  
Select 'Apps' -> 'IDARE' -> 'Create Subnetworks'  
or  
Right Click in an empty space in the network you want to create subnetworks in -> 'Apps' -> 'Create Subnetworks'  
In the resulting dialog, you can select the column to determine the node types as well as the node names.  
If the network is set up for IDARE (see Part 1), appropriate columns are chosen automatically (i.e. if you have set up the network for idare - Step 1.3 - you can just accept the selection) .  
You further have to choose a value for nodes which form the branching points between subnetworks, and a value for nodes that form the "base" of the subnetwork.
Since the aim is to branch at the transporters between compartments reactions will be used as branching nodes and species as subnetwork nodes.   
Click accept, when you finished your selection.  
In the next dialog, you can select the column that contains the subnetwork identifiers as well as the type of layout that should be used for the subnetwork (or the "keep layout" option, if the current layout shoudl be kept)
The table allows you to select nodes, which should not branch and those which should be removed entirely  (e.g. nodes with too many connections).   
Depending on the network size some nodes are already suggested by the tool.  
Since we want to create the networks for the cytosol and external compartment, we select "sbml compartment" as column to determine sub-networks.  
We leave the 'Keep Layout' option as it is and select remove for the Biomass reaction (as this reaction mainly leads to a hairball structure).  
We also select C\_c, the cytosol, and C\_e, the external medium, in the sub-networks to be generated table.   
If we now click on the "network" tab in the 'Control Panel' we will see two new networks that were generated.   

#### 3.2 Create pathway networks in the Cytosol
There is not much to see in the external compartment, as it only contains the transporters and the cytosol is still a hairball. 
To get a better overview, we would like to generate pathway networks in the cytosol.  
To do so, again, select 'Apps' -> 'SubNetworkGenerator'  
However, this time, since reactions belong to pathways and metabolites can be shared between different pathways, we select species as values for branching nodes and reaction as value for subnetwork nodes.  
By default, the "COBRA_SUBSYSTEM" column generated by the SBMLAnnotator is selected, if available, so this selection is already fine.  
From the metabolites we would like to exclude currency metabolites and most cofactors.  
For larger networks, the pre-selection is better, but on small networks (like the e.coli core), the relative amounts of presence tend to be higher, so we have to select additional nodes to remove during the generation process.  
Select M\_h\_c, M\_h2o\_c, M\_atp\_c, M\_pi\_c, M\_pi\_c, M\_nadh\_c, M\_nad\_c, M\_adp\_c, M\_nadph\_c, M\_co2\_c, M\_nadp\_c and M\_coa\_c, as metabolites to remove.  

As pathways, we would like to create 'Glycolysis', the 'Citric Acid Cycle', 'Glyoxylate Shunt' and the 'Pentose phosphate pathway'.  
Click ok.  

You can now navigate through the different pathways. If you wish to create additional pathways, select the source network again and repeat the steps.
New links will be added to the existing networks.

You will notice, that links from parental networks will be included in the newly created networks, but no links are generated in these parental networks. 
In general, links will be generated for all networks built on the same column, that share linking nodes. 
If you create additional subnetworks at a later stage, links to and from old networks will be created for all newly generated networks.

You can see, that the labelling for ketoglutarate is much smaller than the labelling of any other measured metabolite. One potential reason is an increase in the glyoxylate shunt (visible in both the proteomics and gene expression of the enzymes involved in that pathway). 



## Citations
[1]Horinouchi T, Tamaoka K, Furusawa C, Ono N, Suzuki S, Hirasawa T, Yomo T, Shimizu H (2010) Transcriptome analysis of parallel-evolved escherichia coli strains under ethanol stress. BMC Genomics 11(1):579, , URL http://dx.doi.org/10.1186/1471-2164-11-579  
[2]Wang J, Chen L, Tian X, Gao L, Niu X, Shi M, Zhang W (2013) Global metabolomic and network analysis of escherichia coli responses to exogenous biofuels. Journal of Proteome Research 12(11):5302–5312, , URL http://dx.doi.org/10.1021/pr400640u, pMID: 24016299  
[3] Goodarzi H, Bennett BD, Amini S, Reaves ML, Hottes AK, Rabinowitz JD, Tavazoie S (2010) Regulatory and metabolic rewiring during laboratory evolution of ethanol tolerance in e. coli. Molecular Systems Biology 6(1):378–n/a, , URL http://dx.doi.org/10.1038/msb.2010.33, 378  
[4]Soufi B, Krug K, Harst A, Macek B (2015) Characterization of the e. coli proteome and its modifications during growth and ethanol stress. Frontiers in Microbiology 6:103, , URL http://journal.frontiersin.org/article/10.3389/fmicb.2015.00103  




