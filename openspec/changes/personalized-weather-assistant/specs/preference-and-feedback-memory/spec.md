# Spec Delta

## Purpose

Defines explicit versioned user preference profiles, separated storage for trip snapshots and post-trip feedback, and user-confirmed preference update proposals.

## ADDED Requirements

### Requirement: Explicit versioned preference management
The system SHALL allow users to inspect, modify, and delete explicit preferences (rain tolerance, sun protection priority, air quality alert sensitivity, thermal preference) with an identifiable confirmed preference version.

#### Scenario: User updates explicit preferences
- **WHEN** a user edits or deletes an explicit preference setting
- **THEN** the system records a new confirmed preference version and associates subsequent decisions with that version identifier

### Requirement: Separated storage of profiles, trip records, and feedback
The system SHALL maintain explicit preference profiles, itinerary decision snapshots, post-trip feedback records, and preference update candidates as separate managed records to ensure reproducibility.

#### Scenario: Reproducing a past trip recommendation
- **WHEN** an audit or evaluation inspects a historical itinerary recommendation
- **THEN** the system links the itinerary segments, environmental snapshots, rule identifiers, and the confirmed preference version active at decision time

### Requirement: User-confirmed preference update candidates
The system SHALL generate pending preference update candidates from post-trip feedback patterns and SHALL NOT apply preference changes without explicit user confirmation.

#### Scenario: Post-trip feedback suggests preference adjustment
- **WHEN** accumulated post-trip feedback indicates a candidate adjustment to a user's comfort preference
- **THEN** the system creates a pending preference update candidate with reason and evidence summary and waits for user acceptance or rejection before creating a new preference version

### Requirement: Protection against feedback overriding health rules
The system SHALL NOT allow subjective post-trip feedback or accepted comfort preference updates to negate official health risk conditions or override mandatory protective alerts.

#### Scenario: User reports no perceived discomfort under high hazard
- **WHEN** a user submits post-trip feedback reporting no perceived impact despite an official health risk condition
- **THEN** the system records the feedback without disabling official health protection rules for future itineraries
