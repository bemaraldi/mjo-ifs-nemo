# mjo-ifs-nemo
Analysis code for the MJO evaluation in IFS-NEMO (W&amp;CD)

# Vertical structure and MJO propagation in IFS-NEMO

This repository contains the analysis notebooks for the first paper of the
IFS-NEMO MJO evaluation. The notebooks reproduce the mean-state, variability,
wavenumber-frequency, lag-longitude and moist-static-energy diagnostics used in
the manuscript.

## Repository layout

```text
.
├── WHKI09_spectrum_1950-1984.ipynb   # Wheeler-Kiladis spectra of OLR/RLUT
├── lag-lon_SENSITIVITY.ipynb         # Lag-longitude propagation and sensitivity tests
├── process-MSE-mn5.ipynb             # Vertically integrated moist static energy
├── scripts/
│   └── wavenumber_frequency_functions.py
├── environment.yml
└── README.md
```

The notebooks include the figures from the original analysis. Run them in order to
regenerate every figure and diagnostic.

## Data availability

The input data are not distributed with this repository. The notebooks read the
data from four configurable locations, set with environment variables:

| Variable | Default | Contents |
|---|---|---|
| `MJO_DATA_ROOT` | `data` | Processed data and intermediate files |
| `MJO_ARCHIVE_ROOT` | `archive` | Raw IFS-NEMO and ERA5 archive |
| `MJO_FIGURE_ROOT` | `figures` | Output figures |
| `MJO_SCRIPTS_ROOT` | `scripts` | Helper modules |

The expected directory tree is:

```text
$MJO_DATA_ROOT/
├── OLR/
├── IFS-NEMO/
│   ├── a282-hist1950/{pr,rlut,ua,sst}/
│   └── a2de-hist1950/{pr,rlut,ua,sst}/
├── era5/
├── clean_data/
└── WC/

$MJO_ARCHIVE_ROOT/
├── a2de/
├── a282/
├── era5-MSE/
├── era5-postprocessed/

```

The exact file names are given in the loading cell of each notebook.

To use non-default locations:

```bash
export MJO_DATA_ROOT=/path/to/processed/data
export MJO_ARCHIVE_ROOT=/path/to/archive
export MJO_FIGURE_ROOT=/path/to/figures
export MJO_SCRIPTS_ROOT=/path/to/repository/scripts
```

## Environment

The analysis uses Python and the scientific Python stack. Create the environment
with conda (or mamba):

```bash
conda env create -f environment.yml
conda activate mjo-paper1
jupyter lab
```

Then open a notebook and run all cells. `scripts/` is added to `sys.path` by the
configuration cell, so the vendored `wavenumber_frequency_functions.py` module is
imported without any further setup.

## Notebooks

| Notebook | Description |
|---|---|
| `WHKI09_spectrum_1950-1984.ipynb` | Computes symmetric and antisymmetric wavenumber-frequency spectra of OLR/RLUT and compares ERA5/NOAA with IFS-NEMO-HR and IFS-NEMO-ER. |
| `lag-lon_SENSITIVITY.ipynb` | Estimates MJO propagation speed from lag-longitude correlations and runs the sensitivity tests . |
| `process-MSE-mn5.ipynb` | Computes vertically integrated moist static energy, its mean state and Nov-Apr variability, and the charged-MSE diagnostic. |


## Reproducibility notes

- Each notebook starts with a configuration cell that defines the four path
  variables above and creates the required figure directories.
- Stored outputs are limited to the figures from the original analysis; printed
  diagnostics, warnings and other text outputs were removed during cleaning.
- The notebooks are independent, except that `hus_WC.ipynb` documents the
  creation of the repaired ERA5 humidity files used elsewhere.
- Intermediate files written by `process-MSE-mn5.ipynb` go to
  `$MJO_DATA_ROOT/clean_data/MSE/`.
- Figure file names and the sections that produce them are documented at the end
  of the MSE notebook and in the saving cells of the other notebooks.

