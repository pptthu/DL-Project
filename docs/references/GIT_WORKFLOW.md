# GIT_WORKFLOW.md — Quy trình Git cho nhóm 3 người

- **Motivation/Background**: Ba người cùng sửa một repo dễ ghi đè code nhau và commit nhầm dữ liệu lớn.
- **Purpose**: Quy ước nhánh, commit, PR và điều cấm.
- **Overview Pipeline**: Nhánh từ main → commit nhỏ → PR → TV1 review → merge.
- **Detailed Plan**: §1 Nhánh; §2 Commit; §3 Pull request; §4 Điều cấm.
- **References**: [../../README.md](../../README.md)
- **Created**: 2026-11-05T11:21:04+07:00
- **Last Updated**: 2026-11-05T11:21:04+07:00

---

## 1. Nhánh

- `main` luôn chạy được; không push thẳng lên `main`.
- Một nhánh cho một việc: `feat/<phase>-<viec>`, `fix/<loi>`, `docs/<tai-lieu>`, `exp/<NN>-<ten>`.
  - Ví dụ: `feat/data-split`, `feat/model-bilstm`, `exp/05-bilstm-glove`.

## 2. Commit

Theo Conventional Commits: `<type>(<scope>): <mô tả ngắn>`.

- `type`: `feat`, `fix`, `docs`, `test`, `refactor`, `chore`, `exp`.
- `scope`: `data`, `baseline`, `model`, `train`, `eval`, `demo`, `docs`.
- Ví dụ: `feat(data): chia val theo movie_url`, `exp(model): thêm BiLSTM + GloVe frozen`.

## 3. Pull request

- Mô tả PR gồm: làm gì, vì sao, cách kiểm tra (lệnh + kết quả), file `progress/` đã cập nhật.
- Chạy `python -m pytest tests/` trước khi mở PR.
- Cả nhóm cùng review code: TV1 review PR của TV2, TV3; TV2 review PR của TV1. Mỗi người phải tự giải thích được code mình viết.

## 4. Điều cấm

- Commit `data/raw/`, `data/splits/`, `data/glove/`, `data/processed/`, checkpoint (`*.pt`, `*.pth`, `*.joblib`), `.venv/`. Chỉ `data/sample/` được commit.
- Commit đường dẫn tuyệt đối (`C:\Users\...`) hoặc token/API key.
- Sửa số trong tài liệu mà không có file kết quả đi kèm.
