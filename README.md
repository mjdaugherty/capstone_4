### Background
The modern science of astronomy has its roots in ancient Mesopotamia. For thousands of years, humans have been tracking the motions of heavenly bodies and using these ageless cycles as the basis for their calendars, upon which agriculture and the survival of societies crucially depended. Today, astronomers make detailed observations of objects that were unknown to humanity just a few centuries ago.

One such class of objects is galaxies, the brightest of which are the most distant things visible to the naked eye. In the last hundred years or so, scientists have become able to take precise measurements of two quantities that are vital to the study and understanding of galaxies: apparent magnitude and redshift.

Apparent magnitude is a measure of how bright an object appears; it is related, but not identical, to the amount of light produced by that object. Redshift is an effect of the expansion of the universe which causes that light to become stretched out to longer wavelengths as it travels to Earth. By comparing the expected wavelength of light from a galaxy to that actually observed, astronomers can get an idea of how far away that galaxy is. The more redshifted the light, the more distant the source. We also understand intuitively that an object appears fainter with increasing distance; therefore, it is safe to assume that apparent magnitude and redshift are related.

The aim of this project is to explore that relationship and to attempt the construction of a useful model to predict redshift from magnitude measurements in five bands of the electromagnetic spectrum. Although redshifts have been empirically measured for countless astronomical objects, including the tens of thousands of galaxies considered in this project, such measurements are beyond the abilities of most amateur astronomers. Apparent magnitude is easier to measure (though it still requires the use of photographic film or a CCD imaging device) and can be estimated visually to reasonably accuracy by an experienced observer; therefore, a model to predict redshift from apparent magnitude may be of interest to amateur scientists wishing to conduct their own experiments.

### Data Used
In the attempt to create a useful model, several machine learning algorithms were tested on galactic magnitude and redshift data. The data came from [Data Release 16](https://www.sdss.org/dr16/) of the [Sloan Digital Sky Survey](https://www.sdss.org), a project which seeks to make observations of a vast array of astronomical objects and make the data freely accessible to the public for their own scientific endeavors. The data gathered for this project include:

- right ascension - an angular measure of an object's position analogous to longitude (column label: ra)
- declination - counterpart of right ascension, analogous to latitude (dec)
- apparent magnitude (mag.) in the ultraviolet band, centered at 365 nm (u)
- mag. in the green band, centered at 475 nm (g)
- mag. in the red band, centered at 658 nm (r)
- mag. in the infrared band, centered at 806 nm (i) and 900 nm (z)
- redshift

The cleaned data set can be found in the data directory: [./data/sdss_clean.csv](./data/sdss_clean.csv)

### Model Testing, Evaluation and Selection
Five types of model were tested: a multiple linear regression, a polynomial regression of degree 2, another of degree 3, a k-nearest neighbors regression, and a random forest regression. The latter two were tried with a range of hyperparemeters. Each model was evaluated using two metrics, r<sup>2</sup> score and root-mean-square error (RMSE). The most performant model was found to be a random forest regression, which yielded an r<sup>2</sup> of 0.879, higher than all other models, and its RMSE of 0.078 was the lowest of all models tested. The random forest was therefore chosen for redshift prediction. The techniques used and the scores obtained for each model can be found in the [modeling](./03_modeling.ipynb) notebook.

### Assumptions Made and Limitations
The primary assumption made by this project is that the magnitude measurements in the five wavelength bands are separate and unrelated variables. While this is not quite true, the wavelength bands are quite well-separated owing to the filters used to make the magnitude measurements; in other words, there is little (but nonzero) overlap between the light passed by the "green" and the "red" filter, for instance. Using measurements in only five narrow bands of the electromagnetic spectrum imposes limits on the prediction of redshift, but these are the bands that are commonly accessible to amateur astronomers using non-professional equipment.