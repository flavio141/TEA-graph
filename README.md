Graph Deep Learning on WSIs
=====================

## Get Started & Dependencies
In order to work with this code, we need to install the dependencies.
### Windows
1. Microsoft Visual Studio C++ 2015,2017,2019 at this link: https://www.microsoft.com/it-it/Download/confirmation.aspx?id=48145;
2. Torch with this command in the terminal: pip install torch==1.9.1+cu111 torchvision==0.10.1+cu111 torchaudio==0.9.1 -f https://download.pytorch.org/whl/torch_stable.html;
3. Run: pip install -r requirements.txt;
4. Finally run: pip install torch-cluster==1.5.9 -f https://data.pyg.org/whl/torch-1.9.1+cu111.html and after pip install torch-spline-conv==1.2.1 -f https://data.pyg.org/whl/torch-1.9.1+cu111.html.

## Step 1: Processing whole slide image (WSI) into superpatch-graph
#### What is the superpatch-graph?
* Superpatch-graph is the compressed representation of whole slide image into graph structure in memory efficient manner.
* Run the ./Superpatch_network_construction/supernode_generation.py
  * Users can simply run the above script with pre-defined sample data
  * Or, users can use your own whole slide image by setting the "--graphdir"
  * Output files
    * Compressed network as ".pt"
    * Node position information in "_node_location_list.csv"
    * Superpatch aggregated dictionary in "_artifact_sophis_final.csv"

## Step 2: Training TEA-graph using superpatch-graph
* Users can predict the prognosis of entire host with tumor environment-associated context analysis using deep graph learning (TEA-graph)
* Run the ./main.py with appropriate hyperparameters
  * Users can simply run the above script with pre-defined parameters and datasets
  * Or, users can use their own dataset preprocessed by "supernode_generation" script

## Step 3: Visualization of IG (Integrated gradients) value on WSI
* Users can visualize the IG value which is highly correlated with risk value of each region in WSI
* Also, we provide subgraph-level contextual pathological feature extraction
* Run the ./IG_attention_feature_cal_main.py with same parameters you used for training your own TEA-graph model
  * Users must define the trained_parameters as "--load_state_dict"
  * "IG_analysis" directory is created inside the directory you choose as the "--load_state_dict"
    * Subfolder for each patient is created inside the "IG_analysis" 
    * "attention_value.npy" indicates the edge-level attention value
    * "Node_IG_sophis.npy" indicates the node-level IG value
    * "whole_feature.npy" is the trained contextual feature through GNN
    * "_WSI_Image_mask_IG_new.gif" is the heatmap of IG value on WSI
    * "_WSI_graph_wo_IG.jpeg" is the superpatch-graph
    * "_WSI_graph_w_IG.jpeg" is the IG value colored superpatch-graph
  * "IG_again" directory is also created inside each patient's folder
    * "_IG_TME_subgraph.csv" indicates the each IG group's subgraph

## Step 4: Biomarker discovery
* Users can extract the contextual biomarker using the calculated IG values and extracted feature at the previous step
* Run the ./Context_marker_discovery_main.py with approprate directory path
* Users can obtain the several candidate pathology images with visualized graph for contextual biomarker


## Acknowledgments
* http://github.com/mahmoodlab/Patch-GCN
* http://github.com/lukemelas/EfficientNet-PyTorch
* http://github.com/pyg-team/pytorch_geometric

BiNEL (http://binel.snu.ac.kr) - This code is made available under the MIT License and is available for non-commercial academic purposes
