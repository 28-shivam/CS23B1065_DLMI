# Brain MRI Tumor Segmentation using Otsu & Sauvola Thresholding

## Project Overview
This project explores the effectiveness of classical computer vision thresholding techniques for segmenting brain tumors from MRI scans. It compares a global thresholding method (**Otsu**) against a local adaptive method (**Sauvola**) to determine if simple intensity-based segmentation is sufficient for medical imaging tasks.

## Dataset
**Source:** [Brain Tumor Segmentation – Nikhil Tomar (Kaggle)](https://www.kaggle.com/datasets/nikhilroxtomar/brain-tumor-segmentation)

structure:
* `images/` - Folder containing MRI scans (.png)
* `masks/` - Folder containing binary ground truth masks (.png)

## Methodology

The pipeline processes 2D MRI slices using the following steps:

### 1. Preprocessing
To improve the separation between the tumor and the background, we apply:
* **CLAHE (Contrast Limited Adaptive Histogram Equalization):**
    * *Clip Limit:* 2.0
    * *Tile Grid Size:* (8, 8)
    * Enhances local contrast to make the tumor boundaries more distinct.
* **Gaussian Blur:**
    * *Kernel Size:* (5, 5)
    * Reduces high-frequency noise and smoothes the image before thresholding.

### 2. Segmentation Algorithms
* **Otsu Thresholding (Global):** Automatically calculates a single intensity threshold that separates pixels into two classes (foreground/background) by minimizing intra-class variance.
* **Sauvola Thresholding (Local):** Computes a threshold for every pixel based on the mean and standard deviation of its local neighborhood.
    * *Window Size:* 51 (The size of the local neighborhood used for calculation).

### 3. Evaluation Metrics
We quantitatively evaluate performance using:
* **Dice Coefficient (F1 Score):** Measures the harmonic mean of precision and recall.
* **Jaccard Index (IoU - Intersection over Union):** Measures the overlap between the predicted mask and the ground truth.

## Results & Observations
The script calculates the **Mean Dice** and **Mean Jaccard** scores across the entire dataset.

**General Observation:**
While these methods are fast and computationally inexpensive, they often yield low scores for brain tumor segmentation.
* **Why?** MRI images are complex. The skull, eyes, and healthy brain tissue often share similar intensity values with the tumor.
* **Otsu** tends to capture the entire skull structure rather than just the tumor.
* **Sauvola** captures edges well but often results in hollow segmentations or excessive noise.

