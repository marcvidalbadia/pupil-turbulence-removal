## Removal of turbulent flows in the pupil signal

This repository contains a set of routines for pupilometry artifact removal written in R. The main algorithm, described in [1], detects slow and high-frequency turbulences produced by changes in retinal luminance (e.g. due to blinks) and removes them without affecting pupil activity related to cognitive processes. We call this turbulent behavior “responses to ocular events” (ROEs). The algorithm is unsupervised, meaning that it does not rely on any artifact benchmark. To optimize the procedure, the user has to select a dispersion hyperparameter. We model this parameter as a negative exponential that depends on the blink rate, although by default it is set to 3. The algorithm was specifically designed to enhance the estimation of cognitive-related pupil activity during complex motor tasks.


Please, install  the following packages:

```R
install.packages("imputeFin")
install.packages("imputeTS")
```
--

[1] M. Vidal, K. E. Onderdijk, A. M. Aguilera, J. Six, P-J. Maes and T. H. Fritz, M. Leman. "Cholinergic-related pupil activity reflects level of emotionality during motor performance", 2022.

[2] R. Zhou, J. Liu, S. Kumar and D. P. Palomar, "Student's  t  VAR Modeling With Missing Data Via Stochastic EM and Gibbs Sampling," in IEEE Transactions on Signal Processing, vol. 68, pp. 6198-6211, 2020, doi: 10.1109/TSP.2020.3033378.
