# Shadow Segmentation and Removal

This repository combines two research codebases into a single workspace for shadow understanding:

- `SAM-Adapter-PyTorch/` for shadow mask prediction
- `ShadowFormer/` for shadow removal from images using the predicted masks

The project is best understood as an experiment and integration workspace rather than a clean upstream mirror. It keeps the configs, helper scripts, notebooks, logs, and sample assets that were used while building the pipeline.

## What is in this repository

### `SAM-Adapter-PyTorch/`
This is the larger of the two inherited codebases and contains most of the runnable training and evaluation logic.

- `configs/` and `istd/config.yaml`: model and dataset configuration files
- `datasets/`: dataset loaders and wrappers
- `models/`: SAM-Adapter model definitions and segmentation components
- `test.py` and `parser.py`: evaluation-style entry points that generate shadow masks
- `resizing.py`, `imager.py`, `dataset_reorganization.py`: utility scripts for image preparation and dataset reshaping
- `save/`: saved experiment outputs and TensorBoard logs from earlier runs

### `ShadowFormer/`
This folder contains the shadow removal stage and related reference material.

- `dataset.py`: dataset loader for paired shadow / mask / shadow-free images
- `inferrer.py`: inference entry point for producing restored images
- `imager.py`: image resizing helper
- `evaluation/measure_shadow.m`: MATLAB evaluation script
- `doc/`: architecture and result figures from the original paper

## Intended workflow

1. Prepare or resize shadow images using the helper scripts in `SAM-Adapter-PyTorch/` or `ShadowFormer/`.
2. Update dataset and checkpoint paths in `SAM-Adapter-PyTorch/istd/config.yaml`.
3. Run `SAM-Adapter-PyTorch/test.py` or `SAM-Adapter-PyTorch/parser.py` to produce shadow masks.
4. Arrange images, masks, and ground-truth targets into the dataset structure expected by the removal stage:

```text
dataset_root/
  train/
    train_A/   shadow images
    train_B/   shadow masks
    train_C/   shadow-free targets
  test/
    test_A/    shadow images
    test_B/    shadow masks
    test_C/    shadow-free targets
```

5. Run `ShadowFormer/inferrer.py` on the prepared dataset to generate shadow-removed outputs.

## Notes before running

- Several scripts still contain environment-specific paths such as `/teamspace/studios/this_studio/...`. Update those paths before running locally.
- `SAM-Adapter-PyTorch/requirements.txt` is the main dependency list currently present in this snapshot.
- The repository includes experiment artifacts such as `__pycache__/`, TensorBoard event files, logs, and sample images.
- `ShadowFormer/` is a partial project snapshot in this repository. It is useful for understanding the removal stage and running the included helper scripts, but it may need additional cleanup or missing upstream files for a full training setup.
- There is no single top-level runner yet; scripts are executed from within the inherited project folders.

## Project origin

This workspace is built around two upstream research projects:

- **SAM-Adapter**: "SAM Fails to Segment Anything? SAM-Adapter: Adapting SAM in Underperformed Scenes"
- **ShadowFormer**: "ShadowFormer: Global Context Helps Image Shadow Removal"

The nested READMEs from those original folders were removed so this root document can serve as the single project-level reference for the combined repository.
