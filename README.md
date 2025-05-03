# Medical Image Processing Project: CT Analysis and Segmentation

## Overview

This project focuses on processing and analyzing abdominal CT scans and associated segmentations, specifically targeting liver and tumor regions. The primary goal is to implement and evaluate techniques for DICOM data handling, 3D visualization, and semi-automatic image segmentation.

This repository contains implementations for the following objectives:

1.  **DICOM Loading and Visualization:** Loading CT series and segmentations, spatial ordering based on DICOM headers, and generating 3D visualizations like Maximum Intensity Projections (MIPs).
2.  **3D Image Segmentation:** Developing and evaluating a semi-automatic algorithm to segment tumor regions based on limited initial information (centroid).

*(Note: Objective 3, manual 3D rigid coregistration, may be implemented later on.)*

## Features Implemented

* **DICOM Data Handling:**
    * Loads CT image series from a directory using `pydicom`.
    * Loads DICOM Segmentation objects using `pydicom` and `highdicom`.
    * Sorts CT slices based on `ImagePositionPatient` for correct 3D volume assembly.
    * Verifies consistency of `SeriesInstanceUID` and `AcquisitionNumber` (optional check) across CT slices.
    * Maps segmentation frames to corresponding CT slices using spatial information from headers.
* **Visualization:**
    * Displays 2D axial slices of the CT volume with optional segmentation overlays (Liver, Tumor) using `matplotlib`.
    * Applies CT windowing (`apply_window` function) for optimal contrast.
    * Generates rotating Maximum Intensity Projection (MIP) animations (`create_mip_animation` function) showing Axial, Coronal, and Sagittal views of the CT volume with tumor mask overlay.
* **Segmentation (Objective 2):**
    * Calculates the 3D bounding box and centroid of a ground truth tumor mask.
    * Implements a semi-automatic 3D region growing segmentation algorithm using `SimpleITK`, seeded by the tumor centroid.
    * Includes optional pre-processing (Gaussian smoothing) and post-processing (largest connected component, morphological closing) steps.
* **Evaluation (Objective 2):**
    * Calculates numerical segmentation performance metrics: Dice Similarity Coefficient (DSC), Precision, Sensitivity (Recall), Specificity, F1-score.
    * Provides visual comparison plots showing ground truth vs. segmented contours on representative CT slices.
* **Registration (Objective 3 - To be implemented):**
    * Manual implementation of 3D rigid transformation model (rotation, translation matrices).
    * Manual implementation of Trilinear Interpolation.
    * Manual implementation of Sum of Squared Differences (SSD) similarity metric.
    * Basic Coordinate Descent optimizer to find registration parameters.
    * Resampling function to apply the transformation.
    * Visualization of registration results (side-by-side slices, checkerboard).

## Setup and Installation

1.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd <repository-directory>
    ```
2.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```
3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    *(Ensure `requirements.txt` lists all necessary libraries: `numpy`, `pydicom`, `matplotlib`, `scipy`, `imageio`, `highdicom`, `SimpleITK`)*

## Usage

1.  **Configure Paths:** Open the main script (.ipynb file) and update the placeholder paths for your CT directory and segmentation files:
    * `ct_directory`
    * `liver_seg_file`
    * `tumor_seg_file`
    * `input_ct_volume` (for Objective 3, if applicable)
    * `output_dir` / `output_gif_path` (for saving results)
2.  **Run the Script:** Run the Jupyter Notebook cells sequentially
3.  **Outputs:** The script will print progress and results to the output cells. Visualizations (plots, MIP GIF) will be displayed or saved to the specified output directory.

## Configuration Notes

* **Segmentation Parameters:** For Objective 2, key parameters within the script might need tuning for optimal results on different datasets:
    * `intensity_tolerance` (for region growing)
    * Gaussian smoothing `sigma`
    * Morphological closing radius
* **Registration Parameters (To be Implemented):** For Objective 3, optimization parameters might need adjustment:
    * `iterations`
    * `step_sizes`
    * `sampling_step`
    * Similarity metric (`calculate_ssd` or potentially others)


