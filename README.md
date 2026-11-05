# IMDb Sentiment Analysis with LSTM

- **Motivation/Background**: Đồ án môn Học sâu (UTH), chủ đề 3 (nhóm LLMs) — phân loại cảm xúc review phim IMDb.
- **Purpose**: Điểm vào chính của repo: mục tiêu, cấu trúc thư mục, cài đặt, lệnh chạy, kết quả chính.
- **Overview Pipeline**: IMDb raw → làm sạch, loại trùng → split train/val theo phim → baseline Majority, TF-IDF + LR → LSTM / GRU / BiLSTM / BiLSTM + GloVe (3 seed) → đánh giá test, phân tích lỗi, giải thích → demo Gradio.
- **Detailed Plan**: §1 Mục tiêu; §2 Cấu trúc thư mục; §3 Cài đặt; §4 Lệnh chạy; §5 Kết quả; §6 Tài liệu.
- **References**: [docs/PURPOSE.md](docs/PURPOSE.md), [docs/README.md](docs/README.md).
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## 1. Mục tiêu

Phân loại review phim IMDb thành `positive`/`negative` bằng Embedding + LSTM + Linear, so sánh với GRU, BiLSTM, GloVe và baseline TF-IDF + Logistic Regression. Đề bài đầy đủ: [docs/PURPOSE.md](docs/PURPOSE.md).

## 2. Cấu trúc thư mục

```
DL-Project/
├── configs/            # YAML mỗi thí nghiệm (lstm.yaml, gru.yaml, ...)
├── data/               # KHÔNG commit, trừ sample/ — xem data/README.md
│   ├── raw/            # aclImdb_v1 giải nén (+ rotten_tomatoes/ nếu làm E4)
│   ├── splits/         # CSV train/val/test cố định
│   ├── glove/          # glove.6B.100d.txt
│   ├── processed/      # ma trận GloVe đã lọc theo vocab (.npy)
│   └── sample/         # sample_100.csv, handwritten_30.csv (được commit)
├── docs/               # Tài liệu: PURPOSE, OVERVIEW, DATA, MODELS, phases, progress, experiments, bugs
├── experiments/        # KHÔNG commit checkpoint — xem experiments/README.md
├── notebooks/          # EDA, kết quả, phân tích lỗi, demo.ipynb
├── src/
│   ├── train.py        # Entry: train một config với một seed
│   ├── evaluate.py     # Entry: đánh giá checkpoint trên val/test
│   ├── predict.py      # Entry + hàm predict(text) -> (label, prob)
│   ├── data/           # Tải, làm sạch, loại trùng, split, vocab, Dataset/DataLoader
│   ├── models/         # Baseline, SentimentRNN (LSTM/GRU/BiLSTM), nạp GloVe
│   ├── training/       # Vòng train, early stopping, lưu checkpoint
│   ├── eval/           # Metric, phân tích lỗi, giải thích mô hình
│   ├── demo/           # App Gradio
│   └── utils/          # Seed, logging, đường dẫn
└── tests/              # Pytest: shape, overfit 1 batch, save/load
```

## 3. Cài đặt

```bash
python -m venv .venv
.venv\Scripts\activate          # Windows; Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
```

## 4. Lệnh chạy

Lệnh cụ thể ghi vào đây khi code từng phase xong. Thứ tự: tải dữ liệu → chia split → baseline → train (3 seed) → evaluate → demo. Xem [docs/OVERVIEW.md](docs/OVERVIEW.md).

```bash
python -m pytest tests/
python -m src.train --config configs/lstm.yaml --seed 42      # dự kiến
python -m src.evaluate --exp lstm --split val                  # dự kiến
python -m src.predict "not bad at all"                         # dự kiến
python -m src.demo.app                                         # dự kiến
```

## 5. Kết quả

TBD — điền từ `experiments/results/` sau khi chạy (mean ± std 3 seed). Bảng so sánh: [docs/experiments/README.md](docs/experiments/README.md).

## 6. Tài liệu

Mục lục: [docs/README.md](docs/README.md). Prompt để AI tạo/cập nhật từng file `.md`: [docs/MD_PROMPTS.md](docs/MD_PROMPTS.md).
