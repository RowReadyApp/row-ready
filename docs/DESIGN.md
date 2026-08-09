# Row Ready — Design Guidelines

## 1. Design Principles

Row Ready should feel:

- Clear
- Calm
- Practical
- Trustworthy
- Easy to scan before rowing

The interface should prioritise the most important decision first, followed by the information needed to understand that decision.

The design should avoid unnecessary complexity and visual clutter.

---

## 2. Core Information Hierarchy

The primary hierarchy for the app is:

1. **Recommendation**
2. **Confidence**
3. **Best rowing window**
4. **Key conditions**
5. **Explanation**
6. **Detailed information**

For example, the Today page should make the following immediately visible:

> GO ROW  
> High confidence  
> Best rowing window: 09:00–11:00

The user can then see the individual conditions and understand why the recommendation was made.

---

## 3. Layout

The app uses a simple vertical, card-based layout.

### Page layout

- Main content is contained within a vertical Column
- Standard page padding: **24 px**
- Content should generally stretch to the available width
- Sections are vertically stacked
- Avoid unnecessary horizontal complexity

### Cards

Cards are used to group related information.

Standard card properties:

- Padding: **16 px**
- Corner radius: **16 px**
- Background: **Secondary Background**
- No unnecessary shadow
- Consistent spacing between cards

---

## 4. Spacing

The app uses a simple spacing system.

Current conventions include:

- **24 px** page padding
- **16 px** card/container padding
- **4 px** spacing between closely related label/value elements
- Larger spacing between separate sections

Spacing should be consistent across pages.

Avoid adding padding to individual elements when the parent container already provides the required spacing.

---

## 5. Typography

The typography hierarchy should make information easy to scan.

### Page titles

- Large
- Semi-bold
- Primary text colour
- Left aligned

### Section titles

- Medium emphasis
- Primary text colour

### Labels

- Approximately **12 px**
- Secondary text colour

### Values

- Approximately **14–16 px**
- Primary text colour

### Important values

Important information such as recommendations or location names may use a larger or heavier style.

---

## 6. Colour

The design uses a restrained colour palette.

### Primary text

- Black
- Used for most important text and headings

### Primary colour

- Navy blue
- Used for primary actions and selected elements

### Secondary background

- Used for cards and grouped content

### Success

- Used for positive states such as:
  - Available
  - Good conditions
  - Positive recommendation indicators

### Status colours

Status colours should communicate meaning consistently throughout the app.

Avoid using colour as the only way to communicate important information.

---

## 7. Buttons & Actions

Primary actions should use the primary/navy colour.

Examples:

- Add boat
- Add location
- Other major actions

Buttons should be simple and clearly labelled.

Icons should only be used when they add useful meaning.

---

## 8. Navigation & Transitions

The app uses straightforward navigation between major pages.

The Splash screen transitions to the Today page using a **fade transition**.

Back navigation should be disabled or replaced where returning to the previous page would not make sense, such as after the Splash screen.

Navigation should feel predictable and unobtrusive.

---

## 9. Today Page

Today is the main decision-making screen.

The main recommendation card contains:

- Club/location
- GO ROW / other recommendation
- Best rowing window
- Confidence
- Last updated time
- Summary of key conditions

Example:

> Bedford Rowing Club  
> GO ROW  
> Best rowing window: 09:00–11:00  
> High confidence  
> Updated 2 min ago  
>
> Wind: Good  
> Gusts: Good  
> Water level: Good  
> Rain: Good

Detailed condition cards appear below the main recommendation.

The page also contains a:

### Why is it a Go?

section explaining the reasoning behind the recommendation in plain language.

The user should understand both **the decision and the reasons behind it**.

---

## 10. Boats Page

The Boats page uses cards to display individual boats.

Boat cards currently include information such as:

- Boat name
- Boat type
- Location within the club
- Availability

Boats are separated into:

- Available Boats
- Unavailable Boats

Available status uses the Success colour.

The page includes an **Add boat** action.

Future boat functionality should not overload the main list. Details such as personal notes, equipment and additional setup information should be available through the boat details experience.

---

## 11. Locations Page

The Locations page uses the same card design as Boats.

Location cards currently contain:

- Location
- Club
- Status

Location and area are currently combined into a single value.

Example:

> River Great Ouse, Bedford

The design should support different types of water, so locations should not assume that every rowing location is a river.

The page includes an **Add location** action.

---

## 12. Profile Page

The Profile page is divided into simple sections/cards:

### Personal

- Name
- Email

### Club

- Club
- Membership

### Preferences

- Units
- Notifications

Profile information should remain simple and easy to edit as functionality is added.

---

## 13. Empty & Loading States

Future functional screens should include clear states for:

- Loading
- No data
- Error
- Unavailable information

These states should maintain the same visual language as the rest of the app.

---

## 14. Design Direction

Row Ready should favour:

**Simple over complex**  
**Readable over dense**  
**Useful information over decoration**  
**Clear recommendations over raw data**

The interface should allow a rower to understand the situation quickly, particularly when checking conditions shortly before going on the water.
