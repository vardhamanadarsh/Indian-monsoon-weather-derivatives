# # Rainfall Weather Derivatives: Stochastic Modelling and Extreme Risk Analysis

## Overview

This project develops a stochastic rainfall modelling framework for pricing rainfall-based weather derivatives and assessing extreme rainfall risk.

The analysis uses daily CHIRPS precipitation data for 12 major Indian cities covering 1981–2025, with Mumbai used as the primary case study.

The project combines:

- Exploratory Data Analysis
- Rainfall trend analysis
- Markov Chain modelling
- Gamma distribution modelling
- Extreme Value Theory (EVT)
- Peaks Over Threshold (POT)
- Generalized Pareto Distribution (GPD)
- Monte Carlo simulation
- Historical Burn Analysis (HBA)
- Weather derivative pricing

---

## Objectives

1. Model daily rainfall occurrence and rainfall intensity.
2. Capture persistence in wet and dry spells.
3. Model extreme rainfall events.
4. Generate synthetic future rainfall scenarios.
5. Estimate rainfall call and put option payoffs.
6. Calculate fair premiums for rainfall weather derivatives.
7. Compare Historical Burn Analysis with Monte Carlo-based pricing.

---

## Dataset

### Source
CHIRPS (Climate Hazards Group InfraRed Precipitation with Station data)

### Coverage

- Period: 1981–2025
- Frequency: Daily
- Season: June 1 – September 30
- Monsoon observations: 122 days per year
- Cities: 12 major Indian cities
- Primary case study: Mumbai

The rainfall data were processed to construct seasonal rainfall measures and daily wet/dry rainfall states.

---

## Data Preprocessing

The rainfall data were processed in Python before modelling.

### Main preprocessing steps

1. Load daily rainfall data.
2. Convert dates into a suitable datetime format.
3. Restrict observations to the monsoon period.
4. Separate rainfall observations by city.
5. Handle missing observations.
6. Classify rainfall into wet and dry days.

A threshold of **1 mm** was used:

```text
Rainfall < 1 mm  → Dry day
Rainfall ≥ 1 mm → Wet day

For distribution modelling, only positive rainfall observations were retained.
positive_rainfall = monsoon[city][monsoon[city] > 0].dropna()

1. Rainfall Occurrence Modelling
Rainfall occurrence is modelled separately from rainfall intensity.
First-Order Markov Chain
A first-order Markov chain assumes that today's rainfall state depends only on yesterday's state.
Two transition probabilities are estimated:
- p01: Dry → Wet
- p11: Wet → Wet
For Mumbai:
p01 = 0.2389
p11 = 0.7171

These probabilities are estimated from historical rainfall transitions.
Second-Order Markov Chain
The second-order model uses the rainfall states of the previous two days.
Therefore:
P(Wet today | yesterday, day before yesterday)

Four transition probabilities are estimated for Mumbai:
P001 = 0.209
P011 = 0.662
P101 = 0.335
P111 = 0.739

The second-order model is used to better capture persistent wet and dry spells.
2. Rainfall Intensity Modelling
For wet days, rainfall intensity is modelled using the Gamma distribution.
The Gamma distribution is suitable because rainfall amounts on wet days are positive and right-skewed.
For Mumbai:
Shape (α) = 0.72
Scale (β) = 57.45

The modelling framework therefore separates rainfall into:
Markov Chain → Whether it rains
Gamma        → How much it rains

3. Extreme Rainfall Modelling
To specifically study extreme rainfall events, the project uses Extreme Value Theory (EVT).
The Peaks Over Threshold (POT) approach is used.
Extreme rainfall observations above a selected threshold are extracted and modelled using the Generalized Pareto Distribution (GPD).
This helps analyse:
- Extreme rainfall probability
- Tail behaviour
- Return levels
- Catastrophic rainfall risk
The EVT component complements the Markov–Gamma simulation by focusing specifically on the upper tail of rainfall.
4. Monte Carlo Simulation
Monte Carlo simulation is used to generate synthetic rainfall seasons.
Each simulated season contains:
122 daily rainfall observations

A total of:
10,000 synthetic rainfall seasons

are generated.
The simulation process is:
Markov Chain
     ↓
Generate wet/dry states
     ↓
Gamma Distribution
     ↓
Generate rainfall amount on wet days
     ↓
122-day synthetic monsoon season
     ↓
Calculate seasonal rainfall
     ↓
Calculate derivative payoff

5. Weather Derivative Pricing
Two rainfall options are considered.
Rainfall Call Option
The call provides protection against excess rainfall/flood risk.
Call Payoff = min(L, V × max(0, R − K))

Rainfall Put Option
The put provides protection against insufficient rainfall/drought risk.
Put Payoff = min(L, V × max(0, K − R))

Where:
- R = seasonal rainfall
- K = strike rainfall
- V = tick size
- L = maximum liability
6. Monte Carlo Premium
The option premium is calculated as the discounted expected payoff:
Premium = exp(-rT) × E[Payoff]

For Mumbai, the contract assumptions include:
Strike rainfall = ₹2,397.05 mm
Time to maturity = 122 days
Risk-free rate = 5.5%
Tick size = ₹100/mm
Maximum liability = ₹50,000

7. Historical Burn Analysis
Historical Burn Analysis (HBA) is used as a benchmark pricing method.
The historical rainfall observations are used to calculate historical option payoffs, and the average payoff is discounted to obtain the benchmark premium.
HBA is useful as a transparent benchmark but relies heavily on the assumption that historical rainfall is representative of future rainfall.
8. Results
First-Order Markov–Gamma Monte Carlo
Call Premium = ₹12,903.98
Put Premium  = ₹19,417.83

Second-Order Markov–Gamma Monte Carlo
Call Premium = ₹13,468.64
Put Premium  = ₹19,733.51

The second-order Markov–Gamma model provides a more realistic representation of rainfall persistence and variability because it incorporates information from the previous two days.
9. Model Validation
The simulated rainfall seasons are compared with historical rainfall using:
- Seasonal mean rainfall
- Standard deviation
- Coefficient of variation
- Minimum and maximum rainfall
- 95th percentile rainfall
- Wet-day probability
- Dry-day probability
- Upper-tail behaviour
This helps evaluate whether the stochastic model generates realistic rainfall scenarios.
10. Technology Stack
Programming
- Python
- Jupyter Notebook / Google Colab
Python Libraries
- NumPy
- Pandas
- SciPy
- Matplotlib
- Statsmodels
Statistical Methods
- Exploratory Data Analysis
- Markov Chains
- Probability Distributions
- Extreme Value Theory
- Generalized Pareto Distribution
- Monte Carlo Simulation
- Historical Burn Analysis
Project Workflow
CHIRPS Daily Rainfall Data
          ↓
Data Cleaning & Preprocessing
          ↓
Monsoon Season Extraction
          ↓
Exploratory Data Analysis
          ↓
Rainfall Trend Analysis
          ↓
Wet/Dry Classification
          ↓
Markov Chain Modelling
          ↓
Gamma Distribution
          ↓
EVT / POT / GPD
          ↓
Monte Carlo Simulation
          ↓
Synthetic Rainfall Seasons
          ↓
Weather Derivative Payoffs
          ↓
Premium Estimation
          ↓
Model Validation & Comparison
