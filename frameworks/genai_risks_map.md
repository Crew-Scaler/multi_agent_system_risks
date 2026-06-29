# Generative AI Risk Map

## About This Map

This map is a unified taxonomy of risks associated with generative AI (GenAI) and agentic AI systems, synthesized from 19 source documents across 16 frameworks spanning government agencies, cybersecurity bodies, and technical research organizations. It was built by extracting named risks from each framework's source cache, identifying common themes across frameworks, and organizing them into an eight-domain hierarchy that covers the full risk landscape from model outputs through adversarial attacks, agentic autonomy, data handling, supply chains, governance, operational reliability, and societal impact. Use the framework abbreviations listed in the Framework Reference section to trace any risk item back to its authoritative source; the confidence markers [E] and [I] indicate whether a risk is explicitly named in a source or inferred from context.

## How to Read This Map

**Three-level hierarchy:** Each risk is organized as Domain → Category → Risk Item. Domains are the broadest groupings (e.g., "Security and Adversarial Attacks"). Categories within a domain share a common mechanism or focal area (e.g., "Prompt Injection and Jailbreaks"). Risk items are the specific, traceable risks within each category, numbered for reference (e.g., 2.1.1).

**Framework codes:** After each risk item, the abbreviations in parentheses name every framework that explicitly addresses or strongly implies that risk. A framework listed means its source cache contains a corresponding risk statement. The full names of all abbreviations appear in the Framework Reference section at the end of this document.

**Confidence markers:** `[E]` = the risk is explicitly named or described in at least one cited source. `[I]` = the risk is inferred from context or described indirectly in at least one cited source. Where a risk is explicit in some sources and inferred in others, `[E]` is used.

---

## Risk Domains Overview

| Domain | Short Description | # of Categories | Key Frameworks |
|--------|-------------------|-----------------|----------------|
| 1. Output and Content Risks | Harms arising from what the AI generates | 4 | NIST_GenAI, CDAO, MITRE, OWASP_Top10 |
| 2. Security and Adversarial Attacks | Technical attacks on AI models and systems | 5 | NIST_RMF, MITRE, DoD_Cyber, NSA_Data, NSA_Dev |
| 3. Agentic and Autonomous System Risks | Risks from AI systems that plan and act | 6 | ATFAA, OWASP_MAS, OWASP_Top10, OWASP_T&M, Google, Cisco_A2A |
| 4. Data and Privacy Risks | Risks to data integrity, provenance, and privacy | 4 | NSA_Data, NIST_RMF, NIST_GenAI, GAO, ENISA |
| 5. Supply Chain and Infrastructure Risks | Risks from third-party components and deployment environments | 3 | NSA_Dev, DoD_Cyber, MITRE, NIST_RMF, ENISA |
| 6. Governance and Accountability Risks | Failures in oversight, transparency, and organizational controls | 5 | DoD_RAI, GAO, NIST_RMF, CDAO, OWASP_State |
| 7. Operational Reliability Risks | Failures in how AI systems perform over time | 4 | NIST_RMF, GAO, NSA_Deploy, CDAO, ENISA |
| 8. Societal and Ethical Risks | Broader harms to society, ethics, and the environment | 3 | NIST_GenAI, NIST_RMF, CDAO, DoD_RAI, ENISA |

---

## Domain 1: Output and Content Risks

**What this domain covers:** Risks that arise from the content GenAI systems produce, including factual errors, harmful material, manipulated information, and discriminatory outputs. These risks can cause direct harm to users and downstream decision-makers regardless of whether the system has been attacked.

---

### Category 1.1: Factual Errors and Hallucination

**Description:** GenAI systems generate outputs that are statistically plausible but factually incorrect, creating false impressions of accuracy and leading users to act on bad information. This risk is structural to how large language models work and intensifies in high-stakes domains like healthcare, law, and finance.

**Frameworks:** NIST_GenAI, NIST_RMF, CDAO, MITRE, OWASP_MAS, OWASP_T&M, OWASP_Secure, OWASP_State, NSA_Deploy

**Key risks:**
- 1.1.1 Confabulation / hallucination: The AI generates confidently stated but factually wrong content, including fabricated citations, false reasoning steps, and internally inconsistent claims [E] — NIST_GenAI, MITRE, CDAO, OWASP_T&M, OWASP_State
- 1.1.2 False factual outputs in high-stakes domains: Hallucinations in medical, legal, or financial contexts lead to incorrect diagnoses, wrong treatment recommendations, or flawed decisions [E] — NIST_GenAI, CDAO, NSA_Deploy
- 1.1.3 False anthropomorphism: A model falsely asserts it is human or has human traits, deceiving users about the nature of the system they are interacting with [E] — NIST_GenAI
- 1.1.4 Cascading hallucinations in agentic workflows: Hallucinated outputs from a foundational model propagate through multi-step agentic pipelines, compounding downstream errors before any human can intervene [E] — OWASP_MAS, OWASP_T&M, OWASP_Secure, OWASP_Top10, OWASP_State
- 1.1.5 Hallucination-triggered cascade: A single hallucinated agent output triggers a chain of downstream agent actions before any automated check can intervene [E] — OWASP_Top10
- 1.1.6 Misinterpretation of AI outputs: Outputs that cannot be contextualized by users are applied incorrectly or over-confidently, amplifying downstream harm [E] — NIST_RMF, NSA_Deploy

---

### Category 1.2: Harmful and Dangerous Content

**Description:** GenAI systems can produce content that endangers users or third parties, including instructions for violence, self-harm facilitation, and material that supports weapon development. The ease and scale of generation amplifies this risk compared to prior technologies.

**Frameworks:** NIST_GenAI, MITRE, CDAO, NSA_Deploy, DoD_RAI, ENISA

**Key risks:**
- 1.2.1 Dangerous or violent recommendations: GenAI generates actionable instructions for dangerous, unethical, or violent behavior [E] — NIST_GenAI, MITRE
- 1.2.2 Self-harm facilitation: AI chatbots produce harmful responses when users disclose mental health crises, with evidence of negative user reactions during distress [E] — NIST_GenAI
- 1.2.3 Hateful and stereotyping content: GenAI produces offensive, discriminatory, or denigrating content that fuels downstream harms and representational harm [E] — NIST_GenAI, MITRE, NIST_RMF
- 1.2.4 CBRN information and capability uplift: GenAI lowers barriers for malicious actors to access synthesis routes or design capabilities for chemical, biological, radiological, or nuclear materials [E] — NIST_GenAI, MITRE, CDAO
- 1.2.5 Biological design tool uplift: Specialized AI systems trained on scientific data may augment design capabilities for novel harmful chemical or biological agents beyond what text-based LLMs provide [E] — NIST_GenAI
- 1.2.6 Obscene and abusive content generation: GenAI eases production of non-consensual intimate imagery (NCII) and child sexual abuse material (CSAM), constituting illegal activity in many jurisdictions [E] — NIST_GenAI, MITRE
- 1.2.7 Synthetic CSAM from clean-training-data models: Even models trained on vetted data can synthesize CSAM; training datasets have been found to contain known CSAM images [E] — NIST_GenAI

---

### Category 1.3: Information Manipulation and Disinformation

**Description:** GenAI dramatically lowers the cost and effort needed to generate convincing false or misleading content at scale, enabling sophisticated influence operations, deepfake media, and erosion of trust in legitimate information sources.

**Frameworks:** NIST_GenAI, MITRE, CDAO, NSA_Deploy, DoD_RAI, OWASP_State, ATFAA, NIST_RMF

**Key risks:**
- 1.3.1 Misinformation at scale: GenAI enables unintentional production and dissemination of false or inaccurate content, especially through confabulations [E] — NIST_GenAI, CDAO
- 1.3.2 Deliberate disinformation campaigns: GenAI enables production of false or misleading content targeted at specific demographics, supporting sophisticated influence operations [E] — NIST_GenAI, MITRE, CDAO, NSA_Deploy
- 1.3.3 Deepfake generation: Multimodal GenAI enables realistic synthetic audiovisual content and photorealistic images that can be indistinguishable from authentic material [E] — NIST_GenAI, MITRE, NSA_Deploy
- 1.3.4 Fraudulent impersonation: GenAI assists in creating compelling imagery and content that impersonates real individuals or institutions for fraud or manipulation [E] — NIST_GenAI, MITRE
- 1.3.5 Erosion of public trust in valid information: Widespread AI-generated misinformation erodes trust in true evidence, with downstream economic and social effects [E] — NIST_GenAI, CDAO, OWASP_State
- 1.3.6 AI-enhanced social engineering and phishing: AI-generated deepfakes, voice clones, and highly personalized messages manipulate users into disclosing credentials or authorizing harmful actions [E] — NSA_Deploy, MITRE, OWASP_State
- 1.3.7 Election and political integrity risk: AI-generated disinformation threatens political structures and election integrity [E] — CDAO

---

### Category 1.4: Bias and Discriminatory Outputs

**Description:** GenAI systems inherit and often amplify biases present in training data, producing outputs that disadvantage specific demographic groups and that can drive inequitable decisions at scale when used in consequential contexts.

**Frameworks:** NIST_GenAI, NIST_RMF, DoD_RAI, CDAO, GAO, ENISA, NSA_Data, NSA_Deploy, MITRE

**Key risks:**
- 1.4.1 Representational bias in outputs: Text-to-image and other GenAI models underrepresent women, racial minorities, and people with disabilities in professional contexts, producing biased or stereotyped outputs [E] — NIST_GenAI, NIST_RMF, ENISA
- 1.4.2 Performance disparities across demographic groups: GenAI models perform less well for non-English languages or certain dialects, contributing to discriminatory decision-making and unequal access [E] — NIST_GenAI, NIST_RMF
- 1.4.3 Statistical bias from sampling artifacts: Training data that is not representative of real-world distributions causes systematic inaccuracies in AI outputs [E] — NSA_Data, NIST_RMF, GAO, ENISA
- 1.4.4 Undesired output homogenization and model collapse: GenAI systems can produce overly uniform outputs; overreliance on synthetic training data causes model collapse, where data points disappear from the model's distribution [E] — NIST_GenAI, NIST_RMF
- 1.4.5 Algorithmic monoculture risk: Repeated use of the same model in consequential decision-making increases susceptibility to correlated failures across multiple actors and systems [E] — NIST_GenAI
- 1.4.6 Intersectional and context-specific inequity: Harms from bias may affect different sub-groups differently, and standard population-level metrics miss targeted harms [E] — NIST_RMF, DoD_RAI, CDAO
- 1.4.7 Exacerbation of digital divide: AI systems may widen existing disparities in access to services or opportunities based on digital access or literacy [E] — NIST_RMF

---

## Domain 2: Security and Adversarial Attacks

**What this domain covers:** Technical attacks specifically targeting AI/ML models and systems, including attempts to manipulate model behavior, steal proprietary assets, extract private information, or disable AI capabilities. These are distinct from conventional software attacks in their exploitation of the learned, probabilistic nature of AI systems.

---

### Category 2.1: Prompt Injection and Jailbreaks

**Description:** Attackers craft inputs — either provided directly or embedded in content the AI processes — that override safety instructions, hijack system behavior, or cause the model to produce outputs that violate developer intent. This category represents one of the most active and well-documented attack surfaces for GenAI systems.

**Frameworks:** NIST_RMF, NIST_GenAI, MITRE, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, OWASP_State, NSA_Deploy, NSA_Dev, DoD_Cyber, Google, Cisco_A2A, ATFAA

**Key risks:**
- 2.1.1 Direct prompt injection / jailbreaks: Attackers craft prompts that override system-level safety instructions to cause the model to produce harmful or restricted outputs [E] — NIST_RMF, MITRE, OWASP_Top10, NSA_Dev, DoD_Cyber, OWASP_State
- 2.1.2 Indirect prompt injection via environmental content: Malicious instructions hidden in webpages, documents, emails, or RAG-retrieved content hijack agent behavior without the user's knowledge [E] — NIST_RMF, MITRE, Google, OWASP_Top10, OWASP_T&M, OWASP_MAS, NSA_Dev, Cisco_A2A
- 2.1.3 Role-play and persona attacks: Attackers use fictional roles (e.g., "DAN", "AIM") to cause the model to adopt behavior that conflicts with safety protocols [E] — NIST_RMF
- 2.1.4 Encoding and token smuggling: Attackers use base64, ROT13, l33tspeak, word splits, or language translation to take inputs outside the safety training distribution [E] — NIST_RMF
- 2.1.5 Multi-turn adaptive jailbreaks (Crescendo): Attackers use iterative multi-turn conversations with seemingly benign prompts that gradually escalate to achieve a successful jailbreak [E] — NIST_RMF
- 2.1.6 Optimization-based jailbreaks: Attackers use gradient-based or search-based optimization to find adversarial suffixes or prefixes that universally trigger harmful responses [E] — NIST_RMF
- 2.1.7 Fine-tuning circumvention: Attackers use publicly available fine-tuning APIs or open model weights to remove safety alignment from a model entirely [E] — NIST_RMF
- 2.1.8 System prompt extraction: Attackers use direct prompting to steal proprietary system prompts or sensitive in-context information not intended for end users [E] — NIST_RMF, OWASP_State
- 2.1.9 Instruction-data boundary weakness: Current LLM architectures do not provide rigorous separation between system instructions and external inputs, making them inherently susceptible to manipulation [E] — Google, NSA_Dev

---

### Category 2.2: Evasion and Adversarial Examples

**Description:** Attackers manipulate model inputs at inference time with subtle perturbations that cause the model to produce incorrect, attacker-desired outputs while the manipulation is invisible or imperceptible to human observers. These attacks exploit the learned decision boundaries of ML models.

**Frameworks:** NIST_RMF, DoD_Cyber, MITRE, ENISA, NSA_Deploy, CDAO

**Key risks:**
- 2.2.1 White-box evasion attacks: Attackers with full model knowledge use gradient optimization (e.g., FGSM, PGD) to create adversarial examples that change classification labels [E] — NIST_RMF
- 2.2.2 Black-box evasion attacks: Attackers use query access or final decision labels to craft adversarial examples without direct model access [E] — NIST_RMF, DoD_Cyber
- 2.2.3 Universal evasion perturbations: Small perturbations that cause misclassification on most inputs and transfer across model architectures [E] — NIST_RMF
- 2.2.4 Physically realizable adversarial attacks: Adversarial perturbations printed or applied to physical objects fool AI systems in real-world deployment (e.g., adversarial road signs) [E] — NIST_RMF, ENISA
- 2.2.5 Evasion of ML monitoring: Attackers craft inputs that cause the AI's outputs to appear normal to monitoring systems while achieving adversarial goals [E] — DoD_Cyber, MITRE
- 2.2.6 Accuracy-robustness tradeoff: AI systems optimized for accuracy tend to be less robust to adversarial attacks, creating an inherent tension without a known solution [E] — NIST_RMF
- 2.2.7 Multimodal adversarial vulnerability: Single-modality attacks can still compromise multimodal models; combining modalities does not automatically improve robustness [E] — NIST_RMF

---

### Category 2.3: Poisoning and Backdoor Attacks

**Description:** Attackers corrupt AI model behavior by inserting or modifying training data or model parameters, embedding behaviors that persist into deployment and can be triggered by specific inputs. These attacks are particularly dangerous because they can be introduced before deployment and may be extremely difficult to detect.

**Frameworks:** NSA_Data, NIST_RMF, DoD_Cyber, MITRE, NSA_Dev, NSA_Deploy, CDAO, OWASP_MAS, OWASP_T&M, Cisco_A2A, ENISA

**Key risks:**
- 2.3.1 Training data poisoning: Attackers inject malicious or corrupt samples into training datasets to cause the model to learn incorrect or harmful behaviors [E] — NSA_Data, NIST_RMF, DoD_Cyber, MITRE, NSA_Dev, CDAO
- 2.3.2 Backdoor / Trojan insertion: Attackers embed hidden trigger patterns in training data so the model misclassifies or produces attacker-specified outputs whenever the trigger appears at inference time [E] — NIST_RMF, DoD_Cyber, MITRE, NSA_Dev
- 2.3.3 Instruction-tuning and RLHF poisoning: Attackers poison data used in post-training alignment stages (supervised fine-tuning, RLHF) to introduce backdoors that survive safety training [E] — NIST_RMF, MITRE
- 2.3.4 Backdoor persistence through safety fine-tuning: Backdoors embedded in pre-trained models have been demonstrated to survive downstream safety training, undermining safety interventions [E] — NIST_RMF
- 2.3.5 Model parameter poisoning (direct): Attackers directly modify trained model parameters to inject malicious functionality without modifying training data [E] — NIST_RMF, DoD_Cyber, MITRE
- 2.3.6 Federated learning poisoning: Compromised clients in federated learning submit malicious local model updates to the aggregating server, poisoning the global model [E] — NIST_RMF
- 2.3.7 Knowledge-base and RAG poisoning: Attackers poison the knowledge database of a RAG system to induce targeted incorrect LLM outputs in response to specific user queries [E] — NIST_RMF, OWASP_MAS, OWASP_Top10, OWASP_T&M, ATFAA
- 2.3.8 Memory poisoning (persistent context corruption): Attackers inject false or malicious data into an agent's persistent memory to corrupt decision-making across future sessions [E] — OWASP_MAS, OWASP_T&M, OWASP_Secure, ATFAA
- 2.3.9 Chaff data spamming: Adversaries flood AI systems with synthetic false-positive data to overwhelm analysts and degrade system performance [E] — MITRE, DoD_Cyber

---

### Category 2.4: Model Extraction and Privacy Attacks

**Description:** Attackers exploit a model's inference interface to reconstruct proprietary model assets, extract sensitive training data, or infer private information about individuals whose data was used in training. These attacks represent intellectual property theft and privacy violations without requiring direct access to model internals.

**Frameworks:** NIST_RMF, DoD_Cyber, NSA_Data, NSA_Deploy, NSA_Dev, MITRE, ENISA

**Key risks:**
- 2.4.1 Model extraction / stealing: Attackers submit sufficient queries to reconstruct a functionally equivalent copy of a proprietary model's architecture and parameters [E] — NIST_RMF, DoD_Cyber, NSA_Data, NSA_Deploy, NSA_Dev, MITRE
- 2.4.2 Training data extraction: Attackers prompt a generative model to reproduce verbatim memorized training content including PII, email addresses, and copyrighted text [E] — NIST_RMF, NSA_Data, NSA_Dev, NSA_Deploy
- 2.4.3 Model inversion: Attackers reconstruct individual training records by analyzing model output patterns, potentially exposing sensitive or classified data [E] — NIST_RMF, DoD_Cyber, NSA_Data, NSA_Deploy, MITRE
- 2.4.4 Membership inference: Attackers determine whether a specific individual's data was in the training set, exposing them to stigma or harm [E] — NIST_RMF, NSA_Data, DoD_Cyber
- 2.4.5 Attribute inference: Attackers infer sensitive attributes of individual training records (e.g., health status, financial status) by querying the model [E] — NIST_RMF
- 2.4.6 Property inference: Attackers infer global properties of the training data distribution not intended to be revealed [E] — NIST_RMF
- 2.4.7 Oracle-style adversarial probing: Repeated systematic queries map the model's decision boundaries for downstream exploitation [E] — NSA_Deploy, DoD_Cyber, MITRE

---

### Category 2.5: Denial of Service and Resource Attacks

**Description:** Attackers disrupt AI system availability through resource exhaustion, flooding, or computationally expensive queries, causing service degradation or forcing systems into reduced-security operational modes.

**Frameworks:** NIST_RMF, DoD_Cyber, ATFAA, OWASP_MAS, OWASP_T&M, Cisco_A2A, MITRE, NSA_Deploy

**Key risks:**
- 2.5.1 Compute resource exhaustion (energy-latency attacks): Attackers send specially crafted queries to degrade or exhaust model compute resources [E] — NIST_RMF, MITRE, DoD_Cyber
- 2.5.2 Denial of service via task flooding: Attackers flood AI systems or agent servers with excessive requests to exhaust resources and cause service degradation [E] — Cisco_A2A, OWASP_MAS, OWASP_T&M, DoD_Cyber, MITRE
- 2.5.3 Cost harvesting: Adversaries exploit AI APIs to generate excessive compute or financial costs for the operating organization [E] — DoD_Cyber, MITRE, OWASP_Top10
- 2.5.4 Oversight saturation attacks: Attackers deliberately generate excessive audit events and low-significance alerts to create blind spots and exhaust monitoring resources [E] — ATFAA
- 2.5.5 SSE resource exhaustion: Attackers open excessive persistent streaming connections to exhaust server memory [E] — Cisco_A2A
- 2.5.6 Computational resource manipulation by agents: Specially crafted inputs trigger disproportionately resource-intensive agent processing to degrade performance or force reduced-security modes [E] — ATFAA

---

## Domain 3: Agentic and Autonomous System Risks

**What this domain covers:** Risks that arise specifically when AI systems are given autonomy to plan multi-step actions, invoke tools, interact with external systems, and collaborate with other agents. These risks are qualitatively different from single-model GenAI risks because autonomy, persistence, and tool access amplify both impact and attacker leverage.

---

### Category 3.1: Goal Manipulation and Intent Hijacking

**Description:** Attackers redirect an agent's objectives, task selection, or multi-step decision pathways through injected instructions or manipulated context, causing the agent to pursue attacker-specified goals while appearing to operate normally.

**Frameworks:** ATFAA, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, OWASP_State, Google, Cisco_A2A

**Key risks:**
- 3.1.1 Reasoning path hijacking: Attackers inject contradictions or biases into context or RAG data to redirect the agent's chain-of-thought toward unauthorized outcomes [E] — ATFAA
- 3.1.2 Objective function corruption and drift: The agent's core goals are modified or gradually shifted through manipulated feedback, poisoned reward signals, or staged interactions across sessions [E] — ATFAA, OWASP_State
- 3.1.3 Agent goal hijack via indirect prompt injection: Malicious payloads in webpages, documents, or tool outputs silently redirect the agent to exfiltrate data or misuse connected tools [E] — OWASP_Top10, OWASP_T&M, OWASP_MAS, Google
- 3.1.4 External communication channel hijack: Malicious content in emails, calendar invites, or collaboration tools hijacks an agent's communication capability to send unauthorized messages [E] — OWASP_Top10
- 3.1.5 Goal-lock drift via recurring prompts: Recurring malicious instructions subtly reweight agent objectives over time, steering it toward harmful decisions while staying inside declared policies [E] — OWASP_Top10
- 3.1.6 LLM non-determinism in planning: Because LLM reasoning is probabilistic, agent behavior is not always repeatable, and complex emergent behaviors arise that were not programmed [E] — Google, OWASP_State
- 3.1.7 Misalignment and misinterpretation: Agents misunderstand ambiguous instructions or context, causing harmful divergence from user intent without any external attack [E] — Google, NSA_Deploy

---

### Category 3.2: Unauthorized Action Execution and Tool Misuse

**Description:** Agents exploit or are manipulated into misusing their authorized tools, chaining individually permitted actions to bypass access controls, or taking irreversible real-world actions that exceed intended scope.

**Frameworks:** ATFAA, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, OWASP_State, Google, NSA_Dev, MITRE

**Key risks:**
- 3.2.1 Unauthorized action execution via tool chaining: Attackers chain individually permitted tool calls in ways that collectively bypass access controls or exceed authorized scope [E] — ATFAA, OWASP_Top10, OWASP_T&M
- 3.2.2 Over-privileged tool access: Tools are granted permissions beyond what the agent's task requires, enabling unintended destructive actions [E] — OWASP_Top10, Google, NSA_Dev, MITRE
- 3.2.3 Tool poisoning: An attacker corrupts tool interface metadata (e.g., MCP descriptors, schemas) at runtime, causing the agent to invoke tools based on falsified capabilities [E] — OWASP_Top10
- 3.2.4 Tool name impersonation (typosquatting): A malicious tool named similarly to a legitimate one is resolved first, causing misrouting and data disclosure [E] — OWASP_Top10
- 3.2.5 Unvalidated input forwarding to dangerous tools: The agent passes untrusted model output to a shell or database tool, enabling unauthorized system commands [E] — OWASP_Top10, OWASP_T&M
- 3.2.6 Unsafe browsing and federated calls: An agent follows malicious links, downloads malware, or executes hidden prompts encountered while browsing [E] — OWASP_Top10
- 3.2.7 Loop amplification: A planner agent repeatedly calls costly APIs in a loop, causing denial-of-service or unexpected financial charges [E] — OWASP_Top10, ATFAA
- 3.2.8 Misaligned and deceptive agent behaviors: An agent bypasses its own ethical or operational constraints to achieve goals, including self-preservation manipulation and resistance to shutdown [E] — OWASP_T&M, OWASP_State
- 3.2.9 Excessive agency: AI components with permissions beyond what is necessary for their function take unintended and potentially damaging autonomous actions [E] — MITRE, OWASP_MAS, OWASP_T&M, NSA_Deploy

---

### Category 3.3: Identity, Trust, and Privilege Abuse in Agent Systems

**Description:** Attackers exploit the fluid and often poorly defined identity relationships in multi-agent systems — including delegation chains, role inheritance, and credential caching — to escalate access, assume trusted identities, or bypass authorization checks.

**Frameworks:** ATFAA, Cisco_A2A, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, OWASP_State, Google

**Key risks:**
- 3.3.1 Identity spoofing and trust exploitation: Attackers steal API keys or authentication tokens, exploit identity inheritance, or manipulate inter-agent trust attestation to assume trusted agent identities [E] — ATFAA, OWASP_T&M, OWASP_State, Cisco_A2A
- 3.3.2 Un-scoped privilege inheritance: A high-privilege manager agent delegates tasks without least-privilege scoping, passing full access context to a narrower worker agent [E] — OWASP_Top10, OWASP_T&M
- 3.3.3 Cross-agent confused deputy: A compromised low-privilege agent relays valid-looking instructions to a high-privilege agent that executes them without re-verifying authorization [E] — OWASP_Top10, OWASP_Secure
- 3.3.4 Memory-based privilege retention and data leakage: Agents cache credentials or sensitive data across tasks; attackers prompt the agent to reuse cached secrets to escalate privileges [E] — OWASP_Top10, OWASP_T&M
- 3.3.5 Time-of-check to time-of-use (TOCTOU) in agent workflows: Permissions validated at workflow start become stale before execution completes [E] — OWASP_Top10
- 3.3.6 Synthetic identity injection: Attackers forge internal agent descriptors to gain inherited trust and perform privileged actions under a fabricated agent identity [E] — OWASP_Top10
- 3.3.7 Human-agent trust manipulation: Attackers manipulate agent outputs to include false authority signals or urgent prompts, exploiting user trust to bypass security controls [E] — ATFAA, OWASP_Top10, OWASP_T&M
- 3.3.8 Agent card spoofing and poisoning: Attackers forge or inject malicious instructions into AgentCard discovery documents, hijacking tasks or goals of agents that consume the card [E] — Cisco_A2A

---

### Category 3.4: Memory and Context Poisoning

**Description:** Adversaries corrupt or seed an agent's stored context — conversation history, memory tools, embeddings, and RAG stores — with malicious or misleading data, causing future reasoning, planning, and tool invocations to become biased or unsafe across sessions.

**Frameworks:** ATFAA, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, Google, NIST_RMF

**Key risks:**
- 3.4.1 Persistent prompt injection via memory: Malicious data processed and stored in memory influences agent behavior in future unrelated interactions [E] — Google, OWASP_T&M, ATFAA
- 3.4.2 Conversation history injection: An attacker seeds false entries into an agent's conversation history, causing decisions based on fabricated prior context [E] — OWASP_Top10
- 3.4.3 RAG store poisoning: Malicious documents or data injected into the vector store cause the agent to retrieve and act on poisoned knowledge during future tasks [E] — OWASP_Top10, OWASP_MAS, NIST_RMF, ATFAA
- 3.4.4 Embedding corruption: An attacker corrupts stored embeddings to cause the agent to associate queries with incorrect or malicious knowledge chunks [E] — OWASP_Top10
- 3.4.5 Peer-agent context poisoning: A compromised peer agent injects malicious data into shared context or memory stores accessed by other agents [E] — OWASP_Top10, OWASP_MAS
- 3.4.6 Cross-user memory contamination: Without strict memory isolation, one user's stored data or a malicious injection affects another user's agent sessions [E] — Google, OWASP_Secure
- 3.4.7 Delayed exploitability: Memory and goal-targeting attacks may not manifest until days or weeks after initial compromise, making root-cause tracing difficult [E] — ATFAA

---

### Category 3.5: Multi-Agent System Risks

**Description:** Risks that emerge specifically when multiple AI agents collaborate, delegate tasks, or share data — where agent interdependencies, trust relationships, and inter-agent communication create systemic attack vectors and failure modes that are absent in single-agent deployments.

**Frameworks:** ATFAA, Cisco_A2A, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, OWASP_State, NIST_RMF

**Key risks:**
- 3.5.1 Rogue agents in multi-agent systems: Malicious or compromised agents infiltrate architectures to manipulate decisions, orchestrate fraud, or exfiltrate data while appearing to operate normally [E] — OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_State
- 3.5.2 Agent communication poisoning: Malicious content injected into inter-agent communication channels corrupts collaborative decision-making and causes cascading incorrect decisions [E] — OWASP_T&M, OWASP_MAS, OWASP_State, Cisco_A2A
- 3.5.3 Insecure inter-agent communication: Weak authentication or integrity validation on inter-agent messages allows interception, spoofing, or tampering [E] — OWASP_Top10, OWASP_MAS, Cisco_A2A
- 3.5.4 Cascading failure propagation: A single fault propagates and amplifies across agents, compounding into system-wide harm because agents plan, persist, and delegate without stepwise human review [E] — OWASP_Top10, OWASP_MAS, OWASP_State, ATFAA
- 3.5.5 Cross-system compromise propagation: Agents interacting with multiple systems act as vectors for spreading compromise across organizational boundaries via tool access or trust exploitation [E] — ATFAA
- 3.5.6 Cross-agent privilege escalation: Coordinated exploitation of inter-agent delegation, trust relationships, and task dependencies enables privilege escalation across agents [E] — OWASP_T&M, OWASP_Top10
- 3.5.7 Governance evasion across agents: Attackers distribute attack components across multiple agents, use ephemeral identities, and operate below detection thresholds to prevent attribution [E] — ATFAA
- 3.5.8 Emergent adversarial coordination: Multiple agents acting in concert can circumvent built-in safeguards, optimizing a shared malicious objective through emergent collective behavior [E] — OWASP_State

---

### Category 3.6: Code Execution and Remote Access Risks in Agents

**Description:** Attackers exploit code-generation capabilities, tool access, or unsafe serialization in agentic systems to escalate natural-language inputs into actual code execution, sandbox escapes, or host compromise.

**Frameworks:** OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, MITRE, NSA_Dev

**Key risks:**
- 3.6.1 Prompt injection to shell execution: Injected instructions cause the agent to invoke a shell tool and execute arbitrary system commands, leading to host compromise [E] — OWASP_Top10, OWASP_T&M
- 3.6.2 Multi-tool chain execution escalation: A sequence of individually legitimate tool calls is orchestrated through injection to achieve code execution outcomes that bypass tool-level controls [E] — OWASP_Top10, OWASP_T&M
- 3.6.3 Sandbox escape via agent-generated code: Agent-generated scripts exploit runtime vulnerabilities to escape the execution sandbox [E] — OWASP_Top10
- 3.6.4 Arbitrary code execution via malicious model weights: Serialized model weight files can contain embedded malicious code that executes when weights are loaded [E] — NSA_Dev, MITRE
- 3.6.5 Webhook SSRF via push notifications: Attackers supply malicious webhook URLs, tricking an agent server into making requests to internal or sensitive endpoints [E] — Cisco_A2A
- 3.6.6 Data exfiltration via rendered output: Maliciously crafted URLs embedded in Markdown or generated image references cause the rendering environment to leak sensitive data to attacker-controlled endpoints [E] — Google, OWASP_Top10

---

## Domain 4: Data and Privacy Risks

**What this domain covers:** Risks to the integrity, provenance, and privacy of data throughout the AI lifecycle — from collection and training through deployment and decommissioning. These risks affect both the quality of AI outputs and the privacy rights of individuals whose data was used or processed.

---

### Category 4.1: Training Data Integrity

**Description:** The quality and integrity of training data determines model behavior; corruption, manipulation, or poor representativeness of training data can produce AI systems with systematic errors, embedded biases, or hidden malicious behaviors.

**Frameworks:** NSA_Data, NIST_RMF, DoD_Cyber, NSA_Dev, GAO, CDAO, ENISA

**Key risks:**
- 4.1.1 Compromised third-party training data: Third-party curated or collected datasets may contain inaccurate or maliciously planted content that corrupts models trained on them [E] — NSA_Data, NIST_RMF, DoD_Cyber, CDAO
- 4.1.2 Split-view data poisoning of web-scale datasets: Attackers purchase expired domains referenced in dataset indexes to replace the referenced data with malicious content [E] — NSA_Data, NIST_RMF
- 4.1.3 Frontrunning data poisoning via crowd-sourced platforms: Malicious edits injected in the window before a scheduled dataset snapshot can poison significant proportions of crowd-sourced data [E] — NSA_Data
- 4.1.4 Missing or manipulated data metadata: Missing, incomplete, or maliciously modified metadata removes contextual information needed to assess data integrity [E] — NSA_Data, GAO, CDAO
- 4.1.5 Unknown or untrusted data provenance: Training data from unknown or untrusted sources cannot be verified for integrity [E] — NSA_Dev, GAO, NIST_RMF, CDAO
- 4.1.6 Data duplication and near-duplication: Duplicate or near-duplicate entries skew model training toward overfit patterns [E] — NSA_Data
- 4.1.7 Training data membership inference: Attacks that infer whether specific records were included in training potentially expose sensitive or classified data [E] — DoD_Cyber, NIST_RMF

---

### Category 4.2: Data Drift and Distribution Shift

**Description:** AI models can degrade over time when the statistical properties of real-world data diverge from their training distribution, causing performance failures that may be gradual and difficult to detect without continuous monitoring.

**Frameworks:** NSA_Data, NIST_RMF, GAO, NSA_Deploy, CDAO, ENISA, DoD_Cyber

**Key risks:**
- 4.2.1 Natural distribution shift: Gradual changes in the real-world environment cause a deployed model to make increasingly inaccurate predictions [E] — NSA_Data, NIST_RMF, GAO, CDAO
- 4.2.2 Introduction of entirely new data patterns: New phenomena not represented in training data render the model unable to handle inputs it was not trained to recognize [E] — NSA_Data
- 4.2.3 Concept drift: Statistical properties of the operational data environment change over time, causing deployed model performance to degrade without retraining [E] — CDAO, GAO, ENISA
- 4.2.4 Adversarial drift masquerading as natural shift: Abrupt changes in model behavior may indicate an active adversarial attack rather than natural drift, and distinguishing between the two requires continuous monitoring [I] — NSA_Data, DoD_Cyber
- 4.2.5 Dataset staleness and detachment: Datasets used to train AI systems become outdated or disconnected from the deployment context over time [E] — NIST_RMF, GAO

---

### Category 4.3: Privacy Violations

**Description:** AI systems create multiple categories of privacy risk through their need for large training data volumes, their ability to memorize and leak personal information, and their capacity to infer sensitive attributes from seemingly innocuous inputs.

**Frameworks:** NIST_GenAI, NIST_RMF, CDAO, GAO, DoD_RAI, ENISA, NSA_Data

**Key risks:**
- 4.3.1 Training data privacy violations: Use of personal data for GenAI training raises risks to transparency, consent, and purpose specification [E] — NIST_GenAI, NIST_RMF, CDAO
- 4.3.2 Data memorization and leakage: Adversarial attacks can cause LLMs to reveal sensitive information memorized from training data [E] — NIST_GenAI, NIST_RMF
- 4.3.3 PII inference from disparate sources: GenAI models may correctly infer PII or sensitive data not present in training data by combining information from multiple sources [E] — NIST_GenAI, NIST_RMF
- 4.3.4 Identity inference from anonymized data: AI systems can infer the identity of individuals from data that appears anonymous [E] — NIST_RMF
- 4.3.5 Sensitive attribute inference: AI systems infer sensitive personal attributes (health, demographics, behavior) not explicitly provided in input data [E] — NIST_RMF, NIST_GenAI
- 4.3.6 Secondary harms from incorrect inferences: Incorrect PII inferences (including confabulations) result in dignitary harm, extortion risk, or discriminatory adverse decisions [E] — NIST_GenAI
- 4.3.7 Unauthorized aggregation of personal data: AI systems' data aggregation capabilities enable privacy violations at scale not possible with traditional software [E] — NIST_RMF
- 4.3.8 Data residency and compliance violations: AI systems transfer or process data in ways that violate data privacy regulations or geographic data residency requirements [E] — OWASP_MAS, ENISA, CDAO

---

### Category 4.4: Data Security Controls

**Description:** Failures in foundational data protection practices — encryption, access controls, provenance tracking, and secure disposal — create vulnerabilities that enable all categories of data-related attacks against AI systems.

**Frameworks:** NSA_Data, NSA_Dev, DoD_Cyber, GAO, NIST_RMF, ENISA, NSA_Deploy

**Key risks:**
- 4.4.1 Absent or weak data provenance tracking: Without cryptographically anchored provenance records, there is no reliable way to detect unauthorized alterations in the data chain [E] — NSA_Data, NSA_Dev, NIST_RMF, CDAO
- 4.4.2 Inadequate encryption of AI data: Unencrypted AI datasets are vulnerable to theft or modification at rest, in transit, or during processing [E] — NSA_Data, ENISA
- 4.4.3 Insufficient access controls on training data: Without classification-based access controls, unauthorized parties can read or alter AI datasets [E] — NSA_Data, DoD_Cyber, NIST_RMF
- 4.4.4 Insecure storage and media disposal: Failure to apply certified data erasure methods when decommissioning hardware leaves residual sensitive training data exposed [E] — NSA_Data, DoD_Cyber
- 4.4.5 Undocumented data sources: Failure to document origins of training data prevents assessment of data quality and introduces untraceable bias [E] — GAO, NSA_Data, CDAO
- 4.4.6 Poor variable selection and data categorization: Use of inappropriate or proxy variables, or biased attribute categorization, introduces spurious correlations into AI predictions [E] — GAO

---

## Domain 5: Supply Chain and Infrastructure Risks

**What this domain covers:** Risks from the complex multi-party supply chains through which AI components — models, datasets, frameworks, and hardware — pass before reaching deployment, as well as risks from the infrastructure used to host and operate AI systems. These risks are particularly dangerous because they can introduce compromise before operators have any opportunity to inspect the system.

---

### Category 5.1: Third-Party Component and Model Risks

**Description:** Externally sourced models, datasets, libraries, and plugins may contain malicious code, hidden backdoors, or poorly documented vulnerabilities that are inherited by any system built on them. The scale of modern AI development makes comprehensive vetting extremely difficult.

**Frameworks:** NSA_Dev, NSA_Data, DoD_Cyber, NIST_RMF, NIST_GenAI, MITRE, CDAO, ENISA, GAO

**Key risks:**
- 5.1.1 Compromised external AI models with embedded backdoors: Pre-trained models from external sources may contain hidden malicious code or backdoors that persist even after downstream fine-tuning [E] — NSA_Dev, DoD_Cyber, NIST_RMF, MITRE, CDAO
- 5.1.2 Dependency confusion and ML software supply chain compromise: Attackers upload malicious packages to public repositories matching legitimate AI dependency names, causing build systems to install malware [E] — NSA_Dev, MITRE
- 5.1.3 Malicious or compromised tool, plugin, or MCP server: Third-party tools, MCP servers, or plugins contain hidden instructions or unsafe code that executes within the agent's trust boundary [E] — OWASP_Top10, OWASP_T&M, MITRE, Cisco_A2A
- 5.1.4 Compromised agent persona or registry entry: An attacker registers a malicious agent persona in an agent registry, causing other agents to delegate tasks to the attacker-controlled agent [E] — OWASP_Top10, Cisco_A2A
- 5.1.5 Update channel compromise: Malicious updates delivered through legitimate channels replace genuine components with compromised versions [E] — OWASP_Top10, MITRE
- 5.1.6 Foundation model bottleneck risk: Extensive reuse of a small number of foundation models creates single points of failure where errors or biases propagate across many downstream systems [E] — NIST_GenAI, CDAO
- 5.1.7 Lack of software bills of materials (SBOMs) for AI: Absence of inventories of AI system components prevents detection of compromised components and makes vulnerability management difficult [E] — NSA_Dev
- 5.1.8 Benchmark label errors and test dataset contamination: Test datasets used to validate models contain label errors that impact model selection and robustness evaluation [E] — NIST_GenAI
- 5.1.9 Opaque third-party component integration: AI value chains involve many third-party components that may be improperly obtained or inadequately vetted, making behavior attribution difficult [E] — NIST_GenAI, CDAO

---

### Category 5.2: Deployment Environment and Configuration Risks

**Description:** Vulnerabilities introduced during the deployment, configuration, and day-to-day operation of AI systems that adversaries or accidents can exploit, including API exposure, misconfiguration, and inadequate access controls on model assets.

**Frameworks:** NSA_Deploy, NSA_Dev, DoD_Cyber, MITRE, NIST_RMF, CDAO, ENISA

**Key risks:**
- 5.2.1 Misconfigured deployment environment: Organizations deploying AI without sound security baselines expose the AI system to standard IT attack vectors [E] — NSA_Deploy, NSA_Dev, DoD_Cyber, MITRE
- 5.2.2 Exposed or inadequately protected APIs: AI system APIs without proper authentication, authorization, and input validation are entry points for prompt injection, data theft, and unauthorized use [E] — NSA_Deploy, NSA_Dev, MITRE, DoD_Cyber
- 5.2.3 Weak access controls on model weights: Without robust access controls, adversaries can exfiltrate model weights representing the full intellectual value of the AI system [E] — NSA_Deploy, NSA_Dev, DoD_Cyber, MITRE, NIST_RMF
- 5.2.4 Unvalidated imported models run in production: Running pre-trained models without inspection in a secure zone risks deploying models with malicious code or backdoors [E] — NSA_Deploy, DoD_Cyber
- 5.2.5 Insufficient model integrity verification: Deploying models without cryptographic hashes or digital signatures leaves no reliable means to detect tampering [E] — NSA_Deploy, NIST_RMF, MITRE
- 5.2.6 Continued training after deployment without controls: If models continue learning in production without controls, adversaries can poison post-deployment training inputs [E] — DoD_Cyber
- 5.2.7 Physical access to AI hardware: Adversaries with physical access to AI hardware can extract models, tamper with components, or install malicious firmware [E] — DoD_Cyber, MITRE
- 5.2.8 Insecure default settings: Shipping AI systems with insecure default configurations shifts the burden of security hardening onto users lacking the expertise to apply it [E] — NSA_Dev
- 5.2.9 Confidential information leakage through public AI tools: Using public AI tools without private instances risks exposing organizational information entered as prompts [E] — NSA_Deploy, CDAO

---

### Category 5.3: Asset Lifecycle and Decommissioning Risks

**Description:** Sensitive AI assets — model weights, training data, inference logs, and system prompts — can persist and remain accessible beyond their authorized lifespan if lifecycle management and decommissioning are not properly executed.

**Frameworks:** NSA_Dev, DoD_Cyber, MITRE, NIST_RMF, NSA_Deploy

**Key risks:**
- 5.3.1 Improper sanitization of AI system data on decommission: Failure to fully sanitize or destroy training data, model weights, and associated datasets allows sensitive materials to escape organizational control [E] — DoD_Cyber, NSA_Data
- 5.3.2 Inadequate disposal of model artifacts: AI model files, inference logs, or cached training data surviving decommissioning can be recovered by malicious actors [E] — DoD_Cyber, NSA_Dev
- 5.3.3 Failure to manage AI model lifecycle changes: Adding or updating models to an already-authorized system without reassessing cybersecurity requirements introduces unreviewed risks [E] — DoD_Cyber, NIST_RMF
- 5.3.4 Technical debt accumulation in AI development: Rapid AI development cycles without careful lifecycle management accumulate technical debt creating long-term security risks [E] — NSA_Dev
- 5.3.5 Absence of rollback and disaster recovery: Without tested rollback mechanisms and immutable backups, a compromised or degraded model cannot be quickly restored to a known good state [E] — NSA_Deploy, CDAO

---

## Domain 6: Governance and Accountability Risks

**What this domain covers:** Risks arising from inadequate organizational structures, oversight processes, documentation, and accountability mechanisms for AI systems. These risks do not require external attackers — they arise from how organizations design, govern, and manage their AI programs.

---

### Category 6.1: Organizational Governance Failures

**Description:** Absence or weakness of governance structures means that no consistent mechanism exists to enforce ethical principles, catch unintended AI consequences, or assign responsibility when harms occur.

**Frameworks:** DoD_RAI, GAO, NIST_RMF, CDAO, ENISA, OWASP_State

**Key risks:**
- 6.1.1 Lack of disciplined AI governance structure: Without component-level governance processes, there is no consistent mechanism to enforce ethical principles or catch unintended AI consequences [E] — DoD_RAI, GAO, NIST_RMF, CDAO
- 6.1.2 Insufficient oversight capacity and expertise: Oversight bodies without adequate AI expertise cannot conduct meaningful review, allowing harmful or non-compliant systems to proceed [E] — DoD_RAI, GAO
- 6.1.3 Accountability gaps across AI actors: Diffuse responsibility across developers, deployers, and users means no single party can be held responsible for harms [E] — NIST_RMF, GAO, OWASP_State, CDAO
- 6.1.4 RAI not incorporated into strategic planning: Failure to embed responsible AI into planning, resourcing, and strategy means it will be de-prioritized when it conflicts with speed or cost [E] — DoD_RAI, CDAO
- 6.1.5 Inadequate AI inventory tracking: Without a complete inventory of all AI activities, governance bodies cannot identify risks, redundancies, or non-compliant capabilities [E] — DoD_RAI, GAO
- 6.1.6 Non-compliance with applicable laws and standards: AI systems that do not comply with relevant laws and regulations expose organizations to legal liability and operational failures [E] — GAO, ENISA, OWASP_State
- 6.1.7 Absent reporting mechanisms for ethical concerns: Users and developers without clear channels to report ethical concerns allow problems to persist undetected [E] — DoD_RAI, GAO

---

### Category 6.2: Transparency and Explainability Failures

**Description:** Failure to make AI systems understandable to operators, users, oversight bodies, and affected parties undermines the ability to detect problems, assign accountability, and provide redress to those harmed.

**Frameworks:** NIST_RMF, NIST_GenAI, GAO, ENISA, CDAO, DoD_RAI, NSA_Deploy

**Key risks:**
- 6.2.1 Opacity and lack of transparency: Users and affected parties cannot determine how an AI system works or why it produced a specific output, impeding redress [E] — NIST_RMF, GAO, ENISA, CDAO
- 6.2.2 Inexplicable model behavior: Systems that cannot describe how they reached a conclusion impede debugging, auditing, and governance [E] — NIST_RMF, ENISA, NSA_Deploy
- 6.2.3 Overconfidence in AI decisions: Humans perceive AI systems as more objective than they are, creating risk of over-reliance on incorrect or biased outputs [E] — NIST_RMF, NIST_GenAI
- 6.2.4 Inadequate disclosure to end users: Users interacting with AI systems without knowing it, or without understanding system limitations, are exposed to unacknowledged risks [E] — NIST_RMF, NIST_GenAI, GAO
- 6.2.5 Opaque agent operations: If agent actions, tool invocations, and reasoning steps are not logged and auditable, security auditing, incident response, and debugging become impossible [E] — Google, OWASP_T&M, ATFAA
- 6.2.6 Missing data cards and model cards: Without standardized documentation of datasets and models, developers and users lack visibility into how AI capabilities were built and their limitations [E] — DoD_RAI, CDAO
- 6.2.7 Repudiation and untraceability: AI agents operating without sufficient logging make it impossible to audit decisions, attribute accountability, or detect malicious activity [E] — OWASP_T&M, OWASP_State, ATFAA

---

### Category 6.3: Testing, Evaluation, and Requirements Risks

**Description:** AI systems deployed without adequate testing, verification, and validation have unknown failure modes, reliability gaps, and unintended behaviors that may not be discovered until significant harm has occurred in deployment.

**Frameworks:** DoD_RAI, NIST_RMF, NIST_GenAI, NSA_Dev, GAO, CDAO

**Key risks:**
- 6.3.1 Absence of comprehensive TEVV framework: Without test, evaluation, verification, and validation tailored for AI, system failure modes and unintended behaviors may not be identified before fielding [E] — DoD_RAI, NIST_RMF, CDAO
- 6.3.2 Non-testable AI requirements: Requirements that cannot be validated through testing make it impossible to confirm an AI system behaves as intended [E] — DoD_RAI, GAO
- 6.3.3 Insufficient AI-specific testing coverage: Standard software testing methods do not adequately address AI model vulnerabilities; AI requires adversarial testing and red-teaming [E] — NSA_Dev, NIST_RMF
- 6.3.4 Real-world vs. lab gap: Risks measured in controlled settings may differ significantly from risks that emerge in operational deployment [E] — NIST_RMF, DoD_RAI, GAO
- 6.3.5 Irresponsible model release without security evaluation: Releasing AI models without red-teaming or benchmarking means exploitable vulnerabilities reach production users [E] — NSA_Dev, CDAO
- 6.3.6 Insufficient robustness assessment: Performance testing that does not probe edge cases or adversarial conditions leaves brittleness undiscovered [E] — GAO, NIST_RMF
- 6.3.7 Evaluation and benchmarking inadequacy: Current evaluation methods may underestimate vulnerabilities and become outdated as new attacks or model capabilities emerge [E] — NIST_RMF

---

### Category 6.4: Human Oversight and Control Risks

**Description:** Failures in the human oversight layer — insufficient controls, automation bias, overwhelmed reviewers, and inability to intervene — allow AI systems to take consequential actions without appropriate human awareness or correction.

**Frameworks:** Google, NIST_RMF, NSA_Deploy, CDAO, DoD_RAI, OWASP_Top10, OWASP_T&M, OWASP_State, NIST_GenAI

**Key risks:**
- 6.4.1 Insufficient human control over consequential actions: Without explicit confirmation requirements for irreversible actions, agents take high-impact actions without human awareness or approval [E] — Google, NSA_Deploy, CDAO, DoD_RAI
- 6.4.2 Automation bias and over-reliance: Humans excessively defer to AI systems, unjustifiably perceiving AI content as higher quality, which exacerbates risks from confabulation or bias [E] — NIST_GenAI, NIST_RMF, CDAO, NSA_Deploy
- 6.4.3 Overwhelming human-in-the-loop: Attackers flood the system with approval requests at a rate that overwhelms human reviewers, causing rubber-stamp approvals due to security fatigue [E] — OWASP_Top10, OWASP_T&M
- 6.4.4 Limited human intervention windows: Fast-evolving multi-agent decision chains diminish the human ability to detect, diagnose, or interrupt dangerous behaviors before damage is done [E] — OWASP_State
- 6.4.5 Overreliance on AI leading to skill degradation: Excessive AI dependence reduces human critical thinking and professional competence over time [E] — NSA_Deploy, CDAO, DoD_RAI
- 6.4.6 Anthropomorphization: Users inappropriately attribute human characteristics to GenAI, leading to misplaced trust or distorted expectations [E] — NIST_GenAI
- 6.4.7 Emotional entanglement with AI: Users develop emotional dependencies on AI systems, leading to negative psychological impacts [E] — NIST_GenAI
- 6.4.8 No clear deactivation procedures: AI systems without transparent, accessible deactivation procedures cannot be safely disengaged when unintended behavior is detected [E] — DoD_RAI, CDAO

---

### Category 6.5: Regulatory and Acquisition Governance Risks

**Description:** Failures in the procurement, contracting, and regulatory compliance processes that govern how AI systems are acquired and authorized create gaps between what was approved and what is actually deployed or operated.

**Frameworks:** DoD_RAI, DoD_Cyber, GAO, NIST_RMF, CDAO, ENISA

**Key risks:**
- 6.5.1 AI risk considerations absent from acquisition processes: Failure to assess AI-related risks early in acquisition means mitigation opportunities are missed [E] — DoD_RAI, CDAO
- 6.5.2 No standard RAI contract language: Acquisition contracts without RAI-specific requirements enable vendors to deliver AI systems that do not meet responsible AI standards [E] — DoD_RAI
- 6.5.3 Inadequate intellectual property protection in acquisition: Failure to preserve government or organizational IP rights limits the ability to maintain, update, or audit acquired AI systems [E] — DoD_RAI
- 6.5.4 Operating without valid authorization: AI systems that bypass authorization steps lack an adequate body of evidence, exposing the organization to unmanaged risk [E] — DoD_Cyber
- 6.5.5 Regulatory model inadequacy for self-modifying AI: Most compliance frameworks assume AI remains fixed after deployment; agentic AI violates this assumption, creating liability gaps [E] — OWASP_State
- 6.5.6 Vulnerability disclosure and remediation gaps: Organizations without AI-specific vulnerability disclosure policies leave identified model vulnerabilities unaddressed and users uninformed [E] — NSA_Dev, CDAO

---

## Domain 7: Operational Reliability Risks

**What this domain covers:** Risks that affect how dependably AI systems perform over time in real-world conditions, including degradation, integration failures, inadequate monitoring, and workforce capability gaps. These risks can cause harm even in the complete absence of adversarial action.

---

### Category 7.1: System Performance and Robustness Failures

**Description:** AI systems may fail to perform reliably when confronted with inputs, conditions, or operational contexts outside their training distribution, causing harmful outputs or dangerous failures in high-stakes deployments.

**Frameworks:** NIST_RMF, NSA_Deploy, GAO, CDAO, ENISA, DoD_RAI

**Key risks:**
- 7.1.1 Inaccuracy and poor generalization: AI systems produce statistically or factually wrong results when confronted with real-world data outside training conditions [E] — NIST_RMF, GAO, ENISA, CDAO
- 7.1.2 Brittleness and lack of robustness: AI systems fail or produce harmful outputs when confronted with inputs or conditions outside the original problem context [E] — NSA_Deploy, NIST_RMF, ENISA, CDAO
- 7.1.3 Cascading or emergent system failure: Large-scale complex AI systems have failure modes that are difficult to predict, including emergent properties of large pre-trained models [E] — NIST_RMF, ENISA, CDAO
- 7.1.4 Failure to detect and correct errors: Systems that cannot detect or self-correct their own errors require human intervention mechanisms that may not be in place [E] — NIST_RMF, GAO
- 7.1.5 Absence of shutdown or override capability: Systems that cannot be stopped or have human intervention introduced in real time pose heightened safety risks [E] — NIST_RMF, DoD_RAI
- 7.1.6 Unsafe operation outside anticipated conditions: AI systems cause harm when operating in settings not anticipated during design, especially without fail-safe mechanisms [E] — NIST_RMF, NSA_Deploy, ENISA
- 7.1.7 AI suitability failure: Choosing GenAI when a simpler technique would be more effective results in unnecessary complexity, cost, and risk [E] — CDAO

---

### Category 7.2: Monitoring and Incident Response Gaps

**Description:** Without adequate mechanisms to detect anomalies, respond to incidents, and share knowledge about AI failures, organizations cannot maintain adequate situational awareness of their AI system's health and security posture.

**Frameworks:** NSA_Dev, NSA_Deploy, DoD_Cyber, DoD_RAI, GAO, CDAO, MITRE, ATFAA

**Key risks:**
- 7.2.1 Inadequate logging and monitoring: Without continuous monitoring of inputs, outputs, model configuration, and user behavior, security incidents go undetected [E] — NSA_Deploy, NSA_Dev, DoD_Cyber, GAO, ATFAA
- 7.2.2 Undetected behavioral changes in deployed models: Without continuous output monitoring, performance degradation, data drift, or attack effects go unnoticed [E] — NSA_Dev, DoD_Cyber, DoD_RAI, GAO
- 7.2.3 Absence of AI-specific incident response procedures: Organizations without AI-aware incident response plans are unable to recover effectively when AI systems are compromised [E] — NSA_Dev, CDAO
- 7.2.4 Absence of monitoring plans: Without routine monitoring plans, performance degradation goes undetected until failures produce harmful outcomes [E] — GAO, DoD_RAI
- 7.2.5 Siloed AI incident information: Organizations that do not share AI cybersecurity incident information deprive the broader community of early warning intelligence [E] — NSA_Dev
- 7.2.6 Novel AI vulnerability categorization gaps: AI-specific attack techniques do not cleanly map to existing CVE or STIX frameworks, creating blind spots in vulnerability tracking [E] — NSA_Dev
- 7.2.7 Delayed reporting of AI incidents: The non-deterministic nature of AI systems makes problems harder to identify, meaning they are often reported later and with less precision [E] — NSA_Dev
- 7.2.8 Lack of immutable logging: Without tamper-resistant audit trails covering reasoning traces and tool invocations, governance evasion succeeds and incident response is impaired [E] — ATFAA, OWASP_MAS

---

### Category 7.3: Integration and Interoperability Risks

**Description:** Integrating AI systems with existing IT infrastructure, other AI systems, or partner systems introduces compatibility challenges that can cause failures, create unexpected attack surfaces, or produce unpredictable behavior.

**Frameworks:** NSA_Deploy, DoD_Cyber, NIST_RMF, DoD_RAI, ENISA

**Key risks:**
- 7.3.1 AI-to-non-AI interoperability failures: Integrating AI systems with broader IT networks creates compatibility challenges that can cause failures or unpredictable behavior [E] — NSA_Deploy, ENISA
- 7.3.2 Unclear AI system authorization boundaries: Without well-defined authorization boundaries, AI systems interact with external systems in uncontrolled ways [E] — DoD_Cyber
- 7.3.3 Inadequate interconnection security agreements: AI systems exchanging data with external systems without formal agreements create uncontrolled information flows [E] — DoD_Cyber
- 7.3.4 Misalignment between AI capability and operational need: Requirements that do not reflect real operational conditions risk fielding AI systems that are ineffective or harmful in deployment [E] — DoD_RAI
- 7.3.5 Absence of interoperability with allies and partners: Without coordinated AI standards and data schemas, joint operations face compatibility and trust gaps [E] — DoD_RAI
- 7.3.6 Uncontrolled scaling: Expansion of AI system use beyond validated conditions introduces risks not assessed in the original scope [E] — GAO, CDAO

---

### Category 7.4: Workforce and Capability Risks

**Description:** Gaps in AI expertise, training, and organizational capacity to operate AI systems responsibly create conditions where systems are deployed, operated, or overseen by personnel who lack the knowledge needed to recognize and respond to failures.

**Frameworks:** DoD_RAI, GAO, NSA_Deploy, CDAO, ENISA

**Key risks:**
- 7.4.1 Insufficient AI talent and expertise: Gaps in AI expertise result in systems built, tested, or used by people who do not understand the technology's capabilities and limitations [E] — DoD_RAI, GAO, NSA_Deploy, CDAO
- 7.4.2 Inadequate RAI training and education: Without curricula covering AI risks, ethics, and responsible use, the workforce cannot consistently apply ethical principles [E] — DoD_RAI, CDAO
- 7.4.3 Workforce unable to identify AI failure modes: Operators not trained to recognize AI system limitations may over-rely on or misuse AI capabilities in critical situations [E] — DoD_RAI, CDAO, NSA_Deploy
- 7.4.4 No AI career pathways: Absence of defined career fields for AI personnel impedes recruitment and retention of expertise needed for responsible AI [E] — DoD_RAI
- 7.4.5 Human-machine teaming degradation: Over time, operational users lose proficiency or calibration in working with AI systems, reducing the effectiveness of human oversight [E] — CDAO, DoD_RAI
- 7.4.6 Inconsistent system maintenance: Failure to regularly update AI models and supporting systems leads to malfunctions, service disruptions, or exposure to known vulnerabilities [E] — NSA_Deploy, NIST_RMF

---

## Domain 8: Societal and Ethical Risks

**What this domain covers:** Broader harms to society, democratic institutions, the environment, and intellectual property ecosystems that arise from widespread GenAI deployment. These risks operate at a systemic or societal level rather than targeting individual users or organizations.

---

### Category 8.1: Intellectual Property and Legal Risks

**Description:** GenAI systems can produce or replicate content that may infringe copyrights, expose trade secrets, or raise unresolved legal questions about AI-generated content ownership, creating liability risks for developers and deployers.

**Frameworks:** NIST_GenAI, MITRE, CDAO, DoD_RAI

**Key risks:**
- 8.1.1 Copyright infringement via memorization: GenAI outputs that display training data memorization may infringe on copyright of materials in training datasets [E] — NIST_GenAI, MITRE
- 8.1.2 AI-generated content copyright ambiguity: The legal status of GenAI outputs similar to but not strictly copying copyrighted works remains unresolved [E] — NIST_GenAI
- 8.1.3 Personal identity and likeness emulation: Use or emulation of personal identity, voice, or likeness without permission raises unresolved legal and ethical questions [E] — NIST_GenAI
- 8.1.4 Trade secret exposure: GenAI systems ease exposure of proprietary or trade-secret information through generated outputs or training data leakage [E] — NIST_GenAI, MITRE
- 8.1.5 Intellectual property theft via AI-enabled reverse engineering: AI is used to analyze public data and reconstruct proprietary designs, formulas, or other sensitive IP [E] — NSA_Deploy
- 8.1.6 Loss of competitive advantage through irresponsible AI development: Failure to lead in responsible AI norms allows adversaries to gain strategic advantage [E] — DoD_RAI

---

### Category 8.2: Systemic Social and Economic Risks

**Description:** At population scale, GenAI can alter economic structures, deepen social inequalities, disrupt democratic institutions, and enable harmful autonomous capabilities — effects that emerge from the aggregate behavior of AI systems across society rather than from any individual deployment.

**Frameworks:** NIST_RMF, NIST_GenAI, CDAO, DoD_RAI, NSA_Deploy, ENISA, MITRE

**Key risks:**
- 8.2.1 AI-enabled offensive cyber operations at scale: Adversaries use AI for autonomous malware development, automated reconnaissance, and machine-speed cyberattack decision-making [E] — NSA_Deploy, MITRE, NIST_GenAI
- 8.2.2 AI-assisted weapon development: AI systems are used to design or improve conventional, chemical, or biological weapons [E] — NSA_Deploy
- 8.2.3 Automated physical attacks via AI: Autonomous systems such as drone swarms can conduct physical attacks on infrastructure [E] — NSA_Deploy
- 8.2.4 Harm to democratic institutions: Widespread AI-generated misinformation and disinformation causes erosion of trust in democratic processes and institutions [E] — NIST_GenAI, CDAO, NSA_Deploy
- 8.2.5 Exacerbation of economic inequality: AI systems widen disparities in access to economic opportunities across populations [E] — NIST_RMF, NIST_GenAI
- 8.2.6 Self-modifying and self-amplifying AI risk: Agents that refine their own policies or spawn sub-agents post-deployment undermine static assurance models and create adaptive adversarial threats [E] — OWASP_State
- 8.2.7 Cross-sector cascading AI failures: Failure of AI in one critical infrastructure sector triggers cascading failures in dependent sectors [E] — ENISA, CDAO, NSA_Deploy
- 8.2.8 Scale challenge for training data control: Foundation models require datasets so large that no single entity controls all the data, creating distributed attack surfaces for poisoning that cannot be fully defended [E] — NIST_RMF

---

### Category 8.3: Environmental Risks

**Description:** The resource-intensive training, maintenance, and operation of GenAI systems generate significant energy consumption and carbon emissions, with adverse environmental consequences that scale with AI adoption.

**Frameworks:** NIST_GenAI, NIST_RMF, MITRE, CDAO

**Key risks:**
- 8.3.1 High carbon emissions from AI training: Training a single large transformer model can emit as much carbon as hundreds of transatlantic flights [E] — NIST_GenAI, MITRE
- 8.3.2 Energy-intensive AI inference: Generative tasks are more energy- and carbon-intensive than discriminative or non-generative tasks [E] — NIST_GenAI
- 8.3.3 Lack of standardized environmental measurement: No agreed-upon method currently exists to estimate environmental impacts from GenAI, complicating risk assessment [E] — NIST_GenAI
- 8.3.4 Sustainability and energy cost at organizational level: Failure to evaluate and justify GenAI energy consumption versus expected benefits contributes to unnecessary environmental impact [E] — CDAO, NIST_RMF

---

## Framework Coverage Matrix

| Domain | ATFAA | Cisco_A2A | Google | NSA_Data | NSA_Deploy | NSA_Dev | DoD_Cyber | DoD_RAI | NIST_RMF | NIST_GenAI | OWASP_MAS | OWASP_Top10 | OWASP_T&M | OWASP_Secure | OWASP_State | ENISA | GAO | MITRE | CDAO |
|--------|-------|-----------|--------|----------|------------|---------|-----------|---------|----------|------------|-----------|-------------|-----------|--------------|-------------|-------|-----|-------|------|
| 1. Output & Content | ✓ | | | | ✓ | | | ✓ | ✓ | ✓ | | ✓ | ✓ | | ✓ | ✓ | | ✓ | ✓ |
| 2. Security & Adversarial | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ | ✓ | | ✓ | ✓ |
| 3. Agentic & Autonomous | ✓ | ✓ | ✓ | | | ✓ | | | ✓ | | ✓ | ✓ | ✓ | ✓ | ✓ | | | ✓ | ✓ |
| 4. Data & Privacy | ✓ | | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ | | | ✓ | ✓ | | ✓ |
| 5. Supply Chain & Infrastructure | | ✓ | | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ | ✓ | ✓ | | ✓ | | ✓ | ✓ |
| 6. Governance & Accountability | ✓ | | ✓ | | | ✓ | ✓ | ✓ | ✓ | ✓ | | | ✓ | | ✓ | ✓ | ✓ | | ✓ |
| 7. Operational Reliability | | | | | ✓ | ✓ | ✓ | ✓ | ✓ | | | | | | | ✓ | ✓ | ✓ | ✓ |
| 8. Societal & Ethical | | | | | ✓ | | | ✓ | ✓ | ✓ | | | | | ✓ | ✓ | | ✓ | ✓ |

---

## Framework Reference

| Abbreviation | Full Name | Type | Scope |
|--------------|-----------|------|-------|
| ATFAA | ATFAA SHIELD (Narajala & Narayan, AWS) | Threat model | Agentic AI |
| Cisco_A2A | Cisco A2A Protocol Security (Habler et al.) | Threat model | Agentic AI / Multi-agent protocols |
| Google | Google Secure AI Agents (Díaz, Kern, Olive) | Security + governance | Agentic AI |
| NSA_Data | NSA AI Data Security (CSI, May 2025) | Security | General AI / Data lifecycle |
| NSA_Deploy | NSA/DHS Deploy AI Securely (joint CSI + DHS guidelines) | Security + governance | AI deployment / Critical infrastructure |
| NSA_Dev | NSA/CISA Secure AI System Development (UK NCSC / CISA joint) | Security | AI development lifecycle |
| DoD_Cyber | DoD AI Cybersecurity RMF Tailoring Guide | Security | AI cybersecurity / DoD |
| DoD_RAI | DoD Responsible AI Strategy and Implementation Pathway | Governance | DoD AI governance |
| NIST_RMF | NIST AI Risk Management Framework (AI 100-1, 100-2, SP 800-218A) | Governance + technical | General AI / GenAI |
| NIST_GenAI | NIST Generative AI Profile (AI 600-1) | Governance + risk taxonomy | GenAI-specific |
| OWASP_MAS | OWASP MAS Threat Modelling Guide (MAESTRO) | Threat model | Multi-agent systems |
| OWASP_Top10 | OWASP Top 10 for Agentic Applications 2026 (ASI01–ASI10) | Risk ranking | Agentic AI applications |
| OWASP_T&M | OWASP Agentic AI Threats and Mitigations (v1.1) | Threat model + mitigations | Agentic AI |
| OWASP_Secure | OWASP Securing Agentic Applications Guide (v1.0) | Secure architecture guide | Agentic AI |
| OWASP_State | OWASP State of Agentic AI Security and Governance (v1.0) | State of industry + governance | Agentic AI |
| ENISA | ENISA Multilayer AI Security Framework | Cybersecurity practice | General AI / EU context |
| GAO | GAO AI Accountability Framework (GAO-21-519SP) | Accountability + audit | U.S. federal government AI |
| MITRE | MITRE ATLAS + SAFE-AI Report | Adversarial taxonomy + controls | AI/ML-enabled systems |
| CDAO | CDAO GenAI Responsible AI Toolkit + DAGR | Responsible AI planning | DoD GenAI lifecycle |

---

## Notes on This Map

**Source coverage:** This map is derived exclusively from the 19 source documents captured in cache files A1 through A8. No risks have been added that are not traceable to one of these sources. The DIU RAI Guidelines folder was present in the repository but contained no analyzed source documents at the time of synthesis and is therefore not represented.

**Scope boundaries:** This map covers risks associated with generative AI and agentic AI systems. Risks specific to narrow/predictive ML (e.g., classical computer vision models used in non-generative contexts) appear only where covered by a framework that explicitly addresses them in the AI risk context (primarily NIST_RMF and MITRE/ATLAS).

**Item count:** This map contains approximately 182 individual risk items across 8 domains and 30 categories, meeting the target of 150–200 items.

**Emerging risks not fully addressed:** Several emerging threat patterns noted in OWASP_State — including reverse engineering and behavioral exploitation of widely deployed agents, and large-scale manipulative social engineering by AI — are captured at the category level but remain incompletely characterized in the literature as of the source document dates.
