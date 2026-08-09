# Row Ready — Database

## 1. Database Technology

Row Ready uses:

- Firebase
- Cloud Firestore

Firestore is the primary database for application data.

The database should support:

- Users
- Clubs
- Locations
- Boats
- Routes
- Hazards
- User preferences
- Favourites
- Personal notes
- Environmental conditions
- Recommendations

---

## 2. Database Principles

The database should:

- Keep club-owned information separate from user-owned information
- Avoid duplicating data where possible
- Allow users to belong to one or more clubs in the future
- Allow clubs to manage their own locations and boats
- Allow users to maintain private preferences, favourites and notes
- Support locations that are rivers, lakes, canals, reservoirs or other waterways
- Allow environmental data to be associated with a location and time
- Keep the structure flexible enough to support future recommendation and history features

---

## 3. Users

### Collection

`users`

Each document represents one Row Ready user.

Potential fields:

```text
displayName
email
photoUrl
units
notificationsEnabled
createdAt
updatedAt
```

Future fields may include:

```text
clubIds
favouriteLocationIds
favouriteBoatIds
rowingPreferences
```

User-specific information should only be accessible according to the appropriate permissions.

---

## 4. Clubs

### Collection

`clubs`

Each document represents a rowing club.

Potential fields:

```text
name
description
location
logoUrl
createdAt
updatedAt
```

Future fields may include:

```text
contactInformation
website
adminUserIds
memberUserIds
```

Club-owned information should be controlled by authorised club users.

---

## 5. Locations

### Collection

`locations`

Each document represents a rowing location.

Potential fields:

```text
name
area
waterType
clubIds
latitude
longitude
status
createdAt
updatedAt
```

Examples of `waterType`:

```text
river
lake
canal
reservoir
other
```

A location should not assume that the water body is always a river.

A location may be associated with one or more clubs where appropriate.

---

## 6. Boats

### Collection

`boats`

Each document represents a boat.

Potential fields:

```text
name
boatClass
clubId
locationId
locationDetail
availabilityStatus
weightCategory
rowingType
coxed
createdAt
updatedAt
```

Examples of `rowingType`:

```text
sculling
sweep
```

Examples of `availabilityStatus`:

```text
available
unavailable
```

`locationDetail` may contain information such as:

```text
hangar
rack
row
bay
```

The boat name may eventually be generated automatically from the boat class and boat ID.

For example:

```text
4x → Quadruple scull, coxless
4- → Four, coxless
```

The exact boat-class mapping will be defined separately when the Add Boat functionality is implemented.

---

## 7. Boat Favourites

Favourite boats are personal to each user.

A user's favourite boats should not change the underlying club boat record.

Potential structure:

```text
users/{userId}/favouriteBoats/{boatId}
```

This allows the club to maintain the official boat information while each user maintains their own favourites.

The Boats page should eventually allow:

- Club Boats
- Favourite Boats

to be selected using a toggle/filter.

---

## 8. Personal Boat Notes / Reviews

Boat notes are private to the individual user.

They are not intended to be public club reviews.

Potential structure:

```text
users/{userId}/boatNotes/{boatId}
```

Potential fields:

```text
comfortable
shoeAdjustment
stretcherAdjustment
fit
notes
createdAt
updatedAt
```

The exact fields may be refined later.

Examples of personal observations:

- Comfortable
- Shoes need adjustment
- Stretcher position
- Good fit
- Personal setup preference

---

## 9. Routes

### Collection

`routes`

Each document represents a rowing route.

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

## 10. Hazards

### Collection

`hazards`

Each document represents a hazard or warning associated with a location or route.

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

Hazards may eventually include temporary and permanent information.

---

## 11. Environmental Conditions

Environmental data should eventually be associated with a location and a timestamp.

### Collection

`conditions`

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

The exact fields will depend on the data returned by the external APIs.

Environmental data should be stored in a way that can support both current conditions and future historical analysis.

---

## 12. Recommendations

Recommendations are generated from environmental conditions and rowing-specific rules.

### Collection

`recommendations`

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

The recommendation should retain the underlying condition statuses and explanation so that the user can understand why a recommendation was made.

---

## 13. Favourites

Favourite locations are personal to each user.

Potential structure:

```text
users/{userId}/favouriteLocations/{locationId}
```

Favourite status should not modify the underlying location record.

---

## 14. Club Ownership

Club-owned resources should be associated with the relevant club.

Examples:

```text
boats → clubId
locations → clubIds
routes → clubId
hazards → clubId where applicable
```

Club administrators should eventually be able to create and update club-owned information.

Regular users should not be able to modify official club information unless they have the appropriate permissions.

---

## 15. Access & Permissions

Firestore security rules should eventually distinguish between:

### Public / general app data

Examples:

- Locations
- Environmental conditions
- Recommendations

### Club-managed data

Examples:

- Club information
- Boats
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

---

## 16. Timestamps

Firestore timestamps should be used for records that require creation or update tracking.

Common fields:

```text
createdAt
updatedAt
timestamp
```

Server timestamps should be preferred where appropriate.

---

## 17. Future Considerations

The database may eventually need to support:

- Historical environmental conditions
- Historical recommendations
- Notifications
- User rowing activity
- Club announcements
- Community reports
- Route tracking
- Wearable integrations
- Analytics

The schema should not be over-engineered for these features until their requirements are defined.
