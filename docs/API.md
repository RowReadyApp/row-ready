# Row Ready — API Documentation

## 1. Overview

Row Ready uses external data sources to provide environmental and rowing-related information.

Current planned data sources:

- Open-Meteo
- Environment Agency API

These APIs provide the underlying data used by the Today page, condition information and eventually the recommendation engine.

---

## 2. Open-Meteo

Open-Meteo provides weather forecast and historical weather data.

### Planned use

Row Ready will use Open-Meteo to obtain weather conditions for rowing locations.

Potential data includes:

- Wind speed
- Wind direction
- Wind gusts
- Precipitation / rain
- Temperature
- Weather forecast
- Historical weather data

### Location

Weather data will be requested using the coordinates associated with a Row Ready location.

The location should provide:

- Latitude
- Longitude

### Recommendation use

Weather data will contribute to:

- Current condition status
- Best rowing window
- Overall rowing recommendation
- Confidence level
- Recommendation explanation

For example:

```text
Wind: 12 km/h — Good
Gusts: 18 km/h — Good
Rain: Low — Good
```

The exact thresholds for these classifications will be defined as part of the recommendation engine.

### Documentation

Official Open-Meteo documentation should be used when implementing the integration.

---

## 3. Environment Agency API

The Environment Agency API provides information about water levels and river conditions in England.

### Planned use

Row Ready will use Environment Agency data to provide relevant river-level information for supported rowing locations.

Potential data includes:

- Water level
- Water level trend
- Measurement time
- Monitoring station
- Station/location information

### Location matching

A Row Ready location may be associated with an Environment Agency monitoring station.

The relationship may eventually use:

```text
locationId
environmentAgencyStationId
```

This allows the app to retrieve the appropriate water-level data for each rowing location.

### Recommendation use

Water-level data may contribute to:

- Current water condition status
- Overall rowing recommendation
- Confidence level
- Recommendation explanation
- Future condition history

For example:

```text
Water level: Normal — Good
```

The exact thresholds will depend on the characteristics of the individual location.

---

## 4. Data Processing

External API data should not necessarily be displayed directly to users.

The general flow should be:

```text
External API
    ↓
Retrieve data
    ↓
Associate with Row Ready location
    ↓
Process / normalise data
    ↓
Determine condition status
    ↓
Recommendation engine
    ↓
Display to user
```

For example:

```text
Open-Meteo
    ↓
Wind = 12 km/h
    ↓
Row Ready classification
    ↓
Wind = Good
```

---

## 5. API Data & Firestore

External API data may be temporarily retrieved for current conditions and may eventually be stored in Firestore when historical data is required.

The database design is documented in `DATABASE.md`.

The application should avoid storing duplicate or unnecessary API data.

---

## 6. API Error Handling

The app should handle situations where external data is:

- Temporarily unavailable
- Delayed
- Missing
- Invalid
- Outside the expected range

The user should not receive a misleading recommendation when critical data is unavailable.

The UI should clearly indicate when information cannot be retrieved or is outdated.

---

## 7. Data Freshness

The Today page should show when conditions were last updated.

Example:

```text
Updated 2 min ago
```

The acceptable age of data will depend on the type of information.

Weather and river-level data may have different update frequencies.

---

## 8. API Security

API credentials, if required, should not be exposed directly in the client application.

Secrets should be stored using appropriate Firebase/backend configuration where necessary.

Public APIs that do not require credentials may be called directly where appropriate, subject to their terms and usage limits.

---

## 9. Future API Sources

Additional data sources may be added in the future for:

- Weather warnings
- Navigation information
- Club information
- River restrictions
- Hazards
- Community reports
- Other waterways outside the initial Environment Agency coverage

New APIs should be documented here before or when they are integrated.

---

## 10. Implementation Status

### Open-Meteo

Planned — not yet integrated into the functional app.

### Environment Agency API

Planned — not yet integrated into the functional app.

The API implementation should be updated in this document as endpoints, parameters and response structures are established.
