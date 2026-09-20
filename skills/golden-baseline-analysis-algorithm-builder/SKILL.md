---
name: golden-baseline-analysis-algorithm-builder
description: Create or upgrade a safe declarative time-series analysis algorithm SKILL.md for the Golden Baseline Management System from natural-language equipment, process, signal, and deviation requirements. Use when a new deterministic algorithm or a newer version is needed without asking the user to choose mathematical models.
metadata:
  schema_version: golden-baseline-analysis-algorithm/v1
  skill_role: generator
---

# 黃金基線時序分析算法建置器

把領域需求轉成黃金基線管理系統可驗證、可版本化與可稽核的宣告式算法 Skill。

## 使用者互動

- 只詢問缺少的工藝事實：設備、階段、訊號意義與單位、採樣間隔、預期偏差、事件錨點與期望判斷。
- 不得要求使用者命名統計或訊號處理算法。
- 選擇滿足需求的最小確定性方法，並用一般語言說明限制。
- 升級現有 Skill 時保留 `algorithm_id`，依相容性增加語意版本。
- 最終只輸出一份完整 Markdown；不得輸出 patch、Python、SQL、Shell、JavaScript、網址或安裝指令。

## 必要 Frontmatter

```yaml
---
name: readable-skill-name
description: Clear trigger description for this analysis algorithm.
metadata:
  schema_version: golden-baseline-analysis-algorithm/v1
  skill_role: analysis-algorithm
  algorithm_id: stable_lowercase_identifier
  version: 1.0.0
  engine_api_version: deterministic-feature-api/v1
  implementation_kind: declarative_pipeline
---
```

- `algorithm_id` 必須符合 `^[a-z][a-z0-9_]{2,63}$`。
- `version` 必須是嚴格的 `MAJOR.MINOR.PATCH`。
- 新算法使用 `declarative_pipeline`；只有系統已供應的算法可以使用 `builtin_reference`。

## 必要章節

每個章節必須且只能出現一次：

1. `# <Algorithm Name>`
2. `## Purpose`
3. `## Inputs`
4. `## Applicability`
5. `## Calculation Contract`
6. `## Quality Gates`
7. `## Output Contract`
8. `## Golden Fixtures`
9. `## Safety`

## 算法契約

- Inputs：使用訊號角色，不得綁定固定 EDC 位址、帳號、SUID 或 CUID。
- Applicability：說明適用的階段、訊號與資料品質，以及必須停用的情況。
- Calculation Contract：只使用 resample、interpolate、normalize、smooth、difference、integrate、aggregate、correlate、lag-search、envelope-check、threshold、score；以文字或數學式定義參數、單位與邊界，不得含可執行程式。
- Quality Gates：定義覆蓋率、時間單調性、最大缺口、最少點數、單位相容及信心規則；未通過時輸出 `insufficient_data`。
- Output Contract：包含 `algorithm_id`、`algorithm_version`、`status`、`score`、`unit`、`evidence_range`、`quality` 與可讀證據。狀態只可為 `normal`、`attention`、`abnormal`、`insufficient_data`、`not_applicable`。
- Golden Fixtures：至少提供正常、異常與資料不足三個宣告式測試案例，不得嵌入測試程式碼。
- Safety：聲明不得存取網路或憑證、執行程式、修改 EDC 資料，且不能取代安全關鍵工程判斷。

## 版本規則

- PATCH：文字、案例或相容門檻修正，不改必要輸入與結果語意。
- MINOR：向下相容的新能力或可選輸入。
- MAJOR：輸入、計算、輸出、單位或判斷語意不相容。
- 不得降低版本，也不得以相同版本承載不同內容。

## 最終檢查

輸出前確認 frontmatter、必要章節、穩定算法識別碼、語意版本、確定性計算、品質閘門、三類案例與安全邊界。只輸出完整 `SKILL.md`。

