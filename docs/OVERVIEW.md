# OVERVIEW.md — Lộ trình & Trạng thái dự án

- **Motivation/Background**: Cần một chỗ duy nhất để cả nhóm biết dự án đang ở phase nào, ai làm gì, checkpoint nào là tốt nhất.
- **Purpose**: Lộ trình sống của dự án. Cập nhật đầu mỗi buổi làm việc.
- **Overview Pipeline**: DATA_PREP → BASELINE → MODEL → TRAINING → EVAL → DEMO_REPORT (+ mở rộng nếu kịp).
- **Detailed Plan**: §1 Roadmap theo phase; §2 Phần mở rộng; §3 Kiến trúc; §4 Checkpoint; §5 Ràng buộc phần cứng.
- **References**: [PURPOSE.md](PURPOSE.md) §10, [phases/](phases/), [progress/](progress/).
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## 1. Roadmap theo phase

Tuần theo [PURPOSE.md](PURPOSE.md) §10 (6 tuần).

| # | Phase | Đặc tả | Tiến độ | Người chính | Tuần | Trạng thái |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Chuẩn bị dữ liệu & split | [DATA_PREP](phases/DATA_PREP.md) | [status](progress/DATA_PREP_STATUS.md) | TV1 | 1 | To Do |
| 2 | Baseline Majority + TF-IDF + LR | [BASELINE](phases/BASELINE.md) | [status](progress/BASELINE_STATUS.md) | TV1 | 1 | To Do |
| 3 | Định nghĩa mô hình | [MODEL](phases/MODEL.md) | [status](progress/MODEL_STATUS.md) | TV2 | 2–3 | To Do |
| 4 | Huấn luyện (3 seed) | [TRAINING](phases/TRAINING.md) | [status](progress/TRAINING_STATUS.md) | TV2 | 2–3 | To Do |
| 5 | Đánh giá, phân tích lỗi, giải thích | [EVAL](phases/EVAL.md) | [status](progress/EVAL_STATUS.md) | TV3 | 4 | To Do |
| 6 | Demo, README, báo cáo & slide | [DEMO_REPORT](phases/DEMO_REPORT.md) | [status](progress/DEMO_REPORT_STATUS.md) | TV3, cả nhóm | 5–6 | To Do |

## 2. Phần mở rộng (tuần 5, nếu kịp)

| ID | Việc | Thí nghiệm | Người | Trạng thái |
| :--- | :--- | :--- | :--- | :--- |
| E1 | Attention pooling trên BiLSTM | EXP-06 | TBD | To Do |
| E2 | Ablation `max_len` 200 / 400 / 600 | EXP-07 | TBD | To Do |
| E3 | Fine-tune DistilBERT (mốc tham chiếu) | EXP-08 | TBD | To Do |
| E4 | Đánh giá chéo Rotten Tomatoes | EXP-09 | TBD | To Do |

## 3. Kiến trúc

```
review text → clean → tokenize → ids [B, max_len] + lengths [B]
→ Embedding [B, L, 100] → pack → LSTM/GRU (uni/bi) → h_last [B, 128 or 256]
→ Dropout → Linear → logit [B, 1]
```

Chi tiết: [MODELS.md](MODELS.md).

## 4. Checkpoint

| Mô hình | Val acc (mean ± std) | Test acc (mean ± std) | Đường dẫn |
| :--- | :--- | :--- | :--- |
| TBD | — | — | `experiments/checkpoints/<exp>/` |

## 5. Ràng buộc phần cứng

- Máy cá nhân có thể không có GPU: chạy thử với tập con nhỏ, train thật trên Colab/Kaggle.
- Demo phải chạy trên CPU, < 1 giây/review.
- Ghi phần cứng, thời gian train và thời gian inference vào từng báo cáo thí nghiệm.
