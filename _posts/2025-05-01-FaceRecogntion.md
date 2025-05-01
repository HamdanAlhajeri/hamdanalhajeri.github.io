---
title: Face Recognition
description: A web application for face detection and recognition using deep learning models. This system can detect faces in images, extract facial features, and compare facial similarities against reference images.
author: hamdanalhajeri
date: 2025-05-01
categories: [Projects, AI, Computer Vision]
tags: [face-recognition, deep-learning, arcface, flask, react, docker, python, tensorflow, opencv]
pin: false
---

# Face Recognition System

A web application for face detection and recognition using deep learning models. This system can detect faces in images, extract facial features, and compare facial similarities against reference images.

## Features

* Real-time face detection in uploaded images
* Facial feature extraction using ArcFace model
* Similarity comparison between detected faces and reference faces
* Database storage for detected faces
* Interactive web interface

## Technologies Used

* **Frontend**: React, Material-UI
* **Backend**: Flask, TensorFlow, OpenCV, DeepFace, RetinaFace
* **Database**: SQLite
* **Containerization**: Docker, Docker Compose
* **Web Server**: Nginx

## Architecture

The application consists of three main services:

1. **Backend (Python/Flask)**: Handles face detection, feature extraction, and similarity comparison using deep learning models
2. **Frontend (React)**: Provides the user interface for uploading images and viewing results
3. **Nginx**: Serves the static frontend files and routes API requests to the backend

## API Endpoints

* `/api/process_image` - Process an image for face detection and analysis
* `/faces` - Get all faces from the database
* `/faces/<face_id>` - Delete or update a specific face record
* `/reference-folders` - Get available reference image folders

You can explore the full project on [GitHub](https://github.com/HamdanAlhajeri/FaceRec).