# Reducing EV Charging Emissions with Predictive Smart Charging

## 🔎 Overview

Electric vehicle (EV) adoption is growing rapidly, but when vehicles charge can significantly impact grid emissions. Electricity carbon intensity varies throughout the day depending on renewable generation and grid demand.

This project develops a **machine learning–driven smart charging system** that predicts whether **EV charging should be delayed to a cleaner electricity window**. By shifting charging to periods with lower carbon intensity, the system reduces emissions while maintaining user convenience.

The solution integrates EV charging session data and grid emissions data to identify optimal charging windows and quantify potential emissions savings.

## 🔐 Business Problem

EV charging often occurs during evening hours, when grid electricity is more carbon-intensive due to higher reliance on natural gas generation.

However, midday electricity tends to be cleaner due to higher solar production.

This creates an opportunity for smart charging algorithms that:
- Identify when delaying charging reduces emissions
- Automatically shift charging to cleaner electricity periods
- Balance user convenience with environmental impact

Key stakeholders include:
- EV drivers → lower charging costs and minimal disruption
- Automakers (e.g., Rivian) → improved sustainability and smart-charging adoption
- Utilities → reduced peak demand and lower grid emissions

The goal is to build a predictive model that recommends when charging should be delayed to minimize carbon emissions.

## 📊 Dataset

The analysis integrates three datasets:

### 1. EV Charging Sessions
- Timestamp of charging sessions
- Charging start and end times
- Energy delivered (kWh)
- Session duration

### 2. Grid Emissions Data
- Hourly carbon intensity (lbs CO₂ per kWh)
- Timestamp
- Month, weekday, hour
- Weekend indicator

### 3. Vehicle Market Data
- EV price ranges
- Monthly payment estimates
- Sales volume

## 📍 Methodology & Findings

The project follows a structured data science workflow:

### 1. Exploratory Data Analysis

Analyzed charging patterns and grid emissions across:
- Hour of day
- Weekday vs. weekend
- Seasonal trends
- Charging duration and energy delivered

Key findings:
- Most EV charging sessions begin between 7 PM and 10 PM.
- Charging often occurs during high-emission evening hours, when solar generation is low.
- Midday (9 AM–2 PM) consistently shows the lowest grid emissions.

<img width="713" height="393" alt="image" src="https://github.com/user-attachments/assets/4a8545f9-1da6-4e3a-986e-e6494465ae5a" />

<img width="1001" height="574" alt="image" src="https://github.com/user-attachments/assets/d8a21aab-8ec3-4bba-a5ac-5f9f9158aba3" />

<img width="916" height="701" alt="image" src="https://github.com/user-attachments/assets/c32b321a-1ce0-4698-a6c5-f2d729162284" />

This revealed that charging behavior and emissions patterns are misaligned, creating opportunities for load shifting.

### 2. Label Construction

A supervised learning problem was created by defining two time windows:
- Window A: emissions if charging begins immediately (next 8 hours)
- Window B: emissions if charging is delayed by 8 hours

If Window B emissions < 0.9 × Window A emissions, charging should be delayed. Otherwise, charge immediately.

The dataset is imbalanced, with most hours not benefiting from delay (84%). This converts emissions optimization into a binary classification problem.

### 3. Feature Engineering

Several time-series features were engineered to capture emission dynamics:

**Time-based features**
- Hour of day
- Weekday vs. weekend
- Month

**Emission trend features**
- Lagged emissions (1–24 hours)
- Rolling averages
- Rolling volatility
- Differences from historical averages

These features allow the model to detect short-term and seasonal emission patterns.

### 4. Model Development

Multiple models were evaluated:
- Logistic Regression (baseline)
- Logistic Regression with interactions
- Random Forest
- **XGBoost (best performing)**

Models were evaluated using:
- ROC AUC
- Precision/Recall
- Confusion matrix

<img width="613" height="547" alt="image" src="https://github.com/user-attachments/assets/66222be0-e687-41bb-8efa-ac11ca3492ae" />

XGBoost achieved the highest predictive performance among the tested models.

<img width="765" height="860" alt="image" src="https://github.com/user-attachments/assets/a8623691-b8a8-4e15-9631-c051c30df77e" />

### 5. Impact Simulation

The trained model was used to simulate real-world charging decisions and estimate emissions reductions compared with baseline charging behavior.

**Emissions under different charging policies:**
| Policy                       | Emissions         | Reduction |
| ---------------------------- | ----------------- | --------- |
| Always Charge Now (Baseline) | 4,931,979 lbs CO₂ |           |
| Optimal (Perfect Info)       | 4,265,200 lbs CO₂ | -13.52%   |
| XGBoost Smart Charging       | 4,343,579 lbs CO₂ | -11.93%   |

XGBoost captures 88.25% of the theoretical maximum savings.

**Annual emissions saving per vehicle:** 287.44 lbs CO2

**Elasticity modeling:**
- ~94% of the variation in EV sales can be explained solely by differences in vehicle pricing, suggesting price is the dominant driver of EV demand in this dataset.
- A 1% reduction in the effective vehicle price leads to an estimated 3.2% increase in sales.
- The 95% confidence interval (–3.81 to –2.58) reflects the expected inverse relationship between price and demand, but the magnitude indicates that EV demand is highly elastic and consumers are very sensitive to changes in ownership cost.

Overall, even relatively small financial incentives can translate into meaningful adoption gains, making the proposed SCE partnership potentially attractive for automakers seeking higher market penetration.

## ✍️ Business Recommendations

### 1. Implement automated smart charging

Integrate ML models into EV charging systems to automatically delay charging when cleaner electricity is expected.

### 2. Incentivize off-peak charging

Utilities and automakers should introduce:
- Discounted midday charging rates
- Smart-charging reward programs
- Carbon-aware charging notifications

**Incentive design:**
- Recommended incentive rate based on maximizing sales lift: $10.00 per lb CO₂ avoided
- Effective new price after incentive: $57,213
- New predicted annual sales: 365.3 thousand
- Increase in sales: +5.8 thousand (1.6%)
- SCE program cost at the adoption: $103,323,745 per year

### 3. Improve EV charging UX

Smart charging should be fully automated, requiring no manual scheduling by users.

Drivers should only override the system when needed.

### 4. Align EV charging with renewable energy

Automakers and utilities should prioritize charging during solar-rich hours, reducing grid emissions and improving renewable utilization.

## ⚙ Tools & Techniques

- Python
- Data preprocessing (Pandas, NumPy)
- Data visualization (Matplotlib, Seaborn)
- Machine learning (Logistic Regression, Random Forest, XGBoost)
- Model explainability (Feature Importance, SHAP)
