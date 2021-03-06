# Analyzing-Student-Ratio-Using-Linear-Model


```{r, echo=FALSE}
library(tidyverse)
library(ggplot2) 
library(gapminder)
student_teacher_ratio<-read.csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-07/student_teacher_ratio.csv")
avg_student_ratio <- student_teacher_ratio %>%
  filter(!is.na(student_ratio)) %>%
  group_by(year,indicator) %>%
  summarise(mean=mean(student_ratio))
 ggplot(avg_student_ratio,aes(x=year,y=mean,color=fct_reorder2(indicator,year,mean))) + geom_point() + geom_line() + labs(title = "Average Student Ratio by Indicator",x="Year",y="Average Student Ratio")+
scale_x_continuous(breaks = c(2012,2013,2014,2015,2016,2017,2018))

 
```

## Average Student Ratio by Indicator

This dataset including `r nrow(student_teacher_ratio)` rows and `r ncol(student_teacher_ratio)` columns. The average ratio is `r mean(student_teacher_ratio$student_ratio,na.rm=T)`.

The graph shows that the average student ratio in primary education is always the highest. Since 2017, the average proportion of students in preschool education has increased significantly overall. The average share of students in non-higher education declined in 2018, but rose significantly between 2013 and 2014. The average share of students in other education did not change much from 2012 to 2018.

 
```{r,echo=FALSE}
 pe <- student_teacher_ratio %>%
  filter(!is.na(student_ratio)) %>%
  filter(indicator=="Primary Education",country=="United States of America"| country=="India"| country=="China"|country=="United Kingdom of Great Britain and Northern Ireland")%>%
  group_by(year,country) %>%
  summarise(student_ratio=mean(student_ratio))
  
 ggplot(pe,aes(year,y=student_ratio,color= country)) + geom_density2d() + 
   labs(title = "British/USA/China/India - Primary Education",x="Year",y="Mean of Student Ratio" )
 
```

##  Primary Education for British/USA/China/India

From this table, we can roughly observe the average of the data over the years, and the dispersion of the data.

Several representative countries, such as *India, China, Britain and the United States.* Comparing the average student ratio of these four countries, we find that the student ratio of India is much higher than that of other countries, and the data are relatively scattered. The United States has the lowest average percentage of students, and the data are relatively concentrated. China is slightly higher than the us, and the data is relatively concentrated. The UK data are more fragmented.

```{r,echo=FALSE,warning=FALSE}
student_teacher_ratio$country=factor(student_teacher_ratio$country)
gapminder$country=factor(gapminder$country)
joined_gapminder <- left_join(student_teacher_ratio,gapminder,by="country")
 
pe <- joined_gapminder  %>%
  filter(indicator == "Primary Education", !is.na(student_ratio),!is.na(continent) ) %>%
  select(country, student_ratio,year.x,continent)  %>%
  mutate(bucket = ntile(student_ratio, 5))%>%
  mutate(per_class= max(student_ratio) %>%
  paste0("up to ", .))
per_class <- pe %>%
  group_by(bucket) %>%
  summarise(lt = max(student_ratio)) %>%
  pull() %>%
  paste0("up to ", .)
ggplot(pe,aes(year.x,student_ratio,group=factor(continent),
              color=factor(continent)))+geom_point() +facet_wrap(~continent)+
     labs(title = "Student Ratio for Primary Education by Continent",x="Year",y="Student Ratio")+scale_x_continuous(breaks=c(2012,2013,2014,2015,2016,2017,2018))
```

## Student Ratio for Primary Education from 2012-2018 by Continent

In contrast, Americans and europeans have a lower percentage of students. This suggests that developed countries tend to have smaller classes, and that developing countries have a higher proportion of students because they have more students. But we didn't find out whether small classes lead to better grades.

```{r,echo=FALSE,warning=FALSE}
gdp<- joined_gapminder %>% 
  filter(!is.na(pop),!is.na(student_ratio)) %>%
  filter(indicator=="Primary Education")%>%
  filter(year.y >= 2000) %>%
 select(indicator,continent,gdpPercap,student_ratio) %>%
 group_by(indicator,continent)  
ggplot(gdp,aes(gdpPercap,student_ratio,color=continent)) + geom_boxplot()+
  labs(title = "GDP per capital vs. Student Ratio for Primary Education Since 2000", x="GDP per capital",y="Student Ratio") + scale_x_continuous(breaks = c(10000,20000,30000,40000,50000))
 
 
```

## GDP per capital vs. Student Ratio for Primary Education Since 2000

Since 2000, The boxplot shows that the higher the GDP per capital, the lower the student ratio. Europe has the largest GDP per capital and the lowest proportion of students. Economically developed countries adopt small class system, while economically backward countries have a significantly higher proportion of students. This shows that GDP per capital has a certain correlation with the proportion of students. We need to do further analysis.

```{r,echo=FALSE}
joined_gapminder%>%
   filter(!is.na(student_ratio))%>%
   select(student_ratio)%>%
  ggplot(aes(student_ratio))+geom_histogram() +geom_density(kernel = "gaussian")
 
library(moments) # For skewness() and kurtosis()
```
student_ratio is not normally distrubuted(ie.right skewed), skewness is `r skewness(joined_gapminder$student_ratio,na.rm=TRUE)` and kurtosis is `r kurtosis(joined_gapminder$student_ratio,na.rm=TRUE)`,therefore, the median `r median(joined_gapminder$student_ratio,na.rm=TRUE)` is a better represent of the central trendency. 
```{r,echo=FALSE}
 joined_gapminder%>%
  filter(!is.na(student_ratio))%>%
  filter(!is.na(continent)) %>%
  select(student_ratio,gdpPercap,pop,year.x,continent,lifeExp)%>%
  group_by(year.x,continent) %>%
  summarise(median=median(student_ratio,na.rm=TRUE)) %>%
  pivot_wider(names_from = continent,values_from = median) 
```

This table shows the median value of students per year by continent

```{r,echo=FALSE}
joined_gapminder %>%
  lm(student_ratio ~ indicator + continent + year.x + pop + gdpPercap + lifeExp,data = .) %>% summary()
joined_gapminder %>%
  lm(student_ratio ~ indicator + continent + year.x + gdpPercap + lifeExp,data = .) %>% summary()
joined_gapminder %>%
  lm(student_ratio ~ year.x + gdpPercap + lifeExp,data = .) %>% summary()  
```

In the first model, we observed 6 variables **(indicator, continent, year, pop, gdpPercap, lifeExp)** and the results showed that pop was the least significant of the observed values. After the pop variable was reduced, the r square in the model did not decrease, and we could see that there was no correlation between the population and the proportion of students.

The optimized model contains three variables **(year, gdpPercap, lifeExp)**, in which year and student ratio were positively correlated, while the other two variables were negatively correlated.

```{r, echo=FALSE}
joined_gapminder %>%
  filter(year.y >=2000) %>% 
  filter(indicator=="Primary Education") %>%
  ggplot(aes(lifeExp,student_ratio,color=year.y)) + geom_point() + geom_smooth(method = "lm", se = FALSE) +
  labs(title = "LifeExp vs. Student Ratio for Primary Education")
joined_gapminder %>%
 filter(year.y >=2000) %>% 
  filter(indicator=="Primary Education") %>%
  ggplot(aes(gdpPercap,student_ratio,color=year.y)) + geom_point() + geom_smooth(method = "lm", se = FALSE) +
  labs(title = "GDP/capital vs. Student Ratio for Primary Education ")
student_teacher_ratio %>%
  filter(indicator=="Primary Education") %>%
  ggplot(aes(year,student_ratio,color=year)) + geom_point() + geom_smooth(method = "lm", se = FALSE)
```

## Summary of above charts

We observed that an increase in *gdpPercap* per unit led to a  1.415e-04 decrease in the  student ratio of primary education.

At the same time, the increase of each unit of *lifeExp* will lead to a 5.117e-01 decrease in the  student ratio of primary education.

The change in the number of years from *2012 to 2018* will have no effect on primary education's student ratio.
```{r}
library(tidyverse)
student_ratio <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-07/student_teacher_ratio.csv")
p_2012 <- student_ratio %>%
  mutate(
    ncode = as.numeric(country_code),
    student_ratio = round(student_ratio)) %>%
  filter(year == 2012, indicator == "Primary Education", !is.na(student_ratio), is.na(ncode) ) %>%
  select(country, student_ratio)  %>%
  mutate(bucket = ntile(student_ratio, 5))
per_class <- pe %>%
  group_by(bucket) %>%
  summarise(lt = max(student_ratio)) %>%
  pull() %>%
  paste0("up to ", .)
library(maps)
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
```
