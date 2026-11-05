# MD_PROMPTS.md — Mỗi file Markdown làm gì & Prompt để tạo/cập nhật

- **Motivation/Background**: Dự án IMDb Sentiment (Embedding + LSTM, mở rộng GRU / BiLSTM / GloVe) có nhiều file `.md`; AI agent cần prompt thống nhất để viết đúng và không bịa số.
- **Purpose**: Liệt kê mọi file `.md` trong `DL-Project/`: **Làm gì** và **Prompt** dán cho AI agent để tạo mới hoặc cập nhật file đó.
- **Overview Pipeline**: Quy tắc chung → prompt khung theo họ tài liệu → prompt riêng từng file.
- **Detailed Plan**: §0 Quy tắc chung; §1 Prompt khung; §2 Gốc repo; §3 `docs/` cấp 1; §4 Thư mục con của `docs/`; §5 README thư mục code/dữ liệu.
- **References**: [README.md](README.md), [PURPOSE.md](PURPOSE.md). Bản cũ cho đồ án deepfake và 3 lab: `../../reference/MD_PROMPTS_deepfake_lab.md`.
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## 0. Quy tắc chung cho mọi prompt

Thêm đoạn này vào đầu mọi prompt bên dưới:

> Bạn là AI agent làm việc trong repo `DL-Project` (đề tài 3: phân loại cảm xúc IMDb bằng Embedding + LSTM, mở rộng GRU, BiLSTM, GloVe; nhóm 3 người TV1/TV2/TV3). Trước khi viết, đọc `docs/PURPOSE.md` và code/kết quả thật liên quan (`.py`, `.csv`, `.json`, log, checkpoint). Không bịa số liệu: mọi con số phải truy được về một file trong `experiments/results/` hoặc một lần chạy có thật; nếu chưa có, ghi `TBD` và nêu lệnh để tạo ra số đó. Số của mô hình RNN là mean ± std trên 3 seed (42, 123, 2025). Đầu file giữ header chuẩn gồm `Motivation/Background`, `Purpose`, `Overview Pipeline`, `Detailed Plan`, `References`, `Created`, `Last Updated` (ISO-8601, +07:00). Viết tiếng Việt, giữ nguyên thuật ngữ kỹ thuật tiếng Anh. Dùng đường dẫn tương đối. Liên kết chéo tới tài liệu liên quan. Test set chỉ dùng một lần cho số cuối. Không commit/push khi chưa được duyệt.

---

## 1. Họ tài liệu dùng chung (prompt khung)

### 1.1 `bugs/BUG_<NN>_*.md` — Báo cáo lỗi
**Làm gì:** Ghi một sự cố: triệu chứng, cách tái hiện, nguyên nhân gốc, bản vá, kiểm thử hồi quy.
> Viết `docs/bugs/BUG_<NN>_<TÊN>.md` theo `BUG_TEMPLATE.md`. Gồm: (1) Header: Bug ID, mức độ, trạng thái, môi trường (OS, Python, torch, CUDA/CPU); (2) Triệu chứng: trích nguyên văn dòng lỗi quyết định; (3) Các bước tái hiện tối thiểu; (4) Nguyên nhân gốc, chỉ ra file:dòng; (5) Bản vá: diff ngắn và lý do; (6) Kiểm chứng: lệnh test đã chạy và kết quả; (7) Phòng ngừa: test thêm vào `tests/`. Thêm một dòng vào bảng trong `docs/bugs/README.md`.

### 1.2 `progress/<PHASE>_STATUS.md` — Tiến độ phase
**Làm gì:** Checklist tiến độ một phase: đã xong, đang làm, blocker, bằng chứng.
> Cập nhật `docs/progress/<PHASE>_STATUS.md` theo `PROGRESS_TEMPLATE.md`. Liên kết `docs/phases/<PHASE>.md`. Ghi: trạng thái tổng (To Do / In Progress / Done), checklist `[x]`/`[ ]`, bằng chứng cho mỗi việc xong (lệnh, đường dẫn output, commit), log theo ngày, blocker và người gỡ, quyết định kèm lý do, một bước tiếp theo cụ thể. Chỉ đánh dấu Done khi mọi verification gate trong phase doc pass. Cập nhật cột Trạng thái trong `docs/OVERVIEW.md`.

### 1.3 `phases/<PHASE>.md` — Đặc tả phase
**Làm gì:** Đặc tả kỹ thuật một giai đoạn: mục tiêu, input/output, các bước, gotcha, verification gates.
> Viết `docs/phases/<PHASE>.md` theo `PHASE_TEMPLATE.md`. Gồm: người phụ trách; mục tiêu ("xong" là gì, không làm gì); input và output (đường dẫn, cột/shape); module `src/` thực hiện; các bước; gotcha (rò rỉ dữ liệu, padding, seed); verification gates đo được; link tới `docs/progress/<PHASE>_STATUS.md`.

### 1.4 `experiments/EXP_<NN>_*.md` — Báo cáo thí nghiệm
**Làm gì:** Ghi giả thuyết, biến thay đổi, cấu hình, lệnh chạy và kết quả một thí nghiệm.
> Viết `docs/experiments/EXP_<NN>_<TÊN>.md` theo `EXPERIMENT_TEMPLATE.md`. Gồm: Experiment ID; giả thuyết và tiêu chí thành công có số; **một** biến thay đổi so với EXP trước và danh sách biến giữ nguyên (split, `max_len`, vocab, seed, optimizer, epoch tối đa); cấu hình đầy đủ (emb_dim, hidden, layers, dropout, lr, batch, phần cứng); lệnh chạy chính xác; bảng val (và test nếu đã qua phase EVAL) gồm accuracy, precision/recall/F1 từng lớp, macro-F1, ROC-AUC, số tham số, thời gian train và inference, dạng mean ± std 3 seed — lấy từ `experiments/results/<exp>/summary.json`; đường cong loss/acc từ `experiments/plots/`; kết luận giả thuyết đúng/sai; bước tiếp theo. Cập nhật bảng trong `docs/experiments/README.md`.

### 1.5 `*_TEMPLATE.md` — Mẫu
**Làm gì:** Khung rỗng để copy khi tạo tài liệu cùng loại. Không chứa dữ liệu thật.
> Viết `<LOẠI>_TEMPLATE.md` chỉ gồm khung và placeholder `<...>`. Ghi rõ: copy vào đâu, quy tắc đặt tên file, các mục bắt buộc kèm một dòng giải thích. Không điền số liệu.

### 1.6 `src/<tầng>/README.md` — README tầng mã nguồn
**Làm gì:** Mô tả một tầng code: file nào ở đây, cái gì thuộc/không thuộc tầng này.
> Viết `src/<tầng>/README.md` ngắn. Liệt kê từng file `.py` trong thư mục kèm một dòng mô tả (đọc code để lấy đúng tên hàm/lớp chính). Ghi "What belongs here / What does not". Ghi quy tắc riêng (vd. train chỉ chạy từ script). Link tới phase doc liên quan.

### 1.7 `README.md` chỉ mục thư mục (`bugs/`, `experiments/`)
**Làm gì:** Bảng chỉ mục các tài liệu trong thư mục kèm trạng thái.
> Viết README chỉ mục cho `docs/<thư mục>/`: mục đích, quy tắc đặt tên, bảng ID | Tiêu đề | Trạng thái | Ngày | File cho mọi file hiện có. Quét thư mục để không bỏ sót file.

---

## 2. Gốc repo

**`README.md`** — Điểm vào chính: mục tiêu, cấu trúc thư mục, cài đặt, lệnh chạy, kết quả chính.
> Viết README gốc cho dự án phân loại cảm xúc IMDb bằng LSTM. Gồm: mục tiêu một đoạn (link `docs/PURPOSE.md`); cây thư mục; cài đặt (`python -m venv`, `pip install -r requirements.txt`); lệnh theo thứ tự: tải dữ liệu → chia split → baseline → train từng mô hình 3 seed (`src/train.py`) → evaluate (`src/evaluate.py`) → predict (`src/predict.py`) → demo Gradio (lấy đúng tên script và tham số từ `src/`); `pytest`; bảng kết quả test M0a–M4b lấy từ `docs/experiments/README.md`; link `docs/README.md`. Người ngoài nhóm phải chạy được demo chỉ bằng README.

---

## 3. `docs/` cấp 1

**`docs/README.md`** — Mục lục tài liệu `docs/`.
> Viết mục lục `docs/`: bảng tài liệu cốt lõi và bảng thư mục con, mỗi file một dòng mô tả, gom theo vòng đời PURPOSE → DATA → MODEL → TRAINING → EVAL → DEMO/REPORT. Quét thư mục để đủ file.

**`docs/PURPOSE.md`** — Bản đăng ký đề tài theo mẫu 10 mục của môn học; nguồn sự thật.
> PURPOSE.md là bản chép của bản đăng ký đề tài đã nộp; **không tự đổi nội dung §1–§10**. Khi bản đăng ký thay đổi, chép lại đúng nội dung mới: (1) Tên đề tài; (2) Hướng chính: AI dự đoán; (3) Người sử dụng và vấn đề; (4) Mục tiêu M1–M6 kèm chỉ tiêu (ghi rõ là dự kiến); (5) Dữ liệu: aclImdb, trường `text/label/rating/movie_url`, quy mô, quyền sử dụng, GloVe 6B 100d, phương án thay thế, kiểm tra trùng lặp bằng hash; (6) Phạm vi bắt buộc / mở rộng (attention, `max_len`, DistilBERT, Rotten Tomatoes) / ngoài phạm vi; (7) Phương pháp: baseline Majority + TF-IDF + LR, bảng kiến trúc, huấn luyện, chọn mô hình theo val, giải thích mô hình; (8) Sản phẩm; (9) Đánh giá: chỉ số, chia theo `movie_url`, 3 seed, 7 nhóm tình huống; (10) Phân công, mốc 6 tuần, rủi ro. Sau đó §11 ánh xạ 7 yêu cầu báo cáo → file, §12 định nghĩa "xong". Sửa PURPOSE.md xong thì rà lại DATA, MODELS, OVERVIEW, phases cho khớp.

**`docs/OVERVIEW.md`** — Roadmap theo phase, trạng thái, kiến trúc, checkpoint tốt nhất.
> Cập nhật OVERVIEW.md: bảng 6 phase (DATA_PREP, BASELINE, MODEL, TRAINING, EVAL, DEMO_REPORT) với link phase doc, link status, người chính, tuần (theo mốc 6 tuần trong PURPOSE §10), trạng thái lấy từ `docs/progress/`; bảng phần mở rộng E1–E4; sơ đồ shape `[B, L] → Embedding → RNN → Linear → [B, 1]`; bảng checkpoint (mô hình, val acc và test acc mean ± std, đường dẫn) chỉ điền khi file tồn tại. Cập nhật đầu mỗi buổi làm việc.

**`docs/DATA.md`** — Đặc tả dữ liệu IMDb, giao thức split, kiểm tra rò rỉ, GloVe, dữ liệu mẫu.
> Viết DATA.md: nguồn và trích dẫn Maas et al. (2011); cấu trúc thư mục `aclImdb/` và quy tắc nhãn từ rating trong tên file; cách lấy `movie_url` từ `urls_*.txt`; bảng split train/val/test với số dòng thật đọc từ `data/splits/*.csv`; giao thức chia val (group split theo `movie_url`, giữ tỷ lệ nhãn, seed 42, không đổi theo seed train); quy tắc loại trùng bằng hash (trùng trong train giữ 1, trùng train–test bỏ ở train, test giữ nguyên) và bảng số đã loại với lệnh sinh số; EDA; vocab (`vocab.json`) và GloVe 100d (ma trận lọc `data/processed/*.npy`, tỷ lệ phủ đo bằng code); mô tả `data/sample/sample_100.csv` và `handwritten_30.csv`; Rotten Tomatoes (mở rộng); hạn chế (không có review trung tính, HTML, cắt `max_len`).

**`docs/MODELS.md`** — Danh mục mô hình M0a–M4b (+ E1–E3): kiến trúc, số tham số, checkpoint.
> Viết MODELS.md: bảng M0a Majority, M0b (TF-IDF + LR), M1 LSTM, M2 GRU, M3 BiLSTM, M4a BiLSTM + GloVe frozen, M4b BiLSTM + GloVe fine-tune với embedding, loại RNN, số tham số tổng/train được (đếm bằng `sum(p.numel() ...)` trên model thật), đường dẫn checkpoint. Mô tả class `SentimentRNN` trong `src/models/rnn.py` (đọc code lấy đúng tham số), cách lấy hidden state cuối cho BiLSTM, `pack_padded_sequence`, cách lưu/load checkpoint kèm config và vocab theo bố cục `experiments/checkpoints/<exp>/{vocab.json, seed_<s>/best.pt}`; bảng mô hình mở rộng E1–E3.

**`docs/REPORT_CHECKLIST.md`** — Checklist nộp bài: 7 yêu cầu báo cáo → artifact → trạng thái.
> Cập nhật REPORT_CHECKLIST.md: với 7 yêu cầu (vấn đề & phạm vi; dữ liệu mẫu & mô tả; mã nguồn; hướng dẫn cài đặt/chạy; demo; so sánh với baseline; phân tích lỗi & giới hạn) ghi file/notebook/mục báo cáo đáp ứng, người phụ trách, trạng thái Đạt/Chưa — chỉ Đạt khi mở file thấy nội dung thật. Thêm bảng mục tiêu đăng ký M1–M6: chỉ tiêu, kết quả thật, file nguồn, đạt/không. Kiểm tra danh sách trước khi nộp (số khớp `experiments/results/`, clone mới chạy được, không commit dữ liệu/checkpoint, trích dẫn đủ).

**`docs/MD_PROMPTS.md`** — File này.
> Quét mọi file `.md` trong repo; mỗi file mới chưa có trong danh sách thì thêm mục **Làm gì** + **Prompt** (hoặc trỏ tới prompt khung §1). Xóa mục của file không còn tồn tại.

---

## 4. Thư mục con của `docs/`

### 4.1 `docs/phases/` (prompt 1.3)
- **`DATA_PREP.md`** — Tải IMDb, làm sạch, loại trùng bằng hash, chia split theo `movie_url`, EDA, `data/sample/`. (TV1, tuần 1)
- **`BASELINE.md`** — Majority class + TF-IDF (1–2 gram) + Logistic Regression, chọn `C` trên val. (TV1, tuần 1)
- **`MODEL.md`** — `SentimentRNN` tham số hóa LSTM/GRU, uni/bi, nạp GloVe (+ attention E1); test shape, overfit một batch, save/load. (TV2, tuần 2–3)
- **`TRAINING.md`** — `src/train.py`, `BCEWithLogitsLoss`, Adam 1e-3, clipping 5.0, early stopping patience 3, 3 seed, `summary.json`. (TV2, tuần 2–3)
- **`EVAL.md`** — Đánh giá test một lần, metric đủ (macro-F1, ROC-AUC, FP/FN), phân tích lỗi 7 nhóm (≥ 10 ví dụ sai/nhóm), giải thích mô hình (occlusion / gradient × embedding), giới hạn. (TV3, tuần 4)
- **`DEMO_REPORT.md`** — `src/predict.py` dùng chung, demo Gradio + `notebooks/demo.ipynb`, tô màu từ, CPU < 1 giây/review, README, báo cáo và slide. (TV3, cả nhóm, tuần 5–6)
- **`PHASE_TEMPLATE.md`** — Mẫu (1.5).

### 4.2 `docs/progress/` (prompt 1.2)
- `DATA_PREP_STATUS.md`, `BASELINE_STATUS.md`, `MODEL_STATUS.md`, `TRAINING_STATUS.md`, `EVAL_STATUS.md`, `DEMO_REPORT_STATUS.md` — tiến độ từng phase.
- `PROGRESS_TEMPLATE.md` — Mẫu (1.5).

### 4.3 `docs/experiments/` (prompt 1.4)
Chuỗi thí nghiệm, mỗi bước đổi một biến:
- **`EXP_01_BASELINE.md`** — Majority class (mốc sàn) + TF-IDF + LR (mốc cổ điển). (TV1)
- **`EXP_02_LSTM.md`** — Embedding ngẫu nhiên + LSTM 1 chiều theo đề bài. (TV2)
- **`EXP_03_GRU.md`** — Đổi LSTM → GRU. Thêm vào prompt: "so sánh số tham số và thời gian/epoch với EXP_02". (TV2)
- **`EXP_04_BILSTM.md`** — Đổi 1 chiều → 2 chiều. Thêm: "giải thích vì sao đọc ngược giúp/không giúp với review dài". (TV2)
- **`EXP_05_BILSTM_GLOVE.md`** — Thay embedding ngẫu nhiên bằng GloVe 100d, hai biến thể frozen và fine-tune. Thêm: "ghi tỷ lệ phủ vocab của GloVe; so sánh tốc độ hội tụ (epoch tới val loss tốt nhất)". (TV2)
- Mở rộng, nếu kịp: **`EXP_06_ATTENTION.md`** (attention pooling, trực quan trọng số), **`EXP_07_MAXLEN.md`** (`max_len` 200/400/600), **`EXP_08_DISTILBERT.md`** (mốc Transformer), **`EXP_09_ROTTEN_TOMATOES.md`** (đánh giá chéo, không train lại).
- **`README.md`** (1.7) — chỉ mục và bảng kết quả M0a–M4b so với baseline.
- **`EXPERIMENT_TEMPLATE.md`** — Mẫu (1.5).

### 4.4 `docs/bugs/`
- **`README.md`** (1.7), **`BUG_TEMPLATE.md`** (1.5). Báo cáo lỗi mới dùng prompt 1.1.

### 4.5 `docs/references/`
**`GIT_WORKFLOW.md`** — Quy trình Git cho nhóm 3 người.
> Viết quy trình Git cho nhóm 3 người: nhánh (`feat/`, `fix/`, `docs/`, `exp/`), Conventional Commits với scope `data|baseline|model|train|eval|demo|docs`, nội dung mô tả PR, ai review ai, chạy `pytest` trước khi mở PR, điều cấm (push thẳng `main`, commit dữ liệu/split/GloVe/`data/processed`/checkpoint/`.venv`, đường dẫn tuyệt đối, secret).

---

## 5. README thư mục code và dữ liệu (prompt 1.6)

Entry point nằm ở `src/train.py`, `src/evaluate.py`, `src/predict.py` (theo PURPOSE §8); các package con chỉ chứa thư viện.

- **`src/data/README.md`** — Tải, làm sạch, loại trùng, split theo `movie_url`, vocab, Dataset/DataLoader.
- **`src/models/README.md`** — Baseline, `SentimentRNN`, nạp GloVe (+ DistilBERT nếu làm E3).
- **`src/training/README.md`** — Vòng train, early stopping; gọi từ `src/train.py`.
- **`src/eval/README.md`** — Metric, phân tích lỗi 7 nhóm, giải thích mô hình (`explain.py`).
- **`src/demo/README.md`** — App Gradio gọi lại `src/predict.py` và `src/eval/explain.py`.
- **`src/utils/README.md`** — Seed, đường dẫn, logging.
- **`data/README.md`** — Bố cục `raw/`, `splits/`, `glove/`, `processed/`, `sample/`; chỉ `sample/` được commit.
  > Thêm: "liệt kê đúng file hiện có trong `data/`; ghi lệnh sinh ra từng thư mục".
- **`experiments/README.md`** — Bố cục `checkpoints/<exp>/seed_<s>/`, `runs/`, `results/` (+ `summary.json`), `plots/`; cái nào commit, cái nào không.
- **`notebooks/README.md`** — Mỗi notebook làm gì, ai phụ trách; notebook chỉ đọc kết quả, không train.
