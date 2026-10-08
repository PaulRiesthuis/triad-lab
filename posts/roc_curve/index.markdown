---
title: "ROC Curve Brady et al. (2022)"
author: 'Paul Riesthuis'
authors: [Paul Riesthuis]
slug: roc-curve-brady
categories:
  - ROC
tags: []
subtitle: ''
summary: 'In this post, I will conduct the ROC analyses performed in the article of Brady et al. (2022) '
lastmod: '2023-09-23T16:22:26+02:00'
date: "2023-09-23"
projects: []
output: 
  html_document:
    theme: readable
    code_folding: hide
    toc: TRUE
    toc_float: TRUE
---

<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>

# ROC cuve analysis of [Brady et al. (2022)](https://doi.org/10.3758/s13423-022-02179-w). 

- In this blogpost, I will show how to construct the datasets of Tim and John and the Receiver Operator Characteristic (ROC) curves provided in the article of Brady et al. (2022)
  - Then, I will show the following:
    - How to calculate common statistics that can be calculated based on this type of dataset. Most statistics are similar to the ones shown at the page about [“old” and “new” memory task](https://paul-riesthuis.netlify.app/post/new-old-memory-task-data-analysis/)
    - How to construct (ROC) curves by hand and via logistic regression.
    - How to calculate the area under the ROC curve.
    - How to calculate confidence intervals for the AUC
    - How to calculate partial area under the curve.

## Datasets constructed by hand

- Dataset of Tim

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
old_items_tim <- c(50,50,50,150,500,200)
new_items_tim <- c(250,250,400,50,25,25)
confidence <- c("extreme weak","very weak", "fairly weak", "fairly strong", "very strong", "extreme strong")
df_tim <- data.frame(old_items_tim, new_items_tim, confidence)
nice_table(df_tim,
           title= "Dataset for Tim")
```

<div class="tabwid"><style>.cl-9a700c3e{table-layout:auto;}.cl-9a637320{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9a63733e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9a689f9e{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9a689fa8{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9a68c64a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a68c654{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a68c655{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a68c65e{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a68c65f{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a68c660{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-9a700c3e'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-9a68c64a"><p class="cl-9a689f9e"><span class="cl-9a637320">Dataset for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-9a68c654"><p class="cl-9a689fa8"><span class="cl-9a63733e">old_items_tim</span></p></th><th class="cl-9a68c654"><p class="cl-9a689fa8"><span class="cl-9a63733e">new_items_tim</span></p></th><th class="cl-9a68c654"><p class="cl-9a689fa8"><span class="cl-9a63733e">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-9a68c655"><p class="cl-9a689f9e"><span class="cl-9a63733e">50.00</span></p></td><td class="cl-9a68c65e"><p class="cl-9a689fa8"><span class="cl-9a63733e">250.00</span></p></td><td class="cl-9a68c65e"><p class="cl-9a689fa8"><span class="cl-9a63733e">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a68c655"><p class="cl-9a689f9e"><span class="cl-9a63733e">50.00</span></p></td><td class="cl-9a68c65e"><p class="cl-9a689fa8"><span class="cl-9a63733e">250.00</span></p></td><td class="cl-9a68c65e"><p class="cl-9a689fa8"><span class="cl-9a63733e">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a68c655"><p class="cl-9a689f9e"><span class="cl-9a63733e">50.00</span></p></td><td class="cl-9a68c65e"><p class="cl-9a689fa8"><span class="cl-9a63733e">400.00</span></p></td><td class="cl-9a68c65e"><p class="cl-9a689fa8"><span class="cl-9a63733e">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a68c655"><p class="cl-9a689f9e"><span class="cl-9a63733e">150.00</span></p></td><td class="cl-9a68c65e"><p class="cl-9a689fa8"><span class="cl-9a63733e">50.00</span></p></td><td class="cl-9a68c65e"><p class="cl-9a689fa8"><span class="cl-9a63733e">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a68c655"><p class="cl-9a689f9e"><span class="cl-9a63733e">500.00</span></p></td><td class="cl-9a68c65e"><p class="cl-9a689fa8"><span class="cl-9a63733e">25.00</span></p></td><td class="cl-9a68c65e"><p class="cl-9a689fa8"><span class="cl-9a63733e">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a68c65f"><p class="cl-9a689f9e"><span class="cl-9a63733e">200.00</span></p></td><td class="cl-9a68c660"><p class="cl-9a689fa8"><span class="cl-9a63733e">25.00</span></p></td><td class="cl-9a68c660"><p class="cl-9a689fa8"><span class="cl-9a63733e">extreme strong</span></p></td></tr></tbody></table></div>

</div>

- Dataset John

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
old_items_john <- c(50,75,75,500,200,100)
new_items_john <- c(100,400,300,50,75,75) # changed very strong to 75 instead of 25 to have equal number items between tim and john
df_john <- data.frame(old_items_john, new_items_john, confidence)
nice_table(df_john,
           title = "Dataset for John")
```

<div class="tabwid"><style>.cl-9a8a2542{table-layout:auto;}.cl-9a83cefe{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9a83cf08{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9a860250{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9a860251{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9a861c86{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a861c87{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a861c90{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a861c91{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a861c92{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a861c93{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-9a8a2542'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-9a861c86"><p class="cl-9a860250"><span class="cl-9a83cefe">Dataset for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-9a861c87"><p class="cl-9a860251"><span class="cl-9a83cf08">old_items_john</span></p></th><th class="cl-9a861c87"><p class="cl-9a860251"><span class="cl-9a83cf08">new_items_john</span></p></th><th class="cl-9a861c87"><p class="cl-9a860251"><span class="cl-9a83cf08">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-9a861c90"><p class="cl-9a860250"><span class="cl-9a83cf08">50.00</span></p></td><td class="cl-9a861c91"><p class="cl-9a860251"><span class="cl-9a83cf08">100.00</span></p></td><td class="cl-9a861c91"><p class="cl-9a860251"><span class="cl-9a83cf08">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a861c90"><p class="cl-9a860250"><span class="cl-9a83cf08">75.00</span></p></td><td class="cl-9a861c91"><p class="cl-9a860251"><span class="cl-9a83cf08">400.00</span></p></td><td class="cl-9a861c91"><p class="cl-9a860251"><span class="cl-9a83cf08">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a861c90"><p class="cl-9a860250"><span class="cl-9a83cf08">75.00</span></p></td><td class="cl-9a861c91"><p class="cl-9a860251"><span class="cl-9a83cf08">300.00</span></p></td><td class="cl-9a861c91"><p class="cl-9a860251"><span class="cl-9a83cf08">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a861c90"><p class="cl-9a860250"><span class="cl-9a83cf08">500.00</span></p></td><td class="cl-9a861c91"><p class="cl-9a860251"><span class="cl-9a83cf08">50.00</span></p></td><td class="cl-9a861c91"><p class="cl-9a860251"><span class="cl-9a83cf08">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a861c90"><p class="cl-9a860250"><span class="cl-9a83cf08">200.00</span></p></td><td class="cl-9a861c91"><p class="cl-9a860251"><span class="cl-9a83cf08">75.00</span></p></td><td class="cl-9a861c91"><p class="cl-9a860251"><span class="cl-9a83cf08">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a861c92"><p class="cl-9a860250"><span class="cl-9a83cf08">100.00</span></p></td><td class="cl-9a861c93"><p class="cl-9a860251"><span class="cl-9a83cf08">75.00</span></p></td><td class="cl-9a861c93"><p class="cl-9a860251"><span class="cl-9a83cf08">extreme strong</span></p></td></tr></tbody></table></div>

</div>

------------------------------------------------------------------------

## Add hit rate and false alarm rate

------------------------------------------------------------------------

- **Tim**

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
#hit rates Tim
tot_items_tim <- sum(old_items_tim) # to get the total amount of items
df_tim$hit <- c(old_items_tim[1]/tot_items_tim, 
         (old_items_tim[1]+old_items_tim[2])/tot_items_tim,
         (old_items_tim[1]+old_items_tim[2]+old_items_tim[3])/tot_items_tim,
         (old_items_tim[1]+old_items_tim[2]+old_items_tim[3]+old_items_tim[4])/tot_items_tim,
         (old_items_tim[1]+old_items_tim[2]+old_items_tim[3]+old_items_tim[4]+old_items_tim[5])/tot_items_tim,
         (old_items_tim[1]+old_items_tim[2]+old_items_tim[3]+old_items_tim[4]+old_items_tim[5]+old_items_tim[6])/tot_items_tim)
        
  
#false alarm rate Tim
tot_new_tim <- sum(new_items_tim)
df_tim$far <- c(new_items_tim[1]/tot_new_tim, 
         (new_items_tim[1]+new_items_tim[2])/tot_new_tim,
         (new_items_tim[1]+new_items_tim[2]+new_items_tim[3])/tot_new_tim,
         (new_items_tim[1]+new_items_tim[2]+new_items_tim[3]+new_items_tim[4])/tot_new_tim,
         (new_items_tim[1]+new_items_tim[2]+new_items_tim[3]+new_items_tim[4]+new_items_tim[5])/tot_new_tim,
         (new_items_tim[1]+new_items_tim[2]+new_items_tim[3]+new_items_tim[4]+new_items_tim[5]+new_items_tim[6])/tot_new_tim)
nice_table(df_tim,
           title= "Dataset + Hit and False Alarm rate for Tim")
```

<div class="tabwid"><style>.cl-9a9d7778{table-layout:auto;}.cl-9a96c374{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9a96c37e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9a9942c0{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9a9942ca{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9a9954fe{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a995508{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a995509{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a99550a{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a995512{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9a995513{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-9a9d7778'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-9a9954fe"><p class="cl-9a9942c0"><span class="cl-9a96c374">Dataset + Hit and False Alarm rate for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-9a995508"><p class="cl-9a9942ca"><span class="cl-9a96c37e">old_items_tim</span></p></th><th class="cl-9a995508"><p class="cl-9a9942ca"><span class="cl-9a96c37e">new_items_tim</span></p></th><th class="cl-9a995508"><p class="cl-9a9942ca"><span class="cl-9a96c37e">confidence</span></p></th><th class="cl-9a995508"><p class="cl-9a9942ca"><span class="cl-9a96c37e">hit</span></p></th><th class="cl-9a995508"><p class="cl-9a9942ca"><span class="cl-9a96c37e">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-9a995509"><p class="cl-9a9942c0"><span class="cl-9a96c37e">50.00</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">250.00</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">extreme weak</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">0.05</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a995509"><p class="cl-9a9942c0"><span class="cl-9a96c37e">50.00</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">250.00</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">very weak</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">0.10</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a995509"><p class="cl-9a9942c0"><span class="cl-9a96c37e">50.00</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">400.00</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">fairly weak</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">0.15</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">0.90</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a995509"><p class="cl-9a9942c0"><span class="cl-9a96c37e">150.00</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">50.00</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">fairly strong</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">0.30</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">0.95</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a995509"><p class="cl-9a9942c0"><span class="cl-9a96c37e">500.00</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">25.00</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">very strong</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">0.80</span></p></td><td class="cl-9a99550a"><p class="cl-9a9942ca"><span class="cl-9a96c37e">0.97</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9a995512"><p class="cl-9a9942c0"><span class="cl-9a96c37e">200.00</span></p></td><td class="cl-9a995513"><p class="cl-9a9942ca"><span class="cl-9a96c37e">25.00</span></p></td><td class="cl-9a995513"><p class="cl-9a9942ca"><span class="cl-9a96c37e">extreme strong</span></p></td><td class="cl-9a995513"><p class="cl-9a9942ca"><span class="cl-9a96c37e">1.00</span></p></td><td class="cl-9a995513"><p class="cl-9a9942ca"><span class="cl-9a96c37e">1.00</span></p></td></tr></tbody></table></div>

</div>

- **John**

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
#hit rates for john
tot_old_john <- sum(old_items_john)
df_john$hit <- c(old_items_john[1]/tot_old_john, 
         (old_items_john[1]+old_items_john[2])/tot_old_john,
         (old_items_john[1]+old_items_john[2]+old_items_john[3])/tot_old_john,
         (old_items_john[1]+old_items_john[2]+old_items_john[3]+old_items_john[4])/tot_old_john,
         (old_items_john[1]+old_items_john[2]+old_items_john[3]+old_items_john[4]+old_items_john[5])/tot_old_john,
         (old_items_john[1]+old_items_john[2]+old_items_john[3]+old_items_john[4]+old_items_john[5]+old_items_john[6])/tot_old_john)

# false alarm rate john
tot_new_john <- sum(new_items_john)
df_john$far <- c(new_items_john[1]/tot_new_john, 
         (new_items_john[1]+new_items_john[2])/tot_new_john,
         (new_items_john[1]+new_items_john[2]+new_items_john[3])/tot_new_john,
         (new_items_john[1]+new_items_john[2]+new_items_john[3]+new_items_john[4])/tot_new_john,
         (new_items_john[1]+new_items_john[2]+new_items_john[3]+new_items_john[4]+new_items_john[5])/tot_new_john,
         (new_items_john[1]+new_items_john[2]+new_items_john[3]+new_items_john[4]+new_items_john[5]+new_items_john[6])/tot_new_john)
nice_table(df_john,
           title= "Dataset + Hit and False Alarm rate for John")
```

<div class="tabwid"><style>.cl-9ab1fc48{table-layout:auto;}.cl-9aabc3b4{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9aabc3be{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9aadfa94{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9aadfa9e{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9aae0890{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9aae0891{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9aae089a{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9aae089b{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9aae089c{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9aae08a4{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-9ab1fc48'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-9aae0890"><p class="cl-9aadfa94"><span class="cl-9aabc3b4">Dataset + Hit and False Alarm rate for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-9aae0891"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">old_items_john</span></p></th><th class="cl-9aae0891"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">new_items_john</span></p></th><th class="cl-9aae0891"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">confidence</span></p></th><th class="cl-9aae0891"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">hit</span></p></th><th class="cl-9aae0891"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-9aae089a"><p class="cl-9aadfa94"><span class="cl-9aabc3be">50.00</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">100.00</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">extreme weak</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">0.05</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">0.10</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9aae089a"><p class="cl-9aadfa94"><span class="cl-9aabc3be">75.00</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">400.00</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">very weak</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">0.12</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9aae089a"><p class="cl-9aadfa94"><span class="cl-9aabc3be">75.00</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">300.00</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">fairly weak</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">0.20</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">0.80</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9aae089a"><p class="cl-9aadfa94"><span class="cl-9aabc3be">500.00</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">50.00</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">fairly strong</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">0.70</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">0.85</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9aae089a"><p class="cl-9aadfa94"><span class="cl-9aabc3be">200.00</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">75.00</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">very strong</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">0.90</span></p></td><td class="cl-9aae089b"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">0.92</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9aae089c"><p class="cl-9aadfa94"><span class="cl-9aabc3be">100.00</span></p></td><td class="cl-9aae08a4"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">75.00</span></p></td><td class="cl-9aae08a4"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">extreme strong</span></p></td><td class="cl-9aae08a4"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">1.00</span></p></td><td class="cl-9aae08a4"><p class="cl-9aadfa9e"><span class="cl-9aabc3be">1.00</span></p></td></tr></tbody></table></div>

</div>

------------------------------------------------------------------------

## Plot ROC Curves for Tim and John

------------------------------------------------------------------------

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
roc_data <- data.frame(df_tim,df_john)
ggplot(roc_data, aes(x = 1- far, y = 1- hit, color = "Tim")) +
  geom_line()+ geom_point()+
  geom_line(aes(x = 1 - far.1, y = 1 - hit.1, color="John"))+ geom_point(aes(x = 1 - far.1, y = 1 - hit.1, color="John"))+
  geom_abline(intercept = 0, slope = 1, linetype = "dashed") +
  labs(x = "False Positive Rate (1 - Specificity)", y = "True Positive Rate (Sensitivity)") +
  ggtitle("ROC Curve")+
  theme_classic()+
  ylim(0,1)+
  geom_vline(xintercept = .10,
             lwd = 1,
             linetype="dashed")+
  annotate("text", x=.23, y=1, label="10% false positives")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

</div>

## ROC Curve via Logistic regression

- Create datasets for Tim and John that are compatible for logistic regression.

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
correct_mem <- rep(c(1, 0), each = 1000)
confidence_tim  <- c(rep(1,50),
                 rep(2,50),
                 rep(3,50),
                 rep(4,150),
                 rep(5,500),
                 rep(6,200),
                 rep(1,250),
                 rep(2,250),
                 rep(3,400),
                 rep(4,50),
                 rep(5,25),
                 rep(6,25))
confidence_john  <- c(rep(1,50),
                 rep(2,75),
                 rep(3,75),
                 rep(4,500),
                 rep(5,200),
                 rep(6,100),
                 rep(1,100),
                 rep(2,400),
                 rep(3,300),
                 rep(4,50),
                 rep(5,75), # i made this 75 as in the article there are otherwise only 1950 items for john instead 2000 as with Tim's. It was orignally 25
                 rep(6,75))
df <- data.frame(correct_mem,confidence_tim,confidence_john)
```

</div>

------------------------------------------------------------------------

- **Create ROC Curves for both**

------------------------------------------------------------------------

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
glm.fit_tim=glm(correct_mem ~ confidence_tim, family=binomial)
glm.fit_john=glm(correct_mem ~ confidence_john, family=binomial)

par(pty="s")
p1 <- roc(correct_mem, glm.fit_tim$fitted.values, 
    plot= TRUE,
    legacy.axes=TRUE, 
    percent=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="#377eb8", 
    lwd=4,
    print.auc = TRUE)

p2 <- roc(correct_mem, glm.fit_john$fitted.values, 
    plot= TRUE,
    legacy.axes=TRUE, 
    percent=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="red", 
    lwd=4,
    print.auc = TRUE,
    print.auc.y = 40,
    add=TRUE)
abline(v = 90, col = "black", lty = 2)
text(x = 60, y = 98, labels = "10% false positive", col = "black")
legend("bottomright", legend=c("Tim", "John"),
       col=c("Blue","red"), lwd=2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="672" />

</div>

------------------------------------------------------------------------

## True and False Positive per Threshold

------------------------------------------------------------------------

- This can be used to examine the blackstone ratio (10:1; false negative to false positive ratio)
- True and False Positive percentages for each confidence threshold for Tim

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
info_tim <- roc(correct_mem, glm.fit_tim$fitted.values) 
tim.df <- data.frame(
  Confidence = c("","Extreme-very weak", "Very-fairly weak", "Weak-strong", "Fairly-very strong","Very-extreme strong",""),
  tpp=info_tim$sensitivities*100,
  fpp=(1-info_tim$specificities)*100, #to make sure the order goes from weak to high confidence
  thresholds = info_tim$thresholds,
  FN_FP = (length(info_tim$cases)*(1-info_tim$sensitivities))/(length(info_tim$controls)*(1-info_tim$specificities)))

tim.df <- tim.df[-c(1,7), ]  
nice_table(tim.df,
           title="True Positive and False Positive Percentages for each Threshold")
```

<div class="tabwid"><style>.cl-9b1ee966{table-layout:auto;}.cl-9b189b88{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9b189b92{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9b1adad8{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9b1adae2{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9b1ae794{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9b1ae79e{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9b1ae79f{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9b1ae7a8{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9b1ae7a9{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9b1ae7aa{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-9b1ee966'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-9b1ae794"><p class="cl-9b1adad8"><span class="cl-9b189b88">True Positive and False Positive Percentages for each Threshold</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-9b1ae79e"><p class="cl-9b1adae2"><span class="cl-9b189b92">Confidence</span></p></th><th class="cl-9b1ae79e"><p class="cl-9b1adae2"><span class="cl-9b189b92">tpp</span></p></th><th class="cl-9b1ae79e"><p class="cl-9b1adae2"><span class="cl-9b189b92">fpp</span></p></th><th class="cl-9b1ae79e"><p class="cl-9b1adae2"><span class="cl-9b189b92">thresholds</span></p></th><th class="cl-9b1ae79e"><p class="cl-9b1adae2"><span class="cl-9b189b92">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-9b1ae79f"><p class="cl-9b1adad8"><span class="cl-9b189b92">Extreme-very weak</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">95.00</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">75.00</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">0.09</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">0.07</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9b1ae79f"><p class="cl-9b1adad8"><span class="cl-9b189b92">Very-fairly weak</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">90.00</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">50.00</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">0.24</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">0.20</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9b1ae79f"><p class="cl-9b1adad8"><span class="cl-9b189b92">Weak-strong</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">85.00</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">10.00</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">0.49</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">1.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9b1ae79f"><p class="cl-9b1adad8"><span class="cl-9b189b92">Fairly-very strong</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">70.00</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">5.00</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">0.75</span></p></td><td class="cl-9b1ae7a8"><p class="cl-9b1adae2"><span class="cl-9b189b92">6.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9b1ae7a9"><p class="cl-9b1adad8"><span class="cl-9b189b92">Very-extreme strong</span></p></td><td class="cl-9b1ae7aa"><p class="cl-9b1adae2"><span class="cl-9b189b92">20.00</span></p></td><td class="cl-9b1ae7aa"><p class="cl-9b1adae2"><span class="cl-9b189b92">2.50</span></p></td><td class="cl-9b1ae7aa"><p class="cl-9b1adae2"><span class="cl-9b189b92">0.90</span></p></td><td class="cl-9b1ae7aa"><p class="cl-9b1adae2"><span class="cl-9b189b92">32.00</span></p></td></tr></tbody></table></div>

</div>

- True and False Positive percentages for each confidence threshold for John

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
info_john <- roc(correct_mem, glm.fit_john$fitted.values) 
john.df <- data.frame(
  Confidence = c("","Extreme-very weak", "Very-fairly weak", "Weak-strong", "Fairly-very strong","Very-extreme strong",""),
  tpp=info_john$sensitivities*100,
  fpp=(1-info_john$specificities)*100, #to make sure the order goes from weak to high confidence
  thresholds = info_john$thresholds,
  FN_FP = (length(info_john$cases)*(1-info_john$sensitivities))/(length(info_john$controls)*(1-info_john$specificities)))

john.df <- john.df[-c(1,7), ]  
nice_table(john.df,
           title="True Positive and False Positive Percentages for each Threshold for John")
```

<div class="tabwid"><style>.cl-9b352726{table-layout:auto;}.cl-9b2f5d1e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9b2f5d28{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9b31a402{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9b31a40c{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9b31b0dc{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9b31b0e6{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9b31b0e7{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9b31b0e8{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9b31b0f0{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9b31b0f1{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-9b352726'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-9b31b0dc"><p class="cl-9b31a402"><span class="cl-9b2f5d1e">True Positive and False Positive Percentages for each Threshold for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-9b31b0e6"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">Confidence</span></p></th><th class="cl-9b31b0e6"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">tpp</span></p></th><th class="cl-9b31b0e6"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">fpp</span></p></th><th class="cl-9b31b0e6"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">thresholds</span></p></th><th class="cl-9b31b0e6"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-9b31b0e7"><p class="cl-9b31a402"><span class="cl-9b2f5d28">Extreme-very weak</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">95.00</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">90.00</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">0.21</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">0.06</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9b31b0e7"><p class="cl-9b31a402"><span class="cl-9b2f5d28">Very-fairly weak</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">87.50</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">50.00</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">0.35</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9b31b0e7"><p class="cl-9b31a402"><span class="cl-9b2f5d28">Weak-strong</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">80.00</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">20.00</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">0.51</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9b31b0e7"><p class="cl-9b31a402"><span class="cl-9b2f5d28">Fairly-very strong</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">30.00</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">15.00</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">0.68</span></p></td><td class="cl-9b31b0e8"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">4.67</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9b31b0f0"><p class="cl-9b31a402"><span class="cl-9b2f5d28">Very-extreme strong</span></p></td><td class="cl-9b31b0f1"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">10.00</span></p></td><td class="cl-9b31b0f1"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">7.50</span></p></td><td class="cl-9b31b0f1"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">0.81</span></p></td><td class="cl-9b31b0f1"><p class="cl-9b31a40c"><span class="cl-9b2f5d28">12.00</span></p></td></tr></tbody></table></div>

</div>

------------------------------------------------------------------------

## Confidence Intervals

------------------------------------------------------------------------

- The graphs provide important information for whos memory is better.
  - For example, the ROC curve of Tim is greater at eacg threshold than the ROC curve of John.
- However, to fully examine the AUC we would like 95% CI.
  - This can be easily extracted in the pROC package.
  - You can either choose to bootstrap or use the [delong method](https://doi.org/10.1109/LSP.2014.2337313).
  - Using the 95%CI the AUC can be compared to the smallest effect size of interest (e.g., AUC_null = .8).

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# Delong method
auc_tim <- roc(correct_mem, glm.fit_tim$fitted.values)
auc_john <- roc(correct_mem, glm.fit_john$fitted.values)
CI_delong_tim <- ci.auc(auc_tim) # default is 95%
CI_delong_john <- ci.auc(auc_john) 
CI_boot_tim <- ci.auc(auc_tim, method = "bootstrap", boot.n = 1000) # bootstrapping 1000 times
CI_boot_john <- ci.auc(auc_john, method = "bootstrap", boot.n = 1000) # bootstrapping 1000 times
p_tim <- power.roc.test(auc_tim) # null hypothesis is set to .5
p_john <- power.roc.test(auc_john)# null hypothesis is set to .5
tab_ci <- data.frame(
  Name = c("Tim","John"),
  AUC = c(CI_delong_tim[2],CI_delong_john[2]), #AUC values
  CI_low_Delong = c(CI_delong_tim[1],CI_delong_john[1]), # lower bound CI delong
  CI_upper_Delong = c(CI_delong_tim[3],CI_delong_john[3]),# upper bound CI delong
  CI_low_boot = c(CI_delong_tim[1],CI_delong_john[1]),# lower bound CI bootstrap
  CI_upper_boot = c(CI_delong_tim[3],CI_delong_john[3]),# upper bound CI bootstrap
  Power = c(p_tim$power,p_john$power)
)
nice_table(tab_ci,
           title = "AUC values with 95% CI via Delong or bootstrapping",
)
```

<div class="tabwid"><style>.cl-9bf6d39e{table-layout:auto;}.cl-9bf0c95e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9bf0c972{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-9bf33428{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9bf33432{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-9bf34166{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9bf34170{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9bf34171{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9bf34172{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9bf34173{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-9bf3417a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-9bf6d39e'><thead><tr style="overflow-wrap:break-word;"><th  colspan="7"class="cl-9bf34166"><p class="cl-9bf33428"><span class="cl-9bf0c95e">AUC values with 95% CI via Delong or bootstrapping</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-9bf34170"><p class="cl-9bf33432"><span class="cl-9bf0c972">Name</span></p></th><th class="cl-9bf34170"><p class="cl-9bf33432"><span class="cl-9bf0c972">AUC</span></p></th><th class="cl-9bf34170"><p class="cl-9bf33432"><span class="cl-9bf0c972">CI_low_Delong</span></p></th><th class="cl-9bf34170"><p class="cl-9bf33432"><span class="cl-9bf0c972">CI_upper_Delong</span></p></th><th class="cl-9bf34170"><p class="cl-9bf33432"><span class="cl-9bf0c972">CI_low_boot</span></p></th><th class="cl-9bf34170"><p class="cl-9bf33432"><span class="cl-9bf0c972">CI_upper_boot</span></p></th><th class="cl-9bf34170"><p class="cl-9bf33432"><span class="cl-9bf0c972">Power</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-9bf34171"><p class="cl-9bf33428"><span class="cl-9bf0c972">Tim</span></p></td><td class="cl-9bf34172"><p class="cl-9bf33432"><span class="cl-9bf0c972">0.88</span></p></td><td class="cl-9bf34172"><p class="cl-9bf33432"><span class="cl-9bf0c972">0.86</span></p></td><td class="cl-9bf34172"><p class="cl-9bf33432"><span class="cl-9bf0c972">0.89</span></p></td><td class="cl-9bf34172"><p class="cl-9bf33432"><span class="cl-9bf0c972">0.86</span></p></td><td class="cl-9bf34172"><p class="cl-9bf33432"><span class="cl-9bf0c972">0.89</span></p></td><td class="cl-9bf34172"><p class="cl-9bf33432"><span class="cl-9bf0c972">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-9bf34173"><p class="cl-9bf33428"><span class="cl-9bf0c972">John</span></p></td><td class="cl-9bf3417a"><p class="cl-9bf33432"><span class="cl-9bf0c972">0.76</span></p></td><td class="cl-9bf3417a"><p class="cl-9bf33432"><span class="cl-9bf0c972">0.74</span></p></td><td class="cl-9bf3417a"><p class="cl-9bf33432"><span class="cl-9bf0c972">0.78</span></p></td><td class="cl-9bf3417a"><p class="cl-9bf33432"><span class="cl-9bf0c972">0.74</span></p></td><td class="cl-9bf3417a"><p class="cl-9bf33432"><span class="cl-9bf0c972">0.78</span></p></td><td class="cl-9bf3417a"><p class="cl-9bf33432"><span class="cl-9bf0c972">1.00</span></p></td></tr></tbody></table></div>

</div>

------------------------------------------------------------------------

## ROC curves with 95%

------------------------------------------------------------------------

- Preferably, we plot the confidence intervals along the ROC curve to better interpret the results

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
par(pty="s")
p1 <- roc(correct_mem, glm.fit_tim$fitted.values, 
    plot= TRUE,
    legacy.axes=TRUE, 
    percent=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="#377eb8", 
    lwd=4,
    print.auc = TRUE)
p3 <- ci.se(p1,                         # CI of Tim
            specificities = seq(0, 100, 1),  # CI of sensitivity at at what specificities? 0,5,10,15,....,100
            conf.level=0.95,                 # level of Confidence interval
) 

plot(p3, type = "shape", col = "blue")

p2 <- roc(correct_mem, glm.fit_john$fitted.values, 
    plot= TRUE,
    legacy.axes=TRUE, 
    percent=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="red", 
    lwd=4,
    print.auc = TRUE,
    print.auc.y = 40,
    add=TRUE)
p4 <- ci.se(p2,                         # CI of john 
            specificities = seq(0, 100, 1),  # CI at which thresholds?
            conf.level=0.95,                 # indicate confidence interval
) 
plot(p4, type = "shape", col = "red")     # plot as a blue shape
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-11-1.png" width="672" />

</div>

------------------------------------------------------------------------

## Partial AUC

------------------------------------------------------------------------

- We can calculate the area under the curve for a part of the curve.
  - One example is to calculate the curve for values under the 10% false positive
  - That is because we want to minimize the amount of false positive when it comes to eyewitness identification.

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
# Calculate the partial AUC
partial_auc_tim <- roc(correct_mem, glm.fit_tim$fitted.values, partial.auc=c(100,90), percent = T)
partial_auc_john <- roc(correct_mem, glm.fit_john$fitted.values, partial.auc=c(100,90), percent = T)

# Print the partial AUC
par(pty="s")
p1 <- plot(partial_auc_tim,
    plot= TRUE,
    legacy.axes=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="red", 
    lwd=4,
    print.auc = TRUE,
    print.auc.x = 50,
    print.auc.y = 40,
    auc.polygon=TRUE,
    percent=TRUE)
p1 <- plot(partial_auc_john,
    plot= TRUE,
    legacy.axes=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="blue", 
    lwd=4,
    print.auc = TRUE,
    print.auc.x = 50,
    print.auc.y = 30,
    auc.polygon=TRUE,
    percent=TRUE,
    add=T)
abline(v = 90, col = "black", lty = 2)
text(x = 65, y = 98, labels = "10% False positive", col = "black")
legend("bottomright", legend=c("Tim", "John"),
       col=c("Blue","red"), lwd=2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-12-1.png" width="672" />

</div>
