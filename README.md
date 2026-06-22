# Optimizing London’s Public Transit with MetroMove Data

> **From raw trip logs to smarter routes, fairer fares, and smoother rides across London.**

This repository showcases an end‑to‑end exploratory data analysis (EDA) and insights project on **MetroMove** – a fictional but realistic dataset capturing public transit trips across London.

The goal: use data from thousands of trips to identify **inefficiencies, demand patterns, and revenue opportunities** across London’s Tube, Bus, and Overground services.

Project Objectives: this project demonstrates how I use **Python, EDA, and data storytelling** to translate noisy transport data into **operational and customer‑focused recommendations**.

---

## 🧭 Project at a Glance

**Title**: Optimizing London’s Public Transit: Insights from MetroMove Data  
**Focus**: Operational efficiency, passenger behaviour, and fare optimisation  
**Key Questions**:

1. Where are **journey times and congestion** causing inefficiencies?
2. How do **passenger behaviour and demand patterns** vary by time, station, and transport mode?
3. Are **fares aligned** with trip duration and value across services?
4. What **data‑driven actions** can improve both **efficiency** and **passenger satisfaction**?

---

## 📦 Dataset & Features

The MetroMove dataset contains trip‑level records with fields such as:

- `Trip_ID` – unique identifier for each journey
- `Mode_of_Transport` – Tube, Bus, Overground, etc.
- `Departure_Station`, `Arrival_Station` – origin and destination
- `Trip_Duration` – journey time (minutes)
- `Passenger_Count` – number of passengers (group size)
- `Trip_Date` / `Month` / `Quarter` – for temporal trend analysis
- `Fare_Amount` – revenue per trip

These variables allow us to analyse **trip performance, passenger load, station popularity, seasonality, and fares**.

---

## 🕵️‍♀️ Exploratory Analysis (EDA)

### 1. Trip Performance: Uncovering Inefficiencies

_Subtitle: Where the network slows down_

Using EDA, I analysed:

- **Average trip duration** by route and transport mode
- **Most congested routes** (long durations / high variability)
- **Trip frequency by day and hour** to identify peak vs. off‑peak demand

This reveals where London’s network experiences **bottlenecks, delays, and potential under‑ or over‑supply of capacity**.

**Example visual (for GitHub):**

```markdown
![Average Trip Duration by Route](reports/figures/avg_trip_duration_route.png)
*Average journey times across key routes highlight congestion hot spots.*
```

---

### 2. Passenger Behaviour: Understanding Riders

_Subtitle: Who travels when, where, and with whom?_

From the MetroMove records, I explored:

- **Passenger load analysis**  
  Typical group sizes and capacity utilisation per vehicle help avoid overcrowding and improve fleet deployment.

- **Popular departure & arrival stations**  
  Identifying **key hubs and corridors** informs targeted investments and improved connectivity.

- **Seasonal travel patterns**  
  Month‑ and quarter‑level trends show how demand shifts across the year (e.g., summer spikes, winter slowdowns).

**Example visuals:**

```markdown
![Passenger Count Distribution](reports/figures/passenger_count_distribution.png)
*Distribution of passenger group sizes reveals common use cases (solo commuters vs. small groups).*  

![Top Departure and Arrival Stations](reports/figures/top_stations.png)
*Top stations by trip count highlight London’s most critical interchange points.*
```

---

### 3. Fare Patterns: Revenue and Value

_Subtitle: Are riders getting fair value – and is the system sustainable?_

The fare analysis focuses on three angles:

- **Revenue by mode**: comparing financial performance across Tube, Bus, and Overground services
- **Fare vs. duration**: checking whether similar trip durations have **consistent pricing** across modes
- **Fare trends over time**: monitoring price stability and flagging anomalies by quarter

This helps assess **financial sustainability** while preserving **perceived fairness** for passengers.

**Example visuals:**

```markdown
![Revenue by Mode of Transport](reports/figures/revenue_by_mode.png)
*Tube generates the bulk of revenue, but buses and overground play a key role in network coverage.*

![Fare vs Trip Duration](reports/figures/fare_vs_duration.png)
*Relationship between trip length and price across modes – a check on pricing consistency and fairness.*
```

---

## 🧠 Analysis & Code Highlights

_Subtitle: Not just charts – a reproducible analysis pipeline_

The core analysis is implemented in Python using a clean, notebook‑driven workflow.

**Example: loading and preparing the MetroMove data**

```python
import pandas as pd

trips = pd.read_csv("data/metromove_trips.csv")

# Basic cleaning
trips = trips.dropna(subset=["Trip_ID", "Mode_of_Transport", "Departure_Station", "Arrival_Station"])
trips["Trip_Duration"] = trips["Trip_Duration"].clip(lower=0)

# Extract time features
trips["Trip_Date"] = pd.to_datetime(trips["Trip_Date"])
trips["DayOfWeek"] = trips["Trip_Date"].dt.day_name()
trips["Hour"] = trips["Trip_Date"].dt.hour
trips["Month"] = trips["Trip_Date"].dt.month
trips["Quarter"] = trips["Trip_Date"].dt.to_period("Q")
```

**Example: trip performance by route and mode**

```python
route_perf = (
    trips
    .groupby(["Mode_of_Transport", "Departure_Station", "Arrival_Station"], as_index=False)
    .agg(
        avg_duration=("Trip_Duration", "mean"),
        trip_count=("Trip_ID", "count")
    )
)

# Sort to find most congested routes
congested_routes = route_perf.sort_values("avg_duration", ascending=False).head(20)
print(congested_routes)
```

**Example: demand by time of day**

```python
hourly_demand = (
    trips
    .groupby(["Mode_of_Transport", "DayOfWeek", "Hour"], as_index=False)
    .agg(trip_count=("Trip_ID", "count"))
)
```

> In the actual notebooks, these tables are turned into **heatmaps, bar charts, and station‑level rankings** to guide operations decisions.

---

## 🧩 Repository Structure

_A clear structure makes it easy for collaborators and team members to navigate._

```text
.
├── data/
│   ├── metromove_trips_raw.csv
│   └── metromove_trips_clean.csv
├── notebooks/
│   ├── 01_eda_trip_performance.ipynb
│   ├── 02_eda_passenger_behaviour.ipynb
│   └── 03_eda_fares_revenue.ipynb
├── reports/
│   ├── Optimizing-Londons-Public-Transit-MetroMove.pdf
│   └── figures/
│       ├── avg_trip_duration_route.png
│       ├── passenger_count_distribution.png
│       ├── top_stations.png
│       ├── revenue_by_mode.png
│       └── fare_vs_duration.png
└── README.md
```

You can adapt this to match your actual filenames and folder names.

---

## 🧩 From Insights to Impact: Recommendations

_Subtitle: What a transport operator can *actually do* with this analysis_

1. **Re‑schedule high‑congestion routes**  
   Use average trip duration and variability by route and mode to:
   - Add **buffer time** to chronically delayed routes
   - Re‑sequence departures on overlapping corridors
   - Identify where **priority lanes** or signalling improvements would have the biggest impact

2. **Align capacity with demand**  
   
   - Deploy larger vehicles or increased frequency on **peak corridors** and time windows
   - Reduce under‑utilised services during low‑demand periods while maintaining coverage
   - Use passenger load distribution to prevent **overcrowding** and improve comfort

3. **Strengthen key hubs**  
   
   - Invest in **infrastructure and wayfinding** at the most popular departure and arrival stations
   - Improve interchange times between modes (e.g. Tube → Bus) where transfers are most common

4. **Refine fare structures**  
   
   - Align fares more closely with **trip duration and distance** to maintain perceived fairness
   - Review anomalies (e.g. very short but expensive trips or long cheap journeys) for potential fare leakage or poor user experience

5. **Seasonal and event planning**  
   
   - Use month/quarter trends to prepare **additional capacity** for known high‑demand periods
   - Adjust operations for **seasonal slowdowns**, balancing cost savings with service reliability

---

## 🧰 Tech Stack

- **Language**: Python
- **Data**: MetroMove trip‑level CSVs (London public transit use case)
- **EDA & Visualisation**: pandas, NumPy, Matplotlib, Seaborn (or Plotly)
- **Documentation & Reporting**: Jupyter Notebooks + PowerPoint/Slides deck

This project is intentionally **EDA‑focused**: rather than training a prediction model, it emphasises **diagnostics, visual storytelling, and actionable operations insights**.

---

## 🚀 How to Run the Project

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/metromove-london-transit-eda.git
   cd metromove-london-transit-eda
   ```

2. **Create and activate a virtual environment (optional)**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Windows: .venv\\Scripts\\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Explore the analysis**
   - Open the notebooks in the `notebooks/` folder.
   - Run cells to reproduce key charts and tables.

5. **Export visuals** (optional)
   - Save key plots into `reports/figures/` to power the visuals referenced in this README.

---

## 📬 Contact

If you’re interested in **urban mobility, data‑driven operations, or EDA‑based consulting work**, I’d be happy to connect.

- **Author**: Ugwunwa Stella N  
- **Focus**: Data Science · Analytics · Public Transport & Operations Use Cases

> **In Summary:** This project demonstrates my ability to analyse real‑world operational data, uncover patterns that matter, and communicate them in a structured, decision‑ready skills that transfer directly to analytics, data science, and operations roles to both transport and logistics company.
