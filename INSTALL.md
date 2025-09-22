# Installation Guide

This project uses the conda environment setup method as discussed in the [official FAISS installation guide](https://github.com/facebookresearch/faiss/blob/main/INSTALL.md).

We will install the `faiss-gpu-cuvs` package to leverage GPU acceleration and get the optimized version.

## Prerequisites

Ensure Miniconda is installed or set up as described below. This guide is tested on Ubuntu 22.04 with Python 3.12, so we are using the Python 3.12 compatible variant.

### Install Miniconda

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-py312_25.7.0-2-Linux-x86_64.sh
bash ~/Miniconda3-latest-Linux-x86_64.sh
source ~/.bashrc
```

## Environment Setup

Create a conda environment with the required dependencies:

```bash
conda create -n faiss-dev -y \
    -c pytorch -c nvidia -c rapidsai -c conda-forge \
    python=3.11 \
    libnvjitlink \
    faiss-gpu-cuvs=1.12.0 \
    jupyterlab ipykernel
```

## Next Steps

After creating the environment, activate it with:

```bash
conda activate faiss-dev
```


## Running Jupyter Lab

To work with the project, open this folder through Jupyter Lab:

```bash
jupyter lab
```

This will start the Jupyter Lab server and open it in your default web browser, allowing you to work with notebooks and explore the project files.

