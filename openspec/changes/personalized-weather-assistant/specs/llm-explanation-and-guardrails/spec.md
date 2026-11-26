# Spec Delta

## Purpose

Defines strict grounding guardrails for LLM-generated summaries and follow-up explanations, three-layer recommendation presentation, and deterministic fallback behavior on generation failure.

## ADDED Requirements

### Requirement: Grounded LLM summarization and follow-up explanation
The system SHALL generate natural-language trip summaries and answers to user "why?" follow-up questions solely from structured trip and segment decision records, citing recorded evidence and limitations.

#### Scenario: User asks why a recommendation was issued
- **WHEN** a user asks why a specific alert or action appears for a segment
- **THEN** the LLM explanation references the segment's recorded source fields, rule identifier, profile factors, and documented limitations

### Requirement: Prohibition on data fabrication or rule override
The LLM module SHALL NOT invent or interpolate unrecorded environmental values, alter rule engine priorities or health conclusions, extrapolate missing periods, or present real-time observations as future forecasts.

#### Scenario: Data limitation during explanation generation
- **WHEN** a segment contains `missing`, `stale`, `spatially-coarse`, or `unavailable` flags
- **THEN** the generated explanation explicitly states the recorded limitation and does not fill gaps with estimated weather or air quality values

### Requirement: Three-layer result presentation
The system SHALL present outputs in three explicit layers: primary actionable recommendations, concise evidence-backed reasons, and underlying official data sources, timestamps, and limitations.

#### Scenario: Rendering a multi-stop recommendation
- **WHEN** a trip recommendation is returned to the user interface
- **THEN** the interface displays primary actions first, followed by concise per-stop reasons, and exposes official sources, publication times, valid periods, and limitations

### Requirement: Fidelity verification and deterministic fallback
The system SHALL verify that LLM-generated text preserves recorded numbers, critical alerts, valid timeframes, and limitations, and SHALL fall back to structured decisions and evidence if generation or verification fails.

#### Scenario: LLM explanation fails or contradicts structured decisions
- **WHEN** LLM output generation times out, fails, or modifies recorded values, alert priorities, or limitations
- **THEN** the system discards the inconsistent text and displays the structured recommendations, source evidence, and limitations directly
