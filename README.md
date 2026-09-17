# Karnataka Road Safety Observatory

An interactive Streamlit dashboard analyzing 300,000+ Karnataka Police road
accident reports (2016–2023) — built for hackathon submission.

## Stack
`streamlit` · `pandas` · `numpy` · `matplotlib.pyplot` · `pydeck` (bundled with Streamlit, used for the geotagged map layer)

## Run it

```bash
pip install -r requirements.txt
streamlit run app.py
```

The app expects the dataset at `data/AccidentReports.csv` (already included).
If you swap in a different export, keep the same column names or update the
`COLS` list near the top of `app.py`.

## What's inside

- **Overview** — KPI cards, severity mix donut, accidents-vs-fatalities-by-year
- **Trends** — severity mix by year (stacked), hit-and-run rate over time, vehicles-per-accident distribution
- **Districts & Map** — top districts by volume, fatality rate by district, geotagged accident map colored by severity (pydeck)
- **Road & Environment** — road type, weather, surface condition, road character, urban/rural split
- **Causes & Collisions** — primary cause, hit-and-run split, collision type, a cause × severity heatmap
- **Data Explorer** — searchable, filterable table with CSV export

All charts respond live to the sidebar filters (year range, district, severity, main cause).

## Data notes

- Source data ships with a meaningful amount of free-text data-entry noise in
  categorical columns (e.g. a name or religion typed into a dropdown field).
  The loader buckets any category value occurring fewer than 40 times into
  `"Other / Unrecorded"` (or `"Unknown"` for Severity/Cause/Hit & Run) rather
  than dropping rows, so totals still reconcile.
- Only ~32% of records carry usable GPS coordinates; the rest are `(0, 0)`
  placeholders from the source system. The map filters to Karnataka's
  bounding box and only plots valid points.
- Data is cached with `st.cache_data`, so the ~150MB source CSV is parsed once
  per session, not on every filter interaction.
