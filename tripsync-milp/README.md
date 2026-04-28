# TripSync — Optimising Tourist Itineraries with MILP

> **NTU BC2411 Prescriptive Analytics with Generative AI** · AY2024/25 Semester 2  
> Team: Emmanuella Nicole, Felicia Nathania, Isabelle Lim, Nakeisha Leong, Eugene Koh

---

## Overview

TripSync is a prescriptive analytics solution that generates **personalised, optimised travel itineraries** using Mixed-Integer Linear Programming (MILP). It solves a key real-world problem: tourists waste time on inefficient routes and poor accommodation choices due to the limitations of existing planning tools.

**Problem:** How can we reduce tourism fatigue by recommending personalised travel plans that maximise satisfaction?

**Solution:** A two-part MILP model that simultaneously optimises:
1. Which attractions to visit and in what order (subject to time and operating hour constraints)
2. Which accommodation to book (based on user-ranked criteria)

---

## Live Demo

🗺️ **[View Interactive Map](outputs/tripsync_map.html)** — 60 attractions across 4 Dutch cities, with optimised Day 1–5 routes plotted from a real model run.

---

## Model Design

### Part 1 — Tourist Attraction Optimiser

**Objective:** Maximise total satisfaction score across all days

```
Max W = Σ(d∈D) Σ(i∈I) (X_i × Y_id)
```

**Key constraints:**
- Total visit + travel time ≤ daily time limit
- No repeated attraction visits across days
- Valid flow in/out of each visited attraction (no disconnected loops)
- Operating hours enforced per attraction per day
- Each day starts and ends at the Airbnb

**Results:** Achieved satisfaction scores of **165** (Amsterdam) and **240** (Utrecht) across 5-day itineraries, visiting only feasible, non-overlapping attractions in optimal sequence.

### Part 2 — Accommodation Recommender

**Objective:** Maximise criteria satisfaction score weighted by user-ranked importance

```
Max Σ(i=1 to N) [G_i × Σ(k=1 to 5) (L_k × S_k,i)]
```

**Criteria evaluated:** Capacity · Stay duration · Airbnb rating · Host rating · Distance to attractions

---

## Dataset

| File | Description |
|---|---|
| `data/final_attractions.csv` | 60 attractions across Amsterdam, Rotterdam, Utrecht, Haarlem — with coordinates, satisfaction scores, operating hours |
| `data/accommodation_data.csv` | 101 Airbnb listings with capacity, ratings, and coordinates |

Data was simulated based on real-world coordinates and publicly available attraction information.

---

## Results

### Optimised 5-Day Amsterdam Itinerary (Plan 1)
| Day | Attractions | Satisfaction Score |
|---|---|---|
| Day 1 | Anne Frank House → Moco Museum | 5 + 3 |
| Day 2 | A'DAM Lookout → Heineken Experience | 3 + 2 |
| Day 3 | Van Gogh Museum → Amsterdam Museum | 4 + 3 |
| Day 4 | Dam Square → Bloemenmarkt | 4 + 4 |
| Day 5 | Royal Palace → Rembrandt House Museum | 3 + 1 |

**Total satisfaction score: 165**

### Recommended Accommodation
`Large private room for rent Center / De Pijp` — fulfilled all 5 criteria (capacity, stay duration, Airbnb rating ≥ 7.0, host rating ≥ 8.0, distance ≤ 5km)

---

## Tech Stack

- **Optimisation:** Gurobi (MILP solver) via Python
- **Data processing:** Pandas, NumPy
- **Visualisation:** Folium (interactive maps), Matplotlib
- **App mockup:** Figma
- **Language:** Python, R

---

## Project Structure

```
tripsync-milp/
├── data/
│   ├── final_attractions.csv       # 60 attractions, 4 cities
│   └── accommodation_data.csv      # 101 Airbnb listings
├── notebooks/
│   ├── Tourist_Attraction.ipynb    # MILP model — attraction optimiser
│   └── Accommodation.ipynb         # MILP model — accommodation recommender
├── outputs/
│   └── tripsync_map.html           # Interactive map of all attractions + routes
└── README.md
```

---

## Key Learnings & Limitations

- Euclidean distance used for travel time estimation (future: Google Maps API integration)
- Fixed visit durations assumed (future: dynamic based on attraction type)
- Currently single-city per run (future: multi-city with route breakpoints)
- No budget constraints (future: user-defined budget preferences)

---

## How to Run

```bash
# Install dependencies
pip install gurobipy pandas numpy folium

# Run attraction optimiser
jupyter notebook notebooks/Tourist_Attraction.ipynb

# Run accommodation recommender  
jupyter notebook notebooks/Accommodation.ipynb
```

> Note: Gurobi requires a licence. Free academic licences available at [gurobi.com](https://www.gurobi.com/academia/academic-program-and-licenses/).
