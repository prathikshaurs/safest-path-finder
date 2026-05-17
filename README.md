# Safest Path Finder

**CS5800 Algorithms — Final Project (Spring 2023)**  
Aditya Singh · Rishab Jaiprakash Khuba · Prathiksha Mohan Raje Urs · Pranamya Dinesh

[Demo video](https://youtu.be/EDfeYrSToRk)

---

## Overview

Women, especially in urban areas, often face safety concerns while traveling alone. This project builds a path-finding tool that avoids high-crime areas by treating crime hotspots as blocked or heavily-weighted nodes in a geographic grid graph.

We compare two algorithms:

| Algorithm | Heuristic | Time Complexity |
|-----------|-----------|-----------------|
| **A\*** | Euclidean distance | O(E log V) |
| **Dijkstra's** | h(n) = 0 | O((E + V) log V) |

**Result:** A\* consistently outperformed Dijkstra's in speed (~0.052s vs ~0.065s on the same grid) while producing equally safe routes.

---

## How It Works

1. **Load crime data** — reads a shapefile of crime incident coordinates (Boston dataset)
2. **Build a grid** — divides the area into a 2D histogram grid where each cell's value = number of crimes in that cell
3. **Apply threshold** — cells above the N-th percentile are marked as blocked (weight = 1000, impassable); others are passable with weights 1 (cardinal moves) or 1.5 (diagonal moves)
4. **Run A\* or Dijkstra's** — finds the lowest-weight path from start to end, avoiding blocked cells
5. **Visualise** — renders the grid with colour-coded paths (green = optimal path, red = blocked)

---

## Repository Structure

```
safest-path-finder/
├── algo_project.ipynb   # Main Colab notebook (A* + Dijkstra's + visualisation)
├── requirements.txt
└── README.md
```

---

## Setup

This project was built in Google Colab. To run it:

1. Upload `algo_project.ipynb` to [Google Colab](https://colab.research.google.com/)
2. Upload your crime shapefile to Google Drive
3. Update the shapefile path in the notebook:
   ```python
   sf = shapefile.Reader("/content/drive/MyDrive/crime_dt.shp")
   ```
4. Run all cells
   
---

## Data

The notebook uses a Boston crime dataset in shapefile format (`crime_dt.shp`). The data file is **not included** in this repository due to size.

You can download Boston crime data from [Analyze Boston](https://data.boston.gov/dataset/crime-incident-reports-august-2015-to-date-source-new-system).

When using the shapefile, make sure all companion files are in the same folder: `.shp`, `.dbf`, `.shx`.

---

## Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `gridsize` | Size of each grid cell in degrees (~200m at 0.002) | `0.002`, `0.004` |
| `threshold` | Percentile above which a cell is blocked (1–100) | `65`, `75` |
| `x1, y1` | Start coordinate (longitude, latitude) | `-73.58583, 45.52807` |
| `x2, y2` | End coordinate (longitude, latitude) | `-73.55183, 45.49207` |

Higher threshold → fewer blocked cells → easier to find a path.  
Lower threshold → more blocked cells → algorithm may fail to find a path.

---

## References

1. [Isaac Computer Science — A\* Search](https://isaaccomputerscience.org/concepts/dsa_search_a_star)
2. [Amit Patel — Heuristics for A\*](http://theory.stanford.edu/~amitp/GameProgramming/Heuristics.html)
3. [Omdena — Safe path after earthquake with OSM](https://medium.com/omdena/devising-safe-path-to-travel-in-the-aftermath-of-an-earthquake-using-open-street-map-and-595005e8e6aa)
