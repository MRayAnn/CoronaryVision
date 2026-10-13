# ARCADE Dataset Exploration

## 1. Dataset Overview

CoronaryVision uses the publicly available ARCADE dataset (MICCAI 2023), accessed through Kaggle.

- **Kaggle source:** https://www.kaggle.com/datasets/nirmalgaud/arcade-dataset
- **Task:** Coronary artery segmentation
- **Dataset subset:** `syntax`
- **Annotation format:** COCO JSON
- **Image resolution:** 512 × 512 pixels for the inspected image

The project aims to develop a binary segmentation model that distinguishes coronary arteries from the background.

## 2. Dataset Organization

The dataset contains two main tasks: `stenosis` and `syntax`.

For CoronaryVision, we focus on `syntax`, which contains three predefined dataset splits:

| Split | Images | Annotations |
|---|---:|---:|
| Training | 1,000 | 4,976 |
| Validation | 200 | 1,168 |
| Test | 300 | 1,672 |
| **Total** | **1,500** | **7,816** |

Each split contains an `images/` directory and an `annotations/` directory containing a COCO JSON file.

## 3. COCO Annotation Structure

The annotation files contain three main sections:

- `images`: Image identifiers, filenames, and dimensions.
- `annotations`: Object annotations, including image references, category identifiers, and segmentation polygons.
- `categories`: Definitions of the annotation classes.

The `syntax` dataset defines 26 categories, including anatomical coronary artery segments and a category named `stenosis`.

Each annotation is linked to an image through its `image_id`.

## 4. Segmentation Masks

COCO polygon annotations contain sequences of coordinates defining the boundaries of labeled regions.

For the planned binary segmentation task, annotated artery regions are converted into masks containing:

- `0`: Background
- `1`: Coronary artery

For visualization, 8-bit masks use `0` for black and `255` for white.

The initial mask-generation example excludes category 26 (`stenosis`). This category-handling decision requires further verification before preparing the complete training dataset.

## 5. First Image Inspection

We examined image `922.png` from the training set.

- Image ID: 922
- Dimensions: 512 × 512 pixels
- Number of associated annotations: 4

We visualized the original angiography, overlaid its annotation polygons, and created a binary segmentation mask.

The resulting mask visually matched the outlined regions for this example.

This confirms that the visualization procedure works for the inspected image, but does not establish the anatomical accuracy or completeness of the annotations.

## 6. Data Integrity Checks

The following checks were performed on the training set:

| Check | Result |
|---|---|
| Image records in JSON | 1,000 |
| Files in images directory | 1,000 |
| Missing image filenames | 0 |
| Unexpected image filenames | 0 |
| Annotations referencing unknown image IDs | 0 |

The inspected training data passed these basic structural checks.

## 7. Limitations and Next Steps

The following aspects require further investigation:

- Verify annotation integrity across the validation and test splits.
- Inspect additional images and segmentation masks.
- Examine category distributions and determine the appropriate handling of stenosis annotations.
- Investigate whether the dataset splits prevent patient-level data leakage.
- Validate polygon-to-mask conversion more systematically.
- Implement a reproducible PyTorch dataset-loading pipeline.

No model has been trained or evaluated at this stage.