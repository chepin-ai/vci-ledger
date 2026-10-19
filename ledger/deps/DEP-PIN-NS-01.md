CLASSIFY: L1
# DEP-PIN-NS-01 · NS 解决方案依赖钉定簿（M1-2 / L0-2）
枢/PIVOT-01 · 2026-10-17 立案 EXT-WAVE-15「构」EXT18 放行件 · 2026-10-19 EXT-WAVE-16「渡」目标态达成改写

> 哈希方案对偶律：git blob sha1 与 content sha256 双制并录，漂移核验同制比对。

## 一、内部依赖钉（vci-inbox library/ns/ @main）

**现态（2026-10-19 转正后，commit 0027e36f）**：

| 件 | git blob sha1 | content sha256[:16] | 值 |
|---|---|---|---|
| lean-toolchain | a6aff065db2882c5 | ca5f1f601d34c490 | `leanprover/lean4:v4.35.0` |
| lake-manifest.json | 4255e2efc33fb432 | c3981e3ee1f71c18 | mathlib v4.35.0 锁定集（@56a005855ea17e0b0958f73ec801ae140b0a1679；+plausible/+LeanSearchClient） |
| lakefile.lean | 73b7b359cf7e93e6 | 3cc4a6cb853a8e0e | require mathlib @ "v4.35.0" |

**历史态（2026-10-17 → 2026-10-19）**：v4.10.0 钉（lean-toolchain 7f0ea50aaa500c64 / manifest 6a0f97421abf58dd / lakefile f1d169def08f4569）。

目标态达成记录：M1-1 spike 三项全绿（runs 39211461961 首证 / 39214080542 复核；Comparator 核心并轨 + 内核修复 #15241/#15288/#15289 在版）→ EXT-WAVE-16「渡」转正（LAB-CLOSE-EXT16-01）。

## 二、外部锚钉（一律 commit 钉，不钉 branch —— Phase 4 R4 律）

| 锚 | 钉 | 用途 | 门禁 |
|---|---|---|---|
| wwang-math/FluidGalerkinLean | 4d023ac47aea65a4a5dbbc20004f5fda0ee18755 | BLOCKED-NS-1 工程移植思想源 | **无许可证** → CLEANROOM-REDLINE-FGL-01 双人制 |
| google-deepmind/formal-conjectures | 8bf45ed | 陈述层生态先例（OpenAI ComparatorDefinitions 所钉同一 commit） | 参照件 |
| openai/NavierStokesAndEuler | f9e8bc5 | OBL-NS-02 审计对象（2610.08144 同钉） | 钉死防 force-push；复跑产物哈希归档 |
| IlPreteRosso/RadiiPolynomial | 162f（仓库当前私有，404） | OBL-NS-03 Fourier 扩展 | **Gate 0 = 可见性**，过闸前不启动 |
| Lean 工具链 | leanprover/lean4:v4.35.0 | 验证层基座 | **已转正**（iter 11/13 全绿） |
| LeanCert | v4.35.0（2026-10-15 释出） | CapAnchor 外部证明层 | 注意其论文钉 4.33.0 版本漂移 |

## 三、核验规程
- 每次见证窗口开启前：同制比对本簿与仓内实况；漂移即 FINDING 申报。
- 外部锚变动（新 commit / 撤稿 / 许可证变化）：先入监测簿，判定席裁决后方许改钉。
——枢/PIVOT-01
