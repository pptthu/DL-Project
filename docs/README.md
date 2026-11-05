# docs/README.md — Mục lục tài liệu

- **Motivation/Background**: Nhóm 3 người cần biết tài liệu nào ở đâu và file nào là nguồn sự thật.
- **Purpose**: Mục lục toàn bộ tài liệu trong `docs/`, gom theo vòng đời dự án.
- **Overview Pipeline**: PURPOSE → DATA → MODEL → TRAINING → EVAL → DEMO/REPORT.
- **Detailed Plan**: §1 Tài liệu cốt lõi; §2 Thư mục con.
- **References**: [PURPOSE.md](PURPOSE.md), [OVERVIEW.md](OVERVIEW.md).
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T23:23:02+07:00

---

## 1. Tài liệu cốt lõi

| File | Nội dung |
| :--- | :--- |
| [PURPOSE.md](PURPOSE.md) | Đăng ký đề tài (10 mục), phân công, mốc, yêu cầu báo cáo — **nguồn sự thật** |
| [OVERVIEW.md](OVERVIEW.md) | Roadmap theo phase, trạng thái, checkpoint tốt nhất |
| [DATA.md](DATA.md) | Đặc tả IMDb, giao thức split, kiểm tra rò rỉ, GloVe |
| [MODELS.md](MODELS.md) | Danh mục mô hình M0a–M4b (+ mở rộng E1–E3), số tham số, checkpoint |
| [REPORT_CHECKLIST.md](REPORT_CHECKLIST.md) | Yêu cầu báo cáo → artifact → trạng thái |
| [MD_PROMPTS.md](MD_PROMPTS.md) | Mỗi file `.md` làm gì và prompt để AI tạo/cập nhật |

## 2. Thư mục con

| Thư mục | Nội dung |
| :--- | :--- |
| [phases/](phases/) | Đặc tả 6 phase: [DATA_PREP](phases/DATA_PREP.md), [BASELINE](phases/BASELINE.md), [MODEL](phases/MODEL.md), [TRAINING](phases/TRAINING.md), [EVAL](phases/EVAL.md), [DEMO_REPORT](phases/DEMO_REPORT.md) |
| [progress/](progress/) | `<PHASE>_STATUS.md` — tiến độ, blocker, bước tiếp theo của từng phase |
| [experiments/](experiments/) | Chỉ mục và báo cáo thí nghiệm EXP-01 → EXP-05 (+ mở rộng EXP-06 → EXP-09), bảng so sánh với baseline |
| [bugs/](bugs/) | Báo cáo lỗi `BUG_<NN>_*.md` và chỉ mục |
| [references/](references/) | [GIT_WORKFLOW.md](references/GIT_WORKFLOW.md) — quy trình Git cho nhóm |
