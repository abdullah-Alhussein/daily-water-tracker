# Requirements Document — Daily Water Tracker

## 1. Problem Statement
A lightweight, interactive web application that helps users calculate their recommended daily water intake based on body weight and activity level, and visually track their progress cup by cup throughout the day.

## 2. Functional Requirements
- **FR-01 (User Profile Input):** The user can enter their body weight (in kg) and select their daily activity level (`Low`, `Moderate`, `High`).
- **FR-02 (Target Calculation):** When the user submits their details, the app calculates and displays the daily water target in both **liters (L)** and **cups**.
- **FR-03 (Cup Logging):** The user can click a **+1 Cup** button to increment their consumed water count, a **-1 Cup** button to undo a mistake, and a **Reset** button to start over.
- **FR-04 (Live Progress Bar):** A visual progress bar updates its fill width and percentage immediately with each click, changing color according to progress milestones.
- **FR-05 (Goal Completion Feedback):** When the user reaches or exceeds 100% of their daily goal, a clear completion message is displayed.

## 3. Business Rules
- **BR-01 (Intake Formula):**
  - Base intake = `weight_kg * 35` ml.
  - Activity bonus: `Low = +0 ml`, `Moderate = +350 ml`, `High = +700 ml`.
  - Daily target (liters) = `total_ml / 1000`.
- **BR-02 (Cup Conversion):** One standard cup equals **250 ml**. Total target cups are rounded up (`Math.ceil(total_ml / 250)`).
- **BR-03 (Progress Bar Color Thresholds):**
  - `0% – 49%`: Orange / Warning tone.
  - `50% – 99%`: Blue / In-progress tone.
  - `100%+`: Green / Goal reached tone (visual bar width is capped at `100%` so it does not overflow the container).

## 4. Non-Functional Requirements
- **NFR-01 (Input Validation):** Reject empty, non-numeric, zero, or negative weight values (and unrealistic values like `> 300 kg`) with a clear inline error message—no browser `alert()` popups.
- **NFR-02 (Instant Feedback):** All calculations and DOM updates happen instantaneously without reloading the page.
- **NFR-03 (Clean Responsive Layout):** Centered card layout using CSS Flexbox that remains readable on both desktop and mobile screens.

## 5. Edge Cases
- Clicking **+1 Cup** or **-1 Cup** before calculating a daily target (buttons should be disabled until a valid target is set).
- Clicking **-1 Cup** when current cups = `0` (must never drop below `0`).
- Drinking more than the daily target (e.g., 10/8 cups): cup counter continues incrementing, while the progress bar width stays capped at `100%`.
- Entering decimal weight values (e.g., `72.5` kg) should be handled smoothly.

## 6. Out of Scope (v1)
- No backend server, user authentication, or database storage.
- No automatic midnight reset or multi-day history log.