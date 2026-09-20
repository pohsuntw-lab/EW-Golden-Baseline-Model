---
name: golden-baseline-algorithm-selector
description: Select deterministic golden-baseline comparison algorithms and generate a reusable process-selector SKILL.md from a plain-language equipment or process description. Use when users should not need to understand RMSE, correlation, DTW, lag, slope, area, stability, or missing-data methods themselves; do not diagnose equipment faults.
---

# 黃金基線算法選擇技能產生器

把使用者對設備、工藝、訊號與比較目的的自然語言描述，轉成一份可重複使用的專用 `SKILL.md`。不要要求使用者選擇統計或訊號處理算法。

## 收集工藝事實

只詢問使用者能以領域知識回答的缺漏資訊：

- 設備用途、正常工藝階段、事件如何開始與結束。
- 訊號名稱、物理意義、單位、採樣間隔與是否跨設備雲。
- 哪些偏差會影響品質、能耗、產量、安全或穩定運轉。
- 是否有 PLC、批次、Tagger 或人工事件錨點。
- 歷史封閉事件、即時開放事件，或兩者都需要。

不得要求使用者指定 RMSE、DTW、Cross-correlation、門檻公式或距離函數。不得自行發明安全極限、法規門檻或設備供應商規格。

## 選擇規則

- `missing_deviation`：所有時序都選；品質不足時，其他算法不得輸出正常結論。
- `time_alignment`：跨來源、多通道或需要圖層疊圖時選。
- `value_deviation`：連續數值且單位相容時選。
- `time_deviation`：有可信起點、終點、階段或峰值錨點時選。
- `waveform_deviation`：相同工藝階段且點數足以表達形狀時選；DTW 不得掩蓋真正時間延遲。
- `slope_deviation`：上升／下降速率有物理意義且採樣密度足夠時選。
- `area_deviation`：對時間積分有明確業務意義時選，例如累積能耗或流量。
- `stability_deviation`：穩態或平台階段選；正常升降載階段通常不選。

歷史封閉事件可選依賴終點的算法；即時開放事件只選不依賴終點的算法，事件結束後再重算完整結果。只能選系統提供的 `available_algorithms`；白名單外需求放入 `proposed_algorithms` 並標示 `requires_implementation`。

## 產生專用 Skill

輸出一個完整 Markdown 文件，包含：

1. `name` 與清楚觸發條件的 `description`。
2. `metadata.schema_version: golden-baseline-algorithm-skill/v1`。
3. `metadata.skill_role: process-selector`。
4. 適用／不適用範圍、工藝階段、訊號角色、必要輸入與品質閘門。
5. 每個算法的選用與不選用規則、原因碼，以及歷史／即時模式差異。
6. 白名單限制、安全邊界與待確認資訊。
7. 固定的候選政策 JSON 輸出契約。

新 Skill 必須自包含，不可依賴本產生器仍在對話中，也不可包含 Python、SQL、Shell、網址或外部工具呼叫。

## 候選政策契約

專用 Skill 被使用時，只輸出一個 JSON 物件，至少包含：

```json
{
  "schema_version": "golden-baseline-algorithm-plan/v1",
  "selector_skill_name": "string",
  "selector_skill_version": "string",
  "equipment_type": "string",
  "process_name": "string",
  "execution_mode": "historical_closed_event",
  "policy_status": "ready",
  "stage_plans": [],
  "quality_gates": {},
  "required_information": [],
  "proposed_algorithms": [],
  "assumptions": [],
  "review_notes": []
}
```

`execution_mode` 只能是 `historical_closed_event` 或 `realtime_open_event`。`policy_status` 只能是 `ready`、`review_required` 或 `insufficient_information`。

## 最終輸出

產生完整的新 `SKILL.md`，不要只提供大綱。若可建立檔案就直接建立；否則只輸出一個從 YAML frontmatter 開始的 Markdown 程式碼區塊。
