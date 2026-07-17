# CanSat: AI-Eye

An onboard AI vision pipeline for a CanSat payload, reconstructing 3D terrain models from
camera imagery captured during descent.

## Notebooks

- `generate_images.ipynb` &mdash; generates synthetic multi-view training/test imagery using
  [Zero123++](https://github.com/SUDO-AI-3D/zero123plus) (a pretrained single-image-to-multi-view
  diffusion model, loaded via Hugging Face) from a single reference photo.
- `generate_3d.ipynb` &mdash; the core depth-estimation and point-cloud pipelines: single-frame
  [MiDaS](https://github.com/isl-org/MiDaS) depth, multi-frame reconstruction using logged IMU
  pose, stereo disparity (SGBM), ICP-based point cloud merging, and a combined stereo+MiDaS
  fusion approach. See the notebook's section headers for each.
- `fuse_point_clouds.ipynb` &mdash; fuses point clouds across frames using
  [LoFTR](https://github.com/zju3dv/LoFTR) feature matching to estimate inter-frame camera
  motion, then refines alignment with ICP registration.
- `stereo_disparity.ipynb` &mdash; a standalone calibrated stereo-pair disparity-to-point-cloud
  pipeline, as a simpler alternative to the MiDaS-based approaches above.

Raw test imagery, intermediate point-cloud outputs, and large 3D model files aren't included here
(dozens of multi-megabyte `.ply` files) &mdash; the notebooks regenerate them from source images.

## Usage

```bash
pip install torch diffusers accelerate huggingface_hub open3d opencv-python numpy pillow requests kornia
jupyter notebook generate_3d.ipynb
```
