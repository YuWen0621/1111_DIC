# Spec Delta

## Purpose

Defines official CWA meteorological and MOENV air quality data retrieval, source selection by itinerary timeframe, metadata preservation, spatiotemporal alignment, and data-quality flagging.

## ADDED Requirements

### Requirement: Official meteorological forecast retrieval
The system SHALL retrieve weather forecasts (temperature, relative humidity, precipitation probability, UV index, and weather description) from official CWA open data aligned to each segment's township and forecast period.

#### Scenario: Future segment within CWA forecast coverage
- **WHEN** a normalized segment falls within available CWA township forecast coverage
- **THEN** the system retrieves the matching township forecast and retains township name, forecast start/end time, and publication timestamp

### Requirement: Timeframe-based air quality source selection
The system SHALL select nearest-station MOENV real-time observations for current or ongoing segments, official `AQF_P_01` air quality area forecasts for covered next-day segments, and an unavailable status beyond forecast horizon.

#### Scenario: Ongoing or current segment
- **WHEN** a segment occurs in the current or ongoing period
- **THEN** the system retrieves real-time AQI, major pollutant, and pollutant measurements from the nearest available MOENV station and records station name, distance, publication time, and major pollutant

#### Scenario: Future segment within next-day AQF_P_01 horizon
- **WHEN** a future segment falls within official next-day `AQF_P_01` forecast coverage
- **THEN** the system retrieves the air quality forecast for the segment's air quality area and retains area name, forecast date, publication time, and major pollutant

#### Scenario: Segment beyond official air quality forecast horizon
- **WHEN** a segment falls beyond official MOENV air quality forecast availability
- **THEN** the system marks air quality as unavailable with "no official forecast available", suppresses air quality risk judgment, and still permits weather recommendations

### Requirement: Distinction between observations and area forecasts
The system SHALL distinguish real-time station observations from area-level daily air quality forecasts and SHALL NOT extrapolate missing data or substitute current observations for future forecasts.

#### Scenario: Displaying AQF_P_01 area forecast metadata
- **WHEN** air quality data comes from `AQF_P_01`
- **THEN** the system labels it as an air-quality-area and date-level forecast rather than a site-specific hourly observation

### Requirement: Spatiotemporal alignment and quality flag propagation
The system SHALL check publication time, fetch time, valid period, and spatial reference for every snapshot and assign `missing`, `stale`, `spatially-coarse`, or `unavailable` flags when quality criteria are unmet.

#### Scenario: Stale or misaligned environmental data
- **WHEN** retrieved weather or air quality data exceeds acceptable freshness or fails to align with the segment's location or valid timeframe
- **THEN** the system attaches the corresponding quality flags (`stale`, `missing`, `spatially-coarse`, or `unavailable`) and propagates them to the decision and presentation layers
