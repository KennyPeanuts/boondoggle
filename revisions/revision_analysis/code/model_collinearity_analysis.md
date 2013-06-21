# Analysis of the collinearity of the predictors in the survey model used in the GTH 91 case study

These analyses are for the revisions to the boondoggle manuscript for Inland Waters and are based on the comment by Reviewer D:

> They state that thermocline depth in GTH 91 was predicted from the relationship between thermocline depth, Kd, surface area, and Julian day established in the surveys.  In the first section (Lake surveys) the authors note that Kd was significantly related to lake surface area and Julian day. Is there not an issue with collinearly if both Kd, surface area and Julian day are then used as independent variables in a regression? Indeed, there is no mention here that any of the assumptions of linear regression were assessed.

This analysis is to more fully assess the collinearity of the predictors and dertermine if the model needs to be changed.



## Analysis of the relationships between Kd, Lake Area, and Julian Day

### Import Data

~~~~

# make working directory:

# "/Volumes/NO NAME/working_files/current_research/boondoggle/inland_waters_submission/revisions/revision_analysis"

# load the boondoggle workingspace with the data
load("./data/boondoggle")

~~~~

### Code of Plots the relationships between the predictors

# plot the relationship between the predictors
plot(Kd ~ Area, data = boon.tot, type = "n")
points(Kd ~ Area, data = boon.tot, subset = Year == 2006, col = 2)
points(Kd ~ Area, data = boon.tot, subset = Year == 2007, col = 3)
points(Kd ~ Area, data = boon.tot, subset = Year == 2008, col = 4)
legend(60, 0.9, c("2006", "2007", "2008"), pch = 1, col = c(2, 3, 4))

plot(Kd ~ Julian, data = boon.tot, type = "n")
points(Kd ~ Julian, data = boon.tot, subset = Year == 2006, col = 2)
points(Kd ~ Julian, data = boon.tot, subset = Year == 2007, col = 3)
points(Kd ~ Julian, data = boon.tot, subset = Year == 2008, col = 4)
legend(200, 0.9, c("2006", "2007", "2008"), pch = 1, col = c(2, 3, 4))

plot(TD ~ Kd, data = boon.tot, type = "n")
points(TD ~ Kd, data = boon.tot, subset = Year == 2006, col = 2)
points(TD ~ Kd, data = boon.tot, subset = Year == 2007, col = 3)
points(TD ~ Kd, data = boon.tot, subset = Year == 2008, col = 4)
legend(0.8, 10, c("2006", "2007", "2008"), pch = 1, col = c(2, 3, 4))

plot(TD ~ Julian, data = boon.tot, type = "n")
points(TD ~ Julian, data = boon.tot, subset = Year == 2006, col = 2)
points(TD ~ Julian, data = boon.tot, subset = Year == 2007, col = 3)
points(TD ~ Julian, data = boon.tot, subset = Year == 2008, col = 4)
legend(195, 10, c("2006", "2007", "2008"), pch = 1, col = c(2, 3, 4))

~~~~

### Determine the distribution of the Area values

~~~~
# determine distribution of the Area
stem(boon.tot$Area)

  The decimal point is 1 digit(s) to the right of the |

  0 | 01122233444444444566666677788
  1 | 003334447777
  2 | 00
  3 | 00
  4 | 
  5 | 
  6 | 
  7 | 77

~~~~


### Correlation Analysis with and without extreme Area Values

~~~~

# 2006 with largest lake 
cor.test(boon.tot$Kd[boon.tot$Year == 2006], boon.tot$Area[boon.tot$Year == 2006])


	Pearsons product-moment correlation

data:  boon.tot$Kd[boon.tot$Year == 2006] and boon.tot$Area[boon.tot$Year == 2006] 
t = -1.5029, df = 16, p-value = 0.1524
alternative hypothesis: true correlation is not equal to 0 
95 percent confidence interval:
 -0.7031242  0.1377862 
sample estimates:
       cor 
-0.3517087 

# 2006 without largest lake 
cor.test(boon.tot$Kd[boon.tot$Year == 2006 & boon.tot$Area != max(boon.tot$Area)], boon.tot$Area[boon.tot$Year == 2006 & boon.tot$Area != max(boon.tot$Area)])

	Pearsons product-moment correlation

data:  boon.tot$Kd[boon.tot$Year == 2006 & boon.tot$Area != max(boon.tot$Area)] and boon.tot$Area[boon.tot$Year == 2006 & boon.tot$Area != max(boon.tot$Area)] 
t = -1.7336, df = 15, p-value = 0.1035
alternative hypothesis: true correlation is not equal to 0 
95 percent confidence interval:
 -0.74324429  0.08971391 
sample estimates:
       cor 
-0.4085476 

# 2007 - there were no outlying lakes
cor.test(boon.tot$Kd[boon.tot$Year == 2007], boon.tot$Area[boon.tot$Year == 2007])

	Pearsons product-moment correlation

data:  boon.tot$Kd[boon.tot$Year == 2007] and boon.tot$Area[boon.tot$Year == 2007] 
t = 0.3747, df = 10, p-value = 0.7157
alternative hypothesis: true correlation is not equal to 0 
95 percent confidence interval:
 -0.4892653  0.6478291 
sample estimates:
      cor 
0.1176796 

# 2008 with largest lake
cor.test(boon.tot$Kd[boon.tot$Year == 2008], boon.tot$Area[boon.tot$Year == 2008])

	Pearsons product-moment correlation

data:  boon.tot$Kd[boon.tot$Year == 2008] and boon.tot$Area[boon.tot$Year == 2008] 
t = -3.6231, df = 15, p-value = 0.002504
alternative hypothesis: true correlation is not equal to 0 
95 percent confidence interval:
 -0.8761231 -0.3015214 
sample estimates:
       cor 
-0.6831595 

# 2008 w/out largest lake
cor.test(boon.tot$Kd[boon.tot$Year == 2008 & boon.tot$Area != max(boon.tot$Area)], boon.tot$Area[boon.tot$Year == 2008 & boon.tot$Area != max(boon.tot$Area)])

	Pearsons product-moment correlation

data:  boon.tot$Kd[boon.tot$Year == 2008 & boon.tot$Area != max(boon.tot$Area)] and boon.tot$Area[boon.tot$Year == 2008 & boon.tot$Area != max(boon.tot$Area)] 
t = -3.6188, df = 14, p-value = 0.002792
alternative hypothesis: true correlation is not equal to 0 
95 percent confidence interval:
 -0.8856899 -0.3044117 
sample estimates:
       cor 
-0.6952108

# all years together

cor.test(boon.tot$Kd, boon.tot$Area)
cor.test(boon.tot$Kd, boon.tot$Julian)
cor.test(boon.tot$TD, boon.tot$Julian)

> cor.test(boon.tot$Kd, boon.tot$Area)

	Pearsons product-moment correlation

data:  boon.tot$Kd and boon.tot$Area 
t = -2.4425, df = 45, p-value = 0.01858
alternative hypothesis: true correlation is not equal to 0 
95 percent confidence interval:
 -0.57299814 -0.06094817 
sample estimates:
      cor 
-0.342127 

> cor.test(boon.tot$Kd, boon.tot$Julian)

	Pearsons product-moment correlation

data:  boon.tot$Kd and boon.tot$Julian 
t = -3.6074, df = 45, p-value = 0.0007721
alternative hypothesis: true correlation is not equal to 0 
95 percent confidence interval:
 -0.6697059 -0.2158110 
sample estimates:
       cor 
-0.4736254 

> cor.test(boon.tot$TD, boon.tot$Julian)

	Pearsons product-moment correlation

data:  boon.tot$TD and boon.tot$Julian 
t = 5.2506, df = 43, p-value = 4.438e-06
alternative hypothesis: true correlation is not equal to 0 
95 percent confidence interval:
 0.4059824 0.7761642 
sample estimates:
      cor 
0.6250315 

### Linear models of the relationship between the variables

~~~~

# Kd against Area
summary(lm(Kd ~ Area, data = boon.tot))

Call:
lm(formula = Kd ~ Area, data = boon.tot)

Residuals:
     Min       1Q   Median       3Q      Max 
-0.30897 -0.15278 -0.01650  0.09428  0.41110 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept)  0.563548   0.034916  16.140   <2e-16 ***
Area        -0.004416   0.001808  -2.442   0.0186 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 

Residual standard error: 0.1907 on 45 degrees of freedom
Multiple R-squared: 0.1171,	Adjusted R-squared: 0.09743 
F-statistic: 5.966 on 1 and 45 DF,  p-value: 0.01858 

# Kd against Area w/out extreme area values

summary(lm(Kd ~ Area, data = boon.tot, subset = Area != max(Area[Year == 2006]) & Area != max(Area[Year == 2008])))
# plot of the relationship
plot(Kd ~ Area, data = boon.tot, subset = Area != max(Area[Year == 2006]) & Area != max(Area[Year == 2008]))
abline(lm(Kd ~ Area, data = boon.tot, subset = Area != max(Area[Year == 2006]) & Area != max(Area[Year == 2008])))

Call:
lm(formula = Kd ~ Area, data = boon.tot, subset = Area != max(Area[Year == 
    2006]) & Area != max(Area[Year == 2008]))

Residuals:
     Min       1Q   Median       3Q      Max 
-0.31798 -0.16085  0.00486  0.10310  0.38986 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept)  0.592693   0.046201  12.829  2.7e-16 ***
Area        -0.008034   0.004125  -1.948    0.058 .  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 

Residual standard error: 0.1929 on 43 degrees of freedom
Multiple R-squared: 0.08107,	Adjusted R-squared: 0.05969 
F-statistic: 3.793 on 1 and 43 DF,  p-value: 0.05801 

# Kd against Julian
summary(lm(Kd ~ Julian, data = boon.tot))

Call:
lm(formula = Kd ~ Julian, data = boon.tot)

Residuals:
     Min       1Q   Median       3Q      Max 
-0.33483 -0.09221 -0.02470  0.09834  0.42880 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept)  2.155790   0.456412   4.723  2.3e-05 ***
Julian      -0.007829   0.002170  -3.607 0.000772 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 

Residual standard error: 0.1787 on 45 degrees of freedom
Multiple R-squared: 0.2243,	Adjusted R-squared: 0.2071 
F-statistic: 13.01 on 1 and 45 DF,  p-value: 0.0007721 

# with lake as a covatiate
summary(lm(Kd ~ Julian, data = boon.tot, subset = Year != 2007))

~~~~

## Conclusions

There is some collinearity between Kd and Area but it is rather small.  The correlation coeff = -0.34 for all years together and a liner regression between Kd and Area gives an R^2 of only 0.1.  Further some of the relationship seems to be driven by 2 very large lakes, removing these weakens the relationship between Kd and Area.

There is also some collinearity between Kd and Julian day which is greater than for Area but still rather modest.  The correlation coeff for all years is -0.47 for all years together and a linear regression between Kd and Julian gives an R^2 of 0.2

