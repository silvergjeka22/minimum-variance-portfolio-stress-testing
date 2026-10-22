# Minimum Variance Portfolio Stress Testing

This project builds a two asset Minimum Variance Portfolio (MVP) that is rebalanced every day, and then tests how well different models can predict its risk, especially during periods of market stress.

The project was developed for the AI in Finance course of the Master's Degree in Artificial Intelligence at the University of Verona.

## Overview

The portfolio is made of two stocks:

- Pfizer (PFE): a pharmaceutical company that was strongly affected by the COVID-19 pandemic.
- Rheinmetall (RHM.DE): a German defense company that was strongly affected by the war in Ukraine and the rise in European defense spending.

These two assets were chosen because they react in very different ways to major market events, which makes them a good test case for a portfolio that must stay safe under stress.

The main steps of the project are:

1. Download daily prices from 2010 onward using the yfinance library.
2. Compute daily returns from closing prices.
3. Estimate a covariance matrix every day using a rolling window of recent returns.
4. Compute the Minimum Variance Portfolio weights for each day.
5. Estimate the daily Value at Risk (VaR) and Expected Shortfall (ES) at a 95 percent confidence level using several models.
6. Compare the models on a training period (2010 to 2023) and on an unseen test period (2024 to 2025).

## Risk Models Compared

- Normal (parametric) model: assumes returns follow a normal distribution.
- Historical simulation: uses the distribution of past returns in a rolling window.
- Stress amplification: gives more weight to returns from high volatility days, using a fixed volatility threshold.
- Stress amplification with z-score: detects stress days using a z-score of volatility instead of a fixed threshold.
- LSTM neural network: a deep learning model that learns patterns in the return series to forecast risk.

Each model is evaluated with two metrics:

- VaR violation rate: the percentage of days where the real loss was larger than the predicted VaR. With 95 percent confidence the ideal value is close to 5 percent.
- Actual ES compared to predicted ES: how close the predicted average extreme loss is to the real one.

## Main Results

On the 2024 to 2025 test period:

- The historical model and the z-score stress model had a violation rate of 7.29 percent, which means they underestimated risk.
- The fixed threshold stress model had a violation rate of 0.69 percent, which means it was too conservative and overestimated losses.
- The LSTM model gave the most balanced result, with a violation rate of 3.36 percent and a predicted ES (0.0283) close to the actual ES (0.0329).

The portfolio weights put most of the capital in Pfizer, because Rheinmetall has been much more volatile since 2022.

## Repository Structure

```
notebooks/
    mvp_stress_testing_lstm.ipynb   Full code, plots and analysis
docs/
    project_report.pdf              Written project report
    presentation_slides.pdf         Presentation slides
requirements.txt                    Python packages needed to run the notebook
```

## How to Run

1. Install Python 3.10 or newer.
2. Install the required packages:

```bash
pip install -r requirements.txt
```

3. Start Jupyter and open the notebook:

```bash
jupyter notebook notebooks/mvp_stress_testing_lstm.ipynb
```

The notebook downloads the price data from Yahoo Finance when it runs, so an internet connection is needed. Results can change slightly from the ones above because new price data is added over time and because LSTM training is random.

## Authors

- Giacomo Lucato
- Silver Gjeka

University of Verona, Master's Degree in Artificial Intelligence, AI in Finance course.
