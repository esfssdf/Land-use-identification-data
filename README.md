# Land Use Identification Data

## Data Structure

```
data/
├── node_attributes.csv          # Node attributes
├── transition_matrix.csv        # Third-order transition matrix
├── trajectory.csv               # Trajectory Pathway Data
├── poi_remote_features.csv      # POI & remote sensing features
├── hypergraph_edges.csv         # Hyperedge definitions
├── spatial_network.csv          # Spatial edges with weights
├── ground_truth_labels.csv      # Ground truth land use proportions
└── TAZ.zip      		             # Traffic Analysis Zone
```

## Data Files Description

### 1. node_attributes.csv (HIM Module Input)
Contains 36-dimensional node feature vectors for Traffic Analysis Zones (TAZ, stored in the “TAZ.zip” file) with columns for TAZ node ID plus 36 feature columns. The data is derived from mobile phone signaling records from telecommunications operators. The original mobile phone data is pre-processed into aggregated spatiotemporal stay patterns with all user identifiers removed for privacy protection, then transformed into composite activity state vectors that capture "past-present" activity combinations used by the Higher-order Inference Module. Note: The provided CSV file contains simulated data for demonstration purposes.

### 2. transition_matrix.csv (HIM Module)
Represents a 36×36 third-order transition matrix where both rows and columns correspond to TAZ activity state combinations. The matrix is constructed from approximately 4.7 million Weibo public posts with geographic check-in information collected from the Weibo platform (https://weibo.com/) between April 2019 and April 2024. The processing involves extracting consecutive check-in sequences from user posts, applying sliding windows of length 3 to generate activity triplets, counting occurrence frequencies, to form the final transition matrix that captures chain-based higher-order dependencies in residents' travel patterns. Note: The provided CSV file contains simulated data for demonstration purposes.

### 3. trajectory.csv (HIM Module)
Contains the IDs of Traffic Analysis Zones (TAZ) traversed by the movement trajectory (stored in the “TAZ.zip” file). The fields “TAZ1”, “TAZ2”, ‘TAZ3’, and “TAZ4” respectively represent the IDs of the TAZs where the trajectory sequentially passes through. Due to privacy protection policies, the provided CSV file contains simulated data for demonstration purposes.

### 4. poi_remote_features.csv (HTLM Module)
Contains 64-dimensional features combining POI semantic and remote sensing visual features, with columns for TAZ_ID plus 64 feature columns (32 POI features + 32 remote sensing features). The POI data is obtained from Amap services (https://ditu.amap.com/) including POI records, which are processed using Place2Vec embeddings on Delaunay triangulation networks to generate semantic features. The remote sensing component uses cloud-free Landsat 8 imagery available from USGS Earth Explorer (https://earthexplorer.usgs.gov/), from which GIST descriptors are extracted to characterize global spatial layout across five dimensions (naturalness, openness, roughness, expansion, ruggedness), and these features are concatenated to create the final 64-dimensional vectors for the Higher-order Temporal Learning Module. Note: The provided CSV file contains simulated data for demonstration purposes.

### 5. hypergraph_edges.csv (HTLM Module)
Defines hyperedge structures connecting functionally similar regions, with a column containing FID_TAZ values as underscore-separated node IDs that define hyperedges. The data is derived from human mobility patterns extracted from mobile phone signaling data by decomposing pedestrian flow curves into 8 temporal views with 3-hour windows, applying DBSCAN clustering within each view to identify homogeneous functional clusters, and forming hyperedges from each density cluster that connects multiple TAZ nodes sharing similar temporal flow patterns despite spatial separation. Note: The provided CSV file contains simulated data for demonstration purposes.

### 6. spatial_network.csv (SLM Module)
This data was derived from the spatial distances between adjacent TAZs in Shanghai and a Gaussian decay function to simulate the proximity similarity between urban blocks (for the SLM). The actual Shanghai TAZ shp data is stored in the “TAZ.zip” file within the same folder path. This data comprises three columns: TAZID1 (TAZ1 ID), TAZID2 (TAZ2 ID), and disEffect (weight).

### 7. ground_truth_labels.csv (Ground Truth Labels)
Provides land use proportion distributions for model training and validation, containing columns for TAZID plus 6 land use type proportion columns (commercial, resident, industrial, public, transport, other). The data is derived from urban functional classification data from Peking University's Urban Landscape Foundation Dataset (http://geoscape.pku.edu.cn/en.html), which was produced based on remote sensing data and POI data with manual correction in 2019. The original 9 land use types are reclassified by merging water bodies, forests/green spaces, farmland, and undeveloped areas into an "other" category, and area proportions are calculated for each land use type within every TAZ boundary to generate 6-dimensional proportion vectors. Note: The provided CSV file contains simulated data for demonstration purposes.


