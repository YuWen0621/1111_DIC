# Proposal

## Why

Taiwan's Central Weather Administration (CWA) and Ministry of Environment (MOENV) publish rich weather and air quality open data, yet travelers and commuters must manually query multiple platforms and interpret raw metrics across multi-stop itineraries without personalized context or traceable reasoning. Refactoring the existing system specification in `DESIGN.md` into OpenSpec establishes verifiable behavioral contracts and a strict decision boundary between deterministic domain rules and Large Language Model (LLM) natural-language interaction before application implementation begins.

## What Changes

- Formalize the **Personalized Weather Assistant** target system from `DESIGN.md` into spec-driven OpenSpec artifacts while preserving all 11 system modules, 7 design principles, and 6 conceptual data entities.
- Introduce capability specifications for multi-stop itinerary parsing and Taiwan location/timeframe normalization (`ItinerarySegment`).
- Introduce capability specifications for CWA weather forecast and MOENV real-time / `AQF_P_01` next-day air quality retrieval, spatiotemporal alignment, and data-quality flagging (`missing`, `stale`, `spatially-coarse`, `unavailable`).
- Introduce capability specifications for the deterministic rule decision engine (precipitation, UV, pollutant-specific air quality, thermal comfort, and data-quality degradation) and multi-stop trip aggregation (`SegmentDecision`, `TripRecommendation`).
- Introduce capability specifications for strict LLM grounding guardrails, three-layer result presentation, and graceful fallback when explanation generation fails.
- Introduce capability specifications for explicit versioned preference profiles (`UserProfile`), separated feedback logging (`FeedbackRecord`), and user-confirmed preference update candidates (`PreferenceUpdateCandidate`).
- Introduce capability specifications for the planned comparative evaluation protocol (Baseline A, Baseline B, and Proposed across six test scenarios and five metric categories).
- Document deferred implementation choices (technology stack, API endpoints, concrete rule thresholds, freshness/distance thresholds, retention policies, and LLM provider configuration) as `TBD` open questions.

## Capabilities

### New Capabilities

- `itinerary-parsing-and-normalization`: Multi-stop itinerary entry, natural-language supplementary parsing with ambiguity confirmation, and normalization into Taiwan administrative townships, air quality areas, and time intervals.
- `environmental-data-and-quality`: Official CWA weather and MOENV air quality retrieval (real-time nearest station vs. `AQF_P_01` next-day area forecast), metadata preservation, spatiotemporal alignment, and quality-flagging (`missing`, `stale`, `spatially-coarse`, `unavailable`).
- `rule-decision-and-aggregation`: Deterministic, versioned rule evaluation for rain, UV, pollutant-aware air quality, thermal comfort, and data-quality degradation, plus cross-stop deduplication and priority sorting that keeps health protection above comfort preferences.
- `llm-explanation-and-guardrails`: Grounded natural-language summarization and follow-up explanation strictly constrained to structured decision evidence and limitations, plus three-layer presentation and structured fallback on LLM failure.
- `preference-and-feedback-memory`: Versioned explicit user preference management, separate storage of trip snapshots and post-trip feedback, and user-confirmed preference update candidates that never override official health rules.
- `evaluation-protocol`: Planned three-way comparative evaluation (Baseline A, Baseline B, Proposed) across multi-stop, weather, pollutant-specific, and degraded-data scenarios, enforcing reliability over subjective satisfaction.

### Modified Capabilities

<!-- None: no existing specs exist under openspec/specs/. -->

## Impact

- **OpenSpec Artifacts**: Populates `openspec/changes/personalized-weather-assistant/` with `proposal.md`, six `specs/<capability>/spec.md` files, `design.md`, and `tasks.md`.
- **External Data Dependencies (Design Scope)**: Taiwan CWA meteorological open data and MOENV real-time AQI/pollutant monitoring plus `AQF_P_01` air quality forecast APIs (concrete endpoints, keys, and retry policies are `TBD`).
- **Scope Boundaries**: Preserves all original repository documents unmodified; application code implementation and experimental execution are out of scope for this proposal change (`TBD` implementation phase).
