---
title: Remote Sensing Image Classification with YOLOv8
description: YOLOv8-based object detection system for remote sensing imagery using the DIOR dataset, with both CPU and GPU training support.
author: hamdanalhajeri
date: 2025-12-02 14:00:00 +0000
categories: [Projects, AI, Computer Vision]
tags: [python, yolov8, machine-learning, computer-vision, remote-sensing, object-detection, jupyter]
pin: false
---

# Remote Sensing Image Classification with YOLOv8

A lightweight YOLOv8 training and inference pipeline for the DIOR (Dataset for Object Detection in Aerial Images) remote-sensing dataset. This project provides both local CPU training capabilities and Google Colab GPU-accelerated workflows for efficient object detection in aerial and satellite imagery.

## Quick Start 🚀

### Prerequisites
- Python 3.10+ (tested with Python 3.12.10)
- Install dependencies:
```bash
pip install ultralytics pyyaml torch
# Optional for notebook visualizations:
pip install opencv-python-headless matplotlib
```

### Dataset Layout

The project expects the DIOR dataset in the following structure:

```
DIOR-dataset/
├── JPEGImages/          # Image files (or 'images/' in Colab)
├── labels/              # YOLO-format .txt labels
└── ImageSets/Main/      # Split lists
    ├── train.txt
    ├── val.txt
    └── test.txt
```

## Project Overview

This project implements an end-to-end object detection pipeline for remote sensing imagery using YOLOv8, one of the most advanced real-time object detection models. The system is designed to detect various objects in aerial and satellite images from the DIOR dataset.

## Key Features

### Flexible Training Options

- **Local CPU Training** (`train.py`): Conservative defaults optimized for CPU processing
- **GPU Training** (Colab notebook): Leverages cloud GPUs (including A100) for faster training
- **Automatic Path Resolution**: Handles different dataset structures for local and Colab environments

### Training Script (`train.py`)

Run from repository root for CPU training:

```bash
python train.py
```

**Default Configuration:**
- Base model: `yolov8n.pt` (nano variant)
- Epochs: 50
- Image size: 800
- Batch size: 4
- Device: CPU

**Outputs:**
- Training artifacts under `runs/detect/train*/`
- Resolved path lists in `train/` directory
- YAML configuration file
- ONNX export for deployment

### Inference/Testing (`test.py`)

Run predictions on images using trained weights:

```bash
python test.py --weights runs/detect/train8/weights/best.pt \
               --source DIOR-dataset/JPEGImages \
               --sample 0.2 \
               --imgsz 640 \
               --name predict_test
```

**Command-Line Options:**
- `--weights`: Path to trained model weights (.pt file)
- `--source`: Image path or directory
- `--sample`: Fraction (0-1) to subsample directory
- `--imgsz`: Inference image size
- `--name`: Output run name under `runs/predict/`

## Google Colab Notebook

The included Jupyter notebook (`RemoteImageRecognition.ipynb`) provides an end-to-end GPU training workflow optimized for Google Colab:

### Notebook Workflow

1. **Setup Environment**
   - Clone repository
   - Install dependencies
   - Download DIOR dataset via Google Drive

2. **Data Preparation**
   - Unzip and organize dataset
   - Generate clean train/val/test splits
   - Filter out missing or empty labels

3. **Training Configuration**
   - Resolve image paths
   - Generate `data.yaml` configuration
   - Configure GPU training parameters

4. **Model Training**
   - Example: 100 epochs, image size 800, batch 128
   - GPU device selection (device=0)
   - Automatic validation and ONNX export

5. **Inference Demo**
   - GPU-accelerated predictions
   - Visualization of results
   - Performance metrics

6. **Export Results**
   - Zip training outputs
   - Download via Colab interface

### GPU Performance

We utilized A100 GPUs on Google Colab for optimal performance, achieving significantly faster training times compared to CPU execution.

## Technologies Used

- **YOLOv8**: State-of-the-art object detection model
- **Ultralytics**: Official YOLOv8 implementation
- **PyTorch**: Deep learning framework
- **OpenCV**: Image processing
- **Python**: Core programming language
- **Google Colab**: Cloud GPU computing

## Code Architecture

### Key Components

#### `train.py`
- Resolves split lists into full relative image paths
- Builds `data.yaml` with class names and path lists
- Loads YOLOv8n weights and trains with specified parameters
- Runs validation and exports to ONNX format
- CPU-optimized defaults for local development

#### `test.py`
- CLI-based inference tool
- Configurable weights, source, and sampling
- Relative path resolution from script directory
- Optional image subsampling for large datasets
- CPU-based prediction with annotated outputs

#### `RemoteImageRecognition.ipynb`
- End-to-end Colab workflow
- Automated dataset download and preparation
- GPU-accelerated training configuration
- Results visualization and export
- Production-ready model generation

### Helper Scripts

- `annotate.py`: Dataset annotation utilities
- `converter.py`: Format conversion tools
- `split.py`: Train/val/test split generation
- `libtesting.py`: Testing and validation utilities

## Dataset Information

The DIOR (Dataset for Object Detection in Aerial Images) is a large-scale benchmark for object detection in remote sensing imagery, containing:

- High-resolution aerial images
- Multiple object categories
- Diverse scenes and viewing angles
- Challenging detection scenarios

## Training vs. Inference Differences

### Local Training (`train.py`)
- CPU execution (`device="cpu"`)
- Images in `DIOR-dataset/JPEGImages`
- Pre-existing split files required
- Conservative batch size (4) and epochs (50)
- Relative paths from script location

### Colab Training (Notebook)
- GPU execution (`device=0`)
- Images in `DIOR-dataset/images` (after Colab rename)
- Generates split files in notebook
- Larger batch size (128) and epochs (100)
- Absolute paths in `/content/RemoteImageClassification`
- Filters missing/empty labels during preprocessing

## Performance Optimization

**For CPU Training:**
- Use smaller batch sizes (4-8)
- Reduce epochs for faster experimentation
- Consider smaller input sizes (640 vs 800)

**For GPU Training:**
- Increase batch size to maximize GPU usage
- Use larger image sizes for better accuracy
- Leverage mixed precision training

## Results and Outputs

Training generates:
- Model checkpoints (best.pt, last.pt)
- Training metrics and curves
- Validation results
- Confusion matrices
- ONNX exported models (deployment-ready)

Inference creates:
- Annotated images with detected objects
- Confidence scores for each detection
- Bounding box coordinates
- Class predictions

## Use Cases

- Aerial surveillance and monitoring
- Urban planning and analysis
- Agricultural monitoring
- Environmental assessment
- Infrastructure inspection
- Disaster response and assessment

## Future Enhancements

- Real-time video processing
- Multi-scale detection improvements
- Model quantization for edge deployment
- Extended dataset support
- Advanced data augmentation
- Transfer learning capabilities

## Links

You can explore the full project on [GitHub](https://github.com/HamdanAlhajeri/RemoteImageClassification).

---

This project demonstrates the application of state-of-the-art deep learning techniques to remote sensing imagery, providing a robust foundation for aerial and satellite image analysis tasks.
