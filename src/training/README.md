# src/training

Vòng train dùng chung, early stopping, lưu checkpoint. Entry point là `src/train.py`; không train trong notebook.

**What belongs here:** `trainer.py` (vòng train/val, gradient clipping, early stopping), `train_baseline.py`.

**What does not:** Tính metric test, đọc argument dòng lệnh (thuộc `src/train.py`).

Đặc tả: [docs/phases/TRAINING.md](../../docs/phases/TRAINING.md)
