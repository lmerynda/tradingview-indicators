# ORB Indicator Spec (Current Implementation)

Date: 2025-12-29

This document captures the *current* requirements/behavior implemented by the Pine Script indicator in [indicators/ORB.pine](ORB.pine).

## 1. Purpose
- The indicator MUST plot Opening Range (OR) levels for:
  - The first **30 seconds** of a session ("30s OR").
  - The first **30 minutes** of a session ("30m OR").
- The indicator MUST be a standalone overlay indicator.

## 2. Sessions
### 2.1 Session windows
- The indicator MUST support three configurable session windows via `input.session()`:
  - **Asia Session** (default: `1700-0159`)
  - **Europe Session** (default: `0200-0829`)
  - **US Session** (default: `0830-1559`)
- The session windows MUST be interpreted in a single "session timezone" (`sessTz`).

### 2.2 Session timezone
- The indicator MUST default to using the instrument’s exchange timezone: `syminfo.timezone`.
- The indicator MUST provide a boolean input "Use New York time (ET) for sessions".
  - If enabled, the session timezone MUST be `America/New_York`.

### 2.3 Session start detection
- The indicator MUST detect a session start on the first bar that enters each configured session window.
- A new session MUST be considered started when **any** of the three sessions starts.
- On session start, the indicator MUST set:
  - `activeSessionStart` = the bar’s `time` timestamp (in milliseconds).
  - `activeSessionName` = one of: `Asia`, `Europe`, `US` (based on which session start fired).

## 3. Lifecycle: “Current session only”
- The indicator MUST display OR levels for the **current** active session only.
- When a new session starts, the indicator MUST:
  - Reset stored OR highs/lows (`or30s_high`, `or30s_low`, `or30m_high`, `or30m_low`) to `na`.
  - Delete any previously drawn OR lines and labels for both 30s and 30m.
  - Ensure the prior session’s OR levels disappear immediately on the first bar of the new session.

## 4. OR timing
- The indicator MUST define OR windows relative to `activeSessionStart`:
  - `or30s_end = activeSessionStart + 30 * 1000` (30 seconds)
  - `or30m_end = activeSessionStart + 30 * 60 * 1000` (30 minutes)

## 5. OR calculation
### 5.1 Inputs controlling calculation
- The indicator MUST provide toggles:
  - "Show 30s Opening Range"
  - "Show 30min Opening Range"
- The indicator MUST provide calculation mode toggles:
  - "Use 30s security fallback (for coarse charts)"
  - "Use 30min security fallback (for coarse charts)"

### 5.2 Calculation rules (30s)
When "Show 30s Opening Range" is enabled:
- If security fallback is enabled, the indicator MUST compute OR highs/lows from `request.security(..., "30S", [high, low, time])`.
  - It MUST only include 30S bars whose security-time `t30s_s` is within `[activeSessionStart, or30s_end]`.
- If security fallback is disabled, the indicator MUST compute OR highs/lows from chart bars.
  - It MUST only include chart bars whose `time` is within `[activeSessionStart, or30s_end]`.

### 5.3 Calculation rules (30m)
When "Show 30min Opening Range" is enabled:
- If security fallback is enabled, the indicator MUST compute OR highs/lows from `request.security(..., "30", [high, low, time])`.
  - It MUST only include 30m bars whose security-time `t30m_s` is within `[activeSessionStart, or30m_end]`.
- If security fallback is disabled, the indicator MUST compute OR highs/lows from chart bars.
  - It MUST only include chart bars whose `time` is within `[activeSessionStart, or30m_end]`.

## 6. Drawing & visuals
### 6.1 Line rendering
- After an OR window has completed, the indicator MUST draw two horizontal lines:
  - High line at the OR high
  - Low line at the OR low
- Lines MUST:
  - Start at `x1 = activeSessionStart` using `xloc.bar_time`.
  - Extend to the right indefinitely using `extend.right`.
  - Use a configurable color input "OR Line Color" (default: `rgb(255,215,0)`).
  - Use line width `2`.

### 6.2 Label rendering
- When the OR lines are created, the indicator MUST create two right-aligned labels:
  - "30s H: <price>" and "30s L: <price>" for the 30s OR.
  - "30m H: <price>" and "30m L: <price>" for the 30m OR.
- Labels MUST:
  - Use `label.style_label_right`.
  - Use background color input "OR Label Background Color".
  - Use text color input "OR Label Text Color".
  - Use a configurable size input with options: `tiny`, `small`, `normal`, `large`.

### 6.3 Label pinning behavior
- Once labels exist, the indicator MUST keep them pinned to the latest bar by updating:
  - `label.set_x(..., bar_index)` on every bar.
  - `label.set_y(..., OR level)` on every bar.

## 7. Logging
- The indicator MUST emit Pine Logs events:
  - On session start: "ORB session start: name=..., start=..., tz=..."
  - On 30s OR creation: "ORB event: 30s OR created: session=..., start=..., H=..., L=..."
  - On 30m OR creation: "ORB event: 30m OR created: session=..., start=..., H=..., L=..."

## 8. Non-goals (not implemented)
- The indicator does NOT retain or display previous sessions’ OR levels.
- The indicator does NOT draw boxes/fills for the OR; it draws lines + labels only.
- The indicator does NOT expose per-session history controls (e.g., keep N sessions).
- The indicator does NOT provide a manual date/session selector.

## 9. Known limitations / edge cases
- If the chart has no bars that enter a configured session window (e.g., limited data/replay range), `activeSessionStart` will remain `na`, and OR levels will not be computed/drawn.
- If the chart timeframe is coarser than the OR period and security fallback is disabled, the OR will be approximated using the available chart bars.
- The session windows are user-configurable; incorrect windows/timezone selection will shift the session start anchor and therefore the OR.
