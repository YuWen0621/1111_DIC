# Project Proposal Refactor: Personalized Weather Assistant (DIC3)

> **Document Scope & Grounding Policy**: This document refactors the existing research proposal materials into structured academic English (IEEE style) and audits the research framing against [DESIGN.md](file:///d:/碩士/物聯網/1111_DIC/DESIGN.md) (primary technical reference), [ABSTRACT.md](file:///d:/碩士/物聯網/1111_DIC/ABSTRACT.md) (primary research reference), [IEEE_Abstract.md](file:///d:/碩士/物聯網/1111_DIC/IEEE_Abstract.md), [README.md](file:///d:/碩士/物聯網/1111_DIC/README.md), and [個人化天氣助理設計.pdf](file:///d:/碩士/物聯網/1111_DIC/個人化天氣助理設計.pdf). All original files remain unmodified. No references, numeric rule thresholds, technical stack commitments, or experimental results have been fabricated; unresolved items are explicitly marked `TBD`.

---

## Part I. Review of Research Elements (`DESIGN.md` & Supporting Materials)

### 1. Review of Abstract A–F Structure

The six-part A–F narrative in [ABSTRACT.md](file:///d:/碩士/物聯網/1111_DIC/ABSTRACT.md) aligns with the technical design in [DESIGN.md](file:///d:/碩士/物聯網/1111_DIC/DESIGN.md) and the English summary in [IEEE_Abstract.md](file:///d:/碩士/物聯網/1111_DIC/IEEE_Abstract.md):

| Element | Core Claim in `ABSTRACT.md` | Alignment with `DESIGN.md` & Status |
|---|---|---|
| **A. Attention Getter** | Taiwan's CWA and MOENV provide rich meteorological and air quality open data, yet users must manually query, compare, and interpret dispersed sources for daily trip planning. | Grounded in `DESIGN.md` §1 and §5.1 (CWA township forecasts; MOENV real-time stations & `AQF_P_01`). |
| **B. But (Gap)** | Existing services target single-location/single-period raw data or generic alerts without multi-stop itineraries, personal tolerances, or activity context; direct LLM usage suffers from hallucination, unstable tool calls, and unverifiable risk judgment. | Grounded in `DESIGN.md` §1.2, §2, and §7.3 (strict prohibition on LLM decision-making and data fabrication). |
| **C. Cure** | Proposes the *Personalized Weather Assistant* hybrid AI agent integrating CWA/MOENV open data for multi-stop, location-, timeframe-, activity-, and preference-aware pre-trip action advice. | Grounded in `DESIGN.md` §1.1, §3, and §4 (11 modules, multi-stop `ItinerarySegment` to `TripRecommendation`). |
| **D. Development** | Hybrid architecture: deterministic rule engine handles rain, UV, air quality (including pollutant differences), thermal comfort, and data-quality gating; LLM handles NL parsing, clarification, explanation, and follow-up; explicit profile + feedback candidate loop. | Grounded in `DESIGN.md` §4–§9. Concrete frameworks, API endpoints, and numeric rule thresholds remain `TBD` (§12). |
| **E. Experiments** | Plans three-way comparison (**Baseline A**: raw official data; **Baseline B**: fixed rules + templates; **Proposed**: hybrid rule engine + profile + grounded LLM) across multi-stop, high-rain, high-UV, equal-AQI/different-pollutant, and degraded-data scenarios. | Grounded in `DESIGN.md` §11. Execution is **not yet completed** (`README.md`); numeric results are `TBD`. |
| **F. Findings** | Expected to reduce cross-platform retrieval burden and improve relevance, usefulness, interpretability, and traceability while maintaining safety consistency via rules. | Explicitly framed as **expected outcomes** rather than measured results (`README.md`, PDF p. 1). |

> **Consistency Note on Visual Infographic**: Per source-priority rules, illustrative metrics appearing in [個人化天氣助理系統解析資訊圖表.png](file:///d:/碩士/物聯網/1111_DIC/個人化天氣助理系統解析資訊圖表.png) (such as template remnants like `500 筆台式料理標註`, sample sizes, percentage improvement ranges, or fixed cutoffs like `>70%` / `UVI >= 8`) are excluded because `DESIGN.md` (§7.1, §12) intentionally defers thresholds and `README.md` marks evaluation as incomplete.

---

### 2. Review of the Five Core Mindsets

Summarized from [個人化天氣助理設計.pdf](file:///d:/碩士/物聯網/1111_DIC/個人化天氣助理設計.pdf) (p. 2) and mapped to `DESIGN.md`:

1. **Action- and Impact-Centered Design**: Guided by WMO impact-based forecasting principles (`WMO-No. 1150` as cited in PDF), official metrics must be translated into concrete pre-trip actions (umbrellas, sun protection, masks, clothing) rather than raw numbers alone (`DESIGN.md` §1, §7.1).
2. **Same Environment $\neq$ Same Personal Need**: Informed by individual thermal comfort modeling (PCM-RL as cited in PDF), environmental snapshots, user preferences, and activity contexts must be modeled and stored separately (`DESIGN.md` §6: `EnvironmentalSnapshot`, `UserProfile`, `ItinerarySegment`).
3. **Uncertainty-Aware Decisions and Error Cost**: Forecasts carry uncertainty; decisions should reflect asymmetric error cost ($p \times rL > C$ as noted in PDF p. 2) and avoid overconfident claims when data are stale, coarse, or missing (`DESIGN.md` §2 Principle 4, §5.3).
4. **Strict Separation of Comfort Preferences and Health Protection**: Informed by low-concentration $\text{PM}_{2.5}$ health evidence and WHO UVI guidance (PDF p. 2), subjective comfort preferences may tune comfort reminders or ordering, but must never cancel essential health protection alerts (`DESIGN.md` §1.2, §2 Principle 3, §7.1).
5. **Traceability and Layered Verification**: Every recommendation must trace back to official snapshots, rule IDs, and preference versions, with explicit quality flagging (`missing`, `stale`, `spatially-coarse`, `unavailable`) and verification that LLM text preserves structured facts (`DESIGN.md` §2 Principles 5 & 7, §5.3, §7.3).

---

### 3. Review of the Three Trade-offs (Controversies)

Summarized from [個人化天氣助理設計.pdf](file:///d:/碩士/物聯網/1111_DIC/個人化天氣助理設計.pdf) (p. 3) and resolved per `DESIGN.md`:

| # | Trade-off Dimension | Tension | Adopted Architectural Resolution in `DESIGN.md` |
|---|---|---|---|
| 1 | **Fixed Rules $\leftrightarrow$ Autonomous LLM** | Static rules lack conversational flexibility; autonomous LLMs risk numerical hallucination and non-reproducible risk decisions. | **Hybrid Split (`DESIGN.md` §4, §7)**: Versioned deterministic rule engine (Module 7) owns numeric comparisons, health rules, and priority ordering; LLM (Modules 2 & 9) is strictly bounded to NL parsing (with confirmation) and evidence-bound explanation. |
| 2 | **Uniform Standards $\leftrightarrow$ Learned Personal Tolerance** | Purely uniform alerts ignore individual differences; unconstrained preference learning risks weakening health safeguards. | **Controlled Explicit Profiles (`DESIGN.md` §6, §9)**: Start with explicit `UserProfile` settings; post-trip `FeedbackRecord` entries only propose `PreferenceUpdateCandidate` items requiring explicit user confirmation; health rules strictly follow official criteria. |
| 3 | **Simple Actionable Advice $\leftrightarrow$ Preserving Uncertainty & Conditions** | Overly brief directives hide uncertainty; raw probabilities and caveats overwhelm users. | **Three-Layer Presentation (`README.md`, `DESIGN.md` §10)**: (1) Primary actionable recommendation $\rightarrow$ (2) concise causal reasoning $\rightarrow$ (3) underlying official values, source metadata, valid timeframes, and quality limitations. |

---

### 4. Review of the Ten Detailed Questions (Q1–Q10)

The supporting PDF ([個人化天氣助理設計.pdf](file:///d:/碩士/物聯網/1111_DIC/個人化天氣助理設計.pdf), p. 3) enumerates ten deep-research question topics (referencing external line numbers 68–77), while the full Q&A prose is not present in this repository. Below is the verified mapping of each topic to `DESIGN.md`, marking unrecorded full explanations as `TBD`:

| ID | Topic (from PDF p. 3) | Corresponding Mechanism in `DESIGN.md` | Status in Repo |
|---|---|---|---|
| **Q1** | Individual perception differences | Separate `UserProfile` (`rainTolerance`, `sunProtectionPriority`, `airQualityAlertSensitivity`, `thermalPreference`) and `ItinerarySegment` (`activityType`, `outdoorExposureLevel`) (§6). | Mechanism defined; extended Q&A narrative `TBD`. |
| **Q2** | Asymptomatic $\neq$ risk-free | Health protection vs. comfort separation (§2 Principle 3); post-trip subjective feedback cannot negate official health rules (§1.2, §9). | Policy defined; extended Q&A narrative `TBD`. |
| **Q3** | Umbrella threshold trade-off | Rain preparation rule balances forecast probability, timeframe, exposure, and `rainTolerance` (§7.1); exact numeric cutoff `TBD` (§12). | Rule inputs defined; numeric threshold & narrative `TBD`. |
| **Q4** | Data temporal asynchronicity failure case | Explicit `observedOrForecastTime`, `publishedAt`, `fetchedAt`, quality flags (`stale`, `unavailable`), and strict prohibition on substituting current observations for future forecasts (§1.2, §5.2–5.3, §10). | Handling defined; specific case-study log `TBD`. |
| **Q5** | Friendly wording $\neq$ effective decision support | Verifiability & traceability over fluency; 3-layer presentation and fidelity verification falling back to structured `SegmentDecision` if LLM text drifts (§7.3, §10, §11.3). | Guardrails defined; extended Q&A narrative `TBD`. |
| **Q6** | Fair evaluation across three baselines | Baseline A, Baseline B, and Proposed receive identical itineraries, environmental snapshots, preferences, and scoring criteria; multi-run testing for generative variance (§11.1). | Protocol defined; experiment execution `TBD`. |
| **Q7** | Cold-start design | Explicit manual `UserProfile` configuration and versioned defaults prior to accumulating `FeedbackRecord` / `PreferenceUpdateCandidate` (§6, §9). | Mechanism defined; default parameter values `TBD`. |
| **Q8** | Multi-stop itinerary logic | Per-segment normalization, per-stop fetching/quality check/decision (`SegmentDecision`), and cross-stop aggregation (`TripRecommendation`) preserving stop-level evidence while excluding inter-stop routing (§1.2, §4, §8). | End-to-end logic defined; extended Q&A narrative `TBD`. |
| **Q9** | Same AQI, different pollutant protection | Air quality rule explicitly evaluates `aqi`, `majorPollutant`, data type, and sensitivity preference rather than issuing identical advice for equal AQI values (§7.1, §11.2). | Requirement & test scenario defined; pollutant-specific rule table `TBD`. |
| **Q10** | Conflict between satisfaction and missed critical alerts | Evaluation separates subjective satisfaction from reliability/omission metrics; higher satisfaction cannot justify increased critical alert omission (§11.3). | Evaluation rule defined; extended Q&A narrative `TBD`. |

---

## Part II. Refactored Introduction (IEEE-Style Academic English)

### I. Introduction

Public access to meteorological forecasts and air quality monitoring has become foundational for daily commuting, outdoor recreation, exercise, and family activity planning. In Taiwan, the Central Weather Administration (CWA) and the Ministry of Environment (MOENV) maintain extensive environmental open-data services spanning township weather forecasts, precipitation, ultraviolet (UV) radiation, weather warnings, the Air Quality Index (AQI), air quality area forecasts, and pollutant-specific observations [1]–[4]. Despite the breadth of authoritative data, turning raw environmental feeds into actionable pre-trip preparation remains a manual, cognitively demanding process for end users.

In everyday decision-making, travelers rarely evaluate a single static metric in isolation. Preparing for an outing requires coordinated decisions on rain gear, sun protection, respiratory protection, and thermal layering across changing times of day and outdoor exposure levels. Moreover, individuals differ substantially in their thermal comfort tolerances, sensitivity to airborne pollutants, willingness to carry protective gear, and physical exertion levels during specific activities. A morning walk to class in one township and an afternoon outdoor event in another expose the same individual to distinct micro-environments, exposure durations, and protection trade-offs.

Currently, however, official environmental data are dispersed across separate portals and specialized feeds—such as forecast dashboards, rainfall monitors, UV indices, and real-time or forecast air quality pages [1]–[4]. Users must navigate multiple platforms, reconcile mismatched spatial resolutions (such as township forecasts, discrete air-quality monitoring stations, and broader air quality areas), and verify whether data are current or forecast-valid before deciding when to depart, what equipment to carry, or whether to reschedule indoors.

Existing general-purpose weather and air quality services do not resolve this gap because they predominantly report location-averaged values and standardized threshold warnings without modeling multi-stop itineraries, activity contexts, or individual comfort preferences. Conversely, applying Large Language Models (LLMs) directly to raw environmental data or open-ended meteorological reasoning introduces critical reliability risks, including numerical hallucination, unstable tool invocation, opaque or non-reproducible recommendations, and misinterpretation of weather impacts [5]–[8]. For safety-relevant pre-trip guidance, natural-language fluency cannot substitute for verifiable, data-grounded decision logic.

To address these limitations, this proposal specifies a **Personalized Weather Assistant**—a data-grounded, context-aware hybrid AI agent designed not to re-predict weather, but to transform official CWA and MOENV data into traceable, personalized pre-trip decisions. The core architectural principle is a strict separation of responsibilities across an 11-module pipeline: a versioned, deterministic rule decision engine governs numerical comparisons, spatiotemporal quality degradation, pollutant-specific air quality checks, and mandatory health protection priorities, while an LLM module is constrained to natural-language itinerary parsing (with user confirmation) and evidence-bound explanation and follow-up dialogue. Environmental snapshots, explicit user preferences, and post-trip feedback are stored separately so that every recommendation remains reproducible and subjective comfort adjustments never override official health safeguards.

The expected contributions of this work are structured into four areas (with implementation and empirical validation currently planned rather than completed):
1. **Multi-Source Spatiotemporal Integration and Quality Governance**: A unified intake and alignment pipeline combining CWA township forecasts with timeframe-appropriate MOENV real-time station measurements and `AQF_P_01` next-day air quality forecasts, propagating explicit quality flags (`missing`, `stale`, `spatially-coarse`, `unavailable`) without data fabrication.
2. **Versioned Explicit Personalization with Safety Separation**: A conceptual preference and memory model (`UserProfile`, `FeedbackRecord`, and user-confirmed `PreferenceUpdateCandidate`) that adapts comfort guidance and presentation priority while preventing subjective preferences from suppressing critical health alerts.
3. **Traceable Hybrid Pre-Trip Decision Mechanism**: An end-to-end 11-module workflow linking per-segment deterministic evaluation (`SegmentDecision`) and cross-stop aggregation (`TripRecommendation`) to three-layer, evidence-grounded LLM explanations with deterministic structured fallback.
4. **Controlled Evaluation Design**: A planned three-way comparative evaluation framework (raw official data, fixed rule-based templates, and the proposed hybrid agent) across multi-stop, extreme-weather, pollutant-differentiated, and degraded-data scenarios.

---

## Part III. Refactored Related Work (IEEE-Style Academic English)

### II. Related Work

Following a balanced dual-track framing—combining AI agent methodologies with environmental and behavioral domain knowledge—prior literature informs both the system's architectural boundaries and its decision logic. *(Note: Only references with verified URLs in [ABSTRACT.md](file:///d:/碩士/物聯網/1111_DIC/ABSTRACT.md) carry explicit citation links below; numbered literature items cited solely by short topic labels in [個人化天氣助理設計.pdf](file:///d:/碩士/物聯網/1111_DIC/個人化天氣助理設計.pdf) are summarized strictly by their documented scope and marked `TBD` for full bibliographic metadata.)*

#### A. AI Agents, Personalization Memory, and LLM Reliability in Environmental Tasks

Recent AI agent research has explored applying LLMs to meteorological information synthesis, conversational environmental interfaces, and personalized recommendation workflows [5]–[10]. Within weather and air quality applications, conversational systems such as Weathernews Weather Agent (2025) (`[full citation: TBD]`), Model Context Protocol (MCP)-based air quality dialogue interfaces (`[full citation: TBD]`), and Instructor–Worker multi-agent architectures (`[full citation: TBD]`) demonstrate the utility of natural-language access to environmental APIs. However, these systems focus primarily on single-turn or single-context querying and lack persistent, user-controlled environmental tolerance profiles capable of reconciling multi-stop itineraries and heterogeneous data quality.

In parallel, surveys on LLM-based recommender agents structured around Profile, Memory, Planning, and Action components (`[full citation: TBD]`), as well as memory-assisted LLM retrieval designs (`[full citation: TBD]`), highlight how explicit user profiles and structured memory can improve personalization. At the same time, agent engineering guidelines emphasize the importance of well-scoped tool contracts to avoid unstable invocation [7], while meteorological evaluation benchmarks such as WXImpactBench (`[full citation: TBD]`) indicate that current LLMs still require rigorous verification when interpreting severe-weather impacts. Consistent with findings in recent LLM weather and decision-support preprints [5], [6], [8]–[10], delegating numeric thresholds or safety-critical judgments directly to unconstrained text generation risks hallucination and irreproducibility. Accordingly, our design restricts the LLM strictly to structured parsing and evidence-grounded explanation, delegating all threshold and priority logic to a deterministic rule engine.

#### B. Environmental Domain Knowledge, Risk Cognition, and Impact-Based Forecasting

Domain literature underscores that physical environmental metrics do not map uniformly to human risk or protective behavior. In thermal comfort research, personalized models such as PCM-RL (`[full citation: TBD]`) demonstrate substantial inter-individual variation in outdoor thermal sensation, motivating the separation of environmental measurements from personal preference profiles and activity contexts. Regional studies of the Universal Thermal Climate Index (UTCI) in Taipei and Taichung (`[full citation: TBD]`) further highlight urban heat island effects and localized spatial variability across Taiwanese cities, reinforcing the need for township-level alignment rather than coarse regional averages.

For atmospheric and radiative hazards, epidemiological evidence—including low-concentration $\text{PM}_{2.5}$ exposure research in *BMJ* (`[full citation: TBD]`)—demonstrates that health risks persist even at comparatively low pollutant concentrations, supporting the design principle that subjective comfort or asymptomatic perception must not cancel official health protections. Moreover, public health and risk-communication studies, including UV Index (UVI) meta-analyses (`[full citation: TBD]`) and psychological studies of Probability of Precipitation (PoP) interpretation (`[full citation: TBD]`), show that lay users frequently misinterpret probabilistic rainfall values and overlook UV hazards when presented solely with raw indices. To bridge this cognition gap, the World Meteorological Organization (WMO) Guidelines on Multi-hazard Impact-based Forecast and Warning Services (`WMO-No. 1150`, `[full bibliographic edition/URL: TBD]`) advocate shifting from weather-only reporting ("what the weather will be") toward impact-oriented, actionable guidance ("what the weather will do" and how users should prepare).

#### C. Synthesis and Research Gap

Taken together, existing literature reveals a clear progression and gap:
- **Existing Research**: AI agent architectures establish conversational access and memory-assisted personalization, while domain studies in thermal comfort, UTCI, low-level $\text{PM}_{2.5}$, UVI/PoP cognition, and WMO impact-based forecasting establish why environmental guidance must be localized, pollutant-specific, and action-oriented.
- **Limitations**: Indoor or single-model comfort formulations cannot be extrapolated directly to multi-stop outdoor itineraries, conversational weather agents lack explicit tolerance versioning and data-quality governance, and unconstrained LLMs remain unreliable for numerical risk decisions.
- **Research Gap & Positioning**: There remains a need for a verified, data-grounded hybrid agent tailored to Taiwan's CWA and MOENV ecosystems that integrates multi-stop spatiotemporal alignment, explicit quality-flag degradation, deterministic rule traceability, and user-confirmed preference management—without allowing natural-language generation to alter safety decisions.

---

### Reference Grounding Ledger

**Verified URLs present in repository ([ABSTRACT.md](file:///d:/碩士/物聯網/1111_DIC/ABSTRACT.md)):**
- `[1]` MOENV Environmental Data / Issue Reference: https://env.moenv.gov.tw/en/Issue/Index/87c2b327-f773-4d4a-a42c-a7ab390d8d0d
- `[2]` CWA Open Data Overview: https://opendata.cwa.gov.tw/about/opendata
- `[3]` CWA Open Data Developer Manual / Dataset List: https://opendata.cwa.gov.tw/devManual/datalist
- `[4]` Taiwan Government Open Data Dataset 40448: https://data.gov.tw/en/datasets/40448
- `[5]` Meteorological Journal (`qxqk.nmc.cn`), 2025, Vol. 51, Issue 8: http://qxqk.nmc.cn/qxen/article/issue/2025_51_8
- `[6]` arXiv:2510.03349: https://arxiv.org/html/2510.03349
- `[7]` Anthropic Research, Tool Writing for Agents: https://www.anthropic.com/research/writing-tools-for-agents
- `[8]` arXiv:2512.11819: https://web3.arxiv.org/abs/2512.11819
- `[9]` arXiv:2501.13608: https://www.arxiv.org/pdf/2501.13608.pdf
- `[10]` arXiv:2510.04017v2: https://arxiv.org/abs/2510.04017v2

**Literature themes referenced in [個人化天氣助理設計.pdf](file:///d:/碩士/物聯網/1111_DIC/個人化天氣助理設計.pdf) pending full bibliographic details (`TBD`):**
- Weathernews Weather Agent (2025); MCP air quality dialogue interface; Instructor–Worker multi-agent system; LLM recommender agent survey (Profile/Memory/Planning/Action); Memory-Assisted LLM; WXImpactBench; PCM-RL thermal comfort model; Taipei/Taichung UTCI trend study; *BMJ* low-concentration $\text{PM}_{2.5}$ study; UVI meta-analysis; PoP psychology study; WMO Impact-Based Forecasting Guidelines (`WMO-No. 1150`). *(Exact authors, full titles, publication years, DOIs, and mapping to `[5]`–`[10]` are marked `TBD` until the original bibliography table is supplied.)*
