# Time Tally

A single-file web app for adding up two time spans. Pick a start and end time for each span and Time Tally shows the duration of each one plus a running total, in both `Xh YYm` and decimal hours — handy for filling out timesheets.

No build step, no dependencies, no server. Open `index.html` and it works.

## Features

- **Two time spans**, each with `From` / `To` dropdowns in 30-minute increments (00:00 – 23:30).
- **Live totals** — every change recalculates durations and the combined total.
- **Decimal hours** next to the `Hh MMm` total, e.g. `7h 30m` / `7.50 hours`.
- **Overnight handling** — if the end time is earlier than the start, the span wraps past midnight and is counted as overnight (with a note under the field).
- **Toggle Span 2 off** when you only need one span; it dims and is excluded from the total.
- **Remembers your choices** in `localStorage`, so the times you entered are still there on reload.
- **Dark mode** via `prefers-color-scheme`, plus a `data-theme="dark"` / `"light"` override on `<html>`.
- **Mobile friendly** — responsive single column, safe-area insets for notched devices, system font stacks for fast loads.

## Usage

Open the file directly in a browser:

```sh
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Or serve it locally if you prefer:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

Pick times in the dropdowns. The total updates immediately. Uncheck **Span 2** to count only the first span.

## How it works

Everything lives in `index.html` — markup, styles, and a small vanilla JS block at the bottom.

- Time options are generated on load: 48 values at `STEP = 30` minutes, stored as minutes since midnight.
- `span(a, b, n)` computes `end − start`, rolling over `+1440` minutes when negative so overnight spans count forward.
- `calc()` re-renders each duration and the total; the total is `span 1 + (span 2 if enabled)`.
- Settings persist under the `tt-<id>` keys (`tt-a1`, `tt-b1`, `tt-on2`, …). Invalid stored values are ignored, and times not aligned to 30 minutes are discarded.

### Changing the time step

Edit one constant near the top of the script:

```js
const STEP = 30; // minutes — use 15, 60, etc.
```

## Limitations

- Times are limited to 30-minute granularity (configurable above).
- Only two spans are supported; it isn't a full timesheet with multiple entries or dates.
- Durations are unsigned — it measures elapsed clock time, not a signed difference.
- Spans longer than 24 hours can't be expressed since only time-of-day is captured.

## License

MIT
