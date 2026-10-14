# Introduction to Seismic Data Processing with ObsPy

A progressive, eight-week learning project for inspecting, visualizing, and conditioning marine shot gathers in SEG-Y format using Python and ObsPy. The course uses a verified three-shot sample extracted from the MATRIX survey line MX05.

The central question is: **How can we improve seismic data visualization and reduce selected noise components while checking for distortion or loss of useful signal?**

The project combines seismic processing concepts with practical Python development. Each weekly module includes technical study, reinforcement exercises, and a project task. The repository will be updated incrementally as these activities are completed.

## Project status

**Stage: course development with an inspected real-data sample.** The MATRIX MX05 excerpt has been extracted and successfully read with ObsPy. Its trace counts, sampling, and basic waveform validity have been checked. Weekly processing modules and final results will be developed progressively; the roadmap describes planned activities rather than completed processing.

## Objectives

- Read SEG-Y files and inspect their trace and file metadata.
- Understand ObsPy `Stream` and `Trace` objects.
- Check sample counts, sampling intervals, and recording-time references.
- Build trace and shot-gather plots with consistent comparison settings.
- Identify suspicious traces using basic quality-control measurements.
- Evaluate mean removal, detrending, and tapering separately.
- Analyze frequency content and compare candidate filters.
- Understand the effects of gain and automatic gain control (AGC).
- Validate selected processing choices against a synthetic signal with known ground truth.
- Produce a reproducible notebook and an evidence-based interpretation of the results.

## Scope and prerequisites

The course dataset is a small pre-stack excerpt containing three complete shot-ID groups from MATRIX line MX05. The emphasis is on trace conditioning, quality control, and learning Python through a concrete marine geophysical application. A shot gather groups receivers recording the same source event; it must not be interpreted as a stacked seismic section.

This introductory project does not implement a complete production processing sequence. Acquisition geometry reconstruction, velocity analysis, NMO, stacking, migration, deconvolution, and 4D matching are outside the core eight-week scope.

Basic familiarity with seismic traces and sampling is helpful. Python skills will be developed progressively: variables and indexing first, then arrays, loops, conditionals, functions, plotting, and file handling.

## Eight-week roadmap

Suggested workload: **4–6 hours per week**.

| Week | Module | Main activities | Project deliverable |
|---|---|---|---|
| 1 | Environment, SEG-Y reading, and metadata | Introduce marine shots and streamer channels; organize paths; inspect `Stream`, `Trace`, FFID/channel headers, sampling, delays, and prior processing. | Data inventory and selection of the main study shot. |
| 2 | Trace and shot-gather visualization | Select a shot by FFID; build time axes; assemble compatible arrays; plot channel versus time; select windows and control amplitude scales. | Reference figures and a documented comparison region. |
| 3 | Quality control and preprocessing | Measure mean, RMS, absolute peak, non-finite values, and zero-sample fraction; investigate channel anomalies and existing mutes; compare mean removal, linear detrending, and tapering. | Initial diagnosis and individual preprocessing comparisons. |
| 4 | Frequency analysis | Study sampling and Nyquist frequency; compute FFT amplitude spectra; examine window duration, weighting, and frequency resolution; compare representative windows and account for the existing >20 Hz high-pass filter. | Spectral diagnosis and candidate filter bands. |
| 5 | Frequency filtering | Compare low-pass, high-pass, or band-pass configurations; record cutoffs, filter settings, and phase treatment; inspect boundaries and removed content. | Comparison of three filter configurations and a justified selection. |
| 6 | Gain and AGC | Compare constant gain, trace normalization, and moving-RMS AGC; vary the AGC window; inspect noise amplification and amplitude changes. | Gain comparisons and a separate visualization-oriented data version. |
| 7 | Synthetic validation | Generate a small synthetic gather with known events; convolve with a wavelet; add controlled noise with a fixed random seed; apply selected filters; evaluate RMS error and correlation. | Ground-truth experiment showing both benefits and limitations. |
| 8 | Integration and reporting | Consolidate the workflow; document parameters; compare original and processed gathers; check selected parameters on another shot; save results; restart and execute the notebook from beginning to end. | Reproducible final notebook, selected figures, and a short report. |

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
| `data/raw/matrix_shot_sample.sgy` | Verified MX05 excerpt used as unchanged course input. |
| `data/processed/` | Locally generated processed arrays. |
| `outputs/figures/` | Generated diagnostic and comparison figures. |
| `outputs/reports/` | Inventories, parameter tables, and written reports. |
| `requirements.txt` | Dependency versions, to be added after validating the working environment. |
| `.gitignore` | Rules for excluding virtual environments, local data, and regenerable files. |

The first study notebook is `week_01_segy_inspection.ipynb`. Use consistent `week_02_...` through `week_08_...` names for subsequent modules. The separate `matrix_quick_inspection.ipynb` notebook documents inspection and excerpt extraction from the full local source file.

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

## MATRIX dataset and sample provenance

**MATRIX (Mid-Atlantic Resource Imaging Experiment)** was a USGS-led marine survey conducted in August 2018 aboard the research vessel *Hugh R. Sharp*, in partnership with the U.S. Department of Energy and the Bureau of Ocean Energy Management. It acquired high-resolution 2D multichannel seismic data along the **U.S. Atlantic continental margin between North Carolina and New Jersey**, supporting investigations of subseafloor gas hydrates and methane seepage.

Official data and documentation:

- [MATRIX data release — DOI: 10.5066/P91WP1RZ](https://doi.org/10.5066/P91WP1RZ)
- [USGS dataset description and data-access page](https://www.usgs.gov/data/multichannel-seismic-reflection-and-navigation-data-collected-using-sercel-gi-guns-and)
- [Acquisition and processing metadata](https://cmgds.marine.usgs.gov/catalog/whcmsc/SB_data_release/DR_P91WP1RZ/2018-002-FA_MCS_meta.html)

### Source file and course excerpt

A small sample was extracted from **`MX05.sht-raw-tbnav-rbr.segy`**, the shot-gather SEG-Y file for MATRIX line **MX05**, using `matrix_quick_inspection.ipynb`. The extracted file is stored at:

```text
data/raw/matrix_shot_sample.sgy
```

Extraction copied the selected trace records and file headers byte for byte. It did not apply gain, filtering, resampling, time clipping, or coordinate changes. Original trace sequence numbers were retained; the textual header still describes the original MX05 line. A separate provenance report records the subset selection.

| Property | Course sample |
|---|---|
| Source survey | MATRIX, USGS field activity 2018-002-FA |
| Source line/file | MX05 / `MX05.sht-raw-tbnav-rbr.segy` |
| Selected shot IDs (FFIDs) | 780, 781, 782 |
| Channels per selected shot | 112, numbered 1–112 |
| Total traces | 336 |
| Samples per trace | 10,001 |
| Sampling interval | 1 ms |
| First-to-last sample time | 0–10 s |
| Trace-header recording delay | 0 ms in the sample |
| Format | SEG-Y revision 1, big-endian, IBM float32 |
| File size | 13,525,584 bytes (approximately 12.9 MiB) |
| SHA-256 | `79a839018f3896a30ad98cefe762421efd66d1b15dc5ff1d052188393d52134a` |

The sample has been read independently with ObsPy. Trace counts and sampling agree with the inspection report; no NaN/Inf samples or entirely zero traces were found. This check covers the excerpt, not all waveforms in the full line. The full-file header inventory reports 778 shot IDs and 87,136 traces.

Use **shot 780** for guided examples, **shot 781** for exercises, and **shot 782** to check whether selected processing parameters behave consistently. A 2.4–4.0 s window is a useful initial region for closer visual examination; retain the full time range for overview and QC.

### What “raw” means here

The directory name `data/raw/` means **unchanged course input**. The distributed MX05 shot gathers are not untouched instrument recordings. According to their textual header, they were imported from SEG-D, assigned geometry/CMP information and source/receiver static values, and received a Butterworth high-pass filter above 20 Hz plus top and bottom mutes. The source/receiver static values are documented as stored in headers but not applied.

The course therefore studies additional conditioning of the published input. It cannot reconstruct frequency content already removed by earlier processing. Mean removal and tapering should be evaluated for usefulness rather than applied automatically. Ground-truth validation remains a separate synthetic experiment.

### Geometry and header interpretation

The MX05 textual header describes a 112-channel streamer with 6.25 m groups and a 30 m shot interval. Geographic source, receiver, and CMP coordinates are stored in scaled arcseconds; the textual header instructs division by **360,000** to obtain decimal degrees. The inspected coordinate-scalar field contains **+100**, which appears inconsistent with that conversion under the standard scalar rule. Do not silently apply a generic scalar conversion or treat these fields as meters.

Begin with **channel number** on the gather's horizontal axis. Physical geometry calculations require an explicit, documented resolution of coordinate units and scaling. Time axes should be constructed from the sampling and recording-delay headers, rather than interpreting ObsPy `stats.starttime` as reflection time.

### Dataset preparation and first read

The small excerpt is sufficient for the introductory course; downloading or loading the full multi-gigabyte line is unnecessary when the sample is available. Keep it unchanged in `data/raw/`.

Notebook paths are relative to the **kernel working directory**, which may differ from the directory where JupyterLab was launched. The example below handles execution from either the project root or its immediate `notebooks/` directory:

```python
from pathlib import Path
from obspy import read, Stream

cwd = Path.cwd()
project_root = cwd.parent if cwd.name == "notebooks" else cwd
input_file = project_root / "data" / "raw" / "matrix_shot_sample.sgy"
print("Working directory:", cwd)

if not input_file.is_file():
    raise FileNotFoundError(f"Check your project location and input path: {input_file}")

stream = read(str(input_file), format="SEGY")
print("Number of traces:", len(stream))  # Expected: 336
print("Samples in the first trace:", stream[0].stats.npts)  # Expected: 10001
print("Sampling interval (s):", stream[0].stats.delta)  # Expected: 0.001

shot_780 = Stream([
    tr for tr in stream
    if tr.stats.segy.trace_header.original_field_record_number == 780
])
print("Traces in shot 780:", len(shot_780))  # Expected: 112
```

For another working-directory layout, set `project_root` explicitly. A common gather time axis requires compatible sample counts, sampling intervals, and recording delays across its traces.

### Attribution and sharing

Credit **Baldwin et al. (2020), U.S. Geological Survey**, and link to [10.5066/P91WP1RZ](https://doi.org/10.5066/P91WP1RZ). Clearly identify the course file as a three-shot excerpt from MX05. The official release is marked **CC0 1.0 Universal**; the repository's own code license is a separate decision.

Keep the full source file outside the repository. Retain the extraction report and provenance alongside the course documentation. Additional dataset details may be collected in `data/README.md` as the repository develops.

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
feat: add marine shot-gather visualization
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
# Optional: track the small, CC0 course excerpt after reviewing provenance.
# !data/raw/matrix_shot_sample.sgy
data/processed/*
!data/processed/README.md
```

The default rules above exclude seismic amplitudes from Git. If you choose to distribute the verified 12.9 MiB course sample, uncomment its exception and retain the source attribution; otherwise provide sample-access instructions.

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

- [MATRIX data release, Baldwin et al. (2020), USGS](https://doi.org/10.5066/P91WP1RZ)
- [ObsPy documentation](https://docs.obspy.org/)
- [ObsPy core objects](https://docs.obspy.org/packages/obspy.core.html)
- [ObsPy SEG-Y support](https://docs.obspy.org/packages/obspy.io.segy.html)
- [NumPy documentation](https://numpy.org/doc/stable/)
- [SciPy signal processing](https://docs.scipy.org/doc/scipy/reference/signal.html)
- [Matplotlib documentation](https://matplotlib.org/stable/)
- [JupyterLab documentation](https://jupyterlab.readthedocs.io/en/stable/)

## Licensing

A code license has not yet been selected. Add a `LICENSE` file before inviting reuse under a specific license. The MATRIX data release is marked CC0 1.0 Universal. Preserve its attribution and the excerpt provenance independently of the license selected for this repository's code.
