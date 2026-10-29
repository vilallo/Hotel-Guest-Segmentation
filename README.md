# Hotel Guest Segmentation

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=white)

Behaviour-based customer segmentation for an independent Lisbon hotel. The project replaces an industry-based market segmentation with four actionable guest profiles, each linked to a targeted marketing plan.

## Problem

The hotel's existing `MarketSegment` field mostly showed which industry a customer came from, not how they behave as a guest. As a result, the marketing team could not design targeted campaigns. The goal was to segment roughly **111,700 customer records** by booking behaviour, spending and preferences, and to turn each segment into concrete marketing actions.

## Approach

The work follows the **CRISP-DM** methodology end to end in [`hotel_guest_segmentation.ipynb`](hotel_guest_segmentation.ipynb).

**1. Data understanding**
- Profiled demographics, booking behaviour, revenue, distribution channels and 13 special-request flags.
- Found that `Age` and `DocIDHash` are missing together, so the data is not missing at random.
- Reviewed how well the old segmentation actually separated customers.

**2. Data cleaning**
- Removed duplicate customers (same name hash, document hash and nationality).
- Removed about 32k leads that never interacted with the hotel.
- Removed GDPR-sensitive minors (under 16), implausible ages, negative lead times and inconsistent check-in/revenue records.

**3. Feature engineering**
- Built `RevenuePerStay`, `RevenuePerDay`, `TotalSpecialRequests` and `PersonRoom`.
- Added `AvgIncome` per nationality as an external socio-economic signal.

**4. Preparation**
- Treated outliers and applied a **Johnson transformation** to skewed features.
- Kept the top 20 nationalities and grouped the rest as "Other".
- Scaled the features with `StandardScaler`, imputed `Age` with **KNN (k=20)** and selected features by correlation.

**5. Modelling**
- Compared the elbow method, silhouette scores and a Ward **hierarchical clustering** dendrogram.
- Two clusters scored best on silhouette (0.19) but were too generic for marketing. Four clusters (silhouette 0.16) were chosen for richer, still balanced segments.
- Final model: **K-Means** (k=4, k-means++, `n_init=15`).

**6. Evaluation**
- Profiled each cluster's centroid.
- Compared the new segments with the old ones in a contingency table.
- Visualised the separation with **t-SNE**.

## Results

| Segment | Profile | Recommended actions |
|---------|---------|---------------------|
| **Retired** | Older guests who book far in advance, with low revenue per stay | Loyalty rewards, next-stay vouchers, newsletters and direct mail |
| **Free Souls** | Spontaneous last-minute bookers | Flash sales, "book now, pay later", social-media incentives, long-stay discounts |
| **Star Customers** | Premium, high-value guests | Concierge service, exclusive membership perks, tailored welcome |
| **Businessman of the World** | Efficiency-focused business travellers | Lounge access, express check-in, loyalty upgrades, personalised offers |

The new segments are more balanced in size than the original segmentation and reflect how guests actually behave. This lets marketing move from generic campaigns to targeted ones.

## Repository structure

```
Hotel-Guest-Segmentation/
├── hotel_guest_segmentation.ipynb   # Full analysis: cleaning, features, clustering, profiling
├── requirements.txt
└── README.md
```

> **Note:** The dataset (`Case1_HotelCustomerSegmentation.csv`) is not included in this repository.

## Setup

```bash
pip install -r requirements.txt
```

Place `Case1_HotelCustomerSegmentation.csv` in a `data/` folder next to the notebook, then run all cells.

## Tech stack

Python · pandas · NumPy · scikit-learn (K-Means, Agglomerative Clustering, KNN Imputer, t-SNE) · SciPy · Matplotlib · Seaborn · Squarify

## Author

**Vinícius Pinto** · [GitHub](https://github.com/vilallo)
