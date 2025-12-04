# AI Image Processing Models - Inference Guide

> 📦 **All Docker images available at:** [hub.docker.com/u/sameer513](https://hub.docker.com/u/sameer513)
>
> 🔗 **Original GitHub repositories linked at the [bottom of this README](#github-repositories)**

---

## Prerequisites

- **Docker Desktop** installed and running
- Basic terminal/command line knowledge
- Test images ready on your local machine

---

## Quick Reference

| Model | Task | Docker Image |
|-------|------|--------------|
| NAFNet | Denoising / Deblurring | `sameer513/nafnet-image` |
| PCA-KD | Style Transfer | `sameer513/pca-style-transfer-fixed` |
| CodeFormer | Face Restoration | `sameer513/codeformer_app` |
| U²-Net | Background Removal | `sameer513/u2net-inference` |
| SAM | Segment Anything | `sameer513/sam-cpu-final` |
| LaMa | Image Inpainting | `sameer513/better-lama` |
| LYT-Net | Low-Light Enhancement | `sameer513/lowlight-cpu-bullseye` |

---

## 1. NAFNet (Denoising / Deblurring)

**Task:** Remove noise or blur from images

### Pull & Run
```bash
docker pull sameer513/nafnet-image
docker run -it --name nafnet sameer513/nafnet-image
```

### Inside Container
```bash
export PYTHONPATH=/app:$PYTHONPATH

# For DENOISING
python basicsr/demo.py \
  -opt options/test/SIDD/NAFNet-width64.yml \
  --input_path ./demo/<YOUR_NOISY_IMAGE>.png \
  --output_path ./demo/denoised_output.png

# For DEBLURRING
python basicsr/demo.py \
  -opt options/test/REDS/NAFNet-width64.yml \
  --input_path ./demo/<YOUR_BLURRY_IMAGE>.jpg \
  --output_path ./demo/deblurred_output.png
```

### Copy Output (from another terminal)
```bash
docker cp nafnet:/app/demo/denoised_output.png <YOUR_LOCAL_OUTPUT_PATH>/
# OR
docker cp nafnet:/app/demo/deblurred_output.png <YOUR_LOCAL_OUTPUT_PATH>/
```

---

## 2. PCA-KD (Style Transfer)

**Task:** Transfer artistic style from one image to another

### Pull & Run
```bash
docker pull sameer513/pca-style-transfer-fixed
docker run -it --name pca-style sameer513/pca-style-transfer-fixed
```

### Copy Input Images (from another terminal)
```bash
docker cp <YOUR_CONTENT_IMAGE_PATH> pca-style:/app/figures/content/
docker cp <YOUR_STYLE_IMAGE_PATH> pca-style:/app/figures/style/
```

### Run Inference (inside container)
```bash
python demo.py \
  --content figures/content/<CONTENT_IMAGE_NAME> \
  --style figures/style/<STYLE_IMAGE_NAME>
```

### Copy Output (from another terminal)
```bash
docker cp pca-style:/app/results/output.jpg <YOUR_LOCAL_OUTPUT_PATH>/
```

---

## 3. CodeFormer (Face Restoration)

**Task:** Restore and enhance degraded faces

> **Note:** Input images are processed at 512×512 resolution (fixed)

### Pull & Run
```bash
docker pull sameer513/codeformer_app
docker run -it --name codeformer sameer513/codeformer_app
```

### Copy Input Image (from another terminal)
```bash
docker cp <YOUR_FACE_IMAGE_PATH> codeformer:/cf/input/
```

### Run Inference (inside container)
```bash
cd /cf/CodeFormer
python inference_codeformer.py \
  --w 0.7 \
  --test_path /cf/input \
  --face_upsample \
  --has_aligned
```

**Parameters:**
- `--w`: Fidelity weight (0.0-1.0). Higher = more faithful to input, Lower = more enhancement

### Copy Output (from another terminal)
```bash
docker cp codeformer:/cf/output/input_0.7/final_results <YOUR_LOCAL_OUTPUT_PATH>/
```

---

## 4. U²-Net (Background Removal / Segmentation)

**Task:** Remove background or segment objects from images

### Pull & Run
```bash
docker pull sameer513/u2net-inference
docker run -it --name u2net sameer513/u2net-inference
```

### Copy Input Image (from another terminal)
```bash
docker cp <YOUR_IMAGE_PATH> u2net:/app/samples/
```

### Run Inference (inside container)
```bash
# General background removal
python app.py \
  samples/<YOUR_IMAGE_NAME> \
  samples/output.png \
  models/u2net.onnx

# Human segmentation (optimized for people)
python app.py \
  samples/<YOUR_IMAGE_NAME> \
  samples/output.png \
  models/u2net_human_seg.onnx
```

### Copy Output (from another terminal)
```bash
docker cp u2net:/app/samples/output.png <YOUR_LOCAL_OUTPUT_PATH>/
```

---

## 5. SAM - Segment Anything Model

**Task:** Segment any object in an image using point prompts

### Pull & Run
```bash
docker pull sameer513/sam-cpu-final
docker run -it --name sam sameer513/sam-cpu-final
```

### Copy Input Image (from another terminal)
```bash
docker cp <YOUR_IMAGE_PATH> sam:/app/
```

### Run Inference (inside container)
```bash
# Generate segmentation mask (specify point coordinates x,y)
python sam_inference.py \
  --image <YOUR_IMAGE_NAME> \
  --point <X_COORD>,<Y_COORD> \
  --output mask.png \
  --checkpoint sam_vit_b_01ec64.pth

# Optional: Dilate the mask for better coverage
python dialate.py \
  --mask mask.png \
  --kernel 7 \
  --iter 2 \
  --out dilated_mask.png
```

**Parameters:**
- `--point`: X,Y coordinates of the object to segment (e.g., `808,160`)
- `--kernel`: Dilation kernel size
- `--iter`: Number of dilation iterations

### Copy Output (from another terminal)
```bash
docker cp sam:/app/dilated_mask.png <YOUR_LOCAL_OUTPUT_PATH>/
# OR
docker cp sam:/app/mask.png <YOUR_LOCAL_OUTPUT_PATH>/
```

---

## 6. LaMa (Image Inpainting)

**Task:** Remove objects and fill in missing regions

### Pull & Run
```bash
docker pull sameer513/better-lama
docker run -it --name lama sameer513/better-lama
```

### Copy Input Files (from another terminal)
```bash
docker cp <YOUR_IMAGE_PATH> lama:/app/input/
docker cp <YOUR_MASK_PATH> lama:/app/input/
```

### Run Inference (inside container)

**Option A: Using RGB image + binary mask**
```bash
python simple_infer.py \
  --model /app/models/big-lama \
  --image /app/input/<YOUR_IMAGE_NAME> \
  --mask /app/input/<YOUR_MASK_NAME> \
  --out /app/output/result.png \
  --dilate 15
```

**Option B: Using RGB image + subject with transparent background**
```bash
python simple_infer.py \
  --model /app/models/big-lama \
  --image /app/input/<YOUR_IMAGE_NAME> \
  --subject /app/input/<YOUR_SUBJECT_PNG> \
  --out /app/output/result.png \
  --dilate 15
```

**Parameters:**
- `--dilate`: Mask dilation amount (pixels)

### Copy Output (from another terminal)
```bash
docker cp lama:/app/output/result.png <YOUR_LOCAL_OUTPUT_PATH>/
```

---

## 7. LYT-Net (Low-Light Enhancement)

**Task:** Enhance dark/low-light images

### Pull & Run
```bash
docker pull sameer513/lowlight-cpu-bullseye
docker run -it --name lytnet sameer513/lowlight-cpu-bullseye
```

### Copy Input Image (from another terminal)
```bash
docker cp <YOUR_DARK_IMAGE_PATH> lytnet:/app/
```

### Run Inference (inside container)
```bash
python infer.py \
  --weights best_model_LOLv1.pth \
  --input <YOUR_IMAGE_NAME> \
  --output output.jpg \
  --brightness 0.5
```

**Parameters:**
- `--brightness`: Brightness adjustment factor (0.0-1.0)

### Copy Output (from another terminal)
```bash
docker cp lytnet:/app/output.jpg <YOUR_LOCAL_OUTPUT_PATH>/
```

---

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| `docker: command not found` | Install Docker Desktop |
| `permission denied` | Run terminal as Administrator (Windows) or use `sudo` (Linux/Mac) |
| Container not found | Use `docker ps -a` to list all containers and get correct container ID/name |
| Out of memory | Close other applications; these models run on CPU |
| Slow inference | Expected on CPU; see performance table below |

### Useful Docker Commands

```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop a container
docker stop <CONTAINER_NAME>

# Remove a container
docker rm <CONTAINER_NAME>

# Enter a running container
docker exec -it <CONTAINER_NAME> bash
```

---

## Expected Performance (CPU)

| Model | Resolution | Approx. Latency |
|-------|------------|-----------------|
| NAFNet | 512×512 | ~9 seconds |
| PCA-KD | 512×512 | ~0.1 seconds |
| CodeFormer | 512×512 | ~5 seconds |
| U²-Net | 320×320 | ~0.6 seconds |
| SAM | 1024×1024 | ~9 seconds |
| LaMa | 512×512 | ~2.5 seconds |
| LYT-Net | 512×512 | ~1 second |

> **Note:** Actual performance varies based on your hardware.

---

## Placeholder Reference

Replace these placeholders with your actual values:

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `<YOUR_IMAGE_PATH>` | Full path to image on your PC | `C:\Users\Name\image.jpg` or `/home/user/image.jpg` |
| `<YOUR_LOCAL_OUTPUT_PATH>` | Folder to save results | `C:\Users\Name\outputs` or `/home/user/outputs` |
| `<YOUR_IMAGE_NAME>` | Image filename (inside container) | `photo.jpg` |
| `<X_COORD>,<Y_COORD>` | Point coordinates for SAM | `500,300` |
| `<CONTAINER_NAME>` | Name given to container | `nafnet`, `lama`, etc. |

---

## GitHub Repositories

| Model | Original Repository |
|-------|---------------------|
| NAFNet | [github.com/megvii-research/NAFNet](https://github.com/megvii-research/NAFNet) |
| PCA-KD | [github.com/chiutaiyin/PCA-Knowledge-Distillation](https://github.com/chiutaiyin/PCA-Knowledge-Distillation) |
| CodeFormer | [github.com/sczhou/CodeFormer](https://github.com/sczhou/CodeFormer) |
| U²-Net | [github.com/xuebinqin/U-2-Net](https://github.com/xuebinqin/U-2-Net) |
| SAM | [github.com/facebookresearch/segment-anything](https://github.com/facebookresearch/segment-anything) |
| LaMa | [github.com/advimman/lama](https://github.com/advimman/lama) |
| LYT-Net | [github.com/albrateanu/LYT-Net](https://github.com/albrateanu/LYT-Net) |

---

## Docker Hub

All pre-built Docker images are available at: **[hub.docker.com/u/sameer513](https://hub.docker.com/u/sameer513)**

You can browse and pull any model directly from there.

