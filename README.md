Note: The cleaned structured metadata file (clean_metadata_v2.parquet) is stored in the shared Expanse Lustre file system and can be accessed at:
/expanse/lustre/projects/uci150/hzhao16/clean_metadata_v2.parquet


Structured Data - Feature Engineering & First Model
 
Feature Selection and Cleaning(Preprocessing)
 
1.* Selected relevant structured features including:
 
*'coord-lat','coord-lon','image_measurement_value','area_fraction','scale_factor'
    * Later added:'sequence_length','inferred_ranks'
 
2.* Imputed missing values using:
 
  * Mode for categorical fields (e.g.,'country','family')
  * Median for numerical fields (e.g.,'image_measurement_value')
 
3.* Dropped low-utility columns such as 'subfamily','species', and others with excessive missingness
（There already included some preprocessing steps in our milestone2）
 
Model Training: Random Forest Classifier
 
* Label: 'order' (taxonomic order of the insect)
* Model: PySpark ML Random Forest Classifier
* Features assembled into a single 'features' column using'VectorAssembler'
* Used 'StringIndexer' to convert 'order' to indexed 'label'


Image Classification by utilizing CNN (Convolutional Neural Network) model
*PyTorch
	*the network architecture consisted of two convolutional blocks
	
 
Train/Test Split
 
* Split structured metadata into:
 
   * 80% training
   * 20% test
 
* Random seed set to '42' for reproducibility
 
 Evaluation
 
| Dataset | Accuracy | Macro F1 Score |
| ------- | -------- | -------------- |
| Train   | 0.7552   | 0.6772         |
| Test    | 0.7556   | 0.6777         |
 



Split cropped image sets into train, validation, and test splits (70/15/15) ratio. This will be used to test out accuracy and macro-F1 scores

Fit Diagnosis
 
* Training and testing scores are very close, suggesting:
 
  * No overfitting
  * No underfitting
 
* Model is in a 'good fit' region of the fitting graph
 
Conclusion
 
The first model based on structured metadata achieved decent performance (accuracy ≈ 0.76, macro-F1 ≈ 0.68).
 
The model is well-generalized and does not overfit, as train and test results are almost identical.
 
To improve the model further:
 
Incorporate more relevant features (e.g., DNA or image embeddings)
Optimize hyperparameters (e.g., tree depth, number of trees)
Use ensemble methods such as GBT or LightGBM for potential performance gain.
 
 
Due to repeated SDSC JupyterLab proxy timeout errors, and Expanse Lustre file system issues, several components of image processing of Milestone3 could not be fully completed. Specifically: 
	File operations with cropped image folders 
	Model training for CNN model and execution
Validation and evaluation of CNN models 
These components will be re-access once SDSC server access is stable and restored. 

