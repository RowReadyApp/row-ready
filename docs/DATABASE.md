# Row Ready — Database

## 1. Database Technology

Row Ready uses:

- Firebase
- Cloud Firestore

Firestore is the primary database for application data.

---

## 2. Current Database Status

Firebase and Cloud Firestore are connected to the FlutterFlow project.

The following Firestore collections are currently implemented:

- `clubs`
- `locations`
- `boats`
- `users`

Test documents have been created to verify the database structure and relationships.

Current relationships include:

- Locations reference a club using a Document Reference.
- Boats reference a club using a Document Reference.
- Boats reference a location using a Document Reference.

The following are planned but not yet implemented:

- `conditions`
- `recommendations`
- `routes`
- `hazards`
- Favourite locations
- Favourite boats
- Personal boat notes/reviews
- Firebase Authentication
- Granular user and club-admin permissions

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

Club membership and administrator permissions will be implemented later using Firebase Authentication and appropriate Firestore security rules.

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
Coordinates used for future weather and environmental API requests. These may currently be left empty until location coordinates are populated.

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
The current boat name or identifier.

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
Information about the blades or blade setup used with the boat.

This field is currently flexible and may become more structured in the future.

### Future boat improvements

The Add Boat interface should eventually allow the user to select a standard boat class from a dropdown.

The application should then automatically determine the corresponding boat description and characteristics.

For example:

```text
4x → Quadruple scull, coxless
4- → Four, coxless
```

The full boat-class mapping will be defined when the Add Boat functionality is implemented.

Other planned improvements include:

- Favourite boats
- Club Boats / Favourite Boats toggle
- More structured blade information
- Refined rower weight categories

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

Firebase Authentication has not yet been enabled.

User access is therefore currently restricted by the Firestore security rules.

Club membership will be designed separately rather than adding a `clubIds` array at this stage.

Future user-specific functionality will include:

- Favourite locations
- Favourite boats
- Personal boat notes/reviews
- User preferences

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

## 14. Current Firestore Security

The current prototype uses the following Firestore security model:

| Collection | Create | Read | Write | Delete |
|---|---|---|---|---|
| `clubs` | No One | Everyone | No One | No One |
| `locations` | No One | Everyone | No One | No One |
| `boats` | No One | Everyone | No One | No One |
| `users` | No One | No One | No One | No One |

This is a temporary prototype configuration.

The `clubs`, `locations` and `boats` collections are currently public read-only data. This allows the app to display club, location and boat information while preventing changes through the application.

The `users` collection is currently inaccessible because Firebase Authentication has not yet been enabled.

Test documents can still be managed through the Firebase/FlutterFlow development environment.

---

## 15. Future Access & Permissions

The final security model should distinguish between club-managed data and user-private data.

### Club-managed data

Examples:

- Clubs
- Boats
- Club locations
- Club routes
- Club hazards

Eventually, authorised club administrators should be able to create and manage these records.

Regular users should generally have read-only access.

### User-private data

Examples:

- Profile information
- Favourite locations
- Favourite boats
- Personal boat notes/reviews
- Personal preferences

Users should only be able to access and modify their own private data.

### Firebase Authentication

Firebase Authentication will eventually be enabled to support authenticated users and enforce these permissions.

The future Firestore rules should allow:

- Users to access their own user data
- Users to manage their own favourites
- Users to manage their own personal boat notes/reviews
- Club administrators to manage club-owned information
- Regular users to read club-owned information without modifying it

The exact security rules will be implemented when Firebase Authentication and the corresponding functionality are built.

---

## 16. Timestamps

Records that require creation or update tracking should use Firestore timestamps.

Common fields:

```text
createdAt
updatedAt
timestamp
```

Server timestamps should be preferred where appropriate.

---

## 17. Database Development Approach

The database will be implemented incrementally alongside the application.

New collections should only be created when the corresponding functionality is being implemented or when there is a clear need for the collection.

The database documentation should be updated whenever the actual Firestore structure changes.

The implemented Firestore structure should take precedence over earlier proposals in this document.

Future database decisions should be documented here once they become sufficiently defined or are implemented.
