# IDARE Example

This Repository contains basic **artificial** sample data and files to use with the IDARE cytoscape application.
The data can be used with the *E.coli* core network sbml file obtainable from the [UCSD website](http://systemsbiology.ucsd.edu/Downloads/EcoliCore), 
or by right-clicking on this [Link](http://systemsbiology.ucsd.edu/sites/default/files/Attachments/Images/downloads/Ecoli_core/ecoli_core_model.xml) and selecting "save as".
Also download the Files contained in the Data Folder in this repository.

Activity.xls - Predicted Activity in four different phases of the experiment
External Metabolites.xls - Data for 3 external Metabolites (and the biomass amount) over a period of time
Internal Metabolites.xls - Data for 3 internal Metabolites at different textual stages.
ReactionActivity.xls - experimental and predicted activity of each reaction in the network

The Tutorial Contains three parts. 
In Part 1, we set up the network, which is necessary for image matching.
In Part 2, we add images based on the data to the network.


## Part 1: Setting up the Network for IDARE in Cytoscape

#### Loading the E.Coli Core model
After installing the IDARE app (either from the Cytoscape app store or from [here](http://idare-server.uni.lu/IDARE.jar)), load the E.coli core network by selecting
File -> Import -> Network -> File and select the ecoli core file you downloaded.

#### Generating an initial Layout
From the Layout Menu, select a Layout you want to apply (for this tutorial we assume, that the y-Files organic layout was choosen).

#### Setting up the Network for IDARE

Right click on some empty space in the Network view.
Select Apps -> Setup Network For IDARE
A popup will appear That lets you select the Properties to be used in IDARE.
On the left you can select the columns you want to use for the setup. On the right, depending on your choice of columns, you can select the values to be used for compound and interaction.
Select the sbml type column as column to determine node types and the sbml id column as column to determine the node names.
As Compound node value select species, and as interaction node value select reaction.
Check the Overwrite existing values checkbox.

#### Changing to the IDARE Visual Style
From the Style Tab in the Control Panel select th IDARE Visual Style and apply it to the network.

#### Adding SBML Annotations and Gene Nodes
Right click on some empty space in the Network view.
Select Apps -> Add SBML Annotations
Select the ecoli core network file again. If you have cy3sbml this step is not necessary, as Cytoscape will automatically use the SBML Structure provided by cy3sbml for the network.
You will be asked, whether you want to add Gene Nodes. Click Yes
You will notice, that additional nodes have been created, which represent the Gene Nodes.

##Part2: Automated image generation and loading images to the Network.

####Load Data into IDARE
In the Control Panel, select the IDARE tab.
Click on 'Add Dataset'
In the Dialog, click on "Choose File"
Select "Activity.xls" and click "open"
In the DataSet Description Field: Write the text: "Indication of Reaction Activity at different simulation phases"
As DataSet Type, select Itemized Dataset (since this DataSet contains only one sheet).
Uncheck the "Use two column headers" option, as we only have one header.
Click ok.

Repeat the process with "External Metabolites.xls" and "ReactionActivity.xls" which should both be loaded as ValueSet Datasets.
Even though ReactionActivity.xls contains only one sheet, the number of datapoints is unsuitable for itemized representation. 
Thus importing it as a ValuesetDataset with one set allows it to be displayed e.g. as a Graph.
Finally repeat the Process with "Internal Metabolites.xls" which should be loaded as Itemized Dataset and is a two column header file, so the checkbox has to be ticked.
Example Descriptions would be "External Metabolite amount information" and "Reaction Activity during the experiment" and "Internal Metabolite Amounts at specific stages".

#### Create the Visualisation
You can now select individual DataSets you want to visualize. 
To do so, choose a selection of DataSets from the Table by ticking the "selected" boxes.
In the lower left corner, (below the Create and Preview buttons) are indicators, how many nodes the datasets represent in total and how many shared nodes the selection contains.
When creating visualisations for multiple datasets at once, there should be an overlap of the sets, otherwise parts of the generated image will always stay blank since no data is available.
However, you can of course create multiple different nodes by selecting different sets of data. 
Images will only be created for nodes which are part of a selected dataset and old images will be kept unless a new image is created.


##Part3: Subnetwork Generation

This tutorial is independent on the first 2 tutorials. It is however strongly recommended to add the SBML annotation and to set up the network for use with idare prior to creating subnetworks.
SBML annotation can e.g. not be carried out for subnetworks and gene nodes will therefore not be included in a subnetwork, when added to the parental network. 

#### Create Networks for the External compartment and the cytosol
We first want to split the network at the transporters which translocate metabolites from the external compartment to the cytosol, and vice versa.
To do so:
Select Apps -> SubNetworkGenerator
In the resulting dialog, you can select the column to determine the node types as well as the node names.
If the network is set up for IDARE (see Part 1), the corresponding columns choosen there are automatically selected.
You further have to choose a value for nodes which form the branching points between subnetworks, and a value for nodes that form the "base" of the subnetwork.
Since the aim is to branch at the transporters between compartments reactions will be used as branching nodes and species as subsystem nodes. 
Click accept, when you finished your selection.
In the next dialog, you can select the column that contains the subsystem identifiers as well as the type of layout that should be used for the subsystems (or the "keep layout" option, if the current layout shoudl be kept)
The table allows you to select nodes, which should not branch and those which should be removed entirely  (e.g. nodes with too many connections). 
Depending on the network size some nodes are already suggested by the Tool.
Since we want to create the networks for the cytosol and external compartment, we select "sbml compartment" as Column to determine Subnetworks.
We leave the Keep Layout option as it is and select remove for the Biomass reaction (as this reaction mainly leads to a hairball structure).
We also select C\_c,  the cytosol, and C\_e, the external medium, in the Subsystems to be generated table. 
If we now click on the "network" tab in the Control panel we will see two new networks that were generated. 

#### Create pathway networks in the Cytosol
There is not much to see in the External compartment, as it only contains the transporters and the Cytosol is still a hairball. 
To get a better overview, we would like to generate pathway networks in the Cytosol.
To do so, again, select Apps -> SubNetwork Generator
However, this time, since reactions belong to pathways and metabolites can be shared between different pathways, we select species as values for branching nodes and reaction as value for subsystem nodes.
By default, the "COBRA_SUBSYSTEM" column generated by the SBMLAnnotator is selecte,d if available, so this selection is already fine.
From the metabolites we would like to exclude currency metabolites and most cofactors. 
For larger networks, the pre-selection is better, but on small networks (like the e.coli core), the relative amounts of presence tend to be higher, so we have to select additional nodes to remove during the generation process.
Select M\_h\_c, M\_h2o\_c,M\_atp\_c,M\_pi\_c,M\_pi\_c,M\_nadh\_c,M\_nad\_c,M\_adp\_c,M\_nadph\_c,M\_co2\_c,M\_nadp\_c and M\_coa\_c, as metabolites to remove.

As pathways, we would like to create Glycolysis, the Citric Acid Cycle, Anaplerotic reactions and the pentose phosphate pathway.
Click ok.

You can now navigate through the different pathways. If you wish to create additional pathways, select the source network again and repeat the steps.
New links will be added to the existing networks.

You will notice, that links from parental networks will be included in the newly created networks, but no links are generated in these parental networks. 
In general, links will be generated for all networks built on the same column, that share linking nodes.  





