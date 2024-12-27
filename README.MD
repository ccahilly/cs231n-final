# Mug Makeover: Generating Realistic Ray-Traced Images from Rasterized Renders

## Overview
Mug Makeover is a computer vision project that leverages deep learning techniques to generate ray-traced images from rasterized renders. The goal is to approximate the high-quality lighting and reflections of ray-traced images while maintaining the computational efficiency of rasterization. This repository contains code, datasets, and models used to develop and evaluate our approach.

## Table of Contents
- [Abstract](#abstract)
- [Dataset](#dataset)
- [Methods](#methods)
- [Results](#results)
- [Installation](#installation)
- [Usage](#usage)
- [Contributions](#contributions)
- [Acknowledgements](#acknowledgements)

## Abstract
Ray tracing produces realistic scenes but is computationally expensive. Our approach uses a conditional generative adversarial network (cGAN)—specifically the pix2pix architecture—to generate ray-traced approximations from rasterized inputs. By incorporating additional features like depth and normal maps, we achieved a 4.5x speedup compared to traditional ray tracing while maintaining visual quality.

## Dataset
Our dataset includes 117 3D mug models rendered from 100 viewpoints each, resulting in 23,400 rasterized and ray-traced image pairs (46,800 total images with depth and normal maps). Data was generated in Blender using the Eevee and Cycles rendering engines.

- Image Resolution: 256x256 pixels
- Train-Validation-Test Split: 82-18-17

## Methods
### CNN Classifier
We trained a convolutional neural network (CNN) to distinguish between rasterized and ray-traced images, achieving 93.97% accuracy.

### pix2pix Model Variations
We implemented five variations of the pix2pix model:
1. **Base Model** - Rasterized images as input.
2. **Depth Model** - Added depth maps as additional input channels.
3. **Depth + Skip Model** - Added skip connections to prioritize rasterized features.
4. **Depth + Normal Model** - Added both depth and normal maps for enhanced features.
5. **Deeper Depth Model** - Expanded architecture for more expressive features.

### Evaluation Metrics
- Mean L1 Loss
- CNN Classification Accuracy
- Mean Structural Similarity Index (MSSIM)
- Qualitative Observations
- Saliency Map Analysis
- Inference Time

## Results
The Depth + Normal Model outperformed others based on quantitative and qualitative evaluation:
- Mean L1 Loss: **3.730**
- CNN Accuracy: **99.65%**
- MSSIM: **0.934**

Inference time demonstrated a **4.5x speedup** over traditional ray tracing, reducing rendering time from 1.68s to **0.37s**.

## Installation
1. Clone the repository:
```bash
$ git clone https://github.com/username/mug-makeover.git
$ cd mug-makeover
```

2. Install dependencies:
```bash
$ pip install -r requirements.txt
```

3. Prepare data:
Download dataset and place it in the `data/` folder.

## Usage
### Training CNN Classifier:
```bash
$ python train_cnn.py --config configs/cnn_config.yaml
```

### Training pix2pix Model:
```bash
$ python train_pix2pix.py --config configs/pix2pix_config.yaml
```

### Evaluation:
```bash
$ python evaluate.py --model_path checkpoints/pix2pix_best.pth
```

## Contributions
This project was developed by:
- **Caroline Cahilly** - Model evaluation and analysis
- **Thomas Yim** - Infrastructure and model implementation
- **Daniel Lee** - CNN classifier design and training

## Acknowledgements
We thank:
- Jonathan Tremblay (Nvidia) for dataset design feedback.
- Jun-Yan Zhu for insights on modifying the pix2pix model.
- Yuan Gao (TA) for experimental suggestions and feedback.

The project was inspired by Tremblay et al.'s work on large-scale ray-traced datasets and utilized implementations from Jun-Yan Zhu's pix2pix repository.

## References
- Isola, P. et al. (2018). Image-to-Image Translation with Conditional Adversarial Networks.
- Tremblay, J. et al. (2022). RTMV: A Ray-Traced Multi-View Synthetic Dataset for Novel View Synthesis.

For further details, see the full report in the `docs/` folder.
