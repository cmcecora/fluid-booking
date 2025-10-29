# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a CodePen project demonstrating a fluid booking form UI/UX concept. Originally created at https://codepen.io/chriscec/pen/MYKBXVN. The project focuses on making a complex booking process simple and frictionless through fluid animations that maintain spatial relationships between UI elements during state transitions.

## Architecture

### Source vs Distribution

- **`src/`**: Source files requiring compilation
  - `index.pug`: Pug template that compiles to HTML
  - `style.scss`: SCSS styles that compile to CSS
  - `script.js`: jQuery-based JavaScript (same in both src and dist)

- **`dist/`**: Compiled/production files
  - `index.html`: Compiled from Pug
  - `style.css`: Compiled from SCSS
  - `script.js`: Copy of source JavaScript

### State Management System

The booking flow is managed through CSS classes on the `.wrap` container that control visibility and animation of UI elements. The state progression is:

1. **Initial state** → Staff member selection
2. **`member-selected`** → Calendar appears for chosen staff
3. **`date-selected`** → Time slots appear for chosen date
4. **`slot-selected`** → Booking form appears
5. **`booking-complete`** → Confirmation message

Each state transition uses CSS animations with `perspective` transforms and opacity changes to create spatial continuity.

### Key Interactions

**Selection handlers** (`src/script.js`):
- `.member` click → triggers `addCalendar()` to dynamically generate calendar
- Calendar date click → triggers `addSlots()` to randomly generate time slots
- Slot click → reveals booking form with auto-focus
- Form submit → shows confirmation

**Deselection handlers**:
- `.deselect-member` → resets to initial state
- `.deselect-date` → removes date/slot selection
- `.deselect-slot` → removes slot selection only
- `.restart` → full reset after booking complete

### Dynamic Content Generation

**`addCalendar(container)`** (`src/script.js:89-144`):
- Generates current month's calendar table
- Disables past dates and weekends
- Creates clickable date cells with data attributes
- Attaches event listeners via `invokeCalendarListener()`

**`addSlots()`** (`src/script.js:68-85`):
- Generates random number (1-6) of time slots
- Creates random times between 7:00-10:00+ with 15-minute intervals
- Appends to `.selected .slots` container
- Attaches event listeners via `invokeSlotsListener()`

## Development Notes

### Dependencies

- jQuery 3.1.0 (loaded via CDN)
- Google Fonts: Alegreya Sans
- No build tools configured (manual Pug/SCSS compilation required)

### SCSS Variables (`src/style.scss:1-11`)

- Primary color system based on `$hue: 241` (blue)
- Transition timing: `$short: 300ms`, `$mid: 500ms`, `$long: 800ms`
- Custom easing: `$ease-in-out: cubic-bezier(0.645, 0.045, 0.355, 1.000)`
- Font scaling via `html { font-size: 1.3px }` with rem units

### Current Limitations

As noted in README, this is a concept with incomplete functionality:
- Month/year navigation not implemented
- No backend integration (form submission is prevented)
- Time slots are randomly generated, not from real availability
- Calendar only shows current month
