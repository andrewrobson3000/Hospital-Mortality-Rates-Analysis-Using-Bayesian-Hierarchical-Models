# Hospital-Mortality-Rates-Analysis-Using-Bayesian-Hierarchical-Models

## Overview
This repository presents a detailed analysis of mortality rates across hospitals following infant cardiac surgery. Employing Bayesian hierarchical models, we assess hospital performance, providing a nuanced understanding of mortality rates with an emphasis on statistical rigor and data-driven insights.

## Installation

This analysis requires [PyMC](https://docs.pymc.io/) (or the specific library you're using) for Bayesian inference. You can install it directly using pip:


## Contents
- `code/`: Python notebooks with the complete analysis workflow, from data preprocessing to Bayesian inference.
- `report/`: A detailed report including the methodology, results interpretation, and conclusions. See below

# Report
# Hierarchical Bayesian Analysis of Hospital Mortality Rates

## Introduction

In the realm of medical statistics, understanding and accurately estimating mortality rates across different hospitals plays a crucial role in improving healthcare outcomes. Here, I present a hierarchical Bayesian analysis aimed at estimating hospital-specific mortality rates in infant cardiac surgery. Hierarchical Bayesian models offer a robust framework for such analyses, allowing for the sharing of information across groups while accounting for the unique characteristics of each entity—in this case, individual hospitals.

In this hierarchical structure, the mortality rates $\theta_i$ are conditionally independent given $a$ and $b$, facilitating information sharing across hospitals. This approach enhances the robustness of estimates, particularly for hospitals with smaller sample sizes, by leveraging data from the entire network of hospitals.

## Bayesian Inference Process

The Bayesian inference for this hierarchical model was implemented using probabilistic programming. This process involves specifying the model structure, defining prior distributions, and then using observed data to update beliefs about the unknown parameters, resulting in their posterior distributions.

### Model Specification and Prior Selection

The model was specified with Binomial likelihoods for the observed data and Beta priors for the hospital-specific mortality rates $\theta_i$. A key advantage of the Beta distribution is its conjugacy with the Binomial likelihood, which theoretically simplifies the Bayesian updating process. However, in practice, we often rely on numerical methods to estimate the posterior distributions.

For the hyperparameters $a$ and $b$ of the Beta distribution, we chose non-informative or weakly informative priors to ensure that our estimates are primarily driven by the data rather than strong prior beliefs.

The analysis of mortality rates across different hospitals utilizes a hierarchical model with three layers:

**Level 1 (Data Level):**
- For each hospital $i$, the observed number of mortalities $Y_i$ given the mortality rate $\theta_i$ is modeled as:
$Y_i | \theta_i \sim \text{Bin}(n_i, \theta_i)$
where $n_i$ represents the total number of surgeries at hospital $i$.

**Level 2 (Parameter Level):**
- Each hospital-specific mortality rate $\theta_i$ follows a Beta distribution, conditioned on hyperparameters $a$ and $b$:
$\theta_i | (a, b) \sim \text{Beta}(a, b)$
This level models the prior beliefs about the mortality rates.

**Level 3 (Hyperparameter Level):**
- The hyperparameters $a$ and $b$ have their prior distribution $p(a, b)$:
$(a, b) \sim p(a, b)$
This hyperprior allows for adaptability and learning from the data about the parameters $a$ and $b$.

### Computational Methods for Posterior Estimation

To estimate the posterior distributions of the parameters, I employed Markov Chain Monte Carlo (MCMC) methods, specifically the No-U-Turn Sampler (NUTS), an advanced variant of Hamiltonian Monte Carlo (HMC). MCMC methods are powerful tools for Bayesian inference, particularly in complex models, as they allow us to draw samples from the posterior distribution even when it cannot be computed analytically.

The Python library `PyMC` was used to define the model and perform the inference. The key steps in the computational process included:

1. **Defining the model**: Specifying the likelihood of the data and the priors for all unknown quantities.
2. **Sampling**: Running the NUTS algorithm to generate samples from the posterior distribution of the parameters. This step involves iteratively proposing changes to the parameters and accepting or rejecting these proposals based on how well they explain the observed data.
3. **Diagnostics**: Assessing the quality of the MCMC samples using diagnostics like trace plots and the Gelman-Rubin statistic ($\hat{R}$). These checks ensure that the sampling process has converged to the true posterior distribution.
4. **Posterior Analysis**: Analyzing the samples from the posterior distribution to obtain estimates for the parameters, such as their means, standard deviations, and credible intervals.

### Results Interpretation

The output from the Bayesian inference process provided us with posterior distributions for the mortality rates $\theta_i$ across hospitals. These distributions give us not just point estimates but also a measure of uncertainty about these estimates. For example, the mean of the posterior distribution offers an estimate of the mortality rate, while the 95% Highest Density Interval (HDI) provides a credible interval within which the true mortality rate is likely to lie with 95% probability.

The Bayesian inference process yielded posterior distributions for the mortality rates $\theta_i$ across different hospitals, which encapsulate our updated beliefs about these rates after considering the observed data. These posterior distributions are rich in information, offering more than just single-point estimates. Below, we explain the key components of these results in more detail:

### Posterior Distributions

The posterior distributions of the mortality rates $\theta_i$ reflect the probability of various mortality rates given the observed data and our prior beliefs. These distributions are a fundamental outcome of Bayesian analysis, providing a comprehensive view of all possible mortality rates and their associated probabilities. The shape of these distributions can indicate the certainty of our estimates; a narrower distribution suggests more certainty, whereas a wider distribution indicates less certainty.

### Mean of the Posterior Distribution

The mean of each posterior distribution serves as a point estimate for the hospital-specific mortality rate. For instance, the mean estimate for Hospital A is approximately 0.016, and for Hospital H, it is about 0.143. These means represent the expected mortality rate at each hospital, given the observed data and the prior information. In the context of your analysis, a higher mean indicates a higher estimated mortality rate following infant cardiac surgery at that hospital.

### 95% Highest Density Interval (HDI)

The 95% Highest Density Interval (HDI) provides a range within which the true mortality rate is likely to fall with 95% probability, according to our model. This interval is crucial for understanding the uncertainty around our estimates. A wider interval may indicate that we need more data or that there is inherent variability in the mortality rates that cannot be explained by our model alone.

### Interpretation of Specific Results

The Bayesian analysis results provide insightful details on hospital-specific mortality rates, offering a nuanced understanding of performance across hospitals. By examining the posterior distributions of the mortality rates $\theta_i$ for each hospital, we gain a comprehensive view of the expected mortality rates, along with an understanding of the uncertainty associated with these estimates.

#### Hospital A's Performance
- **Hospital A** is highlighted for its notably low mean mortality rate, approximately $\theta_i = 0.016$. This reflects a very low expected rate of mortality following infant cardiac surgery at this institution. The narrow Highest Density Interval (HDI) of $[0.000, 0.047]$, surrounding this estimate, underscores a high level of confidence in its accuracy. This confidence is supported by the hospital's observed outcomes, with no deaths occurring out of 47 surgeries. The Bayesian framework, through its hierarchical modeling, effectively integrates the observed data with broader trends across hospitals, reinforcing this confidence.

#### Hospital H's Performance
- **Hospital H** presents with a higher mean mortality rate, approximately $\theta_i = 0.143$. This rate suggests a higher risk of mortality associated with surgeries performed at this hospital. The accompanying HDI of $[0.101, 0.188]$ for Hospital H, while indicating precision, reflects the 31 deaths out of 215 surgeries. This HDI signifies a degree of uncertainty that urges careful interpretation of this estimate, highlighting the need for targeted investigations into the factors contributing to these outcomes.

#### Comparative Insights and Implications
- The variability in surgical outcomes across institutions, as illustrated by the performances of Hospitals A and H, underscores the importance of hierarchical Bayesian analysis in identifying outliers and areas for improvement. Hospital A's success, characterized by a significantly low mortality rate and a narrow HDI, sets a benchmark for performance. In contrast, Hospital H's higher mortality rate signals a need for review and potential intervention.

- These insights affirm the value of hierarchical Bayesian methods in analyzing complex healthcare data, emphasizing the model's capacity to provide actionable intelligence. By dissecting the components of variability within and across hospitals, the analysis directs attention to both high-performing institutions and those requiring scrutiny and support.

### Comparative Insights

By examining the posterior means and HDIs across hospitals, we can identify patterns, such as which hospitals have higher or lower mortality rates and how uncertain we are about these estimates. Such comparisons are invaluable for healthcare stakeholders, allowing them to target interventions, allocate resources more effectively, and ultimately improve patient outcomes.

## Conclusion

The hierarchical Bayesian model provides a sophisticated and nuanced method for analyzing hospital-specific mortality rates in infant cardiac surgery. By accurately estimating these rates, healthcare professionals can identify areas for improvement, ultimately leading to better patient outcomes. This report underscores the value of hierarchical Bayesian analysis in complex healthcare data analytics, demonstrating its potential to inform and enhance decision-making processes in medical practice.


