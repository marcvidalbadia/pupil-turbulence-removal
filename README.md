## Removal of turbulent flows in the pupil signal

This repository contains a set of routines written in R for pupilometry artifact removal. The main algorithm, described in [1], detects slow and high-frequency turbulences produced by changes in retinal luminance (e.g. due to blinks) and removes them without affecting pupil activity related to cognitive processes. We call this turbulent activity “responses to ocular events” (ROEs). The algorithm is unsupervised, meaning that it does not rely on any artifact benchmark. It was specifically designed to enhance the estimation of cognitive-related pupil activity during complex motor tasks.

To show the performance of our methods, we recorded a participant who was asked to blink four times synchronised with an auditory beat (2 s) in two time frames (shaded in red, see Fig. 1) separated by pauses. The beat also appeared 4 times with a different sound during the pauses to alert the participant of the beginning/end of the blinking task. Blinks were intentionally performed longer to see their effect on the signal. During the pauses, eventual (faster) blinks also occurred.  We recorded pupil activity in a dark environment where, a time before the beginning of the task, a white cross was projected to the scene: this produced a slow ROE not related to blinking activity. 


![Fig. 1](https://github.com/m-vidal/pupil-turbulence-removal/blob/main/plots/plot1.jpg)
#### Fig. 1. Raw pupil signal and blink corrected signal using data imputation.

Blinks can be detected as NA, 0 or even negative time observations in the raw signal. Partial occlusions of the pupil are not easily detectable and usually, one should examine the data after the removal of these points to check for other unusual activity. These observations are usually removed from 100 ms before until 200 ms after the rapid closing of the eyelid. To solve the issue of the missing observations, we implemented in the R function `pup.med` three different kinds of data imputation: Gaussian [3], t-Student [2] and Kalman filtering [3]. Their performance is shown in Fig. 1.

![Fig. 1](https://github.com/m-vidal/pupil-turbulence-removal/blob/main/plots/plot2b.jpg)
#### Fig. 2. ROE corrected pupil signal.
Please, install  the following packages:

```R
install.packages("imputeFin")
install.packages("imputeTS")
```
--

[1] M. Vidal, K. E. Onderdijk, A. M. Aguilera, J. Six, P-J. Maes and T. H. Fritz, M. Leman. "Cholinergic-related pupil activity reflects level of emotionality during motor performance", 2022.

[2] R. Zhou, J. Liu, S. Kumar and D. P. Palomar, "Student's  t  VAR Modeling With Missing Data Via Stochastic EM and Gibbs Sampling," in IEEE Transactions on Signal Processing, vol. 68, pp. 6198-6211, 2020, doi: 10.1109/TSP.2020.3033378.

[3] J. Liu, S. Kumar and D. P. Palomar, "Parameter Estimation of Heavy-Tailed AR Model With Missing Data Via Stochastic EM," in IEEE Transactions on Signal Processing, vol. 67, no. 8, pp. 2159-2172, 2019, doi: 10.1109/TSP.2019.2899816.
