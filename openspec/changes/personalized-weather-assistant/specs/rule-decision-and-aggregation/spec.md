# Spec Delta

## Purpose

Defines the deterministic rule-based decision engine for precipitation, UV, pollutant-specific air quality, thermal comfort, and data-quality degradation, as well as cross-stop risk aggregation and priority ordering.

## ADDED Requirements

### Requirement: Versioned deterministic rule evaluation
The system SHALL evaluate precipitation preparation, UV protection, air quality protection, thermal comfort, and data-quality degradation using versioned rules, attaching a rule identifier and source evidence to every decision.

#### Scenario: Complete environmental snapshot evaluation
- **WHEN** a segment has aligned weather and air quality snapshots alongside user preferences
- **THEN** the rule engine produces structured alerts with category, priority, recommended action, rule identifier, input source fields, and profile factors

### Requirement: Pollutant-specific air quality protection
The system SHALL evaluate air quality risk using both AQI and the major pollutant along with data type and sensitivity preferences, rather than issuing identical advice for equal AQI values with different pollutants.

#### Scenario: Identical AQI with different major pollutants
- **WHEN** two segments have the same AQI value but different major pollutants (such as PM2.5 versus O3)
- **THEN** the rule engine records the distinct major pollutant evidence and adapts protective guidance according to pollutant-specific official rules

### Requirement: Separation of health protection and comfort preferences
The system SHALL prioritize critical health and safety alerts above comfort reminders and SHALL NOT allow user preferences to suppress or delete essential health protection alerts.

#### Scenario: User with low alert sensitivity encounters health risk
- **WHEN** environmental data triggers an official health protection rule while the user's preference profile sets low sensitivity or dislikes protective gear
- **THEN** the system preserves the mandatory health protection alert and ranks it above comfort reminders while reflecting preference factors only in comfort or presentation ordering

### Requirement: Degradation under poor data quality
The system SHALL downgrade or halt affected rule evaluations whenever `missing`, `stale`, `spatially-coarse`, or `unavailable` quality flags are present and SHALL record explicit limitations.

#### Scenario: Official API failure or partial missing fields
- **WHEN** one environmental data dimension fails or carries an `unavailable` or `missing` flag while other dimensions remain valid
- **THEN** the system withholds conclusions for the affected dimension, documents the limitation, and continues evaluating unaffected dimensions

### Requirement: Cross-stop aggregation with per-segment evidence retention
The system SHALL aggregate decisions across all segments in an itinerary, merge duplicate alerts, prioritize critical protections first, and preserve each alert's stop location, timeframe, and rule evidence.

#### Scenario: Multi-stop trip with overlapping rain alerts
- **WHEN** multiple segments in an itinerary trigger precipitation preparation alerts
- **THEN** the aggregator merges duplicate trip-level reminders while preserving the applicable stops, timeframes, and supporting evidence for traceability
