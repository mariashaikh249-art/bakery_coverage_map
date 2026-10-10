# Geospatial Analysis: Location Clustering & Service-Coverage Maps — Export-Oriented Bakery Chain

**SDC Internship — Week 4 (Specialist Advanced Build) | Group 17 (Data Science)**
**Role:** Member 2 — Individual Contributor
**Primary tools:** GeoPandas + Folium, with scikit-learn (DBSCAN, BallTree) and pandas/NumPy

## What I Built and Why
This project maps and analyzes customer/delivery locations for an **Export-Oriented Bakery Chain** in Karachi to find where local demand is geographically clustered, which areas are underserved by the existing 5-branch network, and where the business should add new branches. This is a specialist-level geospatial skill (clustering, distance-to-facility analysis, interactive mapping) that is completely different in tool, topic, and method from Week 3's survey/hypothesis-testing work — no code, data, or topic is reused. See `scope_statement.md` for the full scope statement, written before any analysis began.

## Approach
1. **Generate data:** Simulated 9,250 realistic customer/delivery points across 15 Karachi neighborhoods (no real customer list was available), then deliberately injected realistic data-quality problems — swapped lat/lon, points in the sea, impossible coordinates — to give the validation step something real to catch.
2. **Validate & clean:** Checked for missing values, physically impossible coordinates, swapped lat/lon (detected via Karachi's bounding box), and sea points (simplified coastline rule). Every fix/drop is counted in `validation_log.txt`.
3. **Measure coverage:** Computed each customer's haversine distance to the nearest of 5 existing branches using a BallTree for fast lookup.
4. **Cluster:** Ran DBSCAN (on a locally-projected meters grid) to find 6 natural customer clusters.
5. **Score & rank:** Flagged clusters averaging >5 km from the nearest branch as underserved, scored them by `customer count × avg distance`, and ranked the top 3.
6. **Map it:** Built an interactive Folium map with 4 toggleable layers (customer density heatmap, existing branches, clusters colored by served/underserved, top-3 recommendations).

## Tools and Techniques
- **NumPy/pandas** — data simulation, cleaning, aggregation
- **scikit-learn** — `DBSCAN` for clustering, `BallTree` for fast haversine nearest-neighbor search
- **Folium** — interactive HTML map with layer controls, heatmap, markers, and popups
- **GeoPandas** — geospatial foundation for this track (coordinate handling throughout)
- **Jupyter Notebook** — full reproducible workflow

## Files
| File | Description |
|---|---|
| `Week4_Geospatial_Bakery_Maria.ipynb` | Full notebook: data generation, validation, distance calc, clustering, scoring, map |
| `scope_statement.md` | Scope statement written before analysis began |
| `requirements.txt` | Pinned package versions for reproducibility |
| `bakery_coverage_map.html` | Standalone interactive map (open in any browser) |
| `bakery_customers_raw.csv` | Simulated raw dataset including injected errors |
| `bakery_customers_clean.csv` | Cleaned dataset (9,244 points) after validation |
| `bakery_customers_analyzed.csv` | Clean dataset with distance-to-branch and cluster labels added |
| `existing_branches.csv` | The 5 existing bakery branches/warehouses |
| `cluster_summary.csv` | All 6 clusters with size, centroid, and average distance to branch |
| `top3_recommended_locations.csv` | The 3 ranked recommended new-branch locations |
| `validation_log.txt` | Line-by-line record of every data-quality issue found and fixed |
| `CHANGELOG.md` | What was tried, what didn't work, and what changed |

## What Was Tested and What Was Found
- **8,000+ point requirement:** met — final clean dataset has 9,244 points (started at 9,290 raw, including injected errors).
- **Coordinate validation:** 55/55 deliberately swapped lat/lon pairs were correctly detected and fixed; 30 deliberately placed sea points were all removed (13 via the sea rule directly, the rest via the bounding-box check); 5 impossible-range coordinates and 2 missing-value rows were correctly dropped. Full counts in `validation_log.txt`.
- **Coverage gap:** 24.7% of customers live more than 5 km from the nearest existing branch.
- **Clusters:** DBSCAN found 6 natural customer clusters; 3 of them (Saddar, Baldia Town, Gadap Town) are underserved by the >5 km threshold.
- **Top recommendation:** Saddar area — ~972 customers, averaging 6.3 km from the nearest branch, the highest-priority gap by the demand × distance score.

## Observations
- Coverage gaps aren't random — they cluster in specific, identifiable neighborhoods rather than being evenly spread across the city.
- The existing 5 branches sit mostly on Karachi's periphery, leaving a central pocket (Saddar area) surprisingly underserved despite being dense and close to the city center.
- Smaller, more distant clusters (Gadap Town) have the worst average distance per customer but the smallest population — worth a smaller satellite drop-point rather than a full branch.

<!-- Add your observations below -->


## Recommendations
1. **Open a new branch in the Saddar area first** — highest-priority gap: largest underserved population (~972 customers) combined with a clear 6.3 km average distance from any existing branch.
2. **Add a branch or larger drop-point in Baldia Town** as phase 2 — smaller population (~289 customers) but a meaningful distance reduction.
3. **Add a smaller satellite delivery point in Gadap Town** rather than a full branch — only ~124 customers, but they currently average 13.4 km from any branch, the worst in the network.

## Limitations
- All customer coordinates are **synthetic**, not real geocoded customer/delivery records — a real deployment needs the client's actual address data.
- The sea/coastline check is a simplified rectangular approximation, not a real coastline polygon.
- Distances are straight-line (haversine), not real road-network travel distance or time.

## What I'd Improve With More Time
Replace the simplified coastline rule with a real coastline polygon via GeoPandas, use real road-network travel time (e.g., via a routing API or OSRM) instead of straight-line distance for scoring, and validate recommended locations against actual population density or retail footfall data.

## How to Reproduce
1. `pip install -r requirements.txt` (pinned versions — a classmate using these exact versions and the fixed seed gets identical numbers).
2. Open `Week4_Geospatial_Bakery_Maria.ipynb` in Jupyter and run all cells top to bottom.
3. This regenerates every CSV file and `bakery_coverage_map.html` from scratch. The random seed (`SEED = 42`) is set once at the top of the notebook.

## AI Assistance Disclosure
Used Claude to help structure the geospatial pipeline (validation logic, DBSCAN parameter choices, Folium layer design) as permitted by the internship's AI-assistance policy; reviewed the logic, verified the numbers independently, and understood the reasoning behind each step before submitting.
