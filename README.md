# Problem-Set-2-Econ_5200_PS2.ipynb

# Evaluating A/B Test Shipping Rules

## Objective

I evaluated an A/B testing portfolio of 200 experiments to understand how different shipping rules affect false shipments and realised returns.

## Methodology

- Simulated 200 experiments using a prior with a 90% probability of zero effect and a 10% probability of positive effect.
- Modelled positive effects using the absolute value of a normal distribution with mean 0.08 and standard deviation 0.03.
- Examined experiments with sample sizes ranging from 585 to 19,614.
- Evaluated the current shipping rule and an alternative rule by comparing the number of campaigns shipped, false-ship rates, and realised returns.
- Examined estimated versus true effects and compared results across low-, mid-, and high-effect tiers.
- Calculated uncertainty intervals for the total realised return using different interval estimation methods.

## Key Findings

- Of the 200 experiments, 18 had real positive effects.
- Under the original rule, 28 campaigns shipped, and 42.86% were false ships.
- The average true effect was 0.0459, while the average estimated effect was 0.0466, corresponding to an exaggeration ratio of 1.0161.
- The original rule produced a realised return of 1.285 effect units.
- The alternative rule shipped 17 campaigns, reduced the false-ship rate to 23.5%, and produced a realised return of 1.155 effect units.
- The alternative rule reduced false shipments but lowered realised return by 0.130 effect units compared with the original rule.
- Across the low-, mid-, and high-effect tiers, the estimated-to-true effect ratios were 1.302, 0.916, and 1.021, respectively.
- For the original portfolio, the hand-calculated interval was [0.70, 1.99], the BCa interval was [0.80, 2.05], and the SciPy percentile interval was [0.68, 1.94].
