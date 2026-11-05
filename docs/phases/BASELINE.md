# BASELINE.md — Baseline Majority + TF-IDF + Logistic Regression

- **Motivation/Background**: Cần biết LSTM có thật sự tốt hơn phương pháp cổ điển hay không.
- **Purpose**: Dựng mốc sàn (majority class) và mốc cổ điển mạnh (TF-IDF + LR), không dùng deep learning.
- **Overview Pipeline**: Phạm vi → đặc tả → artifact → kiểm chứng.
- **Detailed Plan**: Header; Goals; Input/Output; How to do it; Detailed plan / gotchas; Verification gates; Links.
- **References**: [OVERVIEW.md](../OVERVIEW.md), [PURPOSE.md](../PURPOSE.md) §7.
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## Header

- **Title:** Baseline Majority + TF-IDF + Logistic Regression
- **Date created:** 2026-11-05
- **Last updated:** 2026-11-05
- **Description:** Dựng mốc sàn và mốc cổ điển mạnh, không dùng deep learning.
- **Status:** To Do
- **Owner:** TV1
- **Tuần:** 1

## Goals / Purpose

- Xong: có số val của M0a (majority) và M0b (TF-IDF + LR); mô hình M0b lưu ở `experiments/checkpoints/`; báo cáo `docs/experiments/EXP_01_BASELINE.md`.
- Không làm: tối ưu baseline quá mức (chỉ tìm `C` trên val).

## Input / Output

- **Input:** `data/splits/train.csv`, `val.csv`.
- **Output:** `experiments/checkpoints/baseline_tfidf_lr.joblib`, `experiments/results/baseline/metrics.json`.

## How to do it (general plan)

1. M0a: đoán nhãn nhiều nhất trong train; ghi metric val.
2. M0b: `TfidfVectorizer(ngram_range=(1,2), min_df=2)` fit trên train; `LogisticRegression`, tìm `C` trong vài giá trị trên val.
3. Ghi metric val (cùng bộ chỉ số với RNN, xem [PURPOSE.md](../PURPOSE.md) §9); số test chạy ở phase EVAL.

## Detailed plan / gotchas

- Fit vectorizer chỉ trên train.
- Dùng cùng hàm làm sạch văn bản với mô hình RNN (`src/data/clean.py`).
- Baseline tất định nên không cần 3 seed.

## Verification gates

- [ ] `metrics.json` có accuracy, precision/recall/F1 từng lớp, macro-F1, ROC-AUC trên val cho M0a và M0b.
- [ ] M0a val acc ≈ 50%.
- [ ] Load lại file `.joblib` cho cùng dự đoán.

## Links

- Progress tracking: [../progress/BASELINE_STATUS.md](../progress/BASELINE_STATUS.md)
- Related: [OVERVIEW.md](../OVERVIEW.md)
