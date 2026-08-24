# Classroom Dashboard Architecture

This document maps the structure of `index.html` to make future updates easier. The dashboard is intentionally kept as a single file to avoid setup complexity, but organized with clear section markers.

## Quick Navigation

### CSS Sections (in `<style>` tag, lines 40-827)

| Section | Lines | Purpose |
|---------|-------|---------|
| **Scrollbar & Global** | 40-45 | Hide scrollbars, set box-sizing |
| **Body & Wrapper** | 47-85 | Background, layout, main container |
| **Top Bar & Pills** | 120-175 | Navigation bar, class title, buttons |
| **Ticker** | 238-280 | Scrolling message/market ticker |
| **Slide Mode Responsive** | 330-395 | Compact mode when presenting or gaming |
| **Bento Grid Layout** | 420-430 | 4-card main dashboard grid |
| **Card & Sub-Card** | 440-520 | Main content cards (Agenda, Warmup, EQ, etc.) |
| **Timer Styling** | 645-700 | Countdown display and progress bars |
| **Focus/Zen Mode** | 720-835 | Distraction-free mode styling |
| **Game Hub** | 855-960 | Game cartridge menu and active game UI |
| **Jeopardy Board** | 980-1050 | 5x5 grid and card styling |
| **Slide Mode Board** | 1070-1090 | Full-screen presentation view |
| **Modals** | 1110-1180 | Overlay styling for all popups |
| **Leaderboard** | 1195-1290 | Points display and bar charts |
| **Tracker Rows** | 1310-1370 | Participation roster styling |
| **Group Cards** | 1390-1510 | Random groups overlay |
| **Wheel** | 1530-1580 | Spin wheel styling |
| **Alert Screens** | 1600-1670 | Tardy, Eyes Up, Exam overlays |

---

### HTML Sections (lines 830+)

| Section | Lines | Purpose |
|---------|-------|---------|
| **Modal Overlays** | 870-1320 | Game join screen, tracker, settings, timer, points, wheel, groups, alerts |
| **Main Wrapper** | 1330-1700 | Ticker, top bar, bento grid |
| **Focus Mode Board** | 1710-1780 | Zen task cards, noise widget, timer |
| **Game Mode Board** | 1790-1950 | Hub menu, Jeopardy, Hot Seat, Titan Says, Hivemind, Debate |
| **Slide Mode Board** | 1960-1970 | Iframe container for presentations |

---

### JavaScript Sections (lines 1975+)

| Section | Start | End | Purpose |
|---------|-------|-----|---------|
| **Modal & Click Handlers** | 1975 | 2030 | Click-outside-to-close, modal toggle |
| **Web Audio (Chimes)** | 2050 | 2130 | Timer warning & completion sounds |
| **Dashboard State** | 2150 | 2250 | Global variables (focus mode, slide mode, game stage, etc.) |
| **Market Ticker** | 2270 | 2360 | Stock data, daily feed, ticker animation |
| **Settings & CSV URLs** | 2380 | 2450 | Sheet URL management, localStorage |
| **Backup & Restore** | 2470 | 2580 | Download/restore backup files |
| **CSV Parsing** | 2600 | 2750 | PapaParse integration, column header lookup, block parsing |
| **Game Vault Parser** | 2770 | 2820 | Jeopardy, Hot Seat, Titan Says, etc. from CSV |
| **Tracker & Absence** | 2840 | 3050 | Render roster, log participation, toggle absent |
| **Points Undo Stack** | 3070 | 3150 | Shared undo history for points changes |
| **Points & Leaderboard** | 3170 | 3320 | Class-wide points, leaderboard rendering |
| **Mode Toggles (Focus/Slide/Game)** | 3340 | 3450 | Switch between dashboard views |
| **Slide Mode Logic** | 3470 | 3580 | Render iframe, slide navigation |
| **Game Hub Menu** | 3600 | 3750 | Build cartridge menu, handle Jeopardy pack selection |
| **Game Module Opener** | 3770 | 3850 | Open specific game (Jeopardy, Bidding War, etc.) |
| **Jeopardy Logic** | 3870 | 4050 | Board rendering, tile state, prompt display |
| **Mini-Game Config** | 4070 | 4230 | Shared engine for Hot Seat, Titan Says, Hivemind, Debate |
| **Mini-Game Prompt Loader** | 4250 | 4350 | Load prompts from Game Vault, filter by period |
| **Mini-Game Next/Reset** | 4370 | 4450 | Cycle through prompts, manage no-repeat state |
| **Board Display Updates** | 3450 | 3580 | Transition between main/focus/slide/game boards |
| **Noise Level Widget** | 3680 | 3750 | Cycle classroom noise levels (0=silent, 1=whisper, 2=group) |
| **Keyboard Shortcuts** | 3770 | 3920 | Shift+key bindings, Space/Arrow no-shift bindings |
| **Confetti** | 3940 | 3950 | Celebration effect (Shift+C) |
| **Game Mode Toggle** | 3970 | 4000 | Display game code from active block |
| **Group Generation** | 4020 | 4180 | Shuffle roster, create groups, drag-drop reordering |
| **Wheel Logic** | 4200 | 4520 | Draw wheel, spin, no-repeat tracking, mode toggle (students/groups) |
| **Text Auto-Fit** | 4540 | 4570 | Shrink card text to fit available space |
| **Dashboard Update Loop** | 4590 | 4800 | Load schedule, find active block, render agenda/warmup/etc. |
| **Checklist Toggle** | 4760 | 4790 | Mark agenda items complete |
| **Timer State** | 4820 | 5050 | Save/load timer from localStorage, run countdown, play sounds |
| **Timer Display & Controls** | 5070 | 5200 | Render time, add time, toggle/clear, update progress bar |
| **Initialization** | 5220 | 5240 | Fetch data, set intervals, load timer state |

---

## How to Use This Guide

**Scenario 1: "I want to change the color scheme"**
→ Look at **CSS > Colors & Theme** section (find the `#5A2B81` purple, `#111111` black, etc. and replace)

**Scenario 2: "I want to add a new keyboard shortcut"**
→ Go to **JS > Keyboard Shortcuts** section (add `if (event.key === 'X')`)

**Scenario 3: "I want to change how groups are generated"**
→ Find **JS > Group Generation** section (modify the shuffle or group size logic)

**Scenario 4: "The timer display is wrong"**
→ Check **JS > Timer Display & Controls** (look for `renderTime()` and `updateTimerDisplay()`)

**Scenario 5: "I want to add a new game mode"**
→ Look at **JS > Mini-Game Config** (copy the pattern from Jeopardy or Hot Seat)

---

## Key Files & URLs

- **Main Sheet (Agenda & Class Data):** Link from Settings dialog (Shift+O) → customSheetUrlInput
- **Game Vault Sheet (Games & Prompts):** Link from Settings dialog (Shift+O) → gameVaultUrlInput
- **Expected Agenda Sheet Columns:** Date, ID, Title, Start, End, Roster, EQ, Standards, Warmup, Agenda, Reminders, Message, Spotlight, GameCode, Resources
- **Expected Game Vault Columns:** ID, Type, Title, Pack, Category, Prompt, Answer, Value, Status

---

## LocalStorage Keys (for reference)

| Key Pattern | Purpose |
|-------------|---------|
| `titan_sheet_url` | Custom CSV URL for agenda data |
| `titan_game_url` | Custom CSV URL for game vault |
| `titan_last_good_csv` | Cached agenda CSV (fallback if fetch fails) |
| `titan_tracker_[classId]` | Participation scores: `{student: points, ...}` |
| `titan_absent_[classId]` | Absent roster: `[student1, student2, ...]` |
| `classPoints_[classId]` | Class-wide Titan Cup points |
| `checklist_[timestamp]_[classId]` | Agenda items marked complete |
| `titan_timer` | Timer state (endTime, totalMs, isRunning, etc.) |
| `titan_wheel_played_[mode]_[period]` | Wheel picks (no-repeat): `[name1, name2, ...]` |
| `titan_jeopardy_played_[packId]` | Jeopardy tiles played: `["cat_100", "cat_200", ...]` |
| `titan_[gametype]_played_[packId]` | Mini-game prompts played (Hot Seat, Titan Says, etc.) |

---

## Testing Checklist

Before making updates, verify:
- [ ] Agenda loads from Google Sheets
- [ ] Game Vault loads (if URL is set)
- [ ] Timer starts/pauses/clears
- [ ] Participation tracking works
- [ ] Focus mode toggle works
- [ ] Slide mode loads presentations
- [ ] Game hub builds menu correctly
- [ ] Backup/restore works

---

## Common Pitfalls

1. **Changing a class name without updating all references** → Search the file for the class name in CSS and HTML before changing
2. **Editing the CSV column names** → Update both `processCSvData()` and `parseGameVault()` getCol() calls
3. **Adding a new game mode** → Need to update `MINI_GAME_CONFIGS`, `loadMiniGamePrompts()`, and the modal HTML
4. **Breaking localStorage keys** → Don't change key patterns without a migration strategy

---

**Last Updated:** 2026-08-24  
**Created for:** JMKaiser17/dashboard
