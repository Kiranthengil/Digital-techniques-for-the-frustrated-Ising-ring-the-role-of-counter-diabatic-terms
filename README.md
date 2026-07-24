# Manuscript Figure Data and Plotting Code

This archive contains the numerical data and Jupyter notebook used to generate the figures associated with the manuscript.

The plotting notebook reads the supplied plain-text `.dat` files and saves all generated figures as PDF files in a single `Final_Results` directory.

## Contents

```text
project_folder/
├── Manuscript_Figure_Plots.ipynb
├── README.md
├── requirements.txt
├── Data/
│   ├── System_Dynamics_And_Methods/
│   │   ├── Fig2a/
│   │   └── Fig2b/
│   └── Results/
│       ├── Fig3/
│       ├── Fig4/
│       ├── Fig5/
│       ├── Fig10/
│       └── Fig11/
└── Final_Results/
```

`Final_Results` is created automatically when the notebook setup cell is executed.

The internal directory structure and filenames inside `Data` must be preserved because the plotting functions locate the source files using relative paths.

## Software requirements

A Python environment with the packages listed in `requirements.txt` is required.

The notebook uses Matplotlib's LaTeX text rendering:

```python
"text.usetex": True
```

A working LaTeX installation containing the `amsmath` package must therefore also be available on the system. For example, MiKTeX or TeX Live may be used.

## Installation

### 1. Create a virtual environment

Windows PowerShell:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

Windows Command Prompt:

```bat
python -m venv .venv
.venv\Scripts\activate.bat
```

Linux or macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Install the Python dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 3. Start Jupyter

```bash
jupyter lab Manuscript_Figure_Plots.ipynb
```

The classic notebook interface can also be used:

```bash
jupyter notebook Manuscript_Figure_Plots.ipynb
```

## Working directory

The notebook defines the project directory using:

```python
PROJECT_ROOT = Path.cwd()
```

Launch Jupyter from the directory containing both `Manuscript_Figure_Plots.ipynb` and the `Data` folder. The following directory should therefore exist relative to the notebook session:

```text
Data/
```

All generated PDFs are written to:

```text
Final_Results/
```

## Generating the figures

Open `Manuscript_Figure_Plots.ipynb` and run all cells once. The plotting commands in the final **Plot control** cell are commented out, so running the full notebook initializes the readers and plotting functions without automatically generating every figure.

To generate a figure, uncomment or execute the corresponding command.

### Figure 2a: instantaneous energy gap

```python
plot_fig2a(N=17);
```

Output:

```text
Final_Results/EnergyGapVsSchedule_N_17.pdf
```

### Figure 2b: minimum energy gap versus system size

```python
plot_fig2b();
```

Output:

```text
Final_Results/MinEnergyGapVsN.pdf
```

### Figure 3: residual energy for fixed schedules

```python
plot_fig3(N=17);
```

Output:

```text
Final_Results/ResidualEnergy_with_fixed_schedule_N_17.pdf
```

### Figure 4: controllability and residual-energy distributions

```python
plot_fig4(N=17);
```

Output:

```text
Final_Results/Controllability_N_17_Jf_compare.pdf
```

### Figure 5: residual energy for all methods

```python
plot_fig5(N=17, Jf=0.45);
```

Output:

```text
Final_Results/Eres_All_Methods_N_17_Jf_0.45.pdf
```

### Figure 6: optimized schedules

```python
plot_fig6(N=17, Jf=0.45, p_index=8, trial=4);
```

Output:

```text
Final_Results/Schedule_sp_All_Methods_N_17_Jf_0.45.pdf
```

### Figure 7: time-step profiles

```python
plot_fig7(N=17, Jf=0.45, p_index=8);
```

Output:

```text
Final_Results/TxTz_Profile_All_Methods_N_17_Jf_0.45_Pindex_8.pdf
```

### Figure 8: counterdiabatic parameters

```python
plot_fig8(N=17, Jf=0.45, p_index=8, trial=4);
```

Output:

```text
Final_Results/Schedule_CD_All_Methods_N_17_Jf_0.45.pdf
```

### Figure 9: DC-QAOA log-gain

```python
plot_fig9(Jf=0.45);
```

Output:

```text
Final_Results/Log_Improvement_Mean_Std_DC_QAOA_vs_QAOA_MultipleNG_Jf_0.45.pdf
```

### Figure 10: ground-state fidelity

```python
plot_fig10(N=35, Jf=0.45, P_index=-2, log_scale=False);
```

Output:

```text
Final_Results/Fidelity_All_Methods_N_35_Jf_0.45.pdf
```

For a logarithmic fidelity axis, use:

```python
plot_fig10(N=35, Jf=0.45, P_index=-2, log_scale=True);
```

### Figure 11: random final-coupling values

```python
plot_fig11(N=17);
```

Output:

```text
Final_Results/Eres_Diff_Jf_N_17.pdf
```

The semicolon after a plotting command suppresses the tuple returned by the function in Jupyter. It does not affect the displayed or saved figure.

## Figures 5–8 data cache

Figures 5–8 share the same source data and use an in-memory cache to avoid repeatedly reading a large collection of `.dat` files.

Under normal use, no additional action is required. If a source file inside `Data/Results/Fig5` is edited or replaced while the Jupyter kernel remains active, clear the cached arrays before regenerating Figures 5–8:

```python
clear_fig5_cache()
```

Then run the required plotting command again. Alternatively, use the `refresh=True` option:

```python
plot_fig5(N=17, Jf=0.45, refresh=True);
```

Restarting the Jupyter kernel also clears the cache.

## Parameters

The plotting functions provide defaults matching the supplied data. Parameters such as `N`, `Jf`, `P_index`, `p_index`, `trial`, `n_p`, `n_reals`, and `n_trials` should only be changed when corresponding data files are present in the expected directory structure.

When an expected input file is absent, the notebook raises a `FileNotFoundError` showing the full path it attempted to read.

## Reproducibility check

Before archiving or publishing a new version:

1. Copy the complete project folder to a clean location.
2. Create a fresh Python environment.
3. Install `requirements.txt`.
4. Confirm that a working LaTeX installation is available.
5. Launch Jupyter from the project folder.
6. Restart the kernel and run all notebook cells.
7. Generate the required figures from the final Plot control cell.
8. Confirm that all PDFs appear in `Final_Results`.

## Data format

The numerical inputs are stored as plain-text `.dat` files. Their formats differ between figures and are parsed by the dedicated reader functions in the notebook. The files should not be renamed or rearranged unless the corresponding path and reader logic are updated.

The Figure 10 fidelity file contains named columns for the normalized layer coordinate and fidelity of each method. Shorter curves may contain `NaN` padding, which is removed automatically when the file is loaded.

## Citation

When using this archive, cite both the associated manuscript and the Zenodo record.

Associated manuscript:

> [Authors], "[Manuscript title]," [Journal or arXiv identifier], [Year].

Zenodo record:

> Thengil, K., Arezzo, V. R.& Santoro, G. E. (2026). Data and plotting code for Digital techniques for the frustrated Ising ring: the role of counter-diabatic terms (Version v1.0.0) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.21525901


## License

The source code and Jupyter notebooks are licensed under the MIT License.
See `LICENSE-CODE`.

The numerical data in `Data/` and the author-generated figures in
`Final_Results/` are licensed under the Creative Commons Attribution
4.0 International License. See `LICENSE-DATA.md`.
