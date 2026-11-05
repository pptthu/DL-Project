# TRAINING.md — Huấn luyện

- **Motivation/Background**: Mọi thí nghiệm cần cùng một vòng train để kết quả so sánh được; số phải tái lập được qua 3 seed.
- **Purpose**: Vòng train dùng chung cho mọi mô hình RNN, có early stopping, lưu checkpoint tốt nhất, chạy 3 seed.
- **Overview Pipeline**: Phạm vi → đặc tả → artifact → kiểm chứng.
- **Detailed Plan**: Header; Goals; Input/Output; How to do it; Detailed plan / gotchas; Verification gates; Links.
- **References**: [OVERVIEW.md](../OVERVIEW.md), [PURPOSE.md](../PURPOSE.md) §4 (M6), §7.
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## Header

- **Title:** Huấn luyện
- **Date created:** 2026-11-05
- **Last updated:** 2026-11-05
- **Description:** Vòng train dùng chung cho mọi mô hình RNN, có early stopping, lưu checkpoint tốt nhất, chạy 3 seed.
- **Status:** To Do
- **Owner:** TV2
- **Tuần:** 2–3

## Goals / Purpose

- Xong: `python -m src.train --config configs/<exp>.yaml --seed <s>` chạy được; lưu `best.pt` theo val loss; log loss/acc mỗi epoch; mỗi cấu hình có 3 seed và file mean ± std.
- Mốc tuần 2: LSTM bắt buộc đạt ≥ 85% trên val.
- Không làm: tìm siêu tham số tự động diện rộng.

## Input / Output

- **Input:** `data/splits/`, `configs/<exp>.yaml`.
- **Output:**
  - `experiments/runs/<exp>/seed_<s>/` (log, `history.csv`).
  - `experiments/checkpoints/<exp>/vocab.json`, `experiments/checkpoints/<exp>/seed_<s>/best.pt`.
  - `experiments/results/<exp>/seed_<s>/metrics.json` (val), `experiments/results/<exp>/summary.json` (mean ± std 3 seed).

## How to do it (general plan)

1. Dataset/DataLoader (`src/data/dataset.py`): tokenize, map id, cắt/đệm `max_len`, trả về độ dài thật.
2. `BCEWithLogitsLoss`, Adam (lr 1e-3), gradient clipping 5.0 (`src/training/`).
3. Mỗi epoch: `model.train()` → train; `model.eval()` + `torch.no_grad()` → val.
4. Early stopping theo val loss (patience 3); lưu checkpoint tốt nhất kèm config.
5. Chạy seed 42, 123, 2025; gộp mean ± std vào `summary.json`.

## Detailed plan / gotchas

- Cố định seed (`torch`, `numpy`, `random`) qua `src/utils/seed.py`; split không đổi theo seed.
- Không đánh giá test trong vòng train.
- Ghi phần cứng, thời gian mỗi epoch và tổng thời gian train.

## Verification gates

- [ ] Chạy 1 epoch trên tập con 500 mẫu không lỗi.
- [ ] Load `best.pt` cho đúng val acc đã log.
- [ ] Chạy lại cùng config + seed lệch ≤ 1% (M6).

## Links

- Progress tracking: [../progress/TRAINING_STATUS.md](../progress/TRAINING_STATUS.md)
- Related: [OVERVIEW.md](../OVERVIEW.md)
