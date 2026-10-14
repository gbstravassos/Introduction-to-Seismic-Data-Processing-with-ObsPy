# Introduction to Seismic Data Processing with ObsPy

A progressive, eight-week learning project for inspecting, visualizing, and conditioning a small seismic section in SEG-Y format using Python.

The central question is: **How can we improve seismic data visualization and reduce selected noise components while checking for distortion or loss of useful signal?**

The project combines seismic processing concepts with practical Python development. Each weekly module includes technical study, reinforcement exercises, and a project task. The repository will be updated incrementally as these activities are completed.

## Project status

**Stage: planning and initial documentation.** This README describes the intended workflow and deliverables. The notebooks, utilities, datasets, and results listed below are planned additions; their presence or validation should not be inferred from this document.

## Objectives

- Read SEG-Y files and inspect their trace and file metadata.
- Understand ObsPy `Stream` and `Trace` objects.
- Check sample counts, sampling intervals, and recording-time references.
- Build trace and section plots with consistent comparison settings.
- Identify suspicious traces using basic quality-control measurements.
- Evaluate mean removal, detrending, and tapering separately.
- Analyze frequency content and compare candidate filters.
- Understand the effects of gain and automatic gain control (AGC).
- Validate selected processing choices against a synthetic signal with known ground truth.
- Produce a reproducible notebook and an evidence-based interpretation of the results.

## Scope and prerequisites

The initial dataset should be a small, preferably post-stack, seismic section with a manageable number of traces. The emphasis is on trace conditioning, quality control, and learning Python through a concrete geophysical application.

This introductory project does not implement a complete production processing sequence. Acquisition geometry reconstruction, velocity analysis, NMO, stacking, migration, deconvolution, and 4D matching are outside the core eight-week scope.

Basic familiarity with seismic traces and sampling is helpful. Python skills will be developed progressively: variables and indexing first, then arrays, loops, conditionals, functions, plotting, and file handling.

## Eight-week roadmap

Suggested workload: **4–6 hours per week**.

| Week | Module | Main activities | Project deliverable |
|---|---|---|---|
| 1 | Environment, SEG-Y reading, and metadata | Organize paths; inspect `Stream`, `Trace`, amplitudes, and headers; verify sample counts and sampling intervals; investigate the time reference. | Data inventory and initial compatibility checks. |
| 2 | Trace and section visualization | Build time axes; inspect individual traces; assemble compatible arrays; select representative traces and time windows; control plotting scales. | Reference figures and a documented comparison region. |
| 3 | Quality control and preprocessing | Measure mean, RMS, absolute peak, non-finite values, and zero-sample fraction; investigate anomalies; compare mean removal, linear detrending, and tapering. | Initial diagnosis and individual preprocessing comparisons. |
| 4 | Frequency analysis | Study sampling and Nyquist frequency; compute FFT amplitude spectra; examine window duration, weighting, and frequency resolution; compare representative windows. | Spectral diagnosis and candidate filter bands. |
| 5 | Frequency filtering | Compare low-pass, high-pass, or band-pass configurations; record cutoffs, filter settings, and phase treatment; inspect boundaries and removed content. | Comparison of three filter configurations and a justified selection. |
| 6 | Gain and AGC | Compare constant gain, trace normalization, and moving-RMS AGC; vary the AGC window; inspect noise amplification and amplitude changes. | Gain comparisons and a separate visualization-oriented data version. |
| 7 | Synthetic validation | Generate reflectivity; convolve with a wavelet; add controlled noise with a fixed random seed; apply selected filters; evaluate RMS error and correlation. | Ground-truth experiment showing both benefits and limitations. |
| 8 | Integration and reporting | Consolidate the workflow; document parameters; compare original and processed data; save results; restart and execute the notebook from beginning to end. | Reproducible final notebook, selected figures, and a short report. |

### Suggested weekly study cycle

1. Read the technical instructions and work through short examples.
2. Predict the output of small code snippets before executing them.
3. Complete reinforcement exercises and explain the results in Markdown cells.
4. Apply the module to the project dataset.
5. Record decisions, limitations, and questions for the next module.

Implement each operation in short, readable cells before turning it into a reusable function. Progress depends on being able to explain what the code does, what changes in the data, and how that change was evaluated.

## Planned repository organization

Paths below are relative to the repository root. Add them as the project develops.

| Path | Purpose |
|---|---|
| `README.md` | Project overview, setup instructions, roadmap, and progress. |
| `notebooks/` | Weekly study notebooks and the integrated final notebook. |
| `src/` | Reusable functions extracted from understood and checked notebook code. |
| `data/README.md` | Dataset provenance, access instructions, sampling information, and reuse conditions. |
| `data/raw/` | Original input files, kept unchanged. |
| `data/processed/` | Locally generated processed arrays. |
| `outputs/figures/` | Generated diagnostic and comparison figures. |
| `outputs/reports/` | Inventories, parameter tables, and written reports. |
| `requirements.txt` | Dependency versions, to be added after validating the working environment. |
| `.gitignore` | Rules for excluding virtual environments, local data, and regenerable files. |

Suggested notebook names: `01_reading_and_metadata.ipynb` through `08_final_workflow.ipynb`, using descriptive names for each intermediate module.

Git tracks files rather than empty directories. Add a short README where a directory needs to be represented before its main contents are available.

## Environment setup

The following commands target Linux or macOS with Python 3 and its `venv` module available. Run them from the local repository root.

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install obspy numpy scipy matplotlib jupyterlab ipykernel
python -m ipykernel install --user --name seismic-obspy --display-name "Python (seismic-obspy)"
python -m jupyterlab
```

In JupyterLab, select **Python (seismic-obspy)** as the notebook kernel.

On Windows PowerShell, create the environment with `py -m venv .venv` and activate it with `.venv\Scripts\Activate.ps1`; the subsequent `python -m ...` commands remain the same.

These commands install the available dependency versions. After confirming a working environment, record the versions used and add a tested dependency file. No dependency versions have been validated by this README alone.

### Main tools

| Tool | Role |
|---|---|
| ObsPy | SEG-Y reading, trace metadata, and supported trace operations. |
| NumPy | Arrays, numerical calculations, FFTs, and synthetic signals. |
| SciPy | Supporting signal-processing routines and numerical utilities. |
| Matplotlib | Trace, section, spectral, and diagnostic plots. |
| JupyterLab | Interactive code, explanations, exercises, and results. |
| pathlib | File paths and directory handling through the Python standard library. |

## Dataset preparation and first read

Place a suitable SEG-Y file in `data/raw/`. The filename below is an example; replace it with the actual input filename.

Launch JupyterLab from the repository root and check the working directory before reading the file:

```python
from pathlib import Path
from obspy import read

print("Working directory:", Path.cwd())
input_file = Path("data/raw/section.sgy")

if not input_file.is_file():
    raise FileNotFoundError(f"Input file not found: {input_file}")

stream = read(str(input_file), format="SEGY")

if len(stream) == 0:
    raise ValueError("The input contains no traces.")

trace = stream[0]
print("Number of traces:", len(stream))
print("Samples in the first trace:", trace.stats.npts)
print("Sampling interval in seconds:", trace.stats.delta)
```

Reading a file successfully does not establish that its geometry or time reference has been interpreted correctly. Inspect the headers and the dataset documentation before assigning physical coordinates to the plot axes.

Use trace number for the horizontal axis until spatial ordering is confirmed. Use time relative to the first sample until the recording-time reference is established. Do not automatically treat ObsPy `stats.starttime` as seismic reflection time.

The initial inspection must check every trace for compatible sample counts and sampling intervals. A common time axis also requires a compatible starting-time reference. Unsupported SEG-Y variants or unusually large files may require a different reading strategy; a small compatible file is the starting point for this course.

### Data provenance

Document the source, filename, acquisition or processing context when known, access instructions, and redistribution conditions in `data/README.md`. Until a dataset is selected, these details remain pending.

Keep large or restricted SEG-Y files outside the committed repository. Provide access instructions for data that can be shared. Public repository visibility does not grant redistribution rights to third-party data.

## Processing and comparison principles

- Keep the original input unchanged and process explicit copies.
- Apply candidate configurations to the same input version.
- Use the same trace selection, time window, and amplitude scale for before/after comparisons.
- Record parameters with their units, including filter cutoffs and AGC window duration.
- Inspect the difference between input and output for coherent events that may have been removed.
- Treat spectral concentration as evidence to investigate, rather than a direct separation of signal and noise.
- Keep AGC or normalized visualization outputs separate from data used for amplitude interpretation.
- Distinguish observations, quantitative measurements, and interpretation in the written conclusions.

On real data, where a clean reference is unavailable, conclusions will rely on visual continuity, spectra, residuals, and diagnostic measurements. In the synthetic experiment, the known clean signal allows direct calculation of reconstruction error. Better-looking plots alone do not establish improved signal quality.

## Incremental development on GitHub

Update this repository after each completed module. Commit the notebook, its explanatory text, and a small selection of useful results together. Suitable commit messages include:

```text
docs: add project overview and eight-week roadmap
feat: add SEG-Y inventory notebook
feat: add trace and section visualization
feat: compare candidate frequency filters
docs: summarize synthetic validation results
```

Track the current status honestly: distinguish planned work, implemented work, and checked results. Use issues to record unresolved questions and follow-up experiments.

Suggested `.gitignore` entries:

```gitignore
.venv/
__pycache__/
.ipynb_checkpoints/
data/raw/*
!data/raw/README.md
data/processed/*
!data/processed/README.md
```

Review generated outputs before committing them. Keep representative figures and concise reports; exclude bulky arrays and files that can be regenerated. Clear notebook outputs that expose restricted data or machine-specific information before publication.

### Progress checklist

- [ ] Week 1: environment and data inventory
- [ ] Week 2: trace and section visualization
- [ ] Week 3: quality control and preprocessing
- [ ] Week 4: spectral analysis
- [ ] Week 5: frequency-filter comparisons
- [ ] Week 6: gain and AGC comparisons
- [ ] Week 7: synthetic validation
- [ ] Week 8: integrated workflow and final report

## Final deliverables and acceptance criteria

The completed project should include:

1. Weekly notebooks with technical explanations and exercise responses.
2. An integrated notebook that runs from a fresh kernel using documented inputs.
3. A SEG-Y inventory and initial quality-control diagnosis.
4. Preprocessing, filter, and gain comparisons with recorded parameters.
5. A reproducible synthetic experiment with a fixed random seed.
6. Selected figures, processed arrays, and a short final report.
7. Updated setup, data-access, and execution instructions.

The final report should explain which problem each operation addressed, what supports the selected configuration, whether useful events or amplitudes changed, and which conclusions remain uncertain.

SEG-Y export is an optional later extension. It requires separate checks of header preservation and output compatibility; saving NumPy arrays is sufficient for the core project.

## Use of AI assistance

AI tools may assist with explanations, code drafting, debugging, and documentation. Any such use should be documented in the relevant notebook, together with the checks performed by the learner. Generated code must be understood and evaluated before it is presented as a validated result. The purpose of this project is to develop independent implementation and interpretation skills.

## Documentation and references

- [ObsPy documentation](https://docs.obspy.org/)
- [ObsPy core objects](https://docs.obspy.org/packages/obspy.core.html)
- [ObsPy SEG-Y support](https://docs.obspy.org/packages/obspy.io.segy.html)
- [NumPy documentation](https://numpy.org/doc/stable/)
- [SciPy signal processing](https://docs.scipy.org/doc/scipy/reference/signal.html)
- [Matplotlib documentation](https://matplotlib.org/stable/)
- [JupyterLab documentation](https://jupyterlab.readthedocs.io/en/stable/)

## Licensing

A code license has not yet been selected. Add a `LICENSE` file before inviting reuse under a specific license. Dataset permissions must be documented separately and may differ from the repository's code license.
