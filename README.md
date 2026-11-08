# Vehicle Fuel Efficiency: Multiple Linear Regression Analysis

## Overview

This project applies multiple linear regression to analyze and predict vehicle fuel efficiency, measured in miles per gallon (mpg). The central question is which vehicle characteristics best explain variation in fuel efficiency, and how accurately a validated regression model can predict the fuel efficiency of a new vehicle.

The analysis selects an appropriate transformation for the response variable, examines all candidate predictors, and refines a sequence of candidate models into a simple and statistically robust final model. The final model is subjected to a complete set of diagnostic checks covering linearity, constant variance, normality of residuals, independence, outliers, and influential points. The validated model is then used to predict the fuel efficiency of a new vehicle, together with a 95% confidence interval and a 95% prediction interval.

## Dataset

The dataset contains 105 automobiles (105 observations). The workbook stores the raw data on a worksheet named auto-mpg.

The response variable is mpg, which is continuous. In the sample, mpg has a mean of 23.64 and a standard deviation of 7.96, with values ranging from 8 to over 40. Because the raw mpg distribution is right skewed, the response used in all regression models is the logarithmic transformation log(mpg). The values in the workbook show that this is the base 10 logarithm (for example, an mpg of 38 corresponds to a log(mpg) of 1.5798).

The predictor variables, with the definitions given in the workbook, are as follows:

1. cyl: number of cylinders, a multi valued discrete variable.
2. displ: engine displacement, a continuous variable.
3. hp: horsepower, a continuous variable.
4. weight: vehicle weight, a continuous variable.
5. accel: acceleration, a continuous variable.
6. year: model year, a multi valued discrete variable.
7. origin: region of manufacture, coded 1 for American and 0 for Non American.

The workbook also contains three constructed interaction variables used in later models: accel*year, weight*origin, and year*origin.

## Methodology

### Exploratory Data Analysis

A histogram and a normal probability plot of mpg were produced to examine its distribution. The histogram showed substantial variability in fuel efficiency, with most vehicles between 15 and 30 mpg and a longer right tail created by a smaller number of high mpg vehicles. The probability plot showed several points deviating from the reference line at the upper and lower ends, with a few observations outside the 95% confidence bounds. The Anderson-Darling statistic was 1.347 with a p value of 0.223. The report concludes that the data are not strongly non normal, but are also not entirely normal.

### Response Transformation

Based on the right skew observed in mpg, a logarithmic transformation was applied. The histogram of log(mpg) was more symmetric and bell shaped, with no long right tail and less prominent extreme values. In the probability plot of log(mpg), almost all observations fell within the 95% confidence bands, with the exception of one point. The Anderson-Darling statistic fell to 0.438 with a p value of 0.289. The report therefore concludes that log(mpg) is a more appropriate response variable than raw mpg, because the transformation reduces skewness and better satisfies the normality assumption of linear regression.

### Variable Selection

A correlation matrix was constructed for log(mpg) and all seven predictors, and every correlation with an absolute value of 0.40 or greater was highlighted. log(mpg) was strongly negatively correlated with cyl (negative 0.800), displ (negative 0.828), hp (negative 0.814), and weight (negative 0.865). It was moderately positively correlated with accel (0.469) and year (0.598), and weakly correlated with origin (0.286).

The predictors cyl, displ, hp, and weight were very highly correlated with one another. For example, the correlation between cyl and displ was 0.957 and the correlation between displ and weight was 0.933. Because including these variables together would create a clear risk of multicollinearity, only weight was retained from this group. The first regression model therefore used weight, accel, year, and origin as predictors.

Variables were then removed one at a time on the basis of statistical significance:

1. First model: weight, accel, year, and origin. origin (p value 0.947) and accel (p value 0.168) were not significant.
2. Second model: origin was removed, leaving weight, accel, and year. R squared remained essentially unchanged at 0.872925, and adjusted R squared increased slightly to 0.869151.
3. Third model: accel was also removed, leaving weight and year as the only predictors.

Interaction models were then explored to test whether the simpler model was missing important joint effects between predictors:

1. A model with weight, year, origin, and weight*origin is included in the workbook (worksheet "fouth model"). In this model, origin (p value 0.417) and weight*origin (p value 0.421) were not significant.
2. A model with weight, year, origin, and year*origin tested whether the effect of model year differs by region of manufacture. origin was reintroduced as a main effect because it is contained within the interaction term.
3. A model with weight, accel, year, and accel*year tested whether the effect of model year depends on acceleration. This model was selected as the final model.

### Model Fitting

The analysis was carried out in Excel and Minitab concurrently. Each model was fitted with the Excel regression tool, which produced the summary statistics, ANOVA table, and coefficient table, and the same models were run in Minitab, which produced ANOVA tables, Durbin-Watson statistics, residual plots, and prediction output. Outputs from Minitab were brought into the Excel workbook alongside the Excel results, so the workbook holds the combined record of both tools. Variance inflation factors were recorded for each predictor.

A formal increase in R squared test (partial F test) was used to compare the final model with the simpler third model. The test is valid because the two models are nested: every predictor in the restricted model (weight and year) also appears in the full model (weight, accel, year, and accel*year).

### Diagnostic Checks

Each model was assessed with the following checks:

1. Linearity and constant variance: residuals versus fitted values plots were inspected for curvature, funnel shapes, or other systematic patterns.
2. Normality of residuals: normal probability plots, histograms of residuals, and the Anderson-Darling test.
3. Independence: residuals versus observation order plots and the Durbin-Watson statistic, with values between 1.5 and 2.5 treated as acceptable.
4. Multicollinearity: variance inflation factors.
5. Outliers and influential points: TRES residual values and DFITS values, with observations flagged when DFITS exceeded a threshold calculated from the number of predictors and the sample size.

## Results

### Model Progression

The fit statistics of the models described in the report are summarized below.

1. First model (weight, accel, year, origin): R squared 0.8729, adjusted R squared 0.8678, standard error 0.0544, F statistic 171.74, Significance F 7.11E-44, Durbin-Watson 1.73449. All VIF values were close to 1: weight 1.39, accel 1.26, year 1.14, and origin 1.12. Twelve points exceeded the DFITS threshold of 0.44.
2. Second model (weight, accel, year): R squared 0.8729, adjusted R squared 0.8692, standard error 0.0542, F statistic 231.27, Significance F 4.30E-45.
3. Third model (weight, year): R squared 0.8705, adjusted R squared 0.8679, standard error 0.0544, F statistic 342.71, Significance F 5.40E-46, Durbin-Watson 1.75535. Both predictors had p values below 0.0001 and VIF values of 1.1. Ten points exceeded the DFITS threshold of 0.34.
4. Interaction model with year*origin (weight, year, origin, year*origin): R squared 0.8773, adjusted R squared 0.8724, standard error 0.0535, F statistic 178.71, Significance F 1.25E-44, Durbin-Watson 1.78215. All four predictors were significant at the 5% level, although origin and year*origin each had p values of about 0.02 and made only a small contribution.
5. Final model with accel*year (weight, accel, year, accel*year): R squared 0.8965, adjusted R squared 0.8923, standard error 0.0491, F statistic 216.52, Significance F 2.57E-48, Durbin-Watson 1.97120.

The equation of the third model, which serves as the simpler comparison model, is:

```
log(mpg) = -27.67 - 0.00013922 weight + 0.01463 year
```

### Final Model Equation

```
log(mpg) = 40.2 - 0.00014616 weight - 4.157 accel - 0.01914 year + 0.002068 accel*year
```

### Coefficients and Interpretation

The estimated coefficients of the final model are as follows:

1. Intercept: 40.2336, standard error 14.3618, t statistic 2.80, p value 0.0061.
2. weight: negative 0.00014616, standard error 0.0000071, t statistic negative 20.49, p value 1.44E-37, VIF 1.44.
3. accel: negative 4.1571, standard error 0.8719, t statistic negative 4.77, p value 6.33E-06, VIF 274644.84.
4. year: negative 0.019143, standard error 0.0071, t statistic negative 2.68, p value 0.0086, VIF 30.92.
5. accel*year: 0.0020683, standard error 0.00043, t statistic 4.77, p value 6.25E-06, VIF 276069.64.

The report interprets these coefficients as follows. weight is the strongest predictor. Its negative coefficient indicates that heavier vehicles achieve lower fuel efficiency, which is consistent with physical expectations. accel, which was not significant in the simpler models, becomes a significant main effect once the accel*year interaction is included. Because the interaction term is present, the year coefficient represents the effect of year when accel equals zero. The negative year main effect combined with the positive accel*year coefficient means that the effect of model year on fuel efficiency depends on the acceleration level of the vehicle. The highly significant interaction term indicates that omitting it would miss an important pattern in the data.

The very large VIF values for accel and accel*year are expected in interaction models, because the interaction term is constructed from its parent variables. The report states that this inflation does not invalidate the model, and that the strong p values indicate that multicollinearity does not distort inference about the interaction.

### Model Fit Statistics

The final model has a multiple R of 0.9468, an R squared of 0.8965, and an adjusted R squared of 0.8923, so it explains about 89% of the variability in log(mpg). The standard error of the regression is 0.0491, based on 105 observations. The ANOVA table reports a regression sum of squares of 2.0896 on 4 degrees of freedom, a residual sum of squares of 0.2413 on 100 degrees of freedom, an F statistic of 216.52, and a Significance F of 2.57E-48.

### Test for Improved Fit

The increase in R squared test compared the final model (R squared 0.8965, 100 residual degrees of freedom) with the third model (R squared 0.8705, 102 residual degrees of freedom). Adding accel and accel*year increased R squared by 0.0260. The resulting F statistic of 12.572 exceeds the critical value of 3.087 at a significance level of 0.05, and the associated p value is 1.35E-05. The null hypothesis of no model improvement is therefore rejected, and the full model fits the data significantly better than the restricted model.

### Diagnostic Conclusions

The diagnostic results for the final model are as follows:

1. Normality: the normal probability plot of residuals aligns well with the reference line, with only minor deviations in the tails. The Anderson-Darling statistic is 0.240 with a p value greater than 0.250, so the normality assumption is met. The histogram of residuals is roughly symmetric and unimodal.
2. Linearity and constant variance: the residuals versus fits plot shows no funneling, curvature, or systematic structure, so the linearity and constant variance assumptions are reasonable.
3. Independence: the residuals versus order plot shows no pattern, and the Durbin-Watson statistic of 1.97120 is close to 2, giving no evidence of autocorrelation.
4. Influential points: nine observations exceeded the DFITS threshold of 0.44. Because statistical diagnostics alone, without external evidence, cannot establish whether these observations are erroneous, no observations were removed from the dataset.

The report concludes that every diagnostic statistic supports the model assumptions, and that the final model is statistically sound, interpretable, and an improved representation of the relationship between vehicle characteristics and fuel efficiency.

## Prediction

The final model was used to predict the fuel efficiency of a new vehicle with the following characteristics:

1. weight: 2200
2. accel: 15
3. year: 2022
4. accel*year: 30330

The predicted log(mpg) is 1.57901, which corresponds to a predicted fuel efficiency of 37.9 mpg. The workbook also reports an estimated mean fuel efficiency, E[mpg], of 38.20.

On the log scale, the 95% confidence interval for mean log(mpg) is 1.5459 to 1.61212, and the 95% prediction interval for log(mpg) is 1.47609 to 1.68193.

On the mpg scale, the approximate 95% confidence interval for mean mpg is 35.15 to 40.94, and the 95% prediction interval for the new vehicle is 29.93 to 48.08 mpg.

For comparison, the simpler third model predicts a log(mpg) of 1.61217, which corresponds to 40.9 mpg, with a 95% prediction interval of 31.58 to 53.01 mpg. The final model produces a narrower prediction interval because it accounts for more of the variability in log(mpg) and has a smaller residual standard error.

## Conclusion

The final model, which includes weight, acceleration, model year, and the interaction between acceleration and model year, gives a statistically stronger and more reliable explanation of log(mpg) than the simpler model containing only weight and year. It has a higher R squared (89.6% compared with 87.1%) and a lower residual standard error, and the increase in R squared test confirms that the improvement is statistically significant. Vehicle weight is the dominant predictor of fuel efficiency, and the effect of model year depends on acceleration.

The final model also yields narrower confidence and prediction intervals. For the new vehicle, its 95% prediction interval of 29.93 to 48.08 mpg is considerably narrower than the interval of 31.58 to 53.01 mpg produced by the simpler model. The report therefore recommends the final adjusted model for prediction.

The report notes the following limitations and cautions:

1. Several observations in each model exceeded the DFITS threshold for influence. These observations were retained because statistical diagnostics alone, without external evidence, cannot determine whether they are erroneous.
2. The predictors in the interaction models have very large variance inflation factors. The report attributes this to the construction of the interaction terms and states that it does not invalidate the significance tests.
3. The residuals of the final model show minor deviations from normality in the tails, which the report considers consistent with acceptable normality.
4. The interaction model with year*origin improved R squared only slightly over the simpler model, which indicates that the simpler model already captured most of the structure in the data.

## Repository Contents

1. PRIDE_MUDONDO_LINEAR_REGRESSION_PROJECT_REPORT.pdf: the full written report, dated December 02, 2025. It contains the introduction, exploratory data analysis, correlation analysis, each regression model with its results and diagnostic plots, the model comparison test, the forecast, and the conclusions and recommendations.
2. PRIDE_MUDONDO_REGRESSION_ANALYISIS_PROJECT_EXCEL.xlsx: the Excel workbook containing the data, all supporting calculations, and the Minitab outputs used in the analysis. Its worksheets are described below.
3. README.md: this document.
4. .gitignore: excludes Excel temporary lock files and operating system files from version control.

The workbook contains the following worksheets:

1. auto-mpg: the full dataset of 105 vehicles, including log(mpg), all predictors, the constructed interaction variables, the variable definitions, and the mean and standard deviation of mpg.
2. correlation_matrix: the correlation matrix of log(mpg) and the seven predictors, with a highlighting threshold of 0.4.
3. regression_model_data, first_regression_model, and influential_points_first_model: the data, Excel regression output, and DFITS influence analysis for the first model.
4. dataset_for_model_2, second_model, and influential_points_second_model: the corresponding sheets for the second model.
5. dataset_for_model_3, Third Model, and influential_points_for_model_3: the corresponding sheets for the third model.
6. fouth & fifth_model_data, fouth model, fifth model, and influential_point_model_5: the data and regression output for the weight*origin model and the year*origin model, with the influence analysis for the latter.
7. sixth model data, sixth model, and influential point for sixth mod: the data, regression output, and influence analysis for the final model.
8. sixth & third model_comparison: the increase in R squared test comparing the final model with the third model.
9. Prediction: the Minitab prediction output and the Excel calculation of the predicted value and the 95% confidence and prediction intervals for the new vehicle.

## Tools Used

Excel and Minitab were used concurrently throughout the project, with Minitab outputs transferred into the Excel workbook so that the results of both tools are kept together.

1. Microsoft Excel: data preparation, the correlation matrix, regression analysis, DFITS influence analysis, the model comparison test, the prediction interval calculations, and storage of the Minitab outputs.
2. Minitab: regression and ANOVA output, Durbin-Watson statistics, histograms, normal probability plots with Anderson-Darling tests, residual plots, and prediction output, including the fitted value and the 95% confidence and prediction limits for the new vehicle.

## Author

Pride Mudondo
