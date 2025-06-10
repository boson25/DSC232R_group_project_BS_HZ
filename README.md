# BIOSCAN-5M: Large-Scale Insect Species Classification and Ecological Trait Analysis

## Overview

The **BIOSCAN-5M\_cropped\_256** dataset provides a comprehensive resource for advancing biodiversity informatics and ecological machine learning. With over 5 million standardized insect specimen images, DNA barcodes, taxonomic classifications, body size measurements, and geographic metadata, it enables scalable, multidimensional analysis of insect diversity.

In this project, we employ **Apache Spark** to build distributed machine learning pipelines for:

* Insect species classification
* Population structure analysis
* Ecological trait prediction

We investigate the interplay between body size, taxonomy, and environmental variables, and assess how genetic divergence correlates with spatial separation, using Spark MLlib and PySpark tools.

This work demonstrates the effectiveness of big data frameworks in extracting complex biogeographic and evolutionary patterns from large-scale ecological datasets.

---

## 🔧 Environment Setup

To process large-scale data, computations are executed on **SDSC Spark Cluster** with the following specs:

* **Account**: TG-CIS240277
* **Partition**: shared
* **Time limit**: 2000 min
* **Cores**: 15
* **Memory per node**: 240 GB
* **Modules**: singularitypro
* **Type**: JupyterLab

## 🧪 Milestone 2: Data Exploration & Preprocessing

### 1. Metadata & Image Inspection

* Load metadata fields: taxonomy, body size, location.
* Validate image files: check missing/corrupt files, inspect image sizes.
* Sample image inspection from `cropped_256/` folder.

### 2. Dataset Overview

* Inspect schema and total entries.
* Group image counts by insect order and family.

### 3. Missing Value Analysis

* Detect missing values in metadata.
* Impute missing numeric values using median or mode.
* Drop excessively missing columns.

### 4. Statistical Summaries

* Mean, median, std dev for body size, etc.

### 5. Exploratory Visualizations

* Histogram: Body size distribution
* Bar chart: Top 10 insect families
* Scatter: Specimen locations (lat/lon)
* Image grid: Random 3x3 sample grid
* Top 10 insect orders by image count

### 6. Image Preprocessing

* Resize: All images to 256×256
* Normalize: Pixel values scaled to \[0, 1]
* Augment: Random flips and rotations
* Split: Train/Val/Test = 70%/15%/15%

### 7. DNA Sequence Processing

* Tokenize barcodes into k-mers
* One-hot encode k-mers
* Remove poor quality sequences

### 8. Geographic Data

* Normalize coordinates
* Apply geohashing and K-Means clustering

### 9. Output

* Export cleaned metadata to CSV/Parquet
* Save preprocessed images

---

## 📊 Milestone 3+4: Classification Pipeline

### A. Image Classification (CNN Baseline)

* Visualized sample images by taxonomic group
* Verified image quality and structure
* Built a basic 3-epoch CNN in PyTorch/Keras as performance benchmark
* Results were limited due to data volume and lack of augmentation

### B. Structured Metadata Classification

#### Feature Engineering

* Selected numerical features:

  * `'coord-lat'`, `'coord-lon'`, `'image_measurement_value'`, `'area_fraction'`, `'scale_factor'`, `'sequence_length'`
  * Taxonomic field: `'inferred_ranks'`
* Imputation:

  * Categorical: Mode (e.g. `'family'`)
  * Numerical: Median
  * Dropped fields with >60% missing (e.g. `'subfamily'`, `'species'`)

#### Model Pipeline (PySpark)

* **Target**: `'order'` (insect order)
* **Pipeline**:

  * `VectorAssembler`
  * `StandardScaler`
  * `StringIndexer`
  * `RandomForestClassifier`
* Split: 80% training / 20% test

#### Hyperparameter Tuning

| Model | maxDepth | numTrees | Train Acc | Train F1 | Test Acc | Test F1 |
| ----- | -------- | -------- | --------- | -------- | -------- | ------- |
| 1     | 3        | 10       | 0.7509    | 0.6745   | 0.7511   | 0.6748  |
| 2     | 5        | 10       | 0.7567    | 0.6782   | 0.7570   | 0.6786  |
| 3     | 5        | 15       | 0.7573    | 0.6787   | 0.7576   | 0.6791  |

#### Feature Importance

* `sequence_length`: >80% importance
* `area_fraction`, `image_measurement_value`: moderate
* Geo features: minor

---

## ✅ Results Summary

* **Structured Model**:

  * Test Accuracy: **75.76%**
  * Test F1 Score: **0.6791**
* **Key Finding**: DNA barcode length is the most predictive feature

## ⚙️ Technical Contributions

* Scalable Spark pipeline for distributed metadata processing
* Feature cleaning and encoding for classification
* Integration of image and structured data modalities
* Visualization support for data insights

## 🔍 Limitations

* Image model trained on limited subset and epochs
* No categorical feature integration in structured model
* No data augmentation used in CNN model

## 🚀 Future Directions

* Add categorical features (e.g., `country`, `family`)
* Deepen CNN architecture, expand image training epochs
* Apply data augmentation: rotation, flipping, cropping
* Add stronger models: Gradient Boosting, MLP, Logistic Regression
* Explore multimodal fusion: combine image and structured inputs

## 📌 Conclusion

This project builds a foundational pipeline for analyzing large-scale insect biodiversity data using Apache Spark. We:

* Successfully applied Random Forest on structured features
* Identified `sequence_length` as a dominant signal
* Explored baseline CNN for image-based classification

The combination of high-dimensional genetic, geographic, and visual data shows promising potential for future ecological ML research. Further improvements can expand on data integration and model depth to build more accurate and interpretable biodiversity classification systems.
