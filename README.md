# MasterThesis_spatioTemporalFusedLasso

## Project Structure

Here is an overview of the key files and directories in this repository:

```         
.
├── PDF-Documentation/                                                          # Final rendered documentation in PDF format
├── data/                                                                       # Input data and study area
│   ├── blackmarble_rasters/                                                    # Monthly nighttime lights stored as SpatRaster (.rds)
│   ├── blackmarble_tif/                                                        # GeoTIFF versions of monthly nighttime lights
│   ├── blackmarble_quarterly_tif/                                              # Quarterly aggregated GeoTIFF outputs
│   └── gadm/                                                                   # Administrative boundaries (GADM, Uganda)
│   │   └── gadm41_UGA_1_pk.rds 
│   └── kampala/                                                                # Processed quarterly GeoTIFF rasters for Kampala
├── output_fits/                                                                # Serialized model outputs and fitted objects (.rds)
├── output_plots/                                                               # Generated graphics, comparative charts, and visualizations
├── renv/                                                                       # Local R library (package management environment)
├── .Rprofile                                                                   # Automatically activates the renv environment upon startup
├── .gitignore                                                                  # Specifies files and folders ignored by Git
├── .renvignore                                                                 # Prevents renv from scanning specific folders to improve performance
├── 00_How-to-Store-NASA-Credentials-as-Environment-Variables-in-R-Rproj.qmd    # Quarto source file: Guide on securely storing NASA credentials
├── 01_BlackMarbleR_Usage-DataDownload.qmd                                      # Quarto source file: Data download and preprocessing of Black Marble nighttime lights (script for testing the blackmarbler package)
├── 02_BlackMarbleR_Preprocessing.qmd                                           # Quarto source file: Monthly to quarterly raster aggregation, spatial missing data imputation via focal filter, and ROI (Kampala) bounding box extraction
├── 03_fusedlasso_2D.qmd                                                        # Quarto source file: Spatial change detection using 2D fused lasso on image grids
├── 04_fusedlasso_1D.qmd                                                        # Quarto source file: Temporal change detection using 1D fused lasso on time-series data
├── 05_fusedlasso_3D.qmd                                                        # Quarto source file: Spatio-temporal change detection using 3D fused lasso on raster time-series data
├── MasterThesis_spatioTemporalFusedLasso.Rproj                                 # RStudio project configuration file
├── README.md                                                                   # Project documentation and setup guide
├── _quarto.yml                                                                 # Project-wide Quarto configuration file
└── renv.lock                                                                   # Snapshot of package dependencies
```

### Key Files

- **`data/`**: Stores the Uganda study area boundaries (`gadm41_UGA_1_pk.rds`) and monthly/quarterly aggregated Black Marble nighttime light datasets. Raster data are primarily processed and stored as stable GeoTIFF (`.tif`) files to ensure data permanence during analysis and modeling.
- **`output_fits/`**: Dynamic directory containing serialized statistical and spatial model objects (`.rds`) saved during execution. Storing results on disk prevents data loss and avoids repeating intensive computation pipelines.
- **`output_plots/`**: Main storage directory for high-resolution diagnostic charts, model performance evaluations, and spatial comparison grid visualizations generated throughout the analysis.
- **`00_How-to-Store-NASA-Credentials-as-Environment-Variables-in-R-Rproj.qmd`**: Contains a step-by-step technical guide on how to configure `.Renviron` locally to securely access NASA data without hardcoding authentication parameters.
- **`01_BlackMarbleR_Usage-DataDownload.qmd`**: Data download and preprocessing workflow for Black Marble nighttime lights. The script demonstrates how to retrieve monthly raster data using the `blackmarbler` package, process and save outputs as RDS (for analysis) and GeoTIFF (for visualization), and generate exploratory spatio-temporal summaries and maps.
- **`02_BlackMarbleR_Preprocessing.qmd`**: Processes monthly Black Marble rasters into quarterly aggregates (Q1–Q4) using pixel-wise means to minimize temporal noise. It then extracts a fixed bounding box for the Kampala Region of Interest (ROI), applies a log-transformation, and handles spatial data gaps by performing a local imputation using a $3 \times 3$ moving window (focal mean) that replaces only missing pixels while preserving original valid radiance signals.
- **`03_fusedlasso_2D.qmd` \| `04_fusedlasso_1D.qmd`**: Analysis using 1D and 2D fused lasso models for spatio-temporal change detection in nighttime lights data. The file implements spatial smoothing via a 2D fused lasso applied to Kampala raster data, temporal smoothing across monthly observations using a 1D fused lasso framework, and derives pixel-wise change detection metrics to identify structural dynamics and temporal shifts in urban night-time illumination patterns.
- **`05_fusedlasso_3D.qmd`**: Proposed 3D fused lasso framework and its application to spatio-temporal change detection on raster time-series datasets.

------------------------------------------------------------------------

## How to Run

### Clone the Repository

To copy the project from the GitHub repository to a local machine:

``` bash
# Clone the repository (Default: main branch)
git clone https://github.com/artessari/MasterThesis_spatioTemporalFusedLasso

# Navigate into the project directory
cd MasterThesis_spatioTemporalFusedLasso

# Optional: Clone a specific branch
git clone --branch <branch-name> --single-branch https://github.com/artessari/MasterThesis_spatioTemporalFusedLasso
```

------------------------------------------------------------------------

## Reproducible R Environment with renv

This project uses **renv** to ensure a consistent and reproducible working environment.

### Restoring the Environment

Since the project is pre-configured, the environment can be restored by running the following command in the R console:

``` r
renv::restore()
```

This command automatically installs all the package versions defined in the `renv.lock` file.

### Project Structure

The following files are critical for environment stability:

- **renv.lock**: Contains the exact list of all package versions.
- **.Rprofile**: Automatically activates renv when R starts up in this directory.
- **renv/activate.R**: The script responsible for loading the local environment.

### Updating the Environment

After installing new packages or changing package versions, the current state of the environment can be saved using:

``` r
renv::snapshot()
```

### Version Control Notes

- The **renv.lock** file must be versioned and tracked in Git.
- The **renv/library/** directory is ignored because it contains local package installations. These can be generated dynamically when needed via `renv::restore()`.

------------------------------------------------------------------------

## Executing Code in Quarto (Interactive Workflow)

In RStudio, code inside a Quarto document can be executed flexibly: either step-by-step **chunk by chunk** or entirely in a single run.

Using the **Run** button options in the editor dropdown:

- **Run Current Chunk**: Executes only the active code block.
- **Run Next Chunk**: Executes the following code block.
- **Run All Chunks Above**: Executes all preceding blocks (crucial when dealing with data dependencies).
- **Run All**: Executes the entire code sequentially (similar to a classic R script).

This allows for isolating and testing specific parts of the analysis, or running the complete pipeline at once to instantly inspect the total output without rendering the entire document.

------------------------------------------------------------------------

## Rendering the Document

To process the Quarto document and generate the final report, two methods are available:

**Option 1: RStudio (Graphical User Interface)**

In RStudio, the compilation can be triggered by clicking the **Render** button (the blue arrow icon located above the script editor).

**Option 2: Terminal (Command Line Interface)**

Alternatively, the document can be rendered directly via the terminal. Run the following command from the project root directory:

``` bash
quarto render filename.qmd
```

To process the entire project and update all reports simultaneously, the general command without specifying a file name can be used:

``` bash
quarto render
```

### Output Formats

Under the current configuration, rendering defaults to **PDF format**. To provide a static, print-optimised version, this process generates a file (e.g., `filename.pdf`) containing high-quality formatted text, code sections, and visualisations.

*Note: Exporting to PDF requires an installed TeX distribution (such as MiKTeX) on the system.*

### Alternative Formats

Quarto supports a wide variety of other output formats (e.g., HTML, MS Word, or ePub). To generate an interactive version for a web browser, the target format can be specified during rendering using the `--to` parameter:

``` bash
quarto render filename.qmd --to html
```

Other formats can be generated using analogous commands to adapt the report to different requirements.
