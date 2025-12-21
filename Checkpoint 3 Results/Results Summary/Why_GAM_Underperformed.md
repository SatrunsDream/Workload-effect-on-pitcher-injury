# Why GAM Underperformed: The Cost of Complexity Without Benefit

## The Fundamental Mismatch

Generalized Additive Models (GAM) are specifically designed to capture non-linear relationships through smooth spline functions. The core strength of GAM lies in its ability to model complex, curved relationships that linear models cannot represent. However, when the underlying relationships are actually linear, as our comprehensive testing has demonstrated, GAM's primary advantage becomes irrelevant. Instead of providing benefits, the model's added complexity becomes a liability.

The non-linearity analysis revealed that all relationships between features and injury risk are linear. Polynomial models performed worse than linear models, residuals showed no non-linear patterns, and visual inspection confirmed linear relationships. This finding directly explains why GAM, despite being a more flexible and sophisticated modeling framework, achieved only marginal improvements and ultimately underperformed compared to logistic regression.

## The Marginal Improvement Explained

After fixing the categorical feature handling in GAM, the model's performance improved from 0.6025 to 0.6271 ROC-AUC. This improvement was not due to capturing non-linear relationships, but rather to properly incorporating categorical features that had been mishandled in the initial implementation. The fix allowed GAM to use all available information, including important categorical predictors like birth country, hand dominance, and pitcher role.

However, even with proper categorical handling, GAM still underperformed logistic regression, which achieved 0.6396 ROC-AUC. This performance gap exists because GAM's smooth spline functions are attempting to model non-linear patterns that do not exist. The splines are essentially trying to fit curves to relationships that are straight lines, which introduces unnecessary complexity without corresponding benefit.

## The Overfitting Problem

When relationships are linear, smooth splines in GAM have nothing meaningful to capture. Instead, they begin to fit noise and random variations in the training data. This overfitting manifests in several ways. First, the model shows larger gaps between training and test performance compared to logistic regression. GAM Variant 3 achieved 0.6858 ROC-AUC on training data but only 0.6025 on test data, a gap of 0.0833. Logistic regression showed a smaller gap of 0.0557, indicating better generalization.

The overfitting occurs because smooth splines have many parameters that can be adjusted to fit the training data. Even with smoothing penalties, when there are no genuine non-linear patterns to capture, these parameters end up fitting random noise. The smoothing parameters attempt to prevent this, but they cannot completely eliminate the problem when the model structure itself is mismatched to the data.

## The Complexity Penalty

GAM introduces significant complexity through its smooth spline terms. Each continuous feature gets its own smooth function with multiple parameters to estimate. This complexity requires more data to estimate reliably and increases the risk of overfitting. With approximately 4,695 training samples, the data may not be sufficient to reliably estimate all the smooth functions, especially when those functions are trying to capture patterns that don't exist.

The effective degrees of freedom in GAM models are much higher than in logistic regression, even when smoothing is applied. This increased complexity means the model has more opportunities to fit noise, and the regularization provided by smoothing penalties may not be sufficient to prevent this when relationships are linear. The model is essentially fighting against itself, trying to be flexible enough to capture non-linear patterns while being constrained enough to avoid overfitting, all in service of relationships that are linear.

## The Categorical Feature Challenge

While our fixes improved GAM's handling of categorical features, the integration of categorical and continuous features in GAM remains more complex than in logistic regression. GAM uses factor terms for categorical features, which work well, but the interaction between these factor terms and smooth splines for continuous features adds another layer of complexity. Logistic regression handles this integration more straightforwardly through one-hot encoding and linear coefficients.

The complexity of integrating different feature types in GAM can lead to suboptimal performance even when individual components are working correctly. The model must balance the smoothness of continuous feature effects with the discrete nature of categorical effects, which can create challenges in parameter estimation and prediction.

## Computational and Practical Costs

Beyond performance, GAM incurs additional costs that are not justified when relationships are linear. The model requires more computational resources to train, takes longer to make predictions, and is more difficult to interpret. The smooth functions, while elegant in theory, create challenges for understanding how individual features influence predictions. Partial dependence plots can help, but they add another layer of complexity to model interpretation.

The increased computational requirements mean that GAM is less suitable for rapid iteration during model development and may face challenges in production environments where prediction speed matters. These practical considerations, combined with the performance disadvantages, make GAM a poor choice when relationships are linear.

## Why Smooth Splines Don't Help

Smooth splines in GAM are designed to capture curves, thresholds, and other non-linear patterns. When applied to linear relationships, they essentially reduce to linear functions, but with the overhead of the spline framework. The smoothing parameters attempt to make the splines as linear as possible, but this process is inefficient compared to simply using a linear model from the start.

The effective degrees of freedom for smooth terms in linear relationships should be close to one, indicating that the spline is essentially linear. However, even when this occurs, the GAM framework still requires estimating spline basis functions and applying smoothing penalties, all of which add complexity without benefit. It's like using a sophisticated curve-fitting tool to draw a straight line when a ruler would be simpler and more effective.

## The Strategic Lesson

The GAM experience provides an important lesson about model selection. More sophisticated models are not always better, and their advantages only materialize when the data exhibits the patterns they are designed to capture. When relationships are linear, simpler models like logistic regression are not just acceptable alternatives, they are actually superior choices.

This finding reinforces the importance of understanding data characteristics before selecting modeling approaches. The comprehensive non-linearity testing we conducted should be a standard part of the modeling process, as it provides crucial information for model selection. Without this testing, one might assume that more complex models would perform better, leading to wasted effort and suboptimal results.

## Conclusion

GAM underperformed because it is designed for a problem that doesn't exist in this dataset. The model's sophisticated non-linear capabilities become liabilities when relationships are linear, leading to overfitting, increased complexity, and reduced performance. The marginal improvement observed after fixing categorical handling represents proper use of available information, not the capture of non-linear patterns. This experience demonstrates that model sophistication must match data characteristics, and that comprehensive diagnostic testing is essential for making informed model selection decisions.

