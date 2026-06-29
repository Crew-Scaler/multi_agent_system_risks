# Category 5.3: Asset Lifecycle and Decommissioning Risks

*Alignment: Expands GenAI Domain 5 with container/GPU infrastructure attack vectors from RATC_11 and fleet-level training compromise from RTC.*

**Description:** Sensitive AI assets — model weights, training data, inference logs, and system prompts — can persist and remain accessible beyond their authorized lifespan if lifecycle management and decommissioning are not properly executed.

## Frameworks

NSA_Dev, DoD_Cyber, MITRE, NIST_RMF, NSA_Deploy

## Key Risks

- 5.3.1 Improper sanitization of AI system data on decommission: Failure to fully sanitize or destroy training data, model weights, and associated datasets allows sensitive materials to escape organizational control [E] — DoD_Cyber, NSA_Data
- 5.3.2 Inadequate disposal of model artifacts: AI model files, inference logs, or cached training data surviving decommissioning can be recovered by malicious actors [E] — DoD_Cyber, NSA_Dev
- 5.3.3 Failure to manage AI model lifecycle changes: Adding or updating models to an already-authorized system without reassessing cybersecurity requirements introduces unreviewed risks [E] — DoD_Cyber, NIST_RMF
- 5.3.4 Technical debt accumulation in AI development: Rapid AI development cycles without careful lifecycle management accumulate technical debt creating long-term security risks [E] — NSA_Dev
- 5.3.5 Absence of rollback and disaster recovery: Without tested rollback mechanisms and immutable backups, a compromised or degraded model cannot be quickly restored to a known good state [E] — NSA_Deploy, CDAO
