# Water Quality Prediction with PySpark and Keras

A machine learning project that predicts water quality across different states in India using big data processing tools and neural networks.

This was developed as part of the **High Volume Data Processing** course at **Pontificia Universidad Javeriana**.

---

## What this project does

We take real water quality measurements from monitoring stations across India and use them to calculate a **Water Quality Index (WQI)** — basically a single score that tells you how clean or dirty the water is. Then we train a neural network to predict that score from raw measurements.

The whole pipeline runs on **PySpark**, which is built for handling large amounts of data, and the model itself is built with **Keras (TensorFlow)**.

---

## The data

The dataset contains measurements from 534 water monitoring stations across 18 Indian states. Each record includes:

| Column | What it means |
|---|---|
| `STATION CODE` | Station ID |
| `LOCATIONS` | Name of the location/river |
| `STATE` | Indian state |
| `TEMP` | Water temperature |
| `DO` | Dissolved oxygen |
| `pH` | pH level |
| `CONDUCTIVITY` | Electrical conductivity |
| `BOD` | Biological oxygen demand |
| `NITRATE_N_NITRITE_N` | Nitrate/Nitrite levels |
| `FECAL_COLIFORM` | Fecal coliform bacteria |

---

## How it works (step by step)

**1. Load and explore the data**
We load the CSV into a PySpark DataFrame and run basic stats on every column to understand what we're working with.

**2. Clean and type-cast the data**
All numeric columns come in as strings, so we cast them to floats. We also remove the `TOTAL_COLIFORM` column since it had too many inconsistencies.

**3. Calculate quality ratings per parameter**
Using reference ranges from the scientific literature, each parameter gets a quality rating (qr) score — for example, a pH between 6.5 and 8.5 gets a score of 100 (good), outside that range it drops.

**4. Apply weights to each parameter**
Each parameter contributes differently to overall water quality. We multiply each rating by its official weight:
- pH: 0.165
- Dissolved Oxygen: 0.281
- Conductivity: 0.234
- BOD: 0.009
- Nitrate/Nitrite: 0.028
- Fecal Coliform: 0.281

**5. Calculate the WQI**
The water quality index is the sum of all weighted scores. We then label each station:

| WQI range | Label |
|---|---|
| 0 – 25 | Excellent (fresh water) |
| 25 – 50 | Good (moderate) |
| 50 – 75 | Low (hard water) |
| 75 – 100 | Very Low (very hard) |
| > 100 | Inadequate (wastewater) |

**6. Visualize by state**
We use GeoPandas to map the water quality across Indian states and a horizontal bar chart to compare WQI values per state.

**7. Train a neural network**
aaaa

---

## Tech stack

- Python 3.9
- PySpark 3.5.8
- Keras/TensorFlow
- Pandas numpy
- Matplotlib seaborn
- GeoPandas shapely

---

## How to run it

> You will need a working spark cluster or local spark installation

1. Clone this repo
2. Install dependencies:
   ```bash
   pip install pyspark pandas numpy seaborn matplotlib geopandas keras tensorflow findspark scikit-learn
   ```
3. Place the water quality CSV and the all the indian shapefile (`.shp`, `.dbf`, `.prj`, `.shx`)
4. Update the file paths inside the notebook to match your setup
5. Run the notebook from the top to the bottom

---

## Results

The model predicts WQI with a mean squared error that steadily drops over the 200 training epochs. The final scatter plot shows the predicted vs actual WQI values on the training set.

**Note importan:** The authors themselves point out that 534 samples is a relatively small dataset for this kind of analysis. The WQI values and model results should be treated as a methodological exercise not as a definitive assessment of water quality in India.

---

## Author

**Mariana Rodriguez**
Pontificia Universidad Javeriana
High Volume Data Processing: Second Test

---

## License

This project is licensed under the MIT License.
