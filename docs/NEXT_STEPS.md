# Row Ready — Next Steps

_Last updated: 2026-08-09_

This document is the handoff point for the next Row Ready development session. Read this file together with `docs/DATABASE.md` and `docs/CHANGELOG.md` before continuing work.

---

## Current checkpoint — v0.2.2

The current FlutterFlow/Firebase implementation has been tested successfully.

### Working functionality

- Locations page has a club dropdown.
- Clubs are loaded from the `clubs` collection.
- The selected club is stored as the `selectedClub` Document Reference.
- The initial `selectedClub` state has been fixed and works on page load.
- The locations list uses the `clubLocations` collection.
- The list query filters:

  `clubId == [selectedClub]`

- Changing the selected club changes the location list.
- Multiple clubs can use the same physical location.
- Location cards display:
  - `locationName`
  - `waterType`
  - `status`
- The Club text follows the selected club.
- The Container/card itself has no additional backend query.

This is a confirmed working checkpoint. **Do not break or restructure this working setup without a reason.**

---

## Important current database design

### `locations`

Represents the physical location.

`locationId` is the canonical reference to this physical location.

### `clubs`

Represents the rowing club.

### `clubLocations`

Represents the relationship between a club and a physical location.

Current relevant fields:

- `clubId` → Document Reference to `clubs`
- `locationId` → Document Reference to `locations`
- `locationName` → currently duplicated from `locations.name`
- `waterType` → currently duplicated from `locations.waterType`
- `status` → currently used by the location card

The same physical location can therefore have multiple `clubLocations` documents, one for each club using it.

---

# Next development tasks

## 1. Clean up duplicated location data — FIRST TASK

**Status: Planned / not yet started**

We currently duplicate:

- `locations.name` → `clubLocations.locationName`
- `locations.waterType` → `clubLocations.waterType`

The reason for the duplication was a FlutterFlow limitation encountered while trying to follow:

`clubLocations.locationId` → `locations` document → location fields

The current working UI reads `locationName` and `waterType` directly from the `clubLocations` document.

### Goal

Determine whether FlutterFlow can now reliably retrieve the referenced `locations` document from `clubLocations.locationId` without adding an unnecessary query or breaking the repeating list.

### Before changing anything

- Preserve the current working `clubLocations` query.
- Do not remove `locationId`.
- Do not change the selected-club logic.
- Test any proposed change with at least two clubs and a shared location.

### If the reference can be followed reliably

Move the UI back to using the canonical `locations` document for:

- location name
- water type

Then remove the duplicated fields from `clubLocations` only after the UI has been tested.

### If FlutterFlow still cannot follow the reference cleanly

Keep the duplicated fields for now. Do not sacrifice a working architecture just to remove a small amount of duplication.

Document the decision and revisit it later.

---

## 2. Review `clubLocations.status`

**Status: Planned**

Clarify whether `status` is intended to represent:

- the physical location's global status, or
- the club's relationship/access status for that location.

The preferred architecture is that physical-location attributes belong to `locations`, while club-specific access/relationship attributes belong to `clubLocations`.

If `status` is club-specific, keep it in `clubLocations`.

If it is global to the physical location, it should eventually live in `locations` instead.

Do not change this until the intended meaning is confirmed.

---

## 3. Add/edit location workflow

**Status: Planned**

The Locations page already has an Add Location button in the prototype.

Next, implement the actual workflow for adding a location.

Important design requirement:

Because multiple clubs can use the same physical location, adding a location should distinguish between:

1. Creating a brand-new physical `locations` document.
2. Adding an existing physical location to a club by creating a `clubLocations` document.

Do not automatically create duplicate physical locations when another club wants to use an existing location.

---

## 4. Review the location data model before building more UI

**Status: Planned**

Confirm the final fields for `locations` and `clubLocations` before adding more location functionality.

Potential future location information may include:

- coordinates
- access information
- rowing direction
- navigation rules
- club-specific notes
- hazards
- launch information

Keep physical-location data separate from club-specific data where possible.

---

## 5. Boats — continue after Locations are stable

**Status: Prototype already exists; functionality still to be built**

The Boats page currently has:

- Available Boats
- Unavailable Boats
- Boat cards
- Boat name/type/location/status
- Add Boat button

Future improvements already identified:

- Standard boat-class dropdown
- Automatic boat-class descriptions
- Club-specific favourite boats
- Club Boats / Favourite Boats toggle
- Private user boat notes/reviews
- Boat equipment and blade information

Do not start these until the current Locations architecture is stable.

---

## 6. Today page / recommendation engine

**Status: Future**

The Today page is currently a UI prototype.

Future functionality will eventually use:

- location
- weather/environmental conditions
- wind
- gusts
- rain
- water level
- rowing-specific rules

The recommendation should eventually produce something like:

- Go
- Caution
- No Go

with an explanation and confidence level.

Do not build the recommendation engine before the location model and location selection flow are stable.

---

## 7. Authentication and permissions

**Status: Future**

Firebase Authentication is not yet implemented.

Eventually distinguish between:

- public/read-only information
- authenticated user information
- club-managed information
- private user information

Security rules will need to evolve as authentication and club administration are implemented.

---

# Development rules for the next session

1. **Start by reading this file.**
2. Read `docs/DATABASE.md` if the database structure is involved.
3. Read `docs/CHANGELOG.md` for the latest milestone.
4. Treat the current v0.2.2 implementation as a working checkpoint.
5. Make one structural change at a time.
6. Test after each significant FlutterFlow/database change.
7. Do not remove working fields or queries until the replacement has been tested.
8. When a milestone is successfully tested, update the changelog and relevant documentation in GitHub.
9. Keep the physical `locations` entity separate from club/location relationships.
10. Remember that **multiple clubs can use the same physical location**.

---

# Immediate next action when we resume

**Start with Task 1: investigate whether `clubLocations.locationId` can now be used to read the canonical `locations` document in FlutterFlow.**

If successful, remove the temporary `locationName` and `waterType` duplication only after testing.

If unsuccessful, leave the current working implementation unchanged and continue with the next appropriate Locations task.
