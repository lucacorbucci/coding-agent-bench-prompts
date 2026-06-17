# Project Specification: BenzoApp

## 1. Project Overview
The goal of this project is to build a web application that helps users find the cheapest petrol stations (Benzinai) in Italy. The application will feature an interactive map using OpenStreetMap and will dynamically update a list of the best-priced stations in the user's current map view. Users can also localize their position to sort nearby stations by distance or by price.

## 2. Core Features

### 2.1 Interactive Map (Italy Focus)
* **Provider**: OpenStreetMap (via Leaflet or React-Leaflet).
* **Behavior**: Users can pan and zoom across the map of Italy.
* **Markers**: Petrol stations will be plotted on the map. Clicking a marker will show a popup with the station's details (name, brand, prices, last updated time).

### 2.2 Dynamic "Best Prices" Sidebar
* **Location**: A dedicated box/sidebar on the right side of the screen.
* **Functionality**: As the user moves the map, the application calculates the bounding box of the visible area, fetches the stations within that area, and updates the sidebar to show the stations offering the best prices.

### 2.3 User Geolocation & Sorting
* **Localization**: A "Find My Location" button that uses the browser's Geolocation API to center the map on the user.
* **Sorting Capabilities**: 
    * **By Price**: Show the absolute cheapest options in the selected area.
    * **By Distance**: Calculate the straight-line distance (or routing distance) from the user's localized position to the stations and rank them from closest to furthest.

### 2.4 Data Source
* **API**: [Prezzi Carburante API](https://github.com/dstmrk/prezzi-carburante)
* **Scope**: Only Italian petrol stations are targeted.

---

## 3. Technology Stack

### 3.1 Backend
* **Language**: Python 3.10+
* **Framework**: FastAPI
* **Responsibilities**:
    * Serve as a proxy/caching layer for the external `prezzi-carburante` API to prevent rate-limiting and improve response times.
    * Filter and process data (e.g., extracting only stations within a specific lat/lon bounding box).
    * Handle distance calculations (using libraries like `geopy` or Haversine formula) if not handled by the frontend.

### 3.2 Frontend
* **Library**: React.js
* **CSS Framework**: Bulma CSS (for clean, responsive flexbox-based layouts, cards, and buttons).
* **Map Integration**: `react-leaflet` (wrapping OpenStreetMap).
* **State Management**: React Hooks.

---

## 4. Architecture & Data Flow

1.  **Initialization**: The React frontend loads and renders the OpenStreetMap centered on a default location in Italy (e.g., Rome).
2.  **Location Request**: If the user grants location permissions, the map centers on their coordinates.
3.  **Data Fetching**: 
    * The map triggers an event capturing its current bounding box (North-East and South-West coordinates).
    * The React frontend sends a request to the FastAPI backend: `GET /api/stations?bbox=...`
4.  **Backend Processing**:
    * FastAPI queries the `prezzi-carburante` dataset.
    * Filters the stations geographically.
    * Returns a clean JSON payload.
5.  **UI Update**: 
    * React plots the markers on the map.
    * The right sidebar updates to display a list of these stations, applying the active Bulma CSS classes for styling (e.g., `panel`, `card`, `tag is-success` for the lowest price).
    * If the user has selected "Sort by Distance", the frontend calculates the distance relative to the user's lat/lon before rendering the sidebar list.

---

## 5. Proposed UI Layout

* **Navbar (Top)**: Logo, Search Bar (for cities), and a "Localize Me" icon button.
* **Main Container (Columns)**:
    * **Left Column (70-80% width)**: The OpenStreetMap canvas.
    * **Right Column (20-30% width)**: 
        * A Bulma `panel` or `menu` component.
        * **Tabs/Buttons**: "Sort by Price" | "Sort by Distance".
        * **List Items**: Bulma `box` elements representing each station with the Brand Logo, Address, and highlighted Price tag.
