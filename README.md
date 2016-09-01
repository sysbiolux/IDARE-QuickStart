# IDARE Example

This Repository contains basic **artificial** sample data and files to use with the IDARE cytoscape application.
The data can be used with the *E.coli* core network sbml file obtainable from the [UCSD website](http://systemsbiology.ucsd.edu/Downloads/EcoliCore), 
or by right-clicking on this [Link](http://systemsbiology.ucsd.edu/sites/default/files/Attachments/Images/downloads/Ecoli_core/ecoli_core_model.xml) and selecting "save as".
Also download the Files contained in the Data Folder in this repository.

Activity.xls - Predicted Activity in four different phases of the experiment
MetaboliteData.xls - Data for 3 Metabolites (and the biomass amount) over a period of time
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

Repeat the process with "MetaboliteData.xls" which you should 




