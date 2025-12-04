# Compute Profile

| Metric | CodeFormer | LaMa | U²-Net | SAM (ViT-B) | PCA-KD | LYT-Net | NAFNet-width64 | PCT-Net ViT |
|--------|------------|------|--------|-------------|--------|---------|----------------|-------------|
| **Task** | Face Restoration | Image Inpainting | Salient Object Segmentation | Promptable Segmentation | Style Transfer | Low-Light Enhancement | Image Denoise/Deblur | Image Harmonization |
| | | | | | | | | |
| **PARAMETERS** | | | | | | | | |
| Total | 94.11M | 51.06M | 44.0M (main) / 1.13M (lite) | 93.74M | 72.84K | 44.92K | 67.89M | 4.81M |
| | | | | | | | | |
| **MODEL SIZE** | | | | | | | | |
| Checkpoint | 376.64 MB | 410.05 MB | 167.8 MB / 4.7 MB (lite) | 357 MB | 0.29 MB | 0.20 MB | 258.98 MB | 18.39 MB |
| | | | | | | | | |
| **INPUT/OUTPUT** | | | | | | | | |
| Input Resolution | 512×512 (FIXED) | Variable + Mask | Variable (opt 320×320) | Variable → 1024 | Variable (÷32) | Variable | Variable | Variable (composite + mask) |
| Output Resolution | 512×512×3 | Same as input | Same (1 channel) | 3 masks @ input res | Same as input | Same as input | Same as input | Same as input |
| | | | | | | | | |
| **CPU INFERENCE** | | | | | | | | |
| Latency (256×256) | - | - | ~200 ms | 9,009 ms | **65.67 ms** | 245.06 ms | 2,013.88 ms | 126 ms |
| Latency (512×512) | 4,761 ms | 2,437 ms | ~600 ms | 9,009 ms | **95.58 ms** | 1,006.31 ms | 8,975.93 ms | 356 ms |
| Latency (Native) | 4,761 ms @512 | 2,437 ms @512 | ~600 ms @320 | 9,009 ms @1024 | 65.67 ms @256 | 245.06 ms @256 | 2,013.88 ms @256 | 659 ms @512 |
| Throughput (FPS) | 0.21 | 0.41 | ~1.5 | 0.11 | **15.23** | 4.08 | 0.50 | 2.4 |
| | | | | | | | | |
| **GPU INFERENCE** | | | | | | | | |
| GPU Latency (mean) | 70 ms (V100) | ~50-100 ms | 19.73 ms | ~50 ms (A100) | 40 ms (GTX 1080) | 2.3 ms | 8.4% SOTA cost | 25.68 ms |
| | | | | | | | | |
| **ACCURACY** | | | | | | | | |
| Benchmark Dataset | CelebA-Test | Places2 | DUTS-TE | SA-1B (23 datasets) | COCO | LOLv1 | GoPro / SIDD | iHarmony4 |
| | | | | | | | | |
| **ARCHITECTURE** | | | | | | | | |
| Type | Transformer + VQGAN | FFT-based CNN | Nested U-Net | Vision Transformer | Distilled CNN | YUV Transformer | UNet + NAFBlocks | Vision Transformer |
| | | | | | | | | |
| **EFFICIENCY** | | | | | | | | |
| FLOPs/GMACs | ~100 GFLOPs | ~50 GFLOPs | ~30 GFLOPs | ~180 GFLOPs | 0.72% of WCT2 | 3.49 GFLOPs | 1.1-65 GMACs | ~10 GFLOPs |
| Speed Rank (CPU) | 7th | 5th | 3rd | 8th (slowest) | **1st (fastest)** | 2nd | 4th | 5th |
| | | | | | | | | |
| **TRAINING DATA** | | | | | | | | |
| Dataset | FFHQ | Places2 | DUTS-TR | SA-1B (11M images) | COCO | LOL | GoPro/SIDD | iHarmony4 |
| | | | | | | | | |
| **VARIANTS** | | | | | | | | |
| Available | CodeFormer | LaMa | U²-Net, U²-Net-lite | ViT-B, ViT-L, ViT-H | MobileNet, VGG | LYT | width32, width64, SIDD | ViT_pct, CNN_pct, sym, polynomial, identity, mul, add |
| Tested Variant | Base | Big-LaMa | Full | ViT-B (93.74M) | MobileNet (72.84K) | Base (44.92K) | width64 (67.89M) | ViT_pct (4.81M) |

---

## Summary Rankings

### By Model Size (Smallest to Largest)
| Rank | Model | Parameters | Checkpoint |
|------|-------|------------|------------|
| 1 | LYT-Net | 44.92K | 0.20 MB |
| 2 | PCA-KD | 72.84K | 0.29 MB |
| 3 | U²-Net-lite | 1.13M | 4.7 MB |
| 4 | PCT-Net ViT | 4.81M | 18.39 MB |
| 5 | U²-Net | 44.0M | 167.8 MB |
| 6 | LaMa | 51.06M | 410.05 MB |
| 7 | NAFNet | 67.89M | 258.98 MB |
| 8 | SAM | 93.74M | 357 MB |
| 9 | CodeFormer | 94.11M | 376.64 MB |

### By CPU Speed @256×256 (Fastest to Slowest)
| Rank | Model | Latency | FPS |
|------|-------|---------|-----|
| 1 | PCA-KD | 65.67 ms | 15.23 |
| 2 | PCT-Net | 126 ms | 2.4 |
| 3 | U²-Net | ~200 ms | ~1.5 |
| 4 | LYT-Net | 245.06 ms | 4.08 |
| 5 | NAFNet | 2,013.88 ms | 0.50 |
| 6 | SAM | 9,009 ms | 0.11 |

### By CPU Speed @512×512 (Fastest to Slowest)
| Rank | Model | Latency | FPS |
|------|-------|---------|-----|
| 1 | PCA-KD | 95.58 ms | 10.46 |
| 2 | PCT-Net | 356 ms | 2.81 |
| 3 | U²-Net | ~600 ms | ~1.67 |
| 4 | LYT-Net | 1,006.31 ms | 0.99 |
| 5 | LaMa | 2,437 ms | 0.41 |
| 6 | CodeFormer | 4,761 ms | 0.21 |
| 7 | NAFNet | 8,975.93 ms | 0.11 |
| 8 | SAM | 9,009 ms | 0.11 |

---

## Key Takeaways

- **Smallest Models:** LYT-Net (44.92K) and PCA-KD (72.84K) - ~1000x smaller than large models
- **Fastest CPU:** PCA-KD at 15.23 FPS @256×256 - 10x faster than next best
- **Only Fixed Resolution:** CodeFormer (512×512 only)
- **Foundation Model:** SAM - trained on 1.1B masks, zero-shot capability
- **Best for Real-time:** PCA-KD and PCT-Net viable for interactive use
- **Newest Addition:** PCT-Net (CVPR 2023) - image harmonization for composites



# Inference Guide
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
| PCT-Net | Image Harmonization | `sameer513/pct-net-final` |
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
Below is the **PCT-Net** section formatted exactly in the same style as your LaMa block.
You can paste this directly into your README.

---

## 7. PCT-Net (Image Harmonization)

**Task:** Harmonize a composite image so foreground matches the background lighting & color.

### Pull & Run

```bash
docker pull sameer513/pct-net-final
docker run -it --name pctnet sameer513/pct-net-final
```

### Copy Input Files (from another terminal)

```bash
docker cp <YOUR_COMPOSITE_IMAGE> pctnet:/workspace/examples/composites/
docker cp <YOUR_MASK_IMAGE> pctnet:/workspace/examples/composites/
```

### Run Inference (inside container)

```bash
python3 run_inference.py \
  --image /workspace/examples/composites/<YOUR_IMAGE_NAME> \
  --mask /workspace/examples/composites/<YOUR_MASK_NAME> \
  --weights pretrained_models/PCTNet_ViT.pth \
  --model_type ViT_pct \
  --out /workspace/examples/composites/output.jpg
```

### Copy Output (from another terminal)

```bash
docker cp pctnet:/workspace/examples/composites/output.jpg <YOUR_LOCAL_OUTPUT_PATH>/
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
| PCT-Net | 512×512 | ~0.4 seconds |

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
| PCT-Net | [https://github.com/rakutentech/PCT-Net-Image-Harmonization](https://github.com/rakutentech/PCT-Net-Image-Harmonization) |

---

## Models & Citations

---

### NAFNet — Image Restoration

**Paper:** *Simple Baselines for Image Restoration*

```bibtex
@article{chen2022simple,
  title={Simple Baselines for Image Restoration},
  author={Chen, Liangyu and Chu, Xiaojie and Zhang, Xiangyu and Sun, Jian},
  journal={arXiv preprint arXiv:2204.04676},
  year={2022}
}
```

---

### LYT-Net — Low-Light Enhancement

**IEEE Signal Processing Letters (2025):**

```bibtex
@article{brateanu2025lyt,
  author={Brateanu, Alexandru and Balmez, Raul and Avram, Adrian and Orhei, Ciprian and Ancuti, Cosmin},
  journal={IEEE Signal Processing Letters},
  title={LYT-NET: Lightweight YUV Transformer-based Network for Low-light Image Enhancement},
  year={2025},
  volume={},
  number={},
  pages={1-5},
  doi={10.1109/LSP.2025.3563125}
}
```

**arXiv Preprint (2024):**

```bibtex
@article{brateanu2024lyt,
  title={LYT-Net: Lightweight YUV Transformer-based Network for Low-Light Image Enhancement},
  author={Brateanu, Alexandru and Balmez, Raul and Avram, Adrian and Orhei, Ciprian and Cosmin, Ancuti},
  journal={arXiv preprint arXiv:2401.15204},
  year={2024}
}
```

---

### U2Net — Salient Object Detection

```bibtex
@InProceedings{Qin_2020_PR,
  title = {U2-Net: Going Deeper with Nested U-Structure for Salient Object Detection},
  author = {Qin, Xuebin and Zhang, Zichen and Huang, Chenyang and Dehghan, Masood and Zaiane, Osmar and Jagersand, Martin},
  journal = {Pattern Recognition},
  volume = {106},
  pages = {107404},
  year = {2020}
}
```

---

### CodeFormer — Blind Face Restoration

```bibtex
@inproceedings{zhou2022codeformer,
  author = {Zhou, Shangchen and Chan, Kelvin C.K. and Li, Chongyi and Loy, Chen Change},
  title = {Towards Robust Blind Face Restoration with Codebook Lookup TransFormer},
  booktitle = {NeurIPS},
  year = {2022}
}
```

BasicSR toolbox:

```bibtex
@misc{basicsr,
  author = {Xintao Wang and Liangbin Xie and Ke Yu and Kelvin C.K. Chan and Chen Change Loy and Chao Dong},
  title = {{BasicSR}: Open Source Image and Video Restoration Toolbox},
  howpublished = {\url{https://github.com/XPixelGroup/BasicSR}},
  year = {2022}
}
```

---

### LaMa — Masked Image Inpainting

```bibtex
@article{suvorov2021resolution,
  title={Resolution-robust Large Mask Inpainting with Fourier Convolutions},
  author={Suvorov, Roman and Logacheva, Elizaveta and Mashikhin, Anton and Remizova, Anastasia and Ashukha, Arsenii and Silvestrov, Aleksei and Kong, Naejin and Goka, Harshith and Park, Kiwoong and Lempitsky, Victor},
  journal={arXiv preprint arXiv:2109.07161},
  year={2021}
}
```

---

### SAM — Segment Anything Model

```bibtex
@article{kirillov2023segany,
  title={Segment Anything},
  author={Kirillov, Alexander and Mintun, Eric and Ravi, Nikhila and Mao, Hanzi and Rolland, Chloe and Gustafson, Laura and Xiao, Tete and Whitehead, Spencer and Berg, Alexander C. and Lo, Wan-Yen and Doll{\'a}r, Piotr and Girshick, Ross},
  journal={arXiv:2304.02643},
  year={2023}
}
```

---

### PhotoWCT2 (PCA-Based) — Photorealistic Style Transfer

```bibtex
@InProceedings{Chiu_2022_CVPR,
  author    = {Chiu, Tai-Yin and Gurari, Danna},
  title     = {PCA-Based Knowledge Distillation Towards Lightweight and Content-Style Balanced Photorealistic Style Transfer Models},
  booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
  month     = {June},
  year      = {2022},
  pages     = {7844-7853}
}
```

### PCT-Net Harmonization Model

```bibtex
@InProceedings{Guerreiro_2023_CVPR,
    author    = {Guerreiro, Julian Jorge Andrade and Nakazawa, Mitsuru and Stenger, Bj\"orn},
    title     = {PCT-Net: Full Resolution Image Harmonization Using Pixel-Wise Color Transformations},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    month     = {June},
    year      = {2023},
    pages     = {5917-5926}
}
```

