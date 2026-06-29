# Category 5.2: Deployment Environment and Configuration Risks

*Alignment: Expands GenAI Domain 5 with container/GPU infrastructure attack vectors from RATC_11 and fleet-level training compromise from RTC.*

**Description:** Vulnerabilities introduced during the deployment, configuration, and day-to-day operation of AI systems that adversaries or accidents can exploit, including API exposure, misconfiguration, and inadequate access controls on model assets.

## Frameworks

NSA_Deploy, NSA_Dev, DoD_Cyber, MITRE, NIST_RMF, CDAO, ENISA

## Key Risks

- 5.2.1 Misconfigured deployment environment: Organizations deploying AI without sound security baselines expose the AI system to standard IT attack vectors [E] — NSA_Deploy, NSA_Dev, DoD_Cyber, MITRE
- 5.2.2 Exposed or inadequately protected APIs: AI system APIs without proper authentication, authorization, and input validation are entry points for prompt injection, data theft, and unauthorized use [E] — NSA_Deploy, NSA_Dev, MITRE, DoD_Cyber
- 5.2.3 Weak access controls on model weights: Without robust access controls, adversaries can exfiltrate model weights representing the full intellectual value of the AI system [E] — NSA_Deploy, NSA_Dev, DoD_Cyber, MITRE, NIST_RMF
- 5.2.4 Unvalidated imported models run in production: Running pre-trained models without inspection in a secure zone risks deploying models with malicious code or backdoors [E] — NSA_Deploy, DoD_Cyber
- 5.2.5 Insufficient model integrity verification: Deploying models without cryptographic hashes or digital signatures leaves no reliable means to detect tampering [E] — NSA_Deploy, NIST_RMF, MITRE
- 5.2.6 Continued training after deployment without controls: If models continue learning in production without controls, adversaries can poison post-deployment training inputs [E] — DoD_Cyber
- 5.2.7 Physical access to AI hardware: Adversaries with physical access to AI hardware can extract models, tamper with components, or install malicious firmware [E] — DoD_Cyber, MITRE
- 5.2.8 Insecure default settings: Shipping AI systems with insecure default configurations shifts the burden of security hardening onto users lacking the expertise to apply it [E] — NSA_Dev
- 5.2.9 Confidential information leakage through public AI tools: Using public AI tools without private instances risks exposing organizational information entered as prompts [E] — NSA_Deploy, CDAO
