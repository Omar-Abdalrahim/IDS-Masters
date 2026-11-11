# IDS-Masters

Network intrusion-detection preprocessing and autoencoder notebooks.

## Change history

For every future repository change, read this README first and add a new entry below without removing or rewriting earlier entries. Put the newest entry first. Each entry must contain the actual date and time in `Africa/Cairo` (including the UTC offset), the affected notebook paths, other changed files, a description of the changes, validation performed, and any rerun instructions or limitations. Commit the README together with the corresponding changes. See [AGENTS.md](AGENTS.md) for the repository editing instructions.

This written history starts with the entry below. Earlier versions remain available in the [Git commit history](https://github.com/Omar-Abdalrahim/IDS-Masters/commits/main/).

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

