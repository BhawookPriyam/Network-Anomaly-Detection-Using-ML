# Data

This project uses the **CIC-IDS2017** network traffic dataset. The full CSV
is not committed to this repository because of its size.

## Setup

1. Download a CIC-IDS2017 CSV (or any CICFlowMeter-style flow export) from:
   https://www.unb.ca/cic/datasets/ids-2017.html
2. Place it in this folder as `network_traffic.csv`, so the path is:
   `Data/network_traffic.csv`
3. Run the notebook from the repo root — it also checks a few other common
   locations (repo root, `/content/Data/`) automatically.

## Scoring data from another source

To test the trained model on traffic from a different source/export, drop
that CSV anywhere and pass its path to `score_new_traffic()` in the last
cell of the notebook — it does not need to match this exact folder or
column naming; see the notebook's feature-engineering section for the list
of column aliases it recognizes.
