# Scope Statement — Geospatial Analysis for an Export-Oriented Bakery Chain

**Written before analysis began, per Week 4 requirements.**

## Business Context
The client is an **Export-Oriented Bakery Chain** based in Karachi, Pakistan. It exports baked goods (biscuits, rusks, bakery products) internationally, and separately runs a local distribution network of 5 branches/warehouses that serve nearby customers and retailers domestically. The export side of the business depends on this local network operating efficiently as the first link in the supply chain.

## Problem
Management does not currently know whether its 5 existing branches provide good geographic coverage of local customer demand, or whether large pockets of customers are consistently underserved (too far from any branch) — leading to longer delivery times, higher logistics cost, and lost local sales opportunities.

## Objective
1. Map the geographic distribution of customer/delivery locations.
2. Measure how far each customer is from the nearest existing branch.
3. Identify natural customer clusters, and flag which clusters are underserved (average distance to nearest branch > 5 km).
4. Recommend the 3 best new locations for additional branches/drop-points, ranked by expected impact (customers served × distance currently traveled).

## What's In Scope
- A geocoded dataset of 8,000+ customer/delivery points (synthetic, since no real customer address list was available — see Limitations in the final report).
- Data validation: checking for and fixing swapped coordinates, points in the sea, and impossible/missing coordinates.
- Distance-to-nearest-branch calculation (haversine/great-circle distance).
- DBSCAN clustering of customer locations.
- A scoring method to rank underserved clusters as candidate new-branch locations.
- An interactive Folium map with branches, customer density, clusters, and the top 3 recommendations.
- Full reproducibility: pinned package versions and a fixed random seed.

## What's Out of Scope
- Real customer address data (would require the client's actual CRM/delivery records).
- Real road-network travel time/distance (this analysis uses straight-line/haversine distance as an approximation).
- Rent, real-estate availability, or zoning constraints for the recommended locations (purely a coverage/demand-distance analysis).
- Cost-benefit/ROI modeling for opening a new branch (recommendations are ranked by coverage gap, not by financial return).

## Success Criteria
- At least 8,000 valid geocoded points in the final clean dataset.
- Every injected data-quality problem (swapped coordinates, sea points, impossible coordinates) is detected and either fixed or removed, with counts documented.
- A working interactive map and a ranked list of exactly 3 recommended locations with supporting numbers (customers served, current average distance).
- A classmate running the notebook with the pinned `requirements.txt` and the fixed seed gets identical numbers.
