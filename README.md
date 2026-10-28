import os, shutil, zipfile
from pathlib import Path

base = Path("/mnt/data")
project = base / "healthcare-business-performance-dashboard"
screenshots_dir = project / "screenshots"
screenshots_dir.mkdir(parents=True, exist_ok=True)

source_files = [
    base / "Screenshot 2026-10-28 095247.png",
    base / "Screenshot 2026-10-28 095316.png",
    base / "Screenshot 2026-10-28 095335.png",
    base / "Screenshot 2026-10-28 095355.png",
    base / "Screenshot 2026-10-28 095418.png",
]
names = [
    "01-business-performance-overview.png",
    "02-order-analysis.png",
    "03-customer-analysis.png",
    "04-revenue-analysis.png",
    "05-data-quality-mapping-validation.png",
]
copied = []
for src, name in zip(source_files, names):
    if src.exists():
        dst = screenshots_dir / name
        shutil.copy2(src, dst)
        copied.append(name)

readme = """# Healthcare Business Performance Dashboard

An interactive healthcare business performance dashboard designed to explore revenue, orders, customer behavior, and data quality through business-focused visualizations.

## Dashboard Preview

### 1. Business Performance Overview
![Business Performance Overview](screenshots/01-business-performance-overview.png)

### 2. Order Analysis
![Order Analysis](screenshots/02-order-analysis.png)

### 3. Customer Analysis
![Customer Analysis](screenshots/03-customer-analysis.png)

### 4. Revenue Analysis
![Revenue Analysis](screenshots/04-revenue-analysis.png)

### 5. Data Quality & Mapping Validation
![Data Quality and Mapping Validation](screenshots/05-data-quality-mapping-validation.png)

## Project Objectives

- Monitor overall business performance using key performance indicators (KPIs).
- Analyze revenue trends by month, city, package type, and order source.
- Compare new and returning customers and explore retention and order frequency.
- Track order statuses, completed orders, cancellations, and cancellation rates.
- Validate order-to-package-category mappings and user classifications.

## Key Metrics Displayed

- Total orders and completed orders
- Total customers and new user count
- Net revenue and average revenue per order
- Revenue growth and customer repeat rate
- Cancellation rate and cancel-to-complete percentage
- Revenue by city, package type, and order source
- Customer retention by cohort and order frequency

## Tools & Skills

- **Power BI** — dashboard design and interactive data visualization *(edit if a different tool was used)*
- **Data Analysis**
- **Business Intelligence**
- **Data Quality & Validation**
- **KPI Reporting**

## Insights to Explore

Use the dashboard filters to explore how business performance changes across the selected date range, city, package type, currency, and order source. The visuals support analysis of revenue contribution, order outcomes, customer behavior, and data mapping quality.

> **Note:** The screenshots are visual previews. Add the original Power BI `.pbix` file (or the source files for the tool you used) to this repository if you are able to share them. Remove any sensitive or confidential data before publishing.

## Repository Structure

```text
healthcare-business-performance-dashboard/
├── README.md
└── screenshots/
    ├── 01-business-performance-overview.png
    ├── 02-order-analysis.png
    ├── 03-customer-analysis.png
    ├── 04-revenue-analysis.png
    └── 05-data-quality-mapping-validation.png
```

## About

This project demonstrates how healthcare business data can be organized into an interactive dashboard to support data-driven decision-making.

If you have feedback or suggestions, feel free to open an issue or connect with me on GitHub.
"""
(project / "README.md").write_text(readme, encoding="utf-8")

zip_path = base / "healthcare-business-performance-dashboard-github.zip"
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as zf:
    for path in project.rglob("*"):
        if path.is_file():
            zf.write(path, path.relative_to(base))

print("Created project package:", zip_path)
print("Included screenshots:", len(copied))
print("README:", project / "README.md")
