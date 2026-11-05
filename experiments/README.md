# experiments/

```
experiments/
├── checkpoints/<exp>/          # KHÔNG commit; ghi nơi lưu vào docs/MODELS.md
│   ├── vocab.json
│   └── seed_<s>/best.pt
├── runs/<exp>/seed_<s>/        # log, history.csv mỗi lần train
├── results/<exp>/              # commit
│   ├── seed_<s>/metrics.json, test_metrics.json
│   ├── summary.json, test_summary.json   # mean ± std 3 seed
│   └── predictions.csv
├── results/error_analysis/     # bảng theo nhóm tình huống, ví dụ sai — commit
└── plots/                      # loss/acc, confusion matrix, ROC, heatmap từ — commit
```

`<s>` ∈ {42, 123, 2025}. Baseline TF-IDF + LR lưu ở `checkpoints/baseline_tfidf_lr.joblib`, `results/baseline/`.

Báo cáo từng thí nghiệm: [docs/experiments/](../docs/experiments/).
