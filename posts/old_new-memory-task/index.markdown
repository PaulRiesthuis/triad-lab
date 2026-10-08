---
title: "New Old Memory Task Data Analysis"
author: 'Paul Riesthuis'
authors: [Paul Riesthuis]
slug: new-old-memory-task-data-analysis
categories: 
  - ROC
tags: []
subtitle: ''
summary: 'In this post, I will show the possible statistical analyses possible with a new - old memory task'
lastmod: '2023-09-22T16:22:26+02:00'
date: "2023-09-22"
projects: []
output: 
  html_document:
    theme: readable
    code_folding: hide
    toc: TRUE
    toc_float: TRUE
---




---

# New_Old (yes/no) Memory task 

In new_old memory task, participants receive an item (new or old).Then asked whether they studied this item (old) or whether they think it is new. In this scenario there are four possible outcomes:

- hit (H)     =     recognizing studied item (indicating *old* when item is *old*)
- false alarm (FA)      =     recognizing non studied item (indicating *old* when item is *new*)
- miss (M)    =     Not recognizing studied item (indicating *new* when item is *old*)
- correct rejection (CR)      = Recognizing that an item was not studied (indicating *new* when item is *new*)
    
## Let's create a dataset. 
- Let's take a study with 30 old and 30 new items (60 in total)

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
set.seed(2794)
participants <- seq(1,100,1) # This creates a column with 100 participants
hit   <- c(round(c(rnorm(100,20,2.5))))#  this creates a random number of hits that are normally distributed with a mean of 20 and standard deviation of 2.5
fa    <- c(round(c(rnorm(100,7.5,2.5))))#  this creates a random number of hits that are normally distributed with a mean of 7.5 and standard deviation of 2.5
miss  <- 30-hit
cr    <- 30-fa
df <- data.frame(participants,hit,fa,miss,cr)
head(df)
```

```
  participants hit fa miss cr
1            1  23  7    7 23
2            2  18  6   12 24
3            3  21  8    9 22
4            4  21  2    9 28
5            5  19  7   11 23
6            6  17  5   13 25
```
</div>

---


## Analyzable Statistics

- I will show the common statistics that can be calculated based on this type of dataset.

---


### **hit rate / True Positive Rate**         
- Rate of choosing the old when old items are presented 
- In other fields also called called sensitivity, but we use sensitivity for d'

\begin{align*}
hit \ rate =  \frac{H}{H + M}\\
\end{align*}

[Click here for source of formula](https://doi.org/10.3758/BF03207704) 

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
df$hit_rate <- df$hit/(df$hit+df$miss)

cat(paste("The hit rate is",format(round(mean(df$hit_rate),2)))) # The function mean turns the hit rate into the mean for the group
```

```
The hit rate is 0.65
```
</div>


---

### **Overall Percent Correct**       
- Percentage of hits and correct rejections together

\begin{align*}
Overall \ Percent \ Correct =  \frac{H + CR}{H + M + FA + CR}\\
\end{align*}

[Click here for source of formula](https://doi.org/10.3758/s13423-022-02179-w) 

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
df$per_corr = (df$hit+df$cr)/(df$hit+df$fa+df$miss+df$cr)*100
cat(paste("The overall percentage correct is",format(round(mean(df$per_corr),2))))
```

```
The overall percentage correct is 70.28
```
</div>



---

### **false alarm rate / False Positive Rate**         
- Rate of choosing the old when new items are presented 

\begin{align*}
False \ alarm \ rate =  \frac{FA}{FA + CR}\\
\end{align*}
[Click here for source of formula](https://doi.org/10.3758/BF03207704) 

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
df$FA_rate <- df$fa/(df$cr+df$fa) # this will give the FA rate for each participant
cat(paste("The false alarm rate is",format(round(mean(df$FA_rate),2))))
```

```
The false alarm rate is 0.24
```
</div>

  
---


### **Specificity**   
- rate of choosing new when new items are presented

\begin{align*}
Specificity =  \frac{CR}{CR + FA}\\
\end{align*}
[Click here for source of formula](https://doi.org/10.3758/BF03207704) 

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
df$spec <- df$cr/(df$fa+df$cr) # this will give the FA rate for each participant
# or 1 - fa_rate
cat(paste("The false alarm rate is",format(round(mean(df$spec),2))))
```

```
The false alarm rate is 0.76
```
</div>

 
---



### **sensitivity (d')**        
- Ability to discriminate between the signal and noise

\begin{align*}
Sensitivity =  Z(H) -  Z(FA)\\
\end{align*}
[Click here for source of formula](https://doi.org/10.3758/BF03207704) 

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
df$sens = qnorm(df$hit_rate) - qnorm(df$FA_rate)
cat(paste("The sensitivity d' is",format(round(mean(df$sens,2)))))
```

```
The sensitivity d' is 1
```
</div>



---


### **Response Bias (c)**       
- Individual's tendency to favor one response alternative over the other

\begin{align*}
Response \ Bias =  -\frac{Z(hit \ rate) +  Z(false \ alarm)}{2}\\
\end{align*} 
[Click here for source of formula](https://doi.org/10.3758/BF03207704) 

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
df$c = -((qnorm(df$hit_rate) + qnorm(df$FA_rate)) / 2)
cat(paste("The response bias c is",format(round(mean(df$c),2))))
```

```
The response bias c is 0.17
```
</div>


---


### **Diagnositicity ratio**       
- The ratio of recognizing "old" items as old to recognizing new items as "old".

\begin{align*}
Diagnositicity \ ratio =  \frac{hit \ rate}{false \ alarm \ rate}\\
\end{align*} 
[Click here for source of formula](https://doi.org/10.3758/BF03207704)

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
df$diag = df$hit_rate/df$FA_rate
cat(paste("The diagnosticity is",format(round(mean(df$diag),2))))
```

```
The diagnosticity is 3.35
```
</div>



----


### **Positive likelihood ratio (PLR)** 
- Probability that participants indicate "old" when the item is "old" divided by the probability that participants indicate "old" for "new" items

\begin{align*}
Positive \ likelihood \ ratio =  \frac{hit \ rate}{1 - false \ alarm \ rate}\\
\end{align*} 
[Click here for source of formula](https://doi.org/10.3758/BF03207704)

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# positive likelihood ratio
df$plr <- df$hit_rate/(1-df$FA_rate)
cat(paste("The PLR is",format(round(mean(df$plr),2))))
```

```
The PLR is 0.87
```
</div>


---


### **Negative likelihood ratio (NLR)** 
-  The probability of participants indicating "new" when the item is "old" divided by the probability that participants indicate "new" when the items is "new" (specificity)

\begin{align*}
Negative \ likelihood \ ratio =  \frac{1- hit \ rate}{specificity}\\
\end{align*} 
[Click here for source of formula](https://doi.org/10.3758/BF03207704)

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">s

``` r
# Negative likelihood ratio
df$nlr <- (1-df$hit_rate)/df$spec
cat(paste("The NLR is",format(round(mean(df$nlr),2))))
```

```
The NLR is 0.47
```
</div>

  
----


### **Positive predictive value (PPV)** 
- Probability that of correctly indicating "old" out of all "old" responses (hits +false alarms)

\begin{align*}
Positive \ predictive \ value =  \frac{H}{H + FA}\\
\end{align*} 
[Click here for source of formula](https://doi.org/10.3389/fpubh.2017.00307)

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# positive likelihood ratio
df$ppv <- df$hit/(df$hit+df$fa) * 100
cat(paste("The PPV is",format(round(mean(df$ppv),2))))
```

```
The PPV is 73.23
```
</div>


---


### **Negative predictive value (NPV)** 
-  The probability of correctly indicating "new" out of all  "new" responses (misses + correct rejections)

\begin{align*}
Negative \ predictive \ value =  \frac{CR}{CR + M}\\
\end{align*} 
[Click here for source of formula](https://doi.org/10.3389/fpubh.2017.00307)

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# Negative likelihood ratio
df$nlr <- df$cr/(df$cr+df$miss) * 100
cat(paste("The NPV is",format(round(mean(df$nlr),2))))
```

```
## The NPV is 68.54
```
</div>
 
