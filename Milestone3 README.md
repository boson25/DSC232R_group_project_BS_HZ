# Structured Data Feature Engineering & First Model

> **Note**  
> The cleaned structured metadata file (`clean_metadata_v2.parquet`) is stored in the shared Expanse Lustre file system and can be accessed at:  
> `/expanse/lustre/projects/uci150/hzhao16/clean_metadata_v2.parquet`



##  Structured Data Preprocessing & Feature Engineering

###  Feature Selection and Cleaning

1. **Selected relevant features:**
   - `'coord-lat'`, `'coord-lon'`, `'image_measurement_value'`, `'area_fraction'`, `'scale_factor'`
   - Later added: `'sequence_length'`, `'inferred_ranks'`

2. **Missing Value Imputation:**
   - **Categorical fields:** Mode imputation (e.g., `'country'`, `'family'`)
   - **Numerical fields:** Median imputation (e.g., `'image_measurement_value'`)

3. **Dropped low-utility columns**  
   Columns like `'subfamily'`, `'species'`, and others with excessive missing values were removed.  
   *(Some preprocessing steps were previously included in Milestone 2.)*



##  Model Training: Random Forest (Structured Metadata)

- **Label**: `'order'` (taxonomic order of the insect)
- **Model**: PySpark ML Random Forest Classifier
- **Pipeline**:
  - `VectorAssembler` to combine features into a single `'features'` column
  - `StringIndexer` to encode `'order'` as the target `'label'`



##  Image Classification (CNN using PyTorch)

- **Model**: Convolutional Neural Network (CNN)
  - Architecture: Two convolutional blocks
- **Split**: Cropped image sets split into:
  - 70% training
  - 15% validation
  - 15% test
- Metrics: Accuracy and macro-F1 scores



##  Train/Test Split (Structured Metadata)

- 80% training, 20% test
- Random seed: `42` for reproducibility



##  Evaluation Results (Random Forest on Metadata)

| Dataset | Accuracy | Macro F1 Score |
| ------- | -------- | -------------- |
| Train   | 0.7552   | 0.6772         |
| Test    | 0.7556   | 0.6777         |

### Fit Diagnosis

- Training and test scores are close:
  -  No overfitting
  -  No underfitting  
→ Indicates the model is in a *"good fit"* region.



##  Conclusion

- The structured metadata-based model achieved:
  - **Accuracy**: ≈ 0.76
  - **Macro F1**: ≈ 0.68
- Well-generalized performance — minimal train-test gap.

###  Next Steps

- Add more features (e.g., DNA barcodes, image embeddings)
- Hyperparameter tuning (e.g., tree depth, number of trees)
- Try ensemble models like **GBT** or **LightGBM**



## SDSC Server Limitation Notice

Due to repeated **SDSC JupyterLab proxy timeout errors** and **Expanse Lustre file system issues**, the following tasks from Milestone 3 could not be completed:

- Accessing cropped image folders
- CNN model training and evaluation
- Validation of CNN model performance
 These components will be revisited once SDSC server access is stable and fully restored.
