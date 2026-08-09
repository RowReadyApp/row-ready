# Row Ready — Database

## 1. Database Technology

Row Ready uses:

- Firebase
- Cloud Firestore

Firestore is the primary database for application data.

---

## 2. Current Database Status

The Firestore database is connected to the FlutterFlow project.

The following collections have currently been created:

- `clubs`
- `locations`
- `boats`
- `users`

Test data has been added to verify the relationships between these collections.

The following collections/features are planned but have not yet been implemented:

- `conditions`
- `recommendations`
- `routes`
- `hazards`
- User favourites
- Personal boat notes/reviews

---

## 3. Database Principles

The database should:

- Keep club-owned information separate from user-owned information
- Avoid unnecessary duplication
- Use Document References for relationships between entities
- Allow clubs to manage their own locations and boats
- Allow users to maintain personal preferences, favourites and notes
- Support different types of rowing locations, including rivers, lakes, canals and reservoirs
- Support current and future historical environmental data
- Remain flexible enough to support the recommendation engine

---

## 4. Clubs

### Collection

`clubs`

Each document represents a rowing club.

### Current fields

```text
name
description
logoUrl
createdAt
updatedAt
```

### Example

```text
name: Bedford Rowing Club
```

A club can be associated with multiple locations and boats.

Club membership and administrator permissions will be defined later.

---

## 5. Locations

### Collection

`locations`

Each document represents a rowing location.

### Current fields

```text
name
waterType
clubId
latitude
longitude
status
createdAt
updatedAt
```

### Field details

`name`  
The display name of the location.

Example:

```text
River Great Ouse, Bedford
```

`waterType`  
The type of waterway.

Potential values:

```text
river
lake
canal
reservoir
other
```

`clubId`  
A Document Reference to the associated document in the `clubs` collection.

`latitude` / `longitude`  
Coordinates used for future weather and environmental API requests.

`status`  
Whether the location is currently active.

Potential values:

```text
active
inactive
```

### Example relationship

```text
clubs/{clubId}
        ↑
        │ Document Reference
        │
locations/{locationId}
```

A club may have multiple locations.

---

## 6. Boats

### Collection

`boats`

Each document represents a boat owned or managed by a club.

### Current fields

```text
name
boatClass
clubId
locationId
availabilityStatus
weightCategory
rowingType
coxed
bladeType
createdAt
updatedAt
```

### Field details

`name`  
The current boat name/identifier.

Example:

```text
Single 1
```

`boatClass`  
The rowing boat classification.

Examples:

```text
1x
4x
4-
```

`clubId`  
A Document Reference to the associated club.

`locationId`  
A Document Reference to the boat's associated location.

`availabilityStatus`  
Whether the boat is currently available.

Potential values:

```text
available
unavailable
```

`weightCategory`  
The intended rower weight category where applicable. The exact categories will be defined later.

`rowingType`  
The type of rowing.

Potential values:

```text
sculling
sweep
```

`coxed`  
Boolean indicating whether the boat is coxed.

`bladeType`  
Information about the blades/setup used with the boat.

This field is currently flexible and may become more structured in the future.

### Future boat improvements

The Add Boat interface should eventually allow the user to select a standard boat class from a dropdown.

The application should then automatically determine the corresponding boat description.

For example:

```text
4x → Quadruple scull, coxless
4- → Four, coxless
```

The full boat-class mapping will be defined when the Add Boat functionality is implemented.

---

## 7. Users

### Collection

`users`

Each document represents a Row Ready user.

### Current fields

```text
displayName
email
photoUrl
units
notificationsEnabled
createdAt
updatedAt
```

Club membership will be designed separately rather than adding a `clubIds` array at this stage.

---

## 8. User Favourites

Favourites are personal to each user and should not modify the underlying club-owned record.

### Favourite locations

Planned structure:

```text
users/{userId}/favouriteLocations/{locationId}
```

### Favourite boats

Planned structure:

```text
users/{userId}/favouriteBoats/{boatId}
```

The Boats page should eventually allow users to switch between:

- Club Boats
- Favourite Boats

This is particularly useful when clubs have large fleets and users do not want to browse every boat.

---

## 9. Personal Boat Notes / Reviews

Boat notes are private to the individual user.

They are not public club reviews.

### Planned structure

```text
users/{userId}/boatNotes/{boatId}
```

Potential fields include:

```text
comfortable
shoeAdjustment
stretcherAdjustment
fit
notes
createdAt
updatedAt
```

Examples of personal observations:

- Comfortable
- Shoes need adjustment
- Stretcher position
- Good fit
- Personal setup preference

The exact fields will be refined when this functionality is implemented.

---

## 10. Environmental Conditions

### Planned collection

`conditions`

Environmental data will eventually be associated with a location and timestamp.

Potential fields:

```text
locationId
timestamp
windSpeed
windDirection
gustSpeed
rain
temperature
waterLevel
waterLevelTrend
```

The exact fields will depend on the external APIs.

Environmental data should eventually support both current conditions and historical analysis.

---

## 11. Recommendations

### Planned collection

`recommendations`

Recommendations will be generated from environmental conditions and rowing-specific rules.

Potential fields:

```text
locationId
timestamp
recommendation
confidence
bestWindowStart
bestWindowEnd
windStatus
gustStatus
waterLevelStatus
rainStatus
explanation
```

Potential recommendation values:

```text
go
caution
noGo
```

Potential confidence values:

```text
low
medium
high
```

The recommendation should retain the contributing condition statuses and explanation so that users can understand why the recommendation was made.

---

## 12. Routes

### Planned collection

`routes`

Potential fields:

```text
name
locationId
clubId
distance
estimatedDuration
mapData
description
createdAt
updatedAt
```

Routes may eventually be created or managed by clubs and/or users.

---

## 13. Hazards

### Planned collection

`hazards`

Potential fields:

```text
title
description
locationId
routeId
severity
status
createdAt
updatedAt
```

Potential severity values:

```text
info
caution
warning
critical
```

Hazards may eventually include both permanent and temporary information.

---

## 14. Access & Permissions

Firestore security rules should eventually distinguish between:

### Club-managed data

Examples:

- Clubs
- Boats
- Club locations
- Club routes
- Club hazards

### User-private data

Examples:

- Profile information
- Favourite locations
- Favourite boats
- Personal boat notes/reviews
- Personal preferences

Users should only be able to modify data they own or are authorised to manage.

The exact security rules will be implemented as the corresponding functionality is built.

---

## 15. Timestamps

Records that require creation or update tracking should use Firestore timestamps.

Common fields:

```text
createdAt
updatedAt
timestamp
```

Server timestamps should be preferred where appropriate.

---

## 16. Database Development Approach

The database will be implemented incrementally alongside the application.

New collections should only be created when the corresponding functionality is being implemented or when there is a clear need for the collection.

The database documentation should be updated whenever the actual Firestore structure changes.

The implemented Firestore structure should take precedence over earlier proposals in this document.
