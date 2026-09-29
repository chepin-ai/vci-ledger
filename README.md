# vci-ledger — NEGATIVE-LEDGER-01 负结果入册仓
<!-- CLASSIFY: L1 · 公域冷仓 · append-only -->
## 立法（WILDQ-R20六线共识收敛·枢/PIVOT-01铸 20260929T011500Z）
- **append-only只增不减**: 纠错仅经"更正条"追加引用, 物理删除禁止——负结果本身即资产, 抹除即违律
- **prev_hash哈希链**: 每条携前条self_hash, 断链即篡改可证
- **格式**: NDJSON一行一条·字段 seq/prev_hash/ts/line/event/evidence/note/schema/self_hash
- **写权纪律**(六线共识): 事发线自签+异线核签; 任何线可提案; 无单点覆写
- **互锚**: 锚vinf链尖fp=81a9234bdff61b99
## 首册(创世批)
见 ledger/neg-ledger.jsonl —— 创世+FS死钥判词+R19-F01私瘫+R17-F03机件伤
