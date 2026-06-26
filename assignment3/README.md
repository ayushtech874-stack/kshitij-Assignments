# Streak — Habit Tracker & Streak Dashboard

A React habit tracker built with Vite, React Router, and Context + localStorage
for persistence. All date math and streak calculations are hand-written —
no date libraries.

## Setup

```bash
npm install
npm run dev
```

Then open the printed local URL (usually http://localhost:5173).

## Build for production

```bash
npm run build
npm run preview
```

## Project structure

```
src/
  components/      Reusable UI pieces (HabitCard, CalendarHeatmap, MiniHeatmap, AddHabitModal, NavBar)
  pages/           Routed pages (Dashboard, HabitDetail, Insights)
  context/         HabitContext (data + localStorage sync), ThemeContext (dark mode)
  utils/
    dateUtils.js     Date key conversion, day math, month grid builder — all from scratch
    streakUtils.js   Current streak, longest streak, completion rate, freeze-aware logic
    storage.js       localStorage read/write
    id.js            Habit ID generator
```

## Data schema

Each habit is stored as:

```js
{
  id: "habit_...",
  name: "Read 20 pages",
  category: "Learning",
  color: "#c2541f",
  frequency: "daily" | "custom",
  customDays: [1, 3, 5],       // weekday numbers, only used if frequency === 'custom'
  freezeTokens: 3,
  freezesUsed: { "2026-06-18": true },   // date -> true
  checkIns: { "2026-06-20": true },      // date -> true
  createdAt: "2026-06-01"
}
```

All habits are stored under one localStorage key: `habit-tracker-data-v1`.
Theme preference is stored separately under `habit-tracker-theme`.

## Features

- Create habits with name, category, color, and daily/custom-day frequency
- Check in for today from the dashboard or detail page
- Full month calendar heatmap per habit — click a day to toggle check-in,
  right-click to spend/refund a freeze token
- Current streak + longest streak, calculated by walking backwards/forwards
  through check-in history (skipping non-due days for custom-frequency habits)
- Streak Freeze Tokens — each habit starts with 3; spending one on a missed
  due-day keeps the streak alive without counting as a real check-in
- Insights page: 7-day completion chart, streak leaderboard, category
  breakdown, freeze token summary, 30-day completion bars per habit
- Dark mode toggle (persisted), fully responsive layout
- React Router routes: `/`, `/habit/:habitId`, `/insights`
