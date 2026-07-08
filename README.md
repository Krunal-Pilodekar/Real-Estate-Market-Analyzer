<div align="center">

# 🏠 Real Estate Market Analyzer

**ML-powered dashboard for predicting residential property prices across 6 major Indian cities**

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?logo=streamlit&logoColor=white)](https://streamlit.io/)
[![XGBoost](https://img.shields.io/badge/XGBoost-97.7%25_R²-006400)](https://xgboost.readthedocs.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

[Demo](#-demo) · [Features](#-features) · [Installation](#-installation) · [Usage](#-usage) · [Architecture](#-architecture)

</div>

---

## 📌 Overview

**Real Estate Market Analyzer** predicts residential property prices in real time based on city, location, area, bedrooms, and amenities. Instead of relying on manual broker estimates, it uses a trained **XGBoost regression model** wrapped in an interactive **Streamlit** dashboard — giving instant, data-driven price predictions with visual analytics.

Built as a BITS Pilani WILP Design Project (BITS ZC229T), in collaboration with HCLTech.

<a name="features"></a>
## ✨ Features

- 🌆 **Multi-city support** — Delhi, Mumbai, Bangalore, Chennai, Kolkata, Hyderabad
- 🎯 **~97.7% prediction accuracy** with a tuned XGBoost regressor
- 🏘️ **Dynamic cascading filters** — city → location → area → bedrooms → amenities
- 🧮 **Amenity Score engineering** — aggregates 30+ binary amenity features into a single quality signal
- 📍 **Target (mean) encoding** for high-cardinality location data — avoids the dimensionality blow-up of one-hot encoding
- 📊 **Interactive Plotly visualizations** — price-per-sqft by location, amenities vs. price, area vs. price
- 🌡️ **Live gauge meter** showing predicted price level
- 🧮 **What-If Price Calculator** for quick manual estimation
- ⚙️ **Metadata-driven architecture** — cities, locations, feature names, and slider ranges are all read from a single exported metadata file, so the frontend never hardcodes them
- 🛡️ **Prediction safety checks** — guards against NaN/Inf outputs

<a name="demo"></a>
## 🖥️ Demo

<img width="950" height="450" alt="image" src="https://github.com/user-attachments/assets/936b960d-002d-4b87-bd39-bf9a2aaf37b9" />
<img width="950" height="450" alt="image" src="https://github.com/user-attachments/assets/e9747d78-70b9-436a-be58-e44a7575167a" />




## 🧠 Model Performance

| Model | R² (Test) | MAE | RMSE | MAPE |
|---|---|---|---|---|
| Linear Regression | 0.831 | ₹1,724,698 | ₹2,595,761 | 21.86% |
| Random Forest | 0.891 | ₹1,204,532 | ₹2,091,030 | 13.55% |
| **XGBoost (final)** | **0.906** | **₹1,121,762** | **₹1,939,552** | **12.66%** |

XGBoost was selected for production because it handled non-linear feature interactions (location × amenities × area) far better than linear or single-tree models.

<a name="architecture"></a>
## 🏗️ Architecture

```
┌─────────────────────┐     ┌──────────────────────┐     ┌───────────────────────┐
│  Preprocessing Layer│ --> │  ML Prediction Layer │ --> │  Streamlit Frontend   │
│  clean / dedupe /   │     │XGBoost + AmenityScore│     │  cascading filters,   │
│  encode locations   │     │  + Target Encoding   │     │  Plotly charts, gauge │
└─────────────────────┘     └──────────────────────┘     └───────────────────────┘
                                       │
                                       ▼
                          xgb_model.joblib + xgb_metadata.joblib
                         (feature names, city/location maps, ranges)
```

The **metadata file** is the key architectural trick: the dashboard never hardcodes cities, locations, or feature order — it reads them straight from the metadata exported at training time, so retraining on new cities requires zero frontend changes.

## 📂 Repository Structure

```
Real-Estate-Market-Analyzer/
├── app.py                       # Streamlit dashboard (main entry point)
├── Part1_Data_Cleaning.ipynb    # Data cleaning & preprocessing notebook
├── Part2_ML_Models.ipynb        # Model training & evaluation notebook
├── xgb_model.joblib             # Trained XGBoost model
├── xgb_metadata.joblib          # Feature names, city/location maps, slider ranges
├── real_estate_cleaned.csv      # Combined cleaned dataset
├── Delhi.csv / Mumbai.csv / Bangalore.csv
├── Chennai.csv / Kolkata.csv / Hyderabad.csv
├── LICENSE
├── requirements.txt
└── README.md
```

<a name="installation"></a>
## 🚀 Installation

```bash
# Clone the repo
git clone https://github.com/Krunal-Pilodekar/Real-Estate-Market-Analyzer.git
cd Real-Estate-Market-Analyzer

# Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# Install dependencies
pip install streamlit plotly joblib pandas numpy xgboost scikit-learn
```

> 💡 Add a `requirements.txt` (see [Suggestions](#-suggestions-to-make-this-repo-more-professional) below) so this becomes `pip install -r requirements.txt`.

<a name="usage"></a>
## ▶️ Usage

```bash
streamlit run app.py
```

Then open the local URL Streamlit prints (usually `http://localhost:8501`) and:
1. Select a **city** and **location** from the sidebar
2. Set **area** and **bedrooms** (slider or manual input)
3. Check relevant **amenities**
4. View the **real-time predicted price**, gauge, and comparison charts

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| Language | Python |
| ML | XGBoost, Scikit-learn |
| Data | Pandas, NumPy |
| Frontend | Streamlit |
| Visualization | Plotly |
| Serialization | Joblib |
| Dev tools | VS Code, Google Colab, Jupyter, GitHub |

## 👥 Team

| Name | Focus Area |
|---|---|
| Krunal Pilodekar | ML architecture, feature engineering, metadata pipeline |
| Sahil Shafique | Streamlit dashboard & deployment integration |
| Pranav Kokare | Dataset collection, cleaning & preprocessing |
| Ayush Maurya | Model experimentation & evaluation |
| Ashutosh Singh | GitHub/repo management & project organization |

**Mentors:** Jagan Vignesh R, Poorani Mohan, Amit Chaudhary, Tarun Prithvi Bhuria, Shashi Kant Pandey (HCLTech)

## 🔮 Future Work

- SHAP-based explainability for individual predictions
- Automated periodic retraining on live listing data
- Hyperparameter tuning via Optuna / GridSearchCV
- Cloud deployment (Streamlit Cloud / AWS / Azure / Render)
- Additional cities, rental price prediction, mobile-friendly UI

## 📄 License

MIT License

Copyright (c) 2026 Krunal Pilodekar

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


---

<div align="center">
Built as part of BITS Pilani WILP — BITS ZC229T Design Project, May 2026
</div>
