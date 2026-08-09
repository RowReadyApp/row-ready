# Row Ready — Product Requirements Document

## 1. Product Overview

Row Ready is a rowing companion app designed to help rowers make safer and smarter decisions before going on the water.

**Tagline:** Know Before You Row.

The app combines weather conditions, river conditions, club information and rowing-specific information to answer a simple question:

> **Is it a good time to go rowing?**

Row Ready should not simply display raw data. It should interpret the available conditions and provide a clear recommendation, while explaining the factors behind that recommendation.

---

## 2. Target Users

Row Ready is primarily designed for recreational and club rowers.

Users may row:

- In different boats and crew configurations
- At different clubs and locations
- On rivers, lakes, canals or other waterways
- Under changing weather and water conditions

The app should work for both experienced rowers and less experienced users who may benefit from clearer interpretation of conditions.

---

## 3. Core User Journey

The primary user journey is:

1. Open Row Ready
2. Select or view the relevant club/location
3. View current and upcoming conditions
4. Receive an overall rowing recommendation
5. See the confidence associated with the recommendation
6. See the best rowing window when appropriate
7. Understand why the recommendation was made
8. Review individual conditions such as:
   - Wind
   - Gusts
   - Water level
   - Rain
9. Access relevant club, boat and location information
10. Decide whether, when and where to row

---

## 4. Today / Recommendation

The Today page is the main decision-making screen.

It should provide a clear overall recommendation such as:

- GO ROW
- CAUTION
- DO NOT ROW

The recommendation should include:

- Selected club/location
- Overall recommendation
- Best rowing window
- Confidence level
- Last updated time
- Summary of contributing conditions
- Detailed condition information
- Explanation of why the recommendation was made

The app should prioritise interpretation over simply displaying measurements.

For example:

> **GO ROW**  
> High confidence  
> Best rowing window: 09:00–11:00  
>
> Wind: Good  
> Gusts: Good  
> Water level: Good  
> Rain: Good  
>
> **Why is it a Go?**  
> Low wind and gusts are expected throughout the rowing window, with suitable water levels and low rainfall risk.

The recommendation engine should eventually consider multiple factors together rather than relying on a single threshold.

---

## 5. Weather & Environmental Conditions

Row Ready should provide relevant environmental information, including:

### Weather

- Wind speed
- Wind direction
- Gusts
- Rain / precipitation
- Temperature
- Other relevant weather variables as required

### Water conditions

- River level
- River level trends where available
- Other relevant waterway information

The app should distinguish between raw measurements and their interpretation.

For example:

> Wind: 12 km/h — Good

rather than only:

> Wind: 12 km/h

---

## 6. Locations

Users should be able to view rowing locations and relevant information about them.

A location may represent:

- River
- Lake
- Canal
- Reservoir
- Other rowing water

Locations should eventually include:

- Location name
- Area
- Water type
- Club association
- Coordinates / map location
- Relevant conditions
- Location-specific information
- Favourite status

Users should be able to favourite locations for quick access.

---

## 7. Clubs

Row Ready should support rowing clubs and club-specific information.

A club may have:

- Club profile
- Members/users
- Locations
- Boat fleet
- Club-specific information
- Club-managed data

Users should be able to select a club and see information relevant to that club.

Club selection should influence relevant content such as available boats and favourite boats.

---

## 8. Boats

Row Ready should provide information about boats available at a selected club.

Boat information may include:

- Boat name / ID
- Boat class
- Location within the club (e.g. hangar, rack, row)
- Availability
- Sculling / sweep configuration
- Coxed / coxless configuration
- Weight category
- Blade/equipment information
- Other relevant setup information

The boat class should eventually be selectable from standard rowing classifications, with the app automatically generating the corresponding description.

For example:

- 4x → Quadruple scull, coxless
- 4- → Four, coxless

Users should eventually be able to:

- Favourite boats
- Filter between club boats and favourite boats
- View boat details
- Add private personal notes/reviews

Personal boat notes are intended for the individual user rather than public club reviews.

Examples include:

- Comfortable
- Shoes need adjustment
- Stretcher position
- Personal setup preferences
- General observations

---

## 9. Routes

Row Ready should eventually allow users/clubs to access rowing routes.

Routes may include:

- Route name
- Location
- Distance
- Map
- Typical duration
- Club association
- Relevant hazards or restrictions

Routes may eventually be created or shared by clubs and users.

---

## 10. Hazards & Safety Information

The app should eventually provide relevant hazards and safety information for rowing locations.

Potential information includes:

- Navigation hazards
- Obstructions
- Strong currents
- High water
- Restricted areas
- Temporary warnings
- Club-specific notices

Hazards should be clearly associated with the relevant location or route.

---

## 11. Personalisation

Users should eventually have a personal profile containing relevant preferences.

Potential personalisation includes:

- Profile information
- Club membership
- Preferred units
- Notifications
- Favourite locations
- Favourite boats
- Personal boat notes
- Other rowing preferences

Personal information and private notes should not automatically be visible to other users.

---

## 12. Condition History & Analytics

Future versions should allow users to view historical conditions.

Potential features include:

- Historical weather
- Historical river levels
- Historical recommendations
- Condition trends
- Changes over time
- Relationship between conditions and rowing recommendations

---

## 13. Notifications

Row Ready should eventually provide useful notifications, such as:

- Significant changes in conditions
- High water
- Strong winds
- Club/location warnings
- Changes to a previously recommended rowing window

Notifications should be useful and avoid unnecessary alerts.

---

## 14. Wearables & Widgets

Future versions may provide quick access to Row Ready information through:

- Home screen widgets
- Apple Watch
- Garmin
- Other supported wearable platforms

The focus should be on quickly checking whether conditions are suitable for rowing.

---

## 15. Recommendation Principles

The recommendation engine should:

- Combine multiple environmental factors
- Consider the selected location
- Consider the relevant time window
- Provide an overall recommendation
- Provide a confidence level
- Identify the key factors influencing the recommendation
- Explain the recommendation in plain language

The recommendation should be transparent rather than a black box.

Users should be able to understand **why** Row Ready recommends going or not going.

---

## 16. MVP Priorities

The initial MVP should focus on:

1. Today / recommendation
2. Weather
3. Gusts
4. River levels
5. Recommendation engine
6. Favourite locations
7. Clubs
8. Boats
9. Basic user profile

More advanced features such as condition history, wearables and analytics can follow after the core experience is established.

---

## 17. Product Principle

Row Ready should answer the question:

> **"Should I go rowing, when should I go, and why?"**

The app should make this decision easier without hiding the underlying information from the user.
