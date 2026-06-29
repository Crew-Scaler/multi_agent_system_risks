# Category 2.4: Model Extraction and Privacy Attacks

**Description:** Attackers exploit a model's inference interface to reconstruct proprietary model assets, extract sensitive training data, or infer private information about individuals whose data was used in training. These attacks represent intellectual property theft and privacy violations without requiring direct access to model internals.

## Frameworks

NIST_RMF, DoD_Cyber, NSA_Data, NSA_Deploy, NSA_Dev, MITRE, ENISA

## Key Risks

- 2.4.1 Model extraction / stealing: Attackers submit sufficient queries to reconstruct a functionally equivalent copy of a proprietary model's architecture and parameters [E] — NIST_RMF, DoD_Cyber, NSA_Data, NSA_Deploy, NSA_Dev, MITRE
- 2.4.2 Training data extraction: Attackers prompt a generative model to reproduce verbatim memorized training content including PII, email addresses, and copyrighted text [E] — NIST_RMF, NSA_Data, NSA_Dev, NSA_Deploy
- 2.4.3 Model inversion: Attackers reconstruct individual training records by analyzing model output patterns, potentially exposing sensitive or classified data [E] — NIST_RMF, DoD_Cyber, NSA_Data, NSA_Deploy, MITRE
- 2.4.4 Membership inference: Attackers determine whether a specific individual's data was in the training set, exposing them to stigma or harm [E] — NIST_RMF, NSA_Data, DoD_Cyber
- 2.4.5 Attribute inference: Attackers infer sensitive attributes of individual training records (e.g., health status, financial status) by querying the model [E] — NIST_RMF
- 2.4.6 Property inference: Attackers infer global properties of the training data distribution not intended to be revealed [E] — NIST_RMF
- 2.4.7 Oracle-style adversarial probing: Repeated systematic queries map the model's decision boundaries for downstream exploitation [E] — NSA_Deploy, DoD_Cyber, MITRE
