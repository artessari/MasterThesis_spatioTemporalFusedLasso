# MasterThesis_spatioTemporalFusedLasso

## Project Structure

Here is an overview of the key files and directories in this repository:

```         
.
├── _freeze/                                                                    # Cached computational results for Quarto documents
├── PDF-Documentation/                                                          # Final rendered documentation in PDF format
├── renv/                                                                       # Local R library (package management environment)
├── .gitignore                                                                  # List of files and folders ignored by Git
├── .Rprofile                                                                   # Automatically activates the renv environment upon startup
├── _quarto.yml                                                                 # Project-wide Quarto configuration file
├── 00_How-to-Store-NASA-Credentials-as-Environment-Variables-in-R-Rproj.qmd    # Quarto source file: Guide on securely storing NASA credentials
├── BlackMarbleR_Usage.Rmd                                                      # R Markdown script for testing the blackmarbler package
├── fusedlasso_1D_2D.qmd                                                        # Quarto source file: Analysis using 1D and 2D fused lasso on spatio-temporal data
├── README.md                                                                   # Project documentation and setup guide
├── renv.lock                                                                   # Snapshot of package dependencies
└── MasterThesis_spatioTemporalFusedLasso.Rproj                                 # RStudio project configuration file
```

### Key Files

- **`00_How-to-Store-NASA-Credentials-as-Environment-Variables-in-R-Rproj.qmd`**: Contains a step-by-step technical guide on how to configure `.Renviron` locally to securely access NASA data without hardcoding authentication parameters.

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

```r
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

```bash
quarto render filename.qmd
```

To process the entire project and update all reports simultaneously, the general command without specifying a file name can be used:

```bash
quarto render
```

### Output Formats

Under the current configuration, rendering defaults to **PDF format**. To provide a static, print-optimised version, this process generates a file (e.g., `filename.pdf`) containing high-quality formatted text, code sections, and visualisations.

*Note: Exporting to PDF requires an installed TeX distribution (such as MiKTeX) on the system.*

### Alternative Formats

Quarto supports a wide variety of other output formats (e.g., HTML, MS Word, or ePub). To generate an interactive version for a web browser, the target format can be specified during rendering using the `--to` parameter:

```bash
quarto render filename.qmd --to html
```

Other formats can be generated using analogous commands to adapt the report to different requirements.

