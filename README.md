# EW Golden Baseline Model

<p align="center">
  <img src="assets/logo.jpg" alt="Embodied Worker logo／具象職人商標" width="320">
</p>

Natural-language algorithm selection and versioned time-series analysis Skills for Golden Baseline systems.

## English

### What this plugin does

EW Golden Baseline Model helps industrial users define deterministic time-series comparison methods without requiring them to understand or choose mathematical models.

It provides two Codex Skills:

- **Golden Baseline Algorithm Selector** — converts equipment, process, signal, event, and deviation requirements into a reusable process-specific algorithm-selection Skill.
- **Golden Baseline Analysis Algorithm Builder** — creates or upgrades a declarative, validated, and versioned time-series analysis algorithm Skill.

### Design principle

> Python computes deterministically, AI explains and configures, and engineers make the decision.

The plugin does not run arbitrary uploaded code. It generates declarative Skill specifications that must still pass the Golden Baseline Management System's schema, compatibility, version, and administrator-review controls.

### Typical workflow

1. Describe the equipment, process stages, signals, units, event markers, and comparison goal in plain language.
2. Let the selector determine the smallest suitable set of deterministic algorithms.
3. Generate a process-specific selector Skill or a new analysis-algorithm Skill.
4. Upload the generated Markdown to the Golden Baseline Management System.
5. The system validates the schema, algorithm identity, semantic version, engine API, required sections, and safety boundary.
6. An administrator installs or upgrades only a compliant candidate.

### Included Skills

| Skill | Purpose |
|---|---|
| `golden-baseline-algorithm-selector` | Select algorithms from process semantics and data conditions. |
| `baseline-analysis-builder` | Create or upgrade a declarative time-series algorithm Skill. |

### Safety boundary

- No EDC address, account, password, API key, SUID, or CUID is embedded in this repository.
- Generated Skills must not contain executable Python, SQL, Shell, JavaScript, credential access, or network operations.
- Generated algorithms do not become active merely because ChatGPT created them.
- Production activation remains subject to backend validation and administrator approval.
- Algorithm results do not replace engineering approval for safety-critical decisions.

### Repository structure

```text
.
├── .codex-plugin/plugin.json
├── assets/logo.jpg
└── skills
    ├── golden-baseline-algorithm-selector
    │   ├── SKILL.md
    │   └── agents/openai.yaml
    └── baseline-analysis-builder
        ├── SKILL.md
        └── agents/openai.yaml
```

### Installation

Install this repository as a Codex plugin using your approved local or organizational plugin marketplace workflow. Start a new Codex task after installation so the two Skills can be discovered.

### Copyright

Copyright © 2026 Embodied Worker Co., Ltd. All rights reserved.

---

## 中文

### 外掛用途

EW Golden Baseline Model 協助工業使用者定義確定性的時序比對方法，不要求使用者理解或自行選擇數學模型。

外掛包含兩個 Codex Skill：

- **黃金基線算法選擇**：把設備、工藝、訊號、事件與偏差需求，轉成可重複使用的工藝專用算法選擇 Skill。
- **時序分析算法建置與升級**：建立或升級宣告式、可驗證及可版本化的時序分析算法 Skill。

### 設計原則

> Python 負責確定性計算，AI 負責解釋與配置，工程師負責做決定。

此外掛不執行任意上傳程式碼。它只產生宣告式 Skill 規格；產生結果仍須通過黃金基線管理系統的格式、相容性、版本與管理員審核。

### 標準使用流程

1. 使用自然語言描述設備、工藝階段、訊號、單位、事件錨點及比較目的。
2. 由算法選擇 Skill 判斷最小且適用的確定性算法組合。
3. 產生工藝專用選擇 Skill，或新的時序分析算法 Skill。
4. 將生成的 Markdown 上傳至黃金基線管理系統。
5. 系統驗證 schema、算法識別碼、語意版本、引擎 API、必要章節與安全邊界。
6. 只有符合規範的候選版本才可由管理員安裝或升級。

### 內含技能

| Skill | 用途 |
|---|---|
| `golden-baseline-algorithm-selector` | 依工藝語意與資料條件自動選擇算法。 |
| `baseline-analysis-builder` | 建立或升級宣告式時序分析算法 Skill。 |

### 安全邊界

- Repository 不包含 EDC 位址、帳號、密碼、API Key、SUID 或 CUID。
- 生成的 Skill 不得包含可執行的 Python、SQL、Shell、JavaScript、憑證存取或網路操作。
- ChatGPT 產生算法 Skill，不代表算法已經啟用。
- 正式啟用仍須通過後端驗證及管理員核准。
- 算法結果不能取代安全關鍵工作的工程師判斷。

### 安裝

請透過核准的本機或組織 Codex marketplace 流程安裝此 repository。安裝後開啟新的 Codex 工作階段，讓系統載入兩個技能。

### 版權

版權所有 © 2026 具象職人股份有限公司。保留所有權利。
