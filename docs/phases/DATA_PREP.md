# DATA_PREP.md — Chuẩn bị dữ liệu & split

- **Motivation/Background**: Mọi phase sau dùng chung một split cố định; split sai làm sai mọi số đo.
- **Purpose**: Tải IMDb, làm sạch, loại trùng, chia train/val theo phim, EDA, tạo dữ liệu mẫu.
- **Overview Pipeline**: Phạm vi → đặc tả → artifact → kiểm chứng.
- **Detailed Plan**: Header; Goals; Input/Output; How to do it; Detailed plan / gotchas; Verification gates; Links.
- **References**: [OVERVIEW.md](../OVERVIEW.md), [PURPOSE.md](../PURPOSE.md) §5, §9, [DATA.md](../DATA.md).
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## Header

- **Title:** Chuẩn bị dữ liệu & split
- **Date created:** 2026-11-05
- **Last updated:** 2026-11-05
- **Description:** Tải IMDb, làm sạch, loại trùng, chia train/val theo phim, EDA, tạo dữ liệu mẫu.
- **Status:** To Do
- **Owner:** TV1
- **Tuần:** 1

## Goals / Purpose

- Xong: 3 CSV trong `data/splits/`; bảng trùng lặp & rò rỉ trong [DATA.md](../DATA.md) §4 đã điền số thật; EDA xong; `data/sample/sample_100.csv` và `data/sample/handwritten_30.csv` có đủ dòng.
- Không làm: tokenize/vocab cho mô hình (thuộc MODEL/TRAINING).

## Input / Output

- **Input:** `aclImdb_v1.tar.gz` từ Stanford (hoặc `stanfordnlp/imdb`).
- **Output:**
  - `data/splits/{train,val,test}.csv` (cột `text,label,rating,movie_url,source_file`).
  - `data/sample/sample_100.csv`, `data/sample/handwritten_30.csv` (cột `text,label,case_type,...`).
  - `notebooks/01_eda.ipynb`.

## How to do it (general plan)

1. Tải và giải nén vào `data/raw/` (`src/data/download.py`).
2. Đọc file, lấy `rating` từ tên file, `movie_url` từ `urls_*.txt`.
3. Làm sạch HTML, hash nội dung đã chuẩn hoá, loại trùng theo quy tắc [DATA.md](../DATA.md) §4 (`src/data/clean.py`).
4. Chia val 10% từ train theo nhóm `movie_url`, giữ tỷ lệ nhãn, `seed=42` (`src/data/split.py`).
5. Kiểm tra giao `movie_url`; ghi mọi số vào DATA.md.
6. EDA: phân bố nhãn, độ dài review (đề xuất `max_len`), từ phổ biến từng lớp (`notebooks/01_eda.ipynb`).
7. Chọn 100 review từ test có gán `case_type`; viết ~30 câu tự viết.

## Detailed plan / gotchas

- Dòng thứ i của `urls_pos.txt` ứng với review có `id = i` trong `pos/`.
- Làm sạch `<br />` trước khi hash để bắt được bản trùng khác thẻ HTML.
- Không xoá review trong test; trùng train–test thì bỏ ở train.
- EDA chỉ trên train; không đọc `test/` khi chọn siêu tham số.

## Verification gates

- [ ] train + val = 25.000 − số trùng đã loại; test = 25.000.
- [ ] 0 `movie_url` chung giữa train và val, và giữa train và test.
- [ ] 0 hash chung giữa train/val và test.
- [ ] Chạy lại script với cùng seed cho file giống hệt (so hash).

## Links

- Progress tracking: [../progress/DATA_PREP_STATUS.md](../progress/DATA_PREP_STATUS.md)
- Related: [OVERVIEW.md](../OVERVIEW.md), [DATA.md](../DATA.md)
