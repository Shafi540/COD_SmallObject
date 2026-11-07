# COD_SmallObject
Uncertainty-Guided Multi-Scale Boundary Refinement Network for Small Camouflaged Object Detection
Overview
Camouflaged Object Detection (COD) aims to segment objects that visually blend into their surroundings. Small camouflaged objects are particularly difficult to detect because they occupy relatively few pixels and often have weak boundaries and limited visual contrast.

This project investigates an Uncertainty-Guided Multi-Scale Boundary Refinement Network (UGBRNet) for camouflaged object segmentation, with particular attention to small objects. The approach combines multi-scale feature fusion, boundary-aware learning, and uncertainty-guided refinement.

Repository: https://github.com/Shafi540/COD_SmallObject
Problem Statement
Existing camouflaged object detection approaches face challenges with:
- Small foreground objects with limited spatial information.
- Ambiguous boundaries between objects and backgrounds.
- Background regions that closely resemble camouflaged objects.
- Uncertain predictions in low-contrast or complex scenes.
The project evaluates whether combining multi-scale feature processing, boundary supervision, and uncertainty-guided refinement can support more accurate segmentation, especially for small camouflaged objects.
Proposed Method
The project uses a deep encoder-decoder segmentation network with:
1. ResNet-34 encoder: Extracts visual features at multiple spatial scales using ImageNet-pretrained weights.
2. Multi-scale feature fusion: Combines information from different encoder stages to retain both semantic and spatial detail.
3. Boundary-aware supervision: Uses object-mask boundaries to encourage better localization of object contours.
4. Uncertainty-guided refinement: Uses uncertainty information derived from segmentation predictions to support refinement of ambiguous regions.
5. Segmentation output: Produces a foreground probability map that can be thresholded to generate a binary object mask.
Datasets
Dataset	Description	Use in reported experiments
COD10K	Large-scale camouflaged object detection benchmark	Training, validation, and testing
CAMO	Benchmark with naturally and artificially camouflaged objects	Training, validation, and testing
NC4K	Additional camouflaged object detection benchmark	Referenced; not evaluated in the reported run
CHAMELEON	Small-scale camouflaged object benchmark	Referenced; not evaluated in the reported run


In the reported experimental setup, the training data included 2,736 COD10K and 900 CAMO images, with 304 COD10K and 100 CAMO validation images. The official test sets contained 2,026 COD10K and 250 CAMO images.
\nCAMO: https://www.kaggle.com/datasets/ivanomelchenkoim11/camo-dataset
\nCOD10K: https://www.kaggle.com/datasets/ivanomelchenkoim11/camo-dataset

The small-object subset is defined as test images in which the ground-truth foreground occupies 10% or less of the image.
Preprocessing and Training
- Image resolution: 352 × 352 pixels.
- RGB image normalization: ImageNet mean and standard deviation.
- Ground-truth masks: Binary segmentation masks.
- Data augmentation: Flipping, rotation, and brightness adjustments on training images.
- Backbone: ImageNet-pretrained ResNet-34.
- Optimizer: AdamW.
- Training duration: 30 epochs.
- Batch size: 8.
- Learning-rate schedule: Cosine annealing.
- Checkpoint selection: Best validation Dice/F1 score.
- Evaluation threshold: 0.5.
The reported experiments used a Kaggle notebook with an NVIDIA Tesla T4 GPU.
Evaluation Metrics
Performance is evaluated using:
- Intersection over Union (IoU): Overlap between predicted and ground-truth foreground regions.
- Dice/F1 Score: Segmentation overlap score.
- Precision: Proportion of predicted foreground pixels that are correct.
- Recall: Proportion of ground-truth foreground pixels recovered.
- Pixel Accuracy: Proportion of correctly classified pixels.
- Mean Absolute Error (MAE): Average absolute difference between prediction and ground truth; lower is better.
- Boundary F1: Agreement between predicted and ground-truth object boundaries.
Benchmark Results
The project reports performance separately for full test sets and small-object subsets. For the latest evaluation values, see [`cod_benchmark_results.csv`](cod_benchmark_results.csv) in this repository.
Dataset	IoU	Dice/F1	Precision	Recall	MAE
COD10K — All	0.5829	0.7365	0.7103	0.7647	0.0551
COD10K — Small ≤10%	0.4624	0.6324	0.5414	0.7602	0.0404
CAMO — All	0.5720	0.7278	0.7508	0.7061	0.1032
CAMO — Small ≤10%	0.4609	0.6310	0.5358	0.7673	0.0598


Values are rounded to four decimal places. The small-object subset contains 1,378 COD10K images and 84 CAMO images in the reported evaluation.
Key Observations
The model achieves stronger overall segmentation overlap than on the small-object subsets. This indicates that detecting and precisely segmenting small camouflaged objects remains challenging. The reported results are evaluations of the complete network and do not independently establish the contribution of each component; controlled ablation experiments would be needed for that.
Repository Contents
File	Description
[`ugbrnet-camouflaged-object-detection-kaggle.ipynb`](ugbrnet-camouflaged-object-detection-kaggle.ipynb)	Kaggle notebook containing the project implementation and experimental workflow.
[`cod_benchmark_results.csv`](cod_benchmark_results.csv)	Benchmark evaluation results.
[`README.md`](README.md)	Project documentation.


Running the Project
1. Open [`ugbrnet-camouflaged-object-detection-kaggle.ipynb`](ugbrnet-camouflaged-object-detection-kaggle.ipynb) in Kaggle Notebooks.
2. Enable a GPU accelerator in the notebook settings.
3. Add the COD10K and CAMO datasets in the dataset configuration expected by the notebook.
4. Review the dataset paths and other configuration cells before execution.
5. Run the notebook cells in order to execute the available preprocessing, training, and evaluation workflow.
6. Inspect the generated evaluation results and compare them with [`cod_benchmark_results.csv`](cod_benchmark_results.csv).
Note: Dataset availability, notebook paths, and Kaggle package versions may require configuration. The README does not assume a separate command-line entry point or installation script.
Limitations and Future Work
- Improve segmentation overlap for small camouflaged objects.
- Reduce false-positive foreground predictions and missed object regions.
- Improve boundary localization in complex scenes.
- Evaluate the relationship between uncertainty estimates and actual prediction errors.
- Conduct controlled ablation studies for multi-scale fusion, boundary supervision, and uncertainty-guided refinement.
- Extend testing to additional benchmark datasets and repeated experimental runs.
Project Team
Department of Computer Science and Engineering, Manipal, Karnataka.
Name	Registration Number	Email
Ashmita Susan Choppala	240905138	ashmita.mitmpl2024@learner.manipal.edu
Jinu Rajeev	240905077	jinu.mitmpl2024@learner.manipal.edu
Muhammad Shafi	240905103	shafi.mitmpl2024@learner.manipal.edu
Prisha Anand	230905110	prisha1.mitmpl2023@learner.manipal.edu


Research Context
The project builds on research directions involving uncertainty estimation, edge-guided segmentation, contextual feature fusion, and boundary refinement in camouflaged object detection. Relevant approaches discussed in the project literature survey include PUENet, MFNet, CABR, BERNet, and feature decomposition with edge reconstruction.

The reported results should not be interpreted as demonstrating superiority over these published methods without matched evaluation protocols and baseline comparisons.
