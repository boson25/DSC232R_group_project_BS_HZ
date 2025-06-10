#  Insect Order Classification with BIOSCAN-5M Dataset  
*Using PySpark + Image + Structured Metadata*

---

##  Usage

1. **Set up the environment**  
   Make sure necessary Python packages are installed and environment variables are configured for data and model caching.

2. **Prepare the data**  
   Ensure the following files and folders are available:
   - `BIOSCAN_5M_Insect_Dataset_metadata.csv`
   - Cleaned metadata file:  
     `/expanse/lustre/projects/uci150/hzhao16/clean_metadata_v2.parquet`
   - Raw image data in the expected folder structure

3. **Run the notebook**  
   Execute `Milestone3+4.ipynb` sequentially in Jupyter Notebook. The notebook covers both structured and image-based classification.

---

##  Methodology Overview

### A. Image-Based Classification Pipeline

- **Data Loading & Inspection**  
  Metadata is loaded using PySpark. Sample images are visualized for qualitative review.

- **Image Quality Verification**  
  Images from each class are checked to ensure integrity and proper formatting.

- **Simple CNN Training**  
  A basic CNN model is trained over 3 epochs for performance benchmarking.

- **Feature Importance**  
  A structured Random Forest model evaluates feature significance, especially `sequence_length`.

---

### B. Structured Metadata Classification Pipeline

####  Feature Engineering & Cleaning

- Selected features:  
  `'coord-lat'`, `'coord-lon'`, `'image_measurement_value'`, `'area_fraction'`, `'scale_factor'`,  
  Later added: `'sequence_length'`, `'inferred_ranks'`

- Imputation:  
  - Categorical: Mode imputation (e.g., `'country'`, `'family'`)  
  - Numerical: Median imputation  
  - Dropped high-missing columns like `'subfamily'`, `'species'`

#### ️ Model Training (PySpark)

- **Target**: `'order'` (insect taxonomic order)  
- **Pipeline**:  
  `VectorAssembler` → `StandardScaler` → `StringIndexer` → `RandomForestClassifier`  
- **Split**: 80% train / 20% test (random seed 42)

####  Hyperparameter Tuning & Results


| Model | maxDepth | numTrees | Train Acc | Train F1 | Test Acc | Test F1 |
|-------|----------|----------|-----------|----------|----------|---------|
| 1     | 3        | 10       | 0.7509    | 0.6745   | 0.7511   | 0.6748  |
| 2     | 5        | 10       | 0.7567    | 0.6782   | 0.7570   | 0.6786  |
| 3     | 5        | 15       | 0.7573    | 0.6787   | 0.7576   | 0.6791  |

- All models performed consistently well with minimal signs of overfitting or underfitting.  
- **Best model: `maxDepth=5`, `numTrees=15`** with highest accuracy and F1 score on both train and test sets.

####  Feature Importance

- `sequence_length`: >80% importance  
- `area_fraction`, `image_measurement_value`: moderate importance  
- Other features (geo, inferred_ranks): minor impact

---

##  Results Summary

- **Structured Model**  
  - Best Test Accuracy: **0.7576**  
  - Best Test F1 Score: **0.6791**

- **Key Insight**:  
  DNA barcode length is a strong signal for taxonomic classification.  
  Image features provide additional context.

---

##  Technical Highlights

- Scalable Spark pipeline for metadata processing  
- Basic CNN for image classification  
- Feature engineering and standardization  

---

##  Limitations

- Image model limited to one order due to data volume  
- CNN used only 3 epochs with no augmentation  
- Structured model did not yet include categorical fields

---

##  Future Directions

- Deepen CNN architecture & increase training epochs  
- Add data augmentation (rotation, scaling, color)  
- Encode and integrate categorical metadata  
- Try stronger models: Gradient Boosting, Logistic Regression, Neural Networks  
- Explore multi-modal fusion (image + metadata)

---

## Conclusion

This project provides a baseline pipeline for classifying insect orders using both structured and unstructured data from the BIOSCAN-5M dataset. Results show that:

- The model achieves solid performance (75.8% accuracy)  
- DNA sequence length is the most predictive feature  
- Structured and image pipelines complement each other for future improvement

> Future work will involve more advanced modeling, richer feature sets, and expanded data scope.
