---
title: "IRT Tutorial 2PL"
author: 'Paul Riesthuis'
authors: [Paul Riesthuis]
slug: IRT2PL
categories: 
  - IRT
tags: []
subtitle: ''
summary: 'In this post, I will show how to conduct a 2PL IRT analysis for data from a DRM study of Otgaar and colleagues'
lastmod: '2024-11-28T16:22:26+02:00'
date: "2024-11-28"
projects: []
output: 
  html_document:
    theme: readable
    code_folding: hide
    toc: TRUE
    toc_float: TRUE
---



## Load Data 
We will again use the DRM list of the study of Otgaar et al (2020) as in [RASCH model](https://paul-riesthuis.netlify.app/post/irt-tutorial/). We will focus on true memory (studied items). 

Let's load the data

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# Load data
df <- read_sav("False_Denials_sav_2.sav")

# Create dataframe
df <- as.data.frame(df)
```
</div>

## 2PL Model  
We will now conduct a 2 parameter logistic model (2PL). A 2PL model  examines:
- the difficulty (i.e., how good does your memory latent trait have to be to answer the answer correctly) of the items 
- and well they discriminate (i.e., how well the item discriminates between people with different latent trait levels of memory). 

In other words, it only estimates 2 parameters (2PL).

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# Create subset for 1st list
df_irt <- df %>% dplyr::select(Table, Sit, Leg, Seat, Couch)

#  First remove first row of NAs
df_irt <- na.omit(df_irt)

# Conduct Rasch model
mod_2pl <- mirt(df_irt,1, itemtype = "2PL", verbose=F, SE=T)

# Results
mod_2pl
```

```
## 
## Call:
## mirt(data = df_irt, model = 1, itemtype = "2PL", SE = T, verbose = F)
## 
## Full-information item factor analysis with 1 factor(s).
## Converged within 1e-04 tolerance after 27 EM iterations.
## mirt version: 1.42 
## M-step optimizer: BFGS 
## EM acceleration: Ramsay 
## Number of rectangular quadrature: 61
## Latent density type: Gaussian 
## 
## Information matrix estimated with method: Oakes
## Second-order test: model is a possible local maximum
## Condition number of information matrix =  35.40356
## 
## Log-likelihood = -352.9353
## Estimated parameters: 10 
## AIC = 725.8706
## BIC = 755.2871; SABIC = 723.6484
## G2 (21) = 18.59, p = 0.6115
## RMSEA = 0, CFI = NaN, TLI = NaN
```
</div>

### Examining fit of individual items
  - This will provide information on how well the datapoints are distributed along the line.

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# Examine item fit
for (i in 1:length(df_irt)){
  ItemPlot <- itemfit(mod_2pl,
                      group.bins = 25,
                      empirical.plot = i,
                      empirical.CI = .95,
                      method = "ML")
  print(ItemPlot)
}
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-2.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-3.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-4.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-5.png" width="672" />
</div>

### Plot the individual items - Difficulty and discriminability
  - Difficulty is given by how high theta (latent trait ability/memory performance) needs to be before endorsing the item (remembering the word)
  - Discriminability is given by how steep the curve is and thus discriminates between different theta levels. 
  
<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# Item specific graphs
plot(mod_2pl, type = "trace")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />
</div>

### Plot together

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
plot(mod_2pl,type= "trace", facet_items=F)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />
</div>


### Coefficients of difficulty (=b) and discriminability (=a)
  - easiness parameter = difficulty/discriminability

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# Get item coefficients
# To get the easy parameter change "IRTpars = F"
mirt::coef(mod_2pl, IRTpars = F, simplify = F)
```

```
## $Table
##            a1     d  g  u
## par     1.602 2.679  0  1
## CI_2.5  0.433 1.570 NA NA
## CI_97.5 2.772 3.789 NA NA
## 
## $Sit
##            a1     d  g  u
## par     1.861 1.945  0  1
## CI_2.5  0.494 0.967 NA NA
## CI_97.5 3.229 2.924 NA NA
## 
## $Leg
##            a1     d  g  u
## par     2.191 1.290  0  1
## CI_2.5  0.290 0.371 NA NA
## CI_97.5 4.092 2.209 NA NA
## 
## $Seat
##            a1     d  g  u
## par     1.013 1.505  0  1
## CI_2.5  0.272 0.944 NA NA
## CI_97.5 1.753 2.067 NA NA
## 
## $Couch
##             a1     d  g  u
## par      0.160 0.887  0  1
## CI_2.5  -0.341 0.519 NA NA
## CI_97.5  0.662 1.254 NA NA
## 
## $GroupPars
##         MEAN_1 COV_11
## par          0      1
## CI_2.5      NA     NA
## CI_97.5     NA     NA
```
</div>

### Information of the test
  - Specifically, this provide a plot at which levels of the latent trait ability the test is most informative
  - Plotting the standard error also indicates where the standard error is largest for the specific test.

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
plot(mod_2pl, type= "infoSE")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="672" />
</div>

### Factor scores

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# Get latent trait scores
df_irt$fscor <- mirt::fscores(mod_2pl)
head(df_irt$fscor)
```

```
##              F1
## [1,] -0.3278053
## [2,] -0.3278053
## [3,]  0.6557888
## [4,] -0.2070695
## [5,] -0.3278053
## [6,]  0.6557888
```
</div>
