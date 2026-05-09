# Chart Blocks

Use `obsidian-charts` code blocks with `type: bar`.

## Revenue vs Net Profit

Preferred structure:

```chart
type: bar
labels:
  - FY2021
  - FY2022
  - FY2023
  - FY2024
  - FY2025
series:
  - title: Revenue
    data:
      - 0
      - 0
      - 0
      - 0
      - 0
  - title: Net Profit
    data:
      - 0
      - 0
      - 0
      - 0
      - 0
```

## Segment Revenue

- If segment taxonomy is stable across years, you may build a multi-series chart.
- If taxonomy changes, use latest-year-only and add a warning line above the chart.

Latest-year-only structure:

```chart
type: bar
labels:
  - Segment A
  - Segment B
  - Segment C
series:
  - title: FY2025 Revenue
    data:
      - 0
      - 0
      - 0
```

## Safety Notes

- Do not mix currencies in one chart.
- Do not chart inferred values.
- If units differ across source years, normalize first or stop and report the issue.
