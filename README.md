# TMEM175 Inclusion Analysis - Day 1

Notebook-driven analysis for TMEM175 day 1 inclusion imaging. This was amongst my first image analysis code -> proceed with caution lol

**Notebook:** [inclusionanalysis_TMEM175_day1.ipynb](inclusionanalysis_TMEM175_day1.ipynb)

## What This Does

This notebook loads .czi images, segments cells from the green channel with Cellpose, detects yellow inclusions in HSV space, and counts inclusions inside each cell.

The current notebook is set up as an analysis script: it shows QC plots and prints the per-image inclusion counts.

## Quickstart

This repository only includes the notebook. Start from a fresh clone, create your own input and output folders for this run, then run the notebook.

1. Open a terminal in the `TMEM175_inclusion_analysis` folder.
2. Create separate folders for this run's images and outputs:

   ```bash
   mkdir -p "data/(data folder name)"
   mkdir -p "Results/(results folder name)"
   ```

3. Copy your .czi files into `data/(data folder name)/`.
4. Install the Python dependencies if needed:

   ```bash
   pip install czifile cellpose opencv-python numpy pandas matplotlib scikit-image scipy
   ```

5. Open [inclusionanalysis_TMEM175_day1.ipynb](inclusionanalysis_TMEM175_day1.ipynb) and set `IMAGE_FOLDER` to `data/090425_TMEM175_INCLUSIONS_day 1`.
6. If you save outputs from the notebook or extend the commented batch block, point `RESULTS_FOLDER` at `Results/090425_TMEM175_INCLUSIONS_day 1`.
7. Run the notebook top to bottom.
8. Review the QC plots and the printed table of inclusion counts.

## Requirements

- Python 3.10 or newer
- `czifile`
- `cellpose`
- `opencv-python`
- `numpy`
- `pandas`
- `matplotlib`
- `scikit-image`
- `scipy`

GPU support is disabled by default in the notebook. If you want to try GPU Cellpose, change `USE_GPU` in the setup cell.

## Inputs

- Multi-channel `.czi` files in `data/090425_TMEM175_INCLUSIONS_day 1/`
- A green channel for Cellpose cell segmentation
- RGB or color-converted data for yellow inclusion detection

## Outputs

- Inline QC figures for each image
- A printed summary of inclusion counts per image
- A `results` list in memory containing filename and inclusion count
- Any saved files should go in `Results/090425_TMEM175_INCLUSIONS_day 1/` so each run stays separate

## Workflow

1. Normalize and contrast-adjust the green channel.
2. Segment cells with Cellpose, expanding the search if needed.
3. Detect yellow inclusions with an HSV threshold.
4. Keep only inclusion pixels that overlap a cell mask.
5. Count inclusions per cell and display the QC figure.

## Main Parameters

The main knobs are near the top of the notebook and in the inclusion helper functions:

- `USE_GPU`: toggle Cellpose GPU usage.
- `diameter` in `segment_cells()`: search range for Cellpose cell size.
- `CONTRAST_THRESHOLD` and `MIN_BRIGHTNESS`: yellow inclusion sensitivity.
- `spot_size_limit`: size scale for the bright-spot top-hat filter.

## Tips

- If Cellpose misses cells, increase the diameter sweep or adjust the preprocessing.
- If the yellow mask is too noisy, raise `CONTRAST_THRESHOLD` or `MIN_BRIGHTNESS`.
- If true inclusions are being removed, lower the bright-spot size and intensity thresholds a bit.

## Troubleshooting

- If the notebook cannot find images, confirm the folder name matches `090425_TMEM175_INCLUSIONS_day 1` exactly.
- If you moved the images into a custom directory, update `IMAGE_FOLDER` to match that path.
- If you save CSVs or figures, make sure `RESULTS_FOLDER` points at the matching per-run output folder.
- If `.czi` loading fails, make sure `czifile` is installed and the files are readable.
- If the notebook errors on Cellpose, set `USE_GPU = False` and rerun the setup cell.

## Notes

This notebook is intended for the Kara Lab TMEM175 day 1 inclusion workflow and can be extended into a batch exporter by adapting the commented processing block near the end.
