# Project Specification: AI Model Timeline Dashboard

## 1. Project Overview
The goal of this project is to build an interactive dashboard to visualize and analyze the release frequency of AI models over time. The application will scrape release data from `llmgateway.io/timeline` and present it through a clean, modern interface. The primary objective is to help users determine if the cadence of new model releases is accelerating, decelerating, or remaining stable.

## 2. Core Features

### 2.1 Automated Data Pipeline (Scraping)
* **Target**: `https://llmgateway.io/timeline`
* **Tooling**: Playwright (Python, Async)
* **Extraction Strategy**: The scraper will intercept the page and iterate through the year-selection buttons (`<button class="...">2026</button>`). It will detect the active year by the `bg-primary` class, click through historical years, wait for DOM updates, and extract the underlying model names and release dates.
* **Initial State Testing**: The system will first be built and tested against the provided local HTML dataset examples before running live network scrapes.

### 2.2 Interactive Dashboard & Trend Analysis
* **Dynamic Filters**: Year selection, model developer/creator, and parameter size (if available).
* **KPI Indicators**: High-level metrics showing Total Models Released, Average Models per Month, and Year-over-Year (YoY) Growth percentages.
* **Trend Analysis Visualizations**: 
    * Cumulative line charts tracking release velocity.
    * Bar charts showing monthly release frequency to easily spot peaks or lulls.
* **Data Tables**: A sortable, paginated table detailing the raw model release data.

### 2.3 2026 Dashboard Design Best Practices
Based on the latest 2026 UI/UX design trends for data visualization, the dashboard must adhere to the following principles:
* **Clarity-First & Low Cognitive Load**: Strip away unnecessary gridlines, legends, and visual noise. Charts should focus on a single, clear takeaway (e.g., "Release frequency doubled in Q2").
* **F-Pattern Visual Hierarchy**: Place critical KPIs above the fold (top left to right), trend comparisons in the middle, and granular drill-down tables at the bottom.
* **Accessibility by Default**: Ensure charts do not rely solely on color to convey meaning. Use patterns or distinct shapes. Implement ARIA labels, descriptive alt text, and minimum 16px typography. Ensure the layout is fully responsive for mobile users.
* **Data Honesty**: Never truncate axes to exaggerate trends. Explicitly state the data source (`llmgateway.io`), scraping frequency, and potential gaps in the dataset.

## 3. Technology Stack

* **Language**: Python 3.12+
* **Backend Framework**: **FastAPI**
* **Frontend Generation**: **FastUI** (A modern 2026 framework that allows defining declarative React frontend components entirely via Python FastAPI endpoints) or **FastAPI + Jinja2 + HTMX** depending on the agent's specific tooling preferences.
* **Scraping Engine**: `playwright` (async).
* **Data Processing**: `pandas` for date aggregations and rolling averages.
* **Visualization Engine**: `plotly` (Python) or `altair` for generating interactive HTML charts that seamlessly embed into the frontend.


## 4. Implementation Plan & Tasks

### Phase 1: Data Extraction Pipeline
- [ ] **Task 1.1**: Set up an async Playwright script to parse the initial local HTML files.
- [ ] **Task 1.2**: Implement regex/BeautifulSoup logic to extract Model Name and Exact Release Date from the parsed HTML.
- [ ] **Task 1.3**: Expand the script for live scraping: locate the `<div class="flex flex-wrap gap-2">` buttons, loop through each, trigger a `.click()`, and await the network/DOM state to settle before extracting.

### Phase 2: Backend API & Data Processing
- [ ] **Task 2.1**: Initialize the FastAPI application structure and CORS middleware.
- [ ] **Task 2.2**: Implement Pandas utility functions to convert raw dates into timeseries data, calculate monthly aggregates, and compute the trend slope (increasing/decreasing).
- [ ] **Task 2.3**: Build internal API endpoints to serve JSON metrics and Plotly Figure HTML.

### Phase 3: Frontend Generation
- [ ] **Task 3.1**: Configure FastUI routes (or Jinja2 templates) to serve the main layout.
- [ ] **Task 3.2**: Implement the F-Pattern UI: `FastUI.Row` for KPIs, `FastUI.Div` for the main Plotly chart, and `FastUI.Table` for the raw data.
- [ ] **Task 3.3**: Wire up the year-selection filter buttons in the UI to trigger backend state changes and re-render the charts.

