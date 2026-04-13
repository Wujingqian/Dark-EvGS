# Dark-EvGS: Event Camera as an Eye for Radiance Field in the Dark

> This repository supports reproducibility for the IEEE TIP paper **Dark-EvGS: Event Camera as an Eye for Radiance Field in the Dark**. It provides code, sample data, and selected pretrained outputs for rendering reproduction and dataset usage.

---

## Paper Information

- **Title**: Dark-EvGS: Event Camera as an Eye for Radiance Field in the Dark  
- **Authors**: Jingqian Wu, Peiqi Duan, Zongqiang Wang, Changwei Wang, Boxin Shi, Edmund Y. Lam  
- **Venue**: IEEE Transactions on Image Processing (TIP), 2026  
- **IEEE Xplore**: https://ieeexplore.ieee.org/abstract/document/11449405  
- **arXiv**: https://arxiv.org/submit/7472197/view  

---

## Repository Contents

This repository currently focuses on reproducibility assets:

1. **Rendering script**: reproduce renders from existing 3D Gaussian checkpoints (`render.py`).
2. **COLMAP conversion script**: build COLMAP structure from custom input images (`convert.py`).
3. **Environment file**: conda dependencies (`environment.yml`).
4. **Sample scene data** (`data/`), including:
   - Low-light frame sequences (`low_light_frames/`)
   - Bright-condition ground-truth frames (`gt_bright_frames/`)
   - Event streams (`event.npz`, typically with `x/y/t/p`)
   - Optional COLMAP sparse outputs (`sparse/0`)
   - Optional pretrained model outputs (`output/point_cloud/...`)

---

## Quick Start

### 1) Environment Setup

```bash
conda env create -f environment.yml
conda activate gaussian_splatting
```

### 2) Reproduce Rendering from a Provided Checkpoint

Using the `lion` scene as an example:

```bash
python render.py -m data/lion/output
```

Default output folders:

- `data/lion/output/train/ours_<iter>/renders`
- `data/lion/output/test/ours_<iter>/renders`

Useful options:

- `--iteration <int>`: load a specific iteration (`-1` for latest)
- `--skip_train`: render test split only
- `--skip_test`: render train split only

Example (test split only):

```bash
python render.py -m data/lion/output --skip_train
```

---

## Data Usage

### Frame Sequences

- Low-light input frames: `data/<scene>/low_light_frames/*.png`
- Bright GT frames: `data/<scene>/gt_bright_frames/*.png`

### Event Stream (`event.npz`)

Typical keys:

- `x`: pixel x-coordinate
- `y`: pixel y-coordinate
- `t`: timestamp
- `p`: event polarity

Example:

```python
import numpy as np

events = np.load("data/lion/event.npz")
x, y, t, p = events["x"], events["y"], events["t"], events["p"]
print(x.shape, y.shape, t.shape, p.shape)
```

---

## Build COLMAP Inputs from Your Own Data (Optional)

If you want to process your own multi-view images into this repo format:

```bash
python convert.py -s <your_scene_path>
```

Expected input:

- `<your_scene_path>/input/` containing images.

The script runs COLMAP feature extraction, matching, sparse reconstruction, and undistortion, then arranges outputs into `sparse/0`.

---

## Visualization Examples (In-Repo Samples)

### Lion Scene (Low-light vs Bright GT)

| Low-light | Bright GT |
|---|---|
| ![lion-low](data/lion/low_light_frames/000013.png) | ![lion-gt](data/lion/gt_bright_frames/000013.png) |
| ![lion-low](data/lion/low_light_frames/000079.png) | ![lion-gt](data/lion/gt_bright_frames/000079.png) |

### House Scene (Low-light vs Bright GT)

| Low-light | Bright GT |
|---|---|
| ![house-low](data/house/low_light_frames/000010.png) | ![house-gt](data/house/gt_bright_frames/000010.png) |

---

## Recommended Per-Scene Directory Structure

```text
data/<scene>/
├── low_light_frames/
├── gt_bright_frames/
├── event.npz
├── timestamp.txt (or timestamps.txt)
├── sparse/0/                 # optional
└── output/                   # optional
    ├── cfg_args
    ├── exposure.json
    ├── cameras.json
    └── point_cloud/
```

---

## Citation

If you find this repository useful in your research, please cite:

```bibtex
@article{wu2026dark,
  title={Dark-EvGS: Event camera as an eye for radiance field in the dark},
  author={Wu, Jingqian and Duan, Peiqi and Wang, Zongqiang and Wang, Changwei and Shi, Boxin and Lam, Edmund Y},
  journal={IEEE Transactions on Image Processing},
  year={2026},
  publisher={IEEE}
}
```

---

## Acknowledgement

This repository builds on the 3D Gaussian Splatting ecosystem. Please also cite foundational related work when appropriate.
