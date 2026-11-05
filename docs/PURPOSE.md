# PURPOSE.md — Đăng ký đề tài & Yêu cầu đồ án

- **Motivation/Background**: Đồ án môn Học sâu (UTH), chủ đề số 3 (nhóm LLMs) — phân loại cảm xúc review phim IMDb bằng LSTM, so sánh với GRU, BiLSTM và GloVe.
- **Purpose**: Bản đăng ký đề tài chính thức theo mẫu 10 mục của môn học. Đây là nguồn sự thật; mọi tài liệu khác dẫn xuất từ đây.
- **Overview Pipeline**: Bài toán → dữ liệu → baseline → LSTM → biến thể (GRU, BiLSTM, GloVe) → đánh giá + phân tích lỗi + giải thích → demo → báo cáo.
- **Detailed Plan**: §1–§10 theo mẫu đăng ký; §11 Yêu cầu báo cáo cuối kỳ; §12 Định nghĩa "xong".
- **References**: [OVERVIEW.md](OVERVIEW.md), [DATA.md](DATA.md), [MODELS.md](MODELS.md), [REPORT_CHECKLIST.md](REPORT_CHECKLIST.md).
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

> Nhóm `[tên nhóm]` — Chủ đề số 3 (nhóm LLMs) — Classification — IMDb — Sentiment analysis
> Thành viên: `[Họ tên 1 – MSSV]`, `[Họ tên 2 – MSSV]`, `[Họ tên 3 – MSSV]`

| Mục | Nội dung |
| :--- | :--- |
| STT đề tài | 3 (nhóm LLMs) |
| Loại bài toán | Classification (nhị phân) |
| Dataset | IMDb Reviews — <https://ai.stanford.edu/~amaas/data/sentiment/> |
| Mô hình đề bài | Embedding + LSTM + Linear |
| Mở rộng đề bài | GRU, Bidirectional LSTM, GloVe embeddings |
| Nhóm | 3 thành viên (xem §10) |

---

## 1. Tên đề tài

Phân loại cảm xúc (tích cực/tiêu cực) cho review phim tiếng Anh trên IMDb bằng mạng hồi quy LSTM, so sánh với GRU, BiLSTM và embedding GloVe.

## 2. Hướng chính

**AI dự đoán**: mô hình học sâu dự đoán nhãn cảm xúc của một đoạn văn bản.

## 3. Người sử dụng và vấn đề

- **Người sử dụng:** nền tảng review phim, hãng phim và bộ phận marketing muốn theo dõi phản hồi của khán giả; sinh viên học NLP cần một pipeline mẫu có thể chạy lại được.
- **Khó khăn hiện tại:**
  - Lượng review rất lớn, đọc và gán nhãn thủ công không kịp.
  - Review dài (trung bình khoảng 230 từ), hay có phủ định ("not bad"), mỉa mai, hoặc vừa khen vừa chê, nên cách đếm từ khoá tích cực/tiêu cực thường đoán sai.
  - Nhiều mô hình chỉ báo một con số accuracy mà không cho biết mô hình sai ở loại câu nào và vì sao sai.

## 4. Mục tiêu

Chỉ tiêu là **dự kiến**; số thật ghi vào [experiments/README.md](experiments/README.md) sau khi chạy.

| # | Đầu ra | Chỉ tiêu đo lường dự kiến |
| :--- | :--- | :--- |
| M1 | Mô hình bắt buộc Embedding + LSTM + Linear | Test accuracy ≥ 85% |
| M2 | Các biến thể GRU, BiLSTM, BiLSTM + GloVe | Mô hình tốt nhất đạt test accuracy ≥ 88%, macro-F1 ≥ 0.88 |
| M3 | So sánh với baseline TF-IDF + Logistic Regression | Bảng so sánh công bằng (cùng cách chia dữ liệu); báo cáo trung thực kể cả khi baseline thắng |
| M4 | Phân tích lỗi theo nhóm tình huống (§9) | Có số liệu riêng cho từng nhóm, mỗi nhóm có ít nhất 10 ví dụ sai kèm giải thích |
| M5 | Demo nhập review, trả về nhãn + xác suất + từ ảnh hưởng mạnh nhất | Chạy được trên CPU, < 1 giây/review |
| M6 | Tái lập kết quả | Cố định seed, chạy 3 seed và báo mean ± std; chạy lại theo README ra kết quả lệch ≤ 1% |

> Lưu ý: M1–M6 ở đây là **mục tiêu**, khác với mã mô hình M0a–M4b trong [MODELS.md](MODELS.md).

## 5. Dữ liệu

- **Nguồn chính:** Large Movie Review Dataset v1.0 (Maas et al., ACL 2011), <https://ai.stanford.edu/~amaas/data/sentiment/>
- **Trường dữ liệu:**
  - `text`: nội dung review (tiếng Anh, có thẻ HTML `<br />` cần làm sạch).
  - `label`: 0 = negative (điểm ≤ 4/10), 1 = positive (điểm ≥ 7/10). Review trung tính (5–6 điểm) không có trong bộ dữ liệu.
  - `rating`: điểm 1–10, lấy từ tên file `id_rating.txt`. Dùng để phân tích lỗi, không dùng để huấn luyện.
  - `movie_url`: link phim của từng review, dùng để chia dữ liệu theo phim.
- **Quy mô:** 50.000 review có nhãn, gồm 25.000 train và 25.000 test, mỗi tập cân bằng 12.500 positive và 12.500 negative. Có thêm 50.000 review không nhãn (không dùng ở phần bắt buộc). Mỗi phim có tối đa 30 review, phim ở tập train và tập test không trùng nhau.
- **Quyền sử dụng:** bộ dữ liệu công khai phục vụ nghiên cứu; trích dẫn bài báo gốc của Maas et al. (2011) trong báo cáo. Nhóm không phát tán lại toàn bộ dữ liệu, chỉ đưa kèm 100 review mẫu để demo.
- **Embedding:** GloVe 6B 100d (Stanford NLP, công khai).
- **Phương án thay thế:**
  1. Bản trên HuggingFace `stanfordnlp/imdb`, cùng nội dung nhưng tải nhanh hơn.
  2. Nếu cần kiểm tra khả năng tổng quát ngoài IMDb: Rotten Tomatoes (câu ngắn) hoặc SST-2.
- **Kiểm tra chất lượng dữ liệu:** hash nội dung review sau khi chuẩn hoá để phát hiện review trùng lặp trong cùng một tập và giữa train với test, rồi loại bỏ và ghi lại số lượng đã loại. Kèm theo: thống kê độ dài review, phân bố nhãn, các từ phổ biến của từng lớp.

Chi tiết và giao thức chia dữ liệu: [DATA.md](DATA.md).

## 6. Phạm vi

**Bắt buộc**
- Pipeline tiền xử lý: làm sạch HTML, tokenize, xây vocab từ tập train, padding/truncation.
- Mô hình Embedding + LSTM + Linear, huấn luyện bằng PyTorch.
- Biến thể GRU, BiLSTM, BiLSTM + GloVe (đóng băng embedding và fine-tune embedding).
- Baseline TF-IDF + Logistic Regression.
- Đánh giá, phân tích lỗi, demo, README.

**Mở rộng** (làm nếu còn thời gian)
- Thêm lớp attention pooling lên BiLSTM để trực quan hoá trọng số từng từ.
- Ảnh hưởng của độ dài cắt `max_len` (200 / 400 / 600 token) đến accuracy.
- Mốc tham chiếu: fine-tune DistilBERT để thấy khoảng cách giữa RNN và Transformer (gắn với nhóm chủ đề LLMs).
- Đánh giá chéo trên Rotten Tomatoes, không huấn luyện lại.

**Ngoài phạm vi:** phân loại nhiều mức (1–10 sao), văn bản tiếng Việt, triển khai production.

## 7. Phương pháp

**Baseline**
1. Majority class: luôn đoán một nhãn, khoảng 50% accuracy (mốc sàn).
2. TF-IDF (unigram + bigram) + Logistic Regression: baseline cổ điển mạnh, cho biết mô hình học sâu có thực sự tốt hơn hay không.

**Giải pháp dự kiến**

| Mô hình | Kiến trúc |
| :--- | :--- |
| LSTM (bắt buộc) | Embedding(V, 100) → LSTM(128, 1 lớp) → hidden cuối → Dropout → Linear(128, 1) → Sigmoid |
| GRU | Giống LSTM, thay cell bằng GRU (ít tham số hơn) |
| BiLSTM | LSTM 2 chiều, ghép hidden 2 chiều → Linear(256, 1) |
| BiLSTM + GloVe | Khởi tạo embedding từ GloVe 100d; so sánh đóng băng với fine-tune |

- **Huấn luyện:** BCEWithLogitsLoss, Adam (lr 1e-3), gradient clipping 5.0, early stopping theo val loss (patience 3), dùng `pack_padded_sequence` để LSTM bỏ qua phần padding.
- **Chọn mô hình:** mọi lựa chọn siêu tham số và checkpoint đều dựa trên **tập validation**. Tập test chỉ chạy **một lần** ở cuối để tránh "nhìn trộm" tập test.
- **Giải thích mô hình:** occlusion (che từng từ rồi đo xác suất thay đổi bao nhiêu) hoặc gradient × embedding, để kiểm tra mô hình có học đúng từ mang cảm xúc hay học "đường tắt" như tên diễn viên hay độ dài review.
- **Thí nghiệm** (mỗi bước đổi một biến): [experiments/README.md](experiments/README.md).

## 8. Sản phẩm

| Sản phẩm | Mô tả |
| :--- | :--- |
| Mã nguồn | `src/data`, `src/models`, `src/train.py`, `src/evaluate.py`, `src/predict.py`; cấu hình trong `configs/*.yaml` |
| Mô hình | Checkpoint `.pt` của 4 mô hình kèm `vocab.json`; hàm `predict(text) -> (label, prob)` |
| Demo | Ứng dụng Gradio: nhập review, hiện nhãn, xác suất và tô màu từ quan trọng; kèm notebook `demo.ipynb` |
| Dữ liệu mẫu | `data/sample/sample_100.csv` gồm 100 review lấy từ test, có cả các ca khó |
| Tài liệu | `README.md` (cài đặt/chạy), `docs/DATA.md` (mô tả dữ liệu), báo cáo cuối kỳ, slide |
| Kiểm thử | `tests/`: shape đầu ra của model, overfit 1 batch, save/load checkpoint |

## 9. Đánh giá

- **Chỉ số:** Accuracy, Precision, Recall, F1 từng lớp và macro-F1, ROC-AUC, confusion matrix; thêm số tham số và thời gian train/inference của từng mô hình. Dữ liệu cân bằng 1:1 nên accuracy dùng được, nhưng vẫn báo đủ cả hai chiều lỗi: dương tính giả (review chê bị đoán là khen) và âm tính giả.
- **Cách chia dữ liệu:**
  - Train gốc 25.000 review chia thành **train 90% / validation 10%, theo phim** (group split theo `movie_url`) để review của cùng một phim không nằm ở cả train lẫn val.
  - Test gốc 25.000 review giữ nguyên, chỉ dùng để báo cáo cuối.
  - Mỗi cấu hình chạy 3 seed (42, 123, 2025) và báo mean ± std.
- **Bộ tình huống để phân tích lỗi:**
  1. Theo độ dài: < 100, 100–300, 300–600, > 600 từ. Với review dài, xem việc cắt ngắn có làm mất câu kết luận không.
  2. Phủ định: "not good", "not bad at all".
  3. Review vừa khen vừa chê, đổi giọng ở cuối ("great acting, but...").
  4. Mỉa mai, châm biếm.
  5. Review có điểm sát ngưỡng (4 hoặc 7 điểm) so với điểm cực trị (1 hoặc 10 điểm).
  6. Có nhiều từ ngoài vocab hoặc từ hiếm.
  7. Bộ câu tự viết khoảng 30 câu, gồm cả câu ngắn và câu ngoài miền phim, để thử độ bền của mô hình trong demo.

## 10. Kế hoạch và rủi ro

### Phân công

| Thành viên | Nhiệm vụ chính | Phase |
| :--- | :--- | :--- |
| TV1 `[Họ tên, MSSV]` | Dữ liệu: tải, EDA, làm sạch, kiểm tra trùng lặp, chia theo phim, `DATA.md`; baseline TF-IDF + LR | DATA_PREP, BASELINE |
| TV2 `[Họ tên, MSSV]` | Mô hình và huấn luyện: LSTM, GRU, BiLSTM, GloVe; script train và config; chạy 3 seed | MODEL, TRAINING |
| TV3 `[Họ tên, MSSV]` | Đánh giá, phân tích lỗi, giải thích mô hình; demo Gradio; README; báo cáo và slide | EVAL, DEMO_REPORT |

Cả nhóm cùng review code và viết báo cáo; mỗi người tự trình bày được phần của mình.

### Mốc hoàn thành

| Tuần | Mốc |
| :--- | :--- |
| 1 | Nộp đăng ký; tải dữ liệu, EDA, kiểm tra trùng lặp, chia train/val; chạy baseline TF-IDF + LR |
| 2 | Pipeline PyTorch; LSTM bắt buộc đạt ≥ 85% trên val |
| 3 | GRU, BiLSTM, GloVe; tinh chỉnh siêu tham số trên val; chạy 3 seed |
| 4 | Chốt mô hình, chạy test một lần; phân tích lỗi theo bộ tình huống; giải thích mô hình |
| 5 | Demo, README, phần mở rộng (nếu kịp) |
| 6 | Hoàn thiện báo cáo và slide, tập dượt trả lời phản biện |

Ngày cụ thể: `[điền theo deadline của lớp]`.

### Phụ thuộc và rủi ro

| Rủi ro / phụ thuộc | Ảnh hưởng | Cách xử lý |
| :--- | :--- | :--- |
| Không có GPU, train chậm | Trễ tiến độ tuần 3 | Dùng Google Colab hoặc Kaggle (GPU miễn phí); giới hạn `max_len`; lưu checkpoint thường xuyên |
| File GloVe lớn (khoảng 800MB) | Tải chậm, tốn dung lượng | Chỉ dùng bản 100d, lọc theo vocab rồi lưu ma trận `.npy` nhỏ |
| Overfit (train cao, val thấp) | Kết quả test kém | Dropout, early stopping, weight decay; theo dõi learning curve |
| LSTM thua baseline TF-IDF + LR | Kết luận "khó bán" | Báo cáo trung thực và phân tích nguyên nhân; nhấn mạnh phần so sánh và phân tích lỗi |
| Rò rỉ dữ liệu (review trùng, cùng phim) | Kết quả bị thổi phồng | Kiểm tra trùng lặp bằng hash và chia theo phim (§5, §9) |
| Thành viên bận hoặc vắng | Thiếu người làm một phần | Code đẩy lên Git hằng tuần; mỗi phần có người dự phòng |
| Nguồn tải Stanford không truy cập được | Không có dữ liệu | Dùng bản HuggingFace `stanfordnlp/imdb` |

## 11. Yêu cầu báo cáo cuối kỳ

| Yêu cầu của môn | Đáp ứng bằng |
| :--- | :--- |
| Báo cáo vấn đề và phạm vi | §3, §6 của file này |
| Dữ liệu mẫu và mô tả | [DATA.md](DATA.md), `data/sample/` |
| Mã nguồn | `src/`, `configs/`, `tests/` |
| Hướng dẫn cài đặt/chạy | [README.md](../README.md) |
| Demo | `src/demo/`, `notebooks/demo.ipynb`, [phases/DEMO_REPORT.md](phases/DEMO_REPORT.md) |
| Kết quả so sánh với baseline | [experiments/README.md](experiments/README.md) |
| Phân tích lỗi và giới hạn | [phases/EVAL.md](phases/EVAL.md) |

Checklist chi tiết: [REPORT_CHECKLIST.md](REPORT_CHECKLIST.md).

## 12. Định nghĩa "xong"

- Mọi con số trong báo cáo truy được về một file kết quả trong `experiments/results/`.
- Số của mô hình RNN là mean ± std trên 3 seed (42, 123, 2025).
- Test set chỉ chạy một lần cho mỗi checkpoint cuối (đã chốt theo val).
- Người ngoài nhóm clone repo, làm theo README và chạy được demo; kết quả chạy lại lệch ≤ 1%.
