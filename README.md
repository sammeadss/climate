Global Temperature Trends in a Warming World
================
Samuel Meads
(02 May, 2024)

- [Introduction](#introduction)
- [Methods](#methods)
  - [Research Objective](#research-objective)
    - [Null Hypotheses (H<sub>0</sub>):](#null-hypotheses-h0)
    - [Alternative Hypothesis
      (H<sub>1</sub>):](#alternative-hypothesis-h1)
  - [Data Analyses](#data-analyses)
    - [Data](#data)
    - [Descriptive Statistics](#descriptive-statistics)
    - [Data Wrangling](#data-wrangling)
    - [Time Series Analysis](#time-series-analysis)
    - [Exploratory Data Analysis](#exploratory-data-analysis)
    - [Correlation Tests](#correlation-tests)
    - [Regression Analysis](#regression-analysis)
- [Results](#results)
  - [CO₂ and Global Average
    Temperature](#co₂-and-global-average-temperature)
  - [CH₄ and Global Average
    Temperature](#ch₄-and-global-average-temperature)
  - [Sea Levels and Global Average
    Temperature](#sea-levels-and-global-average-temperature)
- [Conclusion](#conclusion)
  - [Key Findings](#key-findings)
    - [Strong Correlation between Greenhouse Gases and
      Temperature](#strong-correlation-between-greenhouse-gases-and-temperature)
    - [Moderate Correlation between Sea Level and
      Temperature](#moderate-correlation-between-sea-level-and-temperature)
    - [Regression Analysis Insights](#regression-analysis-insights)
  - [Implications for Climate Science and
    Policy](#implications-for-climate-science-and-policy)
    - [Model Validation and
      Improvement](#model-validation-and-improvement)
    - [Policy Development](#policy-development)
    - [Further Research](#further-research)
- [References](#references)
  - [Articles](#articles)
  - [Data](#data-1)
  - [Pictures](#pictures)

![](Pictures/ClimateChange.jpg)

``` r
# Import Libraries
library(tidyverse)
library(dplyr)
library(knitr)
library(ggplot2)
library(tidyr)
library(reticulate)
library(kableExtra)
```

# Introduction

The relationship between the escalating concentrations of greenhouse
gases, specifically carbon dioxide (CO₂) and methane (CH₄), and the
increases in global average temperatures support contemporary
understanding of climate dynamics. This connection not only highlights
the anthropogenic impacts on climate change but also serves as a
critical point for scientific questions aimed at explaining the effects
and potential mitigation strategies for global warming.

Historical and modern scientific efforts have greatly contributed to the
understanding of this relationship. Eunice Foote and, later in the 19th
century, John Tyndall, with their pioneering experiments, both proved
the heat-trapping characteristics of CO₂, as part of the basic knowledge
toward the greenhouse effect, which is necessary for modern-day climatic
science (Armstrong et al., 2018). In the 20th century, scientists,
including Mann et al. (1998), further attempted to provide an
explanation for the dominant role of the gases in recent climate
variability and to confirm the continual impact that had already been
observed in earlier research by using paleoclimatic information to link
changes in greenhouse gas concentrations with large-scale temperature
patterns. The most recent studies, for instance, Hansen et al. (2006)
and Mann et al. (1998), have not only upheld the early findings with
great analysis but have continued to argue out how these gases influence
modern climatic patterns. Their work indeed underscores near effects of
these gases and points out to the constant increase in the temperatures
around the world, which is in line with past predictions and models that
sought to establish the trajectory of climate change.

They increasingly report that the increase of world temperatures is very
much related to rising sea levels—one of those things they are seeing
more often in climate studies of recent years. Mitchell (1989) pointed
out that the enhanced greenhouse gas concentrations have warmed not only
the atmosphere but have also contributed to its thermal expansion,
melting ice caps, which have emerged as major factors for the increased
observed rise in sea levels over the past century.

Building on these foundational experiments and studies, the present
research aims to delve deeper into the correlations and causal
relationships between the increases in specific greenhouse gases with
global temperature trends. Using context from these studies and modern
technological and methodological advances, this piece of work
contributes to having a more cohesive understanding of the mechanisms
involved in climate change by potentially confirming or challenging past
findings.

# Methods

## Research Objective

The following research aim’s to identify the relationship between
particular greenhouse gases (CO₂ and CH₄) and global average
temperature. Additionally, I want to investigate the effects that global
average temperature has on sea levels. The discoveries made will be
pivotal for enhancing the accuracy of climate models, which are crucial
for predicting and mitigating future climate impacts. Hopefully the
results can better inform policy decisions and develop effective
strategies for protecting coastal communities and ecosystems against the
impending rise of global temperatures and sea levels.

### Null Hypotheses (H<sub>0</sub>):

- There is no significant linear relationship between the levels of CO₂,
  CH₄, and changes in global average temperature.

- There is no significant linear relationship between rising sea levels
  and global average temperature.

### Alternative Hypothesis (H<sub>1</sub>):

- There is a significant linear relationship between CO₂, CH₄, and
  changes in global average temperature.

- There is a significant linear relationship between rising sea levels
  and global average temperature.

## Data Analyses

### Data

The data comes from
[NASA](https://climate.nasa.gov/vital-signs/carbon-dioxide/?intent=111).
The files themselves are only downloadable in .txt format, so I used
used Python to convert these files into .csv format for easier data
observation and manipulation. As a first step, let’s go ahead and load
in the datasets:

``` r
co2_data <- read_csv('https://raw.githubusercontent.com/sammeadss/climate/main/Data/CarbonDioxide.csv')
methane_data <- read_csv('https://raw.githubusercontent.com/sammeadss/climate/main/Data/Methane.csv')
temp_data <- read_csv('https://raw.githubusercontent.com/sammeadss/climate/main/Data/GlobalTemp.csv')
sealevel_data <- read_csv('https://raw.githubusercontent.com/sammeadss/climate/main/Data/SeaLevel.csv')
```

### Descriptive Statistics

Now that the data is loaded, it’s important to follow up with a summary
of the datasets’ contents. This is a key step before continuing with
more complex data analysis; it can help identify trends, patterns, or
anomalies by displaying the basic features that the dataset has to
offer.

``` r
kable(summary(co2_data), format = "markdown", caption = "<b>Carbon Dioxide Data</b>")
```

|     | Year         | Month         | Decimal Date | Monthly Average | De-seasonalized | \#Days        | St.Dev          | Unc. of Mon Mean |
|:----|:-------------|:--------------|:-------------|:----------------|:----------------|:--------------|:----------------|:-----------------|
|     | Min. :1958   | Min. : 1.00   | Min. :1958   | Min. :312.4     | Min. :314.4     | Min. :-1.00   | Min. :-9.9900   | Min. :-0.99000   |
|     | 1st Qu.:1974 | 1st Qu.: 3.75 | 1st Qu.:1975 | 1st Qu.:330.3   | 1st Qu.:330.5   | 1st Qu.:10.75 | 1st Qu.: 0.1675 | 1st Qu.: 0.06750 |
|     | Median :1991 | Median : 6.50 | Median :1991 | Median :355.0   | Median :355.4   | Median :25.00 | Median : 0.4000 | Median : 0.15000 |
|     | Mean :1991   | Mean : 6.50   | Mean :1991   | Mean :358.9     | Mean :358.9     | Mean :19.03   | Mean :-2.0941   | Mean :-0.09773   |
|     | 3rd Qu.:2007 | 3rd Qu.: 9.25 | 3rd Qu.:2008 | 3rd Qu.:384.3   | 3rd Qu.:384.3   | 3rd Qu.:28.00 | 3rd Qu.: 0.5700 | 3rd Qu.: 0.21000 |
|     | Max. :2024   | Max. :12.00   | Max. :2024   | Max. :424.6     | Max. :423.6     | Max. :31.00   | Max. : 1.3100   | Max. : 0.59000   |

<b>Carbon Dioxide Data</b>

``` r
kable(summary(methane_data), format = "markdown", caption = "<b>Methane Data</b>")
```

|     | Year         | Mean         | Uncertainty  |
|:----|:-------------|:-------------|:-------------|
|     | Min. :1984   | Min. :1645   | Min. :0.42   |
|     | 1st Qu.:1994 | 1st Qu.:1739 | 1st Qu.:0.55 |
|     | Median :2003 | Median :1774 | Median :0.63 |
|     | Mean :2003   | Mean :1776   | Mean :0.62   |
|     | 3rd Qu.:2012 | 3rd Qu.:1811 | 3rd Qu.:0.68 |
|     | Max. :2022   | Max. :1912   | Max. :0.82   |

<b>Methane Data</b>

``` r
kable(summary(temp_data), format = "markdown", caption = "<b>Global Temperature Data</b>")
```

|     | Year         | No_Smoothing     | Lowess(5)        |
|:----|:-------------|:-----------------|:-----------------|
|     | Min. :1880   | Min. :-0.48000   | Min. :-0.41000   |
|     | 1st Qu.:1916 | 1st Qu.:-0.20000 | 1st Qu.:-0.22000 |
|     | Median :1952 | Median :-0.04500 | Median :-0.03500 |
|     | Mean :1952   | Mean : 0.06951   | Mean : 0.06917   |
|     | 3rd Qu.:1987 | 3rd Qu.: 0.28000 | 3rd Qu.: 0.28000 |
|     | Max. :2023   | Max. : 1.17000   | Max. : 1.01000   |

<b>Global Temperature Data</b>

``` r
kable(summary(sealevel_data), format = "markdown", caption = "<b>Sea Level Data</b>")
```

|     | Altimeter Type | Merged File Cycle \# | Year + Fraction of Year | Number of Observations | Number of Weighted Observations | GMSL Variation (No GIA) (mm) | Std Dev GMSL (No GIA) (mm) | Smoothed GMSL (No GIA) (mm) | GMSL Variation (GIA) (mm) | Std Dev GMSL (GIA) (mm) | Smoothed GMSL (GIA) (mm) | Smoothed GMSL (GIA, No Seasonal) (mm) | Smoothed GMSL (No GIA, No Seasonal) (mm) |
|:----|:---------------|:---------------------|:------------------------|:-----------------------|:--------------------------------|:-----------------------------|:---------------------------|:----------------------------|:--------------------------|:------------------------|:-------------------------|:--------------------------------------|:-----------------------------------------|
|     | Min. :999      | Min. : 20.0          | Min. :1993              | Min. : 1339            | Min. : 949.8                    | Min. :-38.21                 | Min. : 74.20               | Min. :-35.85                | Min. :-38.15              | Min. : 74.22            | Min. :-35.79             | Min. :-32.85                          | Min. :-32.91                             |
|     | 1st Qu.:999    | 1st Qu.: 98.5        | 1st Qu.:1995            | 1st Qu.:383486         | 1st Qu.:282700.0                | 1st Qu.:-28.35               | 1st Qu.: 87.05             | 1st Qu.:-27.45              | 1st Qu.:-27.66            | 1st Qu.: 87.08          | 1st Qu.:-27.17           | 1st Qu.:-24.44                        | 1st Qu.:-25.53                           |
|     | Median :999    | Median :168.0        | Median :1997            | Median :412730         | Median :306972.8                | Median :-25.60               | Median : 89.70             | Median :-23.69              | Median :-24.61            | Median : 89.74          | Median :-22.73           | Median :-22.36                        | Median :-23.42                           |
|     | Mean :999      | Mean :164.9          | Mean :1997              | Mean :391143           | Mean :288125.6                  | Mean :-23.49                 | Mean : 91.79               | Mean :-22.66                | Mean :-22.45              | Mean : 91.79            | Mean :-21.64             | Mean :-21.67                          | Mean :-22.70                             |
|     | 3rd Qu.:999    | 3rd Qu.:231.5        | 3rd Qu.:1999            | 3rd Qu.:449743         | 3rd Qu.:327720.7                | 3rd Qu.:-17.70               | 3rd Qu.: 95.90             | 3rd Qu.:-17.89              | 3rd Qu.:-16.33            | 3rd Qu.: 95.94          | 3rd Qu.:-16.45           | 3rd Qu.:-16.80                        | 3rd Qu.:-18.14                           |
|     | Max. :999      | Max. :307.0          | Max. :2001              | Max. :468409           | Max. :339368.8                  | Max. : -7.56                 | Max. :113.60               | Max. : -6.95                | Max. : -5.63              | Max. :113.68            | Max. : -5.05             | Max. : -9.83                          | Max. :-11.73                             |

<b>Sea Level Data</b>

Once the summaries load, carefully examine all four tables to possibly
identify any trends, patterns, or anomalies. After doing this with my
data, nothing in particular jumped out at me, which is typical with
larger datasets. However, I noticed that the range for the Year(s)
columns on all four datasets are different; this means I will have to
clean up the data so that when I eventually merge the datasets, there
won’t be any inconsistencies.

### Data Wrangling

In this phase of the analysis, I focus on preparing the data by cleaning
and structurally organizing it into distinct data frames. This
methodical separation ensures that any transformations or manipulations
do not compromise the integrity of the original datasets. Each data
frame is associated with their respective environmental measures: CO₂
levels, CH₄ concentrations, global temperatures, and sea levels.

To begin, I create new data frames from the original datasets. Naming
these data frames explicitly by their content and scope—such as
‘co2_annual’ and ‘methane_annual’—facilitates clearer, more organized
analysis later on. This step is crucial, especially when dealing with
time-series data that spans multiple years, ensuring that analyses
remain consistent across comparable temporal ranges.

For the greenhouse gases analysis, which includes both CO₂ and CH₄ data,
I apply specific filters and transformations to align these datasets
with the period for which complete records are available. Given that the
CH₄ data begins in 1984, I filter the CO₂ and temperature datasets to
start from this year as well, ensuring that the analysis only covers
overlapping periods for accurate comparisons and correlations.

``` r
co2_annual <- co2_data %>%
  filter(Year >= 1984) %>%
  group_by(Year) %>%
  summarize(AnnualAverageCO2 = mean(`Monthly Average`, na.rm = TRUE))

methane_annual <- methane_data %>%
  group_by(Year) %>%
  summarize(AnnualAverageMethane = mean(Mean, na.rm = TRUE))

temp_annual <- temp_data %>%
  filter(Year >= 1984) %>%
  select(Year, No_Smoothing) %>%
  rename(AnnualAverageTemp = No_Smoothing)
```

For the sea level analysis, I modify the sea level dataset to extract
the year from the ‘Year + Fraction of Year’ column and then group the
data annually. This grouping allows for averaging the sea level
measurements over each year, which is critical for observing long-term
trends and reducing temporal noise in the data.

Also in this segment of data preparation, I refine the global
temperature dataset to align it specifically for analysis with sea level
data. This transformation is crucial for ensuring that temperature data
is accurately represented and can be effectively compared with sea level
changes over the same years.

``` r
sealevel_annual <- sealevel_data %>%
  mutate(Year = floor(`Year + Fraction of Year`)) %>%
  group_by(Year) %>%
  summarize(Smoothed_GMSL = mean(`Smoothed GMSL (No GIA) (mm)`))

tempsea_annual <- temp_data %>%
  select(Year, No_Smoothing) %>%
  rename(AnnualAverageTemp = No_Smoothing)
```

Now that I have cleaned and organized the data, I can merge the
dataframes for greenhouse gas analysis. The focus is on consolidating
annual measurements of CO₂ levels, methane concentrations, and global
temperature anomalies into a single dataset.

``` r
greenhouse_merged <- merge(co2_annual, methane_annual, by = "Year")
greenhouse_merged <- merge(greenhouse_merged, temp_annual, by = "Year")
head(greenhouse_merged)
```

    ##   Year AnnualAverageCO2 AnnualAverageMethane AnnualAverageTemp
    ## 1 1984         344.8675              1644.85              0.16
    ## 2 1985         346.3525              1657.29              0.12
    ## 3 1986         347.6083              1670.09              0.18
    ## 4 1987         349.3117              1682.70              0.32
    ## 5 1988         351.6900              1693.16              0.39
    ## 6 1989         353.2050              1704.53              0.27

Same thing for the sea level analysis.

``` r
sealevel_merged <- merge(tempsea_annual, sealevel_annual, by = "Year")
structure(sealevel_merged)
```

    ##   Year AnnualAverageTemp Smoothed_GMSL
    ## 1 1993              0.23     -31.93667
    ## 2 1994              0.31     -29.43333
    ## 3 1995              0.45     -24.91000
    ## 4 1996              0.33     -23.50333
    ## 5 1997              0.46     -19.54250
    ## 6 1998              0.61     -21.73500
    ## 7 1999              0.38     -19.65250
    ## 8 2000              0.39     -14.64333
    ## 9 2001              0.53     -12.91000

### Time Series Analysis

In this section of analysis, I employ time series plots to visually
represent the trends in CO₂ levels, methane concentrations, global
temperature anomalies, and sea level changes over time. These plots are
critical for understanding the temporal dynamics of each environmental
parameter and for assessing their potential impacts on climate change.

The first plot focuses on the trend in CO₂ levels over the years.

``` r
ggplot(data = greenhouse_merged, aes(x = Year, y = AnnualAverageCO2)) +
  geom_line(color = "blue") +
  ggtitle("CO2 Levels Over Time") +
  xlab("Year") +
  ylab("CO2 Levels (ppm)") +
  theme_minimal() + 
  theme(text = element_text(size = 12))
```

![](README_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

Methane, another critical greenhouse gas, is tracked in the second plot.

``` r
ggplot(data = greenhouse_merged, aes(x = Year, y = AnnualAverageMethane)) +
  geom_line(color = "green") +
  ggtitle("Methane Concentrations Over Time") +
  xlab("Year") +
  ylab("Methane (ppb)") +
  theme_minimal() + 
  theme(text = element_text(size = 12))
```

![](README_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

Next, I examine the trend in global temperature anomalies. This plot
will help us identify periods of significant temperature increases or
decreases and correlate these trends with the data on greenhouse gases.

``` r
ggplot(data = greenhouse_merged, aes(x = Year, y = AnnualAverageTemp)) +
  geom_line(color = "red") +
  ggtitle("Global Temperature Anomalies Over Time") +
  xlab("Year") +
  ylab("Temperature Anomaly (C°)") +
  theme_minimal() +
  theme(text = element_text(size = 12))
```

![](README_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Lastly, I plot the changes in global mean sea level, which have been
adjusted for non-climatic factors such as gravitational and elastic
effects (No GIA).

``` r
  ggplot(sealevel_merged, aes(x = Year, y = Smoothed_GMSL)) +
  geom_line(color = "blue") +
  ggtitle("Global Mean Sea Level Changes Over Time (Smoothed, No GIA)") +
  xlab("Year") +
  ylab("Sea Level Change (mm)") +
  theme_minimal() + 
  theme(text = element_text(size = 12))
```

![](README_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

### Exploratory Data Analysis

Now I’ll utilize scatter plots to visually explore the relationships
between key environmental metrics: CO₂ levels, methane concentrations,
and sea levels, each compared against global temperature anomalies.
These plots are instrumental in identifying potential correlations and
establishing a basis for further statistical analysis.

The first scatter plot examines the relationship between CO₂ levels
(ppm) and global temperature anomalies (C°). By visualizing these
variables, I aim to identify how fluctuations in CO₂ concentrations may
correlate with changes in global temperatures, which is central to
understanding the impact of this predominant greenhouse gas

``` r
ggplot(data = greenhouse_merged, aes(x = AnnualAverageCO2, y = AnnualAverageTemp)) +
  geom_point(color = "brown", alpha = 0.7) +
  geom_smooth(method = "lm", color = "blue") +
  ggtitle("CO2 Levels vs. Global Temp Anomalies") +
  xlab("CO2 Levels (ppm)") +
  ylab("Global Temp Anomaly (C°)") +
  theme_minimal() +
  theme(text = element_text(size = 12))
```

![](README_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

Next, I explore the association between methane concentrations (ppb) and
global temperature anomalies. Methane, being a potent greenhouse gas,
might exhibit a moderate to strong correlation with temperature
anomalies, offering insights into its relative climatic impact compared
to CO₂.

``` r
ggplot(data = greenhouse_merged, aes(x = AnnualAverageMethane, y = AnnualAverageTemp)) +
  geom_point(color = "purple", alpha = 0.7) +
  geom_smooth(method = "lm", color = "green") +
  ggtitle("Methane Concentrations vs. Global Temp Anomalies") +
  xlab("Methane (ppb)") +
  ylab("Global Temp Anomaly (C°)") +
  theme_minimal()
```

![](README_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

Lastly, this plot correlates changes in sea levels with global
temperature anomalies. The analysis seeks to demonstrate if and how
temperature changes influence sea level fluctuations, an essential
factor in assessing climate change impacts.

``` r
  ggplot(data = sealevel_merged, aes(x = Smoothed_GMSL, y = AnnualAverageTemp)) +
  geom_point(color = "brown", alpha = 0.7) +
  geom_smooth(method = "lm", color = "blue") +
  ggtitle("CO2 Levels vs. Global Temp Anomalies") +
  xlab("CO2 Levels (ppm)") +
  ylab("Global Temp Anomaly (C°)") +
  theme_minimal() +
  theme(text = element_text(size = 12))
```

![](README_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

### Correlation Tests

Following the organization and visualization of the data, the next
crucial step in analysis is to conduct statistical tests to evaluate the
strength and significance of the correlations identified during the
exploratory phase. These tests are essential to substantiate the
relationships hypothesized between greenhouse gas levels and global
temperature anomalies. The results will help confirm or refute the
initial assumptions laid out in the research objectives.

To rigorously assess the relationships posited between CO₂ and methane
concentrations and global temperature anomalies, I employ Pearson’s
product-moment correlation tests. These tests will quantify the linear
relationships between these variables, providing a statistical basis to
either reject or fail to reject the null hypotheses formulated earlier.

I begin by examining the linear correlation between annual average CO₂
levels and global temperature anomalies. This test aims to quantify the
strength of association, offering insights into how CO₂ emissions may be
driving changes in global temperatures.

``` r
cor.test(greenhouse_merged$AnnualAverageCO2, greenhouse_merged$AnnualAverageTemp)
```

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  greenhouse_merged$AnnualAverageCO2 and greenhouse_merged$AnnualAverageTemp
    ## t = 16.415, df = 37, p-value < 2.2e-16
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  0.8835916 0.9670880
    ## sample estimates:
    ##       cor 
    ## 0.9376913

Similarly, I conduct a correlation test for methane concentrations
against global temperature anomalies. Given methane’s potency as a
greenhouse gas, this analysis is critical to understanding its
comparative impact on climate change alongside CO₂.

``` r
cor.test(greenhouse_merged$AnnualAverageMethane, greenhouse_merged$AnnualAverageTemp)
```

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  greenhouse_merged$AnnualAverageMethane and greenhouse_merged$AnnualAverageTemp
    ## t = 13.447, df = 37, p-value = 8.131e-16
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  0.8358962 0.9527449
    ## sample estimates:
    ##      cor 
    ## 0.911116

Finally, a correlation test for global mean sea level against global
average temperature anomalies.

``` r
cor.test(sealevel_merged$Smoothed_GMSL, sealevel_merged$AnnualAverageTemp)
```

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  sealevel_merged$Smoothed_GMSL and sealevel_merged$AnnualAverageTemp
    ## t = 2.0625, df = 7, p-value = 0.07808
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.08335174  0.90813107
    ## sample estimates:
    ##       cor 
    ## 0.6148028

### Regression Analysis

Building on the foundational correlation analysis, the next analytical
step involves employing regression techniques to delve deeper into the
causal relationships suggested by the observed correlations. This
regression analysis aims to provide quantitative insights into how
changes in greenhouse gas concentrations and sea levels are associated
with alterations in global temperature anomalies.

To quantitatively assess the impact of CO₂ and methane on global
temperature anomalies, I’ll construct a multiple linear regression
model. This model will enable us to isolate the effect of each gas on
temperature changes, adjusting for the presence of the other gas. This
analysis is critical, as it goes beyond correlation by estimating the
actual impact (in terms of temperature change) that can be attributed to
each unit increase in greenhouse gas concentrations.

``` r
greenhouse_regression = lm(AnnualAverageTemp ~ AnnualAverageCO2 + AnnualAverageMethane , data = greenhouse_merged)
summary(greenhouse_regression)
```

    ## 
    ## Call:
    ## lm(formula = AnnualAverageTemp ~ AnnualAverageCO2 + AnnualAverageMethane, 
    ##     data = greenhouse_merged)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -0.122258 -0.081379 -0.004919  0.062878  0.177247 
    ## 
    ## Coefficients:
    ##                        Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)          -3.5329130  0.7177138  -4.922  1.9e-05 ***
    ## AnnualAverageCO2      0.0102061  0.0026638   3.831 0.000492 ***
    ## AnnualAverageMethane  0.0001347  0.0009289   0.145 0.885549    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.08929 on 36 degrees of freedom
    ## Multiple R-squared:  0.8793, Adjusted R-squared:  0.8726 
    ## F-statistic: 131.2 on 2 and 36 DF,  p-value: < 2.2e-16

Similarly, to explore the relationship between rising sea levels and
global temperature anomalies, I’ll perform a linear regression analysis.
This model seeks to quantify how changes in sea levels are potentially
driven by global temperature changes, providing a statistical basis to
understand the dynamics of sea level rise in the context of global
warming.

``` r
sealevel_regression = lm(AnnualAverageTemp ~ Smoothed_GMSL, data = sealevel_merged)
summary(sealevel_regression)
```

    ## 
    ## Call:
    ## lm(formula = AnnualAverageTemp ~ Smoothed_GMSL, data = sealevel_merged)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.10458 -0.06313 -0.01522  0.02152  0.19663 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)   0.662249   0.126613   5.230  0.00121 **
    ## Smoothed_GMSL 0.011450   0.005552   2.062  0.07808 . 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.09824 on 7 degrees of freedom
    ## Multiple R-squared:  0.378,  Adjusted R-squared:  0.2891 
    ## F-statistic: 4.254 on 1 and 7 DF,  p-value: 0.07808

# Results

Let’s recap the findings from the correlation tests and regression
analysis:

## CO₂ and Global Average Temperature

The correlation coefficient of 0.9376913 indicates a very strong
positive correlation between annual average CO₂ levels and global
temperature anomalies. This means as CO₂ levels increase, global
temperatures also tend to increase.

The t-value of 16.415 with 37 degrees of freedom shows the statistical
strength of the correlation. This high t-value indicates that the
correlation coefficient is significantly different from zero.

The p-value \< 2.2e-16 (extremely small) strongly suggests rejecting the
null hypothesis that there is no linear relationship between CO₂ levels
and global temperatures. This low p-value indicates that the probability
of observing such a strong correlation by chance (if there were no
actual correlation) is extremely low.

The interval from 0.8835916 to 0.9670880 suggests with 95% confidence
that the true correlation coefficient between CO₂ levels and global
temperatures lies within this range. The range does not include zero,
reinforcing the significance of the correlation.

## CH₄ and Global Average Temperature

The correlation coefficient of 0.911116 also indicates a very strong
positive correlation between annual average methane levels and global
temperature anomalies. As methane levels increase, global temperatures
also tend to increase similarly.

The t-value of 13.447 with 37 degrees of freedom again highlights the
strength of the correlation, indicating it is significantly different
from zero.

The p-value of 8.131e-16 (also extremely small) leads to a rejection of
the null hypothesis that there is no linear relationship between methane
levels and global temperatures. This indicates a very low likelihood
that this strong correlation is due to random chance.

The confidence interval from 0.8358962 to 0.9527449 similarly indicates
that with 95% confidence, the true correlation coefficient lies within
this range. Again, the interval does not include zero, supporting the
significance of the correlation.

## Sea Levels and Global Average Temperature

The correlation coefficient of 0.6148028 suggests a moderate positive
relationship between sea level changes and global average temperatures.
This indicates that as global temperatures rise, sea levels also tend to
increase, but the relationship is not perfectly linear or extremely
strong.

The t-value of 2.0625 with 7 degrees of freedom suggests that the
observed correlation is just over two times the standard error away from
zero; the 7 degrees of freedom likely reflects the small sample size (n
= 9, since df = n - 2 for correlation tests). A smaller sample size can
limit the reliability of the correlation estimate.

The p-value of 0.07808, which is greater than the conventional alpha
level of 0.05, suggests that the correlation, while suggestive of a
positive relationship, is not statistically significant at the 5% level.
This means we do not have sufficient evidence to reject the null
hypothesis that there is no correlation between sea level changes and
global temperatures.

Finally, the confidence interval from -0.08335174 to 0.90813107 is quite
wide and includes negative values, which further indicates uncertainty
about the strength of the relationship. The fact that it crosses zero
suggests that, statistically, there is a considerable chance that there
could be no effective correlation between the variables. However, the
upper limit being close to 1 indicates that a strong positive
correlation is also possible, reflecting the uncertainty due to possibly
having a small sample size.

# Conclusion

This research set out to explore the relationships between greenhouse
gases—specifically carbon dioxide (CO₂) and methane (CH₄)—and global
average temperature, as well as the implications of rising global
temperatures on sea levels. The study greatly boosted my understanding
of the dynamics of greenhouse gases and their effect on global
temperature and sea levels. It highlights the critical need for robust
environmental policies and further research to explore complex
interactions within the Earth’s climate system. The findings underscore
the urgency of addressing greenhouse gas emissions, particularly CO₂, as
part of global strategies against climate change. Future studies are
recommended to continue this line of inquiry, employing larger and more
detailed datasets to unravel the intricate patterns of climate change
further.

## Key Findings

### Strong Correlation between Greenhouse Gases and Temperature

The analysis confirmed a very strong positive correlation between both
CO₂ and methane concentrations and global average temperatures. The
Pearson’s correlation coefficients of 0.9377 for CO₂ and 0.9111 for
methane are indicative of robust linear relationships, significantly
supporting the alternative hypothesis that these gases are linearly
related to increases in global temperatures.

### Moderate Correlation between Sea Level and Temperature

The relationship between sea levels and global temperatures, while
moderate (correlation coefficient of 0.6148), was not statistically
significant at the conventional 0.05 level (p-value = 0.07808). This
suggests that while there is some indication of a relationship, the
evidence is not strong enough to conclusively assert a linear
correlation at this level of significance.

### Regression Analysis Insights

The regression models further quantified the impacts of these variables.
The model including both CO₂ and methane explained a substantial portion
of the variance in global temperatures, with an R-squared value of
0.8793, confirming the significant predictive power of these gases.
However, the impact of methane, when adjusted for CO₂, was not
significant, highlighting CO₂’s predominant role. The regression model
for sea levels and temperatures showed a weaker relationship, with an
R-squared of 0.378, indicating that sea level changes explain a smaller
portion of the variance in global temperatures within the observed data
set.

## Implications for Climate Science and Policy

### Model Validation and Improvement

These results validate the significant role of CO₂ and, to a lesser
extent, methane in influencing global temperature changes. This
reinforces the need for climate models to incorporate these gases
accurately to predict future climatic conditions effectively.

### Policy Development

With the proven strong influence of CO₂ on temperature, policies aimed
at reducing carbon emissions are validated as essential to controlling
global warming. Although the direct relationship between temperature and
sea levels was less clear, the general trend suggests rising
temperatures could influence sea levels, justifying continued emphasis
on policies to prepare for and mitigate sea level rise.

### Further Research

The moderate and non-significant correlation between sea levels and
temperatures suggests that additional factors or more complex models
might be necessary to fully understand and predict sea level dynamics.
Further research with larger datasets, or incorporating additional
variables such as polar ice melt rates and oceanic thermal expansion,
could provide more insights.

# References

## Articles

Armstrong, Anne K., et al. CLIMATE CHANGE SCIENCE: The Facts.
Communicating Climate Change: A Guide for Educators, Cornell University
Press, 2018, pp. 7-20. JSTOR,
<http://www.jstor.org/stable/10.7591/j.ctv941wjn.5>.

Hansen, James, et al. “Global Temperature Change.” Proceedings of the
National Academy of Sciences, vol. 103, no. 39, 26 Sept. 2006,
pp. 14288–14293. PNAS, www.pnas.org/cgi/doi/10.1073/pnas.0606291103.

Mann, Michael E., Raymond S. Bradley, and Malcolm K. Hughes.
“Global-Scale Temperature Patterns and Climate Forcing Over the Past Six
Centuries.” Nature, vol. 392, 23 Apr. 1998, pp. 779-787,
<https://www.nature.com/articles/33859>

Mitchell, J. F. B. “The ‘Greenhouse’ Effect and Climate Change.”
Rev. Geophys., vol. 27, no. 1, 1989, pp. 115-139,
<doi:10.1029/RG027i001p00115>.

## Data

National Aeronautics and Space Administration. “Climate Change: Vital
Signs of the Planet.” NASA, n.d.,
<https://science.nasa.gov/climate-change/>.

## Pictures

“Electric Towers During Golden Hour.” Pexels, uploaded by Pixabay,
<https://www.pexels.com/photo/electric-towers-during-golden-hour-221012/>.
