# TrustGuard-AIGC

Evaluation artifacts accompanying **“TrustGuard-AIGC: Provenance-Consistent Risk Scoring for Deepfake Verification”** by Prakash Laxman Kharvi and Andrew O. Hall.

TrustGuard-AIGC evaluates whether media is consistently represented by its detector, provenance, disclosure, registry, source, and transmission evidence. It separates representation risk from evidence coverage and supports abstention when evidence is insufficient or uncertain.

## Release contents

The camera-ready release is organized into two downloadable archives.

### `ffpp_xception_results.zip`

FaceForensics++ Xception detector evaluation artifacts:

- Training, validation, and test frame-level predictions.
- Aggregated video-level scores.
- Evaluation logs and configuration files.
- Dataset metadata and split information.
- A balanced TrustGuard media catalog.
- The DeepfakeBench commit identifier.

### `TrustGuard_AIGC_Real_Pilot_Fusion_Results.zip`

Controlled TrustGuard fusion evaluation artifacts:

- A 480-row feature matrix with source-grouped out-of-fold predictions.
- Baseline comparisons and feature ablations.
- Scenario-specific and transformation-specific results.
- Abstention results and source-group bootstrap confidence intervals.
- Evaluation methodology and fold details.
- Final pilot model coefficients and threshold.
- Result figures and a detailed results README.

## Evaluation scope

The fusion pilot contains **480 correlated logical scenarios**, constructed from **80 transformed media variants**, **20 base videos**, and **10 FaceForensics++ source groups**.

Five-fold outer grouped evaluation holds out complete source groups. Four-fold inner grouped validation selects regularization and decision thresholds.

The evaluation is **source-held-out but scenario-familiar**: test folds contain held-out sources, while threat-condition types remain familiar. These results demonstrate controlled feasibility, not population-level or production validation.

## Primary fusion results

| Metric | Result |
|---|---:|
| AUROC | 0.9974 |
| AUPRC | 0.9981 |
| Accuracy | 0.9813 |
| Precision | 0.9788 |
| Recall | 0.9893 |
| F1 | 0.9840 |
| False-positive rate | 0.0300 |

The 95% source-group bootstrap confidence intervals are **0.9952–0.9996** for AUROC and **0.9787–0.9894** for F1.

These are the primary nested source-grouped out-of-fold results. The separately labeled `group_cv_threshold_metrics` in `final_fusion_model.json` report different threshold-selection results and should not replace the primary evaluation metrics.

## Abstention

The reported pilot policy abstains when evidence coverage is below **0.50** or the risk probability lies within **[0.40, 0.60]**.

| Measure | Result |
|---|---:|
| Decided scenarios | 312 / 480 |
| Abstained scenarios | 168 / 480 |
| Decision rate | 65.0% |
| Decided-row F1 | 0.9938 |
| Decided-row false-positive rate | 0.0132 |
| Classification errors captured by abstention | 77.8% |

These thresholds illustrate one conservative operating point. They are not proposed as universal deployment settings.

## Error analysis and limitations

All nine primary full-model classification errors occurred under heavy H.264 re-encoding, T2 (CRF 40):

- Three false negatives involved undisclosed synthetic media.
- Five false positives involved valid human-derived assets with ingredient chains.
- One false positive involved a valid human-origin assertion.

Uncredentialed human-origin scenarios had no false positives in this pilot.

Removing transmission risk or contradiction features slightly improved F1. The pilot therefore does not establish an independent incremental benefit for either feature.

The fitted coefficients are specific to this controlled pilot. Substantially different deployment populations require recalibration or retraining.

## Using the artifacts

1. Download both ZIP files from the repository’s **Releases** page.
2. Extract them into separate directories.
3. Read `trustguard_fusion_pilot_results/README_RESULTS.md`.
4. Consult `evaluation_method.json` and `fold_details.json` for the evaluation design.
5. Use `trustguard_grouped_oof_predictions.csv` to inspect features, labels, risk scores, decisions, and abstentions.
6. Compare aggregate results with the supplied CSV and JSON summaries.

CSV, JSON, JSONL, YAML, and Markdown files can be inspected using standard data-analysis and text-editing tools.

Any dataset paths in the metadata identify the original experimental layout; they do not provide access to the underlying media. Adapt these paths to your own authorized dataset installation.

## Reproducibility boundaries

This release supports inspection and reanalysis of saved detector and fusion outputs. It is **not a complete executable end-to-end reproduction package**.

The current archives do not include:

- C2PA signing and scenario-generation scripts.
- Executable fusion training and evaluation scripts.
- Trained detector checkpoint files.
- Celeb-DF-v2 evaluation outputs.
- Leave-one-condition-out experimental results.
- Unconstrained logistic-regression or rule-based fusion baseline results.
- A coverage-versus-error trade-off curve.

The saved fusion predictions include evidence coverage and risk probabilities, which can support further analysis of abstention policies.

## Dataset access

Raw FaceForensics++ and Celeb-DF-v2 videos are not redistributed.

Researchers must obtain benchmark media independently through the respective dataset providers and follow their access and usage conditions. This repository does not grant rights to third-party datasets, software, or model weights.

## Citation

Please cite the accompanying TrustGuard-AIGC paper when using these artifacts. Final publication details and an archival DOI will be added when available.

For traceability, identify the exact release tag or archived version used in your work.

## License

See the repository’s `LICENSE` file for any reuse permissions granted for the authors’ original materials. Third-party materials remain subject to their respective terms. Public availability alone does not grant additional reuse rights.
