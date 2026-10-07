# Windsurf Racing Timer & Tracker

A specialized Garmin Connect IQ application designed for **windsurfing, foiling, wing slalom, course racing, and training sessions**. Built exclusively for Garmin watches with a **5-button layout**, the app provides precise start-line timing, automatic tack detection, and comprehensive session analytics — all optimized for the demands of racing on the water.

---

## Table of Contents

- [Core Features](#core-features)
- [Supported Devices & Screen Layouts](#supported-devices--screen-layouts)
- [Menu Structure](#menu-structure)
  - [1. Timer](#1-timer)
  - [2. Practice (Tracker View)](#2-practice-tracker-view)
  - [3. Markers Menu](#3-markers-menu)
  - [4. Setup Menu](#4-setup-menu)
  - [5. GPS Status](#5-gps-status)
  - [6. Gear Menu](#6-gear-menu)
  - [7. Quit](#7-quit)
- [Session Summary](#session-summary)
- [Screenshots](#screenshots)
- [Changelog](#changelog)
- [Installation](#installation)

---

---

## Core Features

| Feature | Description |
|---|---|
| **Target Sport** | Windsurfing, Foiling, Wing Slalom, Course Racing, and practicing |
| **Button-Only Control** | All navigation is done exclusively with physical buttons — wet hands won't interfere with operating the app on the water |
| **Unified Timer + Tracker** | Start timer and practice tracker live in one application, so activity recording is never interrupted when switching between modes |
| **On-Water Tack & Jybe Analysis** | Maximum speed on the previous tack and minimum speed during a jybe are shown right after the maneuver — and the full tack history can be reviewed directly on the water |
| **Fully Customizable** | A wide range of setup options let you tailor the app's behavior to your own preferences |
| **Start Line Prediction** | Calculates your time to the start line and tells you whether to speed up or slow down |
| **Jybe Hints** | Vibration-guided maneuvers that deliver you to the start line exactly on time |
| **Gear & Impressions in FIT** | Equipment database and session impressions are recorded into the `.fit` file |
| **Start Line in FIT** | Saved start line points are recorded into the `.fit` file and remain available for post-session analysis |
| **Activity Recording** | Starts automatically on application launch upon the **first stable GPS fix** |
| **Session Handling** | Saved or discarded via an exit dialog upon quitting |
| **Typography** | Dynamic Vector Font handling with custom 7-segment display fallback |
| **Precision Timing** | Millisecond-based timer accuracy; no drift on multiple time syncs |

---

## Supported Devices & Screen Layouts

The app supports **over 96 models** — all modern round and semi-octagon screens with 5 physical buttons. It features completely unique, highly optimized layouts for each of the following resolutions to maximize font sizes for crucial metrics (seconds, speeds).

| Resolution | Layout Type | Notes |
|---|---|---|
| **454 × 454** | Round | Increased timer seconds font size (v3.8) |
| **416 × 416** | Round | Added in v5.3 |
| **280 × 280** | Round | — |
| **260 × 260** | Round | Vector font and non-vector font variants handled separately |
| **240 × 240** | Round | — |
| **176 × 176** | Semi-octagon | — |

> **Typography Note:** The app dynamically handles **Vector Font availability**. For instance, on 260×260 screens, some devices have vector fonts (e.g., Forerunner 955) while others do not (e.g., Fenix 6). On devices lacking vector fonts, the app automatically utilizes a custom, **arbitrary-sized 7-segment display** for the timer digits to ensure perfect readability.

---

## Menu Structure

Upon launching, the user sees a main menu with **7 items**. Each section is detailed below.
![mainmenu1](screenshots/mainmenu1.png)
![mainmenu2](screenshots/mainmenu2.png)

---

### 1. Timer

The **Timer** is both a standard countdown timer and a full-featured **start procedure computer**. Beyond the classic regatta countdown, it helps you execute the entire start sequence using two intelligent assistants:

- **Start Line Prediction** — calculates your time to the start line and tells you whether to speed up or slow down.
- **Jybe Hints** — vibrates at the right moments to guide you through maneuvers and deliver you to the line exactly on time.

Together, these features turn the Timer into a complete virtual coach for the start.
![fenix 8 timer](screenshots/round454x454/timer.png)
Basic timer view on Fenix 8 Pro

**Controls:**

| Button | Action |
|---|---|
| **Left Middle** | Start / Pause timer |
| **Left Bottom** | Reset to default setup minutes |
| **Right Upper** | Sync seconds to zero (round to nearest minute) |

**Audio / Vibration:**
- Beeper and vibration alerts inspired by **Optimum Time Series** yachting watches.
- Controlled via two separate toggles in the **Setup** menu:
  - **Timer beeper** — On / Off
  - **Timer vibrate** — On / Off

**Timer Program Configuration:**
- Configured in the **Setup** menu:
  - **Timer program** — countdown duration from **2 to 12 minutes**
  - **Timer loop** — On / Off (looping the countdown, useful for training sessions)

**Auto-Switching Logic:**
- In **non-loop mode**, 5 seconds after reaching zero, if current speed exceeds the threshold (**default 30 km/h**), the app automatically switches to the **Practice View**.
- If the user crashes or stalls and speed drops below the lower threshold (**default 10 km/h**), the app automatically switches **back to the Timer screen**.

#### Start Line Prediction

The start line prediction is one of the app's most powerful features for racing. It is controlled in the **Setup** menu via the **Timer predict interval** parameter — the number of seconds before the start at which the prediction should be enabled (**0 to 90**).

**How it works:**

1. Save the **Boat** and **Pin** positions of the start line in the **Markers** menu.
2. Once the timer countdown reaches the configured **Timer predict interval** (e.g., 30 seconds before start), the prediction becomes active.
3. The watch uses your **current position**, **speed**, and **course** to calculate the **time to cross the start line** defined by the saved Boat and Pin.
4. The screen then shows:
   - **Predicted time** to the start line
   - **Discrepancy** between the predicted time and the timer value
   - A **hint** indicating whether you should **speed up** or **slow down**

> **GPS Precision Note:** GPS has a ~3-meter dispersion. If you start at lower speeds (for course racing and yachts), this may result in 1–2 seconds of error in predictions. Adding more pins increases pin coordinate accuracy as it averages the coordinates.

#### Jybe & Start Zone Hints

The timer also helps you time your maneuvers and approach the start line correctly. This is done through a combination of **vibration alerts** and **calculations** based on the **Jybe seconds** and **Start zone** parameters in the Setup menu.

**How it works:**

- The watch **vibrates when you cross the start line**.
- After that, the watch **vibrates again when it's time to begin your jybe/tack maneuver**.
- The algorithm is driven by two Setup parameters:
  - **Jybe seconds** — how much time to allocate for performing the maneuver (e.g., 15 seconds).
  - **Start zone** — how much space from the start line you need to sail on a straight course, specified in **seconds** (e.g., 30 seconds).

**Example:**
> With **Jybe seconds = 15** and **Start zone = 30**, the watch will guide you so that you arrive at a point from which the distance to the start line equals **30 seconds**, exactly **30 seconds before the start**. The calculation is based on your **current position** and **speed**, and it updates dynamically.

This provides a reliable countdown sequence:
1. Timer crosses the configured interval → prediction starts.
2. Vibration signals when to begin the jybe/tack.
3. Vibration signals when the start line is crossed.
4. Continuous hinting (speed up / slow down) to hit the line exactly on time.

#### Timer Screenshots

<!-- Add your Timer screenshots here for each resolution and font variant -->

**Round 454 × 454**

![Timer 454x454](screenshots/round454x454/timer.png)

*Width: 220px*

**Round 416 × 416**

![Timer 416x416](screenshots/timer_416x416.png)

*Width: 220px*

**Round 280 × 280**

![Timer 280x280](screenshots/timer_280x280.png)

*Width: 220px*

**Round 260 × 260 — Vector Font (e.g., Forerunner 955)**

![Timer 260x260 Vector](screenshots/timer_260x260_vector.png)

*Width: 220px*

**Round 260 × 260 — Non-Vector Font / 7-Segment (e.g., Fenix 6)**

![Timer 260x260 7-Segment](screenshots/timer_260x260_7seg.png)

*Width: 220px*

**Round 240 × 240**

![Timer 240x240](screenshots/timer_240x240.png)

*Width: 220px*

**Semi-octagon 176 × 176**

![Timer 176x176](screenshots/timer_176x176.png)

*Width: 220px*

#### Start Line Prediction & Jybe Hints Screenshots

<!-- Add your Start Line Prediction and Jybe/Start Zone hint screenshots here -->

![Start Line Prediction](screenshots/start_line_prediction.png)

*Width: 220px*

![Jybe Hint](screenshots/jybe_hint.png)

*Width: 220px*

![Start Zone Hint](screenshots/start_zone_hint.png)

*Width: 220px*

---

---

## 2. Practice (Tracker View)

The Practice view is the core training screen, displaying real-time metrics and automatic tack logging. Its main purpose is to let you **analyze speeds and rig/gear settings right on the water** — without needing to go ashore for a phone or computer.

### Displays

- Current speed
- Session distance travelled
- Current heart rate
- Current local time
- Maximum **2-second average speed** for the previous tack
- **Direction arrow to Home** (if a Home point has been set in the Markers menu)

### Tack Detection & Navigation

- Tacks are detected automatically.
- **Up/Down buttons** step through the tacks history.
- **Right Upper button** returns to the active (current) leg.
- Arrows indicate **current heading** and **average heading** — used internally to control tack detection.

### Jybe Speed Analysis

- Currently available **only on 260×260 devices** and is being tested by an athlete.
- The parameter is displayed **after every maneuver** and can be reviewed in the **tacks history** — not only after the session.
- This allows you to evaluate jybe performance instantly, on the water, and adjust your technique or gear on the fly.

### Session Summary Popup

Pressing the **Right Upper button** while tracking brings up a universal overlay with the following metrics:

- **Peak speed**
- **2s speed**
- **100m speed**
- **Distance**
- **Duration**
- **Max heart beat**
- **Calories burned**

Pressing **Right Bottom (Back)** always closes the summary and returns to the current tack display.

### Practice Screenshots

<!-- Add your Practice screenshots here for each resolution and font variant -->

**Round 454 × 454**

![Practice 454x454](screenshots/practice_454x454.png)

*Width: 220px*

**Round 416 × 416**

![Practice 416x416](screenshots/practice_416x416.png)

*Width: 220px*

**Round 280 × 280**

![Practice 280x280](screenshots/practice_280x280.png)

*Width: 220px*

**Round 260 × 260 — Vector Font (e.g., Forerunner 955)**

![Practice 260x260 Vector](screenshots/practice_260x260_vector.png)

*Width: 220px*

**Round 260 × 260 — Non-Vector Font / 7-Segment (e.g., Fenix 6)**

![Practice 260x260 7-Segment](screenshots/practice_260x260_7seg.png)

*Width: 220px*

**Round 240 × 240**

![Practice 240x240](screenshots/practice_240x240.png)

*Width: 220px*

**Semi-octagon 176 × 176**

![Practice 176x176](screenshots/practice_176x176.png)

*Width: 220px*

---

---

## 3. Markers Menu

The Markers menu replaces the older "Pins" menu (reworked in v5.4) and uses standard sailing/regatta terminology. It features a **smart GPS averaging algorithm** to defeat GPS drift (~3-meter dispersion).

### Menu Structure

| Item | Actions |
|---|---|
| **Set boat** | Set / Reset |
| **Set pin** | Set / Reset |
| **Set home** | Set |
| **List** | Review stored markers |

### Smart Averaging

- Under **Set boat** and **Set pin**, the app displays the number of recorded points.
- If a new point is within a **10-meter radius** of the previous one, coordinates are **averaged together** to achieve high pinpoint precision at low speeds.

### Set Home

- Saves the launch/beach location.
- When a Home point is set in the Markers menu, the **Practice View** displays a **direction arrow pointing to Home**.
- The **GPS Status view** shows the exact distance to Home.

### List & Reset

- **List** reviews stored marker data.

> **Note:** Pin locations are saved directly to the `.fit` file for further post-session analysis.

### Markers Menu Screenshots

<!-- Add your Markers menu screenshots here -->

![Markers Menu](screenshots/markers_menu.png)

*Width: 220px*

![Markers List](screenshots/markers_list.png)

*Width: 220px*

![Set Boat](screenshots/markers_set_boat.png)

*Width: 220px*

![Set Pin](screenshots/markers_set_pin.png)

*Width: 220px*

---

---

## 4. Setup Menu

The Setup menu allows full customization of units, display, timer behavior, and alert preferences.

| Setting | Options |
|---|---|
| **Speed Units** | km/h or knots |
| **Distance Units** | kilometers or sea miles |
| **Color Theme** | Black or White background |
| **Timer program** | Countdown duration from **2 to 12 minutes** |
| **Timer loop** | On / Off (looping countdown, useful for training sessions) |
| **Timer beeper** | On / Off (audio alerts for start timer) |
| **Timer vibrate** | On / Off (vibration alerts for start timer) |
| **Timer threshold speed** | Speed to switch from Timer → Practice |
| **Tracker threshold speed** | Speed to switch from Practice → Timer |
| **Tracker autostart** | On / Off (default: **Off**) — automatically switch to the Practice view when activity is detected |
| **Timer predict interval** | Seconds before start to show prediction info (**0 to 90**) |
| **Jybe seconds** | Time allocated for performing a jybe/tack maneuver (used for start hints) |
| **Start zone** | Distance (in seconds) from the start line to sail on a straight course |
| **Save doppler speed** | On / Off — save raw Doppler speed data to the `.fit` file for post-session analysis |

### Setup Menu Screenshots

<!-- Add your Setup menu screenshots here -->

![Setup Menu](screenshots/setup_menu.png)

*Width: 220px*

![Setup Units](screenshots/setup_units.png)

*Width: 220px*

![Setup Timer](screenshots/setup_timer.png)

*Width: 220px*

---

## 5. GPS Status

Displays current GPS quality and accuracy along with additional telemetry.

### Displays

- GPS quality
- Precision accuracy
- Speed
- Heading
- Altitude
- **Distance to Home** (if a Home point has been set in the Markers menu)

### Compass Fallback (v3.4)

- Uses the **magnetic compass** if stationary or if GPS signal is temporarily lost.
- Heading is normalized to the **0–359 range** (previously 1–360).

### GPS Status Screenshots

<!-- Add your GPS Status screenshots here -->

![GPS Status](screenshots/gps_status.png)

*Width: 220px*

![GPS Status Compass](screenshots/gps_status_compass.png)

*Width: 220px*

---

## 6. Gear Menu

The Gear menu lets you record equipment used and environmental conditions for each session.

### Equipment DB

- Select **Board**, **Sail**, **Fin**, **Wing Board**, **Wing Foil**, or **Wind Foil**.
- Stored directly in the **FIT activity summary** for review.
- Database was generated using AI and is certainly incomplete — please report missing records.

### Environment & Impressions

- **Windspeed** logging
- **Wind Match** — how well the conditions matched your setup
- **Ride Comfort** — subjective comfort rating

### Gear Menu Screenshots

<!-- Add your Gear menu screenshots here -->

![Gear Menu](screenshots/gear_menu.png)

*Width: 220px*

![Gear Board Selection](screenshots/gear_board.png)

*Width: 220px*

![Gear Impressions](screenshots/gear_impressions.png)

*Width: 220px*

---

## 7. Quit

Triggers a safe exit sequence.

### Exit Dialog

- **Save / Discard** activity dialog appears on exit.

### Anti-Accident Protection

- Pressing **Right Bottom (Back)** on this screen acts as an **Undo** command — immediately returning to the active session **without data loss**.
- This is useful if you fall off unintentionally and want to continue your training.

### Quit Screen Screenshots

<!-- Add your Quit screen screenshots here -->

![Quit Dialog](screenshots/quit_dialog.png)

*Width: 220px*

![Quit Undo](screenshots/quit_undo.png)

*Width: 220px*

---

## Session Summary

After the session ends, a summary is shown. The same summary can be accessed during the activity via the **Right Upper button** in the Practice view.

### Metrics Included

- Peak speed
- 2-second speed
- 100m best speed
- Distance
- Duration
- Max heart beat
- Calories burned

### Session Summary Screenshots

<!-- Add your Session Summary screenshots here -->

![Session Summary](screenshots/session_summary.png)

*Width: 220px*

![Session Summary Exit](screenshots/session_summary_exit.png)

*Width: 220px*

---

## Screenshots by Resolution

### Round 454 × 454

![Round 454x454 Timer](screenshots/454x454_timer.png)
![Round 454x454 Practice](screenshots/454x454_practice.png)

*Width: 220px*

### Round 416 × 416

![Round 416x416 Timer](screenshots/416x416_timer.png)
![Round 416x416 Practice](screenshots/416x416_practice.png)

*Width: 220px*

### Round 280 × 280

![Round 280x280 Timer](screenshots/280x280_timer.png)
![Round 280x280 Practice](screenshots/280x280_practice.png)

*Width: 220px*

### Round 260 × 260 — Vector Font (e.g., Forerunner 955)

![Round 260x260 Vector Timer](screenshots/260x260_vector_timer.png)
![Round 260x260 Vector Practice](screenshots/260x260_vector_practice.png)

*Width: 220px*

### Round 260 × 260 — Non-Vector Font / 7-Segment (e.g., Fenix 6)

![Round 260x260 7-Segment Timer](screenshots/260x260_7seg_timer.png)
![Round 260x260 7-Segment Practice](screenshots/260x260_7seg_practice.png)

*Width: 220px*

### Round 240 × 240

![Round 240x240 Timer](screenshots/240x240_timer.png)
![Round 240x240 Practice](screenshots/240x240_practice.png)

*Width: 220px*

### Semi-octagon 176 × 176

![Semi-octagon 176x176 Timer](screenshots/176x176_timer.png)
![Semi-octagon 176x176 Practice](screenshots/176x176_practice.png)

*Width: 220px*

---

## Changelog

### Version 5.4
- Added **7-segment display** for devices with no vector fonts (e.g., Fenix 6 timer view) — now shows numbers of arbitrary size.
- Reworked and simplified **Pins menu** (now named **Markers**), explicitly set **Boat** and **Pin** location.

### Version 5.3
- Added support for **round 416×416** devices.
- **96 models supported** from now on.

### Version 5.2
- **Pins locations** saved to `.fit` file for further analysis.

### Version 5.0
- Updated **gear database**.
- Added **wing boards**, **wing foils**, **wind foils** to the database.
- Better support for **Fenix 6** devices.

### Version 4.6
- Minor update for debug purposes.

### Version 4.3
- Added **jybe speed analysis** for the practice view.

### Version 4.2
- Timer crash fixed.

### Version 4.1
- Start session from **first GPS fix**.
- Do not ask for session save if no session.
- Improved tracker layout for **round 260×260** devices.
- Better **tack detection**, filter out at low speeds.

### Version 3.10
- Added **"Back" button** to the exit screen — if you fall off unintentionally, you can continue your training now.
- Rearranged elements on timer view for **round 260×260** screens to give more space for seconds.
- Increased seconds font on devices with vector font support and round 260×260 screens (Forerunner 955 and similar).

### Version 3.9
- Fixed **Fenix 6** support broken by utilizing vector fonts for bigger seconds in timer. Reverted to raster fonts for devices with no support for vector fonts.

### Version 3.8
- Increased Timer seconds font size by request for **round 454×454** devices.

### Version 3.7
- Added **beeper and vibration** for start timer inspired by **Optimum Time Series** watches.
- Added **Setup menu item** to turn on/off beeper and vibration.
- Increased Seconds font size in Timer view for **round 260×260** devices by request (Forerunner 955 and similar).

### Version 3.6
- Uploaded old image by error in previous release.
- Improved **GPS Status** sensor readings.

### Version 3.5
- Added **"Home" feature**. Now you can set home location in pins menu.
- Once home is set, practice view shows the direction to home with the arrow and GPS Status view shows distance to home.

### Version 3.4
- Use **compass heading** if no GPS or not moving in GPS Status.
- Fit heading to **0–359 range** (was 1–360).

### Version 3.3
- Improved **GPS Status** view — shows accuracy, speed, heading, and altitude.

### Version 3.2
- Show **session summary** on Enter key in tracker.
- Added **100m best speed** calculations.
- Minor code refactor.

### Version 3.1
- Added two options:
  - **Start Zone size** in seconds (set values lower than default 30 seconds)
  - **Jybe duration** in seconds (set your preferred value)
- Both options used to calculate hints for start procedure.

### Version 3.0
- Updated **gear database**.

### Version 2.10
- Increase prediction accuracy.

### Version 2.9
- Adding more than one pin can now increase pin coordinates accuracy as it averages the coordinates.

### Version 2.8
- Fixed broken (in 2.7) **start line detection**.

### Version 2.7
- Some code cleanups and improved **geo calculations** for higher latitudes.

### Version 2.6
- Improved **start line detection**.

### Version 2.4 / 2.3
- Minor bugfixes.

### Version 2.2
- Testing **jybe suggestion vibration**.

### Version 2.1
- Added two options to save your impressions of the session — **wind match** and **ride comfort**.

### Version 2.0
- Added **windspeed** to gear menu.

### Version 1.9
- Improved **gear data rendering** for Connect IQ app.

### Version 1.8
- Added **gear database** under Gear menu item. Specify your board, sail, and fin — that info is stored in activity summary for review.

### Version 1.7
- Added **session summary** exit screen.

### Version 1.6
- Fixed possible race condition causing Array out bounds error and app crash.

### Version 1.5
- Added **save/discard activity** exit dialog.
- Refactored drawing to reduce battery and CPU usage.
- Improved timer accuracy by utilizing **milliseconds** instead of seconds.

### Version 1.4
- Added support for **Fenix 6 family** devices and fixed GPS quality indication.

### Version 1.3
- Added support for **round 240, 260, 280 × 240, 260, 280** screens.

### Version 1.2
- Added support for **round 454×454** screens.
- Added modern API level support (**Fenix 8 AMOLED** and similar).

---

## Installation

1. Ensure your Garmin watch is compatible (5-button layout, one of the supported resolutions).
2. Download the app from the **Garmin Connect IQ Store**.
3. Install via **Garmin Express** or the **Connect IQ app** on your phone.
4. Launch the app from your watch's activity menu.
5. The app will automatically start recording upon the **first stable GPS fix**.

---

## License

This project is provided as-is for personal use. Please report any issues or missing gear database records via the project's issue tracker.

---

*Built for sailors, by sailors. Fair winds and fast tacks!* ⛵
