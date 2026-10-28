# Sentinel-1 SAR Oil Spill EDA: Can We Spot the Spills?

Welcome to my Exploratory Data Analysis (EDA) of the **Sentinel-1 SAR Oil Spill Detection Dataset**!

Synthetic Aperture Radar (SAR) is an amazing tool for marine monitoring because it captures the physical texture of the ocean surface. When an oil spill happens, it dampens the ocean's natural waves, creating a smooth patch that looks dark on the radar. The big question for this project: can we easily distinguish these toxic oil spills from natural "look-alikes" (like algae blooms or calm wind patches) just by crunching the pixel numbers?

Let's dive into the data and find out.

## The Data at a Glance

I used the Sentinel-1 dataset from Kaggle, which gave me exactly **5,548 grayscale images** (400x400 pixels) to work with.

* **The "Clean" Ocean (Non-Oil-Spill):** 3,700 images (~66.7%)
* **The Spills (Oil-Spill):** 1,848 images (~33.3%)

## What I Discovered

1. **The Playing Field Isn't Level:** The dataset is imbalanced (about a 2:1 ratio). If a lazy AI just guessed "No Oil" every single time, it would be right 67% of the time. This means if we build a predictive model later, we can't rely on basic accuracy—we'll need smarter metrics like F1-scores or Precision-Recall to keep it honest.
2. **Looks Can Be Deceiving:** Visually, oil spills show up as smooth, dark patches. But nature plays tricks on the radar! Natural calm zones look incredibly similar to actual oil slicks, making this a tough problem to solve.
3. **The Texture Tells a Story:** This was the most interesting statistical find!
* The **Non-Oil** images are actually much more "noisy" and textured (with an average standard deviation of 28.08). This makes sense—open, wavy water has a lot of bright and dark contrast.
* The **Oil-Spill** images are much smoother and uniform (standard deviation of only 14.43).


4. **Spotting the Oddballs:** By running an outlier analysis, I caught some extreme anomalies. Some images were completely pitch black (zero radar bounce across the whole area), while others were blasted with solid white pixels (likely coastal landmasses or metallic double-bounce reflections from giant cargo ships).
5. **The Final Verdict:** Can we separate oil from water just by looking at average pixel brightness? **No.** There is simply too much statistical overlap between real spills and natural look-alikes. To actually catch these spills in the wild, we'll need to step up to Convolutional Neural Networks (CNNs) that can learn the specific shapes, edges, and morphological textures of the slicks, rather than just measuring how dark the image is.

## What's in this Repo?

* `Exploratory_Data_Analysis_of_Sentinel_1_SAR_Oil_Spill_Images.ipynb`: The main attraction. This Jupyter Notebook contains all my code, from data extraction to the final visualization and statistical breakdowns.
* *Note: The dataset itself isn't uploaded here due to size limits, but the notebook is wired to download it directly via the Kaggle API!*

## Tech Stack & Libraries
* **Language:** Python 3.x
* **Data Manipulation & Analysis:** Pandas, NumPy
* **Image Processing:** OpenCV (`cv2`)
* **Data Visualization:** Matplotlib
* **Environment:** Google Colab & Kaggle API

## Visualizations Included in Notebook
* Class distribution bar & pie charts (~2:1 imbalance split)
* Random sample comparison grids (Oil vs. Non-Oil-Spill)
* Extreme pixel intensity outlier inspection grids (Lowest & Highest mean intensities)
* 2x2 statistical analysis panel (Histograms, Box Plots, and Mean vs. Standard Deviation Scatter Plot)

## References & Dataset Attribution
* **Dataset:** Sentinel-1 SAR Oil Spill Detection Dataset (CSIRO / Kaggle)
* **Imaging Modality:** C-band Synthetic Aperture Radar (SAR) imagery from the European Space Agency's Sentinel-1 satellite.

## Want to run it yourself?

1. Pop open the notebook in **Google Colab**.
2. When the first cell asks for it, drop in your `kaggle.json` API token (or set it as an environment variable).
3. Hit "Run All." The script will fetch the data, crunch the numbers, and generate all the charts natively using NumPy, Pandas, Matplotlib, and OpenCV.
