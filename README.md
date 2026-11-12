# IDS-Masters

Network intrusion-detection preprocessing and autoencoder notebooks.

## Change history

For every future repository change, read this README first and add a new entry below without removing or rewriting earlier entries. Put the newest entry first. Each entry must contain the actual date and time in `Africa/Cairo` (including the UTC offset), the affected notebook paths, other changed files, a description of the changes, validation performed, and any rerun instructions or limitations. Commit the README together with the corresponding changes. See [AGENTS.md](AGENTS.md) for the repository editing instructions.

This written history starts with the entry below. Earlier versions remain available in the [Git commit history](https://github.com/Omar-Abdalrahim/IDS-Masters/commits/main/).

### 2026-11-12 12:19:55+02:00 — Increase ADASYN balance tolerance to 3% across all preprocessing notebooks

**Timezone:** Africa/Cairo.

**Notebooks:**

- `Notebooks/CIC_Preprocessing.ipynb`
- `Notebooks/Fork of CIC_Preprocessing (CIC-BoT-IoT).ipynb`
- `Notebooks/Fork of CIC_Preprocessing (CSE-CIC-IDS2018).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_BOT_IOT).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_CSE_CIC_IDS2018).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_TON_IOT).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_UNSW_NB15).ipynb`
- `Notebooks/NF-Preprocessing.ipynb`

**Other files:** `README.md`.

**Changes:**

- Changed `ADASYN_BALANCE_TOLERANCE` from `0.01` to `0.03` in both main preprocessing notebooks and all six forks (eight notebooks total).
- Updated matching comments and markdown explanations from 1% to 3%. With the existing selection expression, Attack classes with counts at least 97% of the largest class are left unchanged by ADASYN; smaller targeted classes still request the largest class count.
- Preserved preprocessing, dataset-specific settings, sampling caps, splitting, sampler calls, diagnostics and plots. Each notebook changes only its imbalance configuration cell and explanatory markdown. Earlier README history remains intact and records the settings used at those earlier dates.

**Validation:**

- Parsed all eight updated notebooks and checked the actual Python assignment is `0.03`; compiled each changed configuration cell.
- Verified two source-cell edits per notebook and no remaining 1% tolerance assignment/comment/explanation in notebook sources. Resampling and other code cells are unchanged.
- Full dataset or sampler runs were not repeated for this configuration-only change.

**Rerun instructions:**

- Pull/import the updated notebook and rerun its imbalance configuration cell before resampling. Reload prepared training arrays if previous sampler cells already released them; rerun ADASYN, Tomek Links and balanced export to apply the new tolerance to generated data. Existing preprocessing artifacts can be reused.

### 2026-11-12 11:20:36+02:00 — Apply shared improvements to remaining NF and CIC notebooks

**Timezone:** Africa/Cairo.

**Notebooks:**

- `Notebooks/CIC_Preprocessing.ipynb`
- `Notebooks/Fork of CIC_Preprocessing (CIC-BoT-IoT).ipynb`
- `Notebooks/Fork of CIC_Preprocessing (CSE-CIC-IDS2018).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_BOT_IOT).ipynb`
- `Notebooks/NF-Preprocessing.ipynb`

**Other files:** `README.md`.

**Changes:**

- Updated the original NF notebook and its NF_BOT_IOT fork to match the previously updated NF_TON_IOT implementation, preserving each notebook's dataset selection and metadata. This includes fixed SECOND_BYTES log1p compression before imputation, matching fit/transform handling, complete-column skipping, shrinking exclusions, named frozen-imputer lookup, timestamped output folders, optional-benchmark handling, timings/shapes, separate sampler/results/plot cells, categorical-domain snapshots and genuine generated-row previews.
- Adapted the shared improvements to the CIC base and both CIC forks: `skip_complete=True`, second-resolution output folder timestamps, export batch/timing reports, ADASYN/Tomek start/end times and shapes, post-ADASYN matrix size, added/removed rows, and separate Tomek Links, timing/distribution-table and Plotly cells. No periodic status thread or recurring elapsed message is included.
- CIC retains its own `Protocol` one-hot encoding, sampled imputer/scaler fitting, full-training category vocabulary and optional PCA over scaled numeric measurements. Protocol indicators remain outside numeric PCA. CIC receives no extreme-column log transform or shrinking exception; removed the obsolete SECOND_BYTES float64-schema exception and related generated-README wording from those notebooks. Ordinary numeric/PCA outputs use float32 and Protocol indicators use uint8 before resampling; synthetic coordinates remain floating point afterward.
- Added the raw-domain snapshot and independent `RUN_CATEGORY_DIAGNOSTICS` test to CIC using `categories__Protocol_*` output names. The exact raw/processed/final unique counts use the same training population, with bounded value examples, new/non-binary entries and genuine ADASYN previews (up to five per targeted Attack) marked by actual Tomek retention. Diagnostic metadata stays outside model datasets.
- Preserved dataset choices, paths, per-class sampling caps, split/seed/holdout settings, scaler/PCA configuration, target restoration and existing GitHub headings. In particular, retained the CSE-CIC fork's original Label-to-Attack conversion and binary Label creation. The three already updated NF forks are unchanged.
- Updated notebook and generated-output README instructions for each family. Cleared old code outputs/execution metadata because they precede the updated pipeline. Retained all earlier root README history; no separate changes.md currently exists.

**Validation:**

- Parsed all five notebooks and compiled every Python code cell, accounting for IPython magics: 61 code cells in each NF notebook and 41 in each CIC notebook.
- Verified preservation of source-loading/target-conversion cells, dataset selection, sampling and split source, notebook metadata, CIC configuration and existing GitHub headings. Verified NF source parity with the updated NF_TON_IOT reference except the BoT dataset selection. Checked CIC sources contain no SECOND_BYTES/log1p/shrinking-exception handling or NF-specific encoder references.
- Executed the actual fit, Arrow schema, batched export, joblib save/reload, imbalance loading, ADASYN, Tomek, summary, chart, balanced-export and categorical-diagnostic cells on small synthetic datasets in all five notebooks. Exported training tables matched the direct fitted transforms using their exact Arrow schema; repeated chart rendering was tested with browser display suppressed.
- Exercised CIC with numeric PCA disabled and enabled (two components, separate 50-row PCA cap, 80-row imputer/scaler cap). Exercised NF with extreme byte measurements near 1e261/1e257 and missing values; transformed outputs were finite without numerical RuntimeWarnings. Tested both synthesis and already-balanced/skipped-ADASYN cases in the base notebooks.
- Compared final ADASYN/Tomek arrays with the original sampler cells on the same prepared inputs: exact equality. Verified generated preview values/targets against actual ADASYN appended rows, retention against Tomek sample_indices_, raw-domain counts, processed/final unique/new-entry counts and balanced output columns/schema.

**Rerun instructions and limits:**

- Pull/import the updated notebooks and run from Setup through preprocessing, export, ADASYN, Tomek and balanced export to create consistent artifacts and diagnostic snapshots. Then run the categorical Testing cell with `RUN_CATEGORY_DIAGNOSTICS=True` and the matching `OUTPUT_DIR`.
- NF outputs must be regenerated for the new compression flow. Pass raw data to the saved NF preprocessor without pre-logging it; already prepared files must not be transformed again. Define its existing encoding helpers and compression helper before loading joblib in a new session.
- CIC keeps its existing sampled fitting and optional numeric PCA behavior. Changing PCA_COMPONENTS changes numeric output columns and requires downstream retraining. The new diagnostic snapshots require raw training frames during export and generated rows during resampling; older runs lack these files.
- Full source datasets and Kaggle/Colab sessions were not run here. Synthetic integrations establish the checked behavior on those inputs, not full-data runtime, memory use or model accuracy. One-column categorical scans still scale with row count/cardinality.

### 2026-11-12 10:16:00+02:00 — Inspect categorical domains and genuine ADASYN examples in Testing

**Timezone:** Africa/Cairo.

**Notebooks:**

- `Notebooks/Fork of NF-Preprocessing (NF_CSE_CIC_IDS2018).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_TON_IOT).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_UNSW_NB15).ipynb`

**Other files:** `README.md`.

**Changes:**

- Added an independent `RUN_CATEGORY_DIAGNOSTICS = True` flag and one diagnostic code cell near the top of Testing in every fork. It uses the main saved outputs, not the legacy benchmark's different encoding.
- The main export now saves `raw_categorical_summary.json` before releasing raw training frames: exact unique counts (including missing), missing-row counts, up to 20 example entries per cleaned/unencoded categorical training feature, and mappings from raw inputs to encoded outputs. This baseline uses the same training rows as `train.parquet`.
- The Testing cell reads one encoded Parquet column at a time from `train.parquet` and `train_adasyn_tomek.parquet`. It reports exact processed/final unique counts, new unique-entry counts, bounded value previews and non-binary final unique counts for one-hot, binary-port and flag indicators. `MAX_UNIQUE_EXAMPLES = 12` controls encoded-value previews only; counts use every row.
- Joint identifier PCA outputs are explicitly marked continuous and mapped to all of their IP/DNS inputs; they are excluded from binary-indicator domain checks. The report compares domains, not category frequencies or semantic validity.
- ADASYN now saves up to five actual appended synthetic rows per targeted Attack in `adasyn_generated_preview.parquet`, selecting encoded columns plus preview-only row/target metadata. After Tomek, the preview records retention using its actual `sample_indices_`. Testing displays these rows and highlights non-binary generated indicator coordinates. A small preview can miss fractional coordinates, so the final full-data domain report remains separate.
- The generated preview is captured before matrix cleanup, including a valid empty preview when ADASYN is skipped. Diagnostic metadata is not added to model feature matrices or the balanced training schema. Updated the generated output README and Testing instructions with the new files and prerequisites.
- Preserved preprocessing, split/sampling settings, sampler parameters, final resampled arrays, existing result/plot cells, benchmark logic and export schema. No periodic status messages were introduced. All earlier root README history is retained.

**Validation:**

- Parsed each notebook and compiled all 61 code cells, accounting for IPython magic syntax. Verified only five existing sources changed and two diagnostic cells were added.
- Used each notebook's actual Encoding implementation (one-hot, BinaryEncoder, flag decomposition, ICMP decoding and joint PCA) on small synthetic multiclass inputs, with missing raw-category values included. Exercised both ADASYN synthesis and already-balanced/no-synthesis cases.
- Verified raw baseline counts/missing counts, every encoded-column mapping, exact unique/new-entry counts against pandas, PCA exclusion from binary checks, and detection of fractional generated indicators.
- Matched each preview row to its actual ADASYN output index, class and encoded values; checked its Tomek-retention flag and the five-per-Attack cap. Empty previews and missing-diagnostic-file instructions also passed.
- Compared original and updated ADASYN/Tomek outputs: final arrays matched exactly. Executed the actual balanced-export and diagnostic cells; balanced file columns/schema were preserved and preview metadata stayed separate.

**Rerun instructions and limits:**

- Run the updated main export while raw `train` and the fitted preprocessor are available to save the raw-domain baseline. Then run the updated imbalance loading, ADASYN and Tomek/export cells to create the generated preview and final training file. In Testing, enable `RUN_CATEGORY_DIAGNOSTICS` and run the new categorical comparison cell with `OUTPUT_DIR` pointing to that run.
- Older runs lack these snapshots. If raw splits/matrices were released, recreate them through the normal loading/splitting/fitting/export and resampling flow. The diagnostic prints prerequisite guidance instead of silently rerunning samplers or labelling arbitrary final rows as generated.
- The comparison uses all rows of each saved training stage while loading only one projected feature column at a time; full-data scans can take time and memory still scales with one column's cardinality. Example lists and generated previews are intentionally bounded.
- Full source datasets and Kaggle/Colab sessions were not run here. Synthetic integration tests do not establish full-data runtime, accuracy or semantic validity of interpolated categorical coordinates.

### 2026-11-12 08:20:02+02:00 — Remove periodic resampling messages from all three forks

**Timezone:** Africa/Cairo.

**Notebooks:**

- `Notebooks/Fork of NF-Preprocessing (NF_CSE_CIC_IDS2018).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_TON_IOT).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_UNSW_NB15).ipynb`

**Other files:** `README.md`.

**Changes:**

- Removed the 30-second "still running" updates, `RESAMPLING_STATUS_SECONDS`, and their background-thread machinery from all three forks at the user's request.
- Retained stage start, finish/interruption and total elapsed-time messages. Input/output shapes, post-ADASYN size/dtype, added/removed row counts, timing-summary tables and separate Tomek Links/Plotly cells remain intact.
- Updated each notebook's runtime-reporting instructions. Only the reporting helper/configuration and its explanatory markdown changed; sampler calls, preprocessing, dataset settings, result tables, plots and export code are unchanged.
- Preserved all earlier README history; the preceding entry describes the superseded periodic-message implementation.

**Validation:**

- Parsed all three notebooks and compiled their 60 code cells each, accounting for IPython magic syntax.
- Executed the simplified reporting helper on completion and failure; verified start/finish/error messages and exception propagation.
- Checked all notebook sources for removal of the interval, recurring message and thread machinery. Verified all other cells are unchanged.

**Rerun instructions and limits:**

- Import/pull the updated notebooks and rerun the imbalance configuration/helper cell before the next resampling call. No preprocessing or dataset regeneration is required for this reporting-only change.
- Full dataset and Kaggle/Colab runs were not repeated. Sampler behavior is unchanged.

### 2026-11-12 08:13:50+02:00 — Report resampling runtime and separate charts in all three forks

**Timezone:** Africa/Cairo.

**Notebooks:**

- `Notebooks/Fork of NF-Preprocessing (NF_CSE_CIC_IDS2018).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_TON_IOT).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_UNSW_NB15).ipynb`

**Other files:** `README.md`.

**Changes:**

- Added timing around each main ADASYN and Tomek Links call using `perf_counter`. Print feature/target shapes before and after each stage, the number of rows added/removed, and post-ADASYN feature-matrix size and dtype. Retain `adasyn_seconds`, `tomek_seconds`, row counts and a small `resampling_summary` table for inspection.
- Added periodic elapsed-time messages via a small status context manager. `RESAMPLING_STATUS_SECONDS = 30` controls the update interval. Start, finish and interruption/error messages are flushed immediately; the background status thread is stopped on completion, exceptions and keyboard interruption.
- Neither sampler exposes a built-in progress percentage, completed-query count or ETA. The periodic message reports the active stage and elapsed time only; it does not measure row progress or guarantee useful computation is advancing. No invented percentage or remaining-time estimate is displayed. ADASYN skipped due to approximate balance is explicitly marked with zero sampler runtime.
- Separated Tomek Links, the class-distribution/timing tables and the Plotly chart into distinct cells with headings. The plot can be redrawn from small result tables without repeating resampling. Free expanded ADASYN inputs immediately after Tomek Links, before plotting/export.
- Preserved the full training-only samplers, sampling policy, neighbour settings, random seed, class tolerance, dataset-specific configuration, preprocessing, target restoration and export logic. Only runtime reporting and cell organization changed. Cleared stale outputs/execution metadata from modified and following code cells.

**Validation:**

- Parsed all three notebooks and compiled each notebook's 60 Python code cells, accounting for IPython magic syntax.
- Executed the original and updated ADASYN/Tomek cells using the installed imbalanced-learn implementation on small synthetic multiclass inputs, both requiring ADASYN and already balanced. For every fork, final feature/target arrays and class-distribution tables matched exactly; timings were nonnegative and the skipped case reported zero ADASYN time.
- Executed the separate Plotly cell twice after the expanded ADASYN inputs were freed, with browser rendering suppressed. Checked timing-summary row counts and shape/added/removed-row messages.
- Exercised periodic elapsed updates and status-thread cleanup on successful completion, exceptions and keyboard interruption. Verified unrelated existing cell sources are unchanged.

**Rerun instructions and limits:**

- Pull/import the updated notebooks. In an existing session, run the imbalance configuration/status-helper cell, then load the prepared training arrays and run the ADASYN, Tomek Links, results and plot cells in order. ADASYN consumes/deletes `X_train`/`y_train`; Tomek Links releases `X_adasyn`/`y_adasyn`, so reload/recompute inputs before rerunning either sampler. Tables/plots can be repeated using their retained result variables.
- Existing prepared datasets remain compatible; these reporting changes do not require repeating preprocessing. Earlier log1p changes still require regenerated artifacts if not already rerun.
- Full source datasets and Kaggle/Colab sessions were not executed here. Synthetic integration tests establish output parity on the tested inputs, not full-dataset runtime, memory use or rendering behavior. Status output can be delayed by notebook output handling or operations that hold Python's GIL; it is not a liveness guarantee.

### 2026-11-11 11:35:45+02:00 — Apply NF overflow prevention to CSE-CIC-IDS2018 and UNSW-NB15 forks

**Timezone:** Africa/Cairo.

**Notebooks:**

- `Notebooks/Fork of NF-Preprocessing (NF_CSE_CIC_IDS2018).ipynb`
- `Notebooks/Fork of NF-Preprocessing (NF_UNSW_NB15).ipynb`

**Other files:** `README.md`.

**Changes:**

- Applied the same numerical overflow prevention already used by the NF_TON_IOT fork: fixed `log1p` compression of `SRC_TO_DST_SECOND_BYTES` and `DST_TO_SRC_SECOND_BYTES` before imputation, both in the imputer training sample and inside the saved numerical pipeline. This prevents extreme magnitudes from reaching iterative regression or later distance-based resampling. Missing values remain available for imputation; negative byte measurements raise an explicit error. Compression does not establish that the original measurements are valid.
- Enabled `skip_complete=True` in the main and optional comparison IterativeImputers, avoiding unnecessary regression fitting for complete columns.
- Excluded the two raw SECOND_BYTES fields from fastai shrinking and updated its diagnostic to explain their preserved float64 storage.
- Applied compression once in the optional raw-data benchmark loader, including when Testing runs independently after Setup. Updated the saved-imputer lookup to use its step name after insertion of the compression step.
- Added a time and `log1p` marker to generated output-directory names. Updated notebook instructions and generated output README text with the processing order, artifact-loading requirements and regeneration instructions. Cleared execution metadata, outputs and counts that predate this fix.
- Preserved each fork's dataset choice and settings: CSE-CIC retains its 300,000-row per-Attack cap, while UNSW retains its 200,000-row cap. Target/schema, categorical encoding/PCA, split, scaler and main resampling code are unchanged. ADASYN still runs on prepared training data, followed by Tomek Links; validation/test are not resampled.
- Preserved all earlier root README history. The existing `AGENTS.md` continues to require reading this README and adding a dated, timed entry in the same commit for every future repository edit.

**Validation:**

- Parsed both notebooks and compiled all 58 Python code cells in each, accounting for IPython magic syntax.
- Reproduced numerical RuntimeWarnings with the previous imputer on representative extreme inputs. Executed each updated notebook's helper and numerical-pipeline expression on synthetic data with and without missing values: finite outputs, no numerical RuntimeWarnings, matching whole-frame/batch transformations, preserved feature names, correct frozen-imputer lookup and successful joblib save/reload.
- Verified input frames and unrelated features remain unchanged, absent SECOND_BYTES columns are supported, and negative byte measurements are rejected.
- Verified only the intended 14 source cells changed in each notebook; dataset selection, sampling caps, other source cells, output schema and main ADASYN/Tomek code are preserved. Checked the optional loader compresses once and shrinking skips the configured fields.

**Rerun instructions and limits:**

- Restart Kaggle/Colab and rerun each notebook from Setup through preprocessing, export, ADASYN and Tomek Links. Regenerate preprocessors and prepared datasets instead of reusing older artifacts.
- Pass raw data to the saved preprocessor without manually logging it first. Prepared Parquet files are already transformed. In a new session, define `EXTREME_COLUMNS`, `compress_extreme_values` and the existing encoding helpers before loading joblib.
- Full NF-CSE-CIC-IDS2018 and NF-UNSW-NB15 source datasets were unavailable locally. Full categorical processing, Parquet export and full ADASYN/Tomek runs were not executed here; synthetic checks do not establish full-data model performance.

### 2026-11-11 10:51:00 +02:00 — Prevent NF_TON_IOT imputer overflow

**Timezone:** Africa/Cairo.

**Notebook:** `Notebooks/Fork of NF-Preprocessing (NF_TON_IOT).ipynb`.

**Other files:** `README.md`, `AGENTS.md`.

**Changes:**

- Added fixed `log1p` compression to `SRC_TO_DST_SECOND_BYTES` and `DST_TO_SRC_SECOND_BYTES` before imputation. The observed values near `1e261` and `1e257` previously overflowed arithmetic in the imputer's regression models. Compression preserves rows and value ordering; it does not establish that the original measurements are valid.
- Applied the same compression to the imputer's training sample and inside the saved numerical pipeline, so raw train, validation, test, optional held-out data, and future raw inputs receive it exactly once. Other numerical measurements and categorical encoding are unchanged. Negative byte measurements raise an explicit error; missing values remain available for imputation.
- Enabled `skip_complete=True` for the main and comparison IterativeImputers to avoid unnecessary regression fitting for complete columns.
- Excluded both raw SECOND_BYTES columns from fastai shrinking and updated the shrinking diagnostic to report that they retain float64. Their original large magnitudes remain visible in the raw-data diagnostics.
- Updated the optional benchmark loader to apply the same fixed compression before its imputation/scaling experiments, including when Testing is run independently after Setup.
- Updated the saved-imputer lookup to use its step name after adding the compression step. Added a timestamp and `log1p` marker to output-directory names to distinguish regenerated artifacts from older runs.
- Updated notebook instructions and its generated output README with the preprocessing order and loading requirements. Cleared old execution outputs and counts because they predate this fix.
- Kept the prepared output schema, targets (`Label` and `Attack`), split/sampling policy, ADASYN settings, and Tomek Links settings unchanged. The order remains preprocessing and saving, then ADASYN on prepared training data, then Tomek Links. Validation/test are not resampled; synthetic feature coordinates remain floating point.
- Added the repository rule requiring a dated, timed README entry with notebook names for each future change.

**Validation:**

- Parsed the notebook and compiled all 58 Python code cells, treating notebook magic commands as IPython syntax.
- Reproduced numerical RuntimeWarnings using the original imputer with representative extreme values.
- Ran the actual updated numerical-branch code on small synthetic datasets with and without missing values. Outputs were finite without numerical RuntimeWarnings; whole-frame and batched transformations matched; feature names were preserved; the named frozen-imputer lookup and joblib save/reload worked.
- Checked that compression leaves the input frame and unrelated features unchanged, supports datasets without these columns, and rejects negative byte values.
- Confirmed that output-schema and resampling code cells remain unchanged and that optional tests apply compression once.

**Rerun instructions and limits:**

- Restart the Kaggle/Colab session and run the notebook from Setup through preprocessing, export, ADASYN and Tomek Links. Regenerate the preprocessor and prepared datasets; do not reuse older artifacts for this new flow.
- Do not manually log-transform data before passing it to this saved preprocessor, and do not transform the prepared Parquet data again. When loading joblib in a new session, define `EXTREME_COLUMNS` and `compress_extreme_values` along with the existing encoding helpers first.
- The full NF-ToN-IoT source dataset was unavailable in the editing environment. Full categorical processing, Parquet export, and the full ADASYN/Tomek run have not been executed here. Synthetic numerical checks are not a full-data performance or accuracy evaluation.




