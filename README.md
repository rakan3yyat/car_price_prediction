# Used Car Price Prediction

**Type:** Regression

## Goal
Predict the **price of a used car** (USD) from its brand, age, mileage, engine, condition and equipment.

**Target:** `price` (USD)

## Data
The data is **synthetic** (fake, created with Python) so it can be big and have all the problems we want to practice on.

- **Generator:** `generate_data.py` (random seed = 123, so you always get the same data)
- **File:** `data/used_car_listings.csv` - about 120,000 rows (+0.5% duplicate rows) and 51 columns
- **Data types in the file:** int, float, text (object) and dates saved as text

Re-create (or change) the data:
```bash
python generate_data.py
```
Change `N_ROWS` (100,000 - 300,000) or `SEED` at the top of `generate_data.py` to get a bigger / different dataset.

### Problems added on purpose
- **ID / personal columns** (IDs, names, e-mail, phone, date of birth)
- **Messy column names** (`Annual Income ($)`, `CreditScore`, `No. of Dependents`, ...)
- **Short codes** that should be full values (`M`/`F`, `Y`/`N`, `T`/`F`, `FT`/`PT`, ...)
- **Numbers saved as text** (`$1,250.00`, `12,500`, `34 yrs`, `12.5 km`, `15%`)
- **Missing values**, also hidden as text (`N/A`, `unknown`, `?`, `-`, a space) and one **mostly empty** column
- **Outliers** (extreme values) and **impossible values** (age = 150, negative mileage, 9999 codes, ...)
- **Dates saved as text** in different formats
- **Messy text** (`New York`, `NEW YORK`, `  new york`)
- **Categories saved as numbers** (`region_code`)
- **Useless columns**: a constant column, an almost-constant column, a duplicate column
- **Duplicate rows** (~0.5%)

### Columns
| Column in the file | Clean name | Problems |
|---|---|---|
| `Listing ID` | `listing_id` | ID / personal detail -> drop |
| `VIN` | `vin` | ID / personal detail -> drop |
| `Seller Name` | `seller_name` | ID / personal detail -> drop |
| `Seller Phone` | `seller_phone` | ID / personal detail -> drop |
| `Listing Date` | `listing_date` | date saved as text (`%Y-%m-%d`) |
| `Brand` | `brand` | spelling variants: `vw`, `chevy`, `mercedes-benz`, `toyouta`; mixed upper / lower case, extra spaces |
| `Model Trim` | `model_trim` | - |
| `Year` | `model_year` | - |
| `Body Type` | `body_type` | mixed upper / lower case, extra spaces |
| `Fuel Type` | `fuel_type` | short codes: `G`, `D`, `H`, `E` |
| `Transmission` | `transmission` | short codes: `A`, `M` |
| `Drive Type` | `drive_type` | 2% missing |
| `Engine Size (L)` | `engine_size_l` | number saved as text (`12 L`); 2% missing; wrong values: `25` |
| `Cylinders` | `cylinders` | - |
| `HP` | `horsepower` | 3% missing; extreme outliers |
| `Doors` | `doors` | short codes: `two`, `four` |
| `Seats` | `seats` | - |
| `Color` | `color` | 2% missing; mixed upper / lower case, extra spaces |
| `Interior Color` | `interior_color` | mixed upper / lower case, extra spaces |
| `Mileage (km)` | `mileage_km` | number saved as text (`12 km`); 2% missing; wrong values: `-500`, `2,500,000` |
| `Prev. Owners` | `num_previous_owners` | 2% missing; wrong values: `25`, `40` |
| `Accident History` | `accident_history` | short codes: `Y`, `N` |
| `Service History Complete` | `service_history_complete` | short codes: `T`, `F` |
| `City Mpg` | `city_mpg` | 4% missing |
| `Highway Mpg` | `highway_mpg` | 4% missing |
| `Fuel Tank L` | `fuel_tank_l` | - |
| `Weight Kg` | `weight_kg` | number saved as text (`12 kg`); 3% missing; wrong values: `0`, `99,999` |
| `Length Mm` | `length_mm` | - |
| `Width Mm` | `width_mm` | - |
| `Height Mm` | `height_mm` | - |
| `Wheelbase Mm` | `wheelbase_mm` | - |
| `Top Speed Kmh` | `top_speed_kmh` | - |
| `0-100 km/h (s)` | `acceleration_0_100_s` | - |
| `Warranty Remaining Months` | `warranty_remaining_months` | number saved as text (`12 months`) |
| `Has Sunroof` | `has_sunroof` | short codes: `Y`, `N` |
| `Has Navigation` | `has_navigation` | - |
| `Has Leather Seats` | `has_leather_seats` | - |
| `Has Backup Camera` | `has_backup_camera` | - |
| `Has Bluetooth` | `has_bluetooth` | - |
| `Condition` | `condition` | short codes: `EXC`, `GD`, `FR`, `PR`; 3% missing |
| `Seller Type` | `seller_type` | short codes: `DLR`, `PVT` |
| `Region Code` | `region_code` | category saved as a number |
| `Listing Views` | `listing_views` | - |
| `Days On Market` | `days_on_market` | - |
| `State Inspection Passed` | `state_inspection_passed` | short codes: `T`, `F` |
| `Tow Capacity Kg` | `tow_capacity_kg` | 80% missing (mostly empty) |
| `Currency` | `currency` | only one value |
| `Listing Site` | `listing_site` | only one value |
| `Is Salvage Title` | `is_salvage_title` | almost only one value |
| `Seating Capacity` | `seating_capacity` | exact copy of `seats` |
| `Price ($)` | `price` | number saved as text (`$1,250.00`); **TARGET** |

## Files
| File | Description |
|---|---|
| `generate_data.py` | Creates the dataset |
| `car_price_prediction.ipynb` | The full project notebook |
| `data/used_car_listings.csv` | The dataset |
| `data/used_car_listings_metadata.xlsx` | Metadata (data dictionary): description, role, data types, missing values, statistics, codes and problems of every column |
| `models/` | Created when you run the notebook (best model, scaler, selected features) |

## Steps in the notebook
**A. Data cleaning**
- A0 Drop columns that are not useful (IDs, names, ...)
- A1 Rename columns
- A2 Data types: numbers saved as text, messy text, short codes, dates, categories saved as numbers
- A3 Missing values - % per column, drop columns > 50% missing, fill the rest (mean / mode)
- A4 Outliers - box plots, IQR table, wrong values -> median, extreme values -> capping
- A5 Duplicate rows and duplicate columns
- A6 Columns with only one value

**B. EDA**
- B1 Target histogram and box plot
- B2 Correlation heatmap and correlation with the target
- B3 Charts for the independent features (and features vs target)

**C. Feature engineering**
- C1 Binning / new features
- C2 Label encoding and one-hot encoding
- C3 Data splitting: 80% train / 15% test / 5% unseen (random_state = 123)
- C4 Feature scaling (StandardScaler)
- C5 Feature selection (SelectKBest + Random Forest importance)
- C6 20,000-row training sample for comparing / tuning the models

**D. Feature extraction**
- D1 PCA
- D2 Factor Analysis
- D3 Cluster analysis (K-Means with elbow and silhouette)

**E. Machine learning (20 models)**
- E1 Define the models
- E2 Evaluation function (all metrics)
- E3 Cross validation (5 folds)
- E4 Training and testing evaluation
- E5 Hyperparameter dictionary
- E6 Grid search tuning
- E7 Compare the models, select the best, re-train it on the full training data
- E8 Save the best model (`joblib`)
- E9 Load the model
- E10 Predict and evaluate on the 5% unseen data

## How to run
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost joblib jupyter
python generate_data.py
```
Then open `car_price_prediction.ipynb` **from this folder** and run all cells (the full notebook takes about 10-20 minutes).
