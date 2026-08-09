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
- `clubLocations`

Test documents have been created to verify the database structure and relationships.

Current relationships include:

- `clubLocations` references a club using a Document Reference.
- `clubLocations` references a location using a Document Reference.
- `boats` reference a club using a Document Reference.
- `boats` reference a location using a Document Reference.

A location is an independent physical rowing location and is not owned by a single club.

Multiple clubs can therefore use the same location through the `clubLocations` relationship.

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

- Keep physical locations separate from clubs
- Allow multiple clubs to use the same location
- Keep club-owned information separate from user-owned information
- Avoid unnecessary duplication
- Use Document References for relationships between entities
- Allow clubs to manage their own locations and boats
- Allow users to maintain personal preferences, favourites and notes
- Support different types of rowing locations, including rivers, lakes, canals and reservoirs
- Support current and future historical environmental data
- Remain flexible enough to support the recommendation engine

A physical location should exist independently of the clubs that use it.

Club-specific relationships and information should be stored separately from the physical location itself.

---

## 4. Clubs

### Collection

`clubs`

Each document represents a rowing club.

### Current fields

- `name`
- `description`
- `logoUrl`
- `createdAt`
- `updatedAt`

### Example

`name`: Bedford Rowing Club

A club can be associated with multiple locations through the `clubLocations` collection.

Club membership and administrator permissions will be defined later.

---

## 5. Locations

### Collection

`locations`

Each document represents a physical rowing location.

A location is independent of any particular club.

Examples include:

- Rivers
- Lakes
- Canals
- Reservoirs
- Other rowing waterways

### Current fields

- `name`
- `waterType`
- `latitude`
- `longitude`
- `status`
- `createdAt`
- `updatedAt`

### Field details

`name`

The display name of the physical location.

Example:

River Great Ouse, Bedford

`waterType`

The type of waterway.

Potential values:

- `river`
- `lake`
- `canal`
- `reservoir`
- `other`

`latitude` / `longitude`

Coordinates used for future weather and environmental API requests.

`status`

Whether the physical location is currently active.

Potential values:

- `active`
- `inactive`

### Important design principle

Locations do not contain a `clubId`.

A location can be used by multiple clubs.

The relationship between clubs and locations is handled by the `clubLocations` collection.

---

## 6. Club Locations

### Collection

`clubLocations`

This collection represents the relationship between a club and a physical location.

It allows multiple clubs to use the same location.

### Current fields

- `clubId`
- `locationId`
- `status`
- `createdAt`
- `updatedAt`

### Field details

`clubId`

A Document Reference to the associated document in the `clubs` collection.

`locationId`

A Document Reference to the associated document in the `locations` collection.

`status`

The status of the relationship between the club and the location.

Potential values:

- `active`
- `inactive`

### Example relationship

Club:

Bedford Rowing Club

Location:

River Great Ouse, Bedford

The relationship is represented by a `clubLocations` document with:

- `clubId` → `clubs/{clubId}`
- `locationId` → `locations/{locationId}`

### Multiple clubs using one location

For example, the same physical location could be associated with multiple clubs:

River Great Ouse, Bedford

- Club A
- Club B
- Club C

Each club has its own `clubLocations` document.

This allows the same physical location to be associated with multiple clubs without duplicating the location itself.

### Future possibilities

The `clubLocations` relationship may eventually contain club-specific information such as:

- Launch information
- Access information
- Club-specific notes
- Club-specific hazards
- Permissions
- Local rules

These fields will be added when the relevant functionality is implemented.

---

## 7. Boats

### Collection

`boats`

Each document represents a boat owned or managed by a club.

### Current fields

- `name`
- `boatClass`
- `clubId`
- `locationId`
- `availabilityStatus`
- `weightCategory`
- `rowingType`
- `coxed`
- `bladeType`
- `createdAt`
- `updatedAt`

### Field details

`name`

The current boat name/identifier.

Example:

Single 1

`boatClass`

The rowing boat classification.

Examples:

- `1x`
- `4x`
- `4-`

`clubId`

A Document Reference to the associated club.

`locationId`

A Document Reference to the boat's associated physical location.

`availabilityStatus`

Whether the boat is currently available.

Potential values:

- `available`
- `unavailable`

`weightCategory`

The intended rower weight category where applicable. The exact categories will be defined later.

`rowingType`

The type of rowing.

Potential values:

- `sculling`
- `sweep`

`coxed`

Boolean indicating whether the boat is coxed.

`bladeType`

Information about the blades/setup used with the boat.

This field is currently flexible and may become more structured in the future.

### Boat relationships

A boat belongs to a club and is associated with a physical location.

The relationship is:

boats/{boatId}

- `clubId` → `clubs/{clubId}`
- `locationId` → `locations/{locationId}`

### Future boat improvements

The Add Boat interface should eventually allow the user to select a standard boat class from a dropdown.

The application should then automatically determine the corresponding boat description.

For example:

- `4x` → Quadruple scull, coxless
- `4-` → Four, coxless

The full boat-class mapping will be defined when the Add Boat functionality is implemented.

---

## 8. Users

### Collection

`users`

Each document represents a Row Ready user.

### Current fields

- `displayName`
- `email`
- `photoUrl`
- `units`
- `notificationsEnabled`
- `createdAt`
- `updatedAt`

Club membership will be designed separately rather than adding a `clubIds` array at this stage.

---

## 9. User Favourites

Favourites are personal to each user and should not modify the underlying club-owned or location-owned record.

### Favourite locations

Planned structure:

`users/{userId}/favouriteLocations/{locationId}`

### Favourite boats

Planned structure:

`users/{userId}/favouriteBoats/{boatId}`

The Boats page should eventually allow users to switch between:

- Club Boats
- Favourite Boats

This is particularly useful when clubs have large fleets and users do not want to browse every boat.

---

## 10. Personal Boat Notes / Reviews

Boat notes are private to the individual user.

They are not public club reviews.

### Planned structure

`users/{userId}/boatNotes/{boatId}`

Potential fields include:

- `comfortable`
- `shoeAdjustment`
- `stretcherAdjustment`
- `fit`
- `notes`
- `createdAt`
- `updatedAt`

Examples of personal observations:

- Comfortable
- Shoes need adjustment
- Stretcher position
- Good fit
- Personal setup preference

The exact fields will be refined when this functionality is implemented.

---

## 11. Environmental Conditions

### Planned collection

`conditions`

Environmental data will eventually be associated with a physical location and timestamp.

Potential fields:

- `locationId`
- `timestamp`
- `windSpeed`
- `windDirection`
- `gustSpeed`
- `rain`
- `temperature`
- `waterLevel`
- `waterLevelTrend`

The exact fields will depend on the external APIs.

Environmental data should eventually support both current conditions and historical analysis.

Environmental conditions belong to the physical location rather than to an individual club.

---

## 12. Recommendations

### Planned collection

`recommendations`

Recommendations will be generated from environmental conditions and rowing-specific rules.

Potential fields:

- `locationId`
- `timestamp`
- `recommendation`
- `confidence`
- `bestWindowStart`
- `bestWindowEnd`
- `windStatus`
- `gustStatus`
- `waterLevelStatus`
- `rainStatus`
- `explanation`

Potential recommendation values:

- `go`
- `caution`
- `noGo`

Potential confidence values:

- `low`
- `medium`
- `high`

The recommendation should retain the contributing condition statuses and explanation so that users can understand why the recommendation was made.

---

## 13. Routes

### Planned collection

`routes`

Potential fields:

- `name`
- `locationId`
- `clubId`
- `distance`
- `estimatedDuration`
- `mapData`
- `description`
- `createdAt`
- `updatedAt`

Routes may eventually be created or managed by clubs and/or users.

Routes are associated with a physical location but may also be associated with a specific club.

---

## 14. Hazards

### Planned collection

`hazards`

Potential fields:

- `title`
- `description`
- `locationId`
- `routeId`
- `severity`
- `status`
- `createdAt`
- `updatedAt`

Potential severity values:

- `info`
- `caution`
- `warning`
- `critical`

Hazards may eventually include both permanent and temporary information.

---

## 15. Access & Permissions

Firestore security rules should eventually distinguish between:

### Club-managed data

Examples:

- Clubs
- Boats
- Club-location relationships
- Club routes
- Club hazards

### Location data

Examples:

- Physical location information
- Environmental conditions
- Location-level hazards

### User-private data

Examples:

- Profile information
- Favourite locations
- Favourite boats
- Personal boat notes/reviews
- Personal preferences

Users should only be able to modify data they own or are authorised to manage.

A club should not automatically own the underlying physical location simply because it uses that location.

The exact security rules will be implemented as the corresponding functionality is built.

---

## 16. Timestamps

Records that require creation or update tracking should use Firestore timestamps.

Common fields:

- `createdAt`
- `updatedAt`
- `timestamp`

Server timestamps should be preferred where appropriate.

---

## 17. Database Development Approach

The database will be implemented incrementally alongside the application.

New collections should only be created when the corresponding functionality is being implemented or when there is a clear need for the collection.

The database documentation should be updated whenever the actual Firestore structure changes.

The implemented Firestore structure should take precedence over earlier proposals in this document.

### Current architecture principle

Physical entities and organisational relationships should be kept separate where appropriate.

For example:

- `locations` represents the physical rowing location.
- `clubs` represents the rowing club.
- `clubLocations` represents the relationship between a club and a location.

This allows the same physical location to be used by multiple clubs without duplicating location records.
