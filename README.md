# Beyond Visual Quality: Sand-Specific Image Restoration as Preprocessing for Object Detection in Sandstorm Conditions

**ARTI 403 – Image Processing** · Imam Abdulrahman Bin Faisal University, College of Computer Science and Information Technology · Term 1, 2026–2027
**Instructor:** Dr. Manal Almouain · **Group 5**

| # | Team Member | ID |
|---|-------------|----|
| 1 | Maisoon Alabdullah | 2240005251 |
| 2 | Wajd Alkhaldi | 2240005544 |
| 3 | Jolie Jamal | 2240004580 |
| 4 | Manar Alghamdi | 2240005128 |
| 5 | Daniah Alkhabbaz | 2240004129 |

## Overview

Sandstorms add a strong yellow-orange colour cast, reduce contrast and hide distant objects, which lowers the accuracy of vehicle detectors.
This project tests whether **classical image-restoration methods**, used as a preprocessing step, improve the detection accuracy of a **pretrained YOLOv8 detector (not re-trained)** on sandstorm images.

We compare generic enhancement methods with a **sand-specific pipeline**:

1. **Sand colour correction:** channel compensation, Gray-World white balance and CIELAB cast removal
2. **Dust removal:** Dark Channel Prior with a guided filter
3. **Contrast enhancement:** CLAHE
4. Optional **sky segmentation** (Otsu, Sobel and morphology) and **severity-adaptive** parameters

The methods are judged by detection accuracy (mAP@0.5, mAP@0.5:0.95, precision, recall, F1), not only by visual quality.

## Key results (YOLOv8n, 323 DAWN-Sand images)

| Method | Type | mAP@0.5 | Δ vs Original |
|---|---|---|---|
| Original (no preprocessing) | baseline | 0.419 | – |
| Gray-World | generic | 0.392 | −0.027 |
| HE | generic | 0.427 | +0.008 |
| CLAHE | generic | 0.441 | +0.022 |
| Dark Channel Prior | generic | 0.442 | +0.023 |
| SandCC + CLAHE | ours | 0.457 | +0.038 |
| **SandCC + DCP** | **ours** | **0.464** | **+0.044** |
| Full pipeline | ours | 0.455 | +0.035 |

The full tables are in [`results/`](results), including the ablation study, the results by dust severity, the significance tests, the comparison of detector sizes and the runtimes.

## Repository structure

```
├── Sandstorm_Restoration_Detection.ipynb   # full implementation (all steps, with outputs)
├── results/
│   ├── table_*.csv                         # result tables
│   └── figures/                            # figures used in the paper
├── requirements.txt
└── README.md
```

## Dataset

**DAWN – Vehicle Detection in Adverse Weather Nature** (Kenk & Hassaballah, 2020), **Sand** subset: 323 images with PASCAL VOC labels.

- Download: https://data.mendeley.com/datasets/766ygrbt8y/3 (doi:10.17632/766ygrbt8y.3)

The dataset is **not included** in this repository. Download it and place it next to the notebook like this:

```
DAWN/
└── Sand/
    └── Sand/
        ├── *.jpg
        └── Sand_PASCAL_VOC/*.xml
```

## How to run

```bash
git clone <repo-url>
cd <repo-folder>
pip install -r requirements.txt
jupyter notebook Sandstorm_Restoration_Detection.ipynb
```

The YOLOv8 weights (`yolov8n.pt`, `yolov8s.pt`, `yolov8m.pt`) are downloaded automatically the first time the notebook runs.
Detections are cached in `results/cache/`, so a second run is fast. Delete that folder to run everything again from scratch.

The notebook also runs on **Google Colab**: upload the `DAWN/Sand` folder and run all cells.

## References

1. M. A. Kenk and M. Hassaballah, "DAWN: Vehicle detection in adverse weather nature dataset," arXiv:2008.05402, 2020.
2. K. He, J. Sun and X. Tang, "Single image haze removal using dark channel prior," *IEEE TPAMI*, 33(12), 2011.
3. K. He, J. Sun and X. Tang, "Guided image filtering," *IEEE TPAMI*, 35(6), 2013.
4. K. Zuiderveld, "Contrast limited adaptive histogram equalization," *Graphics Gems IV*, 1994.
5. G. Jocher, A. Chaurasia and J. Qiu, *Ultralytics YOLOv8*, 2023. https://github.com/ultralytics/ultralytics

The full reference list is at the end of the notebook and in the paper.
