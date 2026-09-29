# 📈 Stock Market Trend & Technical Analysis Dashboard

[![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-Data%20Analysis%20Expressions-0078D4?style=for-the-badge)](https://learn.microsoft.com/en-us/dax/)
[![Domain](https://img.shields.io/badge/Domain-Financial%20Markets%20%26%20Trading-00C853?style=for-the-badge)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

---

## 📌 Project Overview

An interactive, high-performance financial dashboard built using **Microsoft Power BI** to analyze historical stock prices and trading volumes from **May 21, 2015 to May 25, 2023**. 

Designed with a modern dark cyberpunk/fintech UI, this dashboard combines price action with short-term (**20 DMA**) and medium-term (**50 DMA**) momentum indicators, enabling investors, analysts, and traders to pinpoint market trends, institutional volume surges, and support/resistance zones.

---

## 🖼️ Dashboard Preview

![Dashboard Preview](screenshot.png)

> **Note:** Save your dashboard image as `screenshot.png` in the repository root directory.

---

## 📊 Key Performance Indicators (KPIs)

| Metric | Value | Description |
| :--- | :---: | :--- |
| **Latest Close** | **$57.71** | Most recent closing price in the selected timeframe |
| **Highest Price** | **$176.29** | All-time high price reached during the bull rally |
| **Lowest Price** | **$1.85** | Historical price floor/base within the tracked period |
| **Average Volume** | **18.07M** | Mean daily shares traded across the selected timeframe |

---

## 🎯 Key Visualizations & Technical Indicators

### 1. Volume & Price Correlation (`Sum of Volume and Close Prices`)
- **Type:** Dual-Axis Line & Column Combo Chart
- **Purpose:** Compares price movement against market volume to identify accumulation, distribution, and price breakout confirmations.

### 2. Short-Term Momentum (`Close Prices and 20 Day Moving Average`)
- **Type:** Dual-Line Chart
- **Purpose:** Filters daily market noise to evaluate short-term momentum and dynamic support/resistance levels.

### 3. Trend Direction (`Close Prices and 50 Day Moving Average`)
- **Type:** Dual-Line Chart
- **Purpose:** Tracks medium-to-long term market trends. Useful for identifying trend continuation, Golden Crosses, and Death Crosses.

### 4. Liquidity Profile (`Sum of Volume by Date`)
- **Type:** High-Density Area / Bar Chart
- **Purpose:** Flags anomalous institutional volume surges (e.g., historical peak of **0.21 Billion shares** traded).

### 5. Interactive Date Range Slicer
- **Type:** Dynamic Dual-Handle Date Slider
- **Range:** `21-05-2015` to `25-05-2023`
- **Purpose:** Allows granular, multi-year cross-filtering across all charts and KPI cards simultaneously.

---

## 🧮 DAX Measures Implemented

```dax
// 1. 20-Day Simple Moving Average (SMA)
20 Day Moving Average = 
VAR DaysBack = 20
VAR CurrentDate = MAX('StockData'[Date])
VAR Period =
    DATESINPERIOD(
        'StockData'[Date],
        CurrentDate,
        -DaysBack,
        DAY
    )
RETURN
    AVERAGEX(
        Period,
        CALCULATE(AVERAGE('StockData'[Close]))
    )
// 2. 50-Day Simple Moving Average (SMA)
50 Day Moving Average = 
VAR DaysBack = 50
VAR CurrentDate = MAX('StockData'[Date])
VAR Period =
    DATESINPERIOD(
        'StockData'[Date],
        CurrentDate,
        -DaysBack,
        DAY
    )
RETURN
    AVERAGEX(
        Period,
        CALCULATE(AVERAGE('StockData'[Close]))
    )
// 3. Latest Closing Price
Latest Close = 
CALCULATE(
    SELECTEDVALUE('StockData'[Close]),
    LASTDATE('StockData'[Date])
)
// 4. Highest Recorded Price
Highest Price = 
CALCULATE(
    MAX('StockData'[High]),
    ALLSELECTED('StockData')
)
// 5. Lowest Recorded Price
Lowest Price = 
CALCULATE(
    MIN('StockData'[Low]),
    ALLSELECTED('StockData')
)
// 6. Average Daily Volume
Average Volume = 
AVERAGE('StockData'[Volume])
