# AI Appendix — Task 4.1

I used ChatGPT with GPT-6 Luna to find the answers.

## Version 1 — Initial Prompt

**Prompt:**

[Prep] Act as an experimentation platform engineer who has implemented sequential testing in production.

[Request] I have a simulated portfolio of 200 A/B tests with known true effects. Implement an always-valid sequential test -- a mixture sequential probability ratio test or an alpha-spending boundary -- that allows continuous monitoring without inflating the Type I error rate. Give me a function that takes a stream of observations and returns the stopping decision at each look.

[Iterate] numpy and scipy only. Reuse my log structure. The function must be testable against a pure-null stream.

[Mechanism Check] Explain why the naive approach of testing at every look inflates Type I error, and precisely what the always-valid construction gives up in exchange for continuous monitoring.

[Evaluate] Run it on 1,000 pure-null streams and report the empirical false positive rate. If it is not close to 5%, tell me what is wrong.

**Complete AI Response:**

Paste the complete first AI response here, including the original explanation and Python code. If the original response is unavailable, label any reconstructed response as reconstructed rather than original.

The coding is below:
import numpy as np
from scipy import stats

def msprt_sequential(stream, alpha=0.05, sigma=1.0, tau=1.0):
    x = np.asarray(stream, dtype=float)
    t = np.arange(1, len(x) + 1, dtype=float)
    S = np.cumsum(x)

    log_bf = (
        0.5 * np.log(sigma**2 / (sigma**2 + t * tau**2))
        + tau**2 * S**2
        / (2 * sigma**2 * (sigma**2 + t * tau**2))
    )

    threshold = np.log(1 / alpha)
    crossed = np.flatnonzero(log_bf >= threshold)

    return {
        "decision": "reject" if crossed.size else "continue",
        "stop_look": int(crossed[0] + 1) if crossed.size else None,
        "log_bf_path": log_bf,
        "threshold": threshold
    }


def evaluate_null(n_streams=1000, n_looks=500, seed=42):
    rng = np.random.default_rng(seed)
    rejections = 0

    for _ in range(n_streams):
        stream = rng.normal(0, 1, size=n_looks)
        result = msprt_sequential(stream)

        if result["decision"] == "reject":
            rejections += 1

    fpr = rejections / n_streams
    mcse = np.sqrt(fpr * (1 - fpr) / n_streams)
    ci = (
        max(0, fpr - 1.96 * mcse),
        min(1, fpr + 1.96 * mcse)
    )

    print(f"False positives: {rejections}/{n_streams}")
    print(f"Empirical FPR: {fpr:.4%}")
    print(f"Monte Carlo SE: {mcse:.4%}")
    print(f"Approximate 95% CI: [{ci[0]:.4%}, {ci[1]:.4%}]")


evaluate_null()

### Version 1 — My Simulation Results

- False positives: 27/1,000
- Empirical FPR: 2.70%
- Monte Carlo standard error: 0.5126 percentage points
- Approximate 95% confidence interval: [1.6954%, 3.7046%]

The observed FPR was below the nominal 5% level. This suggests conservative behavior in this simulation, but it does not by itself prove that the implementation is correct or incorrect.




**Complete AI Response:**

Paste the complete second AI response here, including the code, explanations, and actual output.

