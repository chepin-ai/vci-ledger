CLASSIFY: L1
# QFOS-SCHEME-REG-01 · qf-os:// URI 解析规则登记件（在册生效）

- 登记席：QF-OS 联邦 W5 册仓线席（EXT-WAVE-20 批八·完成度线）
- 登记时间戳：2026-10-24T01:39:00Z（UTC(Z) 单时钟域，`date -u` 实测）
- 状态：**已登记生效**——草案 `vci-inbox:board/QFOS-SCHEME-REG-01.md`（2026-10-23 呈）经判定席（乙席）ADJUD-20261023-02 案 (b) 双轮律程序裁定：b-R1 准注册生效；R2 定向复决「注册生效本身：维持」「12-hex 使用范围：修正（BC-B1）」「fail-closed 处置方式：维持 + 补强（BC-B2）」「U-04 销账出路：择定内部登记 + 锚例升格 40-hex（BC-B3）」。本件 = 草案六节成文 + 三补强条款（BC-B1/B2/B3 逐字）+ 裁决引，为 qf-os:// scheme 唯一在册解析依据。
- 裁决引：ADJUD-20261023-02 案 (b) R2 终裁（终裁矩阵行 (b)：「**准注册生效**：草案六节照成文口径准予登记；U-04 择定『内部登记 + 锚例升格 40-hex』出路」，执行义务归属线：册仓 vci-ledger 线登记义务——本件即该义务之履行）。
- 销账对象（依 BC-B3）：formalization.yaml 归属线完成锚例升格 40-hex 且本规则经本仓登记在册（本件）后，U-04 转销账流程。
- 锚例（library/ns/formalization.yaml L25，逐字）：`qf-os://vci-inbox/board/NS-PROOF-FRAME-01.md@58dfb9a94311`

## 一、scheme 语义

`qf-os` 为 QF-OS 联邦内部治理对象引用 scheme，**仅具内部登记语义，无外部协议登记**（证据件检索式③「"qf-os" protocol scheme URI git」0 命中可证）。语义：引用联邦在册仓/线中、特定提交钉下的特定治理文档/工件，供 yaml sources、帧证据锚、裁决格登记等内部场景机读解析。**不构成**任何外部可解析性承诺；对外发布场景须同时给出 https 可公开获取形态或 40-hex 全钉。

## 二、语法结构（ABNF 风格）

```
qf-os-uri = "qf-os://" authority path [ "@" pin ]
authority = 1*( ALPHA / DIGIT / "-" )        ; 仓/线标识，见 §三
path      = "/" *( pchar / "/" )             ; 仓内相对路径，UTF-8
pin       = 40HEXDIG / 12HEXDIG              ; commit 钉，见 §四
```

三段俱全方为佳例；缺 `@pin` 者按「浮动引用」登记，解析结果为 authority 默认线之当前 HEAD——**浮动引用不得用于裁决格证据锚**（证据锚必带钉，帧 v1.1/v1.2 附表体例）。

## 三、authority 映射（仓/线）

- authority 段映射联邦在册**仓标识**（仓级）或**线标识**（线级，指脉冲线/工作线命名实体）；当前唯一在册实例：`vci-inbox`（仓级，即 vci-inbox 仓）。
- 映射表为内部登记项：authority →（仓本地路径，远端脉冲链标识）。新增仓/线标识须先入映射表再用于 URI，否则解析器按未知 authority 拒绝（fail-closed，同 WS-37 见证机纪律）。
- 解析即**本地仓内解析**：`qf-os://vci-inbox/<path>@<pin>` ≡ 在 vci-inbox 仓内、提交 `<pin>` 之树对象中取 `<path>` blob。

## 四、@<40-hex|12-hex> 钉的解析与唯一性规则

1. **两式并容**：`@<40-hex>` 为 git 全钉（SHA-1 全 40 位），全局唯一、跨仓可移植，为**首选式**；`@<12-hex>` 为短钉，**仓库相对**使用，仅在本仓内、且经本条 2–4 唯一性核验后成立。
2. **官方依据（逐字引文，不转述）**：
   - Pro Git《Git Tools - Revision Selection》（git-scm.com/book/en/v2/Git-Tools-Revision-Selection）：「Git can figure out a short, unique abbreviation for your SHA-1 values… it defaults to using seven characters but makes them longer if necessary to keep the SHA-1 unambiguous」「**Generally, eight to ten characters are more than enough to be unique within a project.** … no two objects whose SHA-1s are identical in the first 12 characters.」
   - git-rev-parse 官方文档（git-scm.com/docs/git-rev-parse）：「--short[=<length>] Same as --verify but shortens the object name to a unique prefix with at least length characters. The minimum length is 4…」
   - 惯例层佐证（renenyffenegger.ch 注记）：「the recommendation is to up this value to 12 (at least for Linux Kernel development)」
3. **仓库相对性声明（明示纪律）**：短钉唯一性是**仓库相对且非永久**的——唯一性断言仅在解析时刻、该仓当前对象库内成立；仓历史增长（新对象进入）或重写（见 §五）均可使既有短钉歧义化或失效。故 12-hex 短钉之引用必须随载「解析核验时刻」与「核验仓状态锚」。
4. **解析与核验规程**（对 `@<12-hex>`）：
   - (a) 解析动作 = `git rev-parse --verify <12-hex>^{commit}`（等价唯一前缀解析）；
   - (b) 命中唯一 commit ⇒ 登记其 40-hex 全形为**解析凭据**，与 URI 随件同载（例：锚例 `58dfb9a94311` 于 2026-10-23 在本仓 HEAD e69b122fa 之对象库内解析唯一，全形 commit 信息留痕：「58dfb9a94 NS-PROOF-FRAME-01 v1.1: EXT18卡6落地……」）；
   - (c) 歧义（多对象同前缀）或失配（无对象）⇒ fail-closed 拒绝解析，登记事件，引用方须换 40-hex 全钉重锚；
   - (d) 锚例所涉文档另有内容级 SHA-256 三槽哈希在册者（palomar 注册表体例），内容哈希不因钉漂移失效，与 commit 钉互为冗余锚。

## 五、钉漂移衔接条款（PAL-0001-CORR 前科衔接）

前科（registry/palomar-registry-v1.yaml 勘误段 PAL-0001-CORR，2026-10-21，逐字要点）：PAL-0001 所钉 40-hex「1345a48ba81acb606be533dc25ec6bf4e96127ee」因本地链 rebase 嫁接到远端脉冲链，「原钉哈希在推送后的远端历史中不再出现（rebase 重写），构成钉漂移」；处置为「钉映射 1345a48ba… → 6358f1225（同『EXT-WAVE-17 收波』提交之 rebase 等价体）」，并以 (i) 子树 diff 零变化、(ii) 内容 SHA-256 续有效、(iii) 证据与提交对象不变 三项佐证等价性。

衔接条款：
1. qf-os 钉（无论 40-hex 或 12-hex）遇仓历史重写（rebase/force-push/链嫁接）即按 **PAL-0001-CORR 先例体例**办理：append-only 勘误段登记「旧钉 → 新钉」映射 + supersedes 链，**原 URI 一字不改、原钉不删**；
2. 映射登记必附三佐证：子树/路径 diff 零变化核验、内容 SHA-256 续有效核验、证据对象不变声明（缺一不开登记）；
3. 12-hex 短钉因长度更短，重写后歧义风险更高——凡涉短钉之漂移事件，映射登记同时升格该 URI 为 40-hex 全钉新锚（短钉形态仅作历史留痕）。

## 六、生效与接口

本规则经判定席双轮律程序（首轮全量判定卡；异见收割后定向补强、次轮定向复决）裁定生效（ADJUD-20261023-02 案 (b) R2 终裁），并经本件登记入册仓 vci-ledger 在册：后续帧/yaml/注册表之 qf-os 引用一律按本规则解析。U-04 销账：两可出路（内部登记 / 换 40-hex）已经裁决**择定「内部登记解析规则」为主出路**，并课锚例升格义务（见 BC-B3）；升格完成且本件登记在册后，U-04 转销账流程。

## 七、补强条款（ADJUD-20261023-02 §四，逐字落实，与上六节同具在册效力）

**BC-B1（12-hex 使用限制条款）**：
1. 12-hex 短钉**仅限仓内机读场景**使用（yaml sources、帧证据锚、裁决格登记之内部引用）；凡对外发布件、对外可访问镜像、跨仓引用，一律使用 40-hex 全钉或 https 形态，12-hex 形态在外发件中视为格式错误（对应 D-b1/D-b3）；
2. 12-hex 引用随载 40-hex 解析凭据为**强制项**（草案 §四4(b) 由「登记」升格为「缺一不成立」：无随载全形凭据之 12-hex 引用不予解析登记）；
3. 仓历史重写事件（rebase/force-push/链嫁接）发生后首批续扫，监视件必含「在册 12-hex 引用可达性普查」一项（事件驱动，W30 合规），失配即按草案 §四4(c) fail-closed 登记并呈裁，禁静默重锚。

**BC-B2（fail-closed 历史兼容条款）**：解析器对未知 authority 拒绝解析时必登记事件（authority 字面值 + 出处件 + 时点），不得静默丢弃；若登记事件揭示历史件含未注册 authority，处置路径为「先入映射表、再解析」，**禁止追溯改写历史件内 URI 原文**（append-only 纪律延伸，对应 D-b2）。

**BC-B3（锚例升格与 U-04 销账出路择定）**：两可出路中判定席**择定「内部登记解析规则」为主出路**，同时课升格义务——formalization.yaml v0.5 落版时 sources[0] 之 qf-os URI 由 12-hex 升格为 40-hex 全钉形态（58dfb9a94311 之全形以解析凭据留痕为准），短钉形态仅作历史留痕；升格完成且本规则经 vci-ledger 线登记在册后，U-04 转销账流程。

## 八、纪律自查

1. **append-only**：本件为册仓新增登记件，未删改册仓任何既有字节；本规则自身 §五 与 BC-B2 均承 append-only 纪律（原钉不删、禁追溯改写）。
2. **名值分离**：裁决（名）在 ADJUD-20261023-02，登记执行（值）在本件；锚例之 NS-PROOF-FRAME-01 仅作治理对象引用（名/路径），本件不对其内容作任何状态断言。
3. **NS/RH 钉律**：本件涉 NS 处仅为锚例对象名（formalization.yaml L25 之 URI 路径组件），不涉 NS 实质内容表述；无「NS 已解决」式表述，不触及任何 Clay/RH 相关表述，C1–C3/C1′–C3′ 无触发面。
4. **不 commit**：本件只写文不 commit，经 portal 落仓，枢统一收口；写后复读核验字节数。

——QF-OS 联邦 W5 册仓线席 · EXT-WAVE-20 批八（完成度线）· 2026-10-24 · 登记 ADJUD-20261023-02 案 (b) R2 终裁
