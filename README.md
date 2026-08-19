# Lagos Rent Predictor

A data science project that predicts annual rent for properties in Lagos, Nigeria. It was built to give renters a real benchmark before they negotiate or sign a lease.

## Why this project

Finding a fair rent price in Lagos is mostly guesswork. Listings vary wildly even within the same neighborhood, and renters rarely have a way to check if a price is reasonable. This model learns from thousands of real listings to estimate what a property *should* cost based on its size, features, and location.

## The data

- A [Lagos Rent Prices 2022 (Kaggle)]  with 53,070 listings, cleaned down to 50,048
- Features used: bedrooms, bathrooms, toilets, serviced/furnished/newly-built flags, city, and neighborhood

## What I did

1. **Cleaned the data** — prices came in as messy text ("5,000,000/year", "3,500,000", "78,000,000/day"), so I parsed and standardized everything to annual rent, dropped implausible outliers, and fixed inconsistent bedroom/bathroom counts (some listings said "4 Bedroom Duplex" in the title but had "0" in the structured field).
2. **Explored the data** — rent is heavily skewed toward a few expensive properties, so I log-transformed the target. Location turned out to be the single biggest price driver.
3. **Engineered features** — grouped rare neighborhoods and capped extreme bedroom counts so the model wasn't learning from a handful of outlier listings.
4. **Trained and compared models** — Linear Regression, Random Forest, and Gradient Boosting, all benchmarked against a simple "median by city" baseline.
5. **Evaluated honestly** — checked not just overall accuracy but where the model does well and where it struggles.

## Results

| Model | R² | MAE (log) |
|---|---|---|
| Baseline (city median) | 0.46 | — |
| Linear Regression | 0.77 | 0.36 |
| Random Forest | 0.78 | 0.35 |
| **Gradient Boosting** | **0.78** | **0.35** |

The best model (Gradient Boosting) predicts within about **₦1.5M** of actual rent on average, with a **25% median error**. It's most accurate for typical listings (₦1M–₦10M/year) and less accurate at the very cheap and very expensive ends of the market.

## What drives Lagos rent (based on the data)

- **Location matters most** — Ikoyi rents for over 13x what Yaba does, on average
- **Size matters, but the signals overlap** — bedrooms, bathrooms, and toilet counts are all highly correlated, so they're really capturing one thing: how big the place is
- **Furnished ≠ pricier** — furnished listings actually rent for *less* on average, likely because they skew toward small studios and mini-flats rather than family-sized homes

## Repo structure
`
├── notebooks/
│ ├── Lagos_Rent_Predictor_Final.ipynb ← start here
│ └── exploratory/ ← earlier working notebooks
├── data/
│ ├── raw/
│ └── processed/
├── models/
│ └── best_model.pkl
└── demo/
`

## Running it

Clone the repo, open `notebooks/Lagos_Rent_Predictor_Final.ipynb` in Colab or Jupyter, and run all cells top to bottom.

## Limitations

- Lagos only — Abuja was originally planned, but no reliable Abuja rent dataset turned up (a Nigeria-wide dataset I considered turned out to be sale prices, not rent)
- Some cleaning stats (like imputation medians) were computed on the full dataset before splitting train/test, a minor form of leakage worth noting
- Weaker predictions at the very low and very high end of the price range, likely because those properties depend on things this data doesn't capture (finish quality, amenities, exact street)

## What's next

- Add real Abuja rent data
- Try features like flood risk or distance to business districts
- Handle budget and premium listings with separate models

