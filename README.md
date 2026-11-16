# IDS-Masters

Network intrusion-detection preprocessing and autoencoder notebooks.

## Change history

For every future repository change, read this README first and add a new entry below without removing or rewriting earlier entries. Put the newest entry first. Each entry must contain the actual date and time in `Africa/Cairo` (including the UTC offset), the affected notebook paths, other changed files, a description of the changes, validation performed, and any rerun instructions or limitations. Commit the README together with the corresponding changes. See [AGENTS.md](AGENTS.md) for the repository editing instructions.

This written history starts with the entry below. Earlier versions remain available in the [Git commit history](https://github.com/Omar-Abdalrahim/IDS-Masters/commits/main/).

### 2026-11-16 12:18:52+02:00 — Roll out simplified imbalance and duration reporting across preprocessing notebooks

**Timezone:** Africa/Cairo.

**Notebooks:**

- `Notebooks/CIC_Preprocessing.ipynb`.
- `Notebooks/Fork of CIC_Preprocessing (CIC-BoT-IoT).ipynb`.
- `Notebooks/Fork of CIC_Preprocessing (CSE-CIC-IDS2018).ipynb`.
- `Notebooks/Fork of NF-Preprocessing (NF_BOT_IOT).ipynb`.
- `Notebooks/Fork of NF-Preprocessing (NF_CSE_CIC_IDS2018).ipynb`.
- `Notebooks/Fork of NF-Preprocessing (NF_UNSW_NB15).ipynb`.
- `Notebooks/NF-Preprocessing.ipynb`.
- `Notebooks/UNSW-Preprocessing.ipynb`.

**Other files:** `README.md`.

**Changes:**

- Applied the current NF_TON_IOT imbalance workflow to the three main NF/CIC/native-UNSW notebooks and all remaining preprocessing forks. The already-updated `Notebooks/Fork of NF-Preprocessing (NF_TON_IOT).ipynb` was used as the reference and was not rewritten. No separate native-UNSW preprocessing fork exists in the current Notebooks directory; the NF_UNSW_NB15 fork is included as an NF extractor notebook. Autoencoder and ANN notebooks are unchanged.
- Aligned sampling to 100,000 original rows per Attack before splitting, retaining every smaller-class row. Main ADASYN selects classes below 0.5 of the largest prepared training class and requests 0.5 of that count, using a 3% tolerance around the requested target. This replaces older 200,000/300,000 caps and stale 1% settings in some notebooks. Attack remains the multiclass resampling target; binary Label is restored through the original mapping.
- Added the simplified four-helper workflow: batched matrix loading, genuine generated-row previews, prepared-file fingerprinting and atomic full-ADASYN checkpoint saving. Stage-specific selection and restore checks remain visible in their own cells. Retained the RAM guard, file-content/feature-order/settings/package-version compatibility checks and distinct cached-compute labels. Kept ADASYN, checkpoint saving, Tomek, result tables, Plotly and balanced export in separate stages. Moved existing dtype/outlier inspections into optional Testing cells and replaced the output-folder deletion instructions with checkpoint-retention guidance.
- Added independent prepared-data comparisons for an unresampled baseline plus paired selection/target ratios 0.25/0.25, 0.5/0.5 and 0.75/0.75. All cases use the same full prepared training input and untouched prepared validation set with the same 32-unit ReLU MLP, Adam, seed and 30-epoch settings. Save models, counts, sampler/training durations, matrix sizes, accuracy, balanced accuracy, macro/weighted F1, per-Attack reports, confusion matrices and comparison CSV/HTML files. Reuse compatible checkpoints/results; failures are reported without substituting another sampler.
- Standardized notebook-generated elapsed times to hours/minutes/seconds, including preprocessing fitting, batch exports, resampling, cached results and legacy experiment reports. Time/fit_time and the named threshold-time columns replace seconds-only report columns. Numeric seconds remain inside checkpoint calculations. Compatible older numeric-second model metrics are formatted when reused.
- Added the optional early cuML startup with visible CPU fallback and dispatch logs, before sklearn/fastai imports. GPU dependencies are not automatically installed. Original Colab Drive mounts are retained; no automatic Drive-upload/rclone code was introduced.
- Preserved each file's dataset selection, saved Kaggle/top-level metadata, split/holdout logic, feature encoders, scalers and PCA configuration. NF extreme-value/ICMP/flag handling remains confined to NF. CIC receives no SECOND_BYTES logic; native UNSW keeps its exact header/type/target conversion and ESP destination-slot SPI rules.
- Retained existing raw/encoded/resampled categorical-domain diagnostics and genuine synthetic-row previews. Added the same diagnostics to native UNSW, including its one-hot, binary port and optional joint IP-PCA mappings. Updated every generated output README and preserved all earlier root README history.

**Validation:**

- Validated all eight updated notebook schemas and compiled all 466 code cells. Confirmed original dataset selections/top-level metadata and source loading, split, encoding and native-UNSW casting definitions are preserved. Checked four imbalance helpers per notebook and separation of Tomek from Plotly.
- Executed actual preprocessing, batched Parquet exports, main ADASYN/Tomek, result tables/plots and balanced exports on synthetic fixtures for every updated file. Compared sampler matrices/targets exactly with direct ADASYN/Tomek calls using the requested policy. Verified stored schema/feature order and binary Label/text Attack preservation.
- Ran raw/encoded/resampled categorical audits, checked raw-domain counts and genuine generated-row positions/values, and exercised fresh-session checkpoint restore plus stale-seed rejection in every notebook.
- Executed the baseline and all three 30-epoch MLP cases in every notebook, verified saved models/reports and the shared 0.5 ADASYN checkpoint, then confirmed completed-result reuse skips model training. Checked formatted timing tables, JSON/CSV reports and minute/hour boundary behavior.
- Covered CIC with optional numeric PCA enabled and native UNSW both with and without IP/port identifiers. Verified ESP hexadecimal dsport is retained as 538989345 and the internally spaced ct_src_ltm header is normalized. No NF extreme-column code is present in CIC or native UNSW.

**Rerun instructions and limits:** Start in a fresh kernel and run optional GPU startup before sklearn/fastai imports, then Setup to define format_duration. Reload/sample/split/refit/export from source to apply the 100,000-per-class cap; existing prepared outputs are not retroactively capped. Resume compatible prepared runs by setting RESUME_OUTPUT_DIR and running imbalance settings, both utility cells and checkpoint restore; regenerate stale checkpoints explicitly. Enable RUN_THRESHOLD_TESTS=True for model comparisons and RUN_PREPARED_INSPECTIONS=True only for optional inspections when their original inputs are available. Native UNSW's new categorical audit needs a new export to create its raw baseline. Update downstream readers for renamed time columns. Full source datasets, live GPU acceleration and full-data runtime/peak RAM/model quality were not tested. Validation used synthetic data; tests load full prepared train/validation matrices, and checkpoints add disk/write cost. Saved HTML uses Plotly CDN. Preserved exported Kaggle settings may still differ from settings changed only in a current Kaggle draft.

### 2026-11-15 15:17:51+02:00 — Simplify NF_TON_IOT imbalance cells and format elapsed time

**Timezone:** Africa/Cairo.

**Notebook:** `Notebooks/Fork of NF-Preprocessing (NF_TON_IOT).ipynb`.

**Other files:** `README.md`.

**Changes:**

- Simplified the required class-imbalance workflow from 532 to 327 code lines and reduced its shared helpers from eight to four: batched matrix loading, generated-row previews, prepared-file fingerprinting and atomic checkpoint saving. Removed the timer context wrapper and settings/load/selection wrappers. Selection and checkpoint compatibility checks are now visible directly in their stage cells; consolidated repeated matrix loading and imports. Moved the optional saved-dtype/outlier inspections into Testing behind RUN_PREPARED_INSPECTIONS=False, preserving their functionality.
- Retained the 100,000-per-class cap, automatic 0.5 selection/target policy and 3% tolerance, full ADASYN checkpoints and source/settings checks, RAM guard, binary Label/text Attack mapping, genuine synthetic previews, separate Tomek/table/Plotly cells, optional GPU startup and baseline plus 0.25/0.5/0.75 MLP comparisons. Existing ADASYN checkpoints and compatible model results remain reusable. Drive upload code remains removed.
- Added one small shared format_duration function in Setup. All notebook-generated elapsed-time prints and reports now use hours/minutes/seconds, e.g. 01h 12m 08.4s, including preprocessing fitting, batched exports, restored ADASYN timings, main sampling summaries, prepared-data threshold metrics and legacy experiment fitting. Numeric seconds remain internal to arithmetic and checkpoint state.
- Main timing CSV uses Time in place of Seconds. Threshold metrics use ADASYN time, Tomek time, Training time, Compute time and wall-time fields; legacy comparison results use fit_time instead of fit_seconds. Compatible older numeric-second model metrics are converted on reuse and written back in the new display format. Updated notebook guidance and the generated output README. Saved notebook/Kaggle metadata is unchanged; no other fork was edited.

**Validation:**

- Validated notebook schema and compiled all 67 code cells. Confirmed existing split, encoding, categorical-audit and GPU-startup cells plus top-level metadata remain unchanged.
- Ran actual synthetic NF preprocessing and batched export, and compared old/new ADASYN and Tomek matrices/targets exactly. Verified balanced export and binary Label/text Attack preservation.
- Tested fresh-session checkpoint restore, unchanged checkpoint format, stale settings/source/feature rejection, interrupted-write preservation, portable content signatures and the RAM guard. Checked automatic selection and the 3% tolerance boundary behavior.
- Ran all four 30-epoch prepared-data MLP cases and verified saved-model accuracy, shared 0.5 ADASYN checkpoint reuse and completed-result reuse without retraining, including conversion of older numeric-second metrics. Exercised the legacy MLP comparison timing. Checked duration formatting at minute/hour boundaries and beyond 99 hours, plus formatted main/threshold timing tables and exports.

**Rerun instructions and limits:** Reload the updated notebook and run the Setup/imports cell to define format_duration before running timed cells. Existing prepared outputs/checkpoints can be retained; run imbalance settings, both utility cells and checkpoint restore before continuing the numbered stages or threshold tests. Optional prepared inspections require RUN_PREPARED_INSPECTIONS=True and the original outlier_cols for the outlier report. Regenerate reports to replace earlier second-based columns; update downstream code that reads their former names. Full-dataset execution and live GPU execution were not repeated; validation used synthetic data. Preserving exported Kaggle metadata does not synchronize changes made only in an unsaved Kaggle draft.

### 2026-11-15 14:39:41+02:00 — Remove automatic Drive uploads from NF_TON_IOT

**Timezone:** Africa/Cairo.

**Notebook:** `Notebooks/Fork of NF-Preprocessing (NF_TON_IOT).ipynb`.

**Other files:** `README.md`.

**Changes:** Removed the automatic Drive backup setup, rclone installation option, credentials/upload helper, retry cells and all 24 upload calls at the user's request. Updated notebook guidance and the generated output README to describe local saving. Local prepared datasets, ADASYN checkpoints, previews, reports, models and plots continue to save under OUTPUT_DIR. Retained the original Colab mount/input-output paths, 100,000-per-class cap, 0.5 ADASYN policy, 3% tolerance, checkpoint reuse, GPU option and baseline/threshold model comparisons. Other forks are unchanged.

**Validation:** Validated notebook schema and compiled all 66 remaining code cells. Checked that no automatic-upload settings, helper calls or rclone references remain. Compared the retained executable code against the previous fork: only upload calls and documentation strings changed. Executed local atomic checkpoint save/reload without any Drive helper or configuration and verified matrix equality.

**Rerun instructions and limits:** Reload/import the updated notebook; no rclone installation or Kaggle Drive secret is required. Existing local outputs/checkpoints can still be reused. Existing generated README files are updated when their export cell runs again. Previously uploaded Drive files are not deleted. Full preprocessing/model runs were not repeated because their executable logic is unchanged.

### 2026-11-15 14:23:07+02:00 — Add NF_TON_IOT partial balancing, checkpoints, Drive backup and model comparisons

**Timezone:** Africa/Cairo.

**Notebook:** `Notebooks/Fork of NF-Preprocessing (NF_TON_IOT).ipynb`.

**Other files:** `README.md`.

**Changes:**

- Reduced the per-Attack sample cap to 100,000 before splitting, retaining every row from smaller classes. Main ADASYN now automatically selects classes below 0.5 of the largest prepared training class and requests 0.5 of that count; the existing 3% tolerance applies around this requested target. Unselected classes remain available for neighbour searches. Attack remains the resampling/model target and Label remains binary.
- Save a full, uncompressed ADASYN checkpoint atomically before Tomek Links. Matching restores skip working-matrix loading and ADASYN, restore counts/schema/target mapping/generated previews, and label original cached compute times. SHA256 of prepared training content, feature order, seed, cap, policy and package versions guard reuse; content-based signatures allow copied runs to resume. Disabled the existing output-folder deletion cell so it cannot erase saved checkpoints. Kept Tomek, timing tables and Plotly in separate cells, without periodic still-running messages.
- Added optional cuML acceleration before sklearn/fastai imports, with visible CPU fallback and cuML info dispatch logs. No GPU dependencies are automatically installed. Other supported preprocessing estimators may also accelerate; actual GPU execution depends on compatible installed RAPIDS/CUDA and supported operations.
- Added automatic Drive copying after each completed local export/checkpoint/report/model. Kaggle uses an authorized rclone remote and private `RCLONE_CONFIG` secret; the destination is configurable with `DRIVE_PARENT`/`DRIVE_REMOTE`. Colab uses mounted Drive. Setup/transfer errors preserve local files, record pending uploads and allow a final retry; credentials are held in temporary files outside output directories. Added an opt-in rclone installation cell.
- Added an independent, opt-in prepared-data test block: unresampled baseline plus paired selection/target ratios 0.25/0.25, 0.5/0.5 and 0.75/0.75. All four use the same full prepared training matrix, untouched prepared validation data and MLP configuration (one 32-unit ReLU layer, Adam, learning rate 0.001, alpha 1e-4, seed SEED, 30 epochs). Test and zero-day files are excluded from these comparisons. Equal epochs permit more optimizer updates for larger resampled training sets.
- Save per-case class counts, genuine synthetic encoded-column previews, sampler/training timings, matrix size, accuracy, balanced accuracy, macro/weighted F1, per-Attack precision/recall/F1, confusion matrices, models and comparison tables. Compatible completed results and ADASYN checkpoints are reused with explicit cache labels. Sampling failures are reported without silently substituting another sampler. Updated notebook guidance and generated output README; preserved existing categorical diagnostics and legacy raw-data benchmarks. Other notebooks/forks are unchanged.

**Validation:**

- Validated notebook schema and compiled all 69 code cells.
- Ran the actual NF feature transformations, fitted preprocessing, batched Parquet export, automatic partial-class ADASYN, Tomek, summaries, Plotly export and balanced export on synthetic NF data. Verified binary Label/text Attack preservation and that upload hooks receive closed, readable files.
- Verified a fresh-session checkpoint restore reproduces the same ADASYN matrix and Tomek result without the main loading cell, and that copied prepared content retains its signature. Tested rejection of changed source/settings/features, interrupted checkpoint writes preserving the previous complete file, no-op ADASYN, empty encoded-column previews and singleton-class errors.
- Executed all four 30-epoch MLP cases on the same synthetic prepared inputs, reproduced saved validation accuracy from saved models, and verified completed-result reuse skips training. The 0.5 comparison reused the main ADASYN checkpoint.
- Mock-tested missing Drive setup, successful/failed transfers, local-file retention and credential permissions/removal, plus CPU fallback and the GPU fresh-kernel guard. No live GPU or Google Drive transfer was available for validation.

**Rerun instructions and limits:**

- Restart the kernel, run startup first, and rerun the preprocessing pipeline from source to regenerate the 100,000-per-class sample and prepared splits. Existing 300,000-cap outputs are not automatically converted. ADASYN requested counts are approximate; inspect actual counts after generation.
- For Drive backup, configure an authorized Google Drive remote once using `rclone config`, store its private config as the enabled Kaggle Secret `RCLONE_CONFIG`, install rclone using the opt-in setup cell if needed, enable Internet, and choose `DRIVE_PARENT`. Credentials must not be committed to Git. Files remain local if setup/upload fails; use the final retry cell after correction. Keep the Kaggle outputs before discarding a session. The retry cell should run while no save cell is writing.
- For fresh-session resume, retain/copy the complete prepared output run, run startup/imports, Drive configuration, dataset/sampling constants and imbalance configuration/helpers, set `RESUME_OUTPUT_DIR`, then run Restore a matching ADASYN checkpoint and continue. Skip optional raw-input inspection cells if raw splits are absent. Set `REUSE_ADASYN_CHECKPOINT=False` to recompute after a mismatch; source/policy changes require appropriate prepared outputs.
- Enable `RUN_THRESHOLD_TESTS=True` to run the model comparison. The paired policies change selection and target together; they are not a selection-only ablation. Select a policy with validation results and reserve test data for final evaluation. Stored compute durations, current wall times, result reuse and ADASYN reuse are distinguished.
- Full-dataset runtime, peak RAM, model quality, native GPU execution and live Drive authentication/upload were not measured. Tests load full prepared training and validation matrices and can be expensive; checkpointing adds disk/write time. Matrix GiB is not peak process RAM. CPU/GPU numerical and neighbour-tie differences can affect outcomes. Saved HTML plots use the Plotly CDN.

### 2026-11-15 12:14:09+02:00 — Preserve ESP SPI values in the UNSW destination-port slot

**Timezone:** Africa/Cairo.

**Notebook:** `Notebooks/UNSW-Preprocessing.ipynb`.

**Other files:** `README.md`.

**Changes:**

- Corrected the overly broad 16-bit port validation after the user's diagnostic showed two `proto=esp` rows with `dsport=0x20205321` (538989345).
- Verified the Argus client source: ArgusPrintDstPort routes ESP records to ArgusPrintEspSpi using the ESP flow SPI. RFC 4303 defines SPI as a 32-bit field. These values are consistent with protocol-specific export semantics rather than a transport port.
- Permit 0–4294967295 for dsport only on ESP rows (case-insensitive esp or protocol number 50); preserve the parsed integer and its existing categorical encoding. Keep the 0–65535 bound for source ports and other destination-port rows, with existing missing/fractional/malformed-value handling.
- Include offending protocol/raw/parsed examples in range errors. Updated notebook guidance and the generated output README. No clipping, deletion, imputation or new feature columns were introduced.
- Sources: [Argus client implementation](https://github.com/openargus/clients/blob/main/common/argus_util.c), [RFC 4303 section 2.1](https://www.rfc-editor.org/rfc/rfc4303.html#section-2.1). Numeric consistency does not establish that every source packet/record is valid.

**Validation:**

- Tested the actual casting helper with the reported hexadecimal value, decimal/hexadecimal 32-bit limits, ESP spelling/protocol-code variants, mixed ESP/TCP/UDP rows, missing values and idempotent recasting.
- Verified that values above 65535 still fail for TCP/UDP/ICMP destination slots and all source slots, while negative/fractional and above-32-bit ESP values fail. Ordinary valid ports remain accepted.
- Ran the native binary category encoder on mixed port/SPI/missing inputs; its output was finite. Verified integer storage and preservation of Label/Attack.
- Validated notebook schema and compiled all 48 code cells. The full source dataset and full model/sampler runs were not repeated.

**Rerun instructions:**

- Reload the updated type-conversion definition and rerun Load and inspect to read the raw Parquet again. The two reported ESP values should be retained as 538989345, not replaced with missing or clipped.
- If using the earlier generic diagnostic snippet, its 65535-only check will still flag ESP SPI values; interpret that table by protocol or apply the same ESP-specific bound.

### 2026-11-15 11:51:04+02:00 — Normalize internal whitespace in UNSW headers

**Timezone:** Africa/Cairo.

**Notebook:** `Notebooks/UNSW-Preprocessing.ipynb`.

**Other files:** `README.md`.

**Changes:**

- Remove all column-name whitespace before lowercasing and alias lookup, correcting the source spelling `ct_src_ ltm` to the existing integer feature `ct_src_ltm`.
- Retain duplicate-name rejection after normalization and explicit errors for genuinely unknown fields. Feature values, target normalization and downstream preprocessing are unchanged.
- Updated the column-type guide; preserved all earlier README history.

**Validation:**

- Ran the actual casting helper on synthetic string inputs with the reported internal space, surrounding spaces, tabs and non-breaking spaces. Results matched canonical-header inputs, including nullable integer values and the requested Label/Attack targets.
- Verified duplicate headers created by normalization are rejected and genuinely unknown fields still raise errors.
- Validated notebook schema and compiled all 48 code cells.

**Rerun instructions and limits:**

- Reload the updated notebook, rerun the type-conversion definition cell, then rerun the Load and inspect cell so it reloads the raw Parquet. A full restart is unnecessary for this header-only fix.
- Full source-data execution was not repeated; the validation covers the reported header and synthetic variants.

### 2026-11-15 11:22:01+02:00 — Match UNSW binary and attack-type targets to the NF format

**Timezone:** Africa/Cairo.

**Notebook:** `Notebooks/UNSW-Preprocessing.ipynb`.

**Other files:** `README.md`.

**Changes:**

- Enforced the requested target format before sampling: `Label` is uint8 with 0=benign and 1=attack; `Attack` is text containing `BENIGN` or the specific attack name.
- Normalized Normal/Benign spellings case-insensitively to BENIGN, and filled a blank benign name only for Label=0. Named attacks are retained; missing attack names on Label=1 and contradictory Label/Attack pairs raise clear errors instead of silently relabelling rows.
- Added a visible Attack-by-Label crosstab after loading and updated the type-casting guide and generated output README. Main loading and independent Testing share the same normalization. Split exports preserve both targets; balanced export continues to restore binary Label from the original Attack-to-Label mapping.
- Preserved all prior README history and the existing feature transformations, split/holdout settings and sampling algorithms.

**Validation:**

- Validated the notebook schema and compiled every code cell.
- Tested Normal/Benign variants, whitespace, blank benign names, specific attack names, binary uint8 storage and rejection of contradictory targets.
- Ran synthetic native-UNSW preprocessing with and without optional IPs/ports; verified Label/Attack agreement in train, validation, test and the independent Testing loader.
- Executed the actual prepared-training loader, ADASYN, Tomek Links and balanced export on balanced and imbalanced synthetic inputs. Covered skipped ADASYN and actual generated rows; all saved target pairs followed the requested format and retained the specific attack names.

**Rerun instructions and limits:**

- Run from Setup after pulling/importing the updated PR branch to regenerate the preprocessor and saved datasets. BENIGN replaces the previous Normal spelling; use BENIGN in downstream class filters.
- The full source dataset was unavailable. Validation used synthetic data and does not establish full-data runtime, RAM use or accuracy.

### 2026-11-15 10:55:23+02:00 — Restore native UNSW-NB15 types before preprocessing

**Timezone:** Africa/Cairo.

**Notebook:** `Notebooks/UNSW-Preprocessing.ipynb`.

**Other files:** `README.md`.

**Changes:**

- Corrected this notebook's copied NF-ToN-IoT dataset configuration to `UNSW-NB15` / `UNSW-NB15.parquet`. Native UNSW and NF-UNSW use different feature schemas; the NF notebook remains the workflow for the NetFlow extractor variant.
- Added an explicit UNSW schema conversion before inspection, sampling and fitted preprocessing. Measurements use float64, integer-valued fields use the smallest fitting signed integer (nullable when missing), protocol/service/state use pandas categories, and IPs use strings. Label remains binary uint8 and Attack remains text. No raw float shrinking is applied.
- Normalized case, whitespace, target names and supported native aliases (including attack_cat, Sintpkt, smeansz and res_bdy_len). Supported hexadecimal source/destination ports, preserving them as integer storage and treating them categorically downstream.
- Recognized documented missing tokens in numeric fields, preserved the service category `-`, and filled blank Attack with Normal only for Label=0. Unknown fields require reviewed UNSW_TYPE_OVERRIDES entries; malformed numeric strings, fractional counts, out-of-range ports, missing/invalid targets and duplicate normalized column names raise explicit errors. Exact checks on decimal/scientific integer tokens prevent fractional parts from disappearing through float rounding. Integer parsing deliberately rejects values beyond 2**53-1.
- Replaced NF-only flag/ICMP/category references with native UNSW one-hot protocol/service/state and binary-port encoding. Preserved the experimental ordinal/scaler/PCA IP representation when IP columns exist; omit that branch for variants without IPs. Updated coverage diagnostics, feature selection, saved schema, exported README and independent Testing loader/comparison feature selection accordingly. Learned transformations still fit training data only.
- Displayed before/after types and source missing counts, and saved type_conversion_report.csv alongside prepared outputs. Existing sampling cap, split/holdout settings, scaler choices, imbalance settings and sampler calls are retained. No other notebook is edited. Older README history is preserved verbatim.

**Validation:**

- Validated notebook JSON against its nbformat schema and compiled all code cells with IPython magic handling.
- Exercised string-only synthetic inputs covering every mapped numeric field, large TCP sequence integers (4294967295), hexadecimal/missing ports, float missing markers, category missing values, service `-`, blank normal Attack, native aliases, idempotent recasting and explicit extra-column roles. Verified rejection of malformed numbers, fractional counts (including a fractional token that float parsing would round to 1), invalid labels/ports, unknown fields, duplicate names and missing attack targets.
- Executed the notebook's load, sampling/distribution comparison, cleanup, split, outlier reporting, fit, category coverage, Arrow schema and export cells on synthetic variants both with and without IPs/ports. Checked prepared numeric values against direct fitted transformations, finite outputs, target/feature separation, uint8 Label, optional PCA, saved conversion reports and joblib reload equivalence. Tested with StandardScaler and the configured quantile-to-normal scalers.
- Executed the independent Testing loader on both variants and confirmed numeric targets/measurement columns. Full neural-network comparisons and ADASYN/Tomek runs were not repeated because their sampling algorithms are unchanged.

**Rerun instructions and limits:**

- Restart/import the updated notebook and run from Setup, including the new type-conversion definition cell before Load and inspect or independent Testing. Regenerate the preprocessor and prepared train/validation/test outputs; earlier NF-configured artifacts are incompatible with the corrected native UNSW flow.
- Pass future raw inputs through cast_unsw_types before the saved preprocessor so names/types match. Do not transform already prepared Parquet files again. Review any unmapped source-specific column explicitly instead of guessing its role.
- The user's full source Parquet was unavailable here. Validation used synthetic original-UNSW schemas, so full-data values, runtime, RAM requirements and model accuracy remain unverified. The workflow still uses a fresh 70/15/15 split rather than preserving an externally supplied official train/test partition. Identifier PCA remains experimental and should be compared with dropping IPs.

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





