# Medical Image Processing Project: CT Analysis, Segmentation, and Registration

## Overview

This project focuses on processing and analyzing abdominal CT scans and associated segmentations, specifically targeting liver and tumor regions. The primary goals are to implement and evaluate techniques for DICOM data handling, 3D visualization, semi-automatic image segmentation, and 3D rigid coregistration.

This repository contains implementations for the following objectives:

1. **DICOM Loading and Visualization:**  
   Loading CT series and segmentations, spatial ordering based on DICOM headers, and generating 3D visualizations such as rotating Maximum Intensity Projections (MIPs) with tumor mask overlays.

2. **3D Image Segmentation:**  
   Developing and evaluating a semi-automatic 3D region growing algorithm to segment tumor regions based on limited initial information (centroid), including quantitative and qualitative assessment.

3. **3D Rigid Coregistration:**  
   Manual implementation of a rigid registration pipeline aligning an input CT volume to a reference volume using landmark initialization, optimization of rotation and translation parameters, and validation through numerical metrics and visual overlays.

## Features Implemented

* **DICOM Data Handling:**  
    * Loads CT image series and segmentation masks from directories using `pydicom` and `highdicom`.  
    * Sorts CT slices and segmentation frames based on spatial headers (`ImagePositionPatient`) for accurate 3D volume reconstruction.  
    * Ensures spatial alignment between CT volumes and segmentation masks by matching voxel dimensions, orientation, and origin.

* **Visualization:**  
    * Displays 2D axial, sagittal, and coronal slices with segmentation overlays (Liver, Tumor) using `matplotlib`.  
    * Applies CT windowing functions for optimized contrast visualization.  
    * Generates animated rotating Maximum Intensity Projection (MIP) GIFs showing tumor localization in 3D.

* **Segmentation (Objective 2):**  
    * Computes 3D bounding box and centroid of the ground truth tumor mask.  
    * Implements a semi-automatic seeded region growing segmentation algorithm in 3D using `SimpleITK`.  
    * Applies preprocessing (Gaussian smoothing) and postprocessing (largest connected component extraction, morphological closing) to refine segmentation.  
    * Evaluates segmentation accuracy with Dice Similarity Coefficient (DSC), Precision, Sensitivity (Recall), Specificity, and F1-score.  
    * Provides visual comparison plots between ground truth and segmented masks on representative slices.

* **Registration (Objective 3):**  
    * Manually implements 3D rigid transformation including rotation (via quaternion) and translation parameters.  
    * Performs landmark-based initialization followed by optimization of parameters using a grid-search optimizer minimizing a normalized mutual information based loss.  
    * Applies trilinear interpolation for volume resampling under transformation.  
    * Validates registration accuracy through multiple quantitative metrics (Correlation coefficient, Normalized Mutual Information, Mean Squared Error) and visual overlay of transformed liver segmentation masks.

## Setup and Installation

1. **Clone the repository:**
    ```bash
    git clone https://github.com/abdulaihalidu/Medical-Image-Processing.git
    cd 'Medical Image Processing'
    ```
2. **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```
3. **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    *(Ensure `requirements.txt` includes all necessary libraries such as `numpy`, `pydicom`, `matplotlib`, `scipy`, `imageio`, `highdicom`, `SimpleITK`.)*

## Usage

1. **Configure Paths:** Update the paths in the Jupyter Notebook for your dataset and output locations:  
    - `ct_directory`  
    - `liver_seg_file`  
    - `tumor_seg_file`  
    - `input_ct_volume` (for registration)  
    - `output_dir` / `output_gif_path`  
2. **Run the Notebook:** Execute cells sequentially to perform loading, visualization, segmentation, and registration.  
3. **Outputs:** Results include printed metrics, figures, and saved visualization files such as MIP animations and overlay images.

## Configuration Notes

* **Segmentation Parameters:**  
    * `intensity_tolerance` — defines intensity window around seed voxel for region growing  
    * Gaussian smoothing `sigma` for noise reduction  
    * Morphological closing kernel size for mask refinement

* **Registration Parameters:**  
    * Number of iterations and step sizes for optimizer  
    * Sampling step for grid search  
    * Similarity metric configuration (Normalized Mutual Information)

## Repository Structure

- `notebook.ipynb` — Main Jupyter Notebook containing the full workflow and code  
- `requirements.txt` — List of required Python packages  
- `Data/` — Directory containing the DICOM data files  
- `README.md` — Project overview and instructions

---

This project demonstrates end-to-end medical image analysis workflows, emphasizing practical implementation, quantitative validation, and clear visualization, serving as a solid foundation for further research or clinical application development.
