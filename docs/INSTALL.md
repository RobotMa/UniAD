# Installation

> Modified from bevformer and mmdetection3d.

## Environment Setup

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
```

---

## Google Colab

Colab uses Python 3.12+ which is incompatible with pre-built mmcv wheels. Use **conda-pack** to package the Python 3.9 environment locally and deploy it to Colab:

1. Build the environment locally following the steps above
2. Package it with conda-pack:
   ```shell
   conda install -c conda-forge conda-pack
   conda pack -n uniad2.0 -o uniad_env.tar.gz
   ```
3. Upload `uniad_env.tar.gz` to `Google Drive/colab_cache/UniAD/`:
   ```shell
   # Install rclone (one-time)
   curl https://rclone.org/install.sh | sudo bash
   rclone config   # select "Google Drive", follow OAuth prompts

   # Upload (~5-10GB, supports resume on interruption)
   rclone copy uniad_env.tar.gz gdrive:colab_cache/UniAD/ --progress
   ```
4. Use the provided notebook: [UniAD_Eval_Colab.ipynb](../UniAD_Eval_Colab.ipynb)

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

| Python | PyTorch | CUDA | mmcv | mmdet | mmseg | mmdet3d |
|--------|---------|------|------|-------|-------|---------|
| 3.9 | 2.0.1 | 11.8 | 1.6.1 (full) | 2.26.0 | 0.29.1 | 1.0.0rc6 |

---

## Troubleshooting

**NumPy compatibility errors:**
- Use numpy < 2.0: `pip install "numpy>=1.22.4,<2.0"`

**Import errors after install:**
- Restart your Python session/kernel
- Verify installation: `python -c "import mmcv; import mmdet; import mmdet3d; print('OK')"`

---
-> Next Page: [Prepare The Dataset](./DATA_PREP.md)
