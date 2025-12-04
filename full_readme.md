# Adobe PS Project

A complete image processing application featuring AI powered enhancement capabilities and layer based editing workflows. This project was developed for Inter IIT Tech 14.0.

## Table of Contents

1. [Overview](#overview)
2. [System Requirements](#system-requirements)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Running the Application](#running-the-application)
6. [Docker Containers](#docker-containers)
7. [AI Models and Compute Profile](#ai-models-and-compute-profile)
8. [Inference Guide](#inference-guide)
9. [API Reference](#api-reference)
10. [Project Structure](#project-structure)
11. [Troubleshooting](#troubleshooting)
12. [Citations](#citations)

## Overview

This backend powers an image editing application with two distinct editing workflows.

**AI Sequential Editing** enables chaining multiple AI operations on a single image with full history tracking and undo capability. Users can apply operations such as relight, denoise, face restoration, and style transfer in sequence.

**Layer Based Editing** provides a multi layer composition system similar to Adobe Photoshop. Users can work with multiple independent image layers, each with individual properties including opacity, blend modes, and transformations.

**AI Auto Enhancement** leverages Google Gemini AI to intelligently analyze composite canvases and recommend enhancement priorities. The system automatically detects which enhancements are needed and suggests the optimal application order.

## System Requirements

Before installation, ensure your system meets the following requirements.

A computer running Windows, macOS, or Linux with at least 10GB of free disk space for Docker images. An active internet connection is required for downloading dependencies and Docker images. Basic familiarity with terminal or command prompt operations is assumed.

## Installation

### Node.js

Node.js is the runtime environment required to execute the backend server.

1. Navigate to https://nodejs.org/ and download the LTS version
2. Execute the installer and complete the installation wizard
3. Verify the installation by opening a terminal and running `node --version` and `npm --version`

### MongoDB

MongoDB serves as the database for storing user data, projects, and image metadata.

**Cloud Installation (MongoDB Atlas)**

1. Navigate to https://www.mongodb.com/cloud/atlas and create a free account
2. Create a free tier cluster
3. Create a database user and record the credentials
4. Obtain the connection string in the format `mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/`

**Local Installation**

1. Download MongoDB Community Edition from https://www.mongodb.com/try/download/community
2. Install and start the MongoDB service
3. The default connection string is `mongodb://localhost:27017`

### Docker Desktop

Docker is required to run the AI model containers.

1. Download Docker Desktop from https://www.docker.com/products/docker-desktop/
2. Install the application (installation may require 10 to 15 minutes)
3. Restart your computer after installation
4. Launch Docker Desktop and wait for the service to initialize

### Expo Go (Mobile Testing)

Expo Go enables testing the React Native frontend on physical devices.

1. Install Expo Go from the Google Play Store (Android) or App Store (iOS)
2. Keep the application installed for later use during development

### Backend Dependencies

1. Open a terminal and navigate to the backend directory: `cd backend`
2. Execute `npm install` to install all required packages
3. Wait for the installation to complete (typically 2 to 5 minutes)

### Frontend Dependencies

1. Open a new terminal and navigate to the frontend directory: `cd frontend`
2. Execute `npm install` to install all required packages
3. Wait for the installation to complete (typically 5 to 10 minutes)

## Configuration

### Backend Environment Variables

Create or modify the `.env` file in the backend directory with the following configuration.

```
NODE_ENV=development
PORT=4000
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_random_secret_key
JWT_EXPIRES_IN=30d
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
GEMINI_API_KEY=your_gemini_api_key
```

**Obtaining API Keys**

| Service | Source |
|---------|--------|
| MongoDB | MongoDB Atlas Dashboard at mongodb.com/cloud/atlas |
| Cloudinary | Cloudinary Console at cloudinary.com/console |
| Gemini AI | Google AI Studio at makersuite.google.com/app/apikey |
| JWT Secret | Any random string generator such as randomkeygen.com |

### Frontend API URL Configuration

1. Determine your computer's IP address by running `ipconfig` (Windows) or `ifconfig` (macOS/Linux)
2. Open `frontend/constants/api.ts`
3. Update the API URL with your IP address

```typescript
const DEFAULT_DEVELOPMENT_API_URL = 'http://YOUR_IP_ADDRESS:4000/api/v1/adobe-ps';
const ANDROID_EMULATOR_API_URL = 'http://YOUR_IP_ADDRESS:4000/api/v1/adobe-ps';
```

For Android Emulator, use `http://10.0.2.2:4000/api/v1/adobe-ps`. For iOS Simulator, use `http://localhost:4000/api/v1/adobe-ps`. For physical devices, ensure both the device and computer are connected to the same WiFi network.

## Running the Application

### Starting the Backend Server

1. Open a terminal and navigate to the backend directory
2. Execute `npm start`
3. Verify the output displays "Server running on port 4000" and "Database connected successfully"
4. Keep this terminal window open

### Starting the Frontend Server

1. Open a new terminal and navigate to the frontend directory
2. Execute `npm start` or `expo start`
3. A QR code will appear in the terminal

### Running on Physical Devices

1. Ensure your phone and computer are on the same WiFi network
2. Open the Expo Go application on your phone
3. Scan the QR code displayed in the terminal
4. The application will load on your device

### Running on Emulators

For Android Emulator, press `a` in the Expo terminal. For iOS Simulator (macOS only), press `i` in the Expo terminal. For web browser, press `w` in the Expo terminal.

## Docker Containers

The application requires eight Docker containers for AI image processing operations.

### Container Reference

| Container Name | Docker Image | Purpose |
|----------------|--------------|---------|
| lowlight-service | sameer513/lowlight-cpu-bullseye | Low light image enhancement |
| codeformer-service | sameer513/codeformer_app | Face restoration |
| nafnet-service | sameer513/nafnet-image | Denoise and deblur |
| style-transfer-service | sameer513/pca-style-transfer-fixed | Artistic style transfer |
| background-removal-service | sameer513/u2net-inference | Background removal |
| pct-net-service | sameer513/pct-net-final | Background harmonization |
| object-masking-service | sameer513/sam-cpu-final | Object segmentation |
| object-remover-service | sameer513/better-lama | Object inpainting |

### Pulling Docker Images

Execute the following commands to download all required images. Total download size is approximately 10 to 15GB.

```
docker pull sameer513/lowlight-cpu-bullseye
docker pull sameer513/codeformer_app
docker pull sameer513/nafnet-image
docker pull sameer513/pca-style-transfer-fixed
docker pull sameer513/u2net-inference
docker pull sameer513/pct-net-final
docker pull sameer513/sam-cpu-final
docker pull sameer513/better-lama
```

### Creating and Starting Containers

```
docker run -d --name lowlight-service sameer513/lowlight-cpu-bullseye tail -f /dev/null
docker run -d --name codeformer-service sameer513/codeformer_app tail -f /dev/null
docker run -d --name nafnet-service sameer513/nafnet-image tail -f /dev/null
docker run -d --name style-transfer-service sameer513/pca-style-transfer-fixed tail -f /dev/null
docker run -d --name background-removal-service sameer513/u2net-inference tail -f /dev/null
docker run -d --name pct-net-service sameer513/pct-net-final tail -f /dev/null
docker run -d --name object-masking-service sameer513/sam-cpu-final tail -f /dev/null
docker run -d --name object-remover-service sameer513/better-lama tail -f /dev/null
```

Verify all containers are running by executing `docker ps`. Eight containers should be listed with status "Up".

## AI Models and Compute Profile

### Model Specifications

| Model | Task | Parameters | Checkpoint Size |
|-------|------|------------|-----------------|
| CodeFormer | Face Restoration | 94.11M | 376.64 MB |
| LaMa | Image Inpainting | 51.06M | 410.05 MB |
| U2Net | Salient Object Segmentation | 44.0M | 167.8 MB |
| SAM (ViT-B) | Promptable Segmentation | 93.74M | 357 MB |
| PCA-KD | Style Transfer | 72.84K | 0.29 MB |
| LYT-Net | Low Light Enhancement | 44.92K | 0.20 MB |
| NAFNet | Image Denoise/Deblur | 67.89M | 258.98 MB |
| PCT-Net ViT | Image Harmonization | 4.81M | 18.39 MB |

### CPU Performance at 512x512 Resolution

| Model | Latency | FPS |
|-------|---------|-----|
| PCA-KD | 95.58 ms | 10.46 |
| PCT-Net | 356 ms | 2.81 |
| U2Net | 600 ms | 1.67 |
| LYT-Net | 1,006.31 ms | 0.99 |
| LaMa | 2,437 ms | 0.41 |
| CodeFormer | 4,761 ms | 0.21 |
| NAFNet | 8,975.93 ms | 0.11 |
| SAM | 9,009 ms | 0.11 |

### Model Size Rankings (Smallest to Largest)

| Rank | Model | Parameters | Checkpoint |
|------|-------|------------|------------|
| 1 | LYT-Net | 44.92K | 0.20 MB |
| 2 | PCA-KD | 72.84K | 0.29 MB |
| 3 | U2Net-lite | 1.13M | 4.7 MB |
| 4 | PCT-Net ViT | 4.81M | 18.39 MB |
| 5 | U2Net | 44.0M | 167.8 MB |
| 6 | LaMa | 51.06M | 410.05 MB |
| 7 | NAFNet | 67.89M | 258.98 MB |
| 8 | SAM | 93.74M | 357 MB |
| 9 | CodeFormer | 94.11M | 376.64 MB |

### Key Observations

LYT-Net and PCA-KD are approximately 1000 times smaller than the larger models. PCA-KD achieves the fastest CPU inference at 15.23 FPS at 256x256 resolution, which is 10 times faster than the next best performer. CodeFormer is the only model with a fixed input resolution requirement of 512x512. SAM is a foundation model trained on 1.1 billion masks with zero-shot capability. PCA-KD and PCT-Net are viable for interactive real-time use cases.

## Inference Guide

### NAFNet (Denoising and Deblurring)

```
docker pull sameer513/nafnet-image
docker run -it --name nafnet sameer513/nafnet-image
```

Inside the container:
```
export PYTHONPATH=/app:$PYTHONPATH

# For denoising
python basicsr/demo.py \
  -opt options/test/SIDD/NAFNet-width64.yml \
  --input_path ./demo/input.png \
  --output_path ./demo/denoised_output.png

# For deblurring
python basicsr/demo.py \
  -opt options/test/REDS/NAFNet-width64.yml \
  --input_path ./demo/input.jpg \
  --output_path ./demo/deblurred_output.png
```

Copy output: `docker cp nafnet:/app/demo/denoised_output.png ./output/`

### PCA-KD (Style Transfer)

```
docker pull sameer513/pca-style-transfer-fixed
docker run -it --name pca-style sameer513/pca-style-transfer-fixed
```

Copy input images:
```
docker cp content_image.jpg pca-style:/app/figures/content/
docker cp style_image.jpg pca-style:/app/figures/style/
```

Inside the container:
```
python demo.py \
  --content figures/content/content_image.jpg \
  --style figures/style/style_image.jpg
```

Copy output: `docker cp pca-style:/app/results/output.jpg ./output/`

### CodeFormer (Face Restoration)

Input images are processed at 512x512 resolution.

```
docker pull sameer513/codeformer_app
docker run -it --name codeformer sameer513/codeformer_app
```

Copy input: `docker cp face_image.jpg codeformer:/cf/input/`

Inside the container:
```
cd /cf/CodeFormer
python inference_codeformer.py \
  --w 0.7 \
  --test_path /cf/input \
  --face_upsample \
  --has_aligned
```

The `--w` parameter controls fidelity weight (0.0 to 1.0). Higher values produce output more faithful to the input while lower values apply more enhancement.

Copy output: `docker cp codeformer:/cf/output/input_0.7/final_results ./output/`

### U2Net (Background Removal)

```
docker pull sameer513/u2net-inference
docker run -it --name u2net sameer513/u2net-inference
```

Copy input: `docker cp image.jpg u2net:/app/samples/`

Inside the container:
```
# General background removal
python app.py \
  samples/image.jpg \
  samples/output.png \
  models/u2net.onnx

# Human segmentation
python app.py \
  samples/image.jpg \
  samples/output.png \
  models/u2net_human_seg.onnx
```

Copy output: `docker cp u2net:/app/samples/output.png ./output/`

### SAM (Segment Anything Model)

```
docker pull sameer513/sam-cpu-final
docker run -it --name sam sameer513/sam-cpu-final
```

Copy input: `docker cp image.jpg sam:/app/`

Inside the container:
```
python sam_inference.py \
  --image image.jpg \
  --point 500,300 \
  --output mask.png \
  --checkpoint sam_vit_b_01ec64.pth

# Optional mask dilation
python dialate.py \
  --mask mask.png \
  --kernel 7 \
  --iter 2 \
  --out dilated_mask.png
```

Copy output: `docker cp sam:/app/dilated_mask.png ./output/`

### LaMa (Image Inpainting)

```
docker pull sameer513/better-lama
docker run -it --name lama sameer513/better-lama
```

Copy inputs:
```
docker cp image.jpg lama:/app/input/
docker cp mask.png lama:/app/input/
```

Inside the container:
```
python simple_infer.py \
  --model /app/models/big-lama \
  --image /app/input/image.jpg \
  --mask /app/input/mask.png \
  --out /app/output/result.png \
  --dilate 15
```

Copy output: `docker cp lama:/app/output/result.png ./output/`

### LYT-Net (Low Light Enhancement)

```
docker pull sameer513/lowlight-cpu-bullseye
docker run -it --name lytnet sameer513/lowlight-cpu-bullseye
```

Copy input: `docker cp dark_image.jpg lytnet:/app/`

Inside the container:
```
python infer.py \
  --weights best_model_LOLv1.pth \
  --input dark_image.jpg \
  --output output.jpg \
  --brightness 0.5
```

Copy output: `docker cp lytnet:/app/output.jpg ./output/`

### PCT-Net (Image Harmonization)

```
docker pull sameer513/pct-net-final
docker run -it --name pctnet sameer513/pct-net-final
```

Copy inputs:
```
docker cp composite.jpg pctnet:/workspace/examples/composites/
docker cp mask.png pctnet:/workspace/examples/composites/
```

Inside the container:
```
python3 run_inference.py \
  --image /workspace/examples/composites/composite.jpg \
  --mask /workspace/examples/composites/mask.png \
  --weights pretrained_models/PCTNet_ViT.pth \
  --model_type ViT_pct \
  --out /workspace/examples/composites/output.jpg
```

Copy output: `docker cp pctnet:/workspace/examples/composites/output.jpg ./output/`

## API Reference

### Base URL

```
http://localhost:4000/api/v1/adobe-ps
```

All protected routes require a JWT token in the Authorization header: `Authorization: Bearer YOUR_JWT_TOKEN`

### Authentication Routes

**POST /auth/signup** creates a new user account.

Request body:
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

**POST /auth/login** authenticates a user and returns a JWT token.

Request body:
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

**POST /auth/verify-otp** verifies the OTP sent during signup.

**POST /auth/resend-otp** resends the OTP to the user's email.

**POST /auth/google** authenticates via Google OAuth.

**GET /auth/logout** terminates the user session.

### Layer Based Project Routes

**POST /projects/create** creates a new layer based project.

Request body:
```json
{
  "title": "My Design Project",
  "canvas": {
    "width": 1920,
    "height": 1080,
    "backgroundColor": "#ffffff"
  }
}
```

### Gemini AI Routes

**POST /gemini/interpret** interprets user commands using Gemini AI.

**POST /gemini/auto-enhance/:projectId** analyzes a project and recommends enhancements.

**POST /gemini/apply-enhancements/:projectId** applies recommended enhancements sequentially.

### Settings Routes

**GET /settings** retrieves user settings.

**PATCH /settings/max-versions** updates the maximum versions limit (1 to 15).

## Project Structure

```
Inter-IIT-tech-Adobe-PS/
    backend/
        .env
        package.json
        server.js
    frontend/
        constants/
            api.ts
        package.json
    README.md
```

## Troubleshooting

### Connection Issues

If the frontend cannot connect to the backend, verify that the backend server is running on port 4000. Confirm that the API URL in `frontend/constants/api.ts` matches your IP address. Ensure both devices are on the same WiFi network. Consider temporarily disabling the firewall for testing.

### Database Issues

If MongoDB connection fails, verify the `MONGODB_URI` in the backend `.env` file. For MongoDB Atlas, ensure your IP address is whitelisted. For local MongoDB, confirm the service is running.

### Docker Issues

If containers are not found, verify Docker Desktop is running. List all containers with `docker ps -a`. Start stopped containers with `docker start container-name`.

### Dependency Issues

If npm install fails, verify Node.js is installed correctly. Delete the `node_modules` folder and `package-lock.json`, then run `npm install` again.

### AI Operation Issues

If AI operations are not working, verify all eight Docker containers are running with `docker ps`. Check backend logs for specific errors. Confirm the `GEMINI_API_KEY` is set in the `.env` file.

### Useful Commands

```
# List running containers
docker ps

# List all containers
docker ps -a

# Start a container
docker start container-name

# Stop a container
docker stop container-name

# View container logs
docker logs container-name

# Enter a running container
docker exec -it container-name bash
```

## Citations

### NAFNet

Chen, L., Chu, X., Zhang, X., and Sun, J. (2022). Simple Baselines for Image Restoration. arXiv preprint arXiv:2204.04676.

### LYT-Net

Brateanu, A., Balmez, R., Avram, A., Orhei, C., and Ancuti, C. (2025). LYT-NET: Lightweight YUV Transformer-based Network for Low-light Image Enhancement. IEEE Signal Processing Letters.

### U2Net

Qin, X., Zhang, Z., Huang, C., Dehghan, M., Zaiane, O., and Jagersand, M. (2020). U2-Net: Going Deeper with Nested U-Structure for Salient Object Detection. Pattern Recognition, 106, 107404.

### CodeFormer

Zhou, S., Chan, K. C.K., Li, C., and Loy, C. C. (2022). Towards Robust Blind Face Restoration with Codebook Lookup TransFormer. NeurIPS.

### LaMa

Suvorov, R., Logacheva, E., Mashikhin, A., Remizova, A., Ashukha, A., Silvestrov, A., Kong, N., Goka, H., Park, K., and Lempitsky, V. (2021). Resolution-robust Large Mask Inpainting with Fourier Convolutions. arXiv preprint arXiv:2109.07161.

### SAM

Kirillov, A., Mintun, E., Ravi, N., Mao, H., Rolland, C., Gustafson, L., Xiao, T., Whitehead, S., Berg, A. C., Lo, W., Dollar, P., and Girshick, R. (2023). Segment Anything. arXiv:2304.02643.

### PCA-KD

Chiu, T. and Gurari, D. (2022). PCA-Based Knowledge Distillation Towards Lightweight and Content-Style Balanced Photorealistic Style Transfer Models. Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 7844-7853.

### PCT-Net

Guerreiro, J. J. A., Nakazawa, M., and Stenger, B. (2023). PCT-Net: Full Resolution Image Harmonization Using Pixel-Wise Color Transformations. Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 5917-5926.

## Original Repositories

| Model | Repository |
|-------|------------|
| NAFNet | github.com/megvii-research/NAFNet |
| PCA-KD | github.com/chiutaiyin/PCA-Knowledge-Distillation |
| CodeFormer | github.com/sczhou/CodeFormer |
| U2Net | github.com/xuebinqin/U-2-Net |
| SAM | github.com/facebookresearch/segment-anything |
| LaMa | github.com/advimman/lama |
| LYT-Net | github.com/albrateanu/LYT-Net |
| PCT-Net | github.com/rakutentech/PCT-Net-Image-Harmonization |

## Project Information

Last Updated: December 2024

Project: Inter IIT Tech 14.0 Adobe PS
