# Spec Delta

## Purpose

Defines the planned comparative evaluation protocol across Baseline A, Baseline B, and Proposed configurations, required test scenarios, evaluation metrics, and safety-first evaluation constraints.

## ADDED Requirements

### Requirement: Three-way controlled baseline comparison
The evaluation protocol SHALL compare Baseline A (official raw data only), Baseline B (fixed rules with fixed templates), and Proposed (rule engine, personal preferences, and grounded LLM explanation) under identical environmental data, itineraries, preferences, and scoring criteria.

#### Scenario: Fair comparative trial execution
- **WHEN** an evaluation trial is executed for a test itinerary
- **THEN** all three configurations receive identical normalized segments, environmental snapshots, and preference inputs, and generative outputs are tested across multiple runs to measure variance

### Requirement: Mandatory scenario coverage
The evaluation protocol SHALL include test cases covering multi-stop cross-township itineraries, high precipitation with rain-gear reluctance, prolonged outdoor high-UV exposure, equal AQI with differing major pollutants, real-time vs. next-day air quality switching, and degraded data quality (`missing`, `stale`, `spatially-coarse`, `unavailable`).

#### Scenario: Evaluating degraded and edge-case scenarios
- **WHEN** the test suite evaluates the Proposed system against Baselines A and B
- **THEN** every scenario in the six-category matrix is executed and scored without omitting missing-data or outdated-data cases

### Requirement: Multi-dimensional metrics with safety precedence
The evaluation protocol SHALL assess correctness (reproducibility), completeness (critical alert omission rate and limitation disclosure rate), personalization, interpretability, and system reliability, and SHALL NOT rank a configuration higher solely for subjective satisfaction if critical alert omissions increase.

#### Scenario: Conflict between satisfaction and safety completeness
- **WHEN** a configuration increases user satisfaction ratings while simultaneously increasing the omission rate of critical health protection alerts
- **THEN** the evaluation analysis reports both dimensions separately and rejects declaring that configuration superior on satisfaction alone
