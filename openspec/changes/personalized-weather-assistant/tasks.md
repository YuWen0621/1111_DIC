# Tasks

## 1. Resolve Deferred Architecture & Threshold Decisions (TBD)

- [ ] 1.1 Select frontend, backend, database, and deployment stack (per `design.md` Open Question 1) and verify the architecture choice is documented in `openspec/changes/personalized-weather-assistant/design.md` or implementation docs.
- [ ] 1.2 Document concrete CWA forecast endpoints, MOENV real-time station endpoints, `AQF_P_01` endpoint, API key handling, rate limits, and retry policies, and verify endpoint connectivity tests pass against official schemas.
- [ ] 1.3 Define Taiwan location normalization rules, nearest-station distance calculation, acceptable spatial radius for `spatially-coarse`, and per-source freshness limits for `stale`, and verify unit tests cover boundary distances and timestamps.
- [ ] 1.4 Finalize concrete domain rule thresholds mapped to official CWA/MOENV/WHO health guidelines for rain, UV, pollutant-specific air quality, and thermal comfort, and verify rule traceability metadata (`ruleId`) against test fixtures.
- [ ] 1.5 Specify user authentication, data retention policy, LLM provider/model, timeouts, and fidelity-verification rules, and verify security and fallback configurations pass review.

## 2. Itinerary Parsing & Location Normalization (Modules 1–3)

- [ ] 2.1 Implement multi-stop itinerary form intake and field validation (`placeName`, `startTime`, `endTime`, `activityType`, `outdoorExposureLevel`), and verify invalid/missing fields halt segment processing and prompt correction.
- [ ] 2.2 Implement LLM-assisted natural-language itinerary parsing with explicit user confirmation on ambiguous fields, and verify unconfirmed parses are never promoted to finalized segments.
- [ ] 2.3 Implement Taiwan place validation and normalization to `ItinerarySegment` (`latitude`, `longitude`, `township`, `airQualityArea`, normalized timeframe) while excluding inter-stop route analysis, and verify Taiwan/non-Taiwan test cases pass.

## 3. Environmental Retrieval, Alignment & Quality Control (Modules 5–6)

- [ ] 3.1 Implement CWA township weather forecast fetcher retaining township, forecast period, and `publishedAt` metadata, and verify payload parsing tests pass.
- [ ] 3.2 Implement MOENV timeframe-driven air quality fetcher routing current/ongoing segments to nearest station observations, covered next-day segments to `AQF_P_01` area forecasts, and out-of-horizon segments to `unavailable`, and verify source-switching tests pass.
- [ ] 3.3 Implement spatiotemporal alignment and quality-flag evaluator (`missing`, `stale`, `spatially-coarse`, `unavailable`) outputting `EnvironmentalSnapshot`, and verify quality flags propagate cleanly without extrapolation or substitution.

## 4. Deterministic Rule Engine & Multi-Stop Aggregation (Modules 7–8)

- [ ] 4.1 Implement versioned rule engine producing `SegmentDecision` with `ruleId`, `evidence`, and `limitations` across rain, UV, pollutant-specific air quality (differentiating same AQI with different `majorPollutant`), thermal comfort, and quality degradation, and verify deterministic reproducibility tests pass.
- [ ] 4.2 Enforce health-over-comfort priority ordering so `UserProfile` preferences cannot suppress mandatory health alerts, and verify preference-override boundary tests pass.
- [ ] 4.3 Implement cross-stop `TripRecommendation` aggregator that merges duplicate alerts while preserving per-stop location, timeframe, and evidence, and verify multi-stop itinerary aggregation tests pass.

## 5. Preference Profiles, Feedback & Update Candidates (Modules 4 & 11)

- [ ] 5.1 Implement separated storage for explicit versioned `UserProfile`, immutable itinerary/decision snapshots, `FeedbackRecord`, and `PreferenceUpdateCandidate`, and verify CRUD and historical decision reproducibility tests pass.
- [ ] 5.2 Implement post-trip feedback collection and pending `PreferenceUpdateCandidate` generation requiring explicit user acceptance before creating a new `confirmedPreferenceVersion`, and verify feedback never disables official health rules.

## 6. Grounded LLM Explanation, 3-Layer Presentation & Fallback (Modules 9–10)

- [ ] 6.1 Implement LLM summary and "why?" Q&A generator strictly grounded in `TripRecommendation`, `SegmentDecision.evidence`, and `limitations`, and verify prompt/output tests reject fabricated data or real-time/forecast conflation.
- [ ] 6.2 Implement fidelity verification and deterministic fallback plus three-layer UI presentation (primary action → concise reason → official sources/timestamps/limitations), and verify timeout/mismatch fallback tests pass.

## 7. Planned Comparative Evaluation (Verification Only — No Fabricated Results)

- [ ] 7.1 Implement Baseline A (raw official data) and Baseline B (fixed rules + templates) runners sharing identical `ItinerarySegment`, `EnvironmentalSnapshot`, and `UserProfile` inputs with the Proposed system, and verify input parity across configurations.
- [ ] 7.2 Execute the six-scenario evaluation suite (multi-stop cross-township, high rain with rain-gear reluctance, high UV prolonged exposure, equal AQI different pollutant, real-time vs. `AQF_P_01` switching, and missing/stale/coarse/unavailable data) across multiple runs for LLM variance, and verify all five metric categories (correctness, completeness, personalization, interpretability, system reliability) report without prioritizing satisfaction over missed critical alerts.
