# GAM Logistic Regression Analysis: Summary and Notes

## Overview and Objectives

The Generalized Additive Model (GAM) analysis was conducted to explore whether non-linear modeling approaches could improve upon the performance achieved by standard logistic regression. GAMs use smooth spline functions to capture potentially complex, non-linear relationships between predictors and outcomes, making them more flexible than linear models. The analysis employed the same three feature variants as the logistic regression analysis to enable direct comparison.

## Initial Implementation Challenges

The initial GAM implementation encountered significant challenges with categorical feature handling. The first attempt tried to include categorical features by one-hot encoding them and appending them to the continuous feature matrix. However, pygam's LogisticGAM does not natively support linear terms for one-hot encoded categorical variables in the way initially attempted. This led to the model falling back to using only continuous features, effectively discarding important categorical information such as birth country, hand dominance, and pitcher role.

The fallback mechanisms in the original code attempted to fit models with only continuous features when the full model failed. This meant that Variant 1, which contains several important categorical features, was particularly disadvantaged. The initial results showed poor performance, with GAM underperforming logistic regression across all variants.

## Revised Implementation with Proper Categorical Handling

The implementation was revised to properly handle categorical features using pygam's factor terms, denoted by f(). Categorical features were label-encoded as integers rather than one-hot encoded, which is the format required for factor terms in pygam. This change allowed the model to properly incorporate all available features, both continuous and categorical.

The revised approach separated continuous and categorical features at the design matrix construction stage. Continuous features were prepared as a matrix for smooth spline terms, while categorical features were encoded as integers and stored with their corresponding label encoders for consistent prediction on new data. The GAM terms were constructed by combining smooth terms s() for continuous features with factor terms f() for categorical features.

Smoothing parameters were set to moderate values, with lam=0.6 providing a balance between flexibility and regularization. The number of splines was set to 8 per continuous feature, which provides reasonable flexibility without excessive complexity. Fallback mechanisms were implemented to reduce smoothing or spline count if the initial fit failed, ensuring robust model fitting across different feature combinations.

## Model Building Process

The GAM models were built using pygam's LogisticGAM class, which implements penalized iteratively reweighted least squares (P-IRLS) for fitting. The process involved constructing the design matrix with continuous features first, followed by categorical features, then combining them into a single matrix. The GAM terms specified which columns received smooth spline treatment and which received factor treatment.

Each variant was fit separately, maintaining consistency with the logistic regression analysis. The models stored metadata including the original feature lists, categorical encoders, and design matrix structure to enable proper prediction on new data. This metadata is crucial because prediction requires reconstructing the design matrix in the same format as training.

The prediction function was carefully implemented to handle the encoding of new data. Categorical features must be encoded using the same label encoders from training, and unseen categories are mapped to known categories to prevent prediction failures. The continuous and categorical features are combined in the same order as training to ensure proper alignment with the model's term structure.

## Results and Performance

After fixing the categorical handling, GAM performance improved but still fell short of logistic regression. Variant 1 achieved a test ROC-AUC of 0.5632 with log loss of 0.5271. Variant 2 reached 0.6086 ROC-AUC with log loss of 0.5232. Variant 3, the combined model, achieved 0.6271 ROC-AUC with log loss of 0.5191.

The improvement from the initial implementation to the revised version demonstrates that proper categorical handling was important. However, the fact that GAM still underperforms logistic regression, which achieved 0.6396 ROC-AUC for Variant 3, indicates that the added complexity of smooth splines is not providing benefits for this particular problem.

## Training Versus Test Performance Gaps

The GAM models showed larger gaps between training and test performance compared to logistic regression. Variant 3 achieved a training ROC-AUC of 0.7108 but only 0.6271 on the test set, representing a gap of 0.0837. This is substantially larger than the 0.0557 gap observed in logistic regression, suggesting that GAM is overfitting more despite the smoothing penalties.

The larger overfitting gap indicates that the smooth splines are capturing patterns in the training data that do not generalize to new data. This is consistent with the finding that relationships are linear, as the splines have nothing meaningful to capture and instead fit noise. The smoothing parameters help reduce this overfitting but cannot completely eliminate it when the model structure is mismatched to the data characteristics.

## Effective Degrees of Freedom Analysis

The effective degrees of freedom (EDF) analysis provides insight into whether the smooth splines are actually capturing non-linear relationships. EDF values close to 1 indicate that a smooth term is essentially linear, while higher values suggest non-linearity. The analysis revealed that the smooth terms in the GAM models have EDF values close to 1, confirming that the relationships are linear.

This finding explains why GAM does not provide performance benefits. The smooth splines are reducing to essentially linear functions, but with the overhead of the spline framework. The model is doing extra work to achieve linear relationships that logistic regression captures more directly and efficiently.

## Partial Dependence Plots

Partial dependence plots were generated to visualize the shape of relationships captured by the smooth splines. These plots show how each continuous feature influences the predicted log-odds of injury, holding other features constant. The plots revealed that the smooth functions are essentially linear, with minimal curvature.

The linear appearance of partial dependence plots further confirms that non-linear modeling is unnecessary. The smooth splines are not finding curves, thresholds, or other complex patterns that would justify their use. Instead, they are approximating straight lines, which logistic regression handles more efficiently.

## Why GAM Underperformed

The comprehensive non-linearity testing conducted separately revealed that all relationships between features and injury risk are linear. Polynomial models performed worse than linear models, residuals showed no non-linear patterns, and visual inspection confirmed linear relationships. This fundamental finding explains why GAM underperforms.

GAM is specifically designed to capture non-linear relationships through smooth splines. When relationships are linear, this design becomes a liability rather than an asset. The smooth splines attempt to fit curves to relationships that are straight lines, introducing unnecessary complexity. This complexity leads to overfitting, as the splines have many parameters that can be adjusted to fit noise in the training data.

The smoothing parameters attempt to prevent overfitting by penalizing wiggliness in the smooth functions. However, when relationships are linear, the smoothing parameters essentially force the splines to be linear, but the model still incurs the computational and statistical costs of the spline framework. It's like using a sophisticated curve-fitting tool to draw a straight line when a ruler would be simpler and more effective.

## Marginal Improvement After Fixes

The improvement from the initial broken implementation to the revised version represents proper use of available information rather than capture of non-linear patterns. The fix allowed GAM to properly incorporate categorical features that had been discarded in the original implementation. This improvement demonstrates that the categorical features contain predictive information, but it does not indicate that non-linear modeling is beneficial.

The fact that GAM still underperforms logistic regression even after proper categorical handling shows that the smooth splines are not adding value. The marginal improvement was necessary to make fair comparisons, but it does not change the fundamental conclusion that linear modeling is more appropriate for this problem.

## Computational Considerations

GAM models require more computational resources than logistic regression. The fitting process involves iterative optimization of smooth functions with multiple parameters per feature. Prediction requires evaluating spline basis functions, which adds computational overhead. These costs are not justified when relationships are linear, as simpler models achieve better performance with less computational effort.

The increased complexity also makes GAM models more difficult to interpret. While partial dependence plots can help visualize relationships, understanding how multiple smooth functions interact is more challenging than interpreting linear coefficients. The interpretability advantage of logistic regression is significant for practical application.

## Lessons Learned

The GAM analysis provides important lessons about model selection. More sophisticated models are not always better, and their advantages only materialize when the data exhibits the patterns they are designed to capture. The comprehensive non-linearity testing should be standard practice before selecting modeling approaches, as it provides crucial information for making informed decisions.

The experience also highlights the importance of proper implementation. The initial categorical handling issues masked the true performance of GAM, and only after fixing these issues could fair comparisons be made. However, even with proper implementation, GAM underperforms because it is solving the wrong problem for this dataset.

## Conclusion

The GAM analysis demonstrates that non-linear modeling does not improve upon logistic regression for pitcher injury prediction. The relationships are linear, making GAM's sophisticated non-linear capabilities unnecessary and counterproductive. The smooth splines introduce complexity that leads to overfitting without providing corresponding benefits. The proper categorical handling improved performance but did not change the fundamental conclusion that logistic regression is the more appropriate modeling approach for this problem. The analysis reinforces the importance of matching model complexity to data characteristics and conducting thorough diagnostic testing before selecting modeling approaches.

