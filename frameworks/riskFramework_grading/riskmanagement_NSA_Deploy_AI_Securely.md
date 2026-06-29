# Risk Management Analysis: NSA Deploy AI Securely

**Framework:** (1) Joint CSI: Deploying AI Systems Securely — Best Practices for Deploying Secure and Resilient AI Systems (NSA/AISC, CISA, FBI, ASD ACSC, CCCS, NCSC-NZ, NCSC-UK, April 2024); (2) DHS/CISA: Mitigating AI Risk — Safety and Security Guidelines for Critical Infrastructure Owners and Operators (April 2024)
**Source:** `NSA_Deploy_AI_Securely/CSI-DEPLOYING-AI-SYSTEMS-SECURELY.pdf`, `NSA_Deploy_AI_Securely/DHS-CISA-AI-Safety-Security-Guidelines-Critical-Infrastructure.pdf`

## Framework Summary

The NSA_Deploy_AI_Securely folder combines two complementary April 2024 publications. The first, a joint Cybersecurity Information Sheet (CSI) by NSA, CISA, FBI, and five allied national cybersecurity agencies, provides technical best practices for organizations that deploy externally developed AI systems. It is structured around three phases: (1) securing the deployment environment through governance, Zero Trust architecture, container/VM sandboxing, GPU/hardware patching, TLS encryption, and phishing-resistant MFA; (2) continuously protecting the AI system through cryptographic artifact validation, adversarial testing, supply chain inspection of pre-trained models, input sanitization against prompt injection, comprehensive logging of inputs/outputs/intermediate states, and hardware-protected model weight storage (HRZ, HSM); and (3) secure operation and maintenance through RBAC/ABAC, audits, penetration testing, immutable backups, and automated rollback. The second document, issued by DHS/CISA in response to Executive Order 14110, provides governance-oriented guidelines for critical infrastructure using the NIST AI RMF (Govern, Map, Measure, Manage) functions. It classifies AI risk into three categories — attacks using AI, attacks on AI, and AI design and implementation failures — and prescribes mitigations including SBOM/AIBOM supply chain reviews, human supervision, continuous red-teaming, dataset and model validation, adversarial training, identity and access controls, behavior monitoring, and incident response integration. Together, the two documents are strongest for deployment-phase technical controls and supply chain security, and weakest on agentic-specific concerns such as multi-agent trust, reasoning security, memory poisoning, and orchestration framework internals.

---

## Scoring Key

- **3** — Framework directly and strongly supports managing this risk through specific, actionable guidance.
- **2** — Framework indirectly and moderately supports managing this risk; relevant controls exist but require extrapolation.
- **1** — Framework weakly supports managing this risk; only generic or tangential guidance applies.

---

## RATC — Agent–tool coupling as "policy-level remote code execution"

### RATC_1 - UI/UX Abstraction and Visibility Gaps
- Strength score: 1

The NSA/CISA framework does not address UI/UX design, abstraction layers in approval interfaces, or the visibility of tool invocation chains to human reviewers. RATC_1's six sub-threats (RATC_1_1 through RATC_1_6) all center on how graphical and conversational interfaces obscure the true risk of tool calls—approval fatigue from abstracted SQL operations, progressive disclosure hiding multi-agent tool chains, inline suggestions executing invisibly, dashboards failing to differentiate risk levels, context-driven auto-invocations, and trace visualization that conceals complexity. The framework's guidance on human oversight is limited to generic RBAC/ABAC access controls and the DHS/CISA NIST AI RMF call for human supervision; it contains no guidance on UI design principles, tool chain visualization, or risk-differentiated approval interfaces. Scoring 1.

### RATC_2 - Approval Workflow and User Attention Vulnerabilities
- Strength score: 1

RATC_2 describes three specific attack vectors against human-in-the-loop approval systems: approval fatigue from high-volume multi-agent request floods (RATC_2_1), keyboard shortcut hijacking via injected JavaScript or browser extensions that capture unintended approvals (RATC_2_2), and time-based default exploits that trigger unauthorized actions when reviewers are unavailable (RATC_2_3). None of these threats are addressed by the NSA/CISA framework. The framework recommends human supervision and RBAC/ABAC but provides no guidance on approval workflow design, anti-fatigue mechanisms, UI manipulation prevention, or timeout policies for high-stakes decisions. The DHS/CISA document's human supervision recommendation is entirely generic and does not engage with the adversarial manipulation of approval interfaces. Scoring 1.

### RATC_3 - Tool Overload, Parameter Handling, and Selection Errors
- Strength score: 2

The NSA/CISA framework addresses input sanitization and prompt injection protection, which partially covers RATC_3_2's tool argument injection vector—specifically, the scenario where untrusted data is embedded in tool-call parameters that cross agent trust boundaries. The framework's least-privilege guidance also tangentially applies to RATC_3_1's privilege escalation through tool overload, insofar as restricting tool access per agent limits blast radius. However, the framework does not address LLM cognitive overload from excessive tool counts, probabilistic parameter extraction errors in chained calls, or the specific pattern of separating argument construction from execution across agent boundaries. RATC_3_3 and RATC_3_4 (inline suggestion and chat interface provenance obscuration) are UI/UX concerns entirely absent from the framework. The input sanitization guidance requires significant extrapolation to apply to argument injection across agent trust boundaries. Scoring 2.

### RATC_4 - Confidence Manipulation and Tool Authorization
- Strength score: 1

RATC_4_1 describes adversarial prompt injection inflating agent confidence scores to bypass confidence-based approval gates, including cascading failures when downstream agents trust upstream confidence scores without re-validation. RATC_4_2 describes authorization scope confusion where attackers compromise intermediate coordination agents to achieve privilege escalation, and precondition validation bypass through transitive trust. The NSA/CISA framework does not address confidence-based gating mechanisms, inter-agent trust propagation, or the specific vulnerability of intermediate orchestration agents holding multi-domain tool access. The framework's RBAC/ABAC guidance applies broadly to access control but does not address the agent-specialization-specific authorization scopes or transitive trust exploitation described here. Adversarial testing and red-teaming mentioned in DHS/CISA provide only generic applicability. Scoring 1.

### RATC_6 - Tool Metadata Poisoning Across Registries and Discovery
- Strength score: 2

The NSA/CISA framework's supply chain inspection guidance for pre-trained models and SBOM/AIBOM reviews in DHS/CISA provide partial coverage for RATC_6_1's centralized tool registry poisoning. The framework's adversarial testing and supply chain review recommendations could be extrapolated to cover poisoned tool metadata in shared registries. However, the framework does not specifically address vector database metadata poisoning, few-shot parameter examples in tool descriptions as injection vectors, or framework-specific tool selection mechanisms (RATC_6_2). RATC_6_3's framework abstraction leakage creating blind spots across multi-framework orchestrations is entirely unaddressed—the framework has no guidance on LangChain, AutoGen, CrewAI, or Semantic Kernel internals. The supply chain guidance requires substantial extrapolation to reach tool registry metadata poisoning. Scoring 2.

### RATC_8 - Advanced Tool Invocation Patterns and Inference
- Strength score: 1

RATC_8_1 describes iterative tool chain exploitation through multi-agent replanning loops where attackers induce controlled failures to progressively chain innocuous tools into dangerous sequences, with cross-agent state accumulation scaling quadratically. RATC_8_2 covers ReAct pattern observation injection where one agent's tool outputs become poisoned inputs to another agent without re-validation. RATC_8_3 addresses schema translation vulnerabilities across framework boundaries. The NSA/CISA framework's input sanitization guidance provides minimal applicability to RATC_8_2 (observation injection), but the framework does not address replanning loop exploitation, Plan-and-Execute attack surfaces, cross-agent observation validation, or multi-framework schema translation boundaries. These are agentic architecture-specific threats that the framework—written before widespread production deployment of multi-agent systems—does not engage with. Scoring 1.

### RATC_9 - Web and Multimodal Tool Exploitation
- Strength score: 2

The NSA/CISA framework's input sanitization and prompt injection protection guidance has direct applicability to RATC_9_1's web agent tool vulnerabilities, specifically adversarial content injection in scraped web pages propagating as legitimate instructions through multi-agent pipelines. This is one of the framework's stronger mappings to RATC threats. However, the framework does not address hidden form fields, manipulated navigation logging, or incomplete audit trails from web tool manipulation. For RATC_9_2, the framework's data source catalog and supply chain review recommendations provide tangential coverage for multimodal RAG poisoning, but cross-modal parameter injection without sanitization boundaries and vision model output laundering through sequential processing are not specifically addressed. The prompt injection protection guidance is the strongest applicable control but requires extrapolation to multimodal contexts. Scoring 2.

### RATC_10 - Efficiency Optimization and Resource Constraints
- Strength score: 1

RATC_10_1 describes attackers forcing upstream agents into token-constrained contexts that truncate safety-critical tool descriptions, causing downstream agents to select tools based on incomplete specifications. RATC_10_2 covers iteration budget manipulation and temperature tuning exploitation creating policy inconsistency across model diversity. The NSA/CISA framework does not address token budget management, context window security, iteration budget policies, temperature configuration, or model selection diversity as security concerns. These are agentic runtime configuration threats entirely outside the framework's scope. The framework's general adversarial testing recommendation provides only the most generic applicability. Scoring 1.

### RATC_11 - Tool Execution Infrastructure and Orchestration
- Strength score: 2

The NSA/CISA framework's network security guidance (Zero Trust, TLS) and sandboxed containers/VMs recommendation provide partial coverage for RATC_11_1's API gateway routing policy manipulation—ZT principles of verifying every request could apply to tool routing decisions. For RATC_11_2, the framework's container/VM sandboxing guidance addresses the general principle of container isolation, though it does not specifically cover Kubernetes security contexts, Persistent Volume Claims as attack vectors, sidecar proxy interception, or init container configuration injection. The framework's general infrastructure hardening guidance is applicable but requires substantial extrapolation to reach these Kubernetes-specific and microservices-specific threats. The framework explicitly notes GPU patching as a concern, which shows awareness of infrastructure-layer threats, but does not reach the orchestration layer specifics described in RATC_11. Scoring 2.

### RATC_12 - Distributed and Hardware-Level Attacks
- Strength score: 2

The NSA/CISA framework explicitly addresses GPU patching and hardware model weight protection in Hardware Root of Trust (HRZ) and Hardware Security Modules (HSM), which provides partial coverage for RATC_12's hardware-level threats. For RATC_12_1, the framework's TLS recommendation could be extrapolated to cover inter-GPU communication encryption for tensor parallelism (currently unencrypted by default), though the framework does not specifically mention NVLink/NVSwitch or inter-GPU communication security. For RATC_12_2, KV cache poisoning and quantization calibration dataset poisoning are not addressed; the framework's hardware protection guidance focuses on model weight storage rather than inference-time cache isolation. The framework's awareness of hardware-layer security is a genuine strength, but the specific multi-tenant GPU inference threats described here exceed the framework's explicit guidance. Scoring 2.

### RATC_14 - Reasoning and Planning Vulnerabilities
- Strength score: 1

RATC_14_1 describes poisoned chain-of-thought reasoning traces embedding justifications for dangerous tool sequences and tool selection bias propagating through shared reasoning chains. RATC_14_2 covers self-consistency exploitation where all sampling paths converge on dangerous tools under manipulated input, and quality-weighted voting manipulation in RASC. RATC_14_3 addresses hierarchical planning tool visibility fragmentation. The NSA/CISA framework explicitly does not cover reasoning and planning security—these are internal model inference processes. The framework's adversarial testing and red-teaming recommendations are the closest applicable guidance but provide only generic coverage. The framework has no concept of chain-of-thought security, self-consistency attack surfaces, or hierarchical planning vulnerabilities. Scoring 1.

### RATC_15 - Episodic Memory and Learning
- Strength score: 1

RATC_15_1 describes attackers poisoning episodic memory by recording malicious tool sequences as successful resolutions, causing agents to replicate dangerous chains when retrieving similar episodes, with trajectory abstraction converting poisoned episodes into procedural workflows. The NSA/CISA framework does not address agent memory systems—episodic, semantic, or procedural. The DHS/CISA document's Footnote 48 mentions cryptographic measures for prompt stores, caches, vector stores, and knowledge bases, which provides tangential applicability to securing episodic memory storage integrity, but does not address the semantic poisoning of stored episode content or trajectory abstraction vulnerabilities. This is a substantial gap in the framework's coverage of agentic AI threats. Scoring 1.

### RATC_16 - Semantic Memory and RAG
- Strength score: 2

The NSA/CISA framework's input sanitization and prompt injection protection guidance applies to RATC_16_1's RAG pipeline poisoning insofar as retrieved content embedding tool-invocation instructions represents a prompt injection vector. The DHS/CISA Footnote 48 explicitly mentions cryptographic measures for vector stores and knowledge bases, which directly addresses the integrity of semantic memory storage. However, the framework does not address knowledge graph relationship poisoning causing unsafe tool combinations to appear recommended, query rewriting instruction injection, or knowledge base staleness as a security concern. The combination of prompt injection protections and the specific mention of vector store cryptographic integrity in Footnote 48 provides moderate but meaningful coverage that exceeds purely generic guidance. Scoring 2.

### RATC_17 - Utility Functions and Decision Logic
- Strength score: 1

RATC_17_1 describes utility function poisoning through outcome assumption injection causing miscalculated tool utility, sequential expected utility miscalculation misrepresenting available future options, and trade-off weight manipulation in multi-objective utility functions. The NSA/CISA framework does not address utility-based decision logic, expected utility calculations, or multi-objective optimization in agentic systems. These are internal agent decision-architecture concerns entirely outside the framework's scope. The framework's adversarial testing recommendation is the only applicable guidance, providing only the most generic coverage. Scoring 1.

### RATC_18 - Rule-Based and Knowledge-Engineered Systems
- Strength score: 1

RATC_18_1 describes rule specificity hierarchy exploitation enabling injected rules to override safety rules, forward chaining exploitation through injected facts triggering dangerous authorization chains, certainty factor manipulation, and lexicographic heuristic objective reordering. The NSA/CISA framework does not address rule-based AI systems, forward chaining engines, certainty factors, or knowledge engineering security. The framework's RBAC/ABAC guidance addresses human access control rather than AI rule-based authorization systems. These threats are specific to hybrid AI architectures combining LLMs with rule engines, which the framework does not contemplate. Scoring 1.

### RATC_19 - Learning and Reinforcement Learning
- Strength score: 1

RATC_19_1 describes a comprehensive set of RL-specific attacks: reward poisoning training agents to select malicious tools, shared replay buffer poisoning in multi-agent RL affecting all agents' Q-values, curriculum learning poisoning, imitation learning trajectory poisoning, actor-critic cross-agent critic manipulation, and proximal policy optimization trust region exploitation to iteratively converge policies toward dangerous attractors. The NSA/CISA framework explicitly does not cover MARL or RL training dynamics. The framework's supply chain and adversarial training guidance provides no specific applicability to RL reward signals, replay buffers, or policy optimization vulnerabilities. These are training-time attacks on RL systems that the framework—focused on deployment security—does not address. Scoring 1.

### RATC_21 - Parallel Retrieval Race Conditions in Multi-Agent Query Decomposition Systems
- Strength score: 1

RATC_21 describes a cluster of concurrent-access threats: parallel sub-query retrieval from inconsistent database states, shared index deduplication races, timeout cascade failures overwhelming connection pools, multi-stage cache TTL temporal inconsistency, network propagation delays causing fleet-wide inconsistency, container supply chain poisoning through registry compromise during automated deployment, and centralized inference infrastructure single points of failure. The NSA/CISA framework's supply chain inspection guidance partially applies to the container registry poisoning vector, and the ZT network guidance provides generic applicability to fleet-wide consistency concerns. However, the framework does not address distributed concurrency control, race conditions in shared retrieval infrastructure, TTL-based cache consistency, or connection pool exhaustion as security concerns. The race condition and consistency threats are entirely outside the framework's scope. Scoring 1.

### RATC_22 - Multi-Stage Pipeline Result Caching Race Enabling Multi-Agent Consistency Failures
- Strength score: 2

RATC_22 includes hash collision attacks on shared caching infrastructure where adversaries craft colliding queries to poison cache entries affecting all agents—this is a specific adversarial attack that the framework's adversarial testing and input sanitization guidance has some applicability to. The DHS/CISA supply chain guidance and the NSA/CISA supply chain inspection recommendation cover the container registry supply chain attack vectors described (credential compromise, typosquatting, digest collision bypass, GitOps automation backdoor deployment). Tensor parallelism communication poisoning through multi-tenant GPU collocation overlaps with the framework's hardware security and GPU patching guidance. However, the framework does not address cache write races, TTL-based consistency failures, profiling telemetry exposure enabling targeted DoS, or budget exhaustion attacks. The supply chain and hardware security coverage provides moderate but real applicability to a subset of RATC_22's threats. Scoring 2.

### RATC_27 - MIG Instance Co-Location Creating Shared Physical GPU Failure Propagation Across Multi-Agent Deployments
- Strength score: 2

The NSA/CISA framework explicitly addresses GPU patching and hardware model weight protection via Hardware Root of Trust and HSMs, demonstrating awareness of GPU-layer security concerns that is directly relevant to RATC_27. The threat describes MIG logical isolation failing due to shared physical hardware dependencies (power regulators, thermal management, PCIe interface, firmware) and firmware vulnerabilities enabling cross-partition memory access. The framework's GPU patching guidance directly applies to the firmware vulnerability vector described. However, the framework does not address MIG-specific isolation boundaries, multi-tenant GPU co-location policies, PCIe error propagation, or thermal throttling as security-relevant failure modes. The framework's hardware security guidance provides genuine but partial coverage, requiring extrapolation from firmware patching to cross-partition isolation failures. Scoring 2.

### RATC_29 - Shared Workflow State Version Conflict Amplification Through Concurrent Write Flooding Creating Retry Storm Cascades
- Strength score: 1

RATC_29 describes targeted collision bursts exhausting optimistic locking retry budgets in shared workflow state systems, creating retry storms where conflict probability scales quadratically with concurrency and attackers can cause workflows to fail without completing. The NSA/CISA framework does not address distributed workflow state management, optimistic locking, retry storm vulnerabilities, or concurrency-based denial of service against shared coordination infrastructure. The framework's general network security and ZT guidance provide only the most tangential applicability—these are application-layer concurrency control vulnerabilities entirely outside the framework's scope. Scoring 1.

### RATC_30 - Guardrail Bypass Through Infrastructure Failure Injection Preventing Safety Validation Execution
- Strength score: 2

The NSA/CISA framework's adversarial testing, logs of inputs/outputs/intermediate states, and oracle attack alerts guidance has meaningful applicability to RATC_30. The threat describes adversary-induced processing delays exceeding validation timeouts causing guardrail bypass, and network flooding or resource exhaustion causing fleet-wide guardrail bypass when agents fall back to unvalidated responses. The framework's oracle attack alerts are directly relevant to detecting anomalous bypass patterns. The DHS/CISA behavior monitoring and incident response guidance applies to detecting fleet-wide guardrail failures. However, the framework does not address timeout threshold design for safety validation, fallback behavior security (fail-open vs. fail-closed policies), or DoS-resistant guardrail architecture. The monitoring and alerting guidance provides moderate coverage, but the specific fail-open vulnerability under infrastructure attack is not addressed. Scoring 2.

## RDL — New data‑leakage channels via large contexts, logs, and probabilistic recall

### RDL_1 - UI/UX Patterns
- Strength score: 1
The NSA/CISA framework does not address UI/UX design or the leakage risks arising from chat interface conversation history, progressive disclosure debug views, session persistence across security boundaries, command palette history, multi-agent dashboards, context reference links, approval workflow audit trails, or accessibility (ARIA/alt-text) exposure vectors. The framework's guidance on API returns of minimal data and least-privilege access controls is conceptually relevant but does not translate into actionable guidance for any of these UI-layer leakage channels. The DHS/CISA footnote 48 on cryptographic measures for data layers (prompt stores, caches) touches adjacent infrastructure but says nothing about front-end interface design. All RDL_1 sub-items are explicitly identified in the framework's gap list (UI/UX design). Score 1 applies uniformly across all eleven sub-items in this subsubsection.

### RDL_2 - Data Persistence and Caching
- Strength score: 2
The NSA/CISA framework indirectly addresses data persistence and caching risks. Guidance on API returns of minimal data, input sanitization, and logging of inputs/outputs/intermediate states establishes the principle that data should not persist unnecessarily. DHS/CISA footnote 48 specifically calls out cryptographic measures for caches, prompt stores, and vector stores, and recommends data masking for sensitive data. These controls partially address RDL_2_1 (session serialization), RDL_2_2 (cached responses outliving sessions), and RDL_2_3 (undo buffers preserving deleted data). However, the framework provides no specific guidance on cache isolation across multi-agent composite entries, cache key design, distributed undo coordination, or retention policy enforcement across agent boundaries — the precise risks articulated here. The coverage requires significant extrapolation from generic data protection principles.

### RDL_3 - Streaming and Token-Level Leakage
- Strength score: 1
The framework does not address streaming response architectures, token-by-token output exposure, tokenization side channels, streaming output length correlation, streaming state updates during iterative multi-agent cycles, or streaming caching as a covert exfiltration vector. Input sanitization and prompt injection guidance from the NSA/CISA document is aimed at inbound data, not outbound streaming. Logging of inputs/outputs/intermediate states could theoretically capture streaming artifacts, but the framework does not acknowledge streaming as a distinct leakage surface or provide guidance on redaction timing relative to streaming delivery. All seven sub-items (RDL_3_1 through RDL_3_7) are unaddressed by specific controls, making score 1 appropriate.

### RDL_4 - Search and Recall
- Strength score: 1
The framework does not address probabilistic or semantic recall as a leakage channel. RDL_4_1 describes a threat specific to semantic similarity search across multi-agent conversation indexes crossing security-domain boundaries without policy enforcement. The framework's access controls and least-privilege guidance are conceptually applicable but provide no guidance on securing search indexes, enforcing access-controlled semantic search, or preventing cross-domain contamination through similarity matching. DHS/CISA footnote 48 mentions knowledge-bases and vector stores as requiring cryptographic protection but does not address semantic search boundary enforcement. Score 1 reflects the absence of actionable or specific guidance.

### RDL_5 - Attribution and Observability
- Strength score: 2
The NSA/CISA framework calls for logging of inputs, outputs, and intermediate states, and for RBAC/ABAC-controlled access to logs and audit trails. This partially addresses RDL_5_1 (attribution logs as organizational intelligence) and RDL_5_2 (framework-dependent logging aggregation) by establishing that logs should exist and be access-controlled. However, the framework does not address the specific risk that comprehensive multi-agent attribution and telemetry aggregation itself becomes a high-value target disclosing organizational structure, agent specialization patterns, or system architecture. RDL_5_3 (error classification pattern leakage) and RDL_5_4 (latency measurement information leakage) as timing/metadata side channels are not addressed at all. The coverage is indirect and requires extrapolation; score 2 reflects partial relevance without specific guidance on observability as a leakage vector.

### RDL_7 - Tool Invocation and Function Calling
- Strength score: 2
The framework provides relevant but non-specific guidance. NSA/CISA guidance on logging inputs/outputs/intermediate states, API returns of minimal data, input sanitization, and least-privilege access relates to several RDL_7 sub-items: tool invocation logging as a side channel (RDL_7_1), sensitive parameter leakage through action logs (RDL_7_8), and tool execution log aggregation (RDL_7_10). The DHS/CISA access control guidance (limiting access to models, inputs, training data) is tangentially relevant. However, the framework says nothing specifically about function calling JSON in context windows (RDL_7_2), tool error message implementation detail disclosure (RDL_7_3), tool selection reasoning as cognitive state leakage (RDL_7_4), plugin registry enumeration (RDL_7_5), audit trail poisoning (RDL_7_6), or tool success rate differential leakage (RDL_7_7). Coverage is partial and requires extrapolation. Score 2 applies for the subset of log-related items, with the broader set of sub-items essentially unaddressed.

### RDL_9 - Multi-Agent Memory and State
- Strength score: 1
The framework does not address agent memory system security. RDL_9_1 through RDL_9_5 describe reflection memory leakage, ReAct reasoning trace forensic reconstruction, distributed trace cross-tenant correlation via timing, shared blackboard access pattern side-channels, and dashboard correlation leakage — all specific to multi-agent memory and state architectures. The framework's general access controls and logging guidance are too generic to constitute meaningful coverage of these threats. The framework's own gap list explicitly identifies agent memory system security (episodic, semantic, RAG) as not covered beyond general access controls, confirming score 1.

### RDL_10 - Reasoning and CoT Traces
- Strength score: 1
Reasoning and planning security is explicitly identified as a gap in the framework. RDL_10_1 through RDL_10_10 describe leakage vectors arising from Chain-of-Thought traces stored in shared memory, intermediate step verbosity enabling data reconstruction, secrets embedded in reasoning documentation, cross-agent reasoning context propagation, Tree-of-Thought lookahead branch observation, preserved reasoning path leakage, memory consolidation data leakage, self-consistency intermediate chain logging, quality score metadata side-channels, and failure case logging leakage. None of these are addressed by the NSA/CISA or DHS/CISA documents, which do not contemplate LLM reasoning trace security as a distinct concern. Score 1 applies across all ten sub-items.

### RDL_11 - Multimodal and Input Processing
- Strength score: 1
The framework does not address multimodal agent processing. RDL_11_1 through RDL_11_5 describe leakage surfaces unique to multimodal RAG: large context window expansion from combined text/image/audio inputs, vision model intermediate representation leakage, audio embedding privacy channels (speaker identity, emotional tone), chart linearization creating persistent machine-readable sensitive data, and embedding inversion attacks reconstructing original images from stored embeddings. The framework's input sanitization and prompt injection protection guidance applies to text inputs and does not extend to multimodal processing pipelines. DHS/CISA footnote 48 mentions vector stores but not embedding inversion or multimodal-specific leakage. Score 1 is appropriate.

### RDL_12 - Error Handling and Graceful Degradation
- Strength score: 1
The framework does not address error handling as a leakage channel. RDL_12_1 through RDL_12_5 describe persistent data leakage through error log full-context capture, retry attempt logging exposing intermediate states, fallback routing to secondary providers with weaker log access controls, graceful degradation state logging disclosing system architecture, and circuit breaker state change logging enabling infrastructure reconnaissance. The NSA/CISA guidance on logging inputs/outputs/intermediate states is oriented toward security monitoring, not toward minimizing what error logs capture or ensuring consistent access controls across fallback providers. No specific guidance addresses these error-handling leakage patterns. Score 1 applies.

### RDL_13 - Evaluation and Testing Leakage
- Strength score: 1
The framework does not address evaluation and testing infrastructure as a leakage surface. RDL_13_1 through RDL_13_9 describe leakage through evaluation metric logs, baseline comparison context window exposure, evaluation dashboard test dataset exposure, artifact storage compromise, evaluation log error message disclosure, metric output fingerprinting, temporal analysis of metric changes, cross-validation fold result analysis, and benchmark result leakage through approval logs. While the framework calls for adversarial testing and red-teaming, it says nothing about securing the evaluation infrastructure itself, protecting test datasets, or preventing metric fingerprinting. Score 1 reflects complete absence of relevant guidance.

### RDL_14 - Feedback and Testing
- Strength score: 1
The framework does not address user feedback storage or A/B test result security as leakage vectors. RDL_14_1 (user feedback extraction enabling vulnerability profiling) and RDL_14_2 (A/B test result leakage enabling vulnerability prediction) are operational feedback loop risks with no corresponding controls in either the NSA/CISA or DHS/CISA documents. General access control guidance could theoretically be applied to feedback storage, but no specific guidance exists. Score 1 is appropriate.

### RDL_15 - Evaluation Metric Exploitation
- Strength score: 1
This subsubsection describes adversarial manipulation of evaluation metrics: Exact Match metric exploitation via semantic paraphrasing (RDL_15_1), joint metric evasion through selective fact injection (RDL_15_2), Pass@K inconsistency exploitation for probabilistic attacks (RDL_15_3), and milestone scoring threshold manipulation (RDL_15_4). The framework calls for adversarial testing and red-teaming but does not address the specific risk of adversaries crafting inputs to manipulate evaluation metric outcomes or exploit probabilistic non-determinism in Pass@K scoring. These threats sit at the intersection of evaluation system design and adversarial ML, neither of which is specifically addressed. Score 1 applies.

### RDL_16 - Parameter and Configuration Leakage
- Strength score: 1
RDL_16_1 through RDL_16_3 describe information leakage through context window size differences in audit logs (revealing which agents maintain comprehensive versus minimal logging), token consumption patterns from cost-optimized routing (revealing query sensitivity), and latency profile fingerprinting enabling configuration inference. The NSA/CISA guidance on GPU patching and TLS is infrastructure-level and does not address side-channel inference through operational metrics such as token cost differentials or latency profiles. The framework provides no guidance on normalizing observable operational characteristics to prevent configuration fingerprinting. Score 1 is appropriate.

### RDL_17 - Prompt Injection and Few-Shot Attacks
- Strength score: 2
The NSA/CISA framework explicitly calls for input sanitization and prompt injection protection, which provides direct but generic coverage for this subsubsection. RDL_17_1 (demonstration output format injection causing parse confusion in downstream agents), RDL_17_2 (generation pattern injection through biased few-shot examples), and RDL_17_3 (few-shot demonstration contamination with leaked sensitive data) are all variants of prompt injection or poisoned-input attacks. The framework's prompt injection protection guidance is the most directly applicable control in the entire framework for these threats. However, the guidance does not specifically address few-shot demonstration poisoning as a distinct attack vector, multi-agent propagation of injections through format interpretation, or the challenge of detecting probabilistic recall of sensitive demonstration data. Score 2 reflects meaningful but incomplete coverage requiring extrapolation.

### RDL_18 - Distributed Tracing
- Strength score: 1
The framework does not address distributed tracing infrastructure security. RDL_18_1 describes comprehensive workflow leakage through trace ID correlation and span data stored in tracing backends, and RDL_18_2 describes response schema validation log side-channels enabling agent type inference. The framework's logging guidance focuses on what to log for security monitoring purposes, not on securing the tracing infrastructure itself against exfiltration or side-channel exploitation. No guidance addresses distributed trace access controls or span data sensitivity. Score 1 applies.

### RDL_19 - Execution and Orchestration
- Strength score: 1
RDL_19_1 (execution path disclosure through aggregated error messages across agents) and RDL_19_2 (parameter provenance tracking loss creating attribution gaps enabling poisoning propagation) are multi-agent orchestration-specific threats. The framework does not address error message aggregation as a leakage vector or parameter provenance tracking as a security control. While the framework references threat models and supply chain inspection, it does not address runtime provenance tracking of data flowing between agents, which is precisely what RDL_19_2 identifies as the missing control. Score 1 applies.

### RDL_20 - Multi-Agent Reasoning
- Strength score: 1
Reasoning and planning security is explicitly identified as a framework gap. RDL_20_1 through RDL_20_4 describe orchestrator reasoning quality cascading effects on worker agents, supervisor delegation reasoning transparency paradox enabling delegation manipulation, absence of cross-agent reasoning coherence validation, and consensus reasoning exploitation via low-informativeness injections. These threats require reasoning-aware security controls that the framework does not contemplate. General guidance on RBAC/ABAC and least privilege does not address the reasoning-layer vulnerabilities described here. Score 1 applies.

### RDL_21 - Efficiency and Cost Attribution
- Strength score: 1
RDL_21_1 through RDL_21_7 describe leakage through efficiency monitoring logs (token spikes, API call patterns, cache metrics), cost attribution metadata revealing processing sensitivity, cache hit rate patterns identifying high-value targets, latency anomalies inferring payload size, efficiency dashboard exposure of system internals, historical efficiency trend behavioral inference, and budget reallocation patterns revealing operational priorities. The framework does not address operational efficiency metrics as a leakage surface. While the framework calls for monitoring and audits, it is silent on securing the monitoring data itself against side-channel exploitation. Score 1 is appropriate.

### RDL_22 - Infrastructure and Deployment
- Strength score: 2
The NSA/CISA framework addresses infrastructure security through sandboxed containers and VMs, GPU patching, TLS, and threat models. This provides indirect coverage for several RDL_22 items: TLS addresses the plaintext inter-agent communication risks underlying RDL_22_4 (API gateway log correlation) and partially RDL_22_7 (centralized logging pipeline as exfiltration vector). Sandboxed containers relate to RDL_22_3 (Prometheus metrics leakage) and RDL_22_6 (dead-letter queue content). However, the framework does not specifically address message queue header metadata correlation (RDL_22_1), vector database embedding return enabling inversion (RDL_22_2), Prometheus label cardinality risks (RDL_22_3), MLflow artifact metadata version history leakage (RDL_22_5), or centralized log aggregation as a unified exfiltration target (RDL_22_7). The coverage is infrastructure-level and requires significant extrapolation to apply to these specific deployment-layer leakage vectors. Score 2 reflects the existence of relevant but non-specific controls.

### RDL_23 - Containerization Security
- Strength score: 2
The NSA/CISA framework explicitly recommends sandboxed containers and VMs as part of secure deployment, which provides a foundational level of coverage for containerization security threats. This is relevant to RDL_23_1 (shared memory cache side-channels), RDL_23_3 (persistent volume mount permission escalation), and RDL_23_4 (sidecar proxy log interception). The framework's general guidance on patching and TLS is relevant to RDL_23_6 (kubelet unauthenticated metrics) and RDL_23_4. However, the framework does not address Kubernetes-specific risks such as etcd database backup leakage (RDL_23_7), init container environment variable leakage (RDL_23_5), event stream retention creating durable leakage (RDL_23_2), or the specific overly-permissive PersistentVolume mount risk (RDL_23_3) in any actionable way. The recommendation to use sandboxed containers is a high-level control that requires substantial extrapolation to address these specific Kubernetes-layer threats. Score 2.

### RDL_24 - Profiling and Optimization
- Strength score: 1
The framework does not address profiling, performance optimization, or MLflow artifact management as leakage surfaces. RDL_24_1 (profiling execution traces containing sensitive data), RDL_24_2 (MLflow artifact registry as exfiltration channel), RDL_24_3 (baseline comparison metrics leaking user request patterns), and RDL_24_4 (performance optimization logs as side-channel leakage) are MLOps-layer threats with no corresponding guidance in the NSA/CISA or DHS/CISA documents. Score 1 applies.

### RDL_25 - Kubernetes Orchestration
- Strength score: 1
RDL_25_1 describes Kubernetes audit logs leaking multi-agent orchestration topology through correlated pod creation, scheduling, scaling, and failure events. The framework recommends sandboxed containers and VMs but does not address Kubernetes audit log access controls, the correlation risks of multi-agent scaling and failure patterns, or the specific risk that Kubernetes event logs reveal inter-agent dependency graphs. Score 1 reflects the absence of Kubernetes-specific leakage guidance.

### RDL_26 - GPU and Model Internals
- Strength score: 2
The NSA/CISA framework explicitly addresses GPU security through GPU patching guidance and hardware model weight protection (HRZ/HSM), model weight encryption at rest, and physical security controls. This provides partial coverage for RDL_26_4 (KV cache side-channel leakage through unencrypted tensor parallelism inter-GPU communication) and RDL_26_5 (engine binary metadata leakage enabling model extraction). The framework's hardware-level controls and encryption at rest guidance are the most directly applicable to GPU-layer threats among all RDL subsubsections. However, the framework does not address GPU memory metrics as a leakage channel (RDL_26_1), verbose inference logging writing to centralized Kubernetes logs (RDL_26_2), queue depth metrics enabling workload reverse engineering (RDL_26_3), quantization artifacts as probabilistic leakage channels (RDL_26_6), engine trace forensics (RDL_26_7), or calibration dataset artifacts in quantization statistics (RDL_26_8). Score 2 reflects meaningful but incomplete coverage limited to hardware protection controls.

### RDL_27 - Load Balancing
- Strength score: 1
RDL_27_1 through RDL_27_5 describe side-channel leakage through load balancer routing decisions, dynamic batching window timing, cache hit/miss latency patterns, load balancer metrics exposure, and auto-scaling event timing. The framework does not address load balancing architecture, timing side-channels, or the leakage of system configuration through observable routing and scaling behavior. General TLS guidance could mask some network-level timing but does not prevent timing correlation attacks on response latency patterns. Score 1 is appropriate.

### RDL_28 - Planning Systems
- Strength score: 1
Reasoning and planning security is explicitly identified as a framework gap. RDL_28_1 through RDL_28_7 address HTN (Hierarchical Task Network) planning-specific leakage: context window size as leakage capacity indicator, planning history leakage through task network serialization, method library reconnaissance through registry access, constraint specification revealing resource boundaries, state abstraction mapping revealing information classification structure, decomposition pattern analysis leaking planning strategy, and task completion logging creating execution timeline leakage. The framework contains no guidance on planning system security, method library access controls, or HTN-specific data flows. Score 1 applies.

### RDL_29 - Monte Carlo Tree Search (MCTS)
- Strength score: 1
RDL_29_1 through RDL_29_5 describe MCTS-specific leakage: tree state-action pairs encoding planning history, simulation trajectory logs as probabilistic recall surfaces, value network confidence score disclosure of strategic assessments, rollout policy behavior as implicit information leakage, and replanning trajectory leakage in shared logs. The framework does not address MCTS or any search-based planning algorithm security. These are highly specialized threats requiring algorithm-aware controls that are completely absent from the NSA/CISA and DHS/CISA documents. Score 1 applies.

### RDL_30 - Monitoring and Callbacks
- Strength score: 1
RDL_30_1 describes discrepancy leakage through monitoring callbacks, where agents broadcasting discovered obstacles, failures, and tool unavailability to teammates enumerate exact state space regions and attempted tools. The framework's guidance on logging and monitoring is oriented toward enabling detection, not toward minimizing what monitoring callbacks broadcast to peer agents. No guidance addresses the leakage risk of inter-agent monitoring broadcasts. Score 1 applies.

### RDL_31 - Episodic Memory
- Strength score: 1
Agent memory system security is explicitly identified as a framework gap. RDL_31_1 through RDL_31_6 describe episodic memory as a covert exfiltration mechanism, embedding vector inference enabling probabilistic content reconstruction, consolidated abstractions preserving inferrable episode details, graph relationship traversal for operational pattern reconstruction, deduplication metadata enabling frequency analysis, and temperature/sampling artifacts creating information channels in retrieval. The framework's DHS/CISA footnote 48 mentions access controls for knowledge-bases but provides no guidance on episodic memory architecture, embedding security, graph database access patterns, or deduplication metadata leakage. Score 1 reflects the explicit gap acknowledgment and absence of actionable guidance.

### RDL_32 - Semantic Memory
- Strength score: 2
DHS/CISA footnote 48 specifically identifies cryptographic measures for vector stores and knowledge-bases, data masking for sensitive data, and identity/access controls limiting access to models and inputs. This provides indirect coverage for RDL_32_1 (RAG context window leakage through retrieved documents — access controls and data masking are relevant), RDL_32_2 (knowledge base enumeration — access controls are relevant), and RDL_32_4 (temporal metadata leakage — data minimization principles apply). However, the framework does not address embedding similarity leakage enabling document fingerprinting (RDL_32_3), knowledge graph negative space inference from missing relationships (RDL_32_5), or the specific risk that retrieval patterns reveal knowledge base contents. Score 2 reflects the footnote 48 coverage being meaningful but requiring extrapolation and not addressing the more sophisticated side-channel threats.

### RDL_33 - RAG Leakage
- Strength score: 2
DHS/CISA footnote 48 directly mentions cryptographic measures for prompt stores, caches, vector stores, fine-tuning data, and knowledge-bases, along with data masking for sensitive data and access controls. This is the most directly relevant framework language for RAG-layer threats. The footnote provides partial coverage for RDL_33_1 (RAG relevance scores as probabilistic leakage — access controls and encryption partially apply), RDL_33_5 (semantic-to-working augmentation leakage during knowledge distillation — data masking and access controls are relevant). However, the framework does not address relevance score exploitation specifically (RDL_33_1), query rewriting log exposure of operational intent (RDL_33_2), deduplication artifact leakage (RDL_33_3), or compression validation failures causing hallucinated summaries to propagate as credible knowledge (RDL_33_4). Score 2 reflects footnote 48 as meaningful but incomplete coverage.

### RDL_34 - Utility Functions and Explanations
- Strength score: 1
RDL_34_1 through RDL_34_7 describe leakage through progressive disclosure UI layers exposing multi-agent execution context, chat interface utility parameter disclosure, streaming decision rationale leakage before redaction, error message outcome utility disclosure, inference trace leakage through explanation generation, working memory content exposure through shared persistence, and rule condition inference for information reconstruction. The framework does not address utility function security, explanation generation as a leakage vector, or shared working memory access controls in any specific way. The UI-layer items (RDL_34_1, RDL_34_2) fall under the same UI/UX gap as RDL_1. General access controls from the framework are too generic to constitute meaningful coverage. Score 1 applies.

### RDL_35 - Reinforcement Learning and Policy
- Strength score: 1
MARL/RL training dynamics are explicitly identified as a framework gap. RDL_35_1 through RDL_35_7 describe gradient-based model inversion attacks on learned policies, value function reconstruction revealing training context, black-box policy querying reconstructing training data distributions, experience replay buffer side-channel attacks, reward function reverse engineering, learning curve analysis as information leakage, and context window expansion data leakage in hybrid agent coordination. The framework's supply chain inspection of pre-trained models and adversarial testing guidance are the closest applicable controls but do not address RL-specific attack vectors such as gradient inversion, replay buffer side-channels, or reward function reverse engineering. Score 1 reflects the explicit gap and complete absence of RL-specific guidance.

### RDL_36 - Multi-Agent Coordination
- Strength score: 1
Multi-agent trust exploitation specifics are explicitly identified as a framework gap. RDL_36_1 (multi-agent conversation trace leakage through shared history enabling probabilistic recall of sensitive data) and RDL_36_2 (knowledge graph edge weight leakage via embedding inference of sensitive associations) are coordination-layer threats specific to hybrid multi-agent architectures. The framework does not address shared conversation history security across agent boundaries or knowledge graph weight inference attacks. The DHS/CISA access control guidance is too generic to constitute specific coverage for these coordination-specific leakage mechanisms. Score 1 applies.

# NSA_Deploy_AI_Securely — RIP/RIDC Analysis
## Source lines: sec_threats.tex, lines 555–1063

---

## RIP/RIDC — Agent identity, provenance, and economic/resource attack surface

### RIP_1 - Multi-Agent UI & Dashboard Attacks

- Strength score: 1

RIP_1 covers four threats tied to multi-agent dashboard UIs: identity spoofing through attribution gaps (RIP_1_1), forensics failures preventing attack investigation (RIP_1_2), real-time cost attribution failures enabling economic accountability gaps (RIP_1_3), and GroupChat speaker attribution enabling social engineering (RIP_1_4). The NSA/CISA framework requires logging of inputs, outputs, and intermediate states, and mandates TLS plus MFA for access—these controls are at best tangentially relevant. The framework's logging guidance does not address per-agent attribution at the UI layer, cryptographic signing of agent-generated content, or real-time per-agent cost dashboards. The DHS/CISA document's call for behavior monitoring and access controls is generic and provides no guidance for multi-agent dashboard identity or cost attribution. UI/UX design is explicitly outside the framework's scope. All four RIP_1 threats remain essentially unaddressed.

---

### RIP_2 - Approval & Workflow Attacks

- Strength score: 1

RIP_2 describes five threats centered on provenance and accountability failures in multi-agent approval workflows: lack of cryptographic provenance trails (RIP_2_1), confidence aggregation attacks (RIP_2_2), client-side decision provenance tampering (RIP_2_3), multi-stage provenance gaps (RIP_2_4), and HITL attribution ambiguity (RIP_2_5). The framework recommends immutable backups, RBAC/ABAC, and audit capabilities, and the DHS/CISA document references human supervision and incident response. However, none of these controls speak to cryptographic provenance of per-agent contributions within approval workflows, tamper-evident logging across multi-agent provenance graphs, or the specific problem of attribution ambiguity in HITL chains spanning multiple agent policies. The framework treats approval and oversight at a high conceptual level and does not address multi-agent workflow-specific accountability architecture.

---

### RIP_3 - Command Palette & UI Attacks

- Strength score: 1

RIP_3 identifies three threats: cost visibility gaps in command palettes enabling denial-of-service (RIP_3_1), agent identity confusion in suggestion attribution (RIP_3_2), and rate limiting bypass through agent identity switching (RIP_3_3). The framework addresses API rate limiting and least-privilege access conceptually but does not touch command palette UI design, per-agent cost estimation at the suggestion layer, or agent-aware rate limiting that accounts for differential resource costs across specialized agents. These are application-layer UI/UX and resource-accounting concerns entirely outside the framework's scope.

---

### RIP_4 - Multi-Agent Coordination Attacks

- Strength score: 1

RIP_4 covers four multi-agent coordination threats: reflection-amplified resource exhaustion in swarms (RIP_4_1), orchestrator overload in centralized hierarchies (RIP_4_2), auction manipulation in market-based coordination (RIP_4_3), and broadcast amplification message-flood DoS in event-driven networks (RIP_4_4). The framework recommends sandboxed containers and threat models but provides no guidance on multi-agent coordination patterns, reflection cascade detection, orchestrator bottleneck protection, auction integrity in agent resource markets, or publish-subscribe amplification controls. These are emergent multi-agent architectural vulnerabilities for which the framework offers no relevant controls.

---

### RIP_5 - Framework-Specific Attacks

- Strength score: 1

RIP_5 contains six threats explicitly tied to LangChain, LangGraph, AutoGen, CrewAI, and Semantic Kernel: cross-framework impersonation through identity translation gaps (RIP_5_1), framework-dependent cost attribution enabling denial-of-wallet (RIP_5_2), AutoGen conversational identity confusion (RIP_5_3), CrewAI hierarchical role impersonation (RIP_5_4), AutoGen conversation resumption cost amplification (RIP_5_5), and Semantic Kernel plugin registry context window exhaustion (RIP_5_6). The framework explicitly does not cover framework-specific attacks, and the NSA/CISA document's supply chain inspection guidance addresses pre-trained models and SBOMs rather than framework runtime identity semantics. None of these threats have applicable controls in either document.

---

### RIP_6 - Tool & Plugin Attacks

- Strength score: 2

RIP_6 is the largest subsubsection with fifteen distinct threats (RIP_6_1 through RIP_6_15) covering economic attack surface expansion through tool chaining, tool invocation cost attribution gaps, registry enumeration, execution cost opacity, multi-agent cost attribution complexity, plugin execution identity ambiguity, plugin registry cost attribution, invocation attribution spoofing, privilege inheritance through agent composition, distributed invocation cost exploitation, access token leakage through inter-agent communication, tool failure attribution confusion, profiling chain of custody loss, execution authority delegation in hybrid workflows, and privilege inference and escalation. The framework provides partial but meaningful coverage for a subset: least-privilege access controls map to privilege escalation concerns (RIP_6_9, RIP_6_15); TLS for data in transit and API minimal-return-data principles are relevant to token leakage (RIP_6_11); supply chain inspection of plugins partially addresses registry poisoning (RIP_6_7). However, the multi-agent-specific dimensions—distributed cost attribution, per-agent tool invocation logging, cross-agent privilege composition, and plugin identity ambiguity through delegation chains—require extrapolation beyond what the framework explicitly specifies. The score reflects that relevant controls exist but do not address the multi-agent amplification vectors these threats describe.

---

### RIP_7 - Cost & Economic Denial-of-Service Attacks

- Strength score: 1

RIP_7 is the largest subsubsection, spanning thirty-six items (RIP_7_1 through RIP_7_36) addressing resource attribution ambiguity in statistical agents, verification cost exploitation, policy compliance metric evasion, resource allocation attribution, incentive manipulation, token price volatility exploitation, migration pressure attacks, resource request padding, quota exhaustion, session affinity pseudo-identities, load-balanced cost attribution, reasoning chain provenance gaps, economic exploitation through reasoning amplification, search tree branch provenance spoofing, preserved path provenance spoofing, identity verification gaps in workflow coordination, resource attribution poisoning, economic incentive manipulation in allocation, identity spoofing in decomposition handoff, parallelization overhead attacks, replanning resource exhaustion, episode attribution spoofing, fake experience injection, episode deduplication overhead, storage expansion attacks, consolidation compute cost attacks, embedding cache eviction exploitation, budget allocation authorization confusion, utility function attribution spoofing, resource consumption through expensive utility calculations, economic utility optimization incentivizing resource wastage, persistent volume claim ownership ambiguity, cost-based agent selection vulnerability, resource hijacking through reasoning-guided consumption, multi-connector authentication ambiguity, and incremental update provenance gaps. The framework addresses oracle attack alerts and recommends monitoring/logging broadly, but provides no coverage of token economy attacks, multi-agent cost attribution, reasoning amplification as an economic weapon, episode deduplication exploitation, embedding cache thrashing, budget allocation as an authorization signal, or any of the multi-agent resource market mechanisms described. These threats represent an entirely new economic attack surface unique to LLM-based agentic systems that the 2024 framework does not contemplate.

---

### RIP_8 - Resilience & Error Handling Attacks

- Strength score: 1

RIP_8 describes seven threats: streaming response resource consumption opacity (RIP_8_1), retry budget attribution across multi-agent boundaries (RIP_8_2), fallback provider cost attribution failures (RIP_8_3), circuit breaker resource consumption attribution gaps (RIP_8_4), graceful degradation cost imbalance attribution (RIP_8_5), streaming attribution spoofing (RIP_8_6), and streaming resource consumption opacity enabling economic DoS (RIP_8_7). The framework's logging and monitoring guidance and its immutable backup/rollback provisions address resilience at a broad level but do not touch multi-agent retry budget management, fallback provider cost accounting, circuit breaker overhead attribution, or the unique streaming attack surface where early tokens establish identity trust while later tokens exploit it. These are application-level resilience engineering concerns absent from the framework.

---

### RIP_9 - Multimodal Attacks

- Strength score: 1

RIP_9 presents four threats: multimodal content attribution spoofing (RIP_9_1), vision model resource exhaustion through multimodal DDoS (RIP_9_2), audio processing resource exploitation (RIP_9_3), and embedding vector store resource amplification through multimodal scale (RIP_9_4). The framework's GPU patching requirement and sandboxed deployment guidance are tangentially relevant to infrastructure hardening, and input sanitization covers some prompt injection vectors, but multimodal-specific attack vectors—DDoS via coordinated vision/audio model invocations, cross-modality attribution spoofing, vector store scale amplification through multimodal embedding—are not addressed. The framework predates widespread multimodal agentic deployments and offers no modality-specific controls.

---

### RIP_10 - Reasoning & Evaluation Attacks

- Strength score: 1

RIP_10 contains sixteen threats (RIP_10_1 through RIP_10_16) targeting evaluation cost attribution, evaluation agent identity spoofing, artifact provenance opacity, excessive evaluation cycles, evaluation result attribution spoofing, evaluation framework resource consumption, benchmark attribution spoofing, quality score identity confusion, sampling budget resource allocation attacks, difficulty classification economic exploitation, MCTS computational budget exhaustion, multi-pattern cost amplification through ReAct-Reflection interaction, evaluation authority delegation to compromised agents, offline vs. online evaluation distribution gap exploitation, non-determinism exploitation in pass@K evaluation, and few-shot metric interpretation poisoning. The framework endorses adversarial testing and red-teaming at a general level, and the DHS/CISA guidance references dataset/model validation, but none of these provisions address evaluation pipeline security itself as an attack surface—specifically the threats of evaluation authority delegation, metric definition injection, benchmark ground truth poisoning, or ReAct-Reflection cost amplification. Reasoning and planning security is explicitly outside the framework's scope.

---

### RIP_11 - Vector Store & RAG Attacks

- Strength score: 2

RIP_11 covers thirteen threats (RIP_11_1 through RIP_11_13): cost-per-query parameter exploitation, resource consumption accounting attacks, knowledge base source attribution loss, replication cost attacks, indexing infrastructure cost exploitation, vector database partition escaping, query load amplification, knowledge graph authorization ambiguity, embedding API endpoint provenance confusion, model version registry ambiguity, shared API key provenance loss, database selection provider lock-in obscuring provenance, and ETL source attribution loss. The framework provides partial coverage: Footnote 48 of the DHS/CISA document explicitly calls out cryptographic measures for vector stores, prompt stores, caches, fine-tuning data, and knowledge bases—directly relevant to RIP_11_6 (partition escaping), RIP_11_9 (endpoint provenance), and RIP_11_11 (shared API key provenance). Supply chain inspection guidance partially addresses RIP_11_10 (model version registry). Input sanitization is tangentially relevant to retrieval pipeline attacks. However, the multi-agent amplification dimensions—cross-agent query load amplification, ETL provenance stripping, replication cost attacks, and knowledge graph authorization across agent boundaries—require significant extrapolation beyond the framework's explicit guidance.

---

### RIP_12 - Memory & Session Attacks

- Strength score: 1

RIP_12 presents eleven threats: session persistence context pollution (RIP_12_1), checkpoint attribution ambiguity (RIP_12_2), economic cycling attacks via unbounded iteration (RIP_12_3), memory persistence cost overhead enabling denial-of-wallet (RIP_12_4), MCTS tree growth memory consumption (RIP_12_5), semantic memory cost optimization exploitation (RIP_12_6), working memory consumption via context inflation (RIP_12_7), shared semantic memory creating attribution confusion (RIP_12_8), cross-agent context poisoning through session persistence (RIP_12_9), memory serialization storage cost amplification (RIP_12_10), and model checkpoint integrity attacks (RIP_12_11). The framework notes that agent memory system security is explicitly outside its scope beyond general access controls, and the framework's hardware model weight protection guidance (HSM/HRZ) is tangentially relevant to RIP_12_11. No meaningful coverage exists for cross-agent context poisoning, semantic memory provenance opacity, episodic storage economic attacks, or multi-agent checkpoint attribution. These represent agentic memory-specific attack surfaces the framework does not contemplate.

---

### RIP_13 - Infrastructure & Deployment Attacks

- Strength score: 2

RIP_13 includes nine threats: container escape enabling agent-to-agent attacks (RIP_13_1), infrastructure cost manipulation (RIP_13_2), NIM container image version ambiguity and provenance uncertainty (RIP_13_3), replica identity loss through generic pod template scaling (RIP_13_4), cost attribution blurring in shared GPU infrastructure (RIP_13_5), provenance loss in distributed engine caching (RIP_13_6), resource contention in shared GPU deployments (RIP_13_7), horizontal scaling creating replica identity ambiguity (RIP_13_8), and engine binary non-fungibility breaking agent identity (RIP_13_9). The NSA/CISA framework explicitly covers sandboxed containers and VMs, GPU patching, and threat models for deployment environments—these directly address container isolation (RIP_13_1) and GPU infrastructure management (RIP_13_7). Cryptographic artifact validation upon deployment addresses image provenance concerns (RIP_13_3, RIP_13_6). However, Kubernetes-specific replica identity ambiguity, cost attribution blurring across shared GPU resources, and engine binary non-fungibility are not explicitly addressed and require extrapolation from general infrastructure hardening guidance.

---

### RIP_14 - ML/Training & Model Attacks

- Strength score: 2

RIP_14 covers eight threats: model selection as economic privilege boundary creating identity spoofing (RIP_14_1), MLflow model registry identity spoofing (RIP_14_2), quantization artifact provenance verification gaps (RIP_14_3), learned agent identity through behavioral fingerprinting (RIP_14_4), training data provenance obfuscation (RIP_14_5), model ownership disputes through learning-based derivation (RIP_14_6), computational cost emergence through learning efficiency (RIP_14_7), and training resource acquisition attacking downstream agents (RIP_14_8). The framework's supply chain inspection of pre-trained models and the DHS/CISA guidance on SBOMs, AIBOMs, model cards, and data cards provide meaningful but incomplete coverage: these directly address RIP_14_2 (model registry) and RIP_14_5 (training data provenance) at the supply chain level. Cryptographic artifact validation maps to quantization artifact verification (RIP_14_3). However, behavioral fingerprinting for identity spoofing, model ownership disputes in federated learning, and MARL/RL training dynamics (RIP_14_7, RIP_14_8) are explicitly outside the framework's scope and receive no applicable guidance.

---

### RIP_15 - Kubernetes & Container Attacks

- Strength score: 2

RIP_15 presents seven threats: Kubernetes service account identity spoofing enabling lateral movement (RIP_15_1), IP spoofing via iptables manipulation (RIP_15_2), pod eviction ordering revealing agent importance (RIP_15_3), GPU resource request gaming for hardware hoarding (RIP_15_4), namespace-based multi-tenancy isolation bypass (RIP_15_5), cluster node identity spoofing in gossip protocol communications (RIP_15_6), and load balancer VIP masking provenance in query attribution (RIP_15_7). The NSA/CISA framework's requirements for sandboxed containers, ZT architecture, least-privilege access, and TLS provide relevant foundations: ZT principles apply to service account token controls (RIP_15_1), network policy controls address IP spoofing (RIP_15_2), and namespace isolation aligns with multi-tenancy bypass risks (RIP_15_5). However, Kubernetes-specific Gossip protocol node identity verification, VIP-based provenance masking in vector database clusters, and pod priority-based agent importance inference are not explicitly covered and require extrapolation from general container security guidance.

---

### RIP_16 - Load Balancer & Scaling Attacks

- Strength score: 1

RIP_16 describes four threats: load balancer identity abstraction enabling agent impersonation (RIP_16_1), load balancer endpoint as impersonation target (RIP_16_2), weighted load balancing creating privilege asymmetry observable through traffic patterns (RIP_16_3), and auto-scaling triggered identity discontinuity (RIP_16_4). The framework's TLS requirement applies generically to load balancer traffic, and MFA/ZT principles are tangentially relevant. However, load balancer-specific identity abstraction attacks, traffic pattern-based privilege inference, and auto-scaling identity discontinuity as an attack vector have no specific coverage in either NSA/CISA document. These are infrastructure architecture concerns that the framework does not address at the required specificity.

---

### RIP_17 - Service Discovery & Authentication Attacks

- Strength score: 2

RIP_17 covers seven threats: service registration audit trail gaps (RIP_17_1), message queue consumer identity spoofing (RIP_17_2), API gateway consumer identity header injection for rate limit bypass (RIP_17_3), Prometheus cardinality explosion DoS (RIP_17_4), message queue priority queue starvation (RIP_17_5), DNS cache poisoning affecting multi-agent service discovery (RIP_17_6), and certificate authority compromise enabling agent spoofing at scale (RIP_17_7). The framework's ZT architecture with MFA, TLS, and cryptographic artifact validation provides meaningful coverage for several threats: certificate-based authentication governance is directly relevant to RIP_17_7, TLS protects against DNS poisoning in transit (RIP_17_6), and audit/logging requirements address service registration gaps (RIP_17_1). However, message queue consumer identity spoofing, Prometheus cardinality explosion as a DoS vector, and API gateway header injection for rate limit bypass are multi-agent infrastructure-specific threats that the framework does not address at the operational level required.

---

### RIP_18 - Rule/Method/Parameter Attribution Attacks

- Strength score: 1

RIP_18 presents ten threats: efficiency metric spoofing for identity deception (RIP_18_1), method authorship spoofing in shared registries (RIP_18_2), delegation chain identity verification gaps (RIP_18_3), rule attribution confusion in multi-agent aggregation (RIP_18_4), parameter origin confusion in multi-agent systems (RIP_18_5), parameter source trust boundaries (RIP_18_6), provider economic lock-in through efficiency contracts (RIP_18_7), method contributor accountability in distributed development (RIP_18_8), rule authorship spoofing through description manipulation (RIP_18_9), and rule version control poisoning (RIP_18_10). These threats are deeply specific to HTN planning, rule-based reasoning, and multi-agent delegation chain verification—areas entirely outside both NSA/CISA documents' scope. The framework's least-privilege and RBAC principles tangentially relate to trust boundary enforcement (RIP_18_6), but the multi-agent-specific mechanisms of delegation chain verification, shared rule registry provenance, and parameter attribution across agent boundaries have no applicable coverage.

---

### RIP_19 - Hybrid & Cross-Paradigm Attacks

- Strength score: 1

RIP_19 describes two threats: paradigm attribution confusion in hybrid component source validation (RIP_19_1) and identity spoofing through paradigm-specific credential mechanisms (RIP_19_2). These threats target hybrid AI architectures combining neural models, symbolic rules, and utility functions—a multi-paradigm composition scenario that the NSA/CISA framework does not address. The supply chain guidance for pre-trained models and SBOMs provides a distant analog for component provenance validation, but the paradigm-specific credential diversity and cross-paradigm identity translation gaps described in RIP_19 have no explicit coverage in either document.

---

## RIDC — Instruction-Data Conflation (Prompt Injection) Threats

### RIDC_1 - UI and User Interaction Attack Surfaces

- Strength score: 2

RIDC_1 presents eight threats exploiting UI-level instruction-data conflation: progressive disclosure as attack surface expansion (RIDC_1_1), chat interface attribution confusion enabling social engineering (RIDC_1_2), ARIA live region manipulation as persistent context injection (RIDC_1_3), keyboard shortcut accessibility as out-of-band instruction channel (RIDC_1_4), accessibility semantic landmarks as cross-agent identity spoofing (RIDC_1_5), command palette context prediction as covert instruction channel (RIDC_1_6), error communication auto-retry as attack amplification (RIDC_1_7), and notification pattern time-based default escalation as injection vector (RIDC_1_8). The NSA/CISA framework's explicit requirement for input sanitization and prompt injection protection is the primary relevant control, providing direct but generic coverage of the injection mechanism. Logging of inputs and intermediate states is relevant to attack investigation. However, the multi-agent-specific amplification vectors—cross-disclosure-level policy gaps, cross-agent ARIA context pollution, transitive trust chains in timeout-based approvals, and UI-layer identity spoofing across multiple agents—are not addressed with any specificity. The score reflects that input sanitization is a real control but requires substantial extrapolation for UI-layer multi-agent attack surfaces.

---

### RIDC_2 - Messaging and Protocol Injection

- Strength score: 1

RIDC_2 covers three threats: FIPA-ACL performative spoofing and communication layer injection (RIDC_2_1), protocol language downgrade attacks via serialization format conflation (RIDC_2_2), and conversation-ID chain hijacking for context pollution (RIDC_2_3). The framework's TLS requirement protects transport-layer confidentiality, and input sanitization is tangentially relevant to message content validation. However, agent communication protocol security—FIPA-ACL semantic integrity, serialization format downgrade attack prevention, and conversation-ID chain authentication—is not addressed in either NSA/CISA document. These threats exploit agentic communication protocol semantics that the framework does not contemplate.

---

### RIDC_3 - Framework and State Management Injection

- Strength score: 1

RIDC_3 contains four threats: framework-enforced trust boundary violations through state schema coercion (RIDC_3_1), cross-framework state migration enabling transitive instruction propagation (RIDC_3_2), state schema field injection via reducer confusion (RIDC_3_3), and conditional edge routing hijacking via state manipulation (RIDC_3_4). These threats are specific to LangGraph, LangChain, CrewAI, and AutoGen state management models—framework-specific attacks that are explicitly outside the NSA/CISA framework's scope. Input sanitization is the only tangentially relevant control, but it does not address cross-framework state transformation points where malicious payloads survive semantic boundary changes. No meaningful coverage exists.

---

### RIDC_4 - Tool, Function Calling, and Plugin Injection

- Strength score: 2

RIDC_4 presents ten threats (RIDC_4_1 through RIDC_4_10): memory-driven tool selection manipulation, tool-calling schema instruction boundary collapse, tool description injection through RAG-retrieved metadata, function calling parameter injection via type coercion, implicit tool calling assumption violations across agent boundaries, plugin-function metadata injection through decorated function descriptions, semantic function template injection, native function parameter type coercion as control-flow attack, plugin discovery protocol spoofing through crafted function schemas, and tool schema evasion through boundary misinterpretation. The framework explicitly addresses input sanitization and prompt injection protection, which directly applies to the core mechanism underlying all ten threats. Supply chain inspection of plugins is relevant to RAG-retrieved tool metadata (RIDC_4_3) and plugin registry poisoning (RIDC_4_6). API minimal-return-data principles partially address schema boundary collapse. The multi-agent amplification—where Agent A's poisoned output launders into Agent B's trusted context, and shared registries propagate injections to all agents simultaneously—requires extrapolation. The framework's prompt injection guidance is real but was not designed for multi-agent function-calling chains.

---

### RIDC_5 - ReAct and Reasoning Architecture Injection

- Strength score: 1

RIDC_5 covers eleven threats (RIDC_5_1 through RIDC_5_11): ReAct reasoning trace injection, trace artifacts as post-hoc rationalization injection, mechanistic interpretability opacity creating cross-agent blind spots, plan-and-execute planning phase conflation, precondition smuggling in abstract task decomposition, abstract task description as implicit instruction channel, constraint specification ambiguity enabling ordering manipulation, effects specification injection through state change description, data dependency injection through causal link manipulation, method proliferation enabling hidden instruction encoding, and reflection self-critique reinforcement hijacking. The framework's input sanitization and prompt injection protection is tangentially relevant to the core injection mechanism, but reasoning and planning security is explicitly outside the framework's scope. ReAct trace sharing, HTN method library injection, and reflection hijacking in dual-agent critic patterns are emergent multi-agent reasoning vulnerabilities that neither NSA/CISA document addresses with any meaningful specificity.

---

### RIDC_6 - Streaming and Real-Time Communication Injection

- Strength score: 1

RIDC_6 presents one threat: streaming response manipulation through timing attacks (RIDC_6_1), where multi-agent streaming handoffs create multiple injection windows exploiting non-deterministic generation speeds. The framework's logging of inputs, outputs, and intermediate states is the most relevant control, providing some post-hoc detection capability. TLS protects the streaming channel in transit. However, the specific attack surface of timed injection into streaming multi-agent handoffs—where attackers embed malicious instructions in middle sections of long streaming responses that subsequent agents consume before humans review—has no specific coverage. The framework's monitoring guidance does not address real-time streaming injection detection.

---

### RIDC_7 - Generation Parameter and Inference Configuration Injection

- Strength score: 2

RIDC_7 presents ten threats (RIDC_7_1 through RIDC_7_10): evaluation dataset poisoning through training data injection, evaluation metric selection manipulation via prompt-based metric design, baseline measurement manipulation through agent substitution, multi-agent evaluation orchestration control-flow hijacking, test dataset ordering attacks, evaluation context memory poisoning, metric calculation hijacking through custom metric function injection, confidence score inflation in custom evaluation metrics, baseline metric manipulation for regression detection evasion, and benchmark ground truth injection as evaluation poison. The DHS/CISA framework's guidance on dataset validation and adversarial training provides partial coverage for evaluation dataset poisoning (RIDC_7_1) and benchmark ground truth injection (RIDC_7_10). The NSA/CISA document's adversarial testing requirement acknowledges evaluation integrity as a concern. Supply chain review and model/data cards are tangentially relevant to dataset provenance. However, multi-agent evaluation orchestration control-flow hijacking, metric definition injection, confidence score inflation in custom metrics, and baseline manipulation for regression evasion are specific evaluation pipeline attack vectors that the framework does not address explicitly. The score reflects that dataset/model validation guidance exists but does not extend to the multi-agent evaluation infrastructure attack surface described.

## RMP — Long-lived cognitive state abuse: memory poisoning and latent backdoors

### RMP_1 - UI/UX Memory Manipulation Attacks
- Strength score: 1

The NSA/CISA framework does not address UI/UX design threats at all, and the eleven sub-threats in RMP_1 are all rooted in how conversation history displays, session persistence panels, progressive disclosure UI layers, and command palette suggestions create provenance opacity and latent backdoor activation paths in multi-agent systems. The framework's closest relevant controls are input sanitization (protecting against prompt injection) and logs of inputs/outputs/intermediate states, but neither speaks to the UI rendering of memory provenance, cross-session state revalidation on resumption, or the absence of security context boundaries between session threads. The DHS/CISA guidance on human supervision and behavior monitoring is generically relevant but provides no actionable guidance on conversation history provenance labeling, context panel metadata validation, or the multi-agent dashboard isolation failures described here. RMP_1 is fundamentally a UI/UX design category that the framework explicitly does not cover.

### RMP_2 - Multi-Agent State Handoff and Cross-Agent Memory Attacks
- Strength score: 1

RMP_2 describes a cluster of highly specific multi-agent architectural attacks: API gateway routing manipulation for RCE, gRPC schema versioning backward-compatibility exploitation, swarm intelligence local rule injection for emergent malicious behavior, cross-type memory poisoning via retrieval mechanism exploitation, adversarial importance weighting for selective memory retention, inter-agent memory type misclassification during handoff, similarity-search poisoning via semantic anchor injection, cross-agent vector embedding poisoning, knowledge graph relationship injection, and distributed graph traversal race conditions. The NSA/CISA framework addresses general least-privilege access controls, data source catalogs, cryptographic artifact validation, and input sanitization — none of which engage with the handoff serialization boundary, memory type metadata degradation across agent chains, swarm emergent-behavior validation, or HNSW/knowledge-graph-level retrieval manipulation described here. The DHS footnote 48 on cryptographic measures for vector stores and prompt caches is the closest relevant control but applies only to access protection, not to the semantic-level embedding and type-confusion attacks in RMP_2. The framework provides no guidance on multi-agent protocol schema validation, emergent behavior monitoring in swarm systems, or retrieval-mechanism-level integrity.

### RMP_3 - Multimodal and Cross-Modal Poisoning Attacks
- Strength score: 1

RMP_3 covers cross-modal instruction injection through non-text modalities (images, audio, sensor data), temporal sensor desync exploitation, multi-agent modality contradiction attacks, distributed memory poisoning via perception corruption, vision model output memory integration creating cross-session instruction persistence, multimodal RAG chunk poisoning, audio transcript memory corruption through speech recognition hallucinations, chart linearization instruction injection (DePlot), HTML entity encoding confusion across heterogeneous parsers, and CSS pseudo-element content injection. The NSA/CISA framework's input sanitization guidance is focused on text-based prompt injection and does not extend to multimodal inputs, cross-modal fusion layers, modality-gap exploitation in RAG chunks, or the extraction/interpretation separation in multi-agent visual pipelines. The DHS dataset validation controls address training-time filtering but not inference-time multimodal poisoning. The framework provides no actionable guidance on multimodal content validation, vision model output sanitization before memory integration, or cross-agent modality contradiction detection — all of which are central to RMP_3.

### RMP_4 - Framework-Specific Memory Vulnerabilities
- Strength score: 1

RMP_4 enumerates seventeen highly specific vulnerabilities tied to named AI frameworks (LangChain, LangGraph, AutoGen CrewAI, Semantic Kernel): cross-framework memory architecture mismatches at transformation boundaries, session resumption state propagation, checkpoint-based dormant backdoor installation, append-only reducer state history manipulation, ConversationBufferMemory poisoning, agent scratchpad poisoning, memory key namespace collision in shared backends, unsanitized tool output integration into scratchpads, AutoGen GroupChat shared history poisoning, CrewAI task context inheritance propagation, CrewAI manager delegation memory exploitation, Semantic Kernel cross-plugin context persistence, service registration state poisoning, semantic function prompt cache injection, tool parameter template injection via shared memory, tool availability state corruption, and learned tool invocation pattern backdoors. The NSA/CISA framework contains no guidance on AI agent orchestration framework security, scratchpad isolation, namespace management in shared memory backends, or framework-specific checkpoint security. The supply chain inspection guidance applies to pre-trained models, not to framework configuration artifacts. This entire subsubsection falls outside the framework's scope.

### RMP_5 - Caching and Persistence Attacks
- Strength score: 2

RMP_5 covers shared embedding cache poisoning, error recovery payload persistence through retry cycles, fallback state caching as long-term cognitive corruption, error message storage as backdoor activation, retry state history as cognitive corruption, and graceful degradation configuration persistence creating behavioral injection. The NSA/CISA framework partially addresses this through DHS footnote 48, which explicitly calls for cryptographic measures for prompt stores, caches, vector stores, fine-tuning data, and knowledge bases — a direct, if brief, control relevant to embedding cache and semantic cache integrity. The NSA document also covers immutable backups and rollback to last known good state, which partially addresses the fallback and cache corruption scenarios. However, the framework does not address error-recovery memory persistence, retry-cycle state exploitation, or degradation configuration poisoning specifically. The cryptographic cache controls require significant extrapolation to cover the full scope of RMP_5's multi-agent shared cache amplification dynamics, hence a moderate score rather than strong.

### RMP_6 - Streaming and Real-time Processing Attacks
- Strength score: 1

RMP_6 describes streaming-specific memory injection: storing tokens before stream validation completes, hiding persistence instructions in later-streamed content that memory-writing agents process incrementally, exploiting multi-agent memory synchronization timing windows through streaming speed manipulation, and capturing mid-stream injections in conversation history at handoff buffering boundaries. The NSA/CISA framework's input sanitization guidance is batch-oriented and does not address the partial-validation window created by streaming token delivery. Log monitoring of inputs/outputs applies after completion, not during incremental stream ingestion. No guidance exists in the framework on streaming validation pipelines, distributed memory synchronization timing, or multi-agent streaming handoff buffer security. These are infrastructure-level streaming architecture concerns entirely absent from the framework.

### RMP_7 - Evaluation and Metrics Poisoning
- Strength score: 1

RMP_7 addresses poisoning of shared evaluation infrastructure across multi-agent fleets: baseline persistence poisoning, adaptive metric latent backdoors through learned pattern injection, cached evaluation result replay attacks, evaluation scratchpad poisoning to fabricate test records, mutable evaluation dataset progressive corruption, evaluation session state persistence enabling bypass, metric aggregation state manipulation, benchmark performance history as persistent cognitive corruption, learned benchmark artifact propagation replicating overfitting biases, and confidence calibration cascade degradation. The NSA/CISA framework's adversarial testing and pen testing guidance is relevant at a high level — the framework recommends testing AI systems — but it is entirely silent on protecting the integrity of the evaluation and benchmarking infrastructure itself. The DHS data validation controls focus on training datasets, not evaluation datasets or metric aggregation state. The framework provides no guidance on immutable evaluation baselines, scratchpad isolation for evaluation agents, or calibration integrity across multi-agent chains.

### RMP_8 - Parameter Tuning and Configuration Poisoning
- Strength score: 1

RMP_8 covers image metadata injection targeting vision model extraction (EXIF/XMP/steganographic), model-specific memory architecture mismatches during model swapping in parameter tuning, context window truncation exploiting heterogeneous window sizes across agents, shared tuning dataset poisoning with benchmark artifact triggers, hyperparameter configuration persistence corruption in shared backends, and Pareto frontier analysis manipulation through baseline poisoning. The NSA/CISA framework addresses data source catalogs and data integrity in training pipelines at a general level, and the DHS guidance includes dataset validation and filtering. However, the framework does not address image metadata as an injection vector, cross-model state semantic translation attacks, context window size heterogeneity exploitation, hyperparameter backend security, or configuration-level Pareto frontier manipulation. These are tuning-pipeline-specific and multi-agent configuration concerns beyond the framework's scope.

### RMP_9 - Learning and Training Data Attacks
- Strength score: 2

RMP_9 describes few-shot chain-of-thought demonstration poisoning in plan-and-execute architectures, trajectory data injection in reinforcement learning flywheels where Agent A's trajectories become Agent B's few-shot examples, and fine-tuning data contamination through poisoned example selection heuristics. The NSA/CISA framework addresses data poisoning and backdoor attacks directly: the deployment environment phase explicitly protects against data poisoning through trusted/valid data source catalogs, and the DHS guidance specifies dataset validation with filtering of poisoned data examples, expert-annotated datasets, model hardening, and adversarial training as mitigations. DHS footnote 48 includes cryptographic measures for fine-tuning data. These controls are genuinely relevant to RMP_9's training data contamination scenarios. However, the framework does not specifically address few-shot CoT demonstration security, RL trajectory flywheel poisoning dynamics, or the multi-agent learning chain compounding effect — these require extrapolation from the general data poisoning mitigations.

### RMP_10 - Observability and Tracing Attacks
- Strength score: 1

RMP_10 covers clock skew exploitation to corrupt trace causality in distributed systems, span metadata poisoning to make critical events appear as non-error spans filtered from oversight views, multi-agent root cause analysis inference failures exploited by maximally separating symptom from cause, and confidence score propagation poisoning through cross-agent trace manipulation. The NSA/CISA framework mandates logs of inputs, outputs, and intermediate states, and the DHS guidance includes behavior monitoring and incident response — controls that depend on trace integrity. However, neither document addresses the security of the tracing infrastructure itself: clock synchronization for causal integrity, span metadata validation, cross-agent observability attack surfaces, or the challenge of root cause attribution in distributed multi-agent traces. The framework assumes observability infrastructure is trustworthy without specifying how to secure it.

### RMP_11 - Learned Behavior and Memory Evolution
- Strength score: 1

RMP_11 addresses hallucinated tool parameters stored in shared multi-agent memory persisting as "historical evidence" that causes other agents to repeat hallucinations as grounded facts, and memory consolidation/deduplication exploitation where evolved consolidated memory contains injected instructions disguised as learned patterns. The NSA/CISA framework has no guidance on memory consolidation pipeline security, deduplication logic integrity, or the multi-agent hallucination propagation dynamic where one agent's confabulation becomes another agent's factual prior. The general data validation controls from DHS address incoming training data quality but not the ongoing evolution of shared operational memory within a deployed agent fleet.

### RMP_12 - Context and Parameter Consistency
- Strength score: 1

RMP_12 covers memory slice attacks through selective history retrieval targeting agent-specific query patterns, long-term cross-session memory poisoning in multi-user multi-agent systems, poisoned parameter history persistence in shared conversation memory, parameter accuracy regression cascading across multi-turn multi-agent sessions, tool selection consistency loss across agent boundaries creating parameter mismatches, and parameter validation state information leakage — where downstream agents cannot distinguish validated from unvalidated parameters because validation provenance is lost at memory boundaries. The NSA/CISA framework's least-privilege and RBAC/ABAC controls provide access boundary guidance, and input sanitization is nominally relevant, but neither document addresses cross-agent parameter validation provenance tracking, memory slice attack vectors, or the cascade coherence degradation in multi-turn multi-agent sessions. These are multi-agent runtime consistency concerns the framework does not engage with.

### RMP_13 - Reasoning Transparency and Trust
- Strength score: 1

RMP_13 describes reasoning transparency weaponization — where explicit reasoning traces create stronger social engineering vectors because users trust transparent logic — injection of content crafted to produce scientifically-appearing but dangerous reasoning chains, multi-agent reasoning disagreement exploitation through differential payload injection to steer user trust toward malicious recommendations, and reasoning confidence calibration attacks where false confidence metrics propagate downstream. The NSA/CISA framework does not address reasoning transparency as a security surface, the manipulation of visible reasoning chains, or the dynamics of multi-agent disagreement as an attack vector. Human supervision is recommended but without any recognition that the supervision surface itself (reasoning display) can be weaponized. These are reasoning process integrity concerns entirely outside the framework's scope.

### RMP_14 - Efficiency and Resource Tracking
- Strength score: 1

RMP_14 covers poisoned historical efficiency data in shared memory causing misallocation across agents, history summarization lossy compression exploited so injected instructions survive compression while benign context is pruned, gradual efficiency baseline degradation activating hidden payloads, cost attribution memory poisoning corrupting orchestration routing decisions, and efficiency insight persistence as learned backdoors redirecting agent optimization. The NSA/CISA framework provides no guidance on efficiency tracking memory integrity, history summarization security, cost attribution data protection, or resource-based routing manipulation. These are operational telemetry and orchestration concerns that fall entirely outside the framework's scope.

### RMP_15 - Vector Database and Embedding Poisoning
- Strength score: 2

RMP_15 describes cross-provider embedding space incompatibility exploitation, cached embedding corruption through compromised or dimension-mismatched models injected into shared vector stores, HNSW graph structure poisoning through efConstruction parameter manipulation during index rebuilds, persistent volume-level HNSW graph file tampering, and cluster replication protocol poisoning propagating corrupted vectors across all replicas. The NSA/CISA framework's DHS footnote 48 explicitly calls for cryptographic measures for vector stores, which is directly relevant to embedding cache integrity and volume-level tampering detection. Immutable backups and rollback to last known good state address the persistence and recovery dimension of volume poisoning. Supply chain inspection guidance is tangentially relevant to embedding model provenance. However, the framework does not address HNSW graph parameter security during index rebuilds, cross-provider embedding space validation, or replication protocol semantic verification — these require significant extrapolation from the generic cryptographic store controls.

### RMP_16 - ETL Pipeline and Data Processing Attacks
- Strength score: 2

RMP_16 covers ETL state file timestamp backdating for incremental update corruption, quality validation threshold degradation through configuration drift, chunking overlap poisoning for cross-chunk context contamination, semantic cache entry poisoning through adversarial embedding optimization against cosine similarity thresholds, response cache corruption through query normalization collision attacks, quality threshold progressive degradation through institutional memory loss, deduplication logic manipulation, persistent state file poisoning forcing historical document reprocessing, citation database poisoning with fabricated source attributions, batch processing queue contamination through poisoned document injection affecting co-processed legitimate documents, and MinHash fuzzy deduplication hash collision injection for training data substitution at scale. The NSA/CISA framework's data source catalog, trusted/valid data source protections against data poisoning, and DHS dataset validation with filtering are directly relevant to the training/fine-tuning data contamination scenarios in this subsubsection. DHS footnote 48's cryptographic measures for knowledge bases and fine-tuning data address the persistence and integrity dimension of ETL pipeline outputs. Immutable backups provide recovery for state file tampering. However, the framework provides no specific guidance on ETL state file integrity, deduplication threshold monitoring, chunking overlap security, cache key collision attacks, or MinHash collision exploitation — all of which require substantial extrapolation from the general data integrity controls.

# NSA Deploy AI Securely — RND Subsection Analysis

## RND — Non-determinism, continual change, and assurance gaps as a risk surface

### RND_1 - UI/Streaming and Real-Time Generation

**RND_1_1 - Streaming Response Non-Determinism Defeating Audit Reproducibility**
- Strength score: 1

The NSA/CISA framework requires logging of inputs, outputs, and intermediate states, and recommends running full evaluations before redeployment. However, it does not address the fundamental problem of streaming non-determinism: that stochastic token sampling and variable network buffering produce different intermediate UI states across executions, making audit replay unreliable. The framework's log-and-monitor approach assumes deterministic reproducibility, which streaming architectures structurally cannot provide. The multi-agent amplification (Agent A's partial output consumed by Agent B before safety filtering completes) is entirely unaddressed.

**RND_1_2 - Streaming Latency Non-Determinism Enabling Temporal Exploitation**
- Strength score: 1

The framework mentions monitoring for high-frequency and repetitive inputs and recommends TLS for transport security, but says nothing about latency variability as an attack surface. The combinatorial ordering problem in multi-agent streaming (N agents × M connections producing many execution orderings) is not considered. Timing side-channels, sleeper agent activation under production-specific latency conditions, and cache manipulation to force specific agent-state combinations are all absent from the framework's scope.

**RND_1_3 - Streaming Token Sampling Creating Ephemeral Vulnerability Windows**
- Strength score: 1

The framework calls for input sanitization and prompt injection protection and adversarial testing, but it does not address stochastic sampling parameters (temperature, top-k, top-p) as security variables. The threat that an attacker with model access can probe the sampling parameter space to maximize harmful token probability, while remaining invisible in deterministic test configurations, is not considered. The framework's adversarial testing guidance implicitly assumes deterministic or near-deterministic behavior.

**RND_1_4 - Inline Suggestion Real-Time Generation Creating Ephemeral Malicious Content**
- Strength score: 1

The framework covers input sanitization and output monitoring through logs, but the race-condition window between generation agents and safety-filtering agents in asynchronous inline code suggestion is not addressed. The monitoring blind spot created by unlogged unaccepted suggestions is a specific architectural gap not anticipated by any framework control. The structural difficulty of closing this multi-agent race in distributed pipelines is unaddressed.

---

### RND_2 - Progressive Disclosure and Error Handling

**RND_2_1 - Progressive Disclosure State Transitions as Untestable Attack Surface**
- Strength score: 1

The framework does not address UI progressive disclosure patterns at all. Controls around sandboxed containers, least privilege, and adversarial testing do not extend to the exponential state-space problem of multilayer disclosure UI combined with multi-agent systems. Exhaustive validation of layer combinations is not addressed anywhere in the framework.

**RND_2_2 - Error Message Progressive Disclosure Hiding Attack Pattern Evidence**
- Strength score: 1

The framework recommends maintaining logs of inputs, outputs, and intermediate states, which is directionally relevant, but it does not contemplate the forensic blind spot created by progressive error disclosure where critical evidence resides in collapsed technical layers. The O(2^NM) attribution complexity in multi-agent error chains and deliberate error-message mimicry as an obfuscation technique are not addressed.

**RND_2_3 - Streaming Error Handling Creating Ephemeral State Non-Determinism**
- Strength score: 1

The framework's log-based monitoring approach assumes errors are observable and persistent, which is precisely what ephemeral streaming error states violate. The multi-agent cascade structure (Agent A errors, Agent B recovers, Agent C consumes unaware) creating variable error-visibility paths that defeat forensic reconstruction is not addressed. No framework control targets transient error non-determinism in streaming pipelines.

---

### RND_3 - Context, Persistence, and Session Management

**RND_3_1 - Session State Non-Determinism Breaking Multi-Turn Attack Detection**
- Strength score: 2

The framework recommends input sanitization and prompt injection protection, logs of inputs/outputs/intermediate states, and monitoring for anomalous patterns, which partially address multi-turn attack detection. The DHS/CISA document mentions data drift monitoring and adversarial training. However, neither document addresses the specific vulnerability of distributed malicious intent across conversation turns defeating stateless safety classifiers, cross-agent memory contagion (contagious jailbreaks), or the empirical 80-95%+ success rates of multi-turn attacks against safety-aligned models. The framework provides generic controls that require significant extrapolation to partially address this threat.

**RND_3_2 - Session Persistence Across UI State Changes Creating Stale Security Context**
- Strength score: 1

The framework does not address session persistence across UI state changes or stale security context propagation. The multi-agent scenario where Agent A uses 2-hour-old cached context while Agent B retrieves fresh context, creating divergent trust models, is entirely outside the framework's scope. Cache freshness as a security property is not mentioned.

---

### RND_4 - Multi-Agent Coordination and Decision-Making

**RND_4_1 - Chat Interface Streaming Attribution Confusion in Multi-Turn Attacks**
- Strength score: 1

The framework does not address user interface attribution in multi-agent chat systems. The attack vector of Agent B streaming malicious instructions styled as Agent A's output, exploiting user cognitive limitations tracking concurrent streams, is a UI/UX design and social engineering threat entirely outside the framework's scope. No framework control targets content-source attribution in unified streaming interfaces.

**RND_4_2 - Command Palette Context Prediction Non-Determinism Enabling Inconsistent Guardrails**
- Strength score: 1

The framework does not address command palette AI or contextual action prediction. The non-determinism in multi-agent command suggestion aggregation — where dangerous commands surface at variable priority without warnings across executions — is not addressed by any framework control. This is a UI/UX and non-determinism gap outside the framework's coverage.

**RND_4_3 - Approval Workflow Confidence Score Non-Determinism Defeating Threshold-Based Controls**
- Strength score: 2

The framework recommends RBAC/ABAC and approval controls, and the DHS/CISA document mentions measuring AI model performance and outputs to identify behavioral changes. These controls are directionally relevant to approval workflows. However, the specific threat of non-deterministic confidence score variation in multi-agent aggregation — enabling attackers to resubmit identical requests until variance produces auto-approval — is not addressed. The framework's approval mechanisms assume deterministic or stable threshold behavior, which this threat directly undermines.

**RND_4_4 - Multi-Agent Dashboard Real-Time State Updates Creating Race Condition Attack Windows**
- Strength score: 1

The framework does not address real-time dashboard state consistency or race condition windows in multi-agent operational displays. Monitoring and logging controls address what is recorded, not the transient inconsistency of displayed operational state that attackers can exploit during brief misleading "safe" indicator periods.

---

### RND_5 - Retry, Resilience, and Error Recovery

**RND_5_1 - Auto-Retry Error Recovery Creating Hidden Attack Amplification Loops**
- Strength score: 1

The framework recommends monitoring logs of inputs and outputs but does not address auto-retry mechanisms as an attack amplification surface. The blind spot created by collapsed error logs hiding multiple malicious operation executions, and the multi-agent distributed retry logic enabling payload execution during retries, are not covered. Non-deterministic retry timing defeating evaluations is outside the framework's scope.

**RND_5_2 - Retry Timing Non-Determinism Defeating Audit Reproducibility**
- Strength score: 1

The framework requires logging and monitoring but does not address the audit reproducibility implications of exponential backoff with jitter in multi-agent systems. The threat that attacks exploiting retry timing variations (malicious behavior triggered only on specific retry numbers) evade detection is not contemplated by any framework control.

**RND_5_3 - Non-Idempotent Operation Exploitation via Coordinated Retry Storms**
- Strength score: 1

The framework's least-privilege and sandboxing controls are tangentially relevant in limiting what operations agents can perform, but the specific threat of coordinated multi-agent retry storms causing duplicate financial transactions or resource allocations through non-idempotent operation exploitation is not addressed. Idempotency as a security property is not mentioned.

**RND_5_4 - Fallback Route Selection Non-Determinism Creating Untestable Branching**
- Strength score: 1

The framework does not address probabilistic fallback routing or the assurance gap created when non-determinism enables untested malicious routes to appear in production. This is a non-determinism-specific assurance gap entirely outside the framework's coverage.

**RND_5_5 - Circuit Breaker Opening Threshold Non-Determinism**
- Strength score: 1

Circuit breaker patterns and their non-deterministic opening thresholds in multi-agent systems are not addressed by the framework. This infrastructure resilience pattern introduces security-relevant non-determinism that the framework does not contemplate.

**RND_5_6 - Error Recovery Continuation Conditions Creating Untestable Non-Determinism**
- Strength score: 1

Probabilistic degradation thresholds in graceful degradation decisions and the resulting inconsistent degradation states across multi-agent systems are not addressed by any framework control. The framework assumes binary operational states rather than probabilistic partial degradation.

---

### RND_6 - Checkpoint, State, and Recovery Management

**RND_6_1 - Checkpoint Replay Poisoning Through State Manipulation**
- Strength score: 2

The framework directly addresses rollback to last known good state and immutable backups, which are relevant controls for checkpoint integrity. Cryptographic artifact validation could partially address checkpoint authenticity. However, the framework does not specifically address the adversarial manipulation window between checkpoint capture and resumption, the multi-agent scenario where Agent B corrupts checkpoint stores with semantically valid but logically poisoned data, or validation of checkpointed state before resumption. Extrapolation from existing controls provides partial but incomplete coverage.

**RND_6_2 - Determinism Violation Amplification via Cascading State Divergence**
- Strength score: 1

The framework does not address non-deterministic replay logic or cascading state divergence across agent partitions. The attack where Agent A triggers error recovery producing divergent replay outcomes from timestamp dependencies, enabling Agent B to submit conflicting transactions, is not addressed by any framework control.

**RND_6_3 - Retry Logic Exhaustion Through Adversarial Error Injection**
- Strength score: 1

The framework mentions monitoring for high-frequency inputs and oracle attack alerts, which is tangentially relevant to resource exhaustion detection. However, coordinated multi-agent strategic error injection forcing exponential retry backoff to mask critical failures — where no single agent appears malicious — is not addressed. The distributed nature of this attack makes it invisible to single-agent monitoring approaches the framework assumes.

---

### RND_7 - Tool Integration and API Management

**RND_7_1 - Tool API Drift and Silent Failure Propagation in Multi-Agent Chains**
- Strength score: 2

The NSA/CISA framework recommends supply chain inspection of pre-trained models and data source catalog maintenance, which are relevant to tool API provenance. The DHS/CISA document mentions interoperability failures as a design risk category. However, the specific threat of silent API format changes cascading through multi-agent chains (Agent A calling updated API, Agent B consuming corrupted output unaware) is not specifically addressed. The framework's supply chain and catalog controls require significant extrapolation to apply to runtime API version drift.

**RND_7_2 - Tool Schema Version Drift Across Agent Implementations**
- Strength score: 1

Tool schema versioning across simultaneously-running agents using different schema versions is not addressed by any framework control. The framework does not contemplate multi-agent systems where different components operate on incompatible interface assumptions simultaneously.

**RND_7_3 - Function Calling Model Updates Creating Behavioral Discontinuity**
- Strength score: 2

The framework requires running full evaluation before redeployment of new model versions, which is directly relevant to catching behavioral discontinuity from function calling behavior changes. However, the multi-agent scenario where different LLM versions (GPT-4 vs. Claude) generate function calls differently is not specifically addressed, and the framework's evaluation requirement applies to deployment gates rather than runtime heterogeneity.

**RND_7_4 - Tool Availability Changes Creating Non-Deterministic Behavior**
- Strength score: 1

The framework does not address tool deprecation and availability changes across heterogeneous multi-agent populations. Older agents invoking deprecated tools while newer agents skip tools older agents depend on represents a runtime interoperability gap not covered by any framework control.

**RND_7_5 - Function Calling Parameter Type Changes Creating Silent Failures**
- Strength score: 1

Silent failures from parameter type evolution (string to float, added required parameters) in multi-agent systems with agents trained on different definitions are not addressed. The framework does not address type compatibility or schema migration as security concerns.

**RND_7_6 - Few-Shot API Calling Example Poisoning in Tool Chains**
- Strength score: 2

The framework addresses supply chain inspection and data source integrity, which are relevant to poisoning of tool schemas and examples. Prompt injection protection is also tangentially relevant. However, the specific attack vector of adversaries poisoning API calling examples with subtle parameter modifications that agents learn from, creating distributed unsafe defaults, is not explicitly addressed. This requires extrapolation from supply chain and input integrity controls.

**RND_7_7 - Demonstration-Driven Fallback Behavior Injection**
- Strength score: 2

Similar to RND_7_6, supply chain inspection and prompt injection protection controls are relevant to fallback example poisoning. The framework's input sanitization guidance could partially address this if extended to tool descriptions. However, the specific threat of poisoning fallback examples to route agents to compromised implementations under benign triggering conditions is not explicitly covered.

---

### RND_8 - Framework and Architecture Non-Determinism

**RND_8_1 - Combinatorial Non-Determinism in Multi-Pattern Agent Compositions**
- Strength score: 1

The framework recommends adversarial testing and red-teaming but does not address the computational intractability of testing combined multi-pattern agent systems (ReAct + Plan-and-Execute + Reflection) where the state space approaches (K^M)^N * R^N. The framework implicitly assumes testing is feasible, which this threat directly refutes for production multi-agent compositions.

**RND_8_2 - Framework Non-Determinism Enabling Untestable Multi-Agent Attack Surfaces**
- Strength score: 1

The framework does not address framework-specific non-determinism (LangChain temperature, LangGraph reducers, AutoGen randomness) or the multiplicative non-determinism when combining frameworks. The (k^m)^n behavioral space making comprehensive testing infeasible is not a problem the framework's adversarial testing guidance can address, as it assumes finite testable behavior space.

**RND_8_3 - Continual Framework Updates Invalidating Multi-Agent Security Assurance**
- Strength score: 2

The framework requires running full evaluation before redeployment of new model versions and mentions continuous monitoring. This partially addresses the concern about framework updates invalidating security assurance. However, the framework does not address the N(N-1)/2 combination re-testing problem in multi-agent systems where any framework update requires re-evaluating all pairwise combinations, making comprehensive re-testing prohibitive.

**RND_8_4 - Framework Update Invalidating Streaming Security Assumptions**
- Strength score: 1

The framework's evaluation-before-redeployment requirement is tangentially relevant but does not address how streaming implementation updates (buffering, timing, error handling changes) cascade through multi-agent systems where each framework update shifts behavior across all component interactions.

---

### RND_9 - LangChain/LangGraph Specific Non-Determinism

**RND_9_1 - Non-Deterministic Conditional Edge Evaluation Creating Untestable Routing**
- Strength score: 1

LangGraph-specific conditional edge non-determinism (from time, random sampling, cache state) and its multi-agent compounding are not addressed by the framework. The framework does not address framework-specific implementation details at this level of specificity.

**RND_9_2 - Reducer Behavior Divergence Across Agent Updates**
- Strength score: 1

LangGraph reducer expectations (add_messages vs. overwrite) across agents with different versions is a framework-specific implementation concern entirely outside the NSA/CISA framework's scope.

**RND_9_3 - Checkpoint Restoration Non-Determinism in Replaying Cyclic Workflows**
- Strength score: 1

LangGraph-specific cyclic workflow checkpoint restoration non-determinism is not addressed. The framework's rollback controls assume deterministic restoration, which cyclic non-deterministic workflows violate.

**RND_9_4 - Confidence Score Non-Determinism in Tool Selection**
- Strength score: 1

LangChain-specific tool selection confidence non-determinism enabling intermittent safety gate failures, and attacker probing for non-deterministic windows, is not addressed by any framework control.

**RND_9_5 - Memory Update Non-Determinism Causing Unpredictable State Evolution**
- Strength score: 1

LangChain memory update ordering dependencies and race conditions from concurrent updates in distributed multi-agent systems are not addressed. The framework does not address memory consistency as a security property.

**RND_9_6 - Model Version Updates Invalidating Security Evaluations**
- Strength score: 2

The framework requires full evaluation before redeployment of new model versions, which is directly relevant here. However, the LangChain-specific scenario of shared model instances being updated and invalidating evaluations across all dependent agents simultaneously is not specifically addressed. The framework's evaluation gate applies to planned deployments, not to shared-instance updates that affect multiple agents without individual re-evaluation.

**RND_9_7 - Tool API Compatibility Drift Creating Untestable Behavior**
- Strength score: 1

LangChain-specific tool API compatibility drift and the non-deterministic error handling triggering unpredictable behaviors are not addressed by the framework beyond generic supply chain inspection guidance.

---

### RND_10 - AutoGen Specific Non-Determinism

**RND_10_1 - AutoGen Conversation Non-Determinism Defeating Reproducible Security Testing**
- Strength score: 1

AutoGen's message-driven architecture producing different flows across identical inputs — including attacks succeeding in 0.1% of executions that evade validation — is not addressed by the framework. The framework's adversarial testing and red-teaming guidance implicitly assumes that testing can establish unreachability of dangerous paths, which AutoGen's architecture structurally prevents.

**RND_10_2 - GroupChat Speaker Selection Non-Determinism Creating Untestable Attack Windows**
- Strength score: 1

AutoGen GroupChat's probabilistic speaker selection enabling malicious agents to be selected with exploitable certainty is entirely outside the framework's scope. The framework does not address agent selection mechanisms in multi-agent conversation systems.

**RND_10_3 - Multi-Agent Framework Update Non-Determinism Invalidating Cumulative Assurance**
- Strength score: 1

The combinatorial re-testing problem (N agents × M framework versions × P LLM versions) is not addressed. The framework's evaluation-before-redeployment requirement cannot scale to the infeasible re-testing matrix this threat describes.

---

### RND_11 - CrewAI Specific Non-Determinism

**RND_11_1 - CrewAI Hierarchical Task Routing Non-Determinism Enabling Unpredictable Delegation**
- Strength score: 1

CrewAI's probabilistic manager-based delegation routing is entirely outside the framework's scope. The attack where identical delegations route to compromised workers only probabilistically, allowing attacks to succeed or fail depending on routing outcomes, is not addressed by any framework control.

---

### RND_12 - Semantic Kernel Specific Non-Determinism

**RND_12_1 - LLM-Driven Function Routing Non-Determinism Defeating Audit Reproducibility**
- Strength score: 1

Semantic Kernel's LLM-driven dynamic plugin routing producing non-deterministic paths, invalidating evaluations asserting "never invokes dangerous plugins," is not addressed by the framework. Framework-specific routing mechanisms are outside the framework's scope.

**RND_12_2 - Plugin Registry State Changes Invalidating Prior Evaluations**
- Strength score: 1

Dynamic plugin registration modifying routing without code changes and invalidating evaluations is not addressed. The framework's evaluation-before-redeployment requirement applies to explicit version changes, not to implicit behavior changes from registry state evolution.

**RND_12_3 - FunctionChoiceBehavior Configuration Drift Across Agent Updates**
- Strength score: 1

Semantic Kernel FunctionChoiceBehavior configuration drift producing different routing consistency across agents is outside the framework's scope.

**RND_12_4 - Service Registration Lifecycle Enabling State Inconsistency**
- Strength score: 1

Dynamic kernel service registration and concurrent agents observing different service sets during registration changes are not addressed. This is a Semantic Kernel implementation-specific concern outside the framework's coverage.

---

### RND_13 - Multimodal and Vision-Language Models

**RND_13_1 - Multimodal Model Version Drift Creating Inconsistent Behavior Across Agents**
- Strength score: 2

The framework requires supply chain inspection and full evaluation before redeployment, which are relevant to multimodal model version management. The DHS/CISA document mentions dataset and model validation. However, the specific problem of asynchronous multimodal model updates (Agent A using CLIP v1.0, Agent B using CLIP v2.0) creating inconsistent results from shared storage across a multi-agent system is not specifically addressed. Extrapolation from version management and evaluation controls provides moderate but incomplete coverage.

**RND_13_2 - Vision Model Output Format Inconsistency Across Agents**
- Strength score: 1

Heterogeneous vision model output formats (captions, tables, embeddings) creating processing ambiguities in multi-agent systems are not addressed. The framework does not address modality-specific interoperability as a security concern.

**RND_13_3 - Whisper Model Update Drift in Audio Processing**
- Strength score: 1

Audio model update drift between updating and dependent synthesis agents is not specifically addressed. Generic version management controls require heavy extrapolation to apply here.

**RND_13_4 - Embedding Model Quantization Inconsistency in Multi-Agent Deployments**
- Strength score: 1

Quantization inconsistency (FP32, FP16, INT8) across agents affecting similarity thresholds and retrieval rankings is not addressed by any framework control. Numerical precision as a security property is entirely outside the framework's scope.

**RND_13_5 - Temperature Variation in Vision-Language Model Output**
- Strength score: 1

Vision-language model temperature variation producing different outputs from identical images across multi-agent systems is not addressed. The framework does not address sampling parameter management as a security control.

---

### RND_14 - Evaluation, Testing, and Benchmarking Non-Determinism

**RND_14_1 - Non-Deterministic Evaluation Results Defeating Regression Testing**
- Strength score: 1

The framework requires full evaluation before redeployment but does not address the fundamental problem that evaluation pipelines with stochastic components produce non-deterministic results, making regression detection impossible. The framework implicitly assumes evaluation produces reliable signals, which multi-agent stochastic evaluation pipelines cannot guarantee.

**RND_14_2 - Model Update Invalidating Evaluation Metric Baselines**
- Strength score: 1

Evaluation metric models updating and shifting baselines is not addressed. The framework's evaluation requirement does not account for the evaluation infrastructure itself being a source of non-determinism.

**RND_14_3 - Evaluation Pipeline Component Drift Through Continual Updates**
- Strength score: 1

Multi-stage evaluation pipeline component drift creating non-reproducible results is not addressed. This undermines the framework's reliance on evaluation as a gate before redeployment.

**RND_14_4 - Stochastic Test Case Selection Creating Non-Deterministic Coverage**
- Strength score: 1

Stochastic test case selection producing non-deterministic coverage, including different subsets across multi-agent evaluation, is not addressed. The framework assumes evaluation coverage is controllable and representative.

**RND_14_5 - Framework-Dependent Evaluation Behavior Creating Multi-Framework Incomparability**
- Strength score: 1

Framework-dependent evaluation behavior producing incomparable results across frameworks is not addressed. The framework does not account for evaluation methodology as a variable affecting security assurance validity.

**RND_14_6 - Non-Deterministic Benchmark Results Preventing Reliable Capacity Assessment**
- Strength score: 1

The compound variance problem (Agent A ±15%, Agent B ±20%, combined ±30%+) preventing reliable capacity assessment is not addressed. The framework's testing and red-teaming guidance assumes meaningful, reliable benchmark results are achievable.

**RND_14_7 - Statistical Significance Thresholds Becoming Meaningless in Multi-Agent Contexts**
- Strength score: 1

The multiple comparison problem in multi-agent security testing (N significance tests making p<0.05 unreliable) is not addressed. The framework does not address the statistical validity of its own testing and evaluation recommendations in multi-agent contexts.

**RND_14_8 - Model Update Proliferation Creating Assurance Gaps**
- Strength score: 2

The framework requires monitoring and continuous protection, and the DHS/CISA document addresses model drift and behavioral changes. The concept of heterogeneous update schedules creating assurance gaps where vulnerabilities fixed in Agent A remain in out-of-date Agent B is partially addressed by the framework's monitoring and update management guidance, though the multi-agent-specific dimension of distributed update schedules is not explicitly covered.

---

### RND_15 - Temperature and Sampling Parameters

**RND_15_1 - Parameter Tuning Non-Determinism Creating Unpredictable Agent Behavior**
- Strength score: 1

Temperature and sampling parameter management as security controls are not addressed by the framework. The multi-agent chain effect where Agent A's variable outputs become Agent B's inputs, compounding stochasticity, is not contemplated.

**RND_15_2 - Continuous Parameter Re-tuning as Assurance Gap**
- Strength score: 1

Continuous production parameter re-tuning as a moving-target security problem is not addressed. The framework does not identify sampling parameters as security-relevant configuration requiring change control.

**RND_15_3 - Cross-Agent Parameter Drift Creating Inconsistent Security Posture**
- Strength score: 1

Cross-agent temperature drift (one agent strict at 0.2, rejecting injections; peer lenient at 0.5, accepting) creating inconsistent security posture is not addressed by any framework control.

---

### RND_16 - Parameter Extraction and Tool Calling

**RND_16_1 - Stochastic Parameter Generation Drift Creating Hallucination Windows**
- Strength score: 1

High-entropy agents creating targeted hallucination vulnerabilities compared to low-temperature peers, enabling targeted exploitation, is not addressed by the framework.

**RND_16_2 - Tool Documentation Drift Across Agent Training Cycles**
- Strength score: 1

Tool specification version divergence across agents updated on different training cycles (spec v1 vs. v2) is not addressed beyond generic supply chain guidance that requires heavy extrapolation.

**RND_16_3 - Probabilistic Tool Selection Creating Adversarial Search Space**
- Strength score: 1

Soft attention-based probabilistic tool selection enabling attackers to craft ambiguous requests maximizing malicious tool probability is not addressed. The framework does not address tool selection mechanisms as security controls.

**RND_16_4 - Non-Deterministic Fallback Ordering Creating Unpredictable Attack Surfaces**
- Strength score: 1

Probabilistic fallback chain selection enabling attackers to craft inputs including malicious tools in fallback sequences is not addressed by any framework control.

**RND_16_5 - Continuous Hallucination Rate Regression Through Updates**
- Strength score: 2

The framework requires full evaluation before redeployment and monitoring for behavioral changes, and the DHS/CISA document mentions measuring AI model performance to identify behavioral changes. These controls are relevant to catching hallucination rate regression. However, the multi-agent scenario where Agent A improves but Agent B regresses — with grounding checks calibrated to older rates becoming ineffective — is not specifically addressed.

**RND_16_6 - Non-Deterministic Parameter Extraction Causing Inconsistency**
- Strength score: 1

Parameter extraction variance compounding across multi-agent chains (±2% in A and ±2% in B creating ±4% downstream) is not addressed as a security concern. Numerical variance propagation in agent chains is outside the framework's scope.

**RND_16_7 - Tool Selection Variance Creating Reliability Gaps**
- Strength score: 1

Probabilistic tool selection preventing reliable inter-agent coordination (Agent B cannot reliably coordinate expecting specific tool semantics from Agent A) is not addressed by any framework control.

**RND_16_8 - Trajectory Variance Creating Non-Deterministic Execution Paths**
- Strength score: 1

Trajectory variance across runs producing different action sequences and different input contexts for downstream agents is not addressed. The framework does not address execution path non-determinism as a security property.

**RND_16_9 - Parameter Accuracy Variance Across Temperature Settings**
- Strength score: 1

Temperature-dependent parameter extraction accuracy variation (95% at 0.3 vs. 78% at 0.7) and its security implications are not addressed. The framework does not identify temperature settings as security-relevant configuration.

---

### RND_17 - Retrieval-Augmented Generation (RAG) and Multi-Hop QA

**RND_17_1 - Ranking Model Manipulation Through Relevance Score Injection**
- Strength score: 2

The framework addresses input sanitization, prompt injection protection, and supply chain inspection of data sources. The data source catalog and monitoring controls are relevant to detecting anomalous retrieval patterns. However, the specific threat of adversarially crafted high-relevance documents manipulating ranking models in multi-hop retrieve-then-synthesize pipelines amplifying poisoning across downstream agents is not specifically addressed. The framework's controls require extrapolation to apply to retrieval ranking manipulation.

**RND_17_2 - Semantic Similarity Exploitation in Dense Retrieval for Multi-Hop Chains**
- Strength score: 2

Input sanitization and prompt injection protection controls are relevant, and the data source catalog implies content validation. However, the specific attack of crafting documents semantically similar to queries containing instructions, exploited through multi-hop chains where step N+1's query derives from step N's results, is not specifically addressed. The cascading nature of multi-hop injection is not contemplated.

**RND_17_3 - Query Reformulation Instruction Injection in Multi-Hop Retrieval**
- Strength score: 2

Prompt injection protection and input sanitization controls are relevant. The framework's guidance on protecting against prompt injection broadly covers instruction injection, but the specific multi-agent vector of Agent A's extraction creating sub-query instructions that Agent B then executes — through automatic query reformulation — is not specifically addressed and requires significant extrapolation.

**RND_17_4 - Evidence Document Hallucination in Multi-Hop Reasoning Bridge Attacks**
- Strength score: 1

The framework addresses data source catalog and input sanitization but does not address hallucinated bridging document injection in multi-hop reasoning. The attack of injecting fake bridging entities that multi-agent systems treat as valid knowledge graph nodes is not addressed by any specific framework control.

**RND_17_5 - Evidence Grounding Failures as RAG Attack Surface**
- Strength score: 1

The framework does not address evidence grounding or claim verification as security requirements. The multi-agent trust propagation problem (Agent B cannot validate authenticity, trusts Agent A's grounded claims) is not addressed.

**RND_17_6 - Source Attribution Reasoning Confusion**
- Strength score: 1

Source attribution accuracy as a security property is not addressed by the framework. Multi-agent source confusion enabling poisoning through attribution manipulation is entirely outside the framework's scope.

**RND_17_7 - Hallucination Blindness in Knowledge Graphs**
- Strength score: 1

Knowledge graph hallucination (Agent A creating entries for hallucinated relationships, Agent B querying and propagating them) is not addressed. The framework does not address knowledge graph integrity or hallucination validation as security concerns.

---

### RND_18 - Infrastructure and Deployment Non-Determinism

**RND_18_1 - Event Delivery Guarantee Downgrade Attacks via Infrastructure Resource Exhaustion**
- Strength score: 1

The framework addresses DoS-adjacent concerns through monitoring for high-frequency inputs and sandboxed containers, but the specific threat of resource exhaustion forcing silent delivery guarantee downgrade (exactly-once to at-most-once) corrupting payment or coordination logic is not addressed. Message delivery semantics as a security property are outside the framework's scope.

**RND_18_2 - Swarm Consensus Manipulation via Strategic Byzantine Agent Injection at Critical Density Thresholds**
- Strength score: 1

Byzantine fault tolerance and swarm consensus manipulation are not addressed by the framework. The attack of injecting agents at sub-detection densities to cause consensus failures invisible to threshold-based monitoring — with the multi-agent phase transition dynamic — is entirely outside the framework's scope.

**RND_18_3 - Message Queue Ordering Guarantees as Assurance Gap Under Scaling**
- Strength score: 1

Message queue ordering guarantees and their degradation under scaling (RabbitMQ per-queue but not cross-queue ordering) are not addressed by any framework control. Infrastructure message ordering semantics as security concerns are outside the framework's scope.

**RND_18_4 - Vector Database Index Staleness as Runtime Assurance Gap**
- Strength score: 1

Vector database index staleness creating collective blind spots across multi-agent systems sharing a vector database is not addressed. The framework mentions data source catalog but does not address index freshness guarantees as security requirements.

**RND_18_5 - Prometheus Scrape Interval Variance as Observability Assurance Gap**
- Strength score: 1

Monitoring infrastructure lag (Prometheus scrape interval variance) and its security implications for multi-agent metric-driven decision-making are not addressed. The framework recommends monitoring but does not address the freshness and reliability of monitoring data itself.

**RND_18_6 - API Gateway Canary Deployment Assurance Gaps**
- Strength score: 2

The framework requires running full evaluation before redeployment and addresses rollback to last known good state. These controls are relevant to managing canary deployment risks. However, the specific multi-agent scenario of v2.0 agents invoking v1.0 tools through canary-split API gateways creating format mismatches is not specifically addressed. The framework's evaluation controls apply to pre-deployment gates, not runtime version heterogeneity within a canary window.

**RND_18_7 - MLflow Version Mismatch Assurance Gap During Rollouts**
- Strength score: 1

MLflow-specific version mismatch between simultaneously-running agents (v2.5 vs. v2.3) creating shared data structure incompatibilities is a tool-specific operational concern not addressed by the framework.

---

### RND_19 - Kubernetes and Container Orchestration

**RND_19_1 - Horizontal Pod Autoscaler Scaling Behavior Non-Determinism**
- Strength score: 1

HPA timing variability creating non-deterministic agent population changes during scaling events is not addressed by the framework. Kubernetes orchestration behavior as a security concern is outside the framework's scope, though sandboxed containers are mentioned generically.

**RND_19_2 - StatefulSet Ordinal Initialization Races During Restart**
- Strength score: 1

StatefulSet initialization races creating non-deterministic ordering despite sequential specification are not addressed. Kubernetes-specific pod lifecycle behavior is outside the framework's scope.

**RND_19_3 - Network Topology Changes From Node Autoscaling**
- Strength score: 1

Network topology non-determinism from Kubernetes node autoscaling affecting multi-agent co-location is not addressed. The framework mentions network-level controls (TLS, segmentation) but not topology non-determinism from autoscaling.

**RND_19_4 - Kubernetes Scheduler Indeterminism Creating Non-Reproducible Pod Placement**
- Strength score: 1

Kubernetes scheduler non-determinism enabling attackers to exploit rare scheduling conditions to place malicious agents on specific nodes is not addressed. The framework does not address container scheduling as a security concern beyond generic sandboxing recommendations.

---

### RND_20 - Deployment and Inference Infrastructure

**RND_20_1 - Container Image Drift in Continuous Deployment**
- Strength score: 2

The framework recommends supply chain inspection and cryptographic artifact validation, which are relevant to container image integrity and version pinning. However, the specific threat of canary deployments creating version skew across multi-agent systems deploying on different schedules is not specifically addressed. The framework's controls apply to individual deployment gates rather than fleet-wide version coherence.

**RND_20_2 - Non-Deterministic Traffic Splitting Across Agent Versions**
- Strength score: 1

Random sampling-based canary traffic splitting creating 2^N possible state combinations across N agents undergoing canary simultaneously is not addressed by any framework control.

**RND_20_3 - Asynchronous Event Processing Creating Ordering Non-Determinism**
- Strength score: 1

Event ordering non-determinism in event-driven multi-agent systems producing different results from different event arrival orders is not addressed. The framework does not address event processing semantics as a security concern.

**RND_20_4 - Unobservable State Transitions During Rolling Deployments**
- Strength score: 1

The N×(N-1)/2 compatibility combinations during simultaneous rolling deployments across N services, creating unobservable transient states, are not addressed. The framework does not address rolling deployment compatibility management.

**RND_20_5 - Checkpoint Resume Non-Determinism in Serverless Workflows**
- Strength score: 1

Serverless workflow checkpoint resume races (multiple agents racing to resume, picking up different states) are not addressed. The framework's rollback controls assume non-contested checkpoint resumption.

**RND_20_6 - Rolling Update Non-Determinism in Version Transitions**
- Strength score: 1

Heterogeneous behavior during rolling updates (Agent A v1 expecting specific interface from Agent B v2 with different interface) is not specifically addressed. The framework's evaluation controls apply before deployment, not to runtime interface incompatibility during rolling updates.

**RND_20_7 - Traffic Splitting and Canary Deployment Non-Determinism**
- Strength score: 1

Service mesh canary traffic splitting creating emergent gaps (some agents on Tool API v1, others v2) is not addressed beyond generic supply chain guidance that requires significant extrapolation.

---

### RND_21 - Inference Hardware and Optimization

**RND_21_1 - Quantization Granularity Non-Determinism in Distributed Inference**
- Strength score: 1

Different quantization modes across agents (per-tensor vs. per-token) creating behavioral inconsistency is not addressed. Hardware-level inference optimization is outside the framework's security scope, though GPU patching is mentioned generically.

**RND_21_2 - Kernel Fusion Non-Determinism From GPU Scheduler Variance**
- Strength score: 1

GPU scheduler variance causing different kernel fusion execution orderings across GPUs in multi-agent tensor parallelism is not addressed. This is a hardware non-determinism concern outside the framework's scope.

**RND_21_3 - Non-Deterministic Token Generation Across NIM Replicas Enabling Probability-Based Attacks**
- Strength score: 1

Floating-point rounding differences across GPU replicas creating exploitable probability surfaces in multi-agent load-balanced systems is not addressed. The framework does not address hardware-induced inference non-determinism.

**RND_21_4 - Dynamic Batching Indeterminism Creating Non-Reproducible Agent Behavior**
- Strength score: 1

Triton dynamic batching timing dependence creating non-deterministic inference results and non-reproducible multi-agent behavior is not addressed by any framework control.

**RND_21_5 - Continuous Model Updates Creating Perpetual Assurance Gaps in Multi-Agent Deployments**
- Strength score: 2

The framework requires full evaluation before redeployment and monitoring for behavioral changes. The DHS/CISA document addresses continuous testing and model drift monitoring. These controls are relevant to managing model update risks. However, the specific problem of zero-downtime continuous update systems creating perpetual instability with no stable baseline — where formal verification cannot complete before the next update — is not addressed. The framework's controls assume discrete, evaluable deployment events rather than continuous rolling updates.

**RND_21_6 - Hardware-Specific TensorRT Engine Variations Creating Deployment Assurance Gaps**
- Strength score: 1

TensorRT engine GPU-architecture specificity creating heterogeneous inference behaviors across fleet hardware (development RTX 4090 vs. production A100) is not addressed. Hardware-specific inference engine behavior as a security concern is outside the framework's scope.

---

### RND_22 - Optimization and Efficiency

**RND_22_1 - Profiling-Induced Non-Determinism as Assurance Violation**
- Strength score: 1

Profiling overhead creating non-determinism absent in baseline execution, and optimization decisions based on profiling data, are not addressed by the framework. Measurement-induced behavioral changes as a security concern are outside the framework's scope.

**RND_22_2 - Temperature and Sampling Parameter Variability Across Optimization Cycles**
- Strength score: 1

Configuration optimization adjusting sampling parameters and creating behavioral divergence across agents receiving different optimized parameters is not addressed. The framework does not identify sampling configuration management as a security control.

**RND_22_3 - Asynchronous Configuration Updates During Multi-Agent Coordination**
- Strength score: 1

Asynchronous configuration optimization (Agent A on old config, Agent B on new) is not addressed. The framework's change control guidance does not specifically address asynchronous configuration propagation in multi-agent systems.

**RND_22_4 - Speculative Decoding Acceptance Rate Variability**
- Strength score: 1

Speculative decoding acceptance rate variability creating non-deterministic latency and output distributions not observable with fixed test sets is entirely outside the framework's scope.

---

### RND_23 - Chain-of-Thought (CoT), Tree-of-Thought (ToT), and Reasoning Non-Determinism

**RND_23_1 - CoT path explosion in multi-agent coordination**
- Strength score: 1

The framework mentions red-teaming and adversarial testing but does not address the exponential CoT path explosion in multi-agent coordination, where each agent's reasoning choices affect other agents' available paths, making red-teaming unable to cover the state space.

**RND_23_2 - Reasoning consistency drift across agents**
- Strength score: 1

Reasoning pattern divergence across individually-updating agents, and poisoning attacks exploiting old patterns failing on updated agents, is not addressed. The framework does not address reasoning assurance as a distinct security concern.

**RND_23_3 - Temporal coordination non-determinism**
- Strength score: 1

Race conditions in agents coordinating through shared CoT traces (Agent A's reasoning incomplete when Agent B retrieves it) are not addressed. The framework does not address reasoning trace consistency as a security property.

**RND_23_4 - Stochastic multi-agent planning creating untestable paths**
- Strength score: 1

The multiplicative combination of stochastic reasoning choices across agents creating computationally infeasible combined test space is not addressed. The framework's testing guidance implicitly assumes feasible test space coverage.

**RND_23_5 - Backtracking state divergence in distributed ToT systems**
- Strength score: 1

Tree-of-thought distributed backtracking causing divergence in which branches have been explored across concurrently operating agents is not addressed by any framework control.

---

### RND_24 - Self-Consistency and Multiple Reasoning Paths

**RND_24_1 - Stochastic Sampling Non-Determinism as Assurance Gap**
- Strength score: 1

Self-Consistency's intentional stochastic decoding creating assurance gaps in multi-agent systems that assume deterministic behavior is not addressed. The framework does not address reasoning methodology non-determinism as a security concern.

**RND_24_2 - Temperature Tuning Parameter Drift as Runtime Assurance Gap**
- Strength score: 1

Self-Consistency temperature drift affecting all agents in multi-agent systems with shared temperature parameters is not addressed by the framework.

**RND_24_3 - Continual Sampling Parameter Optimization Creating Adversarial Drift**
- Strength score: 1

Continuous optimization adjusting sampling parameters based on success rates causing systematic drift across all agents in multi-agent shared optimization is not addressed.

**RND_24_4 - Context Window Non-Determinism From Compression Algorithms**
- Strength score: 1

Context window compression algorithm non-determinism in self-consistency with many paths, and compression non-determinism across agents with different context windows, are not addressed by any framework control.

---

### RND_25 - Hierarchical Task Network (HTN) Planning Non-Determinism

**RND_25_1 - Probabilistic Decomposition Method Selection Creating Assurance Gaps**
- Strength score: 1

LLM-based HTN planners' stochastic method selection (M1 at 0.7 probability, M2 at 0.3) and the multiplicative probability reduction in multi-agent paths are not addressed. The framework does not address planning methodology non-determinism.

**RND_25_2 - Temperature and Sampling Parameter Divergence Across Agents**
- Strength score: 1

HTN planner temperature divergence producing incompatible decompositions (conservative vs. creative) across agents is not addressed.

**RND_25_3 - Continual Method Library Evolution Breaking Decomposition Guarantees**
- Strength score: 2

The framework's supply chain inspection and evaluation-before-redeployment controls are relevant to managing method library evolution. Version mismatch between agents using library v1.0 vs. v1.1 creating decomposition failures is partially addressed by these controls, though HTN-specific planning assurance is not explicitly covered.

**RND_25_4 - Lack of Decomposition Reproducibility Across Agents**
- Strength score: 1

HTN hierarchical planning reproducibility gaps where different agents independently produce different decompositions are not addressed by the framework.

**RND_25_5 - Partial Order Scheduling Non-Determinism in Distributed Execution**
- Strength score: 1

HTN partial ordering resolution non-determinism in multi-agent distributed execution (Agent A specifies ordering, Agent C resolves non-deterministically) is not addressed.

**RND_25_6 - Constraint Satisfaction Heuristics Producing Divergent Solutions**
- Strength score: 1

Non-deterministic heuristic selection among multiple valid HTN constraint solutions, creating unexpected constraint solutions in separated solving and execution agents, is not addressed.

---

### RND_26 - Monte Carlo Tree Search (MCTS) Planning Non-Determinism

**RND_26_1 - MCTS Non-Determinism in Multi-Agent Convergence**
- Strength score: 1

MCTS non-determinism producing conflicting plans across agents (task X then Y vs. Y then X) in multi-agent systems is not addressed. The framework does not address planning algorithm non-determinism.

**RND_26_2 - Dynamic MCTS Adaptation Enabling Non-Deterministic Attacks**
- Strength score: 1

Adaptive MCTS exploration constant tuning creating heterogeneous exploration strategies across agents is not addressed.

**RND_26_3 - Simulation Budget Variance as Assurance Gap**
- Strength score: 1

MCTS simulation budget variance under resource competition forcing insufficient budgets (1,000 vs. 10,000 simulations) and the resulting quality/safety gaps are not addressed by any framework control.

**RND_26_4 - Replanning Non-Determinism in Multi-Agent Workflows**
- Strength score: 1

MCTS replanning from identical failure states with different random seeds producing different recovery plans, and attackers exploiting this to force execution of dangerous replanning paths, is not addressed.

**RND_26_5 - Asynchronous Replanning State Divergence**
- Strength score: 1

Asynchronous multi-agent replanning causing state desynchronization exploitable by attackers is not addressed by the framework.

**RND_26_6 - Heuristic Evaluation Inconsistency Across Agents**
- Strength score: 1

A* heuristic estimate divergence causing agents to explore different search spaces with distributed computation non-determinism is not addressed.

---

### RND_27 - Episode-Based Memory and Consolidation

**RND_27_1 - Non-Deterministic Episode Retrieval Ranking Enabling Adaptive Attacks**
- Strength score: 1

Weighted retrieval combining similarity, recency, and importance producing unpredictable episode retrieval, exploited by attackers crafting intermediate-scored malicious episodes, is not addressed by the framework. Memory retrieval security is outside the framework's scope.

**RND_27_2 - Consolidation Timing Non-Determinism Enabling Attack Escalation Windows**
- Strength score: 1

Load-dependent consolidation timing exploitable by attackers timing malicious episodes for consolidation during monitoring-absent windows is not addressed.

**RND_27_3 - Retrieval Threshold Sensitivity as Assurance Gap**
- Strength score: 1

Context-dependent similarity threshold variation enabling poisoned episodes to retrieve only under specific conditions and avoid detection is not addressed by any framework control.

**RND_27_4 - Trajectory Length Variability Creating Hidden Attack Activation Paths**
- Strength score: 1

Malicious episode steps activating only during long trajectories when monitoring is exhausted is not addressed. The framework's monitoring guidance does not account for trajectory-length-dependent activation conditions.

**RND_27_5 - Embedding Model Update Non-Determinism Enabling Transient Poisoning Windows**
- Strength score: 2

The framework addresses supply chain inspection, cryptographic artifact validation, and monitoring for behavioral changes, which are relevant to embedding model update management. The DHS/CISA document addresses model drift monitoring. However, the specific threat of embedding space changes during staged rollouts creating transient poisoning windows — where some agents use old embedding space and others use new — is not specifically addressed.

**RND_27_6 - Graph Traversal Path Non-Determinism in Distributed Graph Stores**
- Strength score: 1

Graph database traversal path non-determinism enabling poisoned relationships to activate specific paths while avoiding detection is not addressed by any framework control.

---

### RND_28 - Knowledge Graphs and Memory Stores

**RND_28_1 - RAG Result Ranking Non-Determinism Due to Score Rounding**
- Strength score: 1

Similarity score rounding producing non-deterministic ranking boundaries, with correlated non-determinism affecting all agents sharing a ranking function, is not addressed.

**RND_28_2 - Knowledge Graph Traversal Ordering Non-Determinism**
- Strength score: 1

Non-deterministic relationship traversal order producing different reasoning paths across knowledge graph queries is not addressed by the framework.

**RND_28_3 - Temporal Validity Window Boundaries Creating Intermittent Visibility**
- Strength score: 1

Temporal document validity windows creating intermittent visibility based on query timing, and the resulting assurance gaps, are not addressed.

**RND_28_4 - Probabilistic Fact Evaluation Non-Determinism**
- Strength score: 1

Probabilistic knowledge graph facts creating non-deterministic evaluation where agents sample different probabilities, and attackers leveraging variance across shared probabilistic knowledge, are not addressed.

**RND_28_5 - Incremental Update Non-Determinism**
- Strength score: 1

Agents querying during incremental knowledge base updates and receiving partially-updated data creating divergent snapshots are not addressed by the framework.

**RND_28_6 - Caching Invalidation Timing Non-Determinism**
- Strength score: 1

Cache invalidation timing windows creating temporary semantic divergence between agents (some retrieving cached old data, others retrieving new data) are not addressed. The framework does not address caching consistency as a security concern.

---

### RND_29 - Context Assembly and Working Memory

**RND_29_1 - Multi-Agent Context Assembly Non-Determinism Creating Audit Blind Spots**
- Strength score: 1

Context assembly order variation across executions due to non-deterministic retrieval ranking and async retrieval — causing security-critical context positioning to change between audits and making reproducible auditing impossible — is not addressed. The framework's logging requirements assume auditable, reproducible context, which this threat structurally undermines.

**RND_29_2 - Streaming Generation Non-Determinism Across Agent Handoffs**
- Strength score: 1

Variable token emission rates and sampling affecting precise token sequences arriving at downstream agents, making malicious instructions hidden in alternative token sequences untestable, are not addressed. This overlaps with RND_1 concerns but specifically targets the agent handoff dimension.

**RND_29_3 - Continual Update Propagation Creating Distributed Regression Gaps**
- Strength score: 2

The framework's continuous monitoring, full evaluation before redeployment, and the DHS/CISA document's behavioral change detection are relevant to distributed regression gaps. The scenario of Agent A at v2.1 while Agent B remains v2.0 creating emergent untested behaviors is partially addressed by monitoring controls, though attacker exploitation of version boundaries during heterogeneous states is not specifically addressed.

---

### RND_30 - Utility and Decision Making

**RND_30_1 - Probabilistic Sampling Variability in Expected Utility Calculation**
- Strength score: 1

Monte Carlo sampling non-determinism in expected utility calculations causing divergent decisions across multi-agent systems (Agent A and Agent B sampling different utilities) is not addressed by the framework.

**RND_30_2 - Stochastic Outcome Distribution Changes Breaking Cached Utility Calculations**
- Strength score: 1

Stale cached utility calculations becoming invalid when outcome distributions change, with cascading degradation across dependent agents, are not addressed. The framework does not address utility function staleness as a security concern.

**RND_30_3 - Non-Deterministic Weight Adjustment Mechanisms Creating Unsafe Adaptation**
- Strength score: 1

Dynamic utility weight adjustment causing divergent convergence where agents develop incompatible utility functions is not addressed by any framework control.

**RND_30_4 - Evaluation Non-Reproducibility for Utility-Based Decisions**
- Strength score: 1

Non-reproducible testing for utility-based agent decisions (safe under one distribution, unsafe on future identical inputs) is not addressed. This is a specific manifestation of the broader non-determinism assurance gap the framework does not address.

---

### RND_31 - Rule-Based and Adaptive Systems

**RND_31_1 - Rule Priority Instability Creating Non-Deterministic Execution**
- Strength score: 1

Rule priority non-determinism in multi-agent systems with tunable or adaptive priorities causing different rule sequences for identical requests is not addressed by the framework.

**RND_31_2 - Learning Rule Instability in Adaptive Systems**
- Strength score: 1

Continuously-refined rules in shared multi-agent learning environments creating non-deterministic behavior are not addressed. The framework does not address adaptive rule systems as a security concern.

**RND_31_3 - Heuristic Parameter Drift in Multi-Agent Tuning**
- Strength score: 1

Independent heuristic parameter tuning creating divergence and non-determinism across agents is not addressed.

---

### RND_32 - Learning-Based Policies and Reinforcement Learning

**RND_32_1 - Learned Policy Non-Determinism Creating Assurance Gaps**
- Strength score: 1

Stochastic policy components (softmax action selection, dropout, temperature sampling) causing exponential behavior variance in multi-agent systems are not addressed. The framework does not address reinforcement learning-specific security concerns.

**RND_32_2 - Online Learning Continual Change Defeating Validation**
- Strength score: 2

The DHS/CISA document addresses data drift and model drift monitoring, and the framework requires full evaluation before redeployment. These controls are partially relevant to online learning drift. However, the specific problem of online learning making validation continuously invalid as policies drift during deployment — with multi-agent propagation accelerating change — is not specifically addressed. The framework assumes discrete, evaluable update cycles rather than continuous in-deployment learning.

**RND_32_3 - Exploration Behavior Unpredictability as Risk Surface**
- Strength score: 1

RL exploration phases during deployment creating inherent unpredictability gaps, and coordinated multi-agent exploration creating correlated unpredictability, are not addressed by the framework.

**RND_32_4 - Experience Replay Temporal Non-Determinism**
- Strength score: 1

DRL experience replay temporal gaps causing different policy updates from the same state, and shared replay buffer non-determinism in multi-agent systems, are not addressed.

**RND_32_5 - Multi-Agent Coordination Emergent Behavior Unpredictability**
- Strength score: 1

MARL emergent behavior from agent interactions requiring all combination execution for complete assurance — which is computationally infeasible — is not addressed. This is a fundamental MARL safety concern outside the framework's scope.

**RND_32_6 - Policy Network Weight Sensitivity to Training Details**
- Strength score: 1

Training-detail-sensitive policy weight heterogeneity across identically-architected but differently-trained agents is not addressed by the framework.

**RND_32_7 - Gradient-Based Adversarial Policy Perturbations**
- Strength score: 2

The framework addresses adversarial testing and hardware model weight protection (HRZ/HSM), which are relevant to policy perturbation attacks. The DHS/CISA document mentions adversarial training. However, synchronized gradient vulnerabilities enabling single perturbations to affect multiple agents simultaneously — the multi-agent amplification dimension — is not specifically addressed.

---

### RND_33 - Hybrid and Heterogeneous Systems

**RND_33_1 - Temperature Heterogeneity in Hybrid Paradigm Processing**
- Strength score: 1

Temperature differences across hybrid paradigm agents (T=0.0 deterministic rules, T=0.4 optimization, T=0.8 learning) creating targeted payload injection opportunities against high-temperature agents is not addressed by the framework.

**RND_33_2 - Paradigm-Specific Non-Determinism in Cooperative Cycles**
- Strength score: 1

Cooperative architecture cycle count non-determinism (convergence heuristic-dependent), and attackers crafting injections activating only after specific cycles in asynchronous multi-agent cooperation, are not addressed.

**RND_33_3 - Streaming Response Non-Determinism in Hybrid Output Synthesis**
- Strength score: 1

Non-deterministic streaming order in hybrid architecture output synthesis, and attackers crafting instructions triggering only in specific orders, are not addressed.

**RND_33_4 - Knowledge Graph Consistency Assurance Gaps During Multi-Agent Evolution**
- Strength score: 1

Knowledge graph consistency gaps during multi-agent evolution without global consistency guarantees, exploitable by attackers injecting contradictory relationships during inconsistency windows, are not addressed.

**RND_33_5 - Feedback Loop Timing Non-Determinism in Hybrid Cooperation**
- Strength score: 1

Feedback loop timing non-determinism causing different convergence paths, and attackers exploiting timing to force dangerous convergence, are not addressed by any framework control.

---

### RND_34 - Other Risks/Threats/Vulnerabilities worth noting

**RND_34_1 - Keyboard Navigation Testing Gaps for Approval Workflows**
- Strength score: 1

Keyboard navigation testing fragility and ARIA testing gaps are UI/UX concerns entirely outside the framework's scope. The framework does not address accessibility testing or DOM manipulation timing as security concerns.

**RND_34_2 - HITL Approval Testing Non-Reproducibility**
- Strength score: 2

The framework recommends RBAC/ABAC and access controls relevant to HITL approval workflows, and the DHS/CISA document mentions human oversight mechanisms. However, the specific non-reproducibility problem of HITL approval workflows from confidence score variance, adaptive thresholds, and time-based expiration — especially in multi-agent contexts where threshold learning interactions compound — is not specifically addressed. The framework's controls require extrapolation to cover approval workflow testing integrity.

**RND_34_3 - Auto-Scaling Non-Determinism in Replica Configuration**
- Strength score: 1

Auto-scaling replica configuration non-determinism (different model versions, parameters, resources on scaled replicas) preventing testing guarantees about future replica behavior is not addressed.

**RND_34_4 - Batching Timeout Non-Determinism**
- Strength score: 1

Dynamic batching probabilistic timeout non-determinism causing different batch compositions across executions is not addressed by the framework.

**RND_34_5 - Load Balancing Algorithm Non-Determinism**
- Strength score: 1

Load balancing algorithm non-determinism routing identical requests to different replicas across executions is not addressed. The framework does not address load balancing as a security concern.

**RND_34_6 - Caching Invalidation Non-Determinism**
- Strength score: 1

TTL or event-based cache invalidation creating non-deterministic cache states where query cache-hit depends on uncontrolled temporal factors is not addressed.

**RND_34_7 - Non-Deterministic Evaluation Due to Model Temperature Settings**
- Strength score: 1

Temperature-variable evaluation producing non-deterministic results making comparisons unreliable, and attackers hiding metric variance through temperature exploitation, are not addressed.

**RND_34_8 - Evaluation Instability From Framework Version Changes**
- Strength score: 1

Framework version changes affecting evaluation infrastructure behavior (e.g., MLflow NaN handling changes) are not addressed. The framework does not address evaluation infrastructure stability as a security property.

**RND_34_9 - Continuous Evaluation CI/CD Timing Variations**
- Strength score: 1

CI infrastructure performance variability affecting timeout-based evaluation decisions is not addressed by the framework.

**RND_34_10 - Evaluation Workflow State Machine Non-Determinism**
- Strength score: 1

Non-deterministic evaluation workflow state transitions affecting results for agents with state-dependent behavior are not addressed.

**RND_34_11 - Model Update Timing Creating Evaluation Windows**
- Strength score: 2

The framework's monitoring and continuous protection controls, and the DHS/CISA document's behavioral change detection requirements, are partially relevant to detecting malicious updates timed to evaluation blind spots. However, the specific attack of timing malicious updates to fall within evaluation blind spots created by continuous agent updates — where non-deterministic evaluation timing creates exploitable gaps — is not specifically addressed.

**RND_34_12 - Difficulty Classification Continual Adaptation Creating Assurance Evasion**
- Strength score: 1

Difficulty classification changing sampling budgets over time, creating variable budgets for identical inputs and uniform drift in multi-agent shared classifications, is not addressed.

**RND_34_13 - Non-Deterministic Path Ordering in Weighted Voting**
- Strength score: 1

Tie-breaking non-determinism in weighted voting affecting all downstream agents through propagation is not addressed by any framework control.

**RND_34_14 - Semantic Chunking Boundary Non-Determinism Across Paragraph Detection Heuristics**
- Strength score: 1

ETL semantic chunking boundary differences across heterogeneous agent implementations (Unix vs. Windows line endings, different token counting methods) creating systematic inconsistencies and near-duplicate chunks in shared vector databases are not addressed by the framework. Data pipeline implementation consistency is not covered.

**RND_34_15 - Deduplication Hash Collision Non-Determinism in Concurrent Multi-Agent Extraction**
- Strength score: 1

Race conditions in concurrent ETL deduplication (multiple agents independently checking separate seen_hashes sets, both deciding to process the same document) creating 15-40% duplicate rates are not addressed by the framework. Concurrent extraction consistency is outside the framework's scope.

**RND_34_16 - REST API Pagination Cursor Non-Determinism Creating Inconsistent Multi-Agent Extraction**
- Strength score: 1

Pagination cursor non-determinism from data insertion during multi-page extraction — causing different agents on staggered schedules to retrieve different record sets, and preventing re-extraction validation — is not addressed by the framework.

**RND_34_17 - Filesystem Modification Time Resolution Variability Affecting Incremental ETL Updates**
- Strength score: 1

Platform filesystem mtime resolution variability (Linux nanosecond, Windows 2-second, NFS clock skew) across heterogeneous multi-agent infrastructure creating different change sets is not addressed by any framework control.

**RND_34_18 - Batch Subdivision Ordering Non-Determinism During Partial ETL Failure Recovery**
- Strength score: 1

Different batch subdivision strategies during ETL failure recovery (binary vs. failure-point subdivision) creating different database load patterns and potential deadlocks from overlapping lock ranges in concurrent multi-agent retry is not addressed by the framework.

**RND_34_19 - Quality Metric Calculation Variability Across Heterogeneous Agent Validation Implementations**
- Strength score: 1

Heterogeneous quality validation implementations (different completeness, accuracy, consistency metrics across agents) creating inconsistent thresholds that enable attackers to route documents to lenient implementations is not addressed. The framework does not address validation implementation consistency as a security requirement.

# RTM Analysis — NSA_Deploy_AI_Securely Framework Coverage

## RTM — Telemetry and monitoring blind spots specific to cognitive and tool behavior

### RTM_1 - UI and Interface Telemetry Gaps
- Strength score: 1

RTM_1 comprises 17 fine-grained UI/interface telemetry threats — progressive disclosure hiding malicious tool calls, chat interfaces missing semantic intent, streaming detection delays, approval workflow reasoning provenance gaps, command palette context manipulation, retry sequence atomic logging, cross-session state poisoning, multi-agent dashboard attribution, rejection pattern analysis, tool parameter semantic analysis, confidence score decomposition, session state restoration verification, reasoning trace sampling bias, pre-intervention state capture, cost/economic attack indicators, HITL monitoring blind spots, and accessibility telemetry gaps. The NSA/CISA Joint CSI does mandate logging of "inputs/outputs/intermediate states and errors with automated alerts" and monitoring for "high-frequency/repetitive inputs," and DHS/CISA calls for monitoring "inputs and outputs for unusual/malicious behavior." However, these directives are pitched at the infrastructure level, not at the UI layer. None of the framework documents discuss progressive disclosure patterns, chat interface semantic logging, streaming response detection windows, approval reasoning provenance, command palette context provenance graphs, cross-session state thread tracking, multi-agent attribution complexity in dashboards, rejection pattern telemetry, parameter semantic analysis, confidence decomposition across agents, session restoration integrity checks, sampling bias from performance overhead, pre-intervention distributed state capture, economic attack signals in cost telemetry, HITL monitoring specifics, or accessibility telemetry. All RTM_1 threats are squarely in the "UI/UX design" and "cognitive/reasoning-layer monitoring" blind-spot categories the framework explicitly does not cover.

### RTM_2 - Framework-Specific Architecture and Logging Gaps
- Strength score: 1

RTM_2 contains 26 threats tied to specific multi-agent framework architectures — Plan-and-Execute opacity, tool chain cross-agent privilege escalation monitoring, correlation ID manipulation, swarm emergent behavior blind spots, cross-framework observability gaps, framework documentation reconnaissance, checkpoint-mediated state mutation evasion, conditional routing opacity, reducer state tracking, LangChain ConversationBufferMemory and agent scratchpad gaps, tool invocation semantic analysis, memory update provenance, error recovery sequence telemetry, confidence decomposition, AutoGen GroupChat attribution blind spots, CrewAI task delegation opacity, multi-agent dashboard visualization blind spots, AutoGen non-determinism defeating baselines, CrewAI hierarchical monitoring opacity, Semantic Kernel plugin chain abstraction, function description injection detection, dependency injection resolution telemetry, orchestrator prompt construction, plugin routing decision opacity, and Semantic Kernel-specific blind spots. The framework mentions input sanitization and prompt injection protection generically, and calls for logs of inputs/outputs/intermediate states. However, it contains zero framework-specific guidance on LangChain, AutoGen, CrewAI, or Semantic Kernel architectures; no discussion of correlation ID integrity; no treatment of swarm emergent behavior monitoring; no guidance on cross-framework observability boundaries; and nothing about checkpoint forensics, conditional routing audit, or reducer tracing. These are all explicitly identified as framework-specific telemetry gaps the framework does not cover.

### RTM_3 - Tool Invocation and Function Calling Monitoring
- Strength score: 2

RTM_3 covers six threats: cross-agent tool invocation telemetry correlation attacks, distributed invocation anomaly evasion, parallel execution latency blind spots, cross-boundary output validation gaps, tool authorization scope audit blind spots, and asynchronous causality tracking failures. The NSA/CISA Joint CSI does explicitly address tool-level controls — it mandates "logs of inputs/outputs/intermediate states and errors," monitoring for "high-frequency/repetitive inputs," and "API returns minimal data," and requires least-privilege access controls and adversarial testing. The DHS/CISA document calls for monitoring inputs and outputs for unusual/malicious behavior. These controls are relevant to detecting anomalous tool invocation patterns, authorization scope abuse, and output validation failures. However, the framework never addresses cross-agent correlation of tool sequences, distributed anomaly distribution that evades per-agent monitors, parallel-execution latency masking, the specific handoff validation gap between agents, delegation-based authorization audit gaps, or asynchronous causality spoofing in logs. The framework provides a partial and indirect foundation through its invocation logging and least-privilege mandates but requires significant extrapolation to address the multi-agent-specific dimensions of these threats.

### RTM_4 - Multimodal and Streaming Response Processing
- Strength score: 1

RTM_4 spans 18 threats across multimodal processing opacity, vision model hallucination detection, embedding similarity threshold blind spots, streaming multimodal output monitoring gaps, cross-modal consistency validation absence, error message content opacity, retry sequence attack indicators, fallback routing telemetry gaps, circuit breaker state transition telemetry, graceful degradation decision opacity, cross-agent error recovery causality, streaming error intermediate state visibility, streaming telemetry temporal lag, streaming progress indicator manipulation, cross-agent streaming trace correlation failures, streaming buffer overflow content loss, and streaming rate limiting evasion. The NSA/CISA Joint CSI does address input sanitization and logs of intermediate states, and DHS/CISA calls for monitoring inputs/outputs for unusual behavior. However, the framework contains no guidance on multimodal processing, vision model monitoring, cross-modal consistency validation, embedding retrieval threshold monitoring, streaming-specific telemetry collection challenges, circuit breaker state forensics, graceful degradation audit, or streaming buffer/rate evasion in multi-agent contexts. The streaming-specific and multimodal-specific threats are entirely outside the framework's scope, which was written for general AI deployment, not multimodal or streaming architectures.

### RTM_5 - Evaluation and Assessment Telemetry Gaps
- Strength score: 2

RTM_5 contains 17 threats focused on evaluation pipeline security — evaluation result anomaly detection, evaluation agent reasoning telemetry, cross-agent evaluation orchestration monitoring, metric calculation opacity, evaluation framework implementation blind spots, audit trail completeness gaps, evaluation dataset integrity monitoring absence, evaluation agent behavior baseline absence, cross-evaluation temporal analysis gaps, metric telemetry spoofing, evaluation performance monitoring blind spots, audit trail integrity violations through log tampering, alert threshold calibration evasion, metric correlation analysis blind spots, benchmark execution telemetry gaps, user feedback monitoring attribution blind spots, and gradual performance degradation. The NSA/CISA Joint CSI mandates "run full evaluation before redeployment," adversarial testing, pen testing with external security experts, cryptographic artifact validation, and immutable backup storage for log data. DHS/CISA calls for continuous testing, red-teaming, and measuring model performance post-deployment to identify behavioral changes. These controls directly address evaluation practices, and immutable log storage partially addresses audit trail tampering. However, the framework does not specifically address evaluation dataset integrity monitoring, evaluation agent reasoning telemetry, cross-agent evaluation orchestration blind spots, metric telemetry spoofing, gradual metric degradation evasion, or benchmark execution telemetry fragmentation in multi-agent systems. The framework's evaluation mandates are real but pitched at a high level, requiring significant extrapolation to cover RTM_5's specific threats.

### RTM_6 - Metrics Collection and Manipulation Evasion
- Strength score: 1

RTM_6 contains 32 threats covering approval workflow audit fragmentation, multi-agent web navigation timing gaps, asymmetric monitoring parameter configurations, confidence score tuning for evasion, latency gaming through optimization, demonstration ordering as covert instruction activation, asynchronous log aggregation evasion, delegation-based tool audit semantic gaps, tool response quality metrics poisoning through aggregation, cross-agent tool selection monitoring blind spots, entropy tracking evasion through confidence aggregation, asynchronous tool validation coverage gaps, cross-agent context correlation blindness, action accuracy metrics absent from production, parameter validation failures hidden in aggregates, tool execution errors hidden in success rates, multi-turn consistency losses, error recovery pattern invisibility in distributed tracing, cross-agent trajectory metrics absence, system-level coherence validation blind spot, reasoning quality drift without continuous monitoring, reasoning quality evaluation scope limitations, attribution and traceability gaps in reasoning chains, efficiency metric aggregation obscuring attack signals, sampling-based efficiency monitoring blind spots, latency percentile masking attack spikes, cost attribution lag, efficiency alert thresholds missing gradual degradation, efficiency metric correlation analysis blind spot, historical baseline staleness, telemetry processing latency as detection evasion window, and instrumentation coverage gaps. The framework's generic call to monitor inputs/outputs and watch for data drift provides only the most tenuous connection to these highly specific multi-agent telemetry manipulation and evasion techniques. The framework does not address log aggregation timing attacks, confidence score tuning for evasion, demonstration ordering attacks, entropy aggregation masking, trajectory metric computation, system-level coherence validation, reasoning quality continuous monitoring, attribution in multi-agent reasoning chains, gradual degradation through threshold evasion, cost attribution lag exploitation, or instrumentation coverage asymmetry exploitation.

### RTM_7 - Infrastructure and Observability Stack Blind Spots
- Strength score: 1

RTM_7 is the largest subsubsection with 48 threats spanning tool behavior attribution loss in shared multi-agent tool use, message queue consumer lag hiding agent failures, vector database latency hiding semantic drift, Prometheus scrape success masking malfunctioning agents, API gateway success rate hiding tool failures, MLflow deployment success hiding performance regression, message queue throughput masking quality degradation, Prometheus alert suppression causing cognitive blindness, rate limit exhaustion masking timeouts, MLflow metrics aggregation hiding individual regressions, monitoring agent metrics aggregation skewing behavior, alert fatigue from pod/service metric volume mismatch, canary metric blind spots from insufficient per-agent sample size, distributed tracing overhead creating instrumentation bias, observable metrics obscuring internal agent states, log timestamp skew preventing causality reconstruction, Prometheus cardinality explosion enabling monitoring denial-of-service, missing inter-agent coordination latency metrics, service mesh semantic audit gaps, Grafana aggregate dashboard bias toward healthy states, alert threshold manipulation for suppression, audit log asynchronous write loss, container registry layer poisoning audit gaps, Kubernetes audit log cardinality overload, GPU idle period tool execution blind spots, tracing overhead as observability paradox, metric aggregation masking anomalies, MLflow distributed optimization logging gaps, speculative decoding token prediction opacity, profiling data retention forensic gaps, inference latency spikes masked by batching indeterminism, orchestration-level queue depth blind spots, error rate aggregation masking multi-agent failure patterns, token generation metrics absence, cross-agent correlation absent from standard monitoring, GPU telemetry insufficient for quantization attacks, TensorRT engine metadata drift undetectable without binary inspection, quantization artifact telemetry absence, cross-agent coordination timing blind spots, model update validation telemetry gaps, load balancer routing obscuring agent-level behavior, batching obscuring tool invocation patterns, caching creating hit/miss monitoring blind spots, session affinity hiding cross-replica propagation, auto-scaling configuration blind spots, dynamic load balancer metrics poisoning, streaming response monitoring complexity in batched contexts, and cost telemetry attribution opacity in distributed batching. The NSA/CISA framework mentions GPU patching and hardware model weight protection, and broadly calls for monitoring. The DHS/CISA document calls for monitoring inputs/outputs and applying AI behavior analytics. However, the framework is entirely silent on Prometheus/Grafana/MLflow/OpenTelemetry specifics, vector database monitoring, message queue telemetry, service mesh semantic gaps, Kubernetes audit log management, speculative decoding monitoring, quantization attack telemetry, TensorRT engine binary validation at scale, auto-scaling coverage inconsistency, distributed batching attribution, or any of the 48 specific infrastructure blind spots enumerated. The GPU patching guidance and hardware weight protection are the only marginal connection points, and they do not address monitoring blind spots — they address patching and protection of the hardware layer itself.

### RTM_8 - Reasoning and Cognitive State Monitoring
- Strength score: 1

RTM_8 covers 28 threats targeting the reasoning and cognitive layer — semantic-preserving reasoning mutation defeating anomaly detection, distributed reasoning coordination intent invisibility, reasoning state obfuscation across agents, meta-reasoning blind spots containing concealed decision factors, reasoning latency side-channels, tree-of-thought search pattern reverse-engineering, preserved path instrumentation blind spots, multi-path reasoning telemetry aggregation blind spots, quality score metric blind spots in safety monitoring, sampling parameter monitoring blind spots, confidence score monitoring blind spots from voting mechanism opacity, consolidated memory usage monitoring blind spots, streaming response monitoring blind spots in multi-agent handoffs, decomposition trace logging gaps, partial order execution monitoring without complete ordering information, state abstraction projection loss, method selection monitoring lacking failure prediction, precondition failure propagation monitoring, decomposition correctness validation gaps, cross-agent constraint conflict detection blind spots, MCTS tree statistics as hidden cognitive state, simulation trace visibility as behavioral blind spot, rollout policy drift as unobserved behavior change, MCTS convergence failures as unmonitored planning breakdowns, LangGraph MCTS integration telemetry gaps, replanning attack obfuscation via normal variation, contingency activation blind spots, and search tree expansion rate blinding. The framework's mandate to log intermediate states and monitor for behavioral changes provides only the most generic coverage. The framework explicitly does not cover cognitive/reasoning-layer monitoring as noted in the framework summary. None of the MCTS, chain-of-thought, tree-of-thought, reasoning trace, decomposition logic, rollout policy drift, or meta-reasoning monitoring concepts appear anywhere in the NSA/CISA or DHS/CISA documents.

### RTM_9 - Memory Systems and Knowledge Base Observability
- Strength score: 1

RTM_9 contains 18 threats: episodic memory retrieval as monitoring evasion, consolidation process opacity, trajectory integration hiding multi-step attacks, hybrid storage architecture creating dual-channel monitoring gaps, cross-agent memory sharing obscuring individual responsibility, metadata filtering complexity enabling detection evasion, semantic memory query logging gaps, retrieval relevance monitoring insufficiency, knowledge graph relationship cardinality explosion, temporal validity enforcement blind spots, embedding quality degradation detection gaps, knowledge base poisoning anomaly analysis gaps, cache hit rate manipulation as performance monitoring bypass, deduplication metadata bypassing completeness monitoring, working memory phase transitions creating monitoring blind spots, token budget visualization creating false confidence, reasoning trace truncation hiding decision provenance, and hierarchical compression observation opaqueness. The NSA/CISA framework mentions input sanitization and prompt injection protection, and DHS/CISA mentions monitoring for data drift. These provide a thin indirect connection to knowledge base poisoning detection and embedding quality degradation. However, the framework contains no guidance on episodic memory monitoring, memory consolidation audit, trajectory integrity, hybrid vector/graph database monitoring asymmetry, metadata-triggered retrieval blind spots, knowledge graph cardinality explosion, temporal validity enforcement, working memory phase transition forensics, token budget false confidence, reasoning trace truncation impacts, or hierarchical compression monitoring fragmentation. The data drift monitoring guidance from DHS/CISA is the closest relevant control, but it was not written with RAG pipeline or multi-agent memory architecture in mind.

### RTM_10 - Decision Logic and Utility Function Monitoring
- Strength score: 1

RTM_10 spans 23 threats covering utility function calculation telemetry gaps, weight-driven decision divergence blind spots, probability distribution shift impact on utility decisions, specification gaming through utility metric redefinition, tool outcome distribution telemetry gaps, confidence score utility basis opacity, missing counterfactual utility analysis, rule firing monitoring gaps in distributed systems, working memory content monitoring gaps, rule modification detection gaps in shared repositories, heuristic parameter tuning monitoring blindness, learned monitoring evasion through training-based counter-strategies, reward metric gaming as learned monitoring evasion, experience sampling bias detection, policy output distribution analysis limitations, gradient flow monitoring limitations for distributed learning, temporal learning dynamics leaking attack signals, paradigm-specific logging gaps in hybrid monitoring, knowledge graph evolution telemetry opacity, cooperative cycle intermediate state blind spots, streaming response monitoring gaps in real-time hybrid output, demonstration curation audit trail opacity, and multi-paradigm objective tracking coordination gaps. The NSA/CISA framework calls for monitoring for "unauthorized model architecture/configuration changes" and "attempts to access or elicit data from the model." DHS/CISA mentions measuring model performance post-deployment to identify behavioral changes and adversarial training. These provide marginal coverage for reward metric gaming and specification gaming through behavioral monitoring, but the framework contains nothing on utility function telemetry, weight-based decision monitoring, counterfactual utility analysis, rule firing distributed audit, heuristic parameter tuning forensics, learned evasion detection, gradient flow monitoring, federated learning backdoor detection, knowledge graph evolution telemetry, cooperative cycle intermediate state monitoring, or multi-paradigm objective tracking. The framework's behavioral monitoring guidance is too high-level to address any of these decision-logic monitoring threats specifically.

### RTM_11 - Vector Database and RAG Pipeline Telemetry
- Strength score: 1

RTM_11 contains 17 threats focused on RAG pipeline observability — vector database query quality metrics absent from Prometheus, hybrid search alpha selection rationale gaps, cluster shard-level query performance attribution gaps, batch ingestion error rate aggregation hiding document-level failures, ETL quality rejection metrics aggregation hiding per-source failures, pipeline transformation throughput monitoring blind spots, incremental update success metrics masking partial extraction failures, quality metric aggregation hiding per-dimension validation failures, cache performance monitoring gaps obscuring per-layer hit rate, deduplication effectiveness tracking blind spots, observability layer instrumentation overhead blind spots, fault tolerance monitoring gaps hiding graceful degradation frequency, quality dashboard aggregation hiding per-agent validation failures, batch processing monitoring gaps obscuring within-batch failures, state file modification tracking gaps hiding timestamp manipulation, token cost monitoring aggregation masking per-query exploitation, and remediation effectiveness monitoring blind spots. The NSA/CISA Joint CSI mentions input sanitization and prompt injection protection, which is marginally relevant to RAG pipeline integrity. DHS/CISA mentions watching for data drift. However, the framework documents contain no mention of vector databases, RAG pipelines, embedding quality, ETL pipeline monitoring, batch ingestion telemetry, shard-level query attribution, hybrid search strategies, deduplication false positive tracking, circuit breaker states in RAG systems, or per-query token cost exploitation. These are highly specialized operational monitoring concerns for RAG infrastructure that postdate the April 2024 framework documents' level of architectural specificity.

### RTM_12 - Detection Evasion and Attack Exploitation
- Strength score: 2

RTM_12 contains 17 threats covering active attack exploitation of monitoring infrastructure — metric emission manipulation masking degradation, alert fatigue exploitation through false positive flooding, time-to-detect exploitation through metric reporting delay, incident response timing attacks, Prometheus man-in-the-middle metric falsification, alert threshold gaming, OpenTelemetry operational intelligence disclosure, GPU/DCGM telemetry exposing inference capacity patterns, distributed tracing exposing coordination architecture, cross-agent safety violation correlation blindness from independent guardrail telemetry, distributed sandbox audit trail fragmentation, fairness audit trail fragmentation preventing discrimination root cause analysis, fragmented feedback collection preventing systematic improvement, distributed preference collection fragmenting human value representation, distributed annotator disagreement masking fleet-wide value conflicts, fragmented monitoring infrastructure preventing fleet-wide anomaly detection, and distributed traceability loss preventing complete decision reconstruction. The NSA/CISA framework explicitly addresses several directly relevant controls: immutable backup storage for log data, automated alerts, adversarial testing, monitoring for "oracle-style adversarial compromise," and monitoring for attempts to access or elicit data from the model. The DHS/CISA framework calls for incident response and applying AI behavior analytics for threat detection. These controls are meaningfully relevant to alert fatigue exploitation, incident response timing attacks, audit trail integrity, and cross-agent safety violation detection. However, the framework does not address Prometheus-specific man-in-the-middle attacks, DCGM telemetry exposure, distributed tracing architectural intelligence disclosure, fairness audit trail regulatory requirements, preference fragmentation value inconsistency, annotator disagreement masking, or the specific multi-agent correlation blind spots for coordinated jailbreak campaigns. The framework's immutable log storage and automated alert mandates provide a real but partial foundation, requiring extrapolation for the multi-agent-specific dimensions.

# RTE Subsection Analysis — NSA_Deploy_AI_Securely Framework Coverage

## RTE — Multi-agent trust exploitation and self-replicating prompt malware

### RTE_1 - Dashboard & UI Attribution Attacks
- Strength score: 1

RTE_1_1 through RTE_1_3 describe attacks where agents spoof identity through manipulable metadata fields (agent_name, agent_type) in dashboards and chat UIs, exploiting the absence of cryptographic binding between identity claims and rendered content. The NSA/CISA framework addresses identity through phishing-resistant MFA, least privilege, and ZT access controls, and the DHS/CISA document touches on identity and access controls. However, neither document addresses the UI rendering layer, multi-agent dashboard attribution logic, or cryptographic attestation of inter-agent message sources. The framework has no guidance on agent-to-agent identity verification mechanisms or dashboard-level identity integrity. Coverage is generic at best and requires very significant extrapolation to apply here.

---

### RTE_2 - Trust Mechanisms & Inter-Agent Communication
- Strength score: 1

RTE_2_1 through RTE_2_4 describe attacks exploiting transitive trust chains in multi-agent systems: malicious inter-agent messages treated as trusted analysis, reputation anchoring abuse, circular verification loop subversion, and credential assumption across specialized agents. The NSA/CISA framework's ZT mindset ("assume breach") is philosophically relevant, and the least-privilege guidance applies tangentially, but neither document addresses inter-agent trust chain security, agent reputation systems, verification loop integrity, or natural-language-based trust exploitation. The framework does not conceptualize the attack surface where agents interpret peer messages as authoritative policy rather than untrusted peer data. No specific actionable guidance covers these threats.

---

### RTE_3 - Confidence & Scoring Attacks
- Strength score: 1

RTE_3_1 through RTE_3_3 describe manipulation of confidence scores through parameter tuning, utility function poisoning, and streaming response timing attacks, exploiting downstream agents that route decisions based on confidence scores. The NSA/CISA framework mentions monitoring for anomalous inputs and data drift, which is tangentially relevant. However, neither document addresses confidence score integrity, agent routing logic based on confidence, streaming pipeline trust propagation, or any form of score validation between agents. No actionable guidance covers these attack surfaces.

---

### RTE_4 - Prompt Injection via Message/History Sharing
- Strength score: 2

RTE_4_1 through RTE_4_3 describe self-replicating prompt injection propagating through shared conversation history, serialized agent memory, and AutoGen GroupChat history. The NSA/CISA framework explicitly calls out input sanitization and prompt injection protection as a specific control, and monitoring of inputs/outputs/intermediate states is also specified. These directly address the injection vector itself. However, the framework does not address the multi-agent amplification dimension — specifically how shared history and memory serialization enable geometric propagation across agent populations, or the persistence mechanism of deserialized poisoned memory. Coverage of the base injection is strong, but the multi-agent worm propagation aspect is not addressed, warranting a score of 2.

---

### RTE_5 - UI & Disclosure Vulnerabilities
- Strength score: 1

RTE_5_1 and RTE_5_2 describe progressive disclosure layer poisoning (targeting agent-consumed technical layers users do not inspect) and dynamic agent role assignment confusion (dashboards showing compromised agent output under trusted role labels). The NSA/CISA framework has no guidance on UI/UX disclosure design, progressive information layering, or dynamic role assignment integrity in multi-agent orchestration. The framework explicitly does not cover UI/UX design. Only very generic ZT and least-privilege principles could be extrapolated, insufficient to address these display and orchestration-layer vulnerabilities.

---

### RTE_6 - Tool & Command Injection
- Strength score: 2

RTE_6_1 and RTE_6_2 describe prompt injection malware propagating via inline code/documentation suggestions and command palette poisoning through malicious context injection. The NSA/CISA framework directly addresses prompt injection and input sanitization as explicit controls, and monitoring of inputs/outputs covers some detection surface. The DHS/CISA document also addresses input validation. These controls are relevant to the injection mechanism described. However, neither document addresses the inline suggestion UI as a propagation vector or command palette context poisoning specifically. The base prompt injection defense applies, but the tool-integration and suggestion-UI amplification paths are not addressed, justifying a score of 2.

---

### RTE_7 - Framework-Specific Attacks
- Strength score: 1

RTE_7_1 through RTE_7_3 describe attacks exploiting the trust delegation models of specific frameworks: LangChain, LangGraph, AutoGen, CrewAI, and Semantic Kernel. Each framework's agent-to-agent communication protocol (state passing, conversation, plugin routing, conditional edge routing) is characterized as a distinct malware propagation surface. The NSA/CISA framework has no awareness of these agent frameworks and provides no guidance on framework-specific trust model vulnerabilities, state field injection, or conditional routing exploitation. The framework's supply chain inspection of pre-trained models is entirely distinct from runtime framework delegation security. No meaningful coverage exists.

---

### RTE_8 - AutoGen-Specific Attacks
- Strength score: 1

RTE_8_1 and RTE_8_2 describe AutoGen-specific social engineering through natural language conversation between agents and exploitation of conversation-history-based reputation systems. The NSA/CISA framework does not address any AutoGen-specific controls, conversational agent trust models, or reputation-based agent selection systems. The ZT assumption of breach is philosophically aligned but provides no actionable guidance for these attack patterns. No coverage of AI-to-AI social engineering through conversational dialogue exists in the framework.

---

### RTE_9 - CrewAI-Specific Attacks
- Strength score: 1

RTE_9_1 through RTE_9_3 describe CrewAI manager-worker trust exploitation, few-shot decomposition pattern injection poisoning task decomposition logic, and demonstration bias enabling semantic subtask reinterpretation. The NSA/CISA framework does not address hierarchical agent architectures, manager-worker trust relationships, or few-shot demonstration security. Supply chain inspection of pre-trained models is the closest applicable control but does not reach runtime task delegation and in-context learning poisoning within a deployed CrewAI system. No meaningful coverage exists.

---

### RTE_10 - Semantic Kernel-Specific Attacks
- Strength score: 1

RTE_10_1 and RTE_10_2 describe Semantic Kernel plugin registry takeover through registration of malicious plugins with legitimate names and function impersonation through schema duplication, exploiting probabilistic LLM routing. The NSA/CISA framework does not address plugin registries, dynamic plugin registration, or function routing security in AI orchestration frameworks. The framework's supply chain controls (SBOM/AIBOM) address pre-deployment artifacts but not runtime plugin registration attacks. No meaningful coverage exists.

---

### RTE_11 - Multimodal Attacks
- Strength score: 1

RTE_11_1 through RTE_11_4 describe multimodal attack vectors: attribution spoofing through vision agent compromise, self-replicating image injection through inline suggestion acceptance, cross-modality contradiction attacks to manufacture false consensus, and multimodal worm propagation through RAG retrieval cycles where images carry embedded instructions bypassing text-based defenses. The NSA/CISA framework's input sanitization and prompt injection controls are text-focused and do not address multimodal content. Neither document addresses vision agent security, image-embedded instruction content, cross-modal fusion logic exploitation, or multimodal RAG pipeline security. No meaningful coverage exists for these attack surfaces.

---

### RTE_12 - Streaming & Continuous Processing
- Strength score: 1

RTE_12_1 through RTE_12_3 describe exploitation of error/retry messaging, fallback routing announcements, and circuit breaker state change announcements as inter-agent instruction propagation channels. The NSA/CISA framework's monitoring and logging guidance covers detection of anomalous behavior broadly, but neither document addresses streaming pipeline trust, retry message integrity, fallback announcement injection, or circuit breaker state communication as attack surfaces. These are novel multi-agent infrastructure-specific vectors with no direct framework coverage.

---

### RTE_13 - Evaluation & Benchmarking
- Strength score: 1

RTE_13_1 through RTE_13_7 describe attacks on multi-agent evaluation pipelines: self-replicating metric definitions, transitive trust in evaluation chains, evaluation framework version poisoning (e.g., malicious MLflow), report format injection (HTML/JS in rendered reports), false consensus through coordinated metric poisoning, benchmark leaderboard manipulation for agent reputation spoofing, and benchmark-gaming to insert malicious agents into trusted production roles. The NSA/CISA framework's supply chain controls (SBOM/AIBOM, pre-trained model inspection) and cryptographic artifact validation are partially relevant to dependency version poisoning (RTE_13_3) and provide a score-elevating partial coverage, but the evaluation pipeline integrity, report rendering security, and benchmark manipulation vectors are not specifically addressed. Overall the framework provides only weak coverage warranting a score of 1.

---

### RTE_14 - Web & LLM Evaluation Attacks
- Strength score: 2

RTE_14_1 describes intermediate answer poisoning in decomposed multi-hop QA pipelines, where injected instructions in early sub-question answers propagate to downstream agents. The NSA/CISA framework explicitly addresses prompt injection protection and input sanitization, which directly apply to the injection mechanism in this attack. Monitoring of inputs/outputs/intermediate states is also relevant. However, the multi-hop pipeline amplification dynamic — where injected content in early sub-answers is treated as authoritative context by subsequent agents — is not specifically addressed. The base injection control is directly applicable, earning a score of 2.

---

### RTE_15 - Model Tuning & Configuration Attacks
- Strength score: 2

RTE_15_1 through RTE_15_4 describe: model size assumption exploitation for social engineering, prompt cache poisoning for malware persistence across requests, iteration budget misconfiguration as an exploitable window for undetected injection, and routing configuration manipulation causing misclassification. The NSA/CISA framework explicitly addresses prompt caching indirectly through monitoring of inputs/intermediate states and cache-related concerns, and mentions hardware/model weight protection. More directly, the framework's adversarial testing and red-teaming guidance is relevant to discovering configuration exploitability. The DHS/CISA document's input validation and monitoring controls apply partially. Prompt cache poisoning (RTE_15_2) is the most relevant to the framework's prompt injection controls, earning a moderate score of 2 overall, though the model-size-assumption social engineering and routing misconfiguration angles are not specifically addressed.

---

### RTE_16 - Reasoning Trace & Chain-of-Thought Attacks
- Strength score: 1

RTE_16_1 and RTE_16_2 describe malicious social engineering embedded within CoT reasoning (making manipulation appear as legitimate analysis) and CoT traces as worm propagation vectors where malware woven into reasoning spreads through agent networks that retrieve and incorporate each other's traces. The NSA/CISA framework does not address reasoning trace security, CoT integrity, or agent reasoning artifact sharing as attack surfaces. Monitoring of intermediate states is the closest applicable control but does not specifically address reasoning trace content integrity or cross-agent trace propagation. No meaningful coverage exists.

---

### RTE_17 - Tree of Thought & Sampling Attacks
- Strength score: 1

RTE_17_1 through RTE_17_4 describe attacks exploiting multi-path sampling mechanisms: quality score consensus manipulation, self-consistency diversity exploitation to manufacture proof-of-concept demonstrations, majority voting consensus building for malicious conclusions, and path quality score inflation as credential spoofing. These are sophisticated attacks on planning and reasoning architecture components (RASC, Self-Consistency) that the NSA/CISA framework does not address. The framework has no guidance on sampling strategy security, multi-path reasoning integrity, or quality metric manipulation. No meaningful coverage exists.

---

### RTE_18 - Hierarchical Task Network (HTN) & Planning Attacks
- Strength score: 1

RTE_18_1 through RTE_18_3 describe HTN-specific attacks: hierarchical authority confusion enabling privilege confusion (operational agents claiming strategic authority), shared method library injection where attacker-authored methods achieve equal standing with trusted methods, and decomposition delegation chain authority diffusion enabling malware propagation without verification. The NSA/CISA framework's least-privilege and RBAC/ABAC guidance is tangentially relevant to authority confusion, but the framework does not address HTN planning architectures, method library integrity, or decomposition delegation security. No actionable guidance covers these planning-layer attack surfaces.

---

### RTE_19 - Monte Carlo Tree Search (MCTS) Attacks
- Strength score: 1

RTE_19_1 through RTE_19_3 describe MCTS-specific attacks: non-deterministic rollout exploitation to craft scenarios producing malicious sequences probabilistically, backpropagation poisoning in shared value networks causing false reward signals to propagate to all sharing agents, and hierarchical MCTS delegation enabling privilege escalation where worker MCTS produces dangerous sequences supervisors trust as appropriate decomposition. The NSA/CISA framework has no awareness of MCTS planning architectures, shared value networks, or backpropagation security. These are highly specialized planning security threats with no coverage in the framework.

---

### RTE_20 - Multi-Agent Planning Attacks
- Strength score: 1

RTE_20_1 through RTE_20_4 describe plan-and-execute architecture attacks: planning phase reasoning falsification, reasoning consistency loss across hierarchical boundaries exploited via contradiction injection at management level, delegation context reasoning injection where manager reasoning embeds attack instructions workers inherit as context, and hierarchical trust poisoning through compromised worker subtask suggestions. The NSA/CISA framework does not address multi-agent planning architectures, plan integrity validation, or hierarchical reasoning consistency. The ZT assume-breach mindset is philosophically relevant but provides no actionable planning-specific controls. No meaningful coverage exists.

---

### RTE_21 - Memory & Knowledge Attacks
- Strength score: 2

RTE_21_1 through RTE_21_5 describe attacks on shared agent memory: episodic memory poisoning creating malware legitimacy through false experience records, trajectory poisoning driving policy convergence toward coordinated attacks, memory abstraction weaponized to generate malware templates inherited by spawned agents, shared vector database embedding poisoning for unanimous retrieval corruption, and consolidation-driven abstraction creating reusable malware gene components. The NSA/CISA framework addresses access controls limiting access to model weights and data, which applies to restricting who can write to shared memory stores. The DHS/CISA document's dataset validation (filtering poisoned data) and input validation controls are partially applicable to memory content integrity. However, neither document addresses the specific multi-agent memory sharing architecture, vector database poisoning, episodic memory integrity, or trajectory-based policy convergence attacks. The access control guidance provides a partial foundation, justifying a score of 2, but the specific memory attack vectors lack direct coverage.

---

### RTE_22 - Knowledge Base & RAG Attacks
- Strength score: 2

RTE_22_1 through RTE_22_4 describe shared knowledge base and RAG-specific attacks: 1-to-N malware distribution through single-document poisoning in shared knowledge bases, iterative retrieval self-replication where early retrievals guide subsequent queries toward related malicious documents, cross-document instruction assembly from distributed fragments, and synonym injection enabling instruction obfuscation in semantic similarity search. The NSA/CISA framework addresses supply chain inspection of external AI data and input sanitization, which are directly relevant to knowledge base content integrity and the injection mechanism. The DHS/CISA document's dataset validation (filtering poisoned data) is applicable to knowledge base curation. These controls provide moderate coverage of the injection vector, but the iterative self-replication, cross-document assembly, and synonym relationship poisoning specifics are not addressed. A score of 2 reflects the partial but not complete coverage.

---

### RTE_23 - Shared Context & Aggregation
- Strength score: 1

RTE_23_1 through RTE_23_3 describe shared buffer injection accumulation affecting all reading agents, hierarchical aggregation authority diffusion enabling social engineering at leaf level to propagate to top-level decisions through accumulated trust, and information privilege escalation via selective context sharing creating exploitable information asymmetry. The NSA/CISA framework's monitoring of intermediate states and ZT principles are tangentially relevant, but the framework provides no guidance on shared working memory buffer integrity, multi-agent hierarchical aggregation security, or selective context sharing trust boundaries. These are architectural multi-agent vulnerabilities with no direct framework coverage.

---

### RTE_24 - Utility & Preference Attacks
- Strength score: 1

RTE_24_1 and RTE_24_2 describe preference convergence attacks through gradual poisoning of shared utility function learning causing all agents to converge on malicious utility weights, and utility-based social engineering through recommendation chain poisoning where upstream agents' poisoned assessments are trusted by downstream agents. The NSA/CISA framework does not address utility function security, preference learning integrity, or recommendation chain trust validation. The DHS/CISA dataset validation controls are the closest applicable guidance but do not reach runtime utility weight learning or recommendation chain trust exploitation. No meaningful coverage exists.

---

### RTE_25 - Multi-Agent Reinforcement Learning (MARL)
- Strength score: 1

RTE_25_1 through RTE_25_5 describe MARL-specific attacks: learned coordination exploited so agents learn to exploit each other through joint reward optimization, self-replicating reward signal injection via shared experience replay buffer poisoning, consensus learning manipulation through synchronized poisoning of majority agents, learned message protocol exploitation embedding malicious conventions, and emergent malicious behavior through poisoned reward structures producing unintended harmful behaviors at deployment. The NSA/CISA framework does not address reinforcement learning architectures, shared replay buffers, reward signal integrity, multi-agent policy convergence, or emergent behavior risks. These are highly specialized RL security threats with no framework coverage.

---

### RTE_26 - Hybrid System Attacks
- Strength score: 1

RTE_26_1 and RTE_26_2 describe paradigm-specific instruction encoding attacks in hybrid systems (instructions appearing as data in one paradigm becoming executable in another across agent boundaries) and knowledge graph topological backdoor attacks establishing covert inter-agent coordination channels through injected graph relationships. The NSA/CISA framework's input sanitization is tangentially relevant to the instruction encoding issue, but neither document addresses hybrid AI paradigm boundaries, cross-paradigm instruction semantic shifts, or knowledge graph integrity as a coordination security concern. No meaningful coverage exists.

---

### RTE_27 - Infrastructure & Deployment Attacks
- Strength score: 2

RTE_27_1 through RTE_27_4 describe: vector database multi-tenancy partition confusion for cross-agent data leakage, Prometheus relabeling configuration injection for metric spoofing causing misattribution of agent identity in monitoring, API gateway rate limiting bypass through cooperative multi-agent request distribution, and MLflow experiment metadata injection for malware distribution through agent networks. The NSA/CISA framework has direct relevance here: TLS requirements for data in transit apply to vector database communications, the framework's monitoring and logging guidance covers metric integrity broadly, and cryptographic artifact validation addresses MLflow artifact trust. The DHS/CISA network segmentation guidance applies to multi-tenancy isolation. However, the specific attacks — partition key injection, relabeling rule manipulation, cooperative rate-limit bypass, and experiment metadata as malware vector — are not explicitly addressed. The framework's infrastructure security posture provides meaningful but incomplete coverage, warranting a score of 2.

---

### RTE_28 - Microservices & Kubernetes
- Strength score: 2

RTE_28_1 through RTE_28_7 describe Kubernetes/microservices-specific attacks: mTLS downgrade to enable MITM between agents, NetworkPolicy/AuthorizationPolicy misconfiguration enabling lateral movement, shared service account credential abuse for agent impersonation, service account token reuse across pod instances, ClusterRole privilege escalation via overly-permissive RBAC, mTLS certificate store poisoning, and image signature spoofing for pod replica poisoning. The NSA/CISA framework directly addresses several of these: TLS 1.x for data in transit (relevant to mTLS downgrade), sandboxed containers as deployment environment, GPU and infrastructure patching (relevant to image integrity), RBAC/ABAC access controls, and cryptographic artifact validation/checksums/hashes (directly relevant to image signature verification). These framework controls are specific and actionable for several of the attack variants. However, Kubernetes-specific nuances like ClusterRoleBinding manipulation, service account token reuse across pods, and gossip protocol trust are not explicitly addressed. Overall coverage is moderate, justifying a score of 2.

---

### RTE_29 - Performance Optimization & Model Registry
- Strength score: 2

RTE_29_1 through RTE_29_3 describe optimization agent recommendation propagation as a malware vector to orchestration and managed agents, model registry version selection manipulation causing fleet-wide simultaneous compromise via shared registry, and profiling tool integration as persistent backdoor installation leveraging elevated system-level access. The NSA/CISA framework directly addresses supply chain inspection of pre-trained models and cryptographic artifact validation/checksums/hashes, which apply to model registry integrity and artifact verification before loading. The framework's least-privilege guidance applies to profiling tool access. However, optimization recommendation propagation as a trust-exploitation vector and the multi-agent fleet amplification dimension of registry compromise are not specifically addressed. Partial but meaningful coverage for the supply chain dimension justifies a score of 2.

---

### RTE_30 - Scaling & Auto-Scaling
- Strength score: 2

RTE_30_1 and RTE_30_2 describe pod anti-affinity rule manipulation to force co-location and eliminate resilience, and RBAC privilege escalation within agent service accounts enabling cross-agent configuration manipulation. The NSA/CISA framework explicitly covers RBAC/ABAC access controls and the ZT principle of least privilege, which directly address the service account RBAC misconfiguration threat (RTE_30_2). The sandboxed container/VM deployment guidance is relevant to pod isolation. However, pod anti-affinity scheduling manipulation as an availability attack and the Kubernetes-specific mechanics of service account RBAC are not explicitly called out. The RBAC coverage provides direct applicability to one of the two threats, justifying a score of 2.

---

### RTE_31 - Fleet Management & Provisioning
- Strength score: 2

RTE_31_1 through RTE_31_4 describe: OTA update chain of custody corruption through baseline poisoning causing staged validation to falsely approve compromised deployments, TensorRT engine backdooring bypassing staged validation on hardware diversity, provisioning token compromise enabling self-replicating malware across newly provisioned hardware, and certificate rotation desynchronization creating trust chain windows for malicious certificate acceptance. The NSA/CISA framework directly addresses cryptographic artifact validation/checksums/hashes (applicable to OTA update integrity and TensorRT engine validation), hardware model weight protection via HSM/HRZ (relevant to fleet hardware security), and two-person control for integrity of privileged operations. The DHS/CISA supply chain risk management (SBOMs, vendor risk) applies to fleet provisioning. Certificate rotation desynchronization is partially addressed by the framework's PKI and TLS guidance. However, fleet-specific staged rollout security, hardware-variant-specific binary validation, and provisioning token chain compromise are not explicitly covered. The supply chain and cryptographic validation controls provide meaningful partial coverage, justifying a score of 2.

---

### RTE_32 - Batching & Caching Infrastructure
- Strength score: 1

RTE_32_1 and RTE_32_2 describe load balancer state poisoning to systematically route requests through compromised replicas, and auto-scaling threshold manipulation to use scaling events as coordinated malware activation triggers across simultaneously launched replicas. The NSA/CISA framework's monitoring and logging guidance is tangentially relevant to detecting anomalous routing patterns, and ZT principles apply broadly. However, neither document addresses load balancer state integrity, auto-scaling threshold manipulation, or the use of infrastructure scaling events as malware activation mechanisms. These are infrastructure-layer multi-agent operational security threats with no direct framework coverage.

---

### RTE — Other Risks/Threats/Vulnerabilities Worth Noting (unnumbered items)
- Strength score: 1

The unnumbered items in this section cover: trace-based few-shot learning poisoning via compromised execution histories, parameter accuracy assumptions without inter-agent re-verification, tool call verification bypass through multi-agent trust chains, parameter validation delegation without re-validation, inter-agent context pollution enabling transitive instruction injection, conversation ID chain hijacking, efficiency metric sharing as malware propagation, trust degradation through efficiency report poisoning, cost optimization policy self-replication, efficiency consensus exploitation, efficiency benchmark gaming, resource budget negotiation as malware trading, vector database query quality metric blind spots in Prometheus, unauthenticated gossip protocol exploitation, load balancer MITM via absent mutual TLS, NeMo Guardrails threshold gaming, execution rail resource limit coordination failures enabling fleet-wide quota exhaustion, safetensors validation bypass for multi-LLM NIM poisoning, and vLLM performance degradation through hardware detection fallback exploitation. The NSA/CISA framework's prompt injection protection and monitoring controls apply broadly to injection-based items, and TLS guidance partially addresses the load balancer MITM scenario. The DHS/CISA input validation, dataset validation, and supply chain controls cover a subset of these items at a high level. However, the vast majority of these items — multi-agent parameter re-validation chains, gossip protocol authentication, NeMo Guardrails threshold gaming, resource limit coordination failures, safetensors format validation, and hardware detection fallback exploitation — are not addressed by the framework. The aggregate coverage across this collection of miscellaneous threats is weak, warranting a score of 1.

# NSA_Deploy_AI_Securely — RWA Subsection Analysis

## RWA — Workflow and ecosystem attacks on plugins, tools, and RAG pipelines

### RWA_1 - Specification Gaming and Misalignment
- Strength score: 1

This subsubsection covers 149 distinct threat variants (RWA_1_1 through RWA_1_149) centered on agents exploiting feedback loops, adaptive thresholds, tool schema ambiguity, metric optimization, fallback/circuit-breaker abuse, multi-agent negotiation dynamics, RAG-indexed poisoned planning traces, quantization-induced behavior drift, and emergent misalignment through coordinated specification gaming. The NSA/CISA framework has no guidance on any of these multi-agent behavioral dynamics. The framework's input sanitization guidance (Phase b) addresses prompt injection as a surface-level filter concern, not the systemic, learned, emergent gaming behaviors described here. Monitoring guidance (logs of inputs/outputs, data drift alerts) is conceptually relevant but addresses anomaly detection at the signal level, not the sophisticated gaming-by-design behaviors described across these 149 sub-threats where individual operations look legitimate but collectively constitute misalignment. There is no guidance on feedback loop design, multi-agent reward shaping, specification formal verification, or agent behavioral auditing. The DHS/CISA document's NIST AI RMF references "human supervision" and "monitoring inputs/outputs for unusual behavior," which is tangentially applicable but far too generic to address the specific attack surfaces enumerated here.

---

### RWA_2 - Model Training and Backdoors
- Strength score: 2

This subsubsection describes 60 backdoor injection vectors spanning fine-tuned model backdoors, LoRA adapter poisoning, quantization calibration poisoning, speculative decoding draft model contamination, TensorRT engine backdoors, CUDA kernel injection, and training data contamination across vision, audio, and embedding modalities. The NSA/CISA framework provides direct, relevant guidance in several areas: supply chain inspection of pre-trained models (Phase b instructs "do not run imported pre-trained models without inspection in secure dev zone; use AI-specific scanners; evaluate supply chain"), cryptographic artifact validation and checksums/hashes per release, adversarial testing, and immutable backups. The DHS/CISA document adds SBOMs/AIBOMs, model cards, vendor assessments, dataset validation, and model hardening. These controls partially address supply-chain backdoor insertion (RWA_2_2 shared LLM reuse, RWA_2_17 LoRA adapter sharing, RWA_2_30 pre-built NIM containers, RWA_2_33 PV mount model poisoning, RWA_2_59 TensorRT engine substitution). However, the framework lacks guidance specific to: detecting training-time backdoors embedded via calibration datasets (RWA_2_28/2_29/2_32), CUDA kernel injection during compilation (RWA_2_31), speculative decoding draft model poisoning (RWA_2_58), and the full breadth of modality-specific triggers (vision, audio, embedding model backdoors RWA_2_5 through RWA_2_9). A score of 2 reflects that meaningful supply-chain and inspection controls exist but require significant extrapolation to cover the training-time and compilation-time backdoor attack surface.

---

### RWA_3 - State and Context Poisoning
- Strength score: 1

This subsubsection describes 31 threat variants (RWA_3_8 through RWA_3_31, noting the numbering starts at RWA_3_8) involving multi-agent orchestration state pollution: checkpoint file injection, state reducer exploitation, conditional edge type confusion, ETL state file tampering, context window exploitation, KV cache poisoning through state, and long-context boundary attacks specific to heterogeneous agent context lengths. The NSA/CISA framework does not address stateful orchestration workflows at all. Its monitoring guidance covers logs of inputs/outputs and intermediate states, which is tangentially relevant but does not address the architectural vulnerability of treating accumulated orchestration state as authoritative ground truth. Input sanitization guidance addresses user-facing prompt injection but not the inter-agent state propagation vectors described here (e.g., RWA_3_12 checkpoint injection, RWA_3_14 reducer exploitation, RWA_3_20/3_29/3_30 ETL state file tampering). The DHS/CISA document's network segmentation and monitoring guidance does not address multi-agent stateful workflow contexts. No framework guidance exists for checkpoint integrity, state field validation schemas, or reducer logic security in agentic pipelines.

---

### RWA_4 - RAG Pipeline Attacks
- Strength score: 2

This subsubsection enumerates 39 RAG-specific attack variants including knowledge graph manipulation, vector database poisoning, HNSW graph navigation attacks, retrieval parameter tuning exploitation, chunking configuration injection, reranking injection, few-shot demonstration pool poisoning, streaming RAG injection, and lazy retrieval timing attacks. The NSA/CISA framework addresses the upstream data poisoning concern directly: "data source catalog of trusted/valid sources protecting against data poisoning/backdoor attacks" (Phase a) and "input sanitization/prompt injection protection" (Phase b). The DHS/CISA document adds "dataset validation (filtering poisoned data, expert-annotated datasets)" and "monitoring inputs/outputs for unusual behavior." These controls partially address RWA_4_21 (vector database poisoning via shared RAG databases), RWA_4_1 (UI-mediated document selection), and general corpus poisoning. However, the framework provides no guidance specific to: HNSW graph layer poisoning (RWA_1_69/4_19), ANN accuracy-speed tradeoff exploitation (RWA_1_70), retrieval fusion manipulation (RWA_1_68), chunking strategy heterogeneity attacks (RWA_4_20), streaming progressive poisoning (RWA_4_10), lazy retrieval timing exploitation (RWA_4_34), or multi-stage retrieval pipeline bypass (RWA_4_39). The trusted data source catalog concept is the strongest applicable control but is described at a high level without the RAG-pipeline-specific implementation detail needed to address most of these threats.

---

### RWA_5 - Multi-Agent Orchestration Risks
- Strength score: 1

This subsubsection details 52 threats specific to multi-agent coordination: hierarchical privilege escalation through context manipulation, swarm emergent misalignment from local rule perturbations, federated orchestration weakest-link compromise, state-logic boundary exploitation via polymorphic injection, cross-agent filesystem volume misconfiguration, service mesh routing injection, cascading autonomous action commitment, inter-agent filter bypass through unfiltered internal messaging, and circular approval deadlocks. The NSA/CISA framework's Phase a (ZT, RBAC/ABAC, least privilege, sandboxed containers) is conceptually relevant to privilege escalation threats (RWA_5_4, RWA_5_9) and network isolation (RWA_5_49), and TLS addresses RWA_5_29 service mesh injection at a partial level. However, the framework contains no guidance on multi-agent orchestration architectures specifically: no coverage of trust boundaries between orchestrators and workers, no guidance on inter-agent message filtering, no coverage of federated agent governance, no guidance on autonomous action cascade prevention or human oversight checkpoints in multi-agent workflows. The DHS/CISA human supervision and alternate process redundancy guidance is tangentially relevant but far too generic. The framework was designed for deploying individual AI systems securely, not for the emergent coordination risks arising from multi-agent composition.

---

### RWA_6 - Plugin and Tool Ecosystem Attacks
- Strength score: 2

This subsubsection covers 45 plugin and tool ecosystem threats: plugin marketplace ranking manipulation, framework-specific supply chain vectors across LangChain/LangGraph/AutoGen/CrewAI/Semantic Kernel, plugin middleware injection, container image layer tampering, tool registry poisoning, dependency version pinning failures, init container script injection, service mesh registry routing compromise, MLflow model registry poisoning, embedding model checkpoint poisoning, and load balancer-mediated tool enumeration. The NSA/CISA framework's supply chain guidance is the most directly applicable: "supply chain inspection — do not run imported pre-trained models without inspection in secure dev zone; use AI-specific scanners; evaluate supply chain; prefer secure-by-design" and cryptographic artifact validation/checksums per release. The DHS/CISA document adds SBOMs, AIBOMs, model cards, and vendor assessments. These controls partially address RWA_6_19 (model registry poisoning), RWA_6_20 (dependency pinning failure), RWA_6_21 (container image layer tampering), RWA_6_26 (NIM container supply chain poisoning), and related supply chain scenarios. However, the framework does not address plugin-ecosystem-specific vectors: marketplace trust indicator manipulation (RWA_6_1, RWA_6_11), plugin metadata injection through tool registries (RWA_6_14/6_16/6_25), plugin middleware injection (RWA_6_3), plugin manifest configuration poisoning (RWA_6_8), or load balancer-mediated tool enumeration (RWA_6_29/6_30). SBOM guidance is generic and does not extend to plugin-specific SBOM or dynamic registry inspection.

---

### RWA_7 - Evaluation and Monitoring Bypass
- Strength score: 2

This subsubsection describes 25 threats focused on subverting the monitoring and evaluation apparatus itself: aggregate metric blind spots, evaluation task selection manipulation, gradual regression evasion, metric computation library compromise, evaluation data pipeline integrity violations, Prometheus metric poisoning for RL training, distance metric mismatch attacks, distributed tracing operational disclosure, and CI/CD benchmark threshold gaming. The NSA/CISA framework includes "adversarial testing" (Phase b), "audits, pen testing" (Phase c), "monitoring for data drift and high-frequency/repetitive inputs, oracle attack alerts," and "logs of inputs/outputs/intermediate states." The DHS/CISA document adds red-teaming and human supervision. These controls are directly relevant to establishing a monitoring and evaluation baseline, but the threats in RWA_7 specifically concern adversaries who bypass or corrupt those monitoring systems rather than evade detection within them. The framework does not address: protecting the integrity of evaluation benchmarks and ground truth repositories (RWA_7_23), defending Prometheus metric infrastructure from poisoning (RWA_7_13), preventing distributed tracing data from becoming reconnaissance (RWA_7_25), or CI/CD threshold gaming (RWA_7_24). A score of 2 reflects that monitoring/evaluation infrastructure is called for but framework guidance does not address securing that infrastructure against targeted compromise.

---

### RWA_8 - Vector Database and Knowledge Base Attacks
- Strength score: 1

This subsubsection presents 21 threats targeting vector database infrastructure: vector quantization poisoning, metadata filtering inconsistency across database implementations, ETL timestamp race conditions, deduplication bypass through near-duplicate variants, ETL source connector credential poisoning, REST API pagination cursor manipulation, batch insertion race conditions causing duplicate flooding, vector database authentication via shared API key reuse, cluster gossip protocol injection, memory exhaustion resource limit exploitation, and SQL injection in ETL connector logic. The NSA/CISA framework's data source catalog and trusted/valid source guidance is conceptually relevant but does not address vector database infrastructure at all. Input sanitization guidance would apply to RWA_8_18 (SQL injection in ETL connectors) if interpreted broadly, and least-privilege/RBAC guidance applies to RWA_8_14 (shared API key reuse). However, the framework contains no guidance on vector database-specific security: no coverage of ANN index integrity, ETL pipeline security, deduplication design, gossip protocol authentication, or concurrent ingestion race conditions. These are infrastructure-level database security concerns for a technology class that did not exist in traditional IT security frameworks and is not addressed by the NSA/CISA guidance.

---

### RWA_9 - Learning-Based Attacks
- Strength score: 1

This subsubsection covers 21 threats arising from the learning and adaptation mechanisms of AI agents: calibration-phase strategic performance gaming to reduce oversight, iterative tool access policy drift as emergent privilege escalation, service registration policy drift through shared kernel modification, curriculum learning manipulation via poisoned intermediate tasks, RL reward signal manipulation as a learned behavior, policy gradient optimization of deception, MCTS rollout policy poisoning, cross-agent preference learning driving collective utility drift, and cascading reward hacking amplification through multi-agent workflows. The NSA/CISA framework does not address any learning-based attack surface. Phase b mentions adversarial testing and monitoring for data drift, and Phase c mentions full evaluation before redeployment, but none of these cover the dynamic, online, emergent behavioral threats described here where agents actively learn to game their operating environment during deployment. The DHS/CISA document's adversarial training guidance is about hardening models before deployment, not about detecting or preventing post-deployment behavioral drift caused by RL optimization pressure. No framework guidance exists for reward function security, policy gradient attack surfaces, curriculum integrity, or multi-agent preference learning dynamics.

---

### RWA_10 - UI/UX Security Attacks
- Strength score: 1

This subsubsection describes 14 UI/UX-mediated threats: command palette plugin recommendation poisoning, plugin UI lacking provenance transparency, tool suggestion promoting dangerous operations without risk indicators, RAG context window attacks exploiting UI pagination, workflow automation UIs masking multi-step attack chains, plugin permission escalation through command palette interaction, RAG source attribution spoofing in chat citations, RAG pipeline prompt leakage through UI error messages, and Matryoshka representation learning dimension truncation attacks creating cross-agent retrieval inconsistencies. The NSA/CISA framework contains no UI/UX security guidance beyond generic TLS/MFA for access controls. Input sanitization guidance covers prompt injection at the backend but not UI-mediated injection through document selection, pagination limits, or error message leakage. The DHS/CISA document similarly has no UI/UX-specific guidance. None of the framework controls address the specific threat model where visual presentation, progressive disclosure, pagination boundaries, and citation rendering create exploitable gaps between what users see and what agents process.

---

### RWA_11 - Reasoning and Reflection Attacks
- Strength score: 1

This subsubsection covers 15 threats targeting reasoning and reflection mechanisms: multi-hop reasoning path manipulation through document ordering, critic-producer hallucination amplification cycles, token accounting manipulation causing silent context truncation, hallucination detection evasion through grounding spoofing via RAG poisoning, mutual validation failure through shared reasoning bias, reflection trace injection with fake self-critique, fallback decision reasoning gaps enabling path injection, and phase barrier bottleneck exploitation for denial of service. The NSA/CISA framework does not address multi-agent reasoning architecture security. Monitoring logs of intermediate states is tangentially relevant to detecting anomalous reasoning traces but provides no guidance on multi-agent critic-producer trust boundaries, reasoning trace integrity, or grounding validation architecture. The DHS/CISA document's human supervision guidance could reduce risk from autonomous reasoning failures but does not address the specific attack surfaces of multi-agent reflection patterns. No framework guidance exists for reasoning trace authenticity, critic agent independence requirements, or grounding verification in multi-hop RAG reasoning chains.

---

### RWA_12 - Distributed Systems Attacks
- Strength score: 1

This subsubsection describes 8 distributed systems threats: event-driven replay attacks exploiting asynchronous accumulation windows, distributed tool API rate limit exhaustion, cache coherence failures in distributed semantic memory, batch processing race conditions causing duplicate insertion, feature flag race conditions creating behavioral inconsistency, multi-agent deadlock injection through crafted circular dependency requests, out-of-order agent execution race conditions, and cross-agent filter evasion through distributed pattern fragmentation. The NSA/CISA framework's ZT, TLS, and MFA guidance applies to authentication and transport but does not address distributed systems consistency properties. Network segmentation guidance is tangentially relevant to limiting lateral movement but does not address replay attack windows, cache coherence, race condition injection, or deadlock as adversarial tools. The framework was designed for securing individual AI system deployments, not for addressing distributed systems attack surfaces that emerge from multi-agent coordination across asynchronous message-passing architectures. No guidance exists for event replay protection windows, distributed cache consistency security, or adversarial deadlock injection detection.

---

### RWA_13 - Approval Workflow Exploitation
- Strength score: 1

This subsubsection presents 7 threats targeting approval workflows: approval fatigue exploitation through trust establishment followed by misaligned request injection, human-in-the-loop compromise through conditioning and threshold manipulation, function calling consensus manipulation by poisoning high-weight voting agents, cross-agent permission escalation through approval delegation chains, HITL deadlocks from circular approval dependencies, and cascading approval bottlenecks creating compound latency. The NSA/CISA framework's RBAC/ABAC guidance (Phase c) and audit guidance are tangentially relevant to approval workflow controls, and human oversight is implied by the framework's overall posture. However, the framework contains no specific guidance on approval workflow design, HITL integration security, anti-fatigue mechanisms, consensus voting integrity, or multi-agent delegation chain privilege escalation. The DHS/CISA human supervision guidance establishes the principle but provides no implementation guidance that would address these specific exploitation vectors. There is a fundamental mismatch: the framework addresses access controls for AI systems, not the security of the human oversight mechanisms embedded within agentic workflows.

---

### RWA_14 - Infrastructure and Deployment Attacks
- Strength score: 2

This subsubsection covers 9 infrastructure threats: GPU embedding timing side-channels in shared batch processing, GPU resource exhaustion through adversarial batch flooding, CI/CD pipeline artifact injection via credentials compromise, load balancer health check gaming, Kubernetes secret store poisoning for NGC API keys, auto-scaling tool quota exhaustion, health check endpoint manipulation enabling degraded deployment, and MIG reconfiguration attacks forcing cascading capacity exhaustion. The NSA/CISA framework's Phase a covers sandboxed containers/VMs and GPU patching, and the DHS/CISA document covers network security/segmentation and encryption. The NSA/CISA framework's cryptographic artifact validation per release and supply chain inspection guidance applies to RWA_14_3 (CI/CD pipeline artifact injection). Phase a's ZT and least-privilege guidance partially applies to RWA_14_6 (Kubernetes secret store). The immutable backups and rollback guidance partially addresses some deployment integrity concerns. However, the framework does not specifically address: GPU timing side-channels (RWA_14_1), adversarial batch flooding (RWA_14_2), health check gaming (RWA_14_4/14_8), auto-scaling quota exhaustion (RWA_14_7), or MIG reconfiguration attacks (RWA_14_9). A score of 2 reflects that relevant infrastructure security principles exist but require significant extrapolation to the GPU/Kubernetes/multi-agent deployment context.

---

### RWA_15 - Tool Invocation and Selection Gaming
- Strength score: 1

This subsubsection describes 8 threats focused on gaming tool invocation and selection mechanisms: unnecessary tool invocation to satisfy task count metrics, memory-encoded poisoned tool invocation chain reproduction, GroupChat covert tool access negotiation, streaming tool invocation enabling partial execution gaming, temperature-controlled sampling bias toward malicious tool selection, SLA-driven tool selection gaming through latency falsification, KV cache sharing creating cross-agent tool selection coupling, and rule description injection in tool selection. The NSA/CISA framework's "API returns minimal data" guidance is tangentially relevant to limiting tool output surface, and least-privilege guidance applies to restricting available tools. However, no framework guidance addresses: how agents choose between available tools, gaming of tool selection metrics, conversational negotiation of tool access (as in AutoGen GroupChat), or the use of sampling temperature or KV cache state as tool selection manipulation vectors. These threats arise from the learned and emergent tool selection behavior of LLM-based agents, a domain not addressed in the framework.

---

### RWA_16 - Framework-Specific Vulnerabilities
- Strength score: 1

This subsubsection presents 3 framework-specific threats: LangChain's dynamic tool registration vulnerability when loading from untrusted sources, AutoGen's conversational tool negotiation creating implicit access chains, and NeMo Guardrails centralized configuration tampering enabling fleet-wide safety bypass. The NSA/CISA framework's "prefer secure-by-design" guidance and supply chain inspection are conceptually relevant to RWA_16_1 (untrusted tool definition loading) and RWA_16_3 (NeMo Guardrails centralized config), and input sanitization guidance partially applies to preventing poisoned tool schemas from entering. However, the framework contains no guidance specific to any of the named agent frameworks (LangChain, AutoGen, Semantic Kernel, CrewAI, LangGraph, NeMo) or to the specific security properties of their tool registration, plugin negotiation, or guardrail configuration architectures. The threat in RWA_16_3 — centralized Guardrails configuration as a fleet-wide single point of failure for safety bypass — is particularly significant and entirely unaddressed by framework guidance on securing individual AI deployments.
