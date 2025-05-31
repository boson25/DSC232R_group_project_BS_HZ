# Data Exploration and Preprocessing of Insect Species Metadata and Images (BIOSCAN-5M)

## 1. Environment setting up
Account: TG-CIS240277

Partition: shared

Time limit (min): 2000

Number of cores: 15

Memory required per node (GB): 240

Environment Modules to be loaded: singularitypro

Working Directory: home

Type: JupyterLab

<img width="1397" alt="image" src="https://github.com/user-attachments/assets/90277034-b8ca-48cd-87a9-01a570dbd4bf" />


### Data downloader 
<img width="1005" alt="image" src="https://github.com/user-attachments/assets/8eef9529-f468-4e7d-bf61-f772ce6a3b2d" />


## 2. Data Exploration and Preliminary Analysis

### **Loading Metadata**
- Load metadata including species classification, body size, and geographic location.
- Load and validate image files:
  - Check for missing or corrupted files.
  - Verify image dimensions and formats.

### **Dataset Overview**
- Display column names, data types, and total number of rows.
- Count the number of image classes grouped by insect taxonomy.
- Validate image dimensions by sampling from the `cropped_256` folder.

### **Missing Value Analysis**
- Identify and count missing values across each metadata field.

### **Basic Statistical Summary**
- Calculate summary statistics (mean, median, standard deviation, etc.) for numerical fields like body size.

### **Exploratory Visualizations**
- **Body Size Histogram**: Visualize the distribution of species' body sizes.
- **Top 10 Insect Families Bar Chart**: Show the most common families by image count.
- **Geographic Scatter Plot**: Plot specimen locations using latitude and longitude.
- **Image Grid**: Display a 3x3 grid of randomly selected images from distinct classes.
- **Top 10 Orders**: Rank and plot the top 10 insect orders based on image count.

## 3. Handling Missing Values and Data Cleaning

### **Missing Value Handling**
- Impute missing values in body size and geographic fields using mode or median.
- Remove or flag columns with excessive missing values.

### **Row Filtering**
- Remove rows with incomplete or invalid data as needed.

## 4. Image and DNA Data Preprocessing

### **Image Preprocessing**
- Resize all images to 256×256 pixels (e.g., folder `train/1` sampled for testing).
- Normalize pixel values to [0, 1] to standardize input for machine learning models.
- Apply data augmentation: random rotation and horizontal/vertical flipping.
- Split the dataset into train/validation/test sets in the ratio 70:15:15.

### **DNA Sequence Preprocessing**
- Tokenize DNA barcode sequences into non-overlapping k-mers.
- Apply one-hot encoding to convert k-mers into numerical vectors.
- Remove low-quality sequences or perform trimming/alignment as needed.

## 5. Geographic Data Normalization

### **Coordinate Normalization**
- Normalize latitude and longitude fields.
- Handle invalid, missing, or outlier coordinates.

### **Geospatial Clustering**
- Apply geohashing and K-Means clustering to group nearby samples for biogeographic analysis.

## 6. Visualization and Reporting

### **Summary Visualizations**
- Body size distribution histogram
- Bar chart of the most common species families
- Geographic scatter plot of specimen locations

### **Saving Outputs**
- Export the cleaned metadata to `clean_metadata.csv`.
- Save preprocessed images in the `cropped_images` directory.
