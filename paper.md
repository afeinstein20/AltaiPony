---
title: 'AltaiPony - Flare science in Kepler, K2 and TESS light curves'
tags:
  - Python
  - astronomy
  - stellar activity
  - Kepler
  - TESS
  - K2
  - stellar flares
authors:
  - name: Ekaterina Ilin
    orcid: 0000-0002-6299-7542
    affiliation: "1, 2" # (Multiple affiliations must be quoted)

affiliations:
 - name: Leibniz-Institute for Astrophysics Potsdam (AIP), An der Sternwarte 16, 14482 Potsdam, Germany
   index: 1
 - name: Institute for Physics and Astronomy, University of Potsdam, Karl-Liebknecht-Str. 24/25, 14476 Potsdam, Germany
   index: 2

date: 03 November 2020
bibliography: paper.bib
---

# Summary 

Flares are unmistakeable signs of stellar magnetic activity, and a key to our understanding
of stellar properties and evolution. They are violent explosions that penetrate 
all layers of a star’s atmosphere, and enhance the overall stellar
brightness by up to orders of magnitude within minutes. We observe them as distinct
signatures in the light curves $-$ repeated measurements of stellar brightness over 
uninterrupted periods of time $-$ of most solar-type and low mass stars. Their rates and energies provide
unique insights to the nature of the stars that produce them. 

Space missions like Kepler [@koch2010], K2 [@howell2014], and TESS [@ricker2014] have
 collected light curves of tens of thousands of flaring stars, for timespans ranging from several weeks
to multiple years. As TESS continues to gather high-cadence data, we developed `AltaiPony` to aid
astronomers who require accurately characterized flare samples for their research. 
`AltaiPony` is a toolbox for statistical flares studies on photometric time series from these missions, including flare search 
and characterization, injection-recovery diagnostics, and statistical analysis 
of flare frequency distributions along with extensive 
documentation and Jupyter-based tutorials.

# Functionality

`AltaiPony` is based on `lightkurve` [@lightkurve2018], and can access most 
features that are implemented in it, which makes it an accessible tool for new 
users who are already familiar with the software. `lightkurve` is a versatile
Python package for light curve handling that includes visualization, basic tools for 
de-trending, transit detection, and asteroseismology. It is the most widely 
used software for handling Kepler, K2, and TESS data. `AltaiPony` inherits its main
class `FlareLightCurve` directly from `lightkurve`'s `LightCurve`, and its mission-specific
derivatives.

`AltaiPony` was designed to be used by astronomers as a one stop shop 
solution that covers the essential steps of a typical flare study. We begin with
adaptations of common de-trending tools like the Savitzky-Golay filter
from `lightkurve.flatten()`, and K2SC [@aigrain2016]. We tailored them 
to preserve flare signal, and remove astrophysical and instrumental variability. 
The design also allows users to add their own custom de-trending functions to `FlareLightCurve.detrend()`.

After de-trending, `FlareLightCurve.find_flares()` returns the occurrence times, amplitudes, durations, 
and relative energies of all flares above the detection threshold in the residual light curve using an adjustable iterative 
sigma-clipping procedure to identify candidate events[@davenport2016]. 

Usually, the measured flare amplitudes and durations differ systematically from their intrinsic properties 
due to the astrophysical and instrumental properties of the light curves in which they were found. 
Therefore, `AltaiPony` features an injection-recovery pipeline for 
synthetic flares with visualization aids that quantifies the cumulated effects
 of noise patterns, time sampling, de-trending and flare finding procedure of choice. 
`FlareLightCurve.sample_flare_recovery()` generates the synthetic data and performs
the full flare search. The resulting sample can be used to determine the recovery 
probability and energy bias of the candidates in the original light curve.

Flare frequency distributions (FFDs) follow a power law in energy $E$:

\begin{equation}
f(>E) = \dfrac{\beta}{\alpha - 1}E^{-\alpha + 1}
\end{equation}

To estimate $\alpha$ and $\beta$ and their respective uncertainties for a given sample
 of flares, `AltaiPony` includes the analysis class `FFD` that includes a fully Bayesian framework [@wheatland2004] that 
incorporates both the power law nature of FFDs, and the exponential flare waiting times 
to predict flare frequencies, and uses emcee [@emcee2013] to sample from the posterior distribution using 
the Markov Chain Monte Carlo method. As a quick alternative, we also implemented a modified maximum likelihood estimator 
[@maschberger2009] for $\alpha$, and a least-squares fit to $\beta$ with 
bootstrapped uncertainties.

# Applications

`AltaiPony` has already been used in peer-reviewed publications that studied flares 
in K2 [@ilin2019; @ilin2020] and TESS [@ramsay2020] light curves, and remains under active development.

# Acknowledgements

I acknowledge valuable contributions from Michael Gully-Santiago and Geert Barentsen,
who offered advice and hands-on support in the early development
stages of the project.

# References
