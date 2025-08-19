<!-- PROJECT LOGO -->

<p align="center">
  <h1 align="center">LongSplat: Robust Unposed 3D Gaussian Splatting for Casual Long Videos</h1>
  <p align="center">
    <a href="https://linjohnss.github.io/"><strong>Chin-Yang Lin</strong></a>
    ·
    <a href="https://sunset1995.github.io/"><strong>Cheng Sun</strong></a>
    ·
    <a href="https://fuenyang1127.github.io/"><strong>Fu-En Yang</strong></a>
    <br>
    <a href="https://minhungchen.netlify.app/"><strong>Min-Hung Chen</strong></a>
    .
    <a href="https://sites.google.com/site/yylinweb/"><strong>Yen-Yu Lin</strong></a>
    ·
    <a href="https://yulunalexliu.github.io/"><strong>Yu-Lun Liu</strong></a>
  </p>
  <h3 align="center"><a href="https://linjohnss.github.io/longsplat">Project Page</a> | <a href="https://linjohnss.github.io/longsplat">Paper</a> | <a href="https://drive.google.com/drive/folders/1b-QgNzWVpDaYkHQf3ycKORd7s5WekjGa?usp=drive_link">Evaluation Results</a></h3>
  <div align="center"></div>
</p>

<p align="center">
  <a href="">
    <img src="./assets/teaser.gif" alt="Logo" width="90%">
  </a>
</p>

## Installation

1. Clone LongSplat.
```bash
git clone --recursive https://github.com/NVlabs/LongSplat.git
cd LongSplat
```

2. Create the environment
```bash
conda create -n longsplat python=3.10.13 cmake=3.14.0 -y
conda activate longsplat
conda install pytorch torchvision pytorch-cuda=12.1 -c pytorch -c nvidia  # use the correct version of cuda for your system
pip install -r requirements.txt
pip install submodules/simple-knn
pip install submodules/diff-gaussian-rasterization
pip install submodules/fused-ssim
```

3. Optional but highly suggested, compile the cuda kernels for RoPE (as in CroCo v2).
```bash
# DUST3R relies on RoPE positional embeddings for which you can compile some cuda kernels for faster runtime.
cd submodules/mast3r/dust3r/croco/models/curope/
python setup.py build_ext --inplace
cd ../../../../../../
```

## Dataset Preparation
DATAROOT is `./data` by default. Please first make data folder by `mkdir data`.


### Free Dataset
Download our preprocessed Free dataset from [Dropbox](https://www.dropbox.com/sh/jmfao2c4dp9usji/AAC7Ydj6rrrhy1-VvlAVjyE_a?dl=0), and save it into the `./data/free` folder.

### Hike Dataset
Download our preprocessed Hike dataset from [Google Drive](https://drive.google.com/drive/folders/1kGY-VijIbXNsNb7ghEywi1fvkH4BaIEz?usp=share_link), and save it into the `./data/hike` folder.

### Tanks and Temples

Download the data preprocessed by [Nope-NeRF](https://github.com/ActiveVisionLab/nope-nerf/?tab=readme-ov-file#Data) as below, and the data is saved into the `./data/Tanks` folder.
```bash
wget https://www.robots.ox.ac.uk/~wenjing/Tanks.zip
```

<!-- ### CO3D
Download our preprocessed [data](https://ucsdcloud-my.sharepoint.com/:u:/g/personal/yafu_ucsd_edu/EftJV9Xpn0hNjmOiGKZuzyIBW5j6hAVEGhewc8aUcFShEA?e=x1aXVx), and put it saved into the `./data/co3d` folder. -->

## Run

### Training and Evaluation
The training scripts include both training, rendering, and evaluation steps:

Each `.sh` script runs three main Python scripts in sequence:
- `train.py`: Trains the LongSplat model
- `render.py`: Renders the trained model to generate novel views
- `metrics.py`: Evaluates the rendering quality and computes metrics

```bash
# For Free dataset
bash scripts/train_free.sh

# For Hike dataset
bash scripts/train_hike.sh

# For Tanks and Temples dataset
bash scripts/train_tnt.sh
```

### Run on your own video

* To run LongSplat on your own video, you need to first convert your video to frames and save them to `./data/$CUSTOM_DATA/images/`

* Before running the script, you need to modify the `scene=` parameter in `scripts/train_custom.sh` to point to your custom data directory. For example, change `scene='./data/IMG_4190'` to `scene='./data/YOUR_CUSTOM_DATA'`.

* Run the following command:
```bash
bash scripts/train_custom.sh
```

## Acknowledgement
Our render is built upon [3DGS](https://github.com/graphdeco-inria/gaussian-splatting). The data processing and visualization codes are partially borrowed from [Scaffold-GS](https://github.com/city-super/Scaffold-GS). We thank all the authors for their great repos.

## Citation

If you find this code helpful, please cite:
```
@inproceedings{lin2025longsplat,
  title={LongSplat: Robust Unposed 3D Gaussian Splatting for Casual Long Videos},
  author={Chin-Yang Lin and Cheng Sun and Fu-En Yang and Min-Hung Chen and Yen-Yu Lin and Yu-Lun Liu},
  booktitle={ICCV},
  year={2025}
}
```
