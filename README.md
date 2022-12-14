# Generalized-Additive-Models-GAMs-
This project is designed to model road accidents based on traffic variables.
title: "Generalized Additive Models (GAMs)"
author: "AliMirzaei"
date: "11/4/2022"
## Fitting a GAM in R
```r
model <- gam(y ~ s(x1) + s(x2) + te(x3, x4), # formuala describing model
             data = my_data_frame,           # your data
             method = 'REML',                # or 'ML'
             family = gaussian)              # or something more exotic
```

`s()` terms are smooths of one or more variables

`te()` terms are the smooth equivalent of *main effects + interactions*

## Smooth interactions

Two ways to fit smooth interactions

1. Bivariate (or higher order) thin plate splines
    * `s(x, z, bs = 'tp')`
    * Isotropic; single smoothness parameter for the smooth
	* Sensitive to scales of `x` and `z`
2. Tensor product smooths
    * Separate marginal basis for each smooth, separate smoothness parameters
	* Invariant to scales of `x` and `z`
	* Use for interactions when variables are in different units
	* `te(x, z)`

## Tensor product smooths

There are multiple ways to build tensor products in *mgcv*

1. `te(x, z)`
2. `t2(x, z)`
3. `s(x) + s(z) + ti(x, z)`

`te()` is the most general form but not usable in `gamm4::gamm4()` or *brms*

`t2()` is an alternative implementation that does work in `gamm4::gamm4()` or *brms*

`ti()` fits pure smooth interactions; where the main effects of `x` and `z` have been removed from the basis

## Type of smooths

The type of smoother is controlled by the `bs` argument (think *basis*)

The default is a low-rank thin plate spline `bs = 'tp'`

Many others available



* Cubic splines `bs = 'cr'`
* P splines `bs = 'ps'`
* Cyclic splines `bs = 'cc'` or `bs = 'cp'`
* Adaptive splines `bs = 'ad'`
* Random effect `bs = 're'`
* Factor smooths `bs = 'fs'`
* Duchon splines `bs = 'ds'`
* Spline on the sphere `bs = 'sos'`
* MRFs `bs = 'mrf'`
* Soap-film smooth `bs = 'so'`
* Gaussian process `bs = 'gp'`

## Factor smooth interactions

Two ways for factor smooth interactions

1. `by` variable smooths
    * entirely separate smooth function for each level of the factor
	* each has it's own smoothness parameter
	* centred (no group means) so include factor as a fixed effect
	* `y ~ f + s(x, by = f)`
2. `bs = 'fs'` basis
    * smooth function for each level of the function
	* share a common smoothness parameter
	* fully penalized; include group means
	* closer to random effects
	* `y ~ s(x, f, bs = 'fs')`


## A bestiary of conditional distributions

A GAM is just a fancy GLM

Simon Wood & colleagues (2016) have extended the *mgcv* methods to some non-exponential family distributions


* `binomial()`
* `poisson()`
* `quasipoisson()`
* `Gamma()`
* `inverse.gaussian()`
* `nb()`
* `tw()`
* `mvn()`
* `multinom()`
* `betar()`
* `scat()`
* `gaulss()`
* `ziplss()`
* `twlss()`
* `cox.ph()`
* `gamals()`
* `ocat()`

## Libraries

```{r libraries, message=FALSE, warning=FALSE}
if(!require('dplyr')) install.packages('dplyr')
if(!require('corrplot')) install.packages('corrplot')
if(!require('mgcv')) install.packages('mgcv')
if(!require('plotly')) install.packages('plotly')
if(!require('GGally')) install.packages('GGally')
if(!require('gratia')) install.packages('gratia')
if(!require('ggeffects')) install.packages('ggeffects')
if(!require('scico')) install.packages('scico')
if(!require('beepr')) install.packages('beepr')
if(!require('purrr')) install.packages('purrr')
if(!require('tibble')) install.packages('tibble')
if(!require('visibly')) devtools::install_github('m-clark/visibly', upgrade = "never")
if(!require('tidyext')) devtools::install_github('m-clark/tidyext', upgrade = "never")
```
