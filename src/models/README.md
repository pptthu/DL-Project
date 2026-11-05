# src/models

Baseline (Majority, TF-IDF + LR) và mô hình RNN (LSTM, GRU, BiLSTM), nạp GloVe.

**What belongs here:** `baseline.py`, `rnn.py` (`SentimentRNN`, tuỳ chọn attention pooling cho E1), `glove.py` (lọc GloVe theo vocab → `data/processed/*.npy`). DistilBERT (E3) nếu làm: `distilbert.py`.

**What does not:** Vòng train, đọc file split.

Đặc tả: [docs/phases/MODEL.md](../../docs/phases/MODEL.md), [docs/MODELS.md](../../docs/MODELS.md)
