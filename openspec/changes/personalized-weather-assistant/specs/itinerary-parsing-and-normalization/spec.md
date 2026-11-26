# Spec Delta

## Purpose

Defines multi-stop itinerary intake, natural-language supplementary parsing with user confirmation on ambiguity, and Taiwan location and timeframe normalization for downstream environmental data retrieval.

## ADDED Requirements

### Requirement: Multi-stop itinerary entry and required fields
The system SHALL accept multi-stop itineraries where each segment specifies a Taiwan place, start and end times, activity type, and outdoor exposure level.

#### Scenario: Valid multi-stop itinerary submission
- **WHEN** a user submits a multi-stop itinerary containing valid locations, timeframes, activity types, and outdoor exposure levels for every segment
- **THEN** the system accepts all segments and forwards them for location and timeframe normalization

#### Scenario: Missing or invalid segment field
- **WHEN** a segment has a missing or invalid location, timeframe, or activity field
- **THEN** the system pauses processing for the invalid segment and prompts the user to supply or correct the field without guessing missing values

### Requirement: Natural-language parsing with ambiguity confirmation
The system SHALL parse natural-language itinerary supplements into structured draft segment fields and require explicit user confirmation whenever parsed fields are ambiguous.

#### Scenario: Ambiguous natural-language itinerary input
- **WHEN** a user provides natural-language text whose location, time interval, or activity type admits multiple interpretations
- **THEN** the system presents the parsed draft fields to the user for confirmation and does not treat unconfirmed fields as finalized itinerary segments

### Requirement: Taiwan location and timeframe normalization
The system SHALL validate locations within Taiwan and resolve each segment to standardized geographic coordinates, township/district, air quality area, and normalized start/end timestamps.

#### Scenario: Successful normalization of a Taiwan segment
- **WHEN** a segment specifies a valid Taiwan place such as a township or named locality and a valid time window
- **THEN** the system resolves coordinates, township, air quality area, and normalized start and end times for that segment

#### Scenario: Out-of-scope location outside Taiwan
- **WHEN** a segment specifies a location outside Taiwan or an unresolvable place name
- **THEN** the system rejects the unresolvable segment and requests user correction before environmental querying

### Requirement: Exclusion of inter-stop transit route analysis
The system SHALL evaluate only user-specified stopover segments and SHALL NOT infer or analyze travel routes between stops.

#### Scenario: Multi-stop itinerary between distinct townships
- **WHEN** a user enters two sequential stops in different townships
- **THEN** the system retrieves environmental data and generates decisions only for each specified stopover segment and its timeframe
