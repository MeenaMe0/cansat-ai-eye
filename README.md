# CanSat: AI-Eye

An onboard AI vision pipeline for a CanSat payload, reconstructing 3D terrain models from
camera imagery captured during descent, plus the rocket flight simulations used to design
the launch vehicle.

## AI-Eye: depth estimation & 3D reconstruction

- `generate_images.ipynb` &mdash; generates synthetic multi-view training/test imagery using
  [Zero123++](https://github.com/SUDO-AI-3D/zero123plus) (a pretrained single-image-to-multi-view
  diffusion model, loaded via Hugging Face) from a single reference photo.
- `generate_3d.ipynb` &mdash; runs [MiDaS](https://github.com/isl-org/MiDaS) monocular depth
  estimation on captured images, then uses Open3D to convert the resulting depth maps into 3D
  point clouds.
- `combined_model_by_gemini.ipynb` &mdash; fuses point clouds from multiple camera angles/frames
  into a single combined 3D terrain model.
- `test_stero_chat.ipynb` &mdash; stereo-camera disparity experiments as an alternative/complementary
  depth-sensing approach to monocular MiDaS estimation.

Raw test imagery, intermediate point-cloud outputs, and large 3D model files aren't included here
(dozens of multi-megabyte `.ply` files) &mdash; the notebooks regenerate them from source images.

## Rocket flight simulation

[`rocket-flight-sim/`](rocket-flight-sim) contains multi-tool launch simulations for the CanSat's
launch vehicle ("CURSR IV C5"), cross-checked across three tools:

- **OpenRocket** (`.ork`) &mdash; general flight data: velocity, altitude, Mach number, stability.
- **RASAero II** (`.CDX1`) &mdash; aerodynamic simulation, paired with the included motor file (`.eng`).
- **RocketPy** (`.ipynb`) &mdash; Python-based simulation with custom CFD-derived drag data (`C4_Mach_Cd_CFD.csv`).

See `rocket-flight-sim/README.txt` for tool-specific usage notes.

## Usage

```bash
pip install torch diffusers accelerate open3d opencv-python numpy pillow requests
jupyter notebook generate_3d.ipynb
```
