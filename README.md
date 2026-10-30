# Sentinel-1 SAR Oil Spill EDA

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Mittal-Sankalp/EDA-Sentinel-1-SAR-Oil-Spill-Images/blob/main/Exploratory_Data_Analysis_of_Sentinel_1_SAR_Oil_Spill_Images.ipynb)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

An exploratory data analysis (EDA) of Sentinel-1 satellite radar images, asking one question: **can simple pixel statistics tell real oil spills apart from natural look-alikes?**

## Background

Synthetic Aperture Radar (SAR) images the sea surface by recording how much radar signal bounces back to the satellite. Rough, wavy water scatters a lot of signal back and looks bright. Oil damps the small waves, leaving a smooth patch that looks dark. Natural calm zones, such as low-wind areas and algae blooms, are smooth too, so they can look just like oil. This makes automatic oil spill detection hard.

## Dataset

The notebook downloads the [Sentinel-1 SAR Oil Spill Detection Dataset](https://www.kaggle.com/datasets/harikrishnacs/sentinel-1-sar-oil-spill-detection-dataset) from Kaggle (licensed CC-BY-SA-4.0). It contains **5,548 grayscale images** (400×400 pixels), labelled by folder:

| Class | Images | Share |
|---|---|---|
| Non-oil-spill (`Class_0`) | 3,700 | 66.7% |
| Oil-spill (`Class_1`) | 1,848 | 33.3% |

The images are not stored in this repository. The notebook downloads them through the Kaggle API.

## Key Findings

1. **The classes are imbalanced (about 2:1).** A model that always answered "no oil" would be right 67% of the time while finding no spills, so a later model should be judged with precision, recall and F1, not accuracy alone.
2. **Oil images are smoother.** Their average pixel standard deviation (contrast) is 14.4, against 28.1 for non-oil images. Open, wavy water has more bright-dark variation than an oil-covered surface.
3. **Brightness alone does not separate the classes.** Average brightness is 122.1 for oil and 137.7 for non-oil on a 0-255 scale, and the distributions overlap heavily. Contrast is more informative, but its distributions overlap too.
4. **There are extreme images.** Some are completely black, and some contain large areas of solid white. These may be land, ships or sensor artefacts; this has not been verified.
5. **Next step: a model that reads shapes.** Per-image averages are not enough, so the natural follow-up is a convolutional neural network (CNN) that learns edges and textures.

## Notebook Contents

`Exploratory_Data_Analysis_of_Sentinel_1_SAR_Oil_Spill_Images.ipynb` covers:

- Downloading the dataset with the Kaggle API
- Extracting per-image statistics (mean, standard deviation, minimum, maximum) into a pandas DataFrame
- Class distribution bar and pie charts
- Random sample grids of oil and non-oil images
- The five darkest and five brightest images (extreme-intensity outliers)
- A 2×2 statistical panel: histograms of mean and standard deviation, a box plot, and a mean-vs-standard-deviation scatter plot
- An IQR outlier summary per class

## Tech Stack

Python · pandas · NumPy · OpenCV · Matplotlib · Kaggle API · Google Colab

## Running the Notebook

The notebook reads its Kaggle credentials from Google Colab secrets, so it is meant to run in Colab.

1. Click the **Open in Colab** badge above.
2. Create an API token in your Kaggle account settings.
3. In Colab, open **Secrets** (the key icon in the left sidebar), add a secret named `KAGGLE_API_TOKEN` with your token as the value, and turn on notebook access.
4. Choose **Runtime → Run all**. The notebook downloads and extracts the dataset, then generates the charts.

To run it elsewhere, replace the `google.colab.userdata` lines in the first code cell with your own way of setting the `KAGGLE_API_TOKEN` environment variable.

## Limitations and Next Steps

- Only summary statistics were analysed; no classifier has been trained yet. A good first step is a logistic regression on mean and standard deviation, as a baseline for a CNN.
- If the images are tiles cut from larger satellite scenes, train and test sets should be split so that tiles from the same scene stay together.

## Acknowledgements

Imagery from the European Space Agency's Sentinel-1 mission (C-band SAR). Dataset on Kaggle by [harikrishnacs](https://www.kaggle.com/datasets/harikrishnacs/sentinel-1-sar-oil-spill-detection-dataset).

## License

Released under the [MIT License](LICENSE). The dataset has its own license (CC-BY-SA-4.0).
