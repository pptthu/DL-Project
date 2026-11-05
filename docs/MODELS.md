# MODELS.md — Danh mục mô hình

- **Motivation/Background**: So sánh công bằng cần ghi rõ kiến trúc, số tham số và checkpoint của từng mô hình.
- **Purpose**: Danh mục các mô hình của dự án: kiến trúc, số tham số, vị trí checkpoint, cách load lại.
- **Overview Pipeline**: Majority → TF-IDF + LR → LSTM → GRU → BiLSTM → BiLSTM + GloVe (frozen / fine-tune) → (mở rộng) attention, DistilBERT.
- **Detailed Plan**: §1 Bảng mô hình; §2 Kiến trúc chung; §3 Checkpoint & load lại; §4 Mô hình mở rộng.
- **References**: [PURPOSE.md](PURPOSE.md) §7, [phases/MODEL.md](phases/MODEL.md), `src/models/`.
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## 1. Bảng mô hình

Số tham số đếm bằng code (`sum(p.numel() for p in model.parameters())`), không ước lượng.

| ID | Mô hình | Embedding | RNN | Tham số (tổng / train được) | Checkpoint |
| :--- | :--- | :--- | :--- | :--- | :--- |
| M0a | Majority class | — | — | 0 | — (không cần lưu) |
| M0b | TF-IDF (1–2 gram) + Logistic Regression | — | — | TBD | `experiments/checkpoints/baseline_tfidf_lr.joblib` |
| M1 | LSTM (bắt buộc) | học từ đầu, 100d | LSTM 1 chiều, 128 | TBD | `experiments/checkpoints/lstm/seed_<s>/best.pt` |
| M2 | GRU | học từ đầu, 100d | GRU 1 chiều, 128 | TBD | `experiments/checkpoints/gru/seed_<s>/best.pt` |
| M3 | BiLSTM | học từ đầu, 100d | LSTM 2 chiều, 128 × 2 | TBD | `experiments/checkpoints/bilstm/seed_<s>/best.pt` |
| M4a | BiLSTM + GloVe (đóng băng) | GloVe 100d, `freeze=True` | LSTM 2 chiều, 128 × 2 | TBD | `experiments/checkpoints/bilstm_glove_frozen/seed_<s>/best.pt` |
| M4b | BiLSTM + GloVe (fine-tune) | GloVe 100d, `freeze=False` | LSTM 2 chiều, 128 × 2 | TBD | `experiments/checkpoints/bilstm_glove_ft/seed_<s>/best.pt` |

`<s>` ∈ {42, 123, 2025}. "Checkpoint của 4 mô hình" trong [PURPOSE.md](PURPOSE.md) §8 là M1, M2, M3 và bản tốt hơn trong M4a/M4b.

## 2. Kiến trúc chung (M1–M4)

```
nn.Embedding(V, 100, padding_idx=0)
→ pack_padded_sequence
→ nn.LSTM / nn.GRU(100, 128, num_layers=1, batch_first=True, bidirectional=?)
→ hidden state cuối (BiLSTM: nối h_n[-2] và h_n[-1] → 256)
→ nn.Dropout → nn.Linear(128 or 256, 1) → logit
```

- Sigmoid chỉ áp dụng khi dự đoán; khi train dùng `BCEWithLogitsLoss` trên logit.
- Một class `SentimentRNN` tham số hóa `rnn_type`, `bidirectional`, `pretrained_embeddings`, `freeze` để các mô hình chỉ khác đúng một biến.
- Siêu tham số mặc định: `emb_dim=100`, `hidden=128`, `num_layers=1`, `lr=1e-3`, `clip=5.0`, `patience=3`. `dropout`, `max_len`, `batch_size`: TBD, ghi vào `configs/`.

## 3. Checkpoint & load lại

```
experiments/checkpoints/<exp>/
├── vocab.json            # xây từ train, giống nhau cho mọi seed
└── seed_<s>/best.pt      # state_dict + config, chọn theo val loss
```

- `best.pt` lưu `state_dict` cùng config; load lại cần `best.pt` + `vocab.json`, không cần train.
- Checkpoint không commit vào git; ghi nơi lưu trữ (Drive/Kaggle) vào bảng §1.
- `src/predict.py` load checkpoint tốt nhất cho CLI và demo.

## 4. Mô hình mở rộng (nếu kịp)

| ID | Mô hình | Ghi chú | Checkpoint |
| :--- | :--- | :--- | :--- |
| E1 | BiLSTM + attention pooling | Thay hidden cuối bằng tổng có trọng số; trực quan hoá trọng số từng từ | TBD |
| E2 | Mô hình tốt nhất với `max_len` 200 / 400 / 600 | Chỉ đổi `max_len`, train lại | `experiments/checkpoints/maxlen_<n>/seed_<s>/best.pt` |
| E3 | DistilBERT fine-tune | Mốc tham chiếu RNN vs Transformer; cần `transformers` | TBD |

E4 (đánh giá chéo Rotten Tomatoes) dùng lại checkpoint ở §1, không train lại. Danh sách: [experiments/README.md](experiments/README.md).
