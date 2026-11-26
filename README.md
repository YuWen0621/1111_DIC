# Personal Weather Broadcaster｜個人化天氣助理

本儲存庫收錄「個人化天氣助理」專題提案的系統性分析、摘要與高密度資訊圖表索引。

## 專題定位
臺灣中央氣象署（CWA）與環境部（MOENV）雖提供豐富氣象、空氣品質開放資料，使用者仍需自行跨平台整合與解讀。本專題規劃 Domain AI Agent，將多站行程、地點與時段、個人偏好、活動情境與環境資料整合為可執行且可追溯的行前建議。

## 系統設計
- **數據來源**：CWA 天氣預報、雨量、紫外線、警特報；MOENV AQI、污染物觀測及空品預報。
- **資料處理**：地點/時段標準化、時空對齊、資料新鮮度與缺漏標記（missing/stale/spatially-coarse）。
- **規則決策引擎**：降雨、UV、空品與冷熱舒適判定，保留決策依據與證據。
- **LLM Agent**：需求解析、澄清追問、自然語言解釋；不得編造環境數據、改寫規則判定或將即時值當作預報。
- **個人化**：UserProfile 明確偏好，行後問卷產生 PreferenceUpdateCandidate，待使用者確認。
- **結果呈現**：主要建議 → 簡短原因 → 原始數據／來源／限制，並提供跨站行程風險彙整。

## 11 大系統模組
1. 使用者介面
2. 行程與需求解析
3. 地點與時段標準化
4. 帳號、偏好與記憶管理
5. 氣象與空品資料取得
6. 資料品質與時空對齊
7. 規則決策引擎
8. 行程建議彙整器
9. LLM 解釋與追問處理
10. 結果呈現
11. 行後問卷與偏好更新候選

## 研究架構
- **Abstract A–F**：Attention / But / Cure / Development / Experiments / Findings
- **Introduction G–L**：背景、個體差異、問題、限制、創新、貢獻
- **Related Work**：AI Agent、個人化記憶與領域知識（UTCI、PM2.5、UVI、WMO 影響導向預報）
- **Deep Research**：5 個核心思維、3 個專業爭論、10 個深度理解問題
- **Proposed Scheme**：11 大模組、核心流程與偽代碼
- **Evaluation**：三組對照實驗與情境評估

## 實驗規劃（尚未完成）
| 比較組 | 設計 |
|---|---|
| Baseline A | 官方原始數據 |
| Baseline B | 固定規則＋模板 |
| Proposed | 規則＋個人偏好＋LLM |

預計情境：多站行程、高降雨、高 UV、相同 AQI 不同污染物、資料缺漏／過期。

預計指標：正確性、完整性、個人化、可理解性、資料時效處理、規則遵循率、幻覺率與使用者接受度。

> **說明**：本專題目前資料重點為提案與系統設計；所有預期成果不應視為已完成實驗結果。原始 PDF 與 PNG 附件尚需透過支援二進位檔案的上傳管道提交。


<WritingBlock id="59317" variant="standard">### DIC3 – Project Proposal Refactor Using OpenSpec

This assignment refactors the Personalized Weather Assistant research proposal using OpenSpec's spec-driven development workflow.
- OpenSpec Change: openspec/changes/personalized-weather-assistant/
- Generated Artifacts: Proposal, six capability specifications, system design, and implementation tasks.
- Research Refactoring: PROPOSAL_REFACTOR.md, including the A–F review, 5 Core Mindsets, 3 Trade-offs, 10 Detailed Questions, and revised Introduction and Related Work.
- Validation: OpenSpec strict validation passed.
- Status: Proposal and specification stage only; implementation and experimental evaluation are planned for future work.</WritingBlock>

## 圖表
<img width="1491" height="1055" alt="個人化天氣助理系統解析資訊圖表" src="https://github.com/user-attachments/assets/a4bb453e-de71-47f4-a10d-05459af73a05" />

# Personalized Weather Assistant: A Data-Grounded and Context-Aware Hybrid AI Agent for Personalized Environmental Risk Assessment and Pre-Trip Decision Support

**Abstract—** Weather forecasts and air quality information play an essential role in daily activity planning, commuting, and outdoor decision-making. Although Taiwan's Central Weather Administration (CWA) and Ministry of Environment (MOENV) provide extensive environmental open data, users must manually retrieve, integrate, and interpret information from multiple platforms to make appropriate decisions. Existing weather and air quality services primarily provide generalized forecasts and standardized alerts, with limited consideration of multi-stop itineraries, individual preferences, activity contexts, and personal environmental tolerances. Furthermore, the direct application of large language models (LLMs) to environmental decision-making introduces challenges related to hallucination, numerical reliability, and the traceability of generated recommendations. To address these limitations, this paper proposes a Personalized Weather Assistant, a data-grounded, context-aware hybrid AI agent designed to transform heterogeneous environmental information into personalized, actionable, and explainable pre-trip recommendations. The proposed framework integrates official weather forecasts, ultraviolet radiation information, air quality indices, pollutant observations, user preference profiles, and itinerary-specific contextual information. A rule-based decision engine performs deterministic assessments of precipitation, ultraviolet exposure, air pollution, thermal comfort, and data quality, while an LLM-based interaction module handles natural language understanding, clarification, recommendation explanation, and follow-up queries. This separation ensures that environmental risk assessments remain governed by verifiable domain-specific rules rather than unconstrained language generation. The system further incorporates spatiotemporal data alignment, data freshness validation, multi-stop risk aggregation, and post-trip feedback mechanisms to support reliable and adaptive recommendations. A comparative evaluation is planned using three configurations: raw environmental data presentation, fixed rule-based recommendations with templates, and the proposed hybrid AI agent. Evaluation scenarios will include multi-stop itineraries, heavy precipitation, high ultraviolet exposure, pollutant-specific air quality conditions, and incomplete or outdated environmental data. The assessment will examine recommendation correctness, completeness, personalization, interpretability, rule compliance, and robustness to data quality limitations. The proposed framework is expected to reduce the burden of cross-platform information retrieval while improving the relevance, transparency, and practical utility of environmental decision support. This study establishes a structured foundation for developing trustworthy, context-aware AI agents that integrate domain knowledge, environmental data analytics, and natural language interaction for personalized daily activity planning.

**Index Terms—** Air quality monitoring, context-aware systems, decision support systems, domain-specific AI agents, environmental risk assessment, explainable artificial intelligence, large language models, personalized recommendations, rule-based reasoning, weather forecasting.
