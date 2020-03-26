# Predicting Galactic Redshift from Apparent Magnitude
Michael Daugherty\
DSI-10 DC\
Instructors: Adi Bronshtein, Chuck Dye

### Contents

- The [data](./data) directory contains the original, unprocessed data, a cleaned version used for modeling, and predictions from the best model.
- The [code](./code) directory contains Jupyter notebooks with the code that was used for data collection, data cleaning, modeling and creating visualizations.
- The [assets](./assets) directory contains the slides, images and visualizations used to present project findings.

### Background and Objective

The modern science of astronomy has its roots in ancient Mesopotamia. For thousands of years, humans have been tracking the motions of heavenly bodies and using these cycles as the basis for their calendars, upon which agriculture and the survival of civilizations crucially depended. The purpose of observational astronomy has gradually shifted from the survival of society to the advancement of human understanding. Today, astronomers make detailed observations of objects that were entirely unknown just a few centuries ago.

One such class of objects is galaxies, the brightest of which are the most distant structures visible to the naked eye. In the last hundred years or so, scientists have become able to take precise measurements of two quantities that are vital to the study and understanding of galaxies: apparent magnitude and redshift.

Apparent magnitude is a measure of how bright an object appears; it is related, but not identical, to the amount of light produced by that object. Redshift is an effect of the expansion of the universe which causes that light to become stretched out to longer wavelengths as it travels to Earth. By comparing the expected wavelength of light from a galaxy to that actually observed, astronomers can get an idea of how far away that galaxy is. The more redshifted the light, the more distant the source. We also understand intuitively that an object appears fainter with increasing distance; therefore, it is safe to assume that apparent magnitude and redshift are related.

The aim of this project is to explore that relationship and to construct a useful model to predict redshift from magnitude measurements in five bands of the electromagnetic spectrum. Although redshifts have been empirically measured for countless astronomical objects, including the tens of thousands of galaxies considered for this project, such measurements require sophisticated equipment that is too expensive for most amateur astronomers to afford. Apparent magnitude is less expensive to measure (though it still usually requires the use of a CCD imaging device) and can even be estimated visually to reasonable accuracy by an experienced observer; therefore, a model to predict redshift from apparent magnitude may be of interest to amateur scientists wishing to conduct their own experiments.

### Data Used

The data used during the attempt to build such a model came from [Data Release 16](https://www.sdss.org/dr16/) of the [Sloan Digital Sky Survey](https://www.sdss.org), a project which seeks to make observations of a vast array of astronomical objects and make the data freely accessible to the public for their own scientific endeavors. The information gathered for this project include:

- right ascension - an angular measure of an object's position analogous to longitude (column label: ra)
- declination - counterpart of right ascension, analogous to latitude (dec)
- apparent magnitude (mag.) in the ultraviolet band, centered at 365 nm (u)
- mag. in the green band, centered at 475 nm (g)
- mag. in the red band, centered at 658 nm (r)
- mag. in the infrared band, centered at 806 nm (i) and 900 nm (z)
- redshift

The collection, exploratory analysis and cleaning of the data are documented here:

- [Collection](./code/01_data_collection.ipynb)
- [EDA and Cleaning](./code/02_eda_and_cleaning.ipynb)

and the [original](./data/sdss.csv) and [cleaned](./data/sdss_clean.csv) datasets and [model predictions](./data/predictions.csv) are located in the [data](./data) directory.

### Model Testing, Evaluation and Selection

Five machine learning algorithms were tested: a multiple linear regression, a polynomial regression of degree 2, another of degree 3, a k-nearest neighbors regression, and a random forest regression. The latter two were tried with a range of hyperparemeters. Each model was evaluated using two metrics, r<sup>2</sup> score and root-mean-square error (RMSE). A random forest model with hyperparameters chosen to maximize r<sup>2</sup> score, which also gave a low RMSE, was selected. The techniques used and the scores obtained for each model can be found in the [modeling](./03_modeling.ipynb) notebook. The following is a summary of findings from these tests:

| Model                          | R<sup>2</sup> (test data) | RMSE (test data) |
|--------------------------------|---------------------------|------------------|
| Multiple Linear Regression     | 0.832                     | 0.092            |
| Degree 2 Polynomial Regression | 0.854                     | 0.086            |
| Degree 3 Polynomial Regression | 0.838                     | 0.085            |
| K-Nearest Neighbors            | 0.859                     | 0.080            |
| Random Forest                  | 0.879                     | 0.080            |

### Assumptions and Limitations

The primary assumption on which this work is based is that the magnitude measurements in the five wavelength bands are separate and unrelated variables. While this is not quite true, the wavelength bands are quite well-separated owing to the use of filters to make the magnitude measurements; in other words, there is little (but nonzero) overlap between the light passed by the "green" and the "red" filter, for instance. Using measurements in only five narrow bands of the electromagnetic spectrum imposes limits on the performance of any model that seeks to use them as predictors, but these bands cover the range of wavelengths that are accessible to amateur scientists without access to professional equipment.\
In addition, the statistical and computational techniques used in this project imply an assumption that the data are of sufficient quality to justify the use of such techniques. There are many examples of valuable science being done with SDSS data; see for example [Salim et al 2016](https://arxiv.org/pdf/1610.00712.pdf), [Tegmark et al 2006](https://arxiv.org/abs/astro-ph/0608632). Technical information about the data collected and the techniques used by SDSS can be found at https://www.sdss.org/science/technical_publications/.

### Necessary Libraries

- [NumPy](https://numpy.org/)
- [pandas](https://pandas.pydata.org/)
- [seaborn](https://seaborn.pydata.org/)
- [scikit-learn](https://scikit-learn.org/stable/index.html)
- [plotly](https://pypi.org/project/plotly/)

### Conclusions and Recommendations

The best of the models tested, a random forest regression, produced acceptable results, though a more specialized model could probably perform better on a subset of the data. For example, by limiting the magnitude values of the galaxies on which a model was trained to those less than, say, 15, one could perhaps obtain better redshift predictions for galaxies in that magnitude range, at the expense of greater error for higher magnitudes and redshifts. Looking at the plots of predicted redshift values [1](./assets/predicted_ra), [2](./assets/predicted_dec), which are limited to measured redshifts < 0.2 to avoid crowding, we see that the predicted values are qualitatively similar to the measured values, but there are still blue dots surrounded by red and vice-versa. A model optimized for predictions in this realm could fix some of these errors. Recommendations for the future include training more specialized models, especially for lower redshifts, since these correspond to brighter galaxies able to be imaged by amateur astronomers even in the presence of light pollution.
