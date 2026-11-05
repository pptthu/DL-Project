# experiments/README.md — Chỉ mục thí nghiệm

- **Motivation/Background**: Mỗi thí nghiệm đổi đúng một biến so với thí nghiệm trước để biết thay đổi nào tạo ra khác biệt.
- **Purpose**: Bảng chỉ mục mọi thí nghiệm và bảng kết quả so sánh với baseline.
- **Overview Pipeline**: EXP_01 baseline → EXP_02 LSTM → EXP_03 GRU → EXP_04 BiLSTM → EXP_05 BiLSTM + GloVe → (mở rộng) EXP_06–09.
- **Detailed Plan**: §1 Quy tắc; §2 Chỉ mục; §3 Bảng kết quả.
- **References**: [EXPERIMENT_TEMPLATE.md](EXPERIMENT_TEMPLATE.md), [../MODELS.md](../MODELS.md), [../PURPOSE.md](../PURPOSE.md) §6–§7.
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## 1. Quy tắc

- Tên file: `EXP_<NN>_<TEN_NGAN>.md`, copy từ [EXPERIMENT_TEMPLATE.md](EXPERIMENT_TEMPLATE.md).
- Giữ nguyên split, `max_len`, vocab, optimizer giữa các thí nghiệm, trừ biến đang thử.
- Mỗi cấu hình RNN chạy 3 seed (42, 123, 2025); báo mean ± std.
- Số val dùng để chọn; số test chỉ ghi sau phase EVAL.

## 2. Chỉ mục

| ID | Tiêu đề | Biến thay đổi so với trước | Người | Trạng thái | File |
| :--- | :--- | :--- | :--- | :--- | :--- |
| EXP-01 | Baseline Majority + TF-IDF + LR | — | TV1 | To Do | `EXP_01_BASELINE.md` |
| EXP-02 | LSTM | mô hình RNN đầu tiên | TV2 | To Do | `EXP_02_LSTM.md` |
| EXP-03 | GRU | LSTM → GRU | TV2 | To Do | `EXP_03_GRU.md` |
| EXP-04 | BiLSTM | 1 chiều → 2 chiều | TV2 | To Do | `EXP_04_BILSTM.md` |
| EXP-05 | BiLSTM + GloVe | embedding ngẫu nhiên → GloVe (đóng băng / fine-tune) | TV2 | To Do | `EXP_05_BILSTM_GLOVE.md` |
| EXP-06 | (Mở rộng) Attention pooling | hidden cuối → attention pooling | TBD | To Do | `EXP_06_ATTENTION.md` |
| EXP-07 | (Mở rộng) Ablation `max_len` | `max_len` 200 / 400 / 600 | TBD | To Do | `EXP_07_MAXLEN.md` |
| EXP-08 | (Mở rộng) DistilBERT | RNN → Transformer fine-tune | TBD | To Do | `EXP_08_DISTILBERT.md` |
| EXP-09 | (Mở rộng) Rotten Tomatoes | đánh giá chéo, không train lại | TBD | To Do | `EXP_09_ROTTEN_TOMATOES.md` |

## 3. Bảng kết quả

Điền từ `experiments/results/<exp>/`. RNN: mean ± std 3 seed. Không điền số chưa chạy.

| Mô hình | Val acc | Test acc | Test macro-F1 | Test ROC-AUC | Tham số | Thời gian train | Inference (ms/review, CPU) | Nguồn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| M0a Majority | TBD | TBD | TBD | — | 0 | — | — | TBD |
| M0b TF-IDF + LR | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| M1 LSTM | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| M2 GRU | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| M3 BiLSTM | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| M4a BiLSTM + GloVe (frozen) | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| M4b BiLSTM + GloVe (fine-tune) | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
