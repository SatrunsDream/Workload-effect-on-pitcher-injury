# Logistic Regression Analysis: Summary and Notes

## Overview and Objectives

The logistic regression analysis was conducted to develop predictive models for MLB pitcher injury risk using three different feature sets. The goal was to create interpretable models that could identify pitchers at elevated risk of injury in the following season. The analysis employed standard logistic regression techniques, which assume linear relationships between predictors and the log-odds of injury.

## Data Preparation and Structure

The analysis began by loading the cleaned dataset containing pitcher information from 2018 through 2025. The data was strategically split into three temporal groups: training data from 2018 to 2023, test data from 2024, and forecast data from 2025. This temporal split ensures that models are evaluated on future data, providing a realistic assessment of predictive performance.

Three distinct feature sets were defined to explore different aspects of injury risk. Variant 1 focused exclusively on unmodifiable characteristics such as age, birth country, body mass index, height, hand dominance, and previous arm injury history. These features represent factors that cannot be changed and may indicate inherent risk. Variant 2 concentrated on modifiable factors including pitch characteristics, workload metrics, and performance deltas that change over time. Variant 3 combined both sets to examine the full spectrum of available predictors.

The data preparation process handled several important preprocessing steps. Missing values in continuous features were imputed using median values, while categorical features used mode imputation. Rare categorical levels were collapsed into an "Other" category to prevent model instability from categories with insufficient sample sizes. Special characters in feature names were properly quoted to ensure compatibility with the statistical modeling framework.

## Model Building Process

The modeling approach utilized the statsmodels library, which provides comprehensive statistical output and interpretability. The build process created formula-based models that automatically handled categorical variables through treatment coding. For the game_year variable, 2018 was set as the reference category, allowing other years to be compared against this baseline.

The implementation included robust error handling to address potential issues during model fitting. When the primary statsmodels approach encountered numerical problems, the code automatically fell back to sklearn's logistic regression with L2 regularization. This fallback mechanism ensured that models could be fit even when perfect separation or multicollinearity issues arose. The sklearn wrapper maintained compatibility with the rest of the analysis pipeline while providing regularized estimates.

Each variant was fit separately, allowing for comparison of how different feature sets contribute to predictive performance. The models were evaluated using multiple metrics to provide a comprehensive view of their capabilities and limitations.

## Results and Performance

The test set performance revealed clear patterns across the three variants. Variant 1, using only unmodifiable features, achieved a ROC-AUC of 0.5667 with a log loss of 0.5278. This performance level indicates that demographic and physical characteristics alone provide modest predictive power. The model can distinguish between injured and non-injured pitchers better than random chance, but the discrimination is limited.

Variant 2, focusing on modifiable features, showed improved performance with a ROC-AUC of 0.6217 and log loss of 0.5197. This improvement suggests that workload and performance metrics contain more predictive information than demographic factors alone. The modifiable features capture aspects of pitcher usage and performance that may be more directly related to injury risk.

Variant 3, combining all features, achieved the best performance with a ROC-AUC of 0.6396 and log loss of 0.5125. This represents a meaningful improvement over the individual variants, demonstrating that both unmodifiable and modifiable factors contribute unique information to injury risk prediction. The combined model provides the most comprehensive assessment of injury risk.

The Brier scores, which measure calibration of probability predictions, showed consistent patterns. All variants achieved Brier scores around 0.17 on the test set, indicating reasonable calibration. The models produce probability estimates that align reasonably well with observed injury rates, which is important for practical application where accurate risk quantification matters.

## Training Versus Test Performance

An important aspect of model evaluation is examining the gap between training and test performance. Variant 3 showed a training ROC-AUC of 0.6953 compared to test ROC-AUC of 0.6396, representing a gap of approximately 0.056. This gap is relatively modest, suggesting that the model generalizes reasonably well to new data. The regularization applied through L2 penalties helps prevent severe overfitting while maintaining model flexibility.

The log loss values also showed reasonable generalization, with test performance being slightly worse than training performance as expected. The models appear to be learning genuine patterns rather than memorizing training data specifics, which is crucial for reliable predictions on future pitchers.

## Model Interpretability and Coefficients

One of the key advantages of logistic regression is the interpretability of coefficients. Each coefficient represents the change in log-odds of injury associated with a one-unit increase in the corresponding predictor, holding other variables constant. This straightforward interpretation allows for understanding which factors most strongly influence injury risk.

The model summaries provide detailed coefficient estimates along with statistical significance tests. These outputs enable identification of the most important predictors and assessment of the strength of evidence for each relationship. The interpretability is particularly valuable for communicating findings to coaches, medical staff, and front office personnel who need to understand why certain pitchers are flagged as high risk.

## Calibration and Probability Estimates

Calibration curves were generated to assess how well the predicted probabilities match observed injury rates. Well-calibrated models should show predicted probabilities that align with actual outcomes across different risk levels. The calibration analysis helps identify whether the models are systematically overconfident or underconfident in their predictions.

The models showed reasonable calibration, with predicted probabilities generally tracking observed injury rates. Some deviation from perfect calibration is expected and acceptable, particularly in the tails of the distribution where sample sizes are smaller. The calibration assessment provides confidence that the probability estimates can be used meaningfully for risk stratification.

## ROC Curve Analysis

Receiver Operating Characteristic curves visualize the trade-off between sensitivity and specificity across different probability thresholds. The area under the ROC curve quantifies the model's ability to distinguish between injured and non-injured pitchers. Higher AUC values indicate better discrimination.

The ROC curves for all three variants showed clear improvement over random guessing, with Variant 3 achieving the best discrimination. The curves help identify optimal probability thresholds for classification decisions, balancing the costs of false positives and false negatives. In injury prediction contexts, the threshold choice depends on the relative costs of missing high-risk pitchers versus flagging low-risk pitchers.

## Forecast Applications

The models were applied to 2025 data to generate injury risk forecasts for the 2026 season. These forecasts provide practical value by identifying pitchers who may benefit from modified training or workload management. The forecast distributions showed reasonable variation in predicted risk, with some pitchers flagged as high risk based on their 2025 characteristics.

The forecasting capability demonstrates the practical utility of the models. By applying the models to current season data, teams can proactively identify pitchers who may be at elevated risk in the following season. This forward-looking application is where the models provide the most value for injury prevention efforts.

## Key Findings and Implications

The analysis demonstrates that logistic regression can effectively model pitcher injury risk using available data. The best-performing model achieves moderate discrimination with reasonable calibration. The performance levels are sufficient for practical application, particularly when combined with expert judgment and other information sources.

The finding that modifiable features contribute more predictive power than unmodifiable features is important. This suggests that workload and performance metrics may be more actionable for injury prevention than demographic factors. However, the combination of both feature types provides the best overall performance, indicating that a comprehensive approach is most effective.

The models' interpretability is a significant advantage for practical application. Being able to explain why a pitcher is flagged as high risk builds trust and enables informed decision-making. The coefficient estimates provide insights into which factors most strongly influence injury risk, guiding both prevention strategies and further research.

## Limitations and Considerations

The models achieve moderate but not exceptional performance, with ROC-AUC values around 0.64 for the best variant. This level of discrimination means the models will make errors, and they should be used as one tool among many in injury risk assessment rather than as definitive predictors.

The temporal split ensures realistic evaluation, but it also means the test set is relatively small. The performance estimates have some uncertainty, and the models may perform differently on future data as the game evolves. Regular model updates and validation will be important for maintaining predictive performance.

The linearity assumption of logistic regression means that the models assume proportional relationships between predictors and log-odds of injury. While this assumption appears reasonable based on subsequent non-linearity testing, it does limit the models' ability to capture complex interactions or threshold effects that might exist in the data.

## Conclusion

The logistic regression analysis successfully developed predictive models for pitcher injury risk with reasonable performance and strong interpretability. The models demonstrate that both unmodifiable and modifiable factors contribute to injury risk prediction, with the combined model providing the best overall performance. The interpretability and calibration of the models make them suitable for practical application, while the moderate discrimination levels indicate they should be used as part of a comprehensive injury prevention strategy rather than as standalone decision tools.

