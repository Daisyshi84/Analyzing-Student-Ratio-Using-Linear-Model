# Analyzing-Student-Ratio-Using-Linear-Model

Analyzing Student Ratio Using Linear Model
Daisy Shi
2/26/2020

## Warning: replacing previous import 'vctrs::data_frame' by
## 'tibble::data_frame' when loading 'dplyr'

## ── Attaching packages ───────────────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

## ✓ ggplot2 3.3.3     ✓ purrr   0.3.3
## ✓ tibble  3.1.0     ✓ dplyr   1.0.0
## ✓ tidyr   1.0.0     ✓ stringr 1.4.0
## ✓ readr   1.3.1     ✓ forcats 0.4.0

## Warning: package 'ggplot2' was built under R version 3.6.2

## Warning: package 'tibble' was built under R version 3.6.2

## Warning: package 'dplyr' was built under R version 3.6.2

## ── Conflicts ──────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()

## `summarise()` regrouping output by 'year' (override with `.groups` argument)

Average Student Ratio by Indicator

This dataset including 5189 rows and 8 columns. The average ratio is 18.2930214.

The graph shows that the average student ratio in primary education is always the highest. Since 2017, the average proportion of students in preschool education has increased significantly overall. The average share of students in non-higher education declined in 2018, but rose significantly between 2013 and 2014. The average share of students in other education did not change much from 2012 to 2018.

## `summarise()` regrouping output by 'year' (override with `.groups` argument)

## Warning: stat_contour(): Zero contours were generated

## Warning in min(x): no non-missing arguments to min; returning Inf

## Warning in max(x): no non-missing arguments to max; returning -Inf

## Warning: stat_contour(): Zero contours were generated

## Warning in min(x): no non-missing arguments to min; returning Inf

## Warning in max(x): no non-missing arguments to max; returning -Inf

Primary Education for British/USA/China/India

From this table, we can roughly observe the average of the data over the years, and the dispersion of the data.

Several representative countries, such as India, China, Britain and the United States. Comparing the average student ratio of these four countries, we find that the student ratio of India is much higher than that of other countries, and the data are relatively scattered. The United States has the lowest average percentage of students, and the data are relatively concentrated. China is slightly higher than the us, and the data is relatively concentrated. The UK data are more fragmented.

## `summarise()` ungrouping output (override with `.groups` argument)

Student Ratio for Primary Education from 2012-2018 by Continent

In contrast, Americans and europeans have a lower percentage of students. This suggests that developed countries tend to have smaller classes, and that developing countries have a higher proportion of students because they have more students. But we didn’t find out whether small classes lead to better grades.

GDP per capital vs. Student Ratio for Primary Education Since 2000

Since 2000, The boxplot shows that the higher the GDP per capital, the lower the student ratio. Europe has the largest GDP per capital and the lowest proportion of students. Economically developed countries adopt small class system, while economically backward countries have a significantly higher proportion of students. This shows that GDP per capital has a certain correlation with the proportion of students. We need to do further analysis.

## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

student_ratio is not normally distrubuted(ie.right skewed), skewness is 2.2075526 and kurtosis is 15.351889,therefore, the median 16.90871 is a better represent of the central trendency.

## `summarise()` regrouping output by 'year.x' (override with `.groups` argument)

year.x
<int>
	
Africa
<dbl>
	
Americas
<dbl>
	
Asia
<dbl>
	
Europe
<dbl>
	
Oceania
<dbl>
2012	29.60260	19.14433	19.04730	11.82716	14.58702
2013	26.90824	17.60952	16.85343	12.02376	17.15687
2014	26.29716	17.23632	16.57657	11.90957	14.36696
2015	25.77835	18.15347	19.88822	11.69956	14.49628
2016	24.46468	17.44522	17.49050	11.53925	14.62250
2017	23.33086	18.65684	17.85986	11.60265	14.91552
2018	24.39397	17.38724	21.52832	NA	NA
7 rows

This table shows the median value of students per year by continent

## 
## Call:
## lm(formula = student_ratio ~ indicator + continent + year.x + 
##     pop + gdpPercap + lifeExp, data = .)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -30.319  -5.073  -0.885   3.767 128.234 
## 
## Coefficients:
##                                                  Estimate Std. Error
## (Intercept)                                     8.590e+02  6.636e+01
## indicatorPost-Secondary Non-Tertiary Education  1.955e+00  2.898e-01
## indicatorPre-Primary Education                  5.551e-01  1.958e-01
## indicatorPrimary Education                      5.826e+00  1.837e-01
## indicatorSecondary Education                   -1.029e+00  1.938e-01
## indicatorTertiary Education                     4.600e-01  2.010e-01
## indicatorUpper Secondary Education             -2.298e+00  2.023e-01
## continentAmericas                              -5.538e+00  1.946e-01
## continentAsia                                  -4.636e+00  1.780e-01
## continentEurope                                -8.207e+00  2.037e-01
## continentOceania                               -4.644e+00  4.964e-01
## year.x                                         -4.079e-01  3.294e-02
## pop                                             8.533e-10  4.250e-10
## gdpPercap                                      -1.397e-04  7.249e-06
## lifeExp                                        -2.099e-01  7.013e-03
##                                                t value Pr(>|t|)    
## (Intercept)                                     12.945  < 2e-16 ***
## indicatorPost-Secondary Non-Tertiary Education   6.746 1.55e-11 ***
## indicatorPre-Primary Education                   2.836  0.00458 ** 
## indicatorPrimary Education                      31.709  < 2e-16 ***
## indicatorSecondary Education                    -5.311 1.10e-07 ***
## indicatorTertiary Education                      2.289  0.02211 *  
## indicatorUpper Secondary Education             -11.360  < 2e-16 ***
## continentAmericas                              -28.456  < 2e-16 ***
## continentAsia                                  -26.042  < 2e-16 ***
## continentEurope                                -40.290  < 2e-16 ***
## continentOceania                                -9.354  < 2e-16 ***
## year.x                                         -12.384  < 2e-16 ***
## pop                                              2.008  0.04469 *  
## gdpPercap                                      -19.272  < 2e-16 ***
## lifeExp                                        -29.928  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 8.941 on 27585 degrees of freedom
##   (4924 observations deleted due to missingness)
## Multiple R-squared:  0.3875, Adjusted R-squared:  0.3872 
## F-statistic:  1247 on 14 and 27585 DF,  p-value: < 2.2e-16

## 
## Call:
## lm(formula = student_ratio ~ indicator + continent + year.x + 
##     gdpPercap + lifeExp, data = .)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -30.324  -5.077  -0.889   3.769 128.189 
## 
## Coefficients:
##                                                  Estimate Std. Error
## (Intercept)                                     8.619e+02  6.635e+01
## indicatorPost-Secondary Non-Tertiary Education  1.968e+00  2.897e-01
## indicatorPre-Primary Education                  5.487e-01  1.957e-01
## indicatorPrimary Education                      5.817e+00  1.837e-01
## indicatorSecondary Education                   -1.030e+00  1.938e-01
## indicatorTertiary Education                     4.487e-01  2.009e-01
## indicatorUpper Secondary Education             -2.294e+00  2.023e-01
## continentAmericas                              -5.546e+00  1.946e-01
## continentAsia                                  -4.531e+00  1.701e-01
## continentEurope                                -8.215e+00  2.037e-01
## continentOceania                               -4.660e+00  4.964e-01
## year.x                                         -4.094e-01  3.293e-02
## gdpPercap                                      -1.421e-04  7.149e-06
## lifeExp                                        -2.080e-01  6.952e-03
##                                                t value Pr(>|t|)    
## (Intercept)                                     12.990  < 2e-16 ***
## indicatorPost-Secondary Non-Tertiary Education   6.792 1.13e-11 ***
## indicatorPre-Primary Education                   2.803  0.00506 ** 
## indicatorPrimary Education                      31.670  < 2e-16 ***
## indicatorSecondary Education                    -5.314 1.08e-07 ***
## indicatorTertiary Education                      2.233  0.02554 *  
## indicatorUpper Secondary Education             -11.339  < 2e-16 ***
## continentAmericas                              -28.501  < 2e-16 ***
## continentAsia                                  -26.630  < 2e-16 ***
## continentEurope                                -40.331  < 2e-16 ***
## continentOceania                                -9.386  < 2e-16 ***
## year.x                                         -12.430  < 2e-16 ***
## gdpPercap                                      -19.879  < 2e-16 ***
## lifeExp                                        -29.922  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 8.942 on 27586 degrees of freedom
##   (4924 observations deleted due to missingness)
## Multiple R-squared:  0.3874, Adjusted R-squared:  0.3871 
## F-statistic:  1342 on 13 and 27586 DF,  p-value: < 2.2e-16

## 
## Call:
## lm(formula = student_ratio ~ year.x + gdpPercap + lifeExp, data = .)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -29.346  -5.781  -1.314   4.078 133.330 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  7.779e+02  7.112e+01   10.94   <2e-16 ***
## year.x      -3.637e-01  3.530e-02  -10.30   <2e-16 ***
## gdpPercap   -1.561e-04  7.503e-06  -20.80   <2e-16 ***
## lifeExp     -3.972e-01  5.702e-03  -69.65   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 9.65 on 27596 degrees of freedom
##   (4924 observations deleted due to missingness)
## Multiple R-squared:  0.2863, Adjusted R-squared:  0.2862 
## F-statistic:  3689 on 3 and 27596 DF,  p-value: < 2.2e-16

In the first model, we observed 6 variables (indicator, continent, year, pop, gdpPercap, lifeExp) and the results showed that pop was the least significant of the observed values. After the pop variable was reduced, the r square in the model did not decrease, and we could see that there was no correlation between the population and the proportion of students.

The optimized model contains three variables (year, gdpPercap, lifeExp), in which year and student ratio were positively correlated, while the other two variables were negatively correlated.

## `geom_smooth()` using formula 'y ~ x'

## `geom_smooth()` using formula 'y ~ x'

## `geom_smooth()` using formula 'y ~ x'

## Warning: Removed 7 rows containing non-finite values (stat_smooth).

## Warning: Removed 7 rows containing missing values (geom_point).

Summary of above charts

We observed that an increase in gdpPercap per unit led to a 1.415e-04 decrease in the student ratio of primary education.

At the same time, the increase of each unit of lifeExp will lead to a 5.117e-01 decrease in the student ratio of primary education.

The change in the number of years from 2012 to 2018 will have no effect on primary education’s student ratio.

library(tidyverse)

student_ratio <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-07/student_teacher_ratio.csv")

## Parsed with column specification:
## cols(
##   edulit_ind = col_character(),
##   indicator = col_character(),
##   country_code = col_character(),
##   country = col_character(),
##   year = col_double(),
##   student_ratio = col_double(),
##   flag_codes = col_character(),
##   flags = col_character()
## )

p_2012 <- student_ratio %>%
  mutate(
    ncode = as.numeric(country_code),
    student_ratio = round(student_ratio)) %>%
  filter(year == 2012, indicator == "Primary Education", !is.na(student_ratio), is.na(ncode) ) %>%
  select(country, student_ratio)  %>%
  mutate(bucket = ntile(student_ratio, 5))

## Warning in mask$eval_all_mutate(dots[[i]]): NAs introduced by coercion

per_class <- pe %>%
  group_by(bucket) %>%
  summarise(lt = max(student_ratio)) %>%
  pull() %>%
  paste0("up to ", .)

## `summarise()` ungrouping output (override with `.groups` argument)

library(maps)

## 
## Attaching package: 'maps'

## The following object is masked from 'package:purrr':
## 
##     map

world <- map_data("world")

mapped <- p_2012 %>%
  mutate(key = case_when(
      country == "United States of America" ~ "USA",
      country == "Russian Federation" ~ "Russia",
      str_detect(country, "Bolivia") ~ "Bolivia",
      str_detect(country, "Iran") ~ "Iran",
      TRUE ~ country ) ) %>%
      right_join(world, by = c("key" = "region"))

mapped %>%
  ggplot() +
  geom_polygon(aes(long, lat, group = group, fill = as.factor(bucket)), color = "#666666", size = 0.1) +
  scale_fill_manual(
    values = c("#E69F00", "#56B4E9", "#009E73", "#F0E442", "#0072B2", "#D55E00", "#CC79A7"),
    labels = per_class,
    name = "Ratio group" ) +
  theme_minimal()  +
  theme(panel.grid = element_blank(), axis.text = element_blank()) +
  labs(title = "2012 Primary Education", subtitle = "Student to teacher ratio", x = "", y = "") +
  coord_cartesian(ylim = c(-55, 80))

