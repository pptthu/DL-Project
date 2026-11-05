# REPORT_CHECKLIST.md — Checklist nộp bài

- **Motivation/Background**: Môn học yêu cầu báo cáo có 7 nội dung bắt buộc; cần biết mục nào đã có bằng chứng.
- **Purpose**: Ánh xạ từng yêu cầu báo cáo → artifact đáp ứng → người phụ trách → trạng thái.
- **Overview Pipeline**: Yêu cầu môn → file/notebook/mục báo cáo → kiểm tra → Đạt/Chưa.
- **Detailed Plan**: §1 Yêu cầu báo cáo; §2 Mục tiêu đăng ký M1–M6; §3 Yêu cầu đăng ký đề tài; §4 Kiểm tra trước khi nộp.
- **References**: [PURPOSE.md](PURPOSE.md).
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## 1. Yêu cầu báo cáo

| # | Yêu cầu | Artifact | Người | Trạng thái |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Báo cáo vấn đề và phạm vi | [PURPOSE.md](PURPOSE.md) §3, §6 | TV3 | Chưa |
| 2 | Dữ liệu mẫu và mô tả | [DATA.md](DATA.md), `data/sample/sample_100.csv` | TV1 | Chưa |
| 3 | Mã nguồn | `src/`, `configs/`, `tests/` | Cả nhóm | Chưa |
| 4 | Hướng dẫn cài đặt/chạy | [README.md](../README.md) §3–§4 | TV3 | Chưa |
| 5 | Demo | `src/demo/`, `notebooks/demo.ipynb`, video/ảnh chụp demo | TV3 | Chưa |
| 6 | Kết quả so sánh với baseline | [experiments/README.md](experiments/README.md) | TV2 | Chưa |
| 7 | Phân tích lỗi và giới hạn | [phases/EVAL.md](phases/EVAL.md), [DATA.md](DATA.md) §9 | TV3 | Chưa |

## 2. Mục tiêu đăng ký M1–M6

Theo [PURPOSE.md](PURPOSE.md) §4. Ghi số thật và file nguồn; trượt chỉ tiêu vẫn báo cáo trung thực.

| # | Chỉ tiêu | Kết quả | Nguồn | Đạt? |
| :--- | :--- | :--- | :--- | :--- |
| M1 | LSTM test acc ≥ 85% | TBD | `experiments/results/lstm/` | — |
| M2 | Mô hình tốt nhất test acc ≥ 88%, macro-F1 ≥ 0.88 | TBD | TBD | — |
| M3 | Bảng so sánh với TF-IDF + LR, cùng split | TBD | [experiments/README.md](experiments/README.md) | — |
| M4 | Số liệu từng nhóm tình huống, ≥ 10 ví dụ sai/nhóm kèm giải thích | TBD | [phases/EVAL.md](phases/EVAL.md) | — |
| M5 | Demo CPU < 1 giây/review, có tô màu từ | TBD | [phases/DEMO_REPORT.md](phases/DEMO_REPORT.md) | — |
| M6 | 3 seed mean ± std; chạy lại lệch ≤ 1% | TBD | `experiments/results/<exp>/summary.json` | — |

## 3. Yêu cầu đăng ký đề tài

Đủ 10 mục của mẫu: [PURPOSE.md](PURPOSE.md) §1–§10. Trạng thái: đã chép bản đăng ký; chờ điền tên nhóm, thành viên và deadline.

## 4. Kiểm tra trước khi nộp

- [ ] Mọi số trong báo cáo/slide khớp file trong `experiments/results/`.
- [ ] Test set chỉ dùng một lần cho mỗi checkpoint cuối.
- [ ] Clone repo mới, làm theo README, chạy được `pytest` và demo.
- [ ] Không commit `data/raw/`, `data/splits/`, `data/glove/`, `data/processed/`, checkpoint, `.venv/`.
- [ ] Trích dẫn Maas et al. (2011) và GloVe (Pennington et al., 2014); DistilBERT (Sanh et al., 2019) nếu làm E3.
