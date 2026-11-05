# DATA.md — Đặc tả dữ liệu IMDb

- **Motivation/Background**: Số đo chỉ đáng tin khi split cố định và không rò rỉ. IMDb có nhiều review cùng một phim nên chia ngẫu nhiên dễ rò rỉ.
- **Purpose**: Đặc tả chính thức về nguồn dữ liệu, cấu trúc file, giao thức chia train/val/test, kiểm tra trùng lặp và rò rỉ, GloVe, dữ liệu mẫu.
- **Overview Pipeline**: Tải `aclImdb_v1.tar.gz` → đọc `train/`, `test/` → gắn `rating` (tên file) và `movie_url` (`urls_*.txt`) → làm sạch → hash, loại trùng → chia val từ train theo phim → ghi CSV vào `data/splits/`.
- **Detailed Plan**: §1 Nguồn; §2 Cấu trúc file; §3 Giao thức split; §4 Trùng lặp & rò rỉ; §5 EDA; §6 Vocab & GloVe; §7 Dữ liệu mẫu; §8 Dữ liệu ngoài miền; §9 Hạn chế.
- **References**: [PURPOSE.md](PURPOSE.md) §5, §9; [phases/DATA_PREP.md](phases/DATA_PREP.md).
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## 1. Nguồn

- **Large Movie Review Dataset v1.0** — Maas, A. L. et al. (2011). *Learning Word Vectors for Sentiment Analysis.* ACL 2011.
- Link: <https://ai.stanford.edu/~amaas/data/sentiment/> (`aclImdb_v1.tar.gz`, ~80 MB).
- Phương án thay thế: `stanfordnlp/imdb` (Hugging Face Datasets). Bản này không có `movie_url`; nếu phải dùng, ghi rõ trong [progress/DATA_PREP_STATUS.md](progress/DATA_PREP_STATUS.md) là không chia được theo phim.
- Không phát tán lại toàn bộ dữ liệu; repo chỉ commit `data/sample/` (§7).

## 2. Cấu trúc file

```
aclImdb/
├── train/{pos,neg}/<id>_<rating>.txt   # 12.500 + 12.500
├── train/unsup/                        # 50.000 không nhãn — không dùng
├── train/urls_{pos,neg}.txt            # dòng i = URL phim của review id i
├── test/{pos,neg}/<id>_<rating>.txt    # 12.500 + 12.500
├── test/urls_{pos,neg}.txt
└── imdb.vocab
```

- Rating 1–4 → `label = 0` (negative); rating 7–10 → `label = 1` (positive). Rating 5–6 không có trong tập có nhãn.
- `rating` chỉ dùng để phân tích lỗi (nhóm 5 trong [PURPOSE.md](PURPOSE.md) §9), không đưa vào mô hình.
- Theo tác giả: tối đa 30 review mỗi phim; phim ở train và test không trùng nhau.

## 3. Giao thức split

| Split | Nguồn | Kích thước | Ghi chú |
| :--- | :--- | :--- | :--- |
| train | 90% `train/` (sau khi loại trùng) | TBD (~22.500) | Dùng để train và fit vocab/TF-IDF |
| val | 10% `train/` (sau khi loại trùng) | TBD (~2.500) | Chọn epoch, siêu tham số, checkpoint |
| test | toàn bộ `test/` | 25.000 | Giữ nguyên; chỉ chạy một lần cho số cuối |

- Chia val theo nhóm `movie_url` (group split), cố gắng giữ tỷ lệ nhãn 50/50 (`StratifiedGroupKFold` hoặc tương đương), `seed = 42`.
- Split cố định một lần cho mọi thí nghiệm; 3 seed huấn luyện (42, 123, 2025) **không** đổi split.
- Vocab và TF-IDF chỉ fit trên train, không fit trên val/test.
- CSV split cột: `text`, `label`, `rating`, `movie_url`, `source_file`.

## 4. Trùng lặp & rò rỉ

Hash nội dung sau chuẩn hoá (bỏ HTML, chữ thường, gộp khoảng trắng).

**Quy tắc loại bỏ:**
- Trùng trong train: giữ 1 bản.
- Trùng giữa train và test: bỏ bản ở train, để test giữ nguyên 25.000.
- Trùng trong test: không xoá (test giữ nguyên); ghi số lượng vào bảng.

| Kiểm tra | Kết quả |
| :--- | :--- |
| Review trùng trong train (đã loại) | TBD |
| Review trùng giữa train và test (đã loại khỏi train) | TBD |
| Review trùng trong test (chỉ ghi nhận) | TBD |
| `movie_url` chung giữa train và val | TBD (kỳ vọng 0) |
| `movie_url` chung giữa train và test | TBD (kỳ vọng 0) |
| Tỷ lệ nhãn mỗi split | TBD |

Ghi lệnh sinh từng số vào [progress/DATA_PREP_STATUS.md](progress/DATA_PREP_STATUS.md).

## 5. EDA

Trong `notebooks/01_eda.ipynb`, chỉ trên train:
- Phân bố nhãn, phân bố `rating`.
- Độ dài review (số từ): trung bình, trung vị, phân vị 90/95 — dùng để chọn `max_len` mặc định.
- Từ phổ biến của từng lớp.

## 6. Vocab & GloVe

- **Vocab:** xây từ train; `<pad>=0`, `<unk>=1`. Lưu `vocab.json` cạnh checkpoint (xem [MODELS.md](MODELS.md) §3). Ngưỡng `min_freq` / `max_size`: TBD, ghi vào `configs/`.
- **GloVe:** `glove.6B.zip` (Wikipedia 2014 + Gigaword 5, 400k từ) — <https://nlp.stanford.edu/projects/glove/>, giấy phép ODC-PDDL. Chỉ dùng `glove.6B.100d.txt`, để ở `data/glove/`.
- Lọc GloVe theo vocab, lưu ma trận `data/processed/glove_100d_vocab.npy` (không commit). Từ không có trong GloVe khởi tạo ngẫu nhiên.
- Tỷ lệ phủ vocab: TBD (đo bằng code).

## 7. Dữ liệu mẫu

Được commit, trong `data/sample/`:

| File | Nội dung | Dùng cho |
| :--- | :--- | :--- |
| `sample_100.csv` | 100 review lấy từ test, có cả ca khó (phủ định, mỉa mai, dài, khen chê lẫn lộn, điểm sát ngưỡng) | Demo, minh họa trong báo cáo |
| `handwritten_30.csv` | ~30 câu nhóm tự viết, gồm câu ngắn và câu ngoài miền phim | Nhóm tình huống 7, thử độ bền demo |

Cột: `text`, `label`, `case_type` (+ `rating`, `source_file` với `sample_100.csv`).

## 8. Dữ liệu ngoài miền (mở rộng)

Rotten Tomatoes (Hugging Face `rotten_tomatoes`), chỉ đánh giá chéo, không train lại. Lưu ở `data/raw/rotten_tomatoes/` (không commit).

## 9. Hạn chế

- Chỉ có review cực tính (≤4 hoặc ≥7 sao): mô hình chưa gặp review trung tính.
- Văn bản có thẻ HTML (`<br />`), cần làm sạch.
- Review dài bị cắt ở `max_len`, có thể mất câu kết luận.
- `sample_100.csv` lấy từ test: chỉ để demo, không dùng chọn mô hình.
