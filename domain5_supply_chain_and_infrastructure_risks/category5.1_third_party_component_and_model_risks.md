# Category 5.1: Third-Party Component and Model Risks

*Alignment: Expands GenAI Domain 5 with container/GPU infrastructure attack vectors from RATC_11 and fleet-level training compromise from RTC.*

**Description:** Externally sourced models, datasets, libraries, and plugins may contain malicious code, hidden backdoors, or poorly documented vulnerabilities that are inherited by any system built on them. The scale of modern AI development makes comprehensive vetting extremely difficult.

## Frameworks

NSA_Dev, NSA_Data, DoD_Cyber, NIST_RMF, NIST_GenAI, MITRE, CDAO, ENISA, GAO

## Key Risks

- 5.1.1 Compromised external AI models with embedded backdoors: Pre-trained models from external sources may contain hidden malicious code or backdoors that persist even after downstream fine-tuning [E] — NSA_Dev, DoD_Cyber, NIST_RMF, MITRE, CDAO
- 5.1.2 Dependency confusion and ML software supply chain compromise: Attackers upload malicious packages to public repositories matching legitimate AI dependency names, causing build systems to install malware [E] — NSA_Dev, MITRE
- 5.1.3 Malicious or compromised tool, plugin, or MCP server: Third-party tools, MCP servers, or plugins contain hidden instructions or unsafe code that executes within the agent's trust boundary [E] — OWASP_Top10, OWASP_T&M, MITRE, Cisco_A2A
- 5.1.4 Compromised agent persona or registry entry: An attacker registers a malicious agent persona in an agent registry, causing other agents to delegate tasks to the attacker-controlled agent [E] — OWASP_Top10, Cisco_A2A
- 5.1.5 Update channel compromise: Malicious updates delivered through legitimate channels replace genuine components with compromised versions [E] — OWASP_Top10, MITRE
- 5.1.6 Foundation model bottleneck risk: Extensive reuse of a small number of foundation models creates single points of failure where errors or biases propagate across many downstream systems [E] — NIST_GenAI, CDAO
- 5.1.7 Lack of software bills of materials (SBOMs) for AI: Absence of inventories of AI system components prevents detection of compromised components and makes vulnerability management difficult [E] — NSA_Dev
- 5.1.8 Benchmark label errors and test dataset contamination: Test datasets used to validate models contain label errors that impact model selection and robustness evaluation [E] — NIST_GenAI
- 5.1.9 Opaque third-party component integration: AI value chains involve many third-party components that may be improperly obtained or inadequately vetted, making behavior attribution difficult [E] — NIST_GenAI, CDAO
