# Traffic Synth — Automated Synthetic Traffic Dataset Generation via Diffusion-Based Inpainting

## Overview

Traffic Synth is an automated pipeline for generating synthetic vehicle detection training datasets by modifying real traffic camera images using diffusion-based inpainting. The system preserves authentic background context while generating new vehicle instances with controlled appearance, orientation, and class distribution.

### Key Features

- **Background preservation** — original road geometry, lighting, and camera perspective remain bitwise identical through mask-based compositing
- **Lane-aware directional prompting** — vehicle orientation matches traffic flow direction relative to the camera
- **Region-specific vehicle database** — weighted brand sampling reflecting real-world fleet composition (configurable per deployment region)
- **Brand-free generation mode** — optional mode that omits brand names from prompts for maximum visual diversity
- **SAM2 label refinement** — post-generation bounding box correction using Segment Anything Model 2
- **FID evaluation** — built-in Fréchet Inception Distance computation for quantitative quality assessment
- **Automatic YOLO annotations** — standard format labels generated for each synthetic vehicle

### Pipeline Architecture

```
Real Traffic Frame → YOLOv8x-seg (instance segmentation)
    → Mask Processing (scaling, dilation)
    → FLUX.1 Fill (diffusion-based inpainting)
    → SAM2 (label refinement)
    → Composited Image + YOLO Labels
```

### Technical Specifications

| Component | Specification |
|-----------|---------------|
| Diffusion Model | FLUX.1 Fill (23.8 GB, full precision) |
| Segmentation | YOLOv8x-seg |
| Label Refinement | SAM2 (Segment Anything Model 2) |
| Resolution | 1920 × 1088 pixels |
| Per-vehicle time | ~48 seconds (NVIDIA RTX 4090) |
| Vehicle database | 21 car + 8 van + 7 truck + 6 bus brands |

### Generation Quality

Overall FID: **28.60** (2,392 vehicle crop pairs)

| Class | FID | Samples |
|-------|-----|---------|
| Truck | 36.00 | 1,131 |
| Car | 40.74 | 883 |
| Van | 78.65 | 355 |
| Bus | 130.43 | 23 |

### Sampling Strategies

```bash
python main.py --sampling weighted --count 100    # Regional brand weights (default)
python main.py --sampling brand-free --count 100   # No brand names in prompts
```

## Associated Publication

**Automated Synthetic Traffic Dataset Generation via Diffusion-Based Inpainting Pipeline**

* Ing.Daniel Gachulinec, Viktória Cvacho, Maroš Jakubec, Radovan Madleňák*

University of Žilina, Faculty of Operation and Economics of Transport and Communications

Funded by: APVV-24-0282 — Synthetic Traffic Data for Improving the Resilience of Transport Systems

## Access to Source Code

This repository provides a project overview and documentation. The full source code is maintained in a private repository for ongoing research purposes.

**To request access to the complete source code**, please:

1. Open an [Issue](https://github.com/gachulinec/traffic_synth/issues/new) in this repository with the subject "Code Access Request"
2. Include your name, institutional affiliation, and intended use case
3. We will respond within 5 working days

Alternatively, contact the corresponding author directly:
- **Daniel Gachulinec** — daniel.gachulinec@stud.uniza.sk

## License

This project is part of ongoing academic research. Access to the source code is granted for research and educational purposes upon request.

## Citation

If you use Traffic Synth in your research, please cite:

```bibtex
@article{gachulinec2026trafficsynth,
  title={Automated Synthetic Traffic Dataset Generation via Diffusion-Based Inpainting Pipeline},
  author={Gachulinec, Daniel and Cvacho, Vikt{\'o}ria and Jakubec, Maro{\v{s}} and M{\'o}ro, R{\'o}bert},
  year={2026}
}
```
