---
title: "Extended results for: The balance of nature: Critical Role of Species Environmental Responses for Stability"
author: "Til Hämmig, Francesco Polazzo, Owen L. Petchey, Frank Pennekamp"
date: "28 January, 2025"
output:
  bookdown::html_document2:
    toc: true
    toc_float:
      collapsed: true
      smooth_scroll: true
    code_folding: hide
    keep_md: yes
    fig_caption: true  
editor_options: 
  markdown: 
    wrap: 72
---






# Introduction

We are going to look at the effect of the distribution of species responses, richness, temperature and nutrients on community temporal stability. Specifically, we are going to look at the effect of fundamental imbalance (our measurement of the distribution of species' fundamental responses) on temporal stability. Species' fundamental responses to environmental change can stabilise community biomass in two ways: through response diversity and /or through population stability. Thus, as response diversity is thought to stabilize temporal stability of aggregate community properties via asynchrony, we are going to look at the relationship between response diversity and asynchrony. Subsequently, we are going to look at the relationship between population stability and temporal stability of community biomass.
Finally, we use a structural equation model to test the direct and indirect effects of balance on temporal stability of community biomass.

In this document, we also analyse the predictive power of imbalance and divergence on temporal stability, and we compare the unique explanatory power of imbalance and divergence. We also look at the interaction between divergence and richness, and between imbalance and richness. Finally, we assess variable importance using the relative importance of predictors in the full model. This part of the document (section 5), is not included in the main article, but it is an important part of the analysis that justify adopting imbalance instead of divergence, which was the original metric used to design the experiment.

This document is produced by an Rmarkdown file that includes code to reproduce from data all results presented in the main article.



# Load datasets, Data wrangling and Imbalance calculation




# Biomass

Let's have a look at the biomass dynamics in the different environmental treatments.

### tot biomass plot

<img src="Extended_Data_files/figure-html/plot_biomass-1.png" style="display: block; margin: auto;" />

 **Figure 1** : Community total biomass during the experiment in different environmental treatments. Different color represent richness levels.



# Main Results 

We now look at the main results of the experiment. We are going to look first at the effect of richness, temperature and nutrients on community temporal stability. Then, we are going to look at the relationship between divergence (original response diversity metric) and temporal stability. Finally, we are going to look at the relationship between imbalance and temporal stability.

In the whole analysis, we calculated the temporal stability of total community biomass as the inverse of the coefficient of variation (ICV) (i.e. $\frac{\sigma}{\mu}$). 


### Effect of T, N and R
<img src="Extended_Data_files/figure-html/boxplots_TNR-1.png" style="display: block; margin: auto;" />
**Figure 2**: Effects of richness (a), temperature (b), and nutrients (c) on community total biomass temporal stability.



We can see that richness does not have a clear effect on community temporal stability, while stability was higher at lower temperature, and nutrients increased community temporal stability.

### Effect of Divergence

We look at the relationship between divergence (our original response diversity metric) and stability

<img src="Extended_Data_files/figure-html/divergence_CV-1.png" style="display: block; margin: auto;" />


**Figure 3**: Relationship between Divergence and temporal stability of total community biomass.



Divergence is positively related to temporal stability, suggesting that response diversity promotes stability. However, the relationship between divergence and stability becomes weaker as richness increases. We think that this is due to divergence considering only the responses of the 2 most "responding" species. Thus, when species richness increases, disregarding the responses of the other species in the community except the 2 responding the most makes the relationship between response diversity and stability weaker. 

This is why, after running the experiment, we developed another metric to measure the distribution of species' responses, which we called **imbalance**, and that is presented in the main text of the publication. 
Imbalance has several desirable features that make it a more suitable metric than divergence: Independence of richness, higher predictive power, and accounts for the responses of all species in the community (as opposed to divergence that accounts for only the 2 most "responding" species).

Here, we provide extensive evidence of why imbalance is a better metric to measure response diversity than divergence, and thus justifying focusing the analysis around imbalance.

# Comparing Divergence and Imbalance

## Predictive power of Divergence and Imbalance

We first compare how well divergence and imbalance predict stability (predictive power). 



### Imbalance


``` r
# 

mod1 <- lm(data=complete_aggr,log10(stability)~log10(balance_f))

# Check model assumptions
#check_model(mod1)
```



### Divergence

``` r
mod2 <- lm(data=complete_aggr,log10(stability)~(divergence))

# Check model assumptions
#check_model(mod2)
```



**Table 1**: Comparison of model performance of divergence and imbalance as predictors of stability. Model 1 has imbalance as predictor and model 2 has divergence as predictor.
<table class="table" style="color: black; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> model </th>
   <th style="text-align:right;"> AIC </th>
   <th style="text-align:right;"> AICc </th>
   <th style="text-align:right;"> BIC </th>
   <th style="text-align:right;"> R2 </th>
   <th style="text-align:right;"> R2_adjusted </th>
   <th style="text-align:right;"> RMSE </th>
   <th style="text-align:right;"> Sigma </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> -89.27328 </td>
   <td style="text-align:right;"> -89.17286 </td>
   <td style="text-align:right;"> -78.79409 </td>
   <td style="text-align:right;"> 0.1917679 </td>
   <td style="text-align:right;"> 0.1884142 </td>
   <td style="text-align:right;"> 0.1510344 </td>
   <td style="text-align:right;"> 0.1516599 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> -55.71579 </td>
   <td style="text-align:right;"> -55.61538 </td>
   <td style="text-align:right;"> -45.23661 </td>
   <td style="text-align:right;"> 0.0720796 </td>
   <td style="text-align:right;"> 0.0682293 </td>
   <td style="text-align:right;"> 0.1618316 </td>
   <td style="text-align:right;"> 0.1625017 </td>
  </tr>
</tbody>
</table>

A model with imbalance as predictor performs better than one with divergence as predictor, and it explains more of the variance in stability than divergence.



Moreover, from **Figure 3**, it looks like divergence declines in performance as richness increases. Let's test this analytically.
To do than we build a linear model having stability as response variable and either log10(imbalance) or divergence as predictor for each richness level. We then extract the R squared of the models and their *standardised* estimates. (standardized estimates were calculated centering divergence and imbalance using the function scale()).


``` r
# getting model estimates for each richness level
lm_divergence_richness_E <- complete_aggr %>%
  nest(data = -richness) %>%
  mutate(
    model = map(data, ~ lm(log10(stability) ~ scale(divergence), data = .x)),
    results = map(model, broom::tidy)
  ) %>%
  unnest(results) %>% dplyr::filter(term=="scale(divergence)") 


# getting model R squared for each richness level

lm_divergence_richness_R <- complete_aggr %>%
  nest(data = -richness) %>%
  mutate(
    model = map(data, ~ lm(log10(stability) ~ scale(divergence), data = .x)),
    results = map(model, broom::glance)
  ) %>%
  unnest(results) 
```



``` r
# getting model estimatesf or each richness level
lm_balance_richness_E <- complete_aggr %>%
  nest(data = -richness) %>%
  mutate(
    model = map(data, ~ lm(log10(stability) ~ scale(log10(balance_f)), data = .x)),
    results = map(model, broom::tidy)
  ) %>%
  unnest(results) %>% dplyr::filter(term=="scale(log10(balance_f))") 



# getting model R squared for each richness level
lm_balance_richness_R <- complete_aggr %>%
  nest(data = -richness) %>%
  mutate(
    model = map(data, ~ lm(log10(stability) ~ scale(log10(balance_f)), data = .x)),
    results = map(model, broom::glance)
  ) %>%
  unnest(results) 
```


<img src="Extended_Data_files/figure-html/R_squared-1.png" style="display: block; margin: auto;" />
**Figure 4**: Performance comparison of divergence vs imbalance. In (a), the R squared of linear models for divergence and imbalance are shown for each richness level. In (b), the estimates of the linear models for divergence and imbalance are shown for each richness level.




We can see that the R squared of divergence as predictor of stability becomes smaller as richness increases, while the R squared of imbalance as predictor of stability does not (actually increases slightly). 


## Comparing unique explanatory power of imbalance and divergence

Now we build a linear model were stability is modeled as a function of imbalance and divergence. 
Then, we compared the variance explained by the full model compared to a model containing either only imbalance or only divergence.


### Full model - imbalance and divergence


``` r
lm_div_balance <- lm(data=complete_aggr,log10(stability)~log10(balance_f)+divergence)

# Check model assumptions
# check_model(lm_div_balance)
```



### model with only divergence

``` r
lm_div <- lm(data=complete_aggr,log10(stability)~divergence)

# Check model assumptions
# check_model(lm_div)
```




### model with only imbalance

``` r
lm_balance <- lm(data=complete_aggr,log10(stability)~log10(balance_f))

# Check model assumptions
# check_model(lm_balance)
```


### Comparision full model vs divergence only and imbalance only


**Table 2**: Comparison of model performance of divergence, imbalance and both as predictors of stability. Model 1 has both imbalance and divergence as predictors, model 2 has divergence as predictor, and model 3 has imbalance as predictor.
<table class="table" style="color: black; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> model </th>
   <th style="text-align:right;"> AIC </th>
   <th style="text-align:right;"> AICc </th>
   <th style="text-align:right;"> BIC </th>
   <th style="text-align:right;"> R2 </th>
   <th style="text-align:right;"> R2_adjusted </th>
   <th style="text-align:right;"> RMSE </th>
   <th style="text-align:right;"> Sigma </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> -88.97683 </td>
   <td style="text-align:right;"> -88.80876 </td>
   <td style="text-align:right;"> -75.00458 </td>
   <td style="text-align:right;"> 0.1974141 </td>
   <td style="text-align:right;"> 0.1907259 </td>
   <td style="text-align:right;"> 0.1505060 </td>
   <td style="text-align:right;"> 0.1514437 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> -55.71579 </td>
   <td style="text-align:right;"> -55.61538 </td>
   <td style="text-align:right;"> -45.23661 </td>
   <td style="text-align:right;"> 0.0720796 </td>
   <td style="text-align:right;"> 0.0682293 </td>
   <td style="text-align:right;"> 0.1618316 </td>
   <td style="text-align:right;"> 0.1625017 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> -89.27328 </td>
   <td style="text-align:right;"> -89.17286 </td>
   <td style="text-align:right;"> -78.79409 </td>
   <td style="text-align:right;"> 0.1917679 </td>
   <td style="text-align:right;"> 0.1884142 </td>
   <td style="text-align:right;"> 0.1510344 </td>
   <td style="text-align:right;"> 0.1516599 </td>
  </tr>
</tbody>
</table>



### Comparision full model vs imbalance only

**Table 3**: Anova table: a model with both imbalance and divergence as predictors is not significantly different from a model with only imbalance as predictor.

``` r
anova1 <- anova(lm_div_balance,  lm_balance)

# Convert to tidy format
anova_tidy1 <- broom::tidy(anova1)
# Display the tidy ANOVA table using gt with formatted p-values and adjusted size
anova_tidy1 %>%
  gt() %>%
  cols_label(
    term = "Term",
    sumsq = "Sum of Squares",
    df = "DF",
    statistic = "F Statistic",
    p.value = "p-value"
  ) %>%
  fmt_number(
    columns = vars(p.value),
    decimals = 3
  ) %>%
  tab_style(
    style = cell_text(weight = "bold"),
    locations = cells_body(
      columns = vars(p.value),
      rows = p.value < 0.05
    )
  ) %>%
  tab_options(
    table.width = px(800),            # Adjust table width (e.g., 400px)
    table.font.size = px(12),        # Adjust font size (e.g., 12px)
    data_row.padding = px(10)         # Adjust row padding (e.g., 4px for more compact rows)
  )
```

```{=html}
<div id="tvxcvtweln" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#tvxcvtweln table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#tvxcvtweln thead, #tvxcvtweln tbody, #tvxcvtweln tfoot, #tvxcvtweln tr, #tvxcvtweln td, #tvxcvtweln th {
  border-style: none;
}

#tvxcvtweln p {
  margin: 0;
  padding: 0;
}

#tvxcvtweln .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 12px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: 800px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#tvxcvtweln .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#tvxcvtweln .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#tvxcvtweln .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#tvxcvtweln .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#tvxcvtweln .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#tvxcvtweln .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#tvxcvtweln .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#tvxcvtweln .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#tvxcvtweln .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#tvxcvtweln .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#tvxcvtweln .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#tvxcvtweln .gt_spanner_row {
  border-bottom-style: hidden;
}

#tvxcvtweln .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#tvxcvtweln .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#tvxcvtweln .gt_from_md > :first-child {
  margin-top: 0;
}

#tvxcvtweln .gt_from_md > :last-child {
  margin-bottom: 0;
}

#tvxcvtweln .gt_row {
  padding-top: 10px;
  padding-bottom: 10px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#tvxcvtweln .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#tvxcvtweln .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#tvxcvtweln .gt_row_group_first td {
  border-top-width: 2px;
}

#tvxcvtweln .gt_row_group_first th {
  border-top-width: 2px;
}

#tvxcvtweln .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#tvxcvtweln .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#tvxcvtweln .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#tvxcvtweln .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#tvxcvtweln .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#tvxcvtweln .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#tvxcvtweln .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#tvxcvtweln .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#tvxcvtweln .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#tvxcvtweln .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#tvxcvtweln .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#tvxcvtweln .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#tvxcvtweln .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#tvxcvtweln .gt_left {
  text-align: left;
}

#tvxcvtweln .gt_center {
  text-align: center;
}

#tvxcvtweln .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#tvxcvtweln .gt_font_normal {
  font-weight: normal;
}

#tvxcvtweln .gt_font_bold {
  font-weight: bold;
}

#tvxcvtweln .gt_font_italic {
  font-style: italic;
}

#tvxcvtweln .gt_super {
  font-size: 65%;
}

#tvxcvtweln .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#tvxcvtweln .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#tvxcvtweln .gt_indent_1 {
  text-indent: 5px;
}

#tvxcvtweln .gt_indent_2 {
  text-indent: 10px;
}

#tvxcvtweln .gt_indent_3 {
  text-indent: 15px;
}

#tvxcvtweln .gt_indent_4 {
  text-indent: 20px;
}

#tvxcvtweln .gt_indent_5 {
  text-indent: 25px;
}

#tvxcvtweln .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#tvxcvtweln div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="term">Term</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="df.residual">df.residual</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="rss">rss</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="df">DF</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="sumsq">Sum of Squares</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="statistic">F Statistic</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="p.value">p-value</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="term" class="gt_row gt_left">log10(stability) ~ log10(balance_f) + divergence</td>
<td headers="df.residual" class="gt_row gt_right">240</td>
<td headers="rss" class="gt_row gt_right">5.504447</td>
<td headers="df" class="gt_row gt_right">NA</td>
<td headers="sumsq" class="gt_row gt_right">NA</td>
<td headers="statistic" class="gt_row gt_right">NA</td>
<td headers="p.value" class="gt_row gt_right">NA</td></tr>
    <tr><td headers="term" class="gt_row gt_left">log10(stability) ~ log10(balance_f)</td>
<td headers="df.residual" class="gt_row gt_right">241</td>
<td headers="rss" class="gt_row gt_right">5.543171</td>
<td headers="df" class="gt_row gt_right">-1</td>
<td headers="sumsq" class="gt_row gt_right">-0.03872444</td>
<td headers="statistic" class="gt_row gt_right">1.688429</td>
<td headers="p.value" class="gt_row gt_right">0.195</td></tr>
  </tbody>
  
  
</table>
</div>
```


### Comparision full model vs imbalance only and divergence only

**Table 4**: Anova table: a model with both imbalance and divergence as predictors is significantly better from a model with only divergence as predictor.

``` r
anova2 <- anova(lm_div_balance,  lm_div)


anova_tidy2 <- broom::tidy(anova2)
# Display the tidy ANOVA table using gt with formatted p-values and adjusted size
anova_tidy2 %>%
  gt() %>%
  cols_label(
    term = "Term",
    sumsq = "Sum of Squares",
    df = "DF",
    statistic = "F Statistic",
    p.value = "p-value"
  ) %>%
  fmt_number(
    columns = vars(p.value),
    decimals = 3
  ) %>%
  tab_style(
    style = cell_text(weight = "bold"),
    locations = cells_body(
      columns = vars(p.value),
      rows = p.value < 0.05
    )
  ) %>%
  tab_options(
    table.width = px(800),            # Adjust table width (e.g., 400px)
    table.font.size = px(12),        # Adjust font size (e.g., 12px)
    data_row.padding = px(10)         # Adjust row padding (e.g., 4px for more compact rows)
  )
```

```{=html}
<div id="ebcxhpmnhj" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#ebcxhpmnhj table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#ebcxhpmnhj thead, #ebcxhpmnhj tbody, #ebcxhpmnhj tfoot, #ebcxhpmnhj tr, #ebcxhpmnhj td, #ebcxhpmnhj th {
  border-style: none;
}

#ebcxhpmnhj p {
  margin: 0;
  padding: 0;
}

#ebcxhpmnhj .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 12px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: 800px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#ebcxhpmnhj .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#ebcxhpmnhj .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#ebcxhpmnhj .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#ebcxhpmnhj .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#ebcxhpmnhj .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ebcxhpmnhj .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#ebcxhpmnhj .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#ebcxhpmnhj .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#ebcxhpmnhj .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#ebcxhpmnhj .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#ebcxhpmnhj .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#ebcxhpmnhj .gt_spanner_row {
  border-bottom-style: hidden;
}

#ebcxhpmnhj .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#ebcxhpmnhj .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#ebcxhpmnhj .gt_from_md > :first-child {
  margin-top: 0;
}

#ebcxhpmnhj .gt_from_md > :last-child {
  margin-bottom: 0;
}

#ebcxhpmnhj .gt_row {
  padding-top: 10px;
  padding-bottom: 10px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#ebcxhpmnhj .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#ebcxhpmnhj .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#ebcxhpmnhj .gt_row_group_first td {
  border-top-width: 2px;
}

#ebcxhpmnhj .gt_row_group_first th {
  border-top-width: 2px;
}

#ebcxhpmnhj .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#ebcxhpmnhj .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#ebcxhpmnhj .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#ebcxhpmnhj .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ebcxhpmnhj .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#ebcxhpmnhj .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#ebcxhpmnhj .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#ebcxhpmnhj .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#ebcxhpmnhj .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ebcxhpmnhj .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#ebcxhpmnhj .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#ebcxhpmnhj .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#ebcxhpmnhj .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#ebcxhpmnhj .gt_left {
  text-align: left;
}

#ebcxhpmnhj .gt_center {
  text-align: center;
}

#ebcxhpmnhj .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#ebcxhpmnhj .gt_font_normal {
  font-weight: normal;
}

#ebcxhpmnhj .gt_font_bold {
  font-weight: bold;
}

#ebcxhpmnhj .gt_font_italic {
  font-style: italic;
}

#ebcxhpmnhj .gt_super {
  font-size: 65%;
}

#ebcxhpmnhj .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#ebcxhpmnhj .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#ebcxhpmnhj .gt_indent_1 {
  text-indent: 5px;
}

#ebcxhpmnhj .gt_indent_2 {
  text-indent: 10px;
}

#ebcxhpmnhj .gt_indent_3 {
  text-indent: 15px;
}

#ebcxhpmnhj .gt_indent_4 {
  text-indent: 20px;
}

#ebcxhpmnhj .gt_indent_5 {
  text-indent: 25px;
}

#ebcxhpmnhj .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#ebcxhpmnhj div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="term">Term</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="df.residual">df.residual</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="rss">rss</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="df">DF</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="sumsq">Sum of Squares</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="statistic">F Statistic</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="p.value">p-value</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="term" class="gt_row gt_left">log10(stability) ~ log10(balance_f) + divergence</td>
<td headers="df.residual" class="gt_row gt_right">240</td>
<td headers="rss" class="gt_row gt_right">5.504447</td>
<td headers="df" class="gt_row gt_right">NA</td>
<td headers="sumsq" class="gt_row gt_right">NA</td>
<td headers="statistic" class="gt_row gt_right">NA</td>
<td headers="p.value" class="gt_row gt_right">NA</td></tr>
    <tr><td headers="term" class="gt_row gt_left">log10(stability) ~ divergence</td>
<td headers="df.residual" class="gt_row gt_right">241</td>
<td headers="rss" class="gt_row gt_right">6.364040</td>
<td headers="df" class="gt_row gt_right">-1</td>
<td headers="sumsq" class="gt_row gt_right">-0.8595933</td>
<td headers="statistic" class="gt_row gt_right">37.47922</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.000</td></tr>
  </tbody>
  
  
</table>
</div>
```

Overall, imbalance explains more of the variance in stability than divergence, and there is virtually no difference between a model containing only imbalance and the full model.




## Interaction divergence and richness

Richness had to be transformed to numeric and to be centered to avoid collinearity with divergence


``` r
lm_rich_div <- lm(data=complete_aggr,log10(stability)~divergence*scale(as.numeric(richness)))

# check model assumptions
# check_model(lm_rich_div)
```




**Table 5**: Type III anova table of the model with divergence and richness as predictors of stability.

``` r
anova3 <- car::Anova(lm_rich_div, type = "III")

anova_tidy3 <- broom::tidy(anova3)
# Display the tidy ANOVA table using gt with formatted p-values and adjusted size
anova_tidy3 %>%
  gt() %>%
  cols_label(
    term = "Term",
    sumsq = "Sum of Squares",
    df = "DF",
    statistic = "F Statistic",
    p.value = "p-value"
  ) %>%
  fmt_number(
    columns = vars(p.value),
    decimals = 3
  ) %>%
  tab_style(
    style = cell_text(weight = "bold"),
    locations = cells_body(
      columns = vars(p.value),
      rows = p.value < 0.05
    )
  ) %>%
  tab_options(
    table.width = px(800),            # Adjust table width (e.g., 400px)
    table.font.size = px(12),        # Adjust font size (e.g., 12px)
    data_row.padding = px(10)         # Adjust row padding (e.g., 4px for more compact rows)
  )
```

```{=html}
<div id="yotzgvxdyr" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#yotzgvxdyr table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#yotzgvxdyr thead, #yotzgvxdyr tbody, #yotzgvxdyr tfoot, #yotzgvxdyr tr, #yotzgvxdyr td, #yotzgvxdyr th {
  border-style: none;
}

#yotzgvxdyr p {
  margin: 0;
  padding: 0;
}

#yotzgvxdyr .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 12px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: 800px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#yotzgvxdyr .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#yotzgvxdyr .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#yotzgvxdyr .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#yotzgvxdyr .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#yotzgvxdyr .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yotzgvxdyr .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#yotzgvxdyr .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#yotzgvxdyr .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#yotzgvxdyr .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#yotzgvxdyr .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#yotzgvxdyr .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#yotzgvxdyr .gt_spanner_row {
  border-bottom-style: hidden;
}

#yotzgvxdyr .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#yotzgvxdyr .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#yotzgvxdyr .gt_from_md > :first-child {
  margin-top: 0;
}

#yotzgvxdyr .gt_from_md > :last-child {
  margin-bottom: 0;
}

#yotzgvxdyr .gt_row {
  padding-top: 10px;
  padding-bottom: 10px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#yotzgvxdyr .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#yotzgvxdyr .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#yotzgvxdyr .gt_row_group_first td {
  border-top-width: 2px;
}

#yotzgvxdyr .gt_row_group_first th {
  border-top-width: 2px;
}

#yotzgvxdyr .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#yotzgvxdyr .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#yotzgvxdyr .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#yotzgvxdyr .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yotzgvxdyr .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#yotzgvxdyr .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#yotzgvxdyr .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#yotzgvxdyr .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#yotzgvxdyr .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yotzgvxdyr .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#yotzgvxdyr .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#yotzgvxdyr .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#yotzgvxdyr .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#yotzgvxdyr .gt_left {
  text-align: left;
}

#yotzgvxdyr .gt_center {
  text-align: center;
}

#yotzgvxdyr .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#yotzgvxdyr .gt_font_normal {
  font-weight: normal;
}

#yotzgvxdyr .gt_font_bold {
  font-weight: bold;
}

#yotzgvxdyr .gt_font_italic {
  font-style: italic;
}

#yotzgvxdyr .gt_super {
  font-size: 65%;
}

#yotzgvxdyr .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#yotzgvxdyr .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#yotzgvxdyr .gt_indent_1 {
  text-indent: 5px;
}

#yotzgvxdyr .gt_indent_2 {
  text-indent: 10px;
}

#yotzgvxdyr .gt_indent_3 {
  text-indent: 15px;
}

#yotzgvxdyr .gt_indent_4 {
  text-indent: 20px;
}

#yotzgvxdyr .gt_indent_5 {
  text-indent: 25px;
}

#yotzgvxdyr .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#yotzgvxdyr div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="term">Term</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="sumsq">Sum of Squares</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="df">DF</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="statistic">F Statistic</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="p.value">p-value</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="term" class="gt_row gt_left">(Intercept)</td>
<td headers="sumsq" class="gt_row gt_right">11.033652088</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="statistic" class="gt_row gt_right">442.55571734</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.000</td></tr>
    <tr><td headers="term" class="gt_row gt_left">divergence</td>
<td headers="sumsq" class="gt_row gt_right">0.807044347</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="statistic" class="gt_row gt_right">32.37025122</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.000</td></tr>
    <tr><td headers="term" class="gt_row gt_left">scale(as.numeric(richness))</td>
<td headers="sumsq" class="gt_row gt_right">0.001236238</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="statistic" class="gt_row gt_right">0.04958505</td>
<td headers="p.value" class="gt_row gt_right">0.824</td></tr>
    <tr><td headers="term" class="gt_row gt_left">divergence:scale(as.numeric(richness))</td>
<td headers="sumsq" class="gt_row gt_right">0.249582101</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="statistic" class="gt_row gt_right">10.01064605</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.002</td></tr>
    <tr><td headers="term" class="gt_row gt_left">Residuals</td>
<td headers="sumsq" class="gt_row gt_right">5.958668583</td>
<td headers="df" class="gt_row gt_right">239</td>
<td headers="statistic" class="gt_row gt_right">NA</td>
<td headers="p.value" class="gt_row gt_right">NA</td></tr>
  </tbody>
  
  
</table>
</div>
```


Divergence significantly interact with richness, suggesting that the relationship between divergence and stability changes with richness. 
While an ideal metric of response diversity should be independent of richness.



We repeat the same model using imbalance instead of divergence.

``` r
lm_rich_balance <- lm(data=complete_aggr,log10(stability)~log10(balance_f)*scale(as.numeric(richness)))

# check model assumptions
# check_model(lm_rich_balance)
```





**Table 6**: Type III anova table of the model with imbalance and richness as predictors of stability.

``` r
anova4 <- car::Anova(lm_rich_balance, type = "III")

anova_tidy4 <- broom::tidy(anova4)
# Display the tidy ANOVA table using gt with formatted p-values and adjusted size
anova_tidy4 %>%
  gt() %>%
  cols_label(
    term = "Term",
    sumsq = "Sum of Squares",
    df = "DF",
    statistic = "F Statistic",
    p.value = "p-value"
  ) %>%
  fmt_number(
    columns = vars(p.value),
    decimals = 3
  ) %>%
  tab_style(
    style = cell_text(weight = "bold"),
    locations = cells_body(
      columns = vars(p.value),
      rows = p.value < 0.05
    )
  ) %>%
  tab_options(
    table.width = px(800),            # Adjust table width (e.g., 400px)
    table.font.size = px(12),        # Adjust font size (e.g., 12px)
    data_row.padding = px(10)         # Adjust row padding (e.g., 4px for more compact rows)
  )
```

```{=html}
<div id="tjzkdqmzch" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#tjzkdqmzch table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#tjzkdqmzch thead, #tjzkdqmzch tbody, #tjzkdqmzch tfoot, #tjzkdqmzch tr, #tjzkdqmzch td, #tjzkdqmzch th {
  border-style: none;
}

#tjzkdqmzch p {
  margin: 0;
  padding: 0;
}

#tjzkdqmzch .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 12px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: 800px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#tjzkdqmzch .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#tjzkdqmzch .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#tjzkdqmzch .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#tjzkdqmzch .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#tjzkdqmzch .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#tjzkdqmzch .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#tjzkdqmzch .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#tjzkdqmzch .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#tjzkdqmzch .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#tjzkdqmzch .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#tjzkdqmzch .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#tjzkdqmzch .gt_spanner_row {
  border-bottom-style: hidden;
}

#tjzkdqmzch .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#tjzkdqmzch .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#tjzkdqmzch .gt_from_md > :first-child {
  margin-top: 0;
}

#tjzkdqmzch .gt_from_md > :last-child {
  margin-bottom: 0;
}

#tjzkdqmzch .gt_row {
  padding-top: 10px;
  padding-bottom: 10px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#tjzkdqmzch .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#tjzkdqmzch .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#tjzkdqmzch .gt_row_group_first td {
  border-top-width: 2px;
}

#tjzkdqmzch .gt_row_group_first th {
  border-top-width: 2px;
}

#tjzkdqmzch .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#tjzkdqmzch .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#tjzkdqmzch .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#tjzkdqmzch .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#tjzkdqmzch .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#tjzkdqmzch .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#tjzkdqmzch .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#tjzkdqmzch .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#tjzkdqmzch .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#tjzkdqmzch .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#tjzkdqmzch .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#tjzkdqmzch .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#tjzkdqmzch .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#tjzkdqmzch .gt_left {
  text-align: left;
}

#tjzkdqmzch .gt_center {
  text-align: center;
}

#tjzkdqmzch .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#tjzkdqmzch .gt_font_normal {
  font-weight: normal;
}

#tjzkdqmzch .gt_font_bold {
  font-weight: bold;
}

#tjzkdqmzch .gt_font_italic {
  font-style: italic;
}

#tjzkdqmzch .gt_super {
  font-size: 65%;
}

#tjzkdqmzch .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#tjzkdqmzch .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#tjzkdqmzch .gt_indent_1 {
  text-indent: 5px;
}

#tjzkdqmzch .gt_indent_2 {
  text-indent: 10px;
}

#tjzkdqmzch .gt_indent_3 {
  text-indent: 15px;
}

#tjzkdqmzch .gt_indent_4 {
  text-indent: 20px;
}

#tjzkdqmzch .gt_indent_5 {
  text-indent: 25px;
}

#tjzkdqmzch .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#tjzkdqmzch div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="term">Term</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="sumsq">Sum of Squares</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="df">DF</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="statistic">F Statistic</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="p.value">p-value</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="term" class="gt_row gt_left">(Intercept)</td>
<td headers="sumsq" class="gt_row gt_right">9.54928736</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="statistic" class="gt_row gt_right">414.779284</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.000</td></tr>
    <tr><td headers="term" class="gt_row gt_left">log10(balance_f)</td>
<td headers="sumsq" class="gt_row gt_right">1.26534818</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="statistic" class="gt_row gt_right">54.961191</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.000</td></tr>
    <tr><td headers="term" class="gt_row gt_left">scale(as.numeric(richness))</td>
<td headers="sumsq" class="gt_row gt_right">0.02471247</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="statistic" class="gt_row gt_right">1.073401</td>
<td headers="p.value" class="gt_row gt_right">0.301</td></tr>
    <tr><td headers="term" class="gt_row gt_left">log10(balance_f):scale(as.numeric(richness))</td>
<td headers="sumsq" class="gt_row gt_right">0.04049552</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="statistic" class="gt_row gt_right">1.758948</td>
<td headers="p.value" class="gt_row gt_right">0.186</td></tr>
    <tr><td headers="term" class="gt_row gt_left">Residuals</td>
<td headers="sumsq" class="gt_row gt_right">5.50239554</td>
<td headers="df" class="gt_row gt_right">239</td>
<td headers="statistic" class="gt_row gt_right">NA</td>
<td headers="p.value" class="gt_row gt_right">NA</td></tr>
  </tbody>
  
  
</table>
</div>
```

Imbalance does not significantly interact with richness, suggesting that the relationship between imbalance and stability is stable across richness levels.


## Variable importance

Finally, we assess variable importance using the relative importance of predictors in the full model.
We use the package vip (https://cran.r-project.org/web/packages/vip/vignettes/vip.html) to calculate the relative importance of predictors in the full model.
The function vip::vip for multiple linear regression, or linear models (LMs), uses the absolute value of the -statistic  as a measure of VI.
Motivation for the use of the associated 𝑡-statistic is given in Bring (1994) [https://www.tandfonline.com/doi/abs/10.1080/00031305.1994.10476059].


``` r
vip::vip(lm_div_balance)
```

<img src="Extended_Data_files/figure-html/vip1-1.png" style="display: block; margin: auto;" />
**Figure 5**: Variable importance in the model including both imbalance and divergence as predictors of stability.


We believe that the extensive evidence here provided justifies focusing the analysis around imbalance, and not divergence, as a metric of response diversity.
We will thus only look at imbalance for the rest of the analysis. 

# Effect RD

We are now going to look at how imbalance affected temporal stability of total community biomass. We are going to look at the relationship between fundamental imbalance (so based only on species response surfaces measured in monoculture), an realised imbalance (measured accounting for species contribution to balance).

This is fundamentally testing our most important hypothesis.

<img src="Extended_Data_files/figure-html/effect_RD-1.png" style="display: block; margin: auto;" />
**Figure 6**: Effects of fundamental and realised imbalance on total community biomass temporal stability.


We can see that imbalance is always negatively related to temporal stability, which means that balance in species responses promotes stability across richness levels. Interestingly, we see that there is little difference between fundamental and realised imbalance. Yet, as the richness increases, the relationship between realised imbalance and stability becomes steeper compared to fundamental balance. 


But is the difference between fundamental and realised imbalance significant? We can test this using a linear model with both fundamental and realised imbalance as predictors of stability, and one with only fundamental imbalance as predictor of stability, and compare whether the models are significantly different.

## Imbalance: realised vs fundamental

``` r
# compare if the slope of fundamental and realised balance is significantly different for each richness level
# Fit the linear model with interaction
complete_aggr_2<- complete_aggr %>%
  # Remove the units from the 'nutrients' and 'temperature' columns
  mutate(
    nutrients = as.numeric(gsub(" g/L", "", nutrients)),  # Convert nutrients to numeric
    temperature = gsub(" °C", "", temperature)            # Remove the unit but keep as character
  ) %>%
  # Convert temperature ranges to numeric codes using case_when
  mutate(
    temperature = case_when(
      temperature == "18-21" ~ 1,
      temperature == "22-25" ~ 2,
      temperature == "25-28" ~ 3,
      TRUE ~ NA_real_         # Handle unexpected values with NA
    )
  )


# Fit the linear model with interaction
lm_full_int1<-lm(data=complete_aggr_2,log10(stability)~log10(balance_f)+scale(nutrients)*scale(temperature)+richness)
lm_full_int2<-lm(data=complete_aggr_2,log10(stability)~log10(balance_f)+ log10(balance_r)+scale(nutrients)*scale(temperature)+richness)
```

**Table 7**: Anova table of the model with only realised balance vs one with both realised and fundamental balance as predictors of stability.

``` r
anova5 <- anova(lm_full_int1, lm_full_int2, test = "Chisq")


anova_tidy5 <- broom::tidy(anova5)
# Display the tidy ANOVA table using gt with formatted p-values and adjusted size
anova_tidy5 %>%
  gt() %>%
  cols_label(
    term = "Term",
    df.residual = "DF",
    rss = "RSS",
    sumsq = "sumsq",
    p.value = "p-value"
  ) %>%
  fmt_number(
    columns = vars(p.value),
    decimals = 3
  ) %>%
  tab_style(
    style = cell_text(weight = "bold"),
    locations = cells_body(
      columns = vars(p.value),
      rows = p.value < 0.05
    )
  ) %>%
  tab_options(
    table.width = px(800),            # Adjust table width (e.g., 400px)
    table.font.size = px(12),        # Adjust font size (e.g., 12px)
    data_row.padding = px(10)         # Adjust row padding (e.g., 4px for more compact rows)
  )
```

```{=html}
<div id="xpinujntdk" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#xpinujntdk table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#xpinujntdk thead, #xpinujntdk tbody, #xpinujntdk tfoot, #xpinujntdk tr, #xpinujntdk td, #xpinujntdk th {
  border-style: none;
}

#xpinujntdk p {
  margin: 0;
  padding: 0;
}

#xpinujntdk .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 12px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: 800px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#xpinujntdk .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#xpinujntdk .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#xpinujntdk .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#xpinujntdk .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#xpinujntdk .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#xpinujntdk .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#xpinujntdk .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#xpinujntdk .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#xpinujntdk .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#xpinujntdk .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#xpinujntdk .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#xpinujntdk .gt_spanner_row {
  border-bottom-style: hidden;
}

#xpinujntdk .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#xpinujntdk .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#xpinujntdk .gt_from_md > :first-child {
  margin-top: 0;
}

#xpinujntdk .gt_from_md > :last-child {
  margin-bottom: 0;
}

#xpinujntdk .gt_row {
  padding-top: 10px;
  padding-bottom: 10px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#xpinujntdk .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#xpinujntdk .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#xpinujntdk .gt_row_group_first td {
  border-top-width: 2px;
}

#xpinujntdk .gt_row_group_first th {
  border-top-width: 2px;
}

#xpinujntdk .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#xpinujntdk .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#xpinujntdk .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#xpinujntdk .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#xpinujntdk .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#xpinujntdk .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#xpinujntdk .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#xpinujntdk .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#xpinujntdk .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#xpinujntdk .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#xpinujntdk .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#xpinujntdk .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#xpinujntdk .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#xpinujntdk .gt_left {
  text-align: left;
}

#xpinujntdk .gt_center {
  text-align: center;
}

#xpinujntdk .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#xpinujntdk .gt_font_normal {
  font-weight: normal;
}

#xpinujntdk .gt_font_bold {
  font-weight: bold;
}

#xpinujntdk .gt_font_italic {
  font-style: italic;
}

#xpinujntdk .gt_super {
  font-size: 65%;
}

#xpinujntdk .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#xpinujntdk .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#xpinujntdk .gt_indent_1 {
  text-indent: 5px;
}

#xpinujntdk .gt_indent_2 {
  text-indent: 10px;
}

#xpinujntdk .gt_indent_3 {
  text-indent: 15px;
}

#xpinujntdk .gt_indent_4 {
  text-indent: 20px;
}

#xpinujntdk .gt_indent_5 {
  text-indent: 25px;
}

#xpinujntdk .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#xpinujntdk div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="term">Term</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="df.residual">DF</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="rss">RSS</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="df">df</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="sumsq">sumsq</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="p.value">p-value</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="term" class="gt_row gt_left">log10(stability) ~ log10(balance_f) + scale(nutrients) * scale(temperature) + richness</td>
<td headers="df.residual" class="gt_row gt_right">236</td>
<td headers="rss" class="gt_row gt_right">3.624661</td>
<td headers="df" class="gt_row gt_right">NA</td>
<td headers="sumsq" class="gt_row gt_right">NA</td>
<td headers="p.value" class="gt_row gt_right">NA</td></tr>
    <tr><td headers="term" class="gt_row gt_left">log10(stability) ~ log10(balance_f) + log10(balance_r) + scale(nutrients) * scale(temperature) + richness</td>
<td headers="df.residual" class="gt_row gt_right">235</td>
<td headers="rss" class="gt_row gt_right">3.609549</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="sumsq" class="gt_row gt_right">0.01511231</td>
<td headers="p.value" class="gt_row gt_right">0.321</td></tr>
  </tbody>
  
  
</table>
</div>
```



A model with both fundamental and realised imbalance as predictors improved very little the variance explained by the model. The two models are not significantly different, suggesting that fundamental imbalance captures well the effect of imbalance on stability, and addiing the species contribution to total biomass (realised imbalnce) does not improved the model.

We now compare also the AIC of the two models

``` r
AIC(lm_full_int1, lm_full_int2)
```

```
##              df       AIC
## lm_full_int1  8 -316.2839
## lm_full_int2  9 -315.2992
```

The AIC of the model with only fundamental imbalance is lower than the AIC of the model with both fundamental and realised imbalance, suggesting that the model with only fundamental imbalance is a better model. However, the difference is very small. 






<!-- # ```{r} -->
<!-- # # Compare AIC -->
<!-- # AIC(lm_balance_f, lm_balance_r, lm_full_int1, lm_full_int2) -->
<!-- #  -->
<!-- # # Compare adjusted R² -->
<!-- # summary(lm_balance_f)$adj.r.squared -->
<!-- #  -->
<!-- # summary(lm_balance_r)$adj.r.squared -->
<!-- #  -->
<!-- # # ANOVA comparison (nested models) -->
<!-- # anova(lm_balance_f, lm_full_int2) -->
<!-- # anova(lm_balance_r, lm_full_int2) -->
<!-- # ``` -->


<!-- # ```{r} -->
<!-- # # Full model with both variables -->
<!-- # lm_both <- lm(data = complete_aggr_2,  -->
<!-- #               log10(stability) ~ log10(balance_f) + log10(balance_r) + scale(nutrients)*scale(temperature) + richness) -->
<!-- #  -->
<!-- # # Get R² values -->
<!-- # r2_f <- summary(lm_balance_f)$r.squared -->
<!-- # r2_r <- summary(lm_balance_r)$r.squared -->
<!-- # r2_both <- summary(lm_both)$r.squared -->
<!-- #  -->
<!-- # # Shared variance -->
<!-- # shared_variance <- r2_both - (r2_f + r2_r) -->
<!-- # shared_variance -->
<!-- #  -->
<!-- # ``` -->





<!-- ```{r} -->

<!-- # Example: Using the R-squared values from the models -->
<!-- r2_f <- summary(lm_balance_f)$r.squared  # R² for balance_f model -->
<!-- r2_r <- summary(lm_balance_r)$r.squared  # R² for balance_r model -->
<!-- r2_both <- summary(lm_both)$r.squared    # R² for model with both variables -->

<!-- # Calculate shared variance -->
<!-- shared_variance <- r2_both - (r2_f + r2_r) -->
<!-- total_variance <- r2_both -->

<!-- # Percentage of shared variance -->
<!-- shared_percentage <- (shared_variance / total_variance) * 100 -->
<!-- shared_percentage -->

<!-- ``` -->


# Linear models

## Dependence of Imbalance on Temperature

Since imbalance depends on the slopes of species' responses, and the slopes of species responses between two environmental conditions depend on the shape of species responses and the position of the optimum of the species response curve, we may expect that imbalance depends on temperature. For example, when the environment (i.e. temperature) fluctuates at higher mean temperatures, the species responses may be more similar (i.e. all species have the same sign of response), leading to less symmetrical response distributions, and thus higher imbalance.

We look now at how imbalance changes with temperature, in all possible combinations of species responses for each richness level and each environmental condition. We calculate the sum of slopes of all possible combinations of species responses, and then calculate the imbalance for each combination. We then plot the relationship between imbalance and temperature for each combination of species responses.




<img src="Extended_Data_files/figure-html/effect_T-1.png" style="display: block; margin: auto;" />

**Figure 7**: Relationship between imbalance and temperature for all possible combinations of species responses. The points represent the imbalance of each combination of species responses The points are colored by temperature regime, and the facets represent whether the combination of species responses was selected for the experiment.


We can see that imbalance increases with temperature, and that the relationship is the same in selected vs non selected community compositions. This suggests that imbalance is intrinsically dependent on temperatures, and that the species responses are more similar when the environment fluctuates at higher temperatures.
Inevitably, this will affect the stability of the community, as we have seen in the previous analysis. Yet, it also mean that the effect of imbalance on stability is not independent of temperature, or more generally, of the environmental conditions. Indeed, temperature and nutrients have been used to estimate species' responses, and thus imbalance ineherently depends on the environment.

Let's look at the variance explained by imbalance and the environment on stability.
<img src="Extended_Data_files/figure-html/Rsquared-1.png" style="display: block; margin: auto;" />
**Figure 8**: Variance explained by imbalance and the environment on stability. The total variance explained by imbalance and the environment is shown in grey, and the unique variance explained by imbalance and the environment is shown in light grey.


95% of the variance explained by balance is shared with nutrients and temperature, which is not suprising considering how imbalance is calculated. However, there is additional variance explained by the environment that is not shared with balance. This suggests that the environment has an independent effect on stability, which is not mediated by imbalance.
This may be related to the fact that imbalance was calculated using monoculture data collected at constant temperature, whereas in the community experiment, temperature was fluctuating, and species were interacting.

To disentangle the shared variance between imbalance, temperature, and nutrients, we first removed the variance in temperature and nutrients that could be explained by imbalance. This can be done by regressing temperature and nutrients on imbalance and extracting the residuals, a method previously used to isolate independent effects among collinear predictors (e.g., [Dormann et al. 2013](https://nsojournals.onlinelibrary.wiley.com/doi/full/10.1111/j.1600-0587.2012.07348.x); [Graham 2003](https://esajournals.onlinelibrary.wiley.com/doi/full/10.1890/02-3114?casa_token=tjLz0BPcLf4AAAAA%3AYjunSb9xlIaHNsO0YLwGDjWzZswdTkVOWfcsBfIC3WjrA-n5B_94cOP2LFP5WIXMhunDwZOPFXCQqg)). The residual temperature and nutrient values thus represented variation independent of imbalance, allowing us to test their effects on stability without confounding influences of shared variance with imbalance. This approach enabled us to evaluate the relative importance of species richness and imbalance while accounting for environmental variability. By including residual temperature and nutrient values in the model, we ensured that our analysis focused on their independent contributions to stability.


## Control effect of environment on balance



Now, we can build a linear model with imbalance, richness, the residuals of temperature and nutrients and their interaction as predictors of stability. We also use composition as random effect to account for some compositions being used in more than one treatment combination. 

We also want to test whether the inclusion of random slopes for log10(balance_f) improves the model’s ability to explain variability in log10(stability) among compositions. We will compare the model with random slopes for log10(balance_f) to the model without random slopes for log10(balance_f) using a Likelihood Ratio Test.


```
## Data: complete_aggr_2
## Models:
## model_no_random: log10(stability) ~ log10(balance_f) + richness + resid_temp * resid_nut + (1 | composition)
## model_with_random: log10(stability) ~ log10(balance_f) + resid_temp * resid_nut + richness + (1 + log10(balance_f) | composition)
##                   npar     AIC     BIC logLik deviance  Chisq Df Pr(>Chisq)
## model_no_random      9 -501.11 -469.68 259.56  -519.11                     
## model_with_random   11 -499.25 -460.83 260.63  -521.25 2.1398  2      0.343
```

A model with random slopes for log10(balance_f) is not better than one without. Thus, we will use the model without random slopes for log10(balance_f) for the rest of the analysis.


``` r
# check model's assumptions
check_model(model_no_random)
```

<div class="figure" style="text-align: center">
<img src="Extended_Data_files/figure-html/model_check_int-1.png" alt="model check 1."  />
<p class="caption">(\#fig:model_check_int)model check 1.</p>
</div>

**Figure 9**: Model check for the linear mixed-effects model with balance, richness, and the residuals of temperature and nutrients as predictors of stability. The model fits well the data.


**Table 10**: Linear mixed-effects model results for the effects of balance, richness, and the residuals of temperature and nutrients on community stability. Estimates are presented with 95% confidence intervals and p-values. 


```{=html}
<div id="byquuxinvl" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#byquuxinvl table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#byquuxinvl thead, #byquuxinvl tbody, #byquuxinvl tfoot, #byquuxinvl tr, #byquuxinvl td, #byquuxinvl th {
  border-style: none;
}

#byquuxinvl p {
  margin: 0;
  padding: 0;
}

#byquuxinvl .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#byquuxinvl .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#byquuxinvl .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#byquuxinvl .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#byquuxinvl .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#byquuxinvl .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#byquuxinvl .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#byquuxinvl .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#byquuxinvl .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#byquuxinvl .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#byquuxinvl .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#byquuxinvl .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#byquuxinvl .gt_spanner_row {
  border-bottom-style: hidden;
}

#byquuxinvl .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#byquuxinvl .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#byquuxinvl .gt_from_md > :first-child {
  margin-top: 0;
}

#byquuxinvl .gt_from_md > :last-child {
  margin-bottom: 0;
}

#byquuxinvl .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#byquuxinvl .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#byquuxinvl .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#byquuxinvl .gt_row_group_first td {
  border-top-width: 2px;
}

#byquuxinvl .gt_row_group_first th {
  border-top-width: 2px;
}

#byquuxinvl .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#byquuxinvl .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#byquuxinvl .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#byquuxinvl .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#byquuxinvl .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#byquuxinvl .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#byquuxinvl .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#byquuxinvl .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#byquuxinvl .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#byquuxinvl .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#byquuxinvl .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#byquuxinvl .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#byquuxinvl .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#byquuxinvl .gt_left {
  text-align: left;
}

#byquuxinvl .gt_center {
  text-align: center;
}

#byquuxinvl .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#byquuxinvl .gt_font_normal {
  font-weight: normal;
}

#byquuxinvl .gt_font_bold {
  font-weight: bold;
}

#byquuxinvl .gt_font_italic {
  font-style: italic;
}

#byquuxinvl .gt_super {
  font-size: 65%;
}

#byquuxinvl .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#byquuxinvl .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#byquuxinvl .gt_indent_1 {
  text-indent: 5px;
}

#byquuxinvl .gt_indent_2 {
  text-indent: 10px;
}

#byquuxinvl .gt_indent_3 {
  text-indent: 15px;
}

#byquuxinvl .gt_indent_4 {
  text-indent: 20px;
}

#byquuxinvl .gt_indent_5 {
  text-indent: 25px;
}

#byquuxinvl .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#byquuxinvl div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_heading">
      <td colspan="5" class="gt_heading gt_title gt_font_normal gt_bottom_border" style>Linear Regression Results</td>
    </tr>
    
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="label"><span class='gt_from_md'><strong>Predictor</strong></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="estimate"><span class='gt_from_md'><strong>Estimate</strong></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="conf.low"><span class='gt_from_md'><strong>95% CI</strong></span><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="t_value"><span class='gt_from_md'><strong>t-value</strong></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="p.value"><span class='gt_from_md'><strong>p-value</strong></span></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="label" class="gt_row gt_left" style="font-weight: bold;">Intercept</td>
<td headers="estimate" class="gt_row gt_center">-0.32</td>
<td headers="conf.low" class="gt_row gt_center">-0.39, -0.24</td>
<td headers="t_value" class="gt_row gt_center">-8.84</td>
<td headers="p.value" class="gt_row gt_center" style="font-weight: bold;"><0.001</td></tr>
    <tr><td headers="label" class="gt_row gt_left" style="font-weight: bold;">log10(balance_f)</td>
<td headers="estimate" class="gt_row gt_center">-0.08</td>
<td headers="conf.low" class="gt_row gt_center">-0.09, -0.06</td>
<td headers="t_value" class="gt_row gt_center">-9.95</td>
<td headers="p.value" class="gt_row gt_center" style="font-weight: bold;"><0.001</td></tr>
    <tr><td headers="label" class="gt_row gt_left" style="font-weight: bold;">richness</td>
<td headers="estimate" class="gt_row gt_center"><br /></td>
<td headers="conf.low" class="gt_row gt_center"><br /></td>
<td headers="t_value" class="gt_row gt_center"><br /></td>
<td headers="p.value" class="gt_row gt_center"><br /></td></tr>
    <tr><td headers="label" class="gt_row gt_left">    richness3 - richness2</td>
<td headers="estimate" class="gt_row gt_center">-0.01</td>
<td headers="conf.low" class="gt_row gt_center">-0.15, 0.12</td>
<td headers="t_value" class="gt_row gt_center">-0.27</td>
<td headers="p.value" class="gt_row gt_center">>0.9</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    richness4 - richness2</td>
<td headers="estimate" class="gt_row gt_center">0.01</td>
<td headers="conf.low" class="gt_row gt_center">-0.15, 0.17</td>
<td headers="t_value" class="gt_row gt_center">0.13</td>
<td headers="p.value" class="gt_row gt_center">>0.9</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    richness4 - richness3</td>
<td headers="estimate" class="gt_row gt_center">0.02</td>
<td headers="conf.low" class="gt_row gt_center">-0.14, 0.18</td>
<td headers="t_value" class="gt_row gt_center">0.35</td>
<td headers="p.value" class="gt_row gt_center">>0.9</td></tr>
    <tr><td headers="label" class="gt_row gt_left" style="font-weight: bold;">resid_temp</td>
<td headers="estimate" class="gt_row gt_center">-0.10</td>
<td headers="conf.low" class="gt_row gt_center">-0.12, -0.08</td>
<td headers="t_value" class="gt_row gt_center">-10.55</td>
<td headers="p.value" class="gt_row gt_center" style="font-weight: bold;"><0.001</td></tr>
    <tr><td headers="label" class="gt_row gt_left" style="font-weight: bold;">resid_nut</td>
<td headers="estimate" class="gt_row gt_center">0.25</td>
<td headers="conf.low" class="gt_row gt_center">0.21, 0.28</td>
<td headers="t_value" class="gt_row gt_center">13.80</td>
<td headers="p.value" class="gt_row gt_center" style="font-weight: bold;"><0.001</td></tr>
    <tr><td headers="label" class="gt_row gt_left" style="font-weight: bold;">resid_temp * resid_nut</td>
<td headers="estimate" class="gt_row gt_center">-0.09</td>
<td headers="conf.low" class="gt_row gt_center">-0.15, -0.03</td>
<td headers="t_value" class="gt_row gt_center">-2.93</td>
<td headers="p.value" class="gt_row gt_center" style="font-weight: bold;">0.004</td></tr>
  </tbody>
  
  <tfoot class="gt_footnotes">
    <tr>
      <td class="gt_footnote" colspan="5"><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span> <span class='gt_from_md'>CI = Confidence Interval</span></td>
    </tr>
  </tfoot>
</table>
</div>
```

The linear mixed-effects model was fitted by maximum likelihood (ML) with log-transformed stability as the response variable. The fixed effects included log10(balance_f), richness, the residuals of temperature (resid_temp), and nutrients (resid_nut), as well as their interaction term. The random effect was specified for the composition group, which accounted for potential variability between different composition categories.

The model’s results indicated that log10(balance_f) (Estimate = -0.078, SE = 0.0079, p < 2e-16), resid_temp (Estimate = -0.099, SE = 0.0093, p < 2e-16), and resid_nut (Estimate = 0.248, SE = 0.018, p < 2e-16) all had significant effects on stability, with both resid_temp and resid_nut showing large, statistically significant relationships with stability. Additionally, the interaction term resid_temp:resid_nut (Estimate = -0.091, SE = 0.031, p = 0.0037) was also significant, indicating that the combined effects of temperature and nutrients on stability are not independent, but interact in shaping stability.

Richness levels (richness3 and richness4) did not show significant effects on stability (p > 0.77), suggesting that, within the context of this model, variations in richness do not explain substantial variation in community stability.


**Table 11**: ANOVA table of the linear mixed-effects model with balance, richness, and the residuals of temperature and nutrients as predictors of stability.


```{=html}
<div id="gppovdsqrc" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#gppovdsqrc table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#gppovdsqrc thead, #gppovdsqrc tbody, #gppovdsqrc tfoot, #gppovdsqrc tr, #gppovdsqrc td, #gppovdsqrc th {
  border-style: none;
}

#gppovdsqrc p {
  margin: 0;
  padding: 0;
}

#gppovdsqrc .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 20px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: 800px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#gppovdsqrc .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#gppovdsqrc .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#gppovdsqrc .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#gppovdsqrc .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#gppovdsqrc .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#gppovdsqrc .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#gppovdsqrc .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#gppovdsqrc .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#gppovdsqrc .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#gppovdsqrc .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#gppovdsqrc .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#gppovdsqrc .gt_spanner_row {
  border-bottom-style: hidden;
}

#gppovdsqrc .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#gppovdsqrc .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#gppovdsqrc .gt_from_md > :first-child {
  margin-top: 0;
}

#gppovdsqrc .gt_from_md > :last-child {
  margin-bottom: 0;
}

#gppovdsqrc .gt_row {
  padding-top: 10px;
  padding-bottom: 10px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#gppovdsqrc .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#gppovdsqrc .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#gppovdsqrc .gt_row_group_first td {
  border-top-width: 2px;
}

#gppovdsqrc .gt_row_group_first th {
  border-top-width: 2px;
}

#gppovdsqrc .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#gppovdsqrc .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#gppovdsqrc .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#gppovdsqrc .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#gppovdsqrc .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#gppovdsqrc .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#gppovdsqrc .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#gppovdsqrc .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#gppovdsqrc .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#gppovdsqrc .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#gppovdsqrc .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#gppovdsqrc .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#gppovdsqrc .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#gppovdsqrc .gt_left {
  text-align: left;
}

#gppovdsqrc .gt_center {
  text-align: center;
}

#gppovdsqrc .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#gppovdsqrc .gt_font_normal {
  font-weight: normal;
}

#gppovdsqrc .gt_font_bold {
  font-weight: bold;
}

#gppovdsqrc .gt_font_italic {
  font-style: italic;
}

#gppovdsqrc .gt_super {
  font-size: 65%;
}

#gppovdsqrc .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#gppovdsqrc .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#gppovdsqrc .gt_indent_1 {
  text-indent: 5px;
}

#gppovdsqrc .gt_indent_2 {
  text-indent: 10px;
}

#gppovdsqrc .gt_indent_3 {
  text-indent: 15px;
}

#gppovdsqrc .gt_indent_4 {
  text-indent: 20px;
}

#gppovdsqrc .gt_indent_5 {
  text-indent: 25px;
}

#gppovdsqrc .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#gppovdsqrc div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="term">Term</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="statistic">F Statistic</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="df">DF</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="p.value">p-value</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="term" class="gt_row gt_left">(Intercept)</td>
<td headers="statistic" class="gt_row gt_right">78.2322254</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.000</td></tr>
    <tr><td headers="term" class="gt_row gt_left">log10(balance_f)</td>
<td headers="statistic" class="gt_row gt_right">99.0517826</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.000</td></tr>
    <tr><td headers="term" class="gt_row gt_left">richness</td>
<td headers="statistic" class="gt_row gt_right">0.1612692</td>
<td headers="df" class="gt_row gt_right">2</td>
<td headers="p.value" class="gt_row gt_right">0.923</td></tr>
    <tr><td headers="term" class="gt_row gt_left">resid_temp</td>
<td headers="statistic" class="gt_row gt_right">111.3786982</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.000</td></tr>
    <tr><td headers="term" class="gt_row gt_left">resid_nut</td>
<td headers="statistic" class="gt_row gt_right">190.4777948</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.000</td></tr>
    <tr><td headers="term" class="gt_row gt_left">resid_temp:resid_nut</td>
<td headers="statistic" class="gt_row gt_right">8.5784507</td>
<td headers="df" class="gt_row gt_right">1</td>
<td headers="p.value" class="gt_row gt_right" style="font-weight: bold;">0.003</td></tr>
  </tbody>
  
  
</table>
</div>
```



# Asynchrony
Response diversity (one of the stabilisng effects captured by imbalance) has been suggested as a mechanism that promotes temporal stability of community biomass by promoting species asynchrony.

We thus calculated the asynchrony index suggested by [Gross et al. 2014](https://www.journals.uchicago.edu/doi/epdf/10.1086/673915) to calculate the effect of asynchrony on temporal stability and to see how response diversity relate to asynchrony.
The index ranges between -1 and 1, with -1 indicating perfect asynchrony and 1 being perfectly synchronous, and 0 indicating random variation.





### Plot stability vs. Asynchrony Gross
<img src="Extended_Data_files/figure-html/async_plots-1.png" style="display: block; margin: auto;" />


**Figure 10**: Relationship between temporal stability and asynchrony (Gross) divided by nutrient level.




### Plot Asynchrony Gross vs fundamental imbalance

<img src="Extended_Data_files/figure-html/async-1.png" style="display: block; margin: auto;" />
**Figure 11**: Relationship between asynchrony (Gross) and fundamental imbalance divided by nutrient level.







<!-- ```{r fig.align="center", fig.height=12, fig.width=16} -->

<!-- plot_asynch_CV_G <- plot_asynch_CV_G + -->
<!--   labs(tag = "(a)") + -->
<!--   theme(plot.tag = element_text(size = 20))+ theme(legend.position = "none") -->

<!-- plot_asynch_B_G <- plot_asynch_B_G + -->
<!--   labs(tag = "(b)") + -->
<!--   theme(plot.tag = element_text(size = 20)) + theme(legend.position = "none") -->

<!-- plot_pop_aggr_balance <- plot_pop_aggr_balance + -->
<!--   labs(tag = "(c)") + -->
<!--   theme(plot.tag = element_text(size = 20)) -->

<!-- plot_pop_balance <- plot_pop_balance + -->
<!--   labs(tag = "(d)") + -->
<!--   theme(plot.tag = element_text(size = 20))+ theme(legend.position = "none") -->




<!-- # Combine plots and share the legend -->
<!-- fig3 <- (plot_asynch_CV_G + plot_asynch_B_G) /  -->
<!--         (plot_pop_aggr_balance + plot_pop_balance) +  -->
<!--         plot_layout(guides = "collect") -->

<!-- fig3 -->




<!-- # ggsave("figures_ms/fig.3.png", plot = fig3, width = 18, height = 12, dpi = 600) -->
<!-- ``` -->



## Eveness
Evenness in species biomass has been identified as an important factor potentially influencing ecosystem stability [Thibaut & Connolly 2013](https://onlinelibrary.wiley.com/doi/full/10.1111/ele.12019). In the context of our experiment, evenness in species biomass could help explaining why there is little difference between fundamental and realized imbalance. If evenness is high, then all species contribute similarly to total biomass. In this case, weighting for species-biomass contribution to total biomass (realized), should not fundamentally change the result, compared to an un-weighted (fundamental) measurement. 
<img src="Extended_Data_files/figure-html/eveness-1.png" style="display: block; margin: auto;" />
**Figure 12**: Distribution of species evenness across experimental communities. The histogram represents the frequency of observed evenness values, while the red dashed line indicates the mean evenness (0.7). This highlights the central tendency of evenness across the dataset and its variation among communities.

Evenness was indeed generally high in our experimental communities, suggesting another potential factor reducing the potential difference between fundamental and realized balance. 

# Population stability

The relationship between community stability and the stability of the individual populations that make up the community is a key question in ecology. Importantly, ecosystem stability can result from low population stability, if populations fluctuate asynchronously, or from high population stability, if populations do not fluctuate much.
Synthesis of the literature suggests diversity can have a positive or negantive effect on population stability [Campbell et al 2010](https://nsojournals.onlinelibrary.wiley.com/doi/full/10.1111/j.1600-0706.2010.18768.x) and (Xu et al 2021)[https://onlinelibrary.wiley.com/doi/full/10.1111/ele.13777].

Theoretical work has suggested that community stability is a product of two quantities: the (a) synchrony of population fluctuations, and an average species-level population stability that is weighted by relative abundance [Thibaut & Connolly 2013](https://onlinelibrary.wiley.com/doi/full/10.1111/ele.12019). 

Critically, a imbalance value close to zero can result from high response diversity, but also from high population stability (population biomass does not change largely over time).
We want to look now at whether our new metric of imbalance can capture these two stabilising mechanisms.

Thus, we calculate species-level population stability weighted by relative abundance and look at how it relates to ecosystem stability. 

![](Extended_Data_files/figure-html/unnamed-chunk-23-1.png)<!-- -->

**Figure 13**: Relationship between log10 of population stability and log 10 of ecosystem stability.  


<img src="Extended_Data_files/figure-html/pop_balance-1.png" style="display: block; margin: auto;" />

**Figure 14**: Relationship between fundamental imbalance and population stability divided by nutrient level.

# SEM 


Finally, we use a structural equation model (SEM) to explore how stability is influenced by asynchrony, population stability, imbalance and, nutrient levels. 
In order to develop a hypothesis regarding the influence of stability, we have drawn on existing literature. This has enabled us to posit that stability is influenced by two key factors: asynchrony and population stability. In turn, these are influenced by balance and, in our particular case, by nutrient levels.



```
## lavaan 0.6-19 ended normally after 1 iteration
## 
##   Estimator                                         ML
##   Optimization method                           NLMINB
##   Number of model parameters                        12
## 
##   Number of observations                           241
## 
## Model Test User Model:
##                                               Standard      Scaled
##   Test Statistic                                 2.502       2.362
##   Degrees of freedom                                 3           3
##   P-value (Chi-square)                           0.475       0.501
##   Scaling correction factor                                  1.059
##     Satorra-Bentler correction                                    
## 
## Model Test Baseline Model:
## 
##   Test statistic                               852.984     819.012
##   Degrees of freedom                                 9           9
##   P-value                                        0.000       0.000
##   Scaling correction factor                                  1.041
## 
## User Model versus Baseline Model:
## 
##   Comparative Fit Index (CFI)                    1.000       1.000
##   Tucker-Lewis Index (TLI)                       1.002       1.002
##                                                                   
##   Robust Comparative Fit Index (CFI)                         1.000
##   Robust Tucker-Lewis Index (TLI)                            1.002
## 
## Loglikelihood and Information Criteria:
## 
##   Loglikelihood user model (H0)                516.122     516.122
##   Loglikelihood unrestricted model (H1)             NA          NA
##                                                                   
##   Akaike (AIC)                               -1008.245   -1008.245
##   Bayesian (BIC)                              -966.427    -966.427
##   Sample-size adjusted Bayesian (SABIC)      -1004.464   -1004.464
## 
## Root Mean Square Error of Approximation:
## 
##   RMSEA                                          0.000       0.000
##   90 Percent confidence interval - lower         0.000       0.000
##   90 Percent confidence interval - upper         0.101       0.097
##   P-value H_0: RMSEA <= 0.050                    0.692       0.723
##   P-value H_0: RMSEA >= 0.080                    0.127       0.107
##                                                                   
##   Robust RMSEA                                               0.000
##   90 Percent confidence interval - lower                     0.000
##   90 Percent confidence interval - upper                     0.102
##   P-value H_0: Robust RMSEA <= 0.050                         0.703
##   P-value H_0: Robust RMSEA >= 0.080                         0.126
## 
## Standardized Root Mean Square Residual:
## 
##   SRMR                                           0.018       0.018
## 
## Parameter Estimates:
## 
##   Standard errors                           Robust.sem
##   Information                                 Expected
##   Information saturated (h1) model          Structured
## 
## Regressions:
##                      Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
##   stability ~                                                             
##     asynchrny_Grss      0.157    0.011   14.493    0.000    0.157    0.326
##     pop_stability       0.957    0.022   42.596    0.000    0.957    0.987
##   asynchrony_Gross ~                                                      
##     log_balance_f      -0.093    0.034   -2.767    0.006   -0.093   -0.185
##     nutrients          -0.211    0.023   -9.072    0.000   -0.211   -0.503
##   pop_stability ~                                                         
##     log_balance_f      -0.073    0.011   -6.852    0.000   -0.073   -0.292
##     nutrients           0.136    0.008   17.854    0.000    0.136    0.653
## 
## Intercepts:
##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
##    .stability         0.098    0.011    8.847    0.000    0.098    0.594
##    .asynchrny_Grss   -0.090    0.055   -1.625    0.104   -0.090   -0.262
##    .pop_stability    -0.630    0.018  -34.512    0.000   -0.630   -3.702
## 
## Variances:
##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
##    .stability         0.003    0.000    7.632    0.000    0.003    0.099
##    .asynchrny_Grss    0.088    0.011    8.214    0.000    0.088    0.753
##    .pop_stability     0.012    0.001   11.568    0.000    0.012    0.406
## 
## R-Square:
##                    Estimate
##     stability         0.901
##     asynchrny_Grss    0.247
##     pop_stability     0.594
```

<div class="figure" style="text-align: center">
<img src="figures_ms/SEM_3.png" alt="SEM." width="6949" />
<p class="caption">(\#fig:SEM)SEM.</p>
</div>
**Figure 15**: Structural equation model (SEM) showing the relationships between stability, asynchrony, population stability, imbalance, and nutrient levels. The model includes standardized regression coefficients and fit indices.

**Model Fit Indices**
The model fit indices suggest that the model fits the data well.

*Chi-Square Test (User Model)*: The chi-square test statistic for the user model is χ 2 =1.626 (scaled = 1.465) with 3 degrees of freedom and a p-value of 0.653 (scaled = 0.690). This indicates a good fit, as the test is non-significant, suggesting no significant difference between the observed and model-implied covariance matrices.

*Comparative Fit Index (CFI)* and *Tucker-Lewis Index (TLI)*: Both CFI and TLI values are 1.000, indicating an excellent model fit. Values close to or above 0.95 are generally considered good.

*Root Mean Square Error of Approximation (RMSEA)*: The RMSEA is 0.000, with a 90% confidence interval ranging from 0 to 0.090 (scaled = 0.080). This indicates a very good fit, as RMSEA values below 0.05 are ideal, and values below 0.08 are acceptable. The p-values for the RMSEA hypothesis tests suggest strong support for a close fit (RMSEA <= 0.05) and little evidence for a poor fit (RMSEA >= 0.08).

*Standardized Root Mean Square Residual (SRMR)*: The SRMR value is 0.017, which is also within the acceptable range (values below 0.08 are generally considered good).
Overall, the fit indices suggest that the model is an excellent fit for the data.

**Regression Paths and Interpretation**

**Stability Regressions**

*Stability ~ Asynchrony_Gross (asynchrny_Grss)*: The standardized estimate for the effect of asynchrony on stability is 0.340 (p < 0.001), indicating a significant positive association. Higher asynchrony in species dynamics is associated with increased community stability.

*Stability ~ Population Stability (pop_stability)*: The standardized estimate is 0.977 (p < 0.001), showing a strong positive relationship. This suggests that community stability is highly dependent on the stability of individual populations within the community.

**Asynchrony_Gross Regressions**

*Asynchrony_Gross ~ Log10(Balance):* The standardized estimate is -0.176 (p = 0.013), indicating a significant negative effect. Higher imbalance leads to lower asynchrony, suggesting that as imbalance increases, species within the community fluctuate more synchronously.

*Asynchrony_Gross ~ Nutrients:* The standardized estimate is -0.469 (p < 0.001), showing a strong negative relationship. Higher nutrient levels appear to reduce asynchrony, possibly by causing similar responses across species.

**Population Stability Regressions**

*Population Stability ~ Log10(Balance)*: The standardized estimate is -0.296 (p < 0.001), indicating that higher imbalance is associated with lower population stability.

*Population Stability ~ Nutrients*: The standardized estimate is 0.635 (p < 0.001), showing that higher nutrient levels are associated with increased population stability, likely because nutrients enhance conditions that support stable population dynamics.

**Variances and R-Squared Values**
*R-Squared for Stability*: The model explains 90.4% of the variance in community stability, indicating strong predictive power.

*R-Squared for Asynchrony_Gross*: The model explains 21.9% of the variance in asynchrony, which is moderate.

*R-Squared for Population Stability*: The model explains 56.2% of the variance in population stability, showing that nutrients and balance are important but not the only factors influencing it.

*Summary Interpretation*
Model Fit: The model has an excellent fit, as indicated by the fit indices.
Stability: Community stability is strongly influenced by both population stability and asynchrony among species, with population stability being the stronger predictor.
Asynchrony and Imbalance: Asynchrony decreases with increasing imbalance and nutrients, suggesting that these factors promote more synchronized fluctuations among species.
Population Stability and Nutrients: Higher nutrient levels are associated with increased population stability, suggesting that nutrient availability supports stable population dynamics. Conversely, higher imbalance is associated with decreased population stability.

