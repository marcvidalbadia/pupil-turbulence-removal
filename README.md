Supplementary Online Material for M. Vidal, K. E. Onderdijk, A. M. Aguilera, J. Six, P-J. Maes and T. H. Fritz, M. Leman.  "Cholinergic-related pupil activity reflects level of emotionality during motor performance", 2023. (Accepted at the European Journal of Neuroscience)

## Unsupervised Artifact Removal in Pupillometry for Cognitive-Motor Analysis

This repository contains a set of routines written in R for artefact removal in pupillometry recordings. The main algorithm, described in [1], detects and suppresses both slow and high-frequency fluctuations caused by changes in retinal luminance (e.g., due to blinks). We refer to this artifactual activity as responses to ocular events (ROE). The algorithm is fully unsupervised (it does not require prior labelling of artefacts) and is specifically designed to enhance the estimation of cognitively driven pupil responses during complex motor tasks.

To show the performance of our methods, we recorded a participant who was asked to blink four times synchronised with an auditory beat (of 2 s duration) in two time frames (shaded in red, see Fig. 1) separated by pauses. The beat appeared 4 times with a different sound during the pauses to alert the participant of the beginning/end of the blinking task. Blinks were intentionally performed longer to see their effect on the signal. During the pauses, eventual (faster) blinks also occurred.  We recorded pupil activity in a dark environment where, a time before the beginning of the blinking task, a white cross was projected to the scene until the end of the recording. This produced a slow ROE not related to blinking activity, rather to a change in constant luminance. 

In raw data, blinks or partial occlusions of the pupil often appear as NA, 0, or even negative values. Partial occlusions are not always automatically detected and should be manually examined after the removal of clearly invalid points. Typically, all such artefactual segments are removed starting 100 ms before eyelid closure and extending to 200 ms after reopening. We implemented this procedure in the R function `pup.med` with three different kinds of data imputation: Gaussian [3], t-Student [2] and Kalman filtering [3]. Their stochastic performance is shown in Fig. 1.

![Fig. 1](https://github.com/m-vidal/pupil-turbulence-removal/blob/main/plots/P1.jpg)
#### Fig. 1. Raw pupil signal and reconstructed signal using data imputation.

The removal of ROE is conducted in two steps, which allows to detect different kinds of ROE (see [1] for further details). Results of this procedure are available by running the R script bellow.

## Methods in practice
```R
#Download the repository and copy this code chunk to a new R script
#setwd(~/pupil-turbulence-removal) #set the working directory 
#Uncomment to install the packages if necessary
#install.packages('signal')
#install.packages('imputeFin')
#install.packages('imputeTS')
source('fn.R') #load functions to the environment
y <- read.csv('blinks.csv')$x #read the data
ry <- pup.med(y, ant=0.1, post=0.2, method="t-Student")
y <- ry$Pupildata #reconstructed pupil signal

#Plot the ROE corrected pupil data
duration <- length(y)/30
arg <- seq(0,duration,duration/length(y))[1:length(y)]
plot(arg, y, ylab='Pupil diameter', xlab='Time (s)', type='l', main='ROE correction')
lines(arg, pup.turbulence(y, sd.factor.high=3*exp(-ry$Blink_rate), LPF=NA), col='orange')
lines(arg, pup.turbulence(y, sd.factor.high=3*exp(-ry$Blink_rate), LPF=1.6), col='darkgreen', lwd=2)
legend('topright',legend=c('Artifact corrected signal', 'ROE corrected signal', 'Final smoothing'),
       col=c('black', 'orange', 'darkgreen'), lwd=c(1,1,2), cex=0.8, bg='lightblue')

y <- pup.turbulence(y, sd.factor.high=3*exp(-ry$Blink_rate), LPF=1.6, Nf=15) #Nf is the Nyquist frequency
```

To isolate cognitively relevant pupil responses, the R function `pup.turbulence` identifies and removes fluctuations caused by non-cognitive factors, such as subtle luminance shifts or motor-related noise [1]. The dispersion hyperparameter for slow turbulences is set by default to 3 (`sd.factor.low=3`); we observed that between 3-5 provides optimal results under constant luminance conditions. For high-frequency turbulences, such as ROE due to blinks, we recommend modeling the hyperparameter as the exponential `3*exp(-ry$Blink_rate)`, where `ry$Blink_rate` is an estimation of the blink rate calculated in the function `pup.med`. By default, the parameter is set to 3. We recommend to perform the final smoothing step using a cutoff frequency between 1 Hz (`LPF=1`) and 4 Hz (`LPF=4`). If `LPF=NA` (default) no smoothing is conducted. The function `pup.turbulence` is configured for a 30 Hz sampling frequency.

## References

[1] M. Vidal, K. E. Onderdijk, A. M. Aguilera, J. Six, P-J. Maes and T. H. Fritz, M. Leman. "Cholinergic-related pupil activity reflects level of emotionality during motor performance", 2022.

[2] J. Liu, S. Kumar and D. P. Palomar, "Parameter Estimation of Heavy-Tailed AR Model With Missing Data Via Stochastic EM," in IEEE Transactions on Signal Processing, vol. 67, no. 8, pp. 2159-2172, 2019, doi: 10.1109/TSP.2019.2899816.

[3] R. Zhou, J. Liu, S. Kumar and D. P. Palomar, "Student's  t  VAR Modeling With Missing Data Via Stochastic EM and Gibbs Sampling," in IEEE Transactions on Signal Processing, vol. 68, pp. 6198-6211, 2020, doi: 10.1109/TSP.2020.3033378.

[4] R. J. Hyndman and Y. Khandakar (2008). "Automatic time series forecasting: the forecast package for R". Journal of Statistical Software, 26(3).
