# 1. Installation

This project uses Python 3.11 and `uv` for dependency management.

Install `uv` by following the official installation guide:

https://docs.astral.sh/uv/getting-started/installation/

After installing `uv`, clone the repository and install the project dependencies:

```bash
git clone https://github.com/xXFarskyXx/DSDE-Midterm.git
cd DSDE-Midterm
uv sync
```

# 2. Reproduce Data Preparation

Download the dataset manually and place it under:

```text
data/raw/
```

The expected structure is:

```text
data/
└── raw/
    ├── train/
    │   ├── train/
    │   │   └── image files...
    │   └── train.csv
    └── test/
        └── test/
            └── image files...
```

Then run:

```text
1. Dataset Preparation.ipynb
```

Run all cells from top to bottom.

This notebook creates the required generated directories automatically, including:

```text
data/interim/
data/processed/
data/rfdetr/
```

# 3. Reproduce Training

After completing the data preparation step, run:

```text
4. RF-DETR.ipynb
```

Run all cells from top to bottom.

Training outputs are stored under:

```text
runs/rfdetr_large/
```

Each run is stored in a separate directory, for example:

```text
runs/
└── rfdetr_large/
    └── run1/
        └── checkpoint_best_ema.pth
```

After training finishes, locate:

```text
checkpoint_best_ema.pth
```

You will need the directory containing this file for the inference step.

# 4. Reproduce Inference

Run:

```text
7. Highest Score Inference.ipynb
```

Before running the notebook, change `ds_dir` so that it points to the directory containing your:

```text
checkpoint_best_ema.pth
```

For example, if your checkpoint is located at:

```text
runs/rfdetr_large/run1/checkpoint_best_ema.pth
```

change:

```python
ds_dir = Path(f"runs/rfdetr_large/run{len(os.listdir(r'runs/rfdetr_large')) + 1}")
```

to:

```python
ds_dir = Path("runs/rfdetr_large/run1")
```

The model loads the checkpoint using:

```python
model = RFDETRLarge(
    pretrain_weights=ds_dir / "checkpoint_best_ema.pth"
)
```

Therefore, `ds_dir` must point to the **directory containing the checkpoint**, not to the checkpoint file itself.

The test images are expected at:

```python
image_dir = Path("data/raw/test/test")
```

After setting `ds_dir` correctly, run all cells in:

```text
7. Highest Score Inference.ipynb
```
