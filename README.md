# 📈 Sales Forecasting Using Facebook Prophet

<p align="center">
  <img src="https://drive.google.com/uc?id=1l7bHyrjzq839zVZE06cfdDksLabCN2hg" width="800" alt="Sales Forecasting Banner"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-blue?style=flat-square&logo=python" />
  <img src="https://img.shields.io/badge/Prophet-Facebook-green?style=flat-square" />
  <img src="https://img.shields.io/badge/Pandas-Data%20Analysis-yellow?style=flat-square&logo=pandas" />
  <img src="https://img.shields.io/badge/Time%20Series-Forecasting-orange?style=flat-square" />
  <img src="https://img.shields.io/badge/Domain-Retail%20%26%20Sales-red?style=flat-square" />
</p>

---

## 📊 Project Overview

This project implements **time series forecasting** for retail sales using **Facebook Prophet**, enabling sales teams to predict future revenue with high accuracy while accounting for trends, seasonality, and holiday effects.

**Business Problem:** Retail chains struggle to forecast sales accurately across multiple stores, leading to poor inventory planning, staffing inefficiencies, and missed revenue opportunities. Traditional forecasting methods fail to capture complex seasonal patterns and holiday impacts.

**Solution:** Built an automated forecasting pipeline using Facebook Prophet that predicts daily sales for 1,115 stores up to 60 days in advance, incorporating holiday calendars (school holidays, state holidays) for improved accuracy.

**Data Source:** [Kaggle — Rossmann Store Sales](https://www.kaggle.com/c/rossmann-store-sales/data)

---

## 🔑 Key Highlights

- **Massive Scale:** Analyzed 1,017,209 transactions across 1,115 unique stores
- **Facebook Prophet:** Advanced time series model capturing trend + seasonality + holidays
- **Holiday Integration:** Incorporated school holidays and state holidays (Easter, Christmas) for better predictions
- **Multi-Store Forecasting:** Created reusable function to forecast any store on demand
- **Business Insights:** Discovered sales peak around Christmas and month-end (30th/1st)
- **Actionable Output:** 60-day forecasts with confidence intervals for inventory planning

---

## 📈 Results & Business Insights

### Key Findings from Data Analysis

| Discovery | Impact |
|-----------|--------|
| **Sales Peak Timing** | Sales & customers peak around Christmas timeframe |
| **Monthly Pattern** | Highest sales on 30th and 1st of each month (payday effect) |
| **Weekly Minimum** | Lowest customer count on the 24th of each month |
| **Promo Effectiveness** | Promo1 correlates with higher sales; Promo2 shows no effect |
| **Customer Correlation** | Strong positive correlation between customers and sales |

### Dataset Statistics

- **Total Transactions:** 1,017,209 records
- **Unique Stores:** 1,115 stores
- **Average Daily Sales:** €6,955 (open stores only)
- **Average Customers/Day:** 762 customers
- **Maximum Daily Sales:** €41,551
- **Store Open Rate:** ~80% of the time

### Forecasting Capabilities

- **Forecast Horizon:** Up to 60 days into the future
- **Granularity:** Daily sales predictions per store
- **Confidence Intervals:** Upper and lower bounds provided
- **Holiday Adjustment:** Automatic adjustment for school and state holidays
- **Trend Detection:** Long-term growth/decline patterns identified

---

## 🛠️ Technologies & Tools

**Language:** Python 3.x

| Category | Libraries |
|----------|-----------|
| **Time Series Forecasting** | Facebook Prophet (fbprophet) |
| **Data Processing** | Pandas, NumPy |
| **Visualization** | Matplotlib, Seaborn |
| **Data Analysis** | Exploratory Data Analysis (EDA) |
| **Environment** | Google Colab, Jupyter Notebook |

**Techniques Applied:**
- Time Series Analysis
- Trend & Seasonality Decomposition
- Holiday Effect Modeling
- Multiplicative Seasonality
- Additive Regression
- Confidence Interval Estimation
- Correlation Analysis

---

## 📁 Dataset Description

### Primary Dataset: `train.csv` (1,017,209 records)

| Feature | Description |
|---------|-------------|
| `Id` | Transaction ID (combination of Store + Date) |
| `Store` | Unique store identifier (1–1115) |
| `Sales` | **Target Variable** — Daily sales in Euros |
| `Customers` | Number of customers on a given day |
| `Open` | Store status (0 = closed, 1 = open) |
| `Promo` | Store running promotion (0 = no, 1 = yes) |
| `StateHoliday` | State holiday type (a = public, b = Easter, c = Christmas, 0 = none) |
| `SchoolHoliday` | School closure impact (0 = no, 1 = yes) |
| `Date` | Transaction date (YYYY-MM-DD) |

### Store Information: `store.csv` (1,115 stores)

| Feature | Description |
|---------|-------------|
| `Store` | Store ID |
| `StoreType` | Store category (a, b, c, d) |
| `Assortment` | Product range (a = basic, b = extra, c = extended) |
| `CompetitionDistance` | Distance to nearest competitor (meters) |
| `CompetitionOpenSince` | When competitor opened (Month/Year) |
| `Promo2` | Continuous promotion participation (0/1) |
| `Promo2Since` | When Promo2 started (Year/Week) |
| `PromoInterval` | Months when Promo2 restarts (e.g., "Feb,May,Aug,Nov") |

---

## 🚀 Project Workflow

### 1. Data Loading & Inspection
- Imported 1,017,209 transaction records from `train.csv`
- Loaded store metadata for 1,115 stores from `store.csv`
- Verified data types and examined structure

### 2. Exploratory Data Analysis (EDA)
- **Missing Value Analysis:** Identified missing competition dates (354 stores)
- **Distribution Analysis:** 
  - Average 600 customers/day, max 7,388
  - Sales equally distributed across days of week (~150K per day)
  - Stores open 80% of time
  - Promo1 active 40% of time
- **Outlier Detection:** Identified extreme sales day (€41,551)

### 3. Data Cleaning & Preprocessing
- **Filtered Closed Stores:** Removed records where `Open = 0`
- **Dropped Redundant Columns:** Removed `Open` column after filtering
- **Date Parsing:** Extracted Year, Month, Day, DayOfWeek features
- **Data Merging:** Combined transaction data with store information

### 4. Pattern Discovery
- **Monthly Trends:** Sales peak in December (Christmas effect)
- **Daily Patterns:** Highest sales on 30th and 1st (payday effect)
- **Weekly Patterns:** Identified Sunday as lowest sales day
- **Correlation Analysis:** 
  - Customers ↔ Sales: Strong positive correlation
  - Promo1 ↔ Sales: Positive correlation
  - Promo2 ↔ Sales: No significant effect

### 5. Prophet Model Training — Part A (Basic Model)
- **Data Formatting:** Renamed columns to Prophet format (`ds`, `y`)
- **Model Configuration:** Initialized Prophet with default parameters
- **Training:** Fit model on historical sales data
- **Forecasting:** Generated 60-day predictions with confidence intervals
- **Visualization:** Plotted forecast with trend and seasonal components

### 6. Prophet Model Training — Part B (With Holidays)
- **Holiday Extraction:** 
  - Created school holiday calendar from dataset
  - Created state holiday calendar (Easter, Christmas, Public holidays)
  - Combined into unified holiday dataframe
- **Model Enhancement:** Initialized Prophet with holiday calendar
- **Improved Training:** Fit model accounting for holiday effects
- **Enhanced Forecasting:** 60-day predictions with holiday-adjusted patterns
- **Comparison:** Validated improvement over basic model

### 7. Function Creation for Reusability
```python
def sales_prediction(Store_ID, sales_df, holidays, periods):
    # Automated forecasting for any store
    # Takes: Store ID, dataframe, holidays, forecast days
    # Returns: Forecast plot with predictions
```

---

## 📊 Prophet Model Architecture

**Facebook Prophet Components:**

```
Sales Forecast = Trend + Seasonality + Holidays + Error

Where:
- Trend: Long-term growth or decline pattern
- Seasonality: 
    * Weekly seasonality (day of week effect)
    * Yearly seasonality (monthly patterns)
- Holidays: 
    * School holidays
    * State holidays (Easter, Christmas, Public)
- Error: Uncertainty captured in confidence intervals
```

**Model Advantages:**
- Robust to missing data and outliers
- Automatically detects changepoints in trends
- Handles multiple seasonality periods
- Incorporates custom holidays
- Provides interpretable forecasts with uncertainty bands

---

## 💡 Business Impact & Applications

### For Sales Teams

| Use Case | Benefit |
|----------|---------|
| **Inventory Planning** | Order stock 60 days in advance based on forecasts |
| **Staffing Optimization** | Schedule employees according to predicted busy periods |
| **Promotional Timing** | Launch campaigns during high-traffic periods |
| **Revenue Forecasting** | Accurate financial planning for quarterly goals |
| **Store Comparison** | Identify underperforming stores vs. forecasts |

### ROI Potential

- **Inventory Waste Reduction:** 15-25% decrease in overstock/stockouts
- **Labor Cost Optimization:** 10-20% savings through better shift planning
- **Revenue Growth:** 5-10% increase from optimized promotional timing
- **Competitive Advantage:** Respond faster to market trends

### Specific Insights for Action

1. **Christmas Strategy:** Double inventory and staff in December
2. **Month-End Focus:** Increase stock on 28th-2nd of each month (payday effect)
3. **Promo2 Review:** Discontinue Promo2 (no impact on sales)
4. **Sunday Optimization:** Reduce staff on Sundays (lowest traffic)

---

## 🔧 How to Use This Project

### Prerequisites
```bash
Python 3.7+
Google Colab (recommended) or Jupyter Notebook
```

### Installation
```bash
# Clone the repository
git clone https://github.com/yourusername/Sales-Forecasting-Prophet.git

cd Sales-Forecasting-Prophet

# Install dependencies
pip install pandas numpy matplotlib seaborn fbprophet
```

### Running the Notebook
```bash
# Option 1: Google Colab (Recommended)
# Upload the .ipynb file to Google Colab
# Mount Google Drive and update dataset paths

# Option 2: Local Jupyter
jupyter notebook Sales_Department_Solution.ipynb
```

### Making Predictions for Any Store
```python
# Load your data
import pandas as pd
from fbprophet import Prophet

# Create holiday calendar
holidays = create_holiday_dataframe()  # see notebook for function

# Forecast for Store 10, next 60 days
sales_prediction(Store_ID=10, sales_df=data, holidays=holidays, periods=60)
```

---

## 📚 Key Learnings

Through this project, I gained expertise in:

- Time series forecasting with industry-standard tools (Prophet)
- Handling large-scale retail datasets (1M+ records)
- Feature engineering from date/time data
- Holiday effect modeling for improved accuracy
- Translating technical forecasts into business recommendations
- Correlation analysis between promotions and sales
- Building reusable forecasting functions for production use

---

## 🔮 Future Enhancements

- [ ] Add ARIMA model comparison for performance benchmarking
- [ ] Implement LSTM neural network for deep learning comparison
- [ ] Build interactive Streamlit dashboard for real-time forecasting
- [ ] Add multi-step ahead forecasting (90, 180 days)
- [ ] Incorporate external features (weather, economic indicators)
- [ ] Deploy model as REST API for production use
- [ ] Add automated retraining pipeline with new data
- [ ] Implement A/B testing framework for promotional impact
- [ ] Create store clustering for group-level forecasts
- [ ] Add anomaly detection for unusual sales patterns

---

## 👨‍💻 About the Developer

Passionate about applying AI and data science to solve real business problems in retail and sales. This project demonstrates my ability to:

- Work with large-scale time series datasets
- Apply advanced forecasting techniques
- Extract actionable business insights from data
- Build production-ready predictive models
- Communicate complex results to non-technical stakeholders

**Connect with me:**
- LinkedIn: [https://www.linkedin.com/in/rony-zeenaldeen-b288112ab/]
- GitHub: [https://github.com/Rony-ZenAlden/]
- Email: [ronizenalden@gmail.com]

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## 🙏 Acknowledgments

- **Dataset:** [Kaggle — Rossmann Store Sales Competition](https://www.kaggle.com/c/rossmann-store-sales/data)
- **Tool:** [Facebook Prophet](https://facebook.github.io/prophet/) — Time series forecasting library
- **Inspiration:** Real-world retail forecasting challenges

---

## 📞 Contact & Feedback

Have questions or suggestions? Feel free to:
- Open an issue on GitHub
- Connect with me on LinkedIn
- Send an email

**⭐ If you found this project helpful, please star the repository!**

---

*Last Updated: February 2026*
