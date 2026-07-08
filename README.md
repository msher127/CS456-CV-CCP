# CS-456 Computer Vision — Complex Computing Problem (CCP)

## Autonomous Visual Intelligence System for Scene Understanding

A multi-stage computer vision pipeline for outdoor scene understanding in an autonomous-robotics context. The project combines classical computer vision techniques (edge detection, feature matching, panorama stitching) with a deep-learning scene classifier, and includes a critical evaluation of the whole pipeline.

**Course:** CS-456 Computer Vision — BSCS
**CLOs covered:** CLO-3, CLO-4, CLO-5

---

## Pipeline Overview

| Task | Description | Technique |
|------|-------------|-----------|
| **1. Edge Detection & Hough Transform** | Detect road boundaries and structural lines | Gaussian smoothing → Canny → Probabilistic Hough Line Transform |
| **2. SIFT Feature Matching** | Match a landmark across two viewpoints | SIFT keypoints/descriptors + FLANN + Lowe's ratio test (0.75) |
| **3. Panorama Construction** | Stitch 3 overlapping frames into a panorama | SIFT + RANSAC homography + progressive warping + alpha blending (no `cv2.Stitcher`) |
| **4. CNN Scene Classification** | Classify outdoor scenes into 4 categories | Transfer learning with MobileNetV2 + data augmentation + Grad-CAM |
| **5. Technical Report** | Critical evaluation of the full pipeline | Quantitative results, failure analysis, trade-offs |

---

## Key Results

- **Task 1:** Selected Canny thresholds 50/150; 43 Hough lines detected.
- **Task 2:** 480 / 410 keypoints; 211 good matches (match ratio 0.44).
- **Task 3:** RANSAC inlier ratios of 0.951 and 0.943 across the two stitching pairs.
- **Task 4:** 92% validation accuracy across 4 classes (target was 70%).

---

## Repository Structure

```
.
├── CS456_CCP_CV_Colab_Notebook.ipynb   # Main source code (runs end-to-end)
├── requirements.txt                    # Python dependencies
├── README.md                           # This file
├── outputs/                            # All generated images, CSVs, model, Grad-CAM
│   ├── task1_final_original_edges_hough.png
│   ├── task2_top50_good_matches.png
│   ├── task3_final_panorama.jpg
│   ├── task3_homography_report.txt
│   ├── task4_confusion_matrix.png
│   ├── task4_gradcam_*.png
│   └── ... (CSV metric files)
└── CS456_CCP_Technical_Report.pdf      # Technical report (3-5 pages)
```

---

## How to Run

The project is designed to run end-to-end in **Google Colab** (recommended, GPU for Task 4).

1. Open the notebook in Google Colab.
2. Enable GPU: **Runtime → Change runtime type → T4 GPU**.
3. Run the **Setup cell** first (installs dependencies).
4. Run **Task 1 → Task 4** cells in order.
5. Run the final cell to download all outputs as a ZIP.

To run locally instead:

```bash
pip install -r requirements.txt
jupyter notebook CS456_CCP_CV_Colab_Notebook.ipynb
```

> Note: SIFT requires `opencv-contrib-python` (already listed in requirements).

---

## Datasets & Image Sources

All images are cited. None were self-captured.

| Task | Source |
|------|--------|
| Task 1 | Road image (Murree road), Unsplash — free-to-use license |
| Task 2 | Wazir Khan Mosque, Lahore — photos by Danish Jaffry, Unsplash |
| Task 3 | Sample overlapping sequence from [Avinash793/panoramic-image-stitching](https://github.com/Avinash793/panoramic-image-stitching) (images only; stitching implemented independently) |
| Task 4 | [EuroSAT](https://www.tensorflow.org/datasets/catalog/eurosat) satellite land-cover dataset (Helber et al., 2019) — classes: Highway (road), Residential (building), Forest (vegetation), River (water) |

---

## Academic Integrity & AI Disclosure

- The stitching pipeline (Task 3) is implemented **manually** — no `cv2.Stitcher` or other pre-built panorama API is used.
- All external image sources are cited above.
- **AI assistance disclosure:** AI assistance (Anthropic's Claude) was used to help structure and debug the code and draft the report wording. The final code, dataset selection, experiments, execution, outputs, and interpretations were reviewed and verified by the student, in accordance with the course academic-integrity rules.
