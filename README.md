# The Quant Pipeline: From Fundamental Data to a Machine Learning-Based Long/Short Portfolio

An end-to-end quantitative finance project using Random Forest classification, fundamental stock data, probability-based stock ranking, long/short portfolio construction, and financial risk analysis.

## Project Overview

This project investigates whether a machine learning model trained on company fundamentals, valuation metrics, and market-related characteristics can identify stocks that outperform the S&P 500 and whether those predictions can be translated into a long/short portfolio with meaningful risk-adjusted performance.

The project follows an end-to-end quantitative finance and machine learning workflow, beginning with data engineering and exploratory analysis, followed by chronological data splitting, Random Forest classification, probability-based stock ranking, portfolio construction, and financial risk evaluation.

Rather than focusing only on classification accuracy, the project also examines whether the predicted probabilities provide useful information when ranking stocks relative to one another.

## Research Question

**Can a machine learning model trained on company fundamentals identify stocks that outperform the S&P 500, and can those predictions later be translated into a long/short portfolio with meaningful risk-adjusted performance?**

## Technologies and Tools

- **Programming:** Python
- **Data Analysis:** Pandas, NumPy
- **Machine Learning:** Scikit-learn, Random Forest Classifier
- **Visualization:** Matplotlib
- **Development Environment:** Jupyter Notebook
- **Financial Analysis:** Long/Short Portfolio Construction, Sharpe Ratio, Sortino Ratio, Value at Risk (VaR), Conditional Value at Risk (CVaR)

## Project Methodology

### Part 1: Data Engineering and Preparation

The dataset contains historical observations of company fundamentals, valuation measures, and market-related characteristics.

The initial dataset consisted of 8,682 observations and 48 columns. Data cleaning included inspecting missing values, identifying repeated ticker-date combinations, standardizing ticker symbols, converting dates into a chronological format, and constructing a binary outperformance target.

The target variable was defined as:

- **1:** The stock's forward return exceeded the S&P 500's corresponding forward return.
- **0:** The stock did not outperform the S&P 500.

The observations were organized into calendar quarters to support later stock ranking and portfolio construction.

### Part 2: Chronological Train-Test Split

Financial observations are time-sensitive, making chronological ordering important for preventing look-ahead bias.

The dataset was divided into historical training and later testing periods:

| Dataset | Period | Observations |
|---|---|---:|
| Training | 2003 Q4 – 2010 Q4 | 5,550 |
| Testing | 2011 Q1 – 2013 Q2 | 2,935 |

This approach allowed the model to learn from earlier observations and be evaluated on later, unseen data.

### Part 3: Feature Selection and Preprocessing

The model used 41 predictor variables covering company fundamentals, valuation metrics, market indicators, and trading-related characteristics.

Missing predictor values were imputed using median values calculated from the training dataset only.

This approach reduced the risk of data leakage by preventing information from the testing period from influencing preprocessing decisions.

### Part 4: Random Forest Classification

A baseline Random Forest classifier was trained to predict whether individual stocks would outperform the S&P 500.

The model was evaluated using training and testing accuracy, precision, recall, F1 score, and a confusion matrix.

**Classification Results**

| Metric | Result |
|---|---:|
| Training Accuracy | 99.6% |
| Testing Accuracy | 49.1% |
| Majority-Class Baseline | 50.1% |

The substantial difference between training and testing accuracy indicates that the Random Forest overfit the training observations and demonstrated limited out-of-sample classification performance.

Rather than relying exclusively on binary predictions, the project continued by evaluating the model's predicted probabilities as relative stock-ranking scores.

### Part 5: Probability-Based Stock Ranking

Within each testing quarter, stocks were ranked according to their predicted probability of outperforming the S&P 500.

Two groups were constructed:

- **Long Portfolio:** Highest-ranked 10% of stocks.
- **Short Portfolio:** Lowest-ranked 10% of stocks.

The long and short groups contained equal numbers of stocks within each quarter.

This approach evaluated whether the model could distinguish between relatively stronger and weaker stocks, even when overall classification accuracy was limited.

### Part 6: Portfolio Performance Evaluation

The average realized forward returns of the long and short groups were calculated for each testing quarter.

The long-short return spread was defined as the average forward return of the long group minus the average forward return of the short group.

**Portfolio Results**

| Metric | Result |
|---|---:|
| Average Long Portfolio Forward Return | 29.01% |
| Average Short-Group Stock Forward Return | 19.59% |
| Average Long-Short Return Spread | 9.43% |
| Quarters with Positive Long-Short Spread | 6 out of 10 |
| Average S&P 500 Forward-Return Benchmark | 17.57% |

Although the model performed poorly as a binary classifier, its probability rankings produced a positive average long-short return spread during the testing period.

However, the results were not consistently positive across every quarter, and stocks assigned to the short group still earned positive forward returns on average.

### Part 7: Financial Risk Analysis

The project evaluated the long-short return spreads using several financial risk measures.

| Risk Metric | Result |
|---|---:|
| Sharpe Ratio (Non-Annualized) | 0.70 |
| Sortino Ratio (0% Target) | 6.49 |
| Historical 95% VaR | -3.00% |
| Historical 95% CVaR | -3.62% |

The Sharpe Ratio indicates positive but moderate return relative to overall variation in the observed spreads.

The larger Sortino Ratio reflects the limited downside variation in the sample. However, the testing period includes only ten quarters, making these statistics sensitive to a small number of observations.

The VaR and CVaR results provide descriptive information about the observed lower tail of the return distribution rather than reliable forecasts of future losses.

### Part 8: Discussion, Limitations, and Future Improvements

The project identified several limitations:

- **Overfitting:** The baseline Random Forest achieved strong training accuracy but weak testing accuracy.
- **Limited Testing Sample:** Only ten testing quarters were available for portfolio and risk evaluation.
- **Overlapping Forward Returns:** Forward-return windows may overlap across adjacent testing quarters.
- **Portfolio Timing:** Stocks grouped within the same calendar quarter may have different observation dates.
- **Trading Constraints:** Transaction costs, taxes, liquidity, short-selling expenses, and execution timing were not incorporated.
- **Predictor Composition:** The model included market and trading indicators in addition to company fundamentals and valuation measures.

Future improvements could include walk-forward hyperparameter tuning, feature reduction, alternative machine learning models, longer out-of-sample testing periods, and a more realistic portfolio implementation framework.

## Main Findings

The project demonstrates that strong training performance does not necessarily translate into successful predictions on unseen financial data.

Although the Random Forest exhibited substantial overfitting and failed to outperform the majority-class baseline in classification accuracy, its predicted probabilities produced some separation between the highest-ranked and lowest-ranked stocks.

The average long-short return spread was positive, but the small testing sample and methodological limitations prevent the results from establishing a reliable, production-ready investment strategy.

## Conclusion

Overall, this project provides a complete baseline framework for applying machine learning to quantitative stock selection.

It demonstrates how financial data preparation, chronological machine learning evaluation, probability-based stock ranking, portfolio construction, and financial risk analysis can be combined into a single analytical pipeline.

The results also highlight the importance of interpreting machine learning outputs cautiously, particularly when dealing with financial data, overfitting, limited historical observations, and investment-related uncertainty.

## Disclaimer

This project was developed for academic and educational purposes. It is not financial advice, and the reported results should not be interpreted as evidence of a deployable or consistently profitable trading strategy.
