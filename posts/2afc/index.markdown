---
title: "2AFC Memory Task Data Analysis"
author: 'Paul Riesthuis'
authors: [Paul Riesthuis]
slug: 2AFC-Memory-Task-Data-Analysis
categories: []
tags: []
subtitle: ''
summary: 'In this post, I will show the possible statistical analyses possible with a 2 alternative forced choice task in a typical memory experiment.'
lastmod: '2023-09-20T16:22:26+02:00'
date: "2023-09-13"
projects: []
output: 
  html_document:
    theme: journal
    code_folding: hide
    toc: TRUE
    toc_float: TRUE
---




---

# 2AFC Memory Task

---

- In this memory task, participants are, for example, asked the following: 
  - Which of the following two words did you see?
    - **Table (old-studied)** or **Chair (new-not studied)**
    
- In this memory task there are two possible outcomes: 
    - hit (H)     =     recognizing studied item
    - False alarm (FA)     =     recognizing non studied item
    

---

## Dataset    
- First, I will create a dataset for this type of memory task with
  - 30 questions
  - hits  with a mean of 2 and a sd of 2.5
  - false alarm with a mean of 10 and sd of 2.5
  
<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# Create variable.
set.seed(2794) #  this is too make sure the results stay the same.
participants <- seq(1,100,1) # This creates a column with 100 participants
hit   <- c(round(c(rnorm(100,20,2.5))))#  this creates a random number of hits that are normally distributed with a mean of 20 and standard deviation of 2.5
FA    <- 30-hit # this create a column with the false alarm rates. 
#put together in dataframe(df)
df <- data.frame(participants, hit,FA)
head(df) #  to show the first 5 participants
```

```
  participants hit FA
1            1  23  7
2            2  18 12
3            3  21  9
4            4  21  9
5            5  19 11
6            6  17 13
```
</div>


---

## Analyzable Statistics

- I will show the common statistics that can be calculated based on this type of dataset.

---

- **Hit rate**           - Rate of choosing the old item out of all items

{{< math >}}
$$
Hit \ rate =  \frac{H}{H + FA}
$$
{{< /math >}}


[Click here for source of formula](https://doi.org/10.3758/BF03207704) 

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
hit_rate <- df$hit/(df$hit+df$FA) # this will give the hit rate for each participant
cat(paste("The hit rate is",format(round(mean(hit_rate),2)))) # The function mean turns the hit rate into the mean for the group
```

```
The hit rate is 0.65
```
</div>

  
  
---

  
- **False Alarm Rate** (FAR)  - Rate of choosing the new item out of all items


{{< math >}}
$$
False \ alarm \ rate =  \frac{FA}{FA + H}
$$
{{< /math >}}
[Click here for source of formula](https://doi.org/10.3758/BF03207704) 

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
FA_rate <- df$FA/(df$hit+df$FA) # this will give the FA rate for each participant
cat(paste("The false alarm rate is",format(round(mean(FA_rate),2))))
```

```
The false alarm rate is 0.35
```
</div>

 
---


### Corrected hit rate

It is possible to calculate a corrected hit rate. However, this depends on several assumptions. For more information [see supplementary information of Brady et al., 2008](https://doi.org/10.1073/pnas.0803390105)

---

### Sensitivity d'

It is possible to calculate or look up the sensitivity d' from the percentage correct. However, this depends on several assumptions. [Click here for formula and table](https://doi.org/10.3758/BF03208311)


---
