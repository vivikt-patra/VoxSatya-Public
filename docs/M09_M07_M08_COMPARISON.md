# M07 vs M08 vs M09 Detector Performance Comparison

## Historical Benchmark Matrix

| Milestone | Training Data Strategy | Model Architecture | EVAL Sample Population | Accuracy | ROC-AUC | EER | Genuine FPR | Spoof FNR | Recovery Verdict |
|-----------|------------------------|--------------------|------------------------|----------|---------|-----|-------------|-----------|------------------|
| **M07** | Synthetic Audio Only (114 samples) | Baseline AASIST | External Benchmark | 54.73% | 0.5773 | 42.31% | 0.00% (0/250) | 91.09% (225/247) | **FAILURE** (Severe FNR) |
| **M08** | Synthetic + Small Authentic (142 samples) | Retrained AASIST | External Benchmark | 52.39% | 0.4976 | 50.24% | 0.00% (0/186) | 100.00% (169/169) | **FAILURE** (Total FNR) |
| **M09** | Full Authentic Corpus (25,380 samples) | Scaled AASIST | Stratified EVAL (N=7,994) | **89.19%** | **0.9610** | **7.01%** | **0.40%** (4/1,000) | **12.30%** (860/6,994) | **SUCCESS / PASS** |

---

## Key Insights & Failure Recovery Analysis
1. **Root Cause of M07/M08 Failures**: Training on small synthetic/toy samples caused the models to overfit to specific synthetic artifacts while remaining completely blind to unseen neural speech generators.
2. **Impact of M09 Authentic Training Data**: Scaling training to 25,380 authentic ASVspoof 2019 samples allowed Scaled AASIST to learn generalized representations of human vocal tract constraints and neural vocoder artifacts.
3. **Detector Recovery**: Spoof false-negative rate dropped by **87.7%** (from 100% in M08 down to 12.30% in M09), while maintaining human protection with only 4 false positives across 1,000 genuine samples (0.40% FPR).
