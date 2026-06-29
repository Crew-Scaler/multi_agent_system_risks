# Category 4.2: Data Drift and Distribution Shift

*Alignment: Expands GenAI Domain 4 with four new categories covering large-context, log-based, streaming, and probabilistic recall leakage vectors unique to MAS architectures (RDL domain, 36 categories).*

**Description:** AI models can degrade over time when the statistical properties of real-world data diverge from their training distribution, causing performance failures that may be gradual and difficult to detect without continuous monitoring.

## Frameworks

NSA_Data, NIST_RMF, GAO, NSA_Deploy, CDAO, ENISA, DoD_Cyber

## Key Risks

- 4.2.1 Natural distribution shift: Gradual changes in the real-world environment cause a deployed model to make increasingly inaccurate predictions [E] — NSA_Data, NIST_RMF, GAO, CDAO
- 4.2.2 Introduction of entirely new data patterns: New phenomena not represented in training data render the model unable to handle inputs it was not trained to recognize [E] — NSA_Data
- 4.2.3 Concept drift: Statistical properties of the operational data environment change over time, causing deployed model performance to degrade without retraining [E] — CDAO, GAO, ENISA
- 4.2.4 Adversarial drift masquerading as natural shift: Abrupt changes in model behavior may indicate an active adversarial attack rather than natural drift, and distinguishing between the two requires continuous monitoring [I] — NSA_Data, DoD_Cyber
- 4.2.5 Dataset staleness and detachment: Datasets used to train AI systems become outdated or disconnected from the deployment context over time [E] — NIST_RMF, GAO
