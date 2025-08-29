# Land Use Identification Data

## Data Structure

```
data/
├── node_attributes.csv          # Node attributes (HIM input features)
├── transition_matrix.csv        # Third-order transition matrix
├── poi_remote_features.csv      # POI & remote sensing features (HTLM)
├── hypergraph_edges.csv         # Hyperedge definitions (HTLM)
├── spatial_network.csv          # Spatial edges with weights (SLM)
└── ground_truth_labels.csv      # Ground truth land use proportions
```

## Data Description

**Important Note**: All CSV files in this repository are simulated/sample data for demonstration purposes only. Users need to prepare their own real datasets following the processing guidelines below.

The HMGNN framework requires 6 input CSV files, each derived from specific data sources as described in the original research:

### 1. node_attributes.csv (HIM Module Input)
This file contains 36-dimensional node feature vectors for Traffic Analysis Zones (TAZ) with columns for TAZ node ID plus 36 feature columns. The data is derived from mobile phone signaling records from telecommunications operators. The original mobile phone data is pre-processed into aggregated spatiotemporal stay patterns with all user identifiers removed for privacy protection, then transformed into composite activity state vectors that capture "past-present" activity combinations used by the Higher-order Inference Module.

### 2. transition_matrix.csv (HIM Module)
This file represents a 36×36 third-order transition matrix where both rows and columns correspond to TAZ activity state combinations. The matrix is constructed from approximately 4.7 million Weibo public posts with geographic check-in information collected from the Weibo platform (https://weibo.com/) between April 2019 and April 2024. The processing involves extracting consecutive check-in sequences from user posts, applying sliding windows of length 3 to generate activity triplets, counting occurrence frequencies, to form the final transition matrix that captures chain-based higher-order dependencies in residents' travel patterns.

### 3. poi_remote_features.csv (HTLM Module)
This file contains 64-dimensional features combining POI semantic and remote sensing visual features, with columns for TAZ_ID plus 64 feature columns (32 POI features + 32 remote sensing features). The POI data is obtained from Amap services (https://ditu.amap.com/) including POI records, which are processed using Place2Vec embeddings on Delaunay triangulation networks to generate semantic features. The remote sensing component uses cloud-free Landsat 8 imagery available from USGS Earth Explorer (https://earthexplorer.usgs.gov/), from which GIST descriptors are extracted to characterize global spatial layout across five dimensions (naturalness, openness, roughness, expansion, ruggedness), and these features are concatenated to create the final 64-dimensional vectors for the Higher-order Temporal Learning Module.

### 4. hypergraph_edges.csv (HTLM Module)
This file defines hyperedge structures connecting functionally similar regions, with a column containing FID_TAZ values as underscore-separated node IDs that define hyperedges. The data is derived from human mobility patterns extracted from mobile phone signaling data by decomposing pedestrian flow curves into 8 temporal views with 3-hour windows, applying DBSCAN clustering within each view to identify homogeneous functional clusters, and forming hyperedges from each density cluster that connects multiple TAZ nodes sharing similar temporal flow patterns despite spatial separation.

### 5. spatial_network.csv (SLM Module)
This file contains spatial edges with distance-based weights in three columns: TAZID1, TAZID2, and disEffect. The data is generated from geographic coordinates of TAZ boundaries obtained from administrative databases available through government open data portals, by constructing a Delaunay triangulation network using TAZ centroids, creating edges between adjacent TAZ pairs, calculating Euclidean distances between connected centroids, and applying Gaussian decay functions to generate distance effect weights that model proximity similarity between urban blocks for the Spatial Learning Module.

### 6. ground_truth_labels.csv (Ground Truth Labels)
This file provides land use proportion distributions for model training and validation, containing columns for TAZID plus 6 land use type proportion columns (commercial, resident, industrial, public, transport, other). The data is derived from urban functional classification data from Peking University's Urban Landscape Foundation Dataset (http://geoscape.pku.edu.cn/en.html), which was produced based on remote sensing data and POI data with manual correction in 2019. The original 9 land use types are reclassified by merging water bodies, forests/green spaces, farmland, and undeveloped areas into an "other" category, and area proportions are calculated for each land use type within every TAZ boundary to generate 6-dimensional proportion vectors.
