# data/

Không commit dữ liệu thô, GloVe, split hay ma trận đã xử lý. Chỉ commit `sample/`.

```
data/
├── raw/aclImdb/          # giải nén aclImdb_v1.tar.gz (src/data/download.py)
├── raw/rotten_tomatoes/  # chỉ khi làm mở rộng E4
├── splits/               # train.csv, val.csv, test.csv (src/data/split.py)
├── glove/                # glove.6B.100d.txt
├── processed/            # glove_100d_vocab.npy (src/models/glove.py)
└── sample/               # commit
    ├── sample_100.csv    # 100 review từ test, có ca khó
    └── handwritten_30.csv # ~30 câu tự viết (nhóm tình huống 7)
```

Đặc tả: [docs/DATA.md](../docs/DATA.md).
