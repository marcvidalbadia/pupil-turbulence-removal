**This repository contains the source code for the ROE (response to ocular event) algorithm in [1].**

## Removal of responses to ocular events

This repository contains a set of routines for pupilometry artifact removal written in R. The main algorithm described in [1] detects slow and high-frequency turbulences produced by changes in retinal luminance and removes them without affecting the high frequency activity of the pupil. We call this turbulent activity “responses to ocular events” (ROEs). The algorithm is unsupervised, meaning that it does not rely on any artifact benchmark. To optimise the procedure, the user has to select a	 dispersion hyperparameter. By default, we advise using `sd.factor= 3` (conservative), although we model this parameter as a negative exponential that depends on the blink rate. The algorithm was specifically designed to enhance the estimation of cognitive-related pupil activity during complex motor tasks.


--

[1] M. Vidal, K. E. Onderdijk, A. M. Aguilera, J. Six, P-J. Maes and T. H. Fritz, M. Leman. "Cholinergic-related pupil activity reflects level of emotionality during motor performance", 2022.

[2] R. Zhou, J. Liu, S. Kumar and D. P. Palomar, "Student's  t  VAR Modeling With Missing Data Via Stochastic EM and Gibbs Sampling," in IEEE Transactions on Signal Processing, vol. 68, pp. 6198-6211, 2020, doi: 10.1109/TSP.2020.3033378.
