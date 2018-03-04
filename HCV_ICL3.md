
Validation of Hepatitis C viral count using R-computing
========================================================

This is a test

```
## Error: cannot change working directory
```

```
## Loading required package: seriation
## Loading required package: nlme
```

```
## Warning: cannot open file
## '/Documents/workspace/R-project/HCV/HCV_ICL3.csv': No such file or
## directory
```

```
## Error: cannot open the connection
```

```
## Error: object 'HCV_ICL3' not found
```
In a summary. The lower limit of detection (LLD) at home-lab is 12 IU/ml and the LLD at the reference-lab os 20 IU/ml. So, if the result is <20IU/ml, the detected value could be anywhere between 1 and 20. Therefore, the lower limit of detection has been set for home-lab at '6 IU/ml' and '10 IU/ml' for the reference lab. 


```r
summary(HCV_ICL3)
```

```
## Error: object 'HCV_ICL3' not found
```

```r
head(HCV_ICL3)
```

```
## Error: object 'HCV_ICL3' not found
```
To make it more easy, the set of values from Reference-lab = 'x'. The set of values from Home-lab = 'y' 

```
## Error: object 'HCV_ICL3' not found
```

```
## Error: object 'HCV_ICL3' not found
```
Calculate the means and difference between the two sets (x and y)

```r
# derive difference
mean(x)
```

```
## Error: object 'x' not found
```

```r
mean(y)
```

```
## Error: object 'y' not found
```

```r
# mean Ref_lab - mean Home_lab
mean(x)-mean(y)
```

```
## Error: object 'x' not found
```
Because n=8 is small, the distribution of the differences should be approximately normal. Check using a boxplot and QQ plot. 
There is some skew.

```r
HepB_Web$diff <- x-y
```

```
## Error: object 'x' not found
```

```r
HepB_Web$diff
```

```
## Error: object 'HepB_Web' not found
```

```r
boxplot(HCV_ICL3$diff)
```

```
## Error: object 'HCV_ICL3' not found
```

```r
qqnorm(HCV_ICL3$diff)
```

```
## Error: object 'HCV_ICL3' not found
```

```r
qqline(HCV_ICL3$diff)
```

```
## Error: object 'HCV_ICL3' not found
```
Shaphiro test of normality. 

```r
shapiro.test(HCV_ICL3$diff)
```

```
## Error: object 'HCV_ICL3' not found
```
The normality test gives p < 0.001, which is small, so we 
reject the null hypothesis that the values are distributed normally. 

This means that we cannot use the student t-test. Instead, use the Mann-Whitney-Wilcoxon Test, we can decide whether the population distributions are identical without assuming them to follow the normal distribution.

```r
wilcox.test(x, y, paired = TRUE)
```

```
## Error: object 'x' not found
```
p > 0.05 and therefore the H0 is NOT rejected. 
The two populations are identical.

Just to see what happens in the Student T-test.
A paired t-test: one sample, two tests
H0 = no difference; H1 = mean of 2 tests are different
mu= a number indicating the true value of the mean 
(or difference in means if you are performing a two sample test).

```r
t.test(x, y, mu=0, paired=T, alternative="greater")
```

```
## Error: object 'x' not found
```
p = 0.8425. Because p is larger than alpha, we do NOT reject H0.
In other words, it is unlikely the observed agreements happened by chance. 
However, because the populations do not have a normal distribution, we can not use the outcome if this test.

For correlation, three methods are used: pearson, kendall and spearman at a confidence level of 95%.

```r
# correlation of the two methods
cor.test(x, y, 
         alternative = c("two.sided", "less", "greater"),
         method = c("pearson", "kendall", "spearman"),
         exact = NULL, conf.level = 0.95)
```

```
## Error: object 'x' not found
```
The correlation with the spearman test is 0.9868827. Almost perfect correlation. 

```
## Error: object 'HCV_ICL3' not found
```
Plotting the two methods using logarithmic scales.

```r
g <- ggplot(HCV_ICL3, aes(log(Home_lab), log(Ref_lab)))
```

```
## Error: object 'HCV_ICL3' not found
```

```r
# add layers
g + 
  geom_smooth(method="lm", se=TRUE, col="steelblue", size = 1) +
  geom_point(size = 3, aes(colour = x)) +
  scale_colour_gradient("IU/ml", high = "red", low = "blue", space = "Lab") +
  labs(y = "Reference lab (log IU/ml)") +
  labs(x = "Home lab (log IU/ml)") +
  theme_bw(base_family = "Helvetica", base_size = 14) +
  scale_x_continuous(breaks=c(0,4,8,12))
```

```
## Error: object 'g' not found
```
Summary data on the correlation line.

```r
regmod <- lm(y~x, data=HepB_Web)
```

```
## Error: object 'HepB_Web' not found
```

```r
summary(regmod)
```

```
## Error: object 'regmod' not found
```
The Bland-Altman Analysis. To check if there is a bias.

```
## Error: object 'HCV_ICL3' not found
```

```r
BlandAltman(x, y,
            x.name = "Reference lab IU/ml",
            y.name = "Home lab IU/ml",
            maintit = "Bland-Altman plot for HBV count",
            cex = 1,
            pch = 16,
            col.points = "black",
            col.lines = "blue",
            limx = NULL,
            limy = NULL,
            ymax = NULL,
            eqax = FALSE,
            xlab = NULL,
            ylab = NULL,
            print = TRUE,
            reg.line = FALSE,
            digits = 2,
            mult = FALSE)
```

```
## NOTE:
##  'AB.plot' and 'BlandAltman' are deprecated,
##  and likely to disappear in a not too distant future,
##  use 'BA.plot' instead.
```

```
## Error: object 'x' not found
```
When the dots are around 0, the two test could be interchanged for a patient. So, the two test can be interchanged. There are, however, some outliners: large difference of viral count between the two labs.




