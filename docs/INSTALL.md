# Installation

> Modified from bevformer and mmdetection3d.

UniAD supports two installation methods depending on your Python version:
- **Python 3.9** (recommended): Use pre-built wheels (faster)
- **Python 3.10-3.12**: Build from source (required for Google Colab)

---

## Option A: Python 3.9 Environment (Recommended)

**a. Create a conda virtual environment and activate it.**
```shell
conda create -n uniad2.0 python=3.9 -y
conda activate uniad2.0
```

**b. Install PyTorch and torchvision following the [official instructions](https://pytorch.org/).**
```shell
pip install torch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 --index-url https://download.pytorch.org/whl/cu118
```

**c. Install mmcv-series packages (pre-built wheels).**
```shell
pip install mmcv-full==1.6.1 -f https://download.openmmlab.com/mmcv/dist/cu118/torch2.0/index.html
pip install mmdet==2.26.0 mmsegmentation==0.29.1 mmdet3d==1.0.0rc6
```

**d. Install UniAD.**
```shell
cd ~
git clone https://github.com/OpenDriveLab/UniAD.git
cd UniAD
pip install -r requirements.txt
pip install -e .
```

---

## Option B: Python 3.10-3.12 Environment (Colab/Modern Systems)

For systems with Python 3.10+ (including Google Colab), you must build mmcv from source.

**a. Create environment (skip if using Colab).**
```shell
conda create -n uniad2.0 python=3.10 -y
conda activate uniad2.0
```

**b. Install PyTorch 2.2+ (required for Python 3.10+).**
```shell
# For Python 3.10-3.11
pip install torch==2.2.0 torchvision==0.17.0 --index-url https://download.pytorch.org/whl/cu118

# For Python 3.12 (Colab)
pip install torch==2.2.0 torchvision==0.17.0 --index-url https://download.pytorch.org/whl/cu118
```

**c. Build mmcv from source (~10 min).**
```shell
pip install -U pip setuptools wheel
pip install addict yapf "numpy>=1.22.4,<2.0" Pillow pyyaml

git clone https://github.com/open-mmlab/mmcv.git -b v1.7.2
cd mmcv
MMCV_WITH_OPS=1 pip install -e . -v
cd ..
```

**d. Build mmdet, mmseg, mmdet3d from source.**
```shell
# mmdetection
git clone https://github.com/open-mmlab/mmdetection.git -b v2.28.2
cd mmdetection
pip install -e . -v
cd ..

# mmsegmentation
git clone https://github.com/open-mmlab/mmsegmentation.git -b v0.30.0
cd mmsegmentation
pip install -e . -v
cd ..

# mmdetection3d
git clone https://github.com/open-mmlab/mmdetection3d.git -b v1.0.0rc6
cd mmdetection3d
pip install -e . -v
cd ..
```

**e. Install UniAD.**
```shell
git clone https://github.com/OpenDriveLab/UniAD.git
cd UniAD
pip install -r requirements.txt
pip install -e .
```

---

## Google Colab

For running on Google Colab, use the provided notebook:
- [UniAD_Eval_Colab.ipynb](../UniAD_Eval_Colab.ipynb)

Or open directly in Colab:
```
https://colab.research.google.com/github/RobotMa/UniAD/blob/v2.0-qianli/UniAD_Eval_Colab.ipynb
```

---

## Prepare Pretrained Weights

We release our pretrained weights in [HuggingFace::OpenDriveLab/UniAD2.0_R101_nuScenes](https://huggingface.co/OpenDriveLab/UniAD2.0_R101_nuScenes/tree/main/ckpts)

```shell
mkdir -p ckpts && cd ckpts

# r101_dcn_fcos3d_pretrain.pth (from bevformer)
wget https://huggingface.co/OpenDriveLab/UniAD2.0_R101_nuScenes/resolve/main/ckpts/r101_dcn_fcos3d_pretrain.pth

# bevformer_r101_dcn_24ep.pth
wget https://huggingface.co/OpenDriveLab/UniAD2.0_R101_nuScenes/resolve/main/ckpts/bevformer_r101_dcn_24ep.pth

# uniad_base_track_map.pth
wget https://huggingface.co/OpenDriveLab/UniAD2.0_R101_nuScenes/resolve/main/ckpts/uniad_base_track_map.pth

# uniad_base_e2e.pth
wget https://huggingface.co/OpenDriveLab/UniAD2.0_R101_nuScenes/resolve/main/ckpts/uniad_base_e2e.pth
```

---

## Version Compatibility Matrix

| Python | PyTorch | mmcv | mmdet | mmseg | mmdet3d | Install Method |
|--------|---------|------|-------|-------|---------|----------------|
| 3.9 | 2.0.1 | 1.6.1 (full) | 2.26.0 | 0.29.1 | 1.0.0rc6 | Pre-built wheels |
| 3.10 | 2.2.0 | 1.7.2 | 2.28.2 | 0.30.0 | 1.0.0rc6 | From source |
| 3.11 | 2.2.0 | 1.7.2 | 2.28.2 | 0.30.0 | 1.0.0rc6 | From source |
| 3.12 | 2.2.0 | 1.7.2 | 2.28.2 | 0.30.0 | 1.0.0rc6 | From source |

---

## Troubleshooting

**mmcv build fails:**
- Ensure CUDA toolkit is installed: `nvcc --version`
- Install build dependencies: `pip install ninja`

**NumPy compatibility errors:**
- Use numpy < 2.0: `pip install "numpy>=1.22.4,<2.0"`

**Import errors after install:**
- Restart your Python session/kernel
- Verify installation: `python -c "import mmcv; import mmdet; import mmdet3d; print('OK')"`

---
-> Next Page: [Prepare The Dataset](./DATA_PREP.md)
