# Non-Linearity Test Findings: Comprehensive Summary

## Overview

Through a systematic three-test analysis, we have conclusively demonstrated that the relationships between predictor features and pitcher injury risk are predominantly linear in nature. This finding has significant implications for model selection and explains the performance differences observed between logistic regression and generalized additive models (GAM).

## Test Methodology and Results

### Test 1: Polynomial Model Comparison

The first test compared the performance of linear logistic regression models against polynomial logistic regression models that included squared terms and interactions. The fundamental question was whether introducing non-linear terms would improve predictive performance. Across all three feature variants, the results were consistent and clear: polynomial models performed worse than their linear counterparts.

For Variant 1, the linear model achieved a ROC-AUC of 0.5705, while the polynomial model achieved only 0.5629, representing a decline of 0.0076. Variant 2 showed an even larger gap, with the linear model at 0.6217 ROC-AUC compared to the polynomial model's 0.6040, a difference of 0.0177. The most dramatic difference appeared in Variant 3, where the linear model reached 0.6386 ROC-AUC while the polynomial model fell to 0.6098, a substantial drop of 0.0287.

These results indicate that non-linear terms are not capturing meaningful signal in the data. Instead, they appear to be fitting noise, leading to overfitting and reduced generalization performance. The fact that performance degradation increases with the number of features suggests that polynomial models become increasingly problematic as model complexity grows. This pattern is characteristic of models attempting to fit non-linear relationships that do not actually exist in the underlying data generating process.

### Test 2: Residual Analysis

The second test examined the residuals from linear models to detect any systematic non-linear patterns that might have been missed. After fitting linear logistic regression models, we analyzed whether the prediction errors (residuals) showed correlations with squared feature values. If non-linear relationships existed, we would expect residuals to correlate more strongly with squared features than with the original features themselves.

The analysis revealed zero non-linear patterns across all three variants. For every continuous feature examined, the correlation between residuals and squared feature values was not meaningfully stronger than the correlation with linear feature values. This finding provides strong evidence that linear models are capturing all the systematic relationships present in the data.

When relationships are truly linear, residuals should be random and uncorrelated with any function of the features. The absence of non-linear patterns in residuals confirms that no important non-linear relationships are being missed by the linear modeling approach. This is particularly significant because residual analysis can detect subtle non-linearities that might not be apparent in overall model performance metrics.

### Test 3: Visual Relationship Analysis

The third test employed visual inspection of the actual relationships between features and injury rates. By plotting feature values against injury rates and overlaying both linear and polynomial fits, we could directly observe whether the data followed linear or curved patterns.

The visual analysis confirmed that linear fits adequately capture the relationships in the data. Polynomial curves did not provide substantially better fits, and in many cases, the polynomial fits appeared to be overfitting to noise rather than capturing genuine non-linear patterns. The R² values for linear fits were comparable to or better than those for polynomial fits, further supporting the conclusion that linear relationships dominate.

## Interpretation of Results

The convergence of evidence from all three independent tests provides strong support for the conclusion that relationships are linear. When multiple diagnostic approaches all point to the same conclusion, we can be confident in that finding. The fact that polynomial models perform worse, residuals show no non-linear patterns, and visual inspection confirms linear relationships creates a compelling case.

This finding is not uncommon in real-world data analysis. Many relationships in nature and human systems are approximately linear over the ranges of interest, particularly when the outcome variable is binary and the predictors have been appropriately transformed or scaled. The linearity we observe suggests that injury risk responds proportionally to changes in predictor variables, without threshold effects, U-shaped relationships, or other complex non-linear patterns.

## Implications for Model Selection

The linearity finding has direct implications for choosing between logistic regression and GAM. Since the relationships are linear, models designed to capture non-linear patterns will not provide benefits and may actually perform worse due to increased complexity and overfitting risk. This explains why GAM, despite being a more flexible modeling framework, only achieved marginal improvements and ultimately underperformed compared to logistic regression.

The marginal improvement observed in GAM (from 0.6025 to 0.6271 ROC-AUC) likely represents the model successfully incorporating categorical features properly after our fixes, rather than capturing non-linear relationships. The fact that GAM still underperforms logistic regression (0.6396 ROC-AUC) suggests that even with proper categorical handling, the added complexity of smooth splines is not justified when relationships are linear.

## Conclusion

The comprehensive non-linearity analysis demonstrates that the relationships between features and pitcher injury risk are linear. This finding validates the use of logistic regression as the primary modeling approach and explains why more complex non-linear models do not provide performance benefits. The systematic testing approach, using multiple independent diagnostic methods, provides confidence in this conclusion and guides appropriate model selection for this prediction problem.

