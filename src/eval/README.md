# src/eval

Metric, phân tích lỗi theo nhóm tình huống, giải thích mô hình. Entry point là `src/evaluate.py` và `src/predict.py`.

**What belongs here:** `metrics.py` (accuracy, P/R/F1 từng lớp, macro-F1, ROC-AUC, confusion matrix), `error_analysis.py` (gán 7 nhóm tình huống), `explain.py` (occlusion, gradient × embedding).

**What does not:** Code train, đọc argument dòng lệnh.

Đặc tả: [docs/phases/EVAL.md](../../docs/phases/EVAL.md)
