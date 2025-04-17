---
layout: default
title: "IS445 Week 10 Assignment"
permalink: /week10/
---

## Plot 1: Licenses by State and Type

<div id="vis1"></div>
<script src="https://cdn.jsdelivr.net/npm/vega@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@6"></script>
<script>
  vegaEmbed('#vis1','/assets/viz/chart1.html');
</script>

In this bar chart, I visualize how many professional licenses are issued in each U.S. state, broken down by license type. On the **x‑axis** I encode each **State** as a nominal category and explicitly sort the states in descending order of total licenses so that the largest bars appear first; on the **y‑axis** I encode the **count** of licenses issued. I chose a bar mark because it provides an immediate, intuitive comparison of discrete categories. I color each bar by **License Type** using a distinct categorical palette—this lets viewers see, for example, that “DETECTIVE BOARD” licenses dominate certain states, while other types predominate elsewhere. I also added a **tooltip** showing the exact state, license type, and count for any bar segment the user hovers over. In my Python notebook, I loaded the CSV directly from GitHub, parsed the date columns, dropped any records missing a state, and then used Pandas’ `groupby(["State","License Type"]).size()` to compute the tallies before handing the tidy DataFrame to Altair. This static plot provides a clear geographic overview without additional interactivity.

---

## Plot 2: Monthly License Issues by Type (Interactive)

<div id="vis2"></div>
<script>
  vegaEmbed('#vis2','/assets/viz/chart2.html');
</script>

In this time‑series chart, I plot the number of licenses issued each month, with a separate line for each license type. The **x‑axis** is the **Issue Month** (a true temporal scale, derived by converting the original issue dates to month‑level timestamps), and the **y‑axis** is the **count** of licenses issued that month. I use line marks with overlaid points to highlight individual months, and a **tooltip** to display the exact month, type, and count. To add interactivity beyond basic pan/zoom, I bound a **dropdown selector**—implemented via Vega‑Lite’s modern `param` API—to the **License Type** field. When the user selects a type, that line is rendered in full color while all others fade to light gray, making it easy to focus on one category’s trend over time. This dropdown binding is configured in Python by creating an Altair parameter (`alt.param`) with a `binding_select`, and then conditioning the `color` and `opacity` encodings on that parameter. All grouping and aggregation (month conversion and `groupby`) were done in Pandas before plotting.

---

## Extra Credit: Brush & Zoom Focus & Context

<div id="vis3"></div>
<script>
  vegaEmbed('#vis3','/assets/viz/chart3.html');
</script>

For extra credit, I implemented a **focus‑and‑context** view that lets users brush any time window and zoom the main plot accordingly. First, I aggregated total license counts per month in Pandas. The **context** view at the bottom is a light‑gray area chart with an **interval selection** on the x‑axis (`selection_interval(encodings=['x'])`), allowing users to drag and select a range. The **focus** view above it is a line chart of the same totals, but filtered (`transform_filter`) to show only the brushed range. Stacking these with a shared x‑scale (`resolve_scale(x='shared')`) gives a seamless drag‑to‑zoom experience: as you adjust the brush, the focus chart automatically updates to zoom in on the selected period. This advanced interaction—combining two coordinated views—is well beyond built‑in pan/zoom and provides an intuitive way to explore both big‑picture trends and specific intervals.

---

### Links

- **The Data**: [licenses_fall2022.csv](https://raw.githubusercontent.com/UIUC-iSchool-DataViz/is445_data/main/licenses_fall2022.csv)  
- **The Analysis**: [licenses_analysis.ipynb](https://github.com/sshetty6/sshetty6.github.io/blob/main/python_notebooks/licenses_analysis.ipynb)  
