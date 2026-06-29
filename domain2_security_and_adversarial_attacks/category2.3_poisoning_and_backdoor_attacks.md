# Category 2.3: Poisoning and Backdoor Attacks

**Description:** Attackers corrupt AI model behavior by inserting or modifying training data or model parameters, embedding behaviors that persist into deployment and can be triggered by specific inputs. These attacks are particularly dangerous because they can be introduced before deployment and may be extremely difficult to detect.

## Frameworks

NSA_Data, NIST_RMF, DoD_Cyber, MITRE, NSA_Dev, NSA_Deploy, CDAO, OWASP_MAS, OWASP_T&M, Cisco_A2A, ENISA

## Key Risks

- 2.3.1 Training data poisoning: Attackers inject malicious or corrupt samples into training datasets to cause the model to learn incorrect or harmful behaviors [E] — NSA_Data, NIST_RMF, DoD_Cyber, MITRE, NSA_Dev, CDAO
- 2.3.2 Backdoor / Trojan insertion: Attackers embed hidden trigger patterns in training data so the model misclassifies or produces attacker-specified outputs whenever the trigger appears at inference time [E] — NIST_RMF, DoD_Cyber, MITRE, NSA_Dev
- 2.3.3 Instruction-tuning and RLHF poisoning: Attackers poison data used in post-training alignment stages (supervised fine-tuning, RLHF) to introduce backdoors that survive safety training [E] — NIST_RMF, MITRE
- 2.3.4 Backdoor persistence through safety fine-tuning: Backdoors embedded in pre-trained models have been demonstrated to survive downstream safety training, undermining safety interventions [E] — NIST_RMF
- 2.3.5 Model parameter poisoning (direct): Attackers directly modify trained model parameters to inject malicious functionality without modifying training data [E] — NIST_RMF, DoD_Cyber, MITRE
- 2.3.6 Federated learning poisoning: Compromised clients in federated learning submit malicious local model updates to the aggregating server, poisoning the global model [E] — NIST_RMF
- 2.3.7 Knowledge-base and RAG poisoning: Attackers poison the knowledge database of a RAG system to induce targeted incorrect LLM outputs in response to specific user queries [E] — NIST_RMF, OWASP_MAS, OWASP_Top10, OWASP_T&M, ATFAA
- 2.3.8 Memory poisoning (persistent context corruption): Attackers inject false or malicious data into an agent's persistent memory to corrupt decision-making across future sessions [E] — OWASP_MAS, OWASP_T&M, OWASP_Secure, ATFAA
- 2.3.9 Chaff data spamming: Adversaries flood AI systems with synthetic false-positive data to overwhelm analysts and degrade system performance [E] — MITRE, DoD_Cyber
- 2.3.10 Backpropagation Poisoning in Multi-Agent Shared Value Networks: MCTS backpropagates reward signals updating ancestor nodes. Shared value networks enable attackers poisoning simulations to cause false reward signals. A single malicious simulation backpropagates through all agents sharing the network, creating coordinated value function corruption through the shared backpropagation mechanism. [NoFW]
- 2.3.11 Preference Convergence Attacks via Shared Utility Function Learning: Multi-agent systems where agents learn from shared experience can suffer preference convergence attacks where attackers gradually poison the shared learning data causing all agents to learn the same incorrect utility weights. Over time, all agents converge to malicious utility functions as they individually learn from the poisoned shared corpus. [NoFW]
  - Ezra Edelman, and Surbhi Goel. 2026. Reliable Abstention under Adversarial Injections: Tight Lower Bounds and New Upper Bounds. In *7th Symposium on Foundations of Responsible Computing (FORC 2026)*. https://arxiv.org/abs/2602.20111
    - no strong evidence found
  - Aftab Hussain, Md Rafiqul Islam Rabin, and Mohammad Amin Alipour. 2024. Measuring Impacts of Poisoning on Model Parameters and Embeddings for Large Language Models of Code. In *the 1st ACM International Conference on AI-powered Software (AIware), co-located with the ACM International Conference on the Foundations of Software Engineering (FSE) 2024, Porto de Galinhas, Brazil*. https://doi.org/10.1145/3664646.3664764
    - no strong evidence found
