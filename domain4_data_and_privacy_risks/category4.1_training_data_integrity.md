# Category 4.1: Training Data Integrity

*Alignment: Expands GenAI Domain 4 with four new categories covering large-context, log-based, streaming, and probabilistic recall leakage vectors unique to MAS architectures (RDL domain, 36 categories).*

**Description:** The quality and integrity of training data determines model behavior; corruption, manipulation, or poor representativeness of training data can produce AI systems with systematic errors, embedded biases, or hidden malicious behaviors.

## Frameworks

NSA_Data, NIST_RMF, DoD_Cyber, NSA_Dev, GAO, CDAO, ENISA

## Key Risks

- 4.1.1 Compromised third-party training data: Third-party curated or collected datasets may contain inaccurate or maliciously planted content that corrupts models trained on them [E] — NSA_Data, NIST_RMF, DoD_Cyber, CDAO
- 4.1.2 Split-view data poisoning of web-scale datasets: Attackers purchase expired domains referenced in dataset indexes to replace the referenced data with malicious content [E] — NSA_Data, NIST_RMF
- 4.1.3 Frontrunning data poisoning via crowd-sourced platforms: Malicious edits injected in the window before a scheduled dataset snapshot can poison significant proportions of crowd-sourced data [E] — NSA_Data
- 4.1.4 Missing or manipulated data metadata: Missing, incomplete, or maliciously modified metadata removes contextual information needed to assess data integrity [E] — NSA_Data, GAO, CDAO
- 4.1.5 Unknown or untrusted data provenance: Training data from unknown or untrusted sources cannot be verified for integrity [E] — NSA_Dev, GAO, NIST_RMF, CDAO
- 4.1.6 Data duplication and near-duplication: Duplicate or near-duplicate entries skew model training toward overfit patterns [E] — NSA_Data
- 4.1.7 Training data membership inference: Attacks that infer whether specific records were included in training potentially expose sensitive or classified data [E] — DoD_Cyber, NIST_RMF
- 4.1.8 Training Data Provenance Obfuscation in Multi-Agent Learning Systems: Learning-based models obscure training data provenance—weight parameters don't obviously indicate training sources. Multi-agent systems using aggregated training data hide component sources more effectively than centralized training, enabling attackers to introduce poisoned training data whose provenance cannot be reconstructed after the fact. [NoFW]
  - Nicholas Konz, Charles Godfrey, Madelyn Shapiro, Jonathan Tu, Henry Kvinge, and Davis Brown. 2023. Attributing Learned Concepts in Neural Networks to Training Data. In *ATTRIB Workshop at NeurIPS 2023*. https://arxiv.org/abs/2310.03149
    - "Data attribution methods have proven to be effective for identifying brittle predictions, quantifying train-test leakage, and tracing factual knowledge in language models back to training examples"
  - Sheng-Yu Wang, Aaron Hertzmann, Alexei A. Efros, Jun-Yan Zhu, and Richard Zhang. 2024. Data Attribution for Text-to-Image Models by Unlearning Synthesized Images. In *Proceedings of the 38th Conference on Neural Information Processing Systems (NeurIPS 2024)*. https://doi.org/10.52202/079017-0138
    - no strong evidence found
