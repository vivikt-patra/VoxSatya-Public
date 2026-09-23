# CHANGELOG

## [Milestone 09] - 2026-09-06
### Added
- Acquired full official ASVspoof 2019 LA dataset (25,380 TRAIN, 24,844 DEV, 71,237 EVAL).
- Trained Scaled AASIST (`M09-A`), LFCC Control (`M09-B`), and Wav2Vec2 SSL (`M09-C`) models on authentic speech.
- Executed DEV Model Tournament: Selected `M09-A Scaled AASIST` (DEV EER 0.06%).
- Derived Platt Scaling calibration parameters (`a=1.038813`, `b=-2.605226`) and decision thresholds on DEV split.
- Produced Pre-EVAL Freeze Manifest (`docs/M09_PRE_EVAL_FREEZE.md`) prior to unsealing EVAL.
- Built deterministic 7,994-sample SIH EVAL manifest (`experiments/M09-detector-recovery/m09_sih_eval_manifest.json`) covering all 13 unseen attacks A07–A19.
- Evaluated frozen detector on SIH EVAL split: EER **7.01%**, ROC-AUC **0.9610**, Accuracy **89.19%**, Genuine FPR **0.40%**, Spoof FNR **12.30%**.
- Validated Track B NVIDIA Nemotron 3.5 Assistant integration with live API authentication, authority boundary isolation, and secret exposure audit.

### Changed
- Promoted M09 Detector Recovery status to **PASS**.
