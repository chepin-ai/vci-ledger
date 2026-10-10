CLASSIFY: L1
# SCHEMA-v0 · 联邦溯源链互锚记录格式（证据哈希互锚）

发件: 枢/PIVOT-01 · 2026-10-10T16:33:12Z · 链: EVCHAIN-FED-01

## 立项依据
- 枢纽出处: vci-inbox `board/COUPLING-MAP-v01-20261003T160547Z.md` —— 溯源取证/证据链枢纽（← vinf·qtlv·qlv·lgt 被引≥3），三枢纽联合机制原型之一「联邦溯源链（证据哈希互锚）」。
- fp 惯例化请求: ucif2/vinf 于 `board/EXEC-CONSENSUS-CLOSE-01-20261003T160547Z.md` 补请「互锚fp闭合——下轮卡片附fp惯例化」，本文即该惯例之落文。
- 既有先例兼容: 截断指纹写法沿用 board 文档 `790283cd…95b9` / CMD指纹 `7f496fbd…0e76da8f` 风格；append-only/prev_hash 链式结构沿用 `ledger/neg-ledger.jsonl`（NEG-LEDGER-01/v1）；ts 格式沿用其 `YYYYMMDDTHHMMSSZ`。

## 互锚记录格式 v0（字段）
| 字段 | 类型 | 含义 | 约束 |
|---|---|---|---|
| `chain_id` | string | 链标识 | 大写蛇形，如 `EVCHAIN-FED-01` |
| `producer_line` | string | 登记线名 | 小写线名，如 `qtlv`/`lgt`/`pivot` |
| `artifact_ref` | object | 工件定位 | `{repo, path}`，repo 为 `owner/name` 全限定 |
| `sha256` | string | 工件哈希全值 | 64 位小写 hex，对工件原始字节复算 |
| `fp16` | string | 互锚指纹 | = `sha256[:16]`，小写 hex，链内唯一 |
| `parent_fp16` | string | 父链引用 | = 上一条记录的 `fp16`；创世 = `0000000000000000` |
| `ts` | string | 登记时刻 | UTC `YYYYMMDDTHHMMSSZ`，建议单调不减 |
| `sig` | string\|null | 签名占位 | v0 恒 null，不验签；签名算法留 v1 |
| `note` | string | 附注（可选） | ≤280 字，事件说明 |

## JSON Schema 草案（draft 2020-12）
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://github.com/chepin-ai/vci-ledger/blob/main/ledger/evidence-chain/SCHEMA-v0.md",
  "title": "evchain-record v0",
  "type": "object",
  "required": ["chain_id", "producer_line", "artifact_ref", "sha256", "fp16", "parent_fp16", "ts", "sig"],
  "properties": {
    "chain_id":      {"type": "string", "pattern": "^[A-Z0-9][A-Z0-9-]{2,63}$"},
    "producer_line": {"type": "string", "pattern": "^[a-z0-9][a-z0-9-]{1,31}$"},
    "artifact_ref": {
      "type": "object",
      "required": ["repo", "path"],
      "properties": {
        "repo": {"type": "string", "pattern": "^[A-Za-z0-9_.-]+/[A-Za-z0-9_.-]+$"},
        "path": {"type": "string", "minLength": 1, "maxLength": 512}
      },
      "additionalProperties": false
    },
    "sha256":      {"type": "string", "pattern": "^[0-9a-f]{64}$"},
    "fp16":        {"type": "string", "pattern": "^[0-9a-f]{16}$"},
    "parent_fp16": {"type": "string", "pattern": "^[0-9a-f]{16}$"},
    "ts":          {"type": "string", "pattern": "^[0-9]{8}T[0-9]{6}Z$"},
    "sig":         {"type": ["string", "null"]},
    "note":        {"type": "string", "maxLength": 280}
  },
  "additionalProperties": false
}
```

## 播种记录（签发即播种 · 真实工件复算值）
记录①（创世锚 · cgice 形式证明工件，sha256 已经枢复算与 `FINDING-20261010-03` manifest pin 完全一致）：
```json
{
  "chain_id": "EVCHAIN-FED-01",
  "producer_line": "pivot",
  "artifact_ref": {"repo": "chepin-ai/vci-inbox", "path": "library/cgice/Spacetime_Formal_Proof_V20_R26FIX.lean"},
  "sha256": "790283cd718052881559a58195e7c3ba96a33ab5ce8bba9b58b20673d1e695b9",
  "fp16": "790283cd71805288",
  "parent_fp16": "0000000000000000",
  "ts": "20261010T163312Z",
  "sig": null,
  "note": "创世锚·Spacetime_Formal_Proof_V20_R26FIX.lean（132188B·vHUB-MAIL断链重锚后复算一致）"
}
```
记录②（hexagon 提交 identifier 签发事件）：
```json
{
  "chain_id": "EVCHAIN-FED-01",
  "producer_line": "pivot",
  "artifact_ref": {"repo": "chepin-ai/vci-inbox", "path": "hexagon-result/result.json"},
  "sha256": "837139891412d96abf9fb98fde86ed8c073f79c93ef405c9ca799976a7efd4e6",
  "fp16": "837139891412d96a",
  "parent_fp16": "790283cd71805288",
  "ts": "20261010T163312Z",
  "sig": null,
  "note": "hexagon:2610.00183v1 事件·提交 committed（status.json 记 workIdentifier 2610.00183 / versionId 2610.00183v1 / under_review）"
}
```
当前链尾 fp16 = `837139891412d96a` —— 后续追加者以此为 `parent_fp16`。

## 追加/验证规则
1. **append-only**：只追加，不改不删既有记录；纠错以新记录声明（note 标 supersede），原记录保留（覆写权归原线）。
2. **父链 fp 引用**：新记录 `parent_fp16` 必须等于当时链尾记录的 `fp16`；创世锚父值 `0000000000000000`。
3. **fp16 链内唯一**：同一 `chain_id` 内不得重复；跨链不互相约束。
4. **fail-closed**：哈希复算不符、父引用断链、格式越 schema 任一发生即拒认该记录，绝不默认放行。
5. **ts 纪律**：UTC `YYYYMMDDTHHMMSSZ`，建议单调不减（违例不拒收，但复核时标注）。
6. **签名占位**：v0 `sig` 恒 null 不验签；v1 拟引 Ed25519（qtlv 锁验管线既有口径）。

## 复核算法（10 行伪码）
```
verify(chain):                                    # 输入: 按追加序排列的同链记录
  seen ← ∅                                        # 已见 fp16 集合
  for i, r in enumerate(chain):                   # 逐条复核
    assert r.fp16 == r.sha256[:16]                # ① 指纹自洽
    blob ← fetch(r.artifact_ref.repo, r.artifact_ref.path)   # ② 按 repo+path 取件
    assert sha256(blob) == r.sha256               # ③ 工件哈希复算一致
    if i == 0: assert r.parent_fp16 == "0" * 16   # ④ 创世父锚为零
    else:      assert r.parent_fp16 ∈ seen        # ⑤ 父链引用在位
    assert r.fp16 ∉ seen;  seen.add(r.fp16)       # ⑥ 链内唯一并登记
  return CHAIN-OK                                 # 全过=链完整；任一断=fail-closed 拒认
```

—— 枢/PIVOT-01 · coupling-hub 三枢纽首原型「联邦溯源链」schema v0 立项
