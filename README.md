# Agglomerator

A PyQt5 desktop widget that aggregates information from different sources into a single application. Built while studying at university to keep everything useful in one place — class schedule, weather, a calendar, and a SpaceX launch tracker.

The name comes from biology. To agglomerate is to collect things together into a mass.

The window is frameless, translucent, and draggable. It lives in the system tray and can be minimised down to just the tab bar. Tabs are loaded dynamically from the `tabs/` directory at startup, so new tabs can be added without touching the main window code.

Tab icons change colour to reflect status: **yellow** for a notice, **red** for an alert.

## Tabs

### Schedule

A weekly timetable spanning 6am–10pm in 15-minute segments.

- **Classes** are pulled from a PostgreSQL database and displayed as locked cells
- **Planner events** can be created by clicking and dragging on any time slot, then filling in the details and colour in the input popup
- Events support create, edit, and delete
- The tab icon turns yellow when a class is currently in progress
- Database queries run on a background thread; a spinner plays while updates are in flight

### Weather

Current conditions and a 5-day forecast for Auckland, hardcoded to Auckland's coordinates.

- Fetches from the [Dark Sky API](https://darksky.net) (now defunct — see note below)
- Shows current temperature, high/low, humidity, and a weather summary
- 5-day forecast with weather icons, wind speed, and a rotating wind direction arrow
- 12-hour hourly forecast strip along the bottom
- MetService button links to the Auckland forecast page
- Refreshes every 5 minutes

> **Note:** Dark Sky was acquired by Apple in 2020 and the API was shut down in 2023. The Weather tab will not work without swapping in a different API.

### SpaceX

A live countdown to the next SpaceX launch, plus an upcoming launch schedule.

- Scrapes the [r/SpaceX launch manifest](https://www.reddit.com/r/SpaceX/wiki/launches/manifest)
- Countdown updates every second (T- / T+)
- Shows the next 7 launches: date, vehicle, and payload
- Tab icon turns yellow within 1 hour of launch, red within 5 minutes
- "OPEN STREAM" button appears near launch time
- Refreshes the schedule every 24 hours

> **Note:** The Reddit wiki URL format may have changed since this was written.

### Calendar

A general calendar view.

## Architecture

```
main.pyw          Entry point, main window, tab loader, system tray
tabs/
  Calendar.py     Calendar tab
  Schedule.py     Weekly timetable with PostgreSQL backend
  Spacex.py       SpaceX launch tracker
  Weather.py      Auckland weather via Dark Sky API
threads.py        Background workers (DatabaseFetcher, WebFetcher)
modules.py        Shared widgets (GifPlayer, etc.)
settings.py       App-wide constants and configuration
icons/            Icons for tabs, weather conditions, and UI elements
```

## Requirements

```
PyQt5
requests
beautifulsoup4
psycopg2
```

## Running

```bash
python main.pyw
```

The Schedule tab requires a PostgreSQL database. Connection details and the schema are defined in `settings.py` and referenced in `tabs/Schedule.py`.

## License

GPLv3 — see [LICENSE](LICENSE).
