# AI Appendix — Task 4.1

## Version 1 — Initial Prompt

**Prompt:**

[Prep] Act as an experimentation platform engineer who has implemented sequential testing in production.

[Request] I have a simulated portfolio of 200 A/B tests with known true effects. Implement an always-valid sequential test -- a mixture sequential probability ratio test or an alpha-spending boundary -- that allows continuous monitoring without inflating the Type I error rate. Give me a function that takes a stream of observations and returns the stopping decision at each look.

[Iterate] numpy and scipy only. Reuse my log structure. The function must be testable against a pure-null stream.

[Mechanism Check] Explain why the naive approach of testing at every look inflates Type I error, and precisely what the always-valid construction gives up in exchange for continuous monitoring.

[Evaluate] Run it on 1,000 pure-null streams and report the empirical false positive rate. If it is not close to 5%, tell me what is wrong.

**Complete AI Response:**

Paste the complete first AI response here, including the original explanation and Python code. If the original response is unavailable, label any reconstructed response as reconstructed rather than original.

### Version 1 — My Simulation Results

- False positives: 27/1,000
- Empirical FPR: 2.70%
- Monte Carlo standard error: 0.5126 percentage points
- Approximate 95% confidence interval: [1.6954%, 3.7046%]

The observed FPR was below the nominal 5% level. This suggests conservative behavior in this simulation, but it does not by itself prove that the implementation is correct or incorrect.

## Version 2 — Revised Prompt

**Why I revised the prompt:**

I revised the prompt to require a direct comparison between naive repeated testing and the always-valid procedure, along with explicit reporting of the empirical FPR, Monte Carlo standard error, confidence interval, and power under a nonzero effect. This makes the implementation easier to verify.

**Prompt:**

Act as an experimentation platform engineer who has implemented sequential testing in production.

Implement an always-valid sequential test using an mSPRT or alpha-spending boundary, using NumPy and SciPy only.

Please provide a reproducible verification script that:

1. Simulates 1,000 independent pure-null streams using a fixed random seed.
2. Reports the empirical false positive rate, Monte Carlo standard error, and a 95% confidence interval.
3. Compares naive repeated two-sided 5% z-tests with the always-valid test at multiple stream lengths.
4. Evaluates power under a nonzero true effect.
5. Explains the statistical assumptions and investigates unexpected results.

**Complete AI Response:**

Paste the complete second AI response here, including the code, explanations, and actual output.

## Summary of Revisions

The second prompt adds explicit verification requirements and a comparison with naive repeated testing. The goal is to check not only whether the code runs, but also whether its statistical behavior matches the intended error-rate guarantee.
