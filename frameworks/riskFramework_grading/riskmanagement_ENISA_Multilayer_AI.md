# Risk Management Analysis: ENISA Multilayer Framework for Good Cybersecurity Practices for AI

**Framework:** ENISA Multilayer Framework for Good Cybersecurity Practices for AI (FAICP), June 2023
**Source:** `ENISA_Multilayer_AI/ENISA-Multilayer-Framework-Cybersecurity-AI.pdf`

## Framework Summary

The FAICP is a three-layer framework designed to guide national competent authorities (NCAs) and AI stakeholders on securing AI systems within ICT infrastructure:

- **Layer I (Cybersecurity Foundations):** Securing the ICT environment hosting AI — risk management, access control (ISO 27002), availability (DoS), supply chain security (NIS2), cybersecurity certification.
- **Layer II (AI Fundamentals and Cybersecurity):** ML-specific threats (evasion, poisoning, model/data disclosure, component compromise, failure/malfunction) and AI trustworthiness properties (accuracy, explainability, fairness, privacy, reliability, resiliency, robustness, safety, security, transparency).
- **Layer III (Sector-Specific):** Sector-tailored guidance for energy, health, automotive, and telecommunications.

The framework was published in June 2023, before modern agentic AI architectures (tool-calling agents, multi-agent orchestration, RAG pipelines, LLM-based planning) became mainstream. It treats AI systems as components within ICT infrastructure and focuses on ML training-phase threats rather than inference-time agentic behaviors.

---

## Scoring Key

- **3** — Framework directly and strongly supports management of the risks (specific applicable guidance).
- **2** — Framework indirectly and moderately supports management (relevant principles or controls that partially apply).
- **1** — Framework weakly supports management (only general principles with minimal direct applicability).

---

## RATC — Agent–tool coupling as "policy-level remote code execution"

### RATC_1 - UI/UX Abstraction and Visibility Gaps
- Strength score: 1

ENISA Layer II defines transparency and explainability as core AI trustworthiness properties, which are conceptually relevant since UI abstractions that conceal true tool invocation chains directly undermine both properties. However, ENISA operationalizes these properties at the model output level (e.g., explanation of decisions) rather than at the agentic approval UI level. The framework provides no guidance on multi-agent dashboard risk differentiation, progressive disclosure attack surfaces, or trace visualization design for tool chains spanning multiple agents. The transparency and explainability principles offer general direction but no actionable controls for this threat category.

### RATC_2 - Approval Workflow and User Attention Vulnerabilities
- Strength score: 1

ENISA Layer I classifies human error as an accidental threat and requires security management to account for human factors within ICT environments. This provides marginal conceptual relevance to approval fatigue as a systemic vulnerability. The framework does not address agentic approval workflow design, multi-agent queue flooding that induces reviewer overload, keyboard shortcut hijacking in approval UIs, or timeout-based default exploit patterns. Human factors are treated at the organizational ICT security level, not at the level of AI agent oversight interface design.

### RATC_3 - Tool Overload, Parameter Handling, and Selection Errors
- Strength score: 1

ENISA Layer II identifies compromise of ML application components as a threat category and recommends that ML components comply with protection policies and be integrated with security operations and asset management. This provides weak relevance to argument injection across agent trust boundaries as a form of component compromise. However, ENISA's controls address ML component integrity at the software library level (e.g., open-source library vulnerabilities) rather than at the level of LLM tool-calling parameter handling, multi-agent delegation chains, or tool overload-induced selection degradation. No applicable controls exist for these agentic-specific attack surfaces.

### RATC_4 - Confidence Manipulation and Tool Authorization
- Strength score: 1

ENISA Layer II covers evasion attacks, defined as crafting inputs to exploit model outputs, which provides a loose conceptual connection to adversarial confidence score inflation. The framework recommends adversarial training and input validation to counter evasion. However, confidence threshold manipulation in agentic authorization gates — where inflated confidence scores bypass approval checkpoints — and authorization scope confusion arising from multi-agent specialization hierarchies are agentic-specific mechanisms not addressed by ENISA's evasion controls, which target traditional ML classification task outputs rather than LLM authorization gating.

### RATC_6 - Tool Metadata Poisoning Across Registries and Discovery
- Strength score: 2

ENISA Layer II explicitly covers poisoning attacks and recommends securing ML components over time, assessing model exposure, enlarging training datasets to reduce susceptibility to malicious samples, and applying pre-processing to clean training data. While these controls target ML training pipelines, centralized tool registry poisoning and vector database metadata poisoning are conceptually analogous: adversaries inject malicious content that propagates to downstream decision-making across all consuming agents. Additionally, ENISA's Layer I and NIS2 supply chain security guidance apply to registry compromise as a supply chain attack vector. The poisoning mitigations translate moderately, though no agent registry-specific guidance exists.

### RATC_8 - Advanced Tool Invocation Patterns and Inference
- Strength score: 1

ENISA Layer II's evasion and poisoning threat categories provide a loose conceptual umbrella for observation manipulation (injecting malicious content into tool results) and replanning exploitation. However, ENISA does not address ReAct-pattern observation injection, multi-agent replanning loop exploitation through controlled failure injection, or cross-framework schema translation layer attacks. These are emergent behaviors of agentic inference-time architectures that the 2023 ENISA framework was not designed to address. No security control guidance is applicable to these specific attack patterns.

### RATC_9 - Web and Multimodal Tool Exploitation
- Strength score: 1

ENISA Layer II mentions adversarial perturbations on image processing models in the automotive sector context, and covers evasion as a general ML threat. Multimodal tool exploitation shares conceptual ground with these evasion threats. However, web agent prompt injection via adversarial web content, cross-agent multimodal content laundering where malicious vision outputs gain legitimacy through sequential agent processing, and cross-modal parameter injection without sanitization boundaries are agentic pipeline-level attacks that substantially exceed ENISA's evasion framework, which focuses on traditional ML model inputs rather than agentic web retrieval and multimodal tool chains.

### RATC_10 - Efficiency Optimization and Resource Constraints
- Strength score: 1

ENISA does not address LLM-specific inference efficiency parameters: token allocation, context window constraints, iteration budgets, temperature settings, or model selection diversity. Layer I covers general availability threats including resource exhaustion and DoS, providing marginal relevance to attacker-induced context constraints that truncate safety-critical tool descriptions passed to downstream agents. The framework offers no controls applicable to inference parameter manipulation or cross-agent context truncation attacks.

### RATC_11 - Tool Execution Infrastructure and Orchestration
- Strength score: 2

ENISA Layer I provides substantive guidance on securing ICT infrastructure, including network security, access control via ISO 27002, and compliance with NIS2 requirements covering cloud and supply chain security. API gateway routing manipulation (redirecting tool calls to attacker-controlled services), Kubernetes container privilege escalation, persistent volume claim compromise, sidecar proxy interception, and init container configuration injection are ICT infrastructure-layer threats that fall within ENISA's general security management scope. The framework's risk management methodology and ISO 27002 access controls apply meaningfully to these infrastructure attacks, though no AI agent-specific orchestration security guidance exists.

### RATC_12 - Distributed and Hardware-Level Attacks
- Strength score: 1

ENISA Layer I addresses physical security of ICT infrastructure, and the framework references the European Chips Act in acknowledging the security importance of semiconductors used in AI. These provide marginal relevance to hardware-level attacks. GPU tensor parallelism communication interception (unencrypted inter-GPU traffic), KV cache poisoning via multi-tenant shared GPU inference, and quantization calibration dataset poisoning biasing quantized outputs are highly specialized AI inference hardware vulnerabilities entirely outside ENISA's guidance scope. The framework provides no actionable controls for AI-specific distributed compute or hardware exploitation.

### RATC_14 - Reasoning and Planning Vulnerabilities
- Strength score: 1

ENISA Layer II acknowledges that AI security testing faces unique challenges because "not all expected results are known a priori" and that AI systems may evolve non-deterministically. This oblique recognition of AI reasoning complexity provides minimal support. Chain-of-thought reasoning poisoning (embedding justifications for dangerous tool sequences in shared reasoning traces), self-consistency voting manipulation (forcing convergence through manipulated inputs), and hierarchical task network planning fragmentation (tool-awareness gap between planning and execution layers) are agentic reasoning-layer attacks not covered by ENISA's threat taxonomy or security controls.

### RATC_15 - Episodic Memory and Learning
- Strength score: 2

ENISA Layer II explicitly identifies poisoning as a core ML threat and recommends maintaining security of ML components over time, assessing exposure levels, and pre-processing to remove malicious samples from training data. Episodic memory poisoning — where adversaries corrupt stored action trajectories to propagate malicious tool-invocation policies to agents that retrieve those episodes — is directly analogous to training data poisoning and is partially covered by these controls. The framework's data integrity and access control guidance (Layer I, ISO 27002) also applies to episodic memory stores. However, ENISA does not address cross-agent shared episodic memory propagation, where one agent's compromised episodes become learned policy for all agents accessing shared memory stores.

### RATC_16 - Semantic Memory and RAG
- Strength score: 2

ENISA Layer II explicitly covers poisoning attacks and recommends securing data pipelines, applying access controls, and pre-processing to remove malicious content. RAG knowledge base poisoning — injecting tool-invocation instructions, adversarial retrieval triggers, or knowledge graph relationship manipulations into vector stores — is directly analogous to ML data poisoning and is partially covered by these controls. Layer I's data integrity and access management guidance (ISO 27002) applies to vector database security. However, RAG-specific attack patterns such as query rewriting instruction injection and knowledge base staleness exploits causing agents to invoke tools with obsolete parameters are not specifically addressed.

### RATC_17 - Utility Functions and Decision Logic
- Strength score: 1

ENISA Layer II discusses AI trustworthiness in terms of accuracy, reliability, and fairness — properties conceptually related to correct utility function behavior. However, adversarial manipulation of expected utility calculations (injecting false outcome assumptions to miscalculate tool utility), sequential utility miscalculation through misrepresented intermediate options, and trade-off weight poisoning in multi-objective utility functions are specific agentic decision-logic attacks entirely outside ENISA's guidance. The framework offers no controls for securing agent utility functions, decision-theoretic planning components, or reward/objective function integrity.

### RATC_18 - Rule-Based and Knowledge-Engineered Systems
- Strength score: 1

ENISA Layer II classifies expert systems as a type of AI but provides no security controls specific to rule-based AI authorization. Rule injection attacks exploiting specificity hierarchies to override safety rules, forward chaining exploitation through fact injection triggering malicious authorization chains, certainty factor manipulation, and lexicographic heuristic objective reordering are logic-layer attacks on knowledge-engineered systems. ENISA Layer I's general access control guidance (ISO 27002) provides minimal relevance by restricting who can modify rule bases, but the framework contains no guidance for securing inference engines, rule prioritization mechanisms, or heuristic objective ordering.

### RATC_19 - Learning and Reinforcement Learning
- Strength score: 2

ENISA Layer II explicitly covers ML training data poisoning, identifying it as a core threat and recommending maintaining ML component security over time, assessing exposure, expanding training datasets, and pre-processing to remove malicious samples. Reward poisoning, curriculum learning poisoning, and imitation learning trajectory poisoning fall within ENISA's poisoning threat category and are partially addressed by these controls. However, ENISA does not address multi-agent RL shared replay buffer attacks (where poisoned transitions affect all agents' learned Q-values simultaneously), actor-critic cross-agent critic poisoning, or PPO trust region manipulation — multi-agent RL coordination vulnerabilities that amplify impact beyond single-agent training scenarios.

### RATC_21 - Parallel Retrieval Race Conditions in Multi-Agent Query Decomposition Systems
- Strength score: 2

ENISA Layer I addresses ICT availability and integrity as core CIA dimensions and identifies DoS as an adversarial threat category. NIS2 compliance under Layer I encompasses supply chain security. Concurrent retrieval race conditions causing fleet-wide retrieval outages, cache write races degrading integrity, and container supply chain poisoning via registry credential compromise are ICT infrastructure availability and supply chain threats within ENISA's security management scope. The framework's supply chain security guidance and availability threat mitigation apply. However, AI agent-specific multi-agent retrieval parallelism, vector store deduplication races, and agent-fleet consistency vulnerabilities from stale cache mixing are not addressed.

### RATC_22 - Multi-Stage Pipeline Result Caching Race Enabling Multi-Agent Consistency Failures
- Strength score: 2

ENISA Layer I addresses ICT integrity and availability, and NIS2 compliance encompasses supply chain security applicable to container registry compromise, GitOps pipeline integrity, and typosquatting attacks. Cache write race conditions and hash collision attacks targeting shared caching infrastructure are ICT integrity concerns within ISO 27002's scope. The framework's general integrity controls and supply chain security guidance apply partially. However, AI-specific multi-stage pipeline caching consistency failures — where per-stage TTL expiry causes agents to mix results from different knowledge states — and tensor parallelism poisoning via multi-tenant GPU collocation are outside ENISA's guidance.

### RATC_27 - MIG Instance Co-Location Creating Shared Physical GPU Failure Propagation Across Multi-Agent Deployments
- Strength score: 1

ENISA Layer I covers physical security of ICT infrastructure, and the framework references the European Chips Act in acknowledging hardware security for AI semiconductor platforms. Physical infrastructure isolation failures — where shared GPU hardware components (power regulators, thermal management, PCIe interface, firmware) cause cross-MIG-partition failures — are notionally hardware security concerns within ENISA's broad ICT physical security scope. However, GPU MIG partitioning security, CUDA driver vulnerabilities causing fleet-wide outages, and firmware exploits enabling cross-partition memory access are AI inference infrastructure specifics entirely outside ENISA's guidance. The framework provides no actionable controls for GPU-level multi-tenant AI deployment security.

### RATC_29 - Shared Workflow State Version Conflict Amplification Through Concurrent Write Flooding Creating Retry Storm Cascades
- Strength score: 1

ENISA Layer I addresses availability as a core CIA dimension and identifies DoS as an adversarial threat. Retry storm cascades induced by concurrent write flooding — where contention grows quadratically with agent concurrency and targeted collision bursts exhaust retry budgets — are a form of resource exhaustion affecting system availability, nominally within ENISA's threat scope. However, ENISA provides no controls for distributed workflow state management, optimistic locking protocol security, or multi-agent coordination protocol resilience. The availability guidance operates at general ICT level and offers no actionable mitigation for agentic workflow concurrency exploitation.

### RATC_30 - Guardrail Bypass Through Infrastructure Failure Injection Preventing Safety Validation Execution
- Strength score: 2

ENISA Layer I explicitly identifies DoS as an adversarial threat and requires availability management within security management practices. Layer II covers AI safety and robustness as trustworthiness properties and recommends that AI systems maintain safe operating conditions and resist adversarial attacks. Infrastructure failure injection to bypass guardrails combines DoS against validation endpoints (Layer I) with evasion of AI safety controls (Layer II), and both dimensions are nominally within ENISA's scope. The framework's recommendations for anomaly detection, resilience to adversarial attacks, and availability management apply in principle. However, ENISA provides no specific guidance on timeout-threshold safety bypass, fleet-wide guardrail bypass through validation infrastructure DoS, or fail-open versus fail-closed safety validation design.


---

## RDL — New data-leakage channels via large contexts, logs, and probabilistic recall

### RDL_1 - UI/UX Patterns
- Strength score: 2

ENISA Layer I grounds data security in the CIA triad with explicit reference to GDPR, NIS2, and ISO 27002, all of which govern PII handling, data minimization, and access control. These principles are directly relevant: chat histories aggregating credentials and PII from multiple agents without adequate controls, command palette logs revealing behavioral patterns, and approval workflow audit trails becoming reconnaissance targets all represent data protection and access management failures addressed in principle by ENISA's Layer I. Layer II identifies privacy as a core AI trustworthiness property. However, ENISA provides no guidance specific to multi-agent chat UI design, aggregated dashboard cross-domain correlation, or progressive disclosure attack surfaces unique to agentic orchestration.

### RDL_2 - Data Persistence and Caching
- Strength score: 2

ENISA Layer I references GDPR (including the right to be forgotten) and ISO 27002 for data lifecycle management, both directly applicable to session serialization creating queryable sensitive records, cached multi-agent responses persisting beyond session lifetime, and undo buffers retaining "deleted" data contrary to data minimization obligations. Access controls for session and cache storage are basic ICT security practices within ENISA's scope. The framework does not address agentic-specific complexity — multi-agent composite cache entries reconciling heterogeneous access controls, or distributed undo creating inconsistent retention — but its data lifecycle and access management guidance applies moderately.

### RDL_3 - Streaming and Token-Level Leakage
- Strength score: 1

ENISA Layer I covers network confidentiality and data-in-transit protection, providing marginal relevance to streaming exposure in transit. LLM-specific streaming vulnerabilities — early token exposure before redaction can apply, tokenization inference enabling context window mapping, streaming length correlation for session reconstruction, and multi-agent cycling state leakage through sequential handoff windows — are AI inference-time attack surfaces not present in traditional ICT systems and entirely outside ENISA's guidance scope.

### RDL_4 - Search and Recall
- Strength score: 1

ENISA Layer I covers access control (ISO 27002) and data classification, and Layer II identifies privacy as an AI trustworthiness property. These nominally apply to unified multi-agent conversation search indexes crossing security boundaries. However, probabilistic and semantic search recall is fundamentally different from access-controlled traditional queries: similarity-driven retrieval surfaces semantically related content regardless of classification labels. ENISA's framework was not designed for this probabilistic access paradigm and provides no applicable guidance for containing cross-domain semantic leakage in multi-agent memory corpora.

### RDL_5 - Attribution and Observability
- Strength score: 2

ENISA Layer I includes log management and monitoring security within security management practices, and ISO 27002 covers audit log access control and data minimization. Multi-agent attribution logs revealing organizational hierarchy, agent usage distribution, and decision workflows are log security concerns addressed in principle by ENISA's access management and log protection guidance. Error classification pattern leakage and latency measurement side-channels are monitoring security concerns within Layer I's scope. ENISA does not address how multi-agent log aggregation creates organizational blueprints exceeding any single-agent log's value, but its log security principles apply moderately.

### RDL_7 - Tool Invocation and Function Calling
- Strength score: 2

ENISA Layer I covers log management, access control (ISO 27002), and audit trail integrity — directly relevant to tool invocation logging as side channels, function calling JSON persisting sensitive parameters across agent context windows, and audit trail poisoning by compromised agents injecting false entries. Log minimization, access control for audit repositories, and log integrity monitoring are basic ICT security practices within ENISA's guidance. Layer II mentions compromise of ML application components (relevant to audit trail poisoning). LLM function calling JSON as a persistent context leakage vector and tool error message reconnaissance in multi-agent networks are agentic inference-time patterns not addressed.

### RDL_9 - Multi-Agent Memory and State
- Strength score: 1

ENISA Layer I covers data classification and access management, and Layer II briefly classifies multi-agent systems as an AI type. Cross-agent reflection memory leakage and blackboard shared memory access pattern attacks are data isolation failures conceptually within ENISA's access control scope. However, ENISA does not address multi-agent memory architectures (blackboard pattern, shared reflection memory, distributed trace correlation) or the AI-specific challenge of non-deterministic probabilistic leakage obscuring detection through traditional log audits. No actionable controls exist for these attack surfaces.

### RDL_10 - Reasoning and CoT Traces
- Strength score: 1

ENISA Layer II defines transparency and explainability as AI trustworthiness properties, acknowledging reasoning trace management at a high principle level. CoT trace leakage as a data exfiltration channel — where sensitive context in reasoning steps leaks through memory queries or cross-agent context propagation — probabilistic recall of sensitive data from preserved reasoning paths, and memory consolidation merging sensitive information from multiple agents are agentic reasoning-layer vulnerabilities entirely outside ENISA's guidance scope. The framework recognizes AI reasoning complexity but provides no security controls for these attack surfaces.

### RDL_11 - Multimodal and Input Processing
- Strength score: 1

ENISA Layer I covers data privacy (GDPR) and confidentiality, broadly applicable to sensitive multimodal data. Layer II mentions computer vision in the context of adversarial perturbations. Embedding inversion attacks reconstructing original images from stored vectors, vision model intermediate representation leakage in multi-agent pipelines, audio embedding privacy channels via voice characteristics, and chart linearization creating persistent sensitive numerical records are technically specialized AI attacks on multimodal inference architectures for which ENISA provides no controls.

### RDL_12 - Error Handling and Graceful Degradation
- Strength score: 2

ENISA Layer I includes secure error handling in ICT security management: minimizing information disclosure in error messages is a basic practice referenced in ISO 27002, and ENISA covers availability management and incident response. Error logging creating centralized high-value leakage repositories, retry attempt state accumulation, and fallback provider logging with inconsistent security postures are ICT error-handling concerns within ENISA's scope. Circuit breaker state leakage as infrastructure reconnaissance is a monitoring security concern in Layer I. ENISA's principles apply moderately, though multi-agent specifics (distributed error accumulation across agent boundaries) are not addressed.

### RDL_13 - Evaluation and Testing Leakage
- Strength score: 2

ENISA Layer II explicitly addresses security testing as a component of the AI life cycle and identifies challenges in evaluating AI systems throughout development and deployment. Evaluation artifact security (access control for logs, datasets, metric outputs), test dataset protection, and evaluation pipeline log integrity are AI development security concerns within ENISA's AI life cycle guidance. Layer I's access management applies to evaluation dashboard exposure and artifact storage access. Multi-agent evaluation dashboards aggregating test samples across all agents, metric fingerprinting enabling targeted attacks, and temporal behavior pattern inference from metric evolution are agentic evaluation security concerns not addressed.

### RDL_14 - Feedback and Testing
- Strength score: 1

ENISA Layer II covers the AI life cycle including iterative improvement through feedback, and Layer I addresses data access control. The principle that user feedback data should be access-controlled is within ENISA's scope. However, systematically mining stored user feedback to construct agent vulnerability profiles and exploiting A/B test result leakage to identify weaker agent variants for targeted attacks are adversarial uses of AI operational data not addressed by ENISA's life cycle guidance.

### RDL_15 - Evaluation Metric Exploitation
- Strength score: 1

ENISA Layer II explicitly acknowledges that "assigning a test verdict is more difficult for AI-based systems, since not all expected results are known a priori" — recognizing the fundamental AI evaluation difficulty without providing controls. Adversarial exploitation of specific evaluation metrics (exact match manipulation, pass@k probabilistic attack surface creation, milestone threshold exploitation) represents AI assurance gaming that ENISA identifies as an open challenge but provides no guidance to address. Multi-agent amplification of these evaluation exploits receives no coverage.

### RDL_16 - Parameter and Configuration Leakage
- Strength score: 1

ENISA Layer I covers system configuration security and operational monitoring, providing marginal relevance. LLM-specific parameter tuning creating observable leakage channels — context window size determining log verbosity targets, cost-optimization routing revealing query sensitivity distributions through differential token consumption, and latency fingerprinting through inference parameter profiles — are AI inference-time side-channel attacks exploiting the interaction between LLM efficiency tuning and operational observability, a paradigm absent from traditional ICT systems and unaddressed by ENISA.

### RDL_17 - Prompt Injection and Few-Shot Attacks
- Strength score: 2

ENISA Layer II identifies poisoning as a core ML threat and recommends data sanitization, training data security, and exposure minimization. Few-shot demonstration contamination is conceptually analogous to training data poisoning — adversaries inject malicious content into in-context learning data that propagates through agent reasoning. ENISA's poisoning controls (securing data pipelines, pre-processing, access control) partially apply to demonstration integrity. Format injection for cross-agent instruction propagation and generation bias injection are agentic inference-time patterns operating during deployment rather than training, which ENISA's training-focused controls do not directly address.

### RDL_18 - Distributed Tracing
- Strength score: 2

ENISA Layer I covers log management, monitoring infrastructure security, and access control (ISO 27002) — directly applicable to distributed tracing backend security. Securing tracing systems against unauthorized access to correlation IDs and span data is an ICT infrastructure security practice within ENISA's scope. NIS2 compliance includes monitoring system integrity. ENISA's access control and log management guidance applies moderately to tracing infrastructure security, though multi-agent workflow reconstruction from aggregated traces and cross-tenant correlation leakage in shared tracing backends are not specifically addressed.

### RDL_19 - Execution and Orchestration
- Strength score: 2

ENISA Layer II covers accountability and traceability as AI trustworthiness properties, and Layer I addresses audit trail integrity and access control. Parameter provenance tracking loss — where multi-agent handoff causes attribution amnesia enabling undetected attack propagation — directly violates ENISA's accountability and traceability requirements. Error accumulation across agent boundaries disclosing execution paths is an error-handling security concern within Layer I's scope. ENISA provides no specific controls for multi-agent parameter provenance serialization or inter-agent handoff audit chain integrity.

### RDL_20 - Multi-Agent Reasoning
- Strength score: 1

ENISA Layer II covers AI trustworthiness including robustness, reliability, and accuracy — properties conceptually relevant to orchestrator reasoning quality failures cascading to all worker agents. The coherence validation gap (no single agent validating system-level reasoning across boundaries) relates to accountability. However, orchestrator reasoning cascade exploitation through injected coordination logic, supervisor delegation transparency paradox, and consensus reasoning exploitation through non-contributory injection are multi-agent coordination attack patterns that ENISA's guidance was not designed to address. No applicable controls exist.

### RDL_21 - Efficiency and Cost Attribution
- Strength score: 2

ENISA Layer I covers monitoring and operational security, and ISO 27002 addresses access control for monitoring dashboards and log repositories. Efficiency monitoring data (token consumption, cost attribution, cache metrics) as a leakage channel is an operational data security concern: minimizing exposed operational data and controlling dashboard access are basic ICT security practices within ENISA's scope. AI-specific cost attribution revealing sensitive operation complexity distributions and budget reallocation patterns as organizational priority disclosure are not addressed, but monitoring security and access management principles apply.

### RDL_22 - Infrastructure and Deployment
- Strength score: 2

ENISA Layer I provides comprehensive guidance on ICT infrastructure security, with ISO 27002 and NIS2 covering message queue security, API gateway log management, centralized logging pipeline access control, and Prometheus metrics security as infrastructure components. These ICT infrastructure concerns are directly within ENISA's Layer I scope. Vector database embedding exposure enabling content reconstruction and MLflow artifact registry leakage are AI-specific extensions of data store security covered only indirectly. ENISA's infrastructure security principles apply substantively to the non-AI-specific components while AI-specific storage components receive only indirect coverage.

### RDL_23 - Containerization Security
- Strength score: 2

ENISA Layer I covers ICT infrastructure security comprehensively, with NIS2 compliance requirements addressing container environment security. Shared memory CPU cache side-channels, sidecar proxy interception, persistent volume permission escalation, init container environment variable leakage, kubelet metrics endpoint exposure, event broker retention creating persistent data exposure, and etcd database backup leakage are container and Kubernetes infrastructure security topics within the ICT security management scope ENISA addresses via ISO 27002, CIS Critical Security Controls, and NIS2. These are ICT infrastructure threats well within ENISA's scope, though no Kubernetes-specific hardening guidance is provided.

### RDL_24 - Profiling and Optimization
- Strength score: 2

ENISA Layer I covers data access control and development tool security, and Layer II covers AI assets including "environment/tools" such as algorithm libraries and ML platforms as part of the AI life cycle. MLflow and profiling tool repositories are AI development tool assets within ENISA's AI asset taxonomy. Access control for profiling artifacts, optimization logs, and baseline comparison records are data security concerns addressed by Layer I's ISO 27002 guidance. Performance optimization log sensitivity and MLflow artifact sensitivity are AI development security concerns within ENISA's life cycle scope.

### RDL_25 - Kubernetes Orchestration
- Strength score: 2

ENISA Layer I covers ICT infrastructure monitoring and audit log security through NIS2 compliance and ISO 27002. Kubernetes audit log access control is an ICT infrastructure security concern within ENISA's scope. NIS2 requires essential entities to implement appropriate technical measures including monitoring system security. Multi-agent deployment topology inference through correlated Kubernetes scaling and failure events — reconstructing inter-agent dependency graphs from audit log patterns — is an AI operational security concern beyond ICT infrastructure log management and not addressed by ENISA.

### RDL_26 - GPU and Model Internals
- Strength score: 1

ENISA Layer I covers physical ICT infrastructure security and references the European Chips Act on hardware security, providing only marginal relevance. GPU memory metrics leakage through Prometheus, verbose inference log capture of token generation, KV cache extraction through inter-GPU traffic, quantization artifact inference, and engine binary metadata extraction are highly specialized AI inference hardware attacks requiring GPU architecture knowledge not covered by ENISA. Securing GPU metrics endpoints (access control) is the only actionable guidance Layer I provides for this threat category.

### RDL_27 - Load Balancing
- Strength score: 2

ENISA Layer I covers ICT network and infrastructure security including monitoring endpoint access control. Load balancer routing as a side channel, cache hit/miss timing inference, and auto-scaling event pattern analysis are infrastructure monitoring security concerns. Securing load balancer metrics endpoints and minimizing observable operational patterns are ICT security practices within ENISA's scope. AI-specific batching window timing attacks revealing ML workload composition and probabilistic reconstruction of agentic workflows from load patterns are not addressed.

### RDL_28 - Planning Systems
- Strength score: 1

ENISA Layer I covers access control and data classification; Layer II classifies expert systems as an AI type. Method library access control (restricting HTN method registry queries) is a basic ICT security practice, but ENISA provides no guidance for HTN-specific leakage: constraint specification revealing resource boundaries, state abstraction mapping disclosing information classification policy, decomposition pattern analysis revealing optimization targets, or planning history serialization creating inter-agent topology leakage. These are AI planning architecture specifics entirely outside ENISA's guidance scope.

### RDL_29 - Monte Carlo Tree Search (MCTS)
- Strength score: 1

ENISA's framework provides no guidance relevant to MCTS-specific leakage. MCTS tree state-action pair disclosure, simulation trajectory as probabilistic recall surface for unexecuted action data, value network confidence score leakage of strategic assessments, rollout policy behavior inference, and replanning trajectory log reconstruction of infrastructure topology are AI planning algorithm vulnerabilities entirely outside ENISA's guidance. General log access control from Layer I is the only marginally applicable control.

### RDL_30 - Monitoring and Callbacks
- Strength score: 1

ENISA Layer I covers monitoring and operational security, providing marginal relevance: securing monitoring callback channels through access control and encryption is a basic ICT security practice. The specific threat — agents broadcasting discrepancy discoveries that enumerate exact state space regions and tools attempted, inadvertently mapping system state to monitoring infrastructure — is an agentic coordination observability pattern with no applicable ENISA guidance.

### RDL_31 - Episodic Memory
- Strength score: 2

ENISA Layer II identifies model and data disclosure as a core ML threat and recommends access control and federated learning to minimize breach risk. Episodic memory storage as a data exfiltration channel, embedding vector inference enabling content reconstruction (a form of model inversion), and consolidated abstraction records enabling episode reconstruction are forms of AI data disclosure addressed in principle by ENISA's Layer II controls. Layer I's data access management and encryption guidance apply to episodic memory store security. ENISA does not address multi-agent shared episodic memory enabling organization-wide operational pattern reconstruction at scale.

### RDL_32 - Semantic Memory
- Strength score: 2

ENISA Layer I covers data access control and confidentiality, and Layer II covers AI data security as part of the AI life cycle. Knowledge base access control, encryption of vector stores, and data minimization for retrieval systems are ICT and AI data security concerns within ENISA's scope. RAG context window leakage through retrieved documents and temporal metadata exposure are data access security failures addressable by Layer I controls. Embedding similarity leakage enabling document fingerprinting and knowledge graph structure inference through relationship absence patterns are AI-specific vector store attacks not covered.

### RDL_33 - RAG Leakage
- Strength score: 2

ENISA Layer II covers data security as part of the AI life cycle, and Layer I addresses data access control and integrity. RAG pipeline data security — query log access control, relevance score exposure management, deduplication artifact protection — involves ICT data security practices within ENISA's scope. Layer II's poisoning controls are relevant to hallucination propagation through failed compression validation, where corrupted summaries enter semantic memory as credible content. Probabilistic leakage through relevance score analysis, query rewriting log disclosure of operational intent, and privacy breach through semantic-to-working memory distillation are RAG-specific attack vectors not addressed.

### RDL_34 - Utility Functions and Explanations
- Strength score: 1

ENISA Layer II covers explainability as an AI trustworthiness property, which is conceptually relevant to the tradeoff where explanation mechanisms simultaneously enable and leak decision logic. Progressive disclosure UI leakage of utility parameters, streaming explanation leakage before redaction can apply, working memory content exposure through shared persistence, rule condition inference from explanation content, and error message outcome utility disclosure are agentic explanation-layer attack vectors not addressed. ENISA encourages explainability without addressing its associated security risks.

### RDL_35 - Reinforcement Learning and Policy
- Strength score: 2

ENISA Layer II explicitly identifies model and data disclosure as a core ML threat and recommends access control and federated learning to minimize breach risk. Gradient-based model inversion attacks reconstructing training data, value function querying to reconstruct training contexts, and policy behavior analysis to infer training data distributions are forms of model/data disclosure directly within ENISA's Layer II threat category. Access control for model artifacts and minimizing model exposure are applicable controls. Replay buffer side-channel attacks, reward function reverse engineering, and multi-agent shared gradient leakage amplifying disclosure across the agent fleet are RL-specific mechanics not covered.

### RDL_36 - Multi-Agent Coordination
- Strength score: 1

ENISA Layer I covers data access control and confidentiality; Layer II briefly classifies multi-agent systems. Shared conversation history access control is a basic data security concern within ENISA's scope. Probabilistic recall from shared multi-agent history — where sensitive data surfaces during normal token generation, bypassing access control through the model's own generative process — and knowledge graph edge weight leakage through embedding inference are AI-specific coordination leakage patterns for which ENISA provides no applicable controls.

## RIP — Agent identity, provenance, and economic/resource attack surface

### RIP_1 - Multi-Agent UI & Dashboard Attacks
- Strength score: 2

ENISA Layer I addresses identity management and access control under ISO 27002, which applies to dashboard authentication, user identity verification, and audit logging for attribution forensics. Access control lists, session management, and audit trail requirements are within ENISA's ICT security scope and directly mitigate identity spoofing in multi-agent dashboards and cost attribution failures traceable through proper audit logs. Layer I's availability controls partially address cost-driven economic denial-of-service through resource monitoring. ENISA does not address GroupChat social engineering targeting AI agent personas, multi-agent cost attribution mechanisms, or real-time identity federation across agentic orchestration layers.

### RIP_2 - Approval & Workflow Attacks
- Strength score: 1

ENISA does not address multi-agent approval workflow integrity, decision provenance tracking, or confidence aggregation attacks. While Layer I process integrity and audit logging are broadly applicable, the specific attacks — provenance tampering in LLM-based workflow decisions, confidence score aggregation poisoning, and asynchronous approval process exploitation — are agentic workflow mechanisms with no analog in ENISA's ICT or ML security categories.

### RIP_3 - Command Palette & UI Attacks
- Strength score: 2

ENISA Layer I covers access control and rate limiting as ICT security measures under ISO 27002. Rate limiting bypass through identity spoofing and privilege escalation through command palette manipulation are access control failures addressable by ENISA's identity and access management guidance. Cost visibility gaps in UI design are a user awareness and security-by-design consideration within ENISA's scope. ENISA does not address AI-powered command palettes, cost attribution in token-based systems, or identity confusion specific to multi-agent UI coordination patterns.

### RIP_4 - Multi-Agent Coordination Attacks
- Strength score: 2

ENISA Layer I explicitly identifies denial of service as an adversarial threat and recommends availability controls, resilience measures, and resource management. Reflection-amplified resource exhaustion, orchestrator overload, and message flood attacks are availability threats that ENISA's DoS controls can mitigate at the infrastructure level through rate limiting, circuit breakers, and resource quotas. Availability is a core CIA triad property in ENISA's framework. However, ENISA does not address multi-agent reflection loops creating multiplicative costs, auction manipulation in multi-agent resource allocation, or orchestrator-specific overload patterns unique to agentic coordination architectures.

### RIP_5 - Framework-Specific Attacks
- Strength score: 1

ENISA does not address framework-specific multi-agent attacks targeting LangChain, AutoGen, CrewAI, or Semantic Kernel identity mechanisms. Cross-framework impersonation exploiting each framework's unique agent naming conventions, role assignment schemas, and credential mechanisms are attack vectors that emerged after ENISA's June 2023 publication. General identity management principles from Layer I apply abstractly, but ENISA provides no applicable controls for framework-specific agent identity spoofing or cost attribution complexity in heterogeneous agentic deployments.

### RIP_6 - Tool & Plugin Attacks
- Strength score: 2

ENISA Layer I supply chain security guidance addresses plugin and tool integrity, third-party component vetting, and access control for external integrations. ISO 27002 supply chain controls are applicable to tool registration integrity and preventing malicious tool injection through compromised plugin repositories. Privilege inheritance through tool chaining is an access control concern within ENISA's scope. Token leakage through tool parameters is a data confidentiality issue addressable by ENISA's data protection principles. ENISA does not address economic attack surfaces specific to LLM tool invocation costs, agentic tool chaining amplification, or budget-based authorization patterns.

### RIP_7 - Cost & Economic Denial-of-Service Attacks
- Strength score: 2

ENISA Layer I explicitly addresses denial of service and availability threats, recommending resource management controls, rate limiting, and capacity planning as mitigation measures. The economic dimension of resource exhaustion — inflated infrastructure costs through adversarially triggered compute consumption — aligns with ENISA's DoS threat category and financial risk assessment methodology. Resource quotas and monitoring recommended by ENISA's ICT security guidance are applicable controls. ENISA does not address denial-of-wallet attacks exploiting LLM token pricing mechanisms, agent-specific budget-as-authorization patterns, cost attribution failures across planning/memory/RL agent architectures, or multi-agent cost amplification dynamics unique to agentic systems.

### RIP_8 - Resilience & Error Handling Attacks
- Strength score: 2

ENISA Layer I covers resilience and business continuity as ICT security requirements, and Layer II explicitly identifies failure and malfunction as ML-specific threat categories. Error handling security, retry storm prevention, and circuit breaker patterns are resilience engineering concerns within ENISA's availability scope. Streaming opacity and retry budget attribution attacks exploit resilience mechanisms and are partially addressed by ENISA's general resilience guidance requiring proper monitoring and bounded retry logic. ENISA does not address LLM inference streaming-specific attack surfaces, token-budget-aware retry attribution, or multi-agent resilience cascade patterns unique to agentic error propagation.

### RIP_9 - Multimodal Attacks
- Strength score: 2

ENISA Layer II identifies evasion attacks as a core ML threat category, which broadly encompasses adversarial inputs crafted to exploit model weaknesses. Multimodal DDoS attacks exploiting vision model computational complexity and adversarial image-based resource exhaustion are evasion-family attacks at the ML layer. ENISA's Layer I DoS controls apply to the availability dimension. Layer II's evasion threat category provides a framework for recognizing image-based adversarial attacks, though ENISA's guidance predates multi-modal agentic deployments and does not address cross-modal attack amplification in multi-agent vision pipelines.

### RIP_10 - Reasoning & Evaluation Attacks
- Strength score: 1

ENISA does not address LLM reasoning evaluation pipelines, ReAct/MCTS-based planning systems, or sampling budget allocation mechanisms. Evaluation cost attribution failures, identity spoofing in evaluation agent decision displays, MCTS computational budget exhaustion, and pass@K probabilistic instruction activation are evaluation framework attacks specific to modern agentic AI with no analog in ENISA's 2023 framework. Layer I audit logging principles abstractly apply to evaluation result provenance, but ENISA provides no applicable controls for the reasoning evaluation attack surface.

### RIP_11 - Vector Store & RAG Attacks
- Strength score: 2

ENISA Layer II covers AI data security as part of the AI life cycle, and Layer I addresses data access control and confidentiality. Vector database access control, partition isolation, and audit logging for query attribution are data security concerns within ENISA's scope. Embedding endpoint authentication and API key management apply ENISA's access management principles. Query load amplification attacks are DoS threats addressed by Layer I availability controls. ENISA does not address vector partition escape attacks enabling cross-agent identity fraud, embedding model version registry ambiguity, or RAG-specific provenance loss mechanisms unique to multi-agent shared vector stores.

### RIP_12 - Memory & Session Attacks
- Strength score: 2

ENISA Layer I addresses data security, session management, and access control as ICT security fundamentals. Persistent context pollution, checkpoint integrity, and memory serialization security are data integrity and session security concerns within ENISA's scope. ISO 27002 controls for data protection and access management apply to agent memory store security. Checkpoint integrity against backdoor activation aligns with ENISA's Layer II component compromise threat. ENISA does not address cross-agent session context poisoning propagation, LangGraph checkpoint attribution ambiguity, or multi-agent memory serialization overhead exploitation specific to agentic memory architectures.

### RIP_13 - Infrastructure & Deployment Attacks
- Strength score: 2

ENISA Layer I is specifically focused on ICT infrastructure security, making it directly applicable to container deployment security, image provenance verification, and GPU resource management. Supply chain security — a core ENISA concern — applies to NIM container image integrity and registry security. ISO 27002 controls for physical and virtual infrastructure protection are relevant. Cost manipulation through infrastructure resource accounting is a DoS and financial risk within ENISA's scope. ENISA does not address multi-agent Kubernetes-specific container escape enabling agent-to-agent attacks, MIG-based GPU partition contention, or TensorRT engine binary non-fungibility as an identity spoofing vector.

### RIP_14 - ML/Training & Model Attacks
- Strength score: 2

ENISA Layer II explicitly covers model and data disclosure, component compromise, and supply chain security for ML systems. Model registry integrity, artifact provenance verification, and training data security are core Layer II concerns. MLflow registry identity spoofing through version tagging is a supply chain integrity attack within ENISA's ML-specific threat category. Quantization artifact provenance verification aligns with ENISA's component integrity guidance. ENISA does not address model selection as an implicit privilege boundary, behavioral fingerprinting-based identity forgery, or learning-based model ownership disputes unique to multi-agent training ecosystems.

### RIP_15 - Kubernetes & Container Attacks
- Strength score: 2

ENISA Layer I covers ICT infrastructure security including network security and access control aligned with ISO 27002. Kubernetes service account identity management, RBAC enforcement, namespace isolation, and network policy security are ICT security concerns within ENISA's infrastructure scope. Certificate management and PKI security for inter-service authentication are explicitly covered by ENISA's identity management guidance. GPU resource hoarding as a denial-of-service vector aligns with ENISA's availability threat category. ENISA does not address Kubernetes service account token pooling vulnerabilities, gossip protocol identity spoofing in vector database clusters, or load balancer VIP masking of node provenance in multi-agent query attribution.

### RIP_16 - Load Balancer & Scaling Attacks
- Strength score: 2

ENISA Layer I covers network infrastructure security and ICT system availability. Load balancer security, traffic routing integrity, and auto-scaling policy protection are network and infrastructure security concerns within ENISA's scope. Weighted routing as a privilege inference vector and auto-scaling identity discontinuity creating impersonation opportunities are availability and identity management concerns addressable at the infrastructure level by ENISA's ICT security principles. ENISA does not address load balancer abstraction as an identity spoofing surface for agentic systems or scaling-triggered identity discontinuity as an attack vector unique to cloud-native multi-agent deployments.

### RIP_17 - Service Discovery & Authentication Attacks
- Strength score: 2

ENISA Layer I directly covers network security, authentication, certificate management, and DNS security under ISO 27002. Certificate authority compromise, DNS cache poisoning, and API gateway authentication are explicit ICT security concerns within ENISA's scope. Service registration audit trails and message queue consumer authentication align with ENISA's access management and audit logging guidance. PKI and certificate authority security are explicitly addressed. ENISA does not address message queue consumer identity spoofing through loosely validated consumer tags in multi-agent systems, Prometheus cardinality explosion through agent identity label injection, or RabbitMQ priority queue starvation as a fleet-wide economic denial-of-service vector.

### RIP_18 - Rule/Method/Parameter Attribution Attacks
- Strength score: 1

ENISA does not address agentic planning systems using HTN method libraries, LLM-based rule repositories, or multi-agent delegation chains with natural-language-specified authority. Efficiency metric spoofing for agent trust manipulation, method authorship spoofing in shared registries, delegation chain identity verification gaps, and rule attribution confusion in multi-agent aggregation are agentic governance mechanisms with no analog in ENISA's ICT or ML security categories. General data integrity principles from Layer I abstractly apply to method registry integrity, but ENISA provides no applicable controls for the natural-language-based trust and attribution attack surface.

### RIP_19 - Hybrid & Cross-Paradigm Attacks
- Strength score: 1

ENISA does not address hybrid neural-symbolic AI systems combining LLM agents with symbolic planners and utility function frameworks. Cross-paradigm attribution confusion — where multi-component provenance chains obscure the origin of compromised inputs — and identity spoofing through paradigm-specific credential mechanism diversity (embedding vectors, rule schemas, preference signatures) are attack patterns specific to hybrid agentic architectures that postdate ENISA's 2023 framework. Layer I supply chain security abstractly applies to external neural model provenance, but ENISA provides no guidance for cross-paradigm trust models or hybrid component credential security.

## RIDC — Instruction–data conflation as a foundational "cognitive" vulnerability

### RIDC_1 - UI and User Interaction Attack Surfaces
- Strength score: 1

ENISA Layer I includes user security awareness and secure UI design principles from ISO 27002, but these do not address the LLM-specific instruction-data conflation vulnerability where natural-language UI content functions simultaneously as data and executable control logic for AI agents. Progressive disclosure injection targeting backend agents through unexpanded UI layers, ARIA live region manipulation as persistent context injection, and keyboard shortcut configurations as out-of-band instruction channels exploit the cognitive boundary collapse unique to LLM-based systems. ENISA's general ICT security guidance predates generative AI and provides no applicable controls for these agentic UI attack surfaces.

### RIDC_2 - Messaging and Protocol Injection
- Strength score: 2

ENISA Layer I covers communications security, message integrity, and protocol security under ISO 27002, which addresses authentication and anti-spoofing measures in inter-system messaging protocols. Message authentication, transport security, and protocol integrity are ICT security concerns within ENISA's scope applicable to FIPA-ACL message authentication and protocol downgrade prevention through strict format negotiation. Conversation integrity and anti-replay measures align with ENISA's communications security guidance. ENISA does not address the semantic gap between message performative metadata and LLM-interpreted content in FIPA-ACL, natural-language protocol downgrade enabling injection through format conflation, or conversation-ID chain hijacking exploiting multi-agent trust assumptions.

### RIDC_3 - Framework and State Management Injection
- Strength score: 1

ENISA does not address LangGraph, LangChain, CrewAI, or AutoGen state management architectures. Framework-enforced trust boundary violations through state schema coercion, cross-framework state migration enabling transitive instruction propagation, and reducer confusion creating heterogeneous agent state views are agentic framework vulnerabilities rooted in instruction-data conflation within agent state. These attacks exploit the specific state management models of individual agentic frameworks — an entirely post-2023 attack surface that ENISA's ICT and ML security categories do not cover.

### RIDC_4 - Tool, Function Calling, and Plugin Injection
- Strength score: 2

ENISA Layer I supply chain security guidance and Layer II component integrity principles apply to tool and plugin security. Third-party plugin vetting, tool registry integrity, and function schema validation align with ENISA's supply chain and component compromise threat categories. Plugin registration security and tool metadata integrity are supply chain concerns ENISA explicitly addresses. ENISA does not address the core LLM-specific vulnerability of instruction-data conflation through tool schemas — where tool descriptions, return values, or parameter content become executable instructions — nor memory-driven tool selection manipulation, RAG-retrieved tool metadata poisoning, or transitive function calling injection unique to multi-agent tool chaining.

### RIDC_5 - ReAct and Reasoning Architecture Injection
- Strength score: 1

ENISA does not address ReAct reasoning traces, HTN task decomposition methods, reflection self-critique architectures, or Plan-and-Execute planning phase security. These reasoning architecture injection attacks exploit the instruction-data conflation in LLM reasoning processes — where observation data, task descriptions, precondition specifications, and effect definitions function as executable instructions. ENISA's 2023 framework has no concept of agentic reasoning architectures, hierarchical task networks, or multi-agent reasoning trace sharing, making it unable to provide applicable controls for these attack surfaces.

### RIDC_6 - Streaming and Real-Time Communication Injection
- Strength score: 1

ENISA Layer I covers communications integrity broadly, but streaming LLM response injection through timing attacks is a generative AI-specific vulnerability with no analog in traditional ICT security. The attack exploits the progressive disclosure of token-by-token generation in streaming responses, embedding malicious instructions in middle sections that exploit human review timing gaps and multi-agent handoff windows. ENISA's communications security guidance addresses network-level streaming security but does not address the semantic trust model of progressively generated LLM content or timing-based instruction injection in streaming agentic workflows.

### RIDC_7 - Generation Parameter and Inference Configuration Injection
- Strength score: 2

ENISA Layer II covers data quality for AI systems and component integrity, providing partial applicability to evaluation pipeline security. Evaluation dataset poisoning aligns directly with ENISA's Layer II poisoning threat category, which identifies training and operational data manipulation as a core ML threat. Dataset integrity controls and data provenance verification recommended by ENISA apply to evaluation corpus security. Custom metric function injection from untrusted sources is a component integrity concern within ENISA's supply chain guidance. ENISA does not address natural language metric definition manipulation in LLM-based evaluators, baseline metric manipulation in mutable agent evaluation databases, benchmark ground truth injection corrupting agent-to-agent trust, or multi-agent evaluation orchestration control-flow hijacking through instruction injection.

## RMP — Long-lived cognitive state abuse: memory poisoning and latent backdoors

### RMP_1 - UI/UX Memory Manipulation Attacks
- Strength score: 2

ENISA Layer I addresses session management, access control, and audit logging as ICT security fundamentals, which apply to persistent agent session security, user identity verification in conversation histories, and audit trail requirements for state modifications. Layer II's poisoning threat category provides a framework for recognizing memory content integrity attacks. ENISA's access control guidance (ISO 27002) is applicable to restricting who can write to shared session state. However, ENISA does not address LLM-specific long-lived cognitive state, UI-driven memory injection through multi-agent context sharing, context window eviction manipulation for security constraint removal, or latent backdoor installation through natural language memory entries that activate across sessions.

### RMP_2 - Multi-Agent State Handoff and Cross-Agent Memory Attacks
- Strength score: 2

ENISA Layer II's poisoning threat category and Layer I's network and API security guidance are applicable to cross-agent state integrity, API gateway security, and vector embedding integrity during handoffs. Multi-agent knowledge graph security aligns with ENISA's data security and integrity principles. Protocol security for gRPC and inter-agent communication falls within ENISA's Layer I communications security scope. ENISA does not address swarm intelligence rule injection for emergent behavior corruption, adversarial importance weighting exploiting selective memory retention, memory type misclassification across serialization boundaries, or multi-hop knowledge graph relationship injection through cross-agent coordination.

### RMP_3 - Multimodal and Cross-Modal Poisoning Attacks
- Strength score: 2

ENISA Layer II explicitly identifies evasion and poisoning as core ML threat categories, broadly applicable to cross-modal adversarial attacks and multimodal RAG poisoning. Adversarial inputs crafted to exploit model weaknesses across modalities are evasion-family attacks within ENISA's Layer II threat recognition. Distributed perception corruption and vision-based memory poisoning align with ENISA's data integrity and poisoning controls at a conceptual level. ENISA does not address multimodal fusion manipulation, temporal sensor desynchronization attacks in multi-agent perception systems, cross-modal contradiction exploitation for consensus poisoning, or modality-specific injection vectors such as CSS pseudo-element rendering attacks and DePlot chart linearization injection.

### RMP_4 - Framework-Specific Memory Vulnerabilities
- Strength score: 1

These are framework-specific vulnerabilities in LangChain, LangGraph, AutoGen, CrewAI, and Semantic Kernel — all of which postdate ENISA's June 2023 publication. The core vulnerability of LLM cognitive state poisoning through framework-specific memory mechanisms (ConversationBufferMemory, LangGraph reducer append, AutoGen GroupChat shared history, CrewAI task context inheritance, Semantic Kernel kernel context persistence) has no analog in ENISA's ICT or ML security categories. Layer I data integrity principles apply abstractly to memory store access control, but ENISA provides no applicable controls for framework-specific memory architecture vulnerabilities, checkpoint poisoning for latent backdoor installation, or cross-agent memory namespace collision exploitation.

### RMP_5 - Caching and Persistence Attacks
- Strength score: 2

ENISA Layer I addresses system resilience, data integrity, and configuration management, and Layer II's poisoning category is applicable to cache poisoning as a form of data corruption. Embedding cache integrity, error recovery configuration security, and fallback state management are data integrity concerns within ENISA's scope. Layer I's resilience requirements recommend bounded retry behavior and state validation across recovery cycles. ENISA does not address LLM-specific embedding semantic caches, AI agent retry and fallback mechanisms that persist malicious error context as "prior attempt state," or graceful degradation configuration persistence enabling permanent capability disablement.

### RMP_6 - Streaming and Real-time Processing Attacks
- Strength score: 1

These attacks exploit the incremental nature of LLM streaming output to inject malicious content into agent memory before complete validation occurs, exploiting multi-agent memory synchronization windows through streaming timing manipulation. ENISA's communications security guidance addresses network-level streaming integrity but does not cover streaming LLM content injection into AI agent persistent memory or the race condition between content ingestion and validation in streaming multi-agent pipelines. The semantic trust model of progressively generated token streams — where memory-writing agents process content before the complete stream is available — is a generative AI-specific attack surface with no analog in ENISA's framework.

### RMP_7 - Evaluation and Metrics Poisoning
- Strength score: 2

ENISA Layer II covers poisoning as a core ML threat category and addresses data quality for AI systems, making it conceptually applicable to evaluation dataset integrity and baseline metric security. Data quality controls and provenance verification recommended by ENISA apply to evaluation corpus management. Layer I's audit logging requirements are relevant to evaluation result integrity and baseline storage access control. ENISA does not address LLM-specific evaluation pipeline memory architecture, multi-agent shared baseline storage vulnerabilities, learned evaluation pattern backdoors in adaptive metrics, or AI agent confidence calibration degradation through cascading multi-agent measurement bias.

### RMP_8 - Parameter Tuning and Configuration Poisoning
- Strength score: 2

ENISA Layer I covers configuration management and system integrity, and Layer II addresses training data quality and model integrity as part of the AI life cycle. Hyperparameter configuration security, tuning dataset integrity, and benchmark data provenance align with ENISA's system configuration security and data poisoning threat categories. Multi-modal web benchmark injection via image metadata is an evasion-class attack within Layer II's scope. ENISA does not address context window truncation manipulation as an LLM-specific attack vector, Pareto frontier configuration poisoning in multi-objective ML optimization, or model-specific memory architecture mismatches enabling cross-model state corruption during parameter tuning.

### RMP_9 - Learning and Training Data Attacks
- Strength score: 3

ENISA Layer II directly and explicitly identifies training data poisoning as one of the primary ML-specific threats and recommends controls including data provenance tracking, access control, and integrity verification for training datasets. Few-shot CoT demonstration poisoning, trajectory data injection in reinforcement learning flywheels, and fine-tuning data contamination are all forms of training data and learning process poisoning that fall squarely within ENISA's Layer II poisoning threat category. The recommended controls — data access restriction, provenance verification, and training data integrity monitoring — directly address these attack patterns, making this one of the few RMP subsubsections where ENISA provides strong, directly applicable guidance.

### RMP_10 - Observability and Tracing Attacks
- Strength score: 2

ENISA Layer I addresses audit logging, security monitoring, and system integrity through ISO 27002 controls. Audit trail integrity, timestamp trustworthiness for log entries, and access controls for monitoring system state are within ENISA's scope. Trace manipulation to evade detection aligns with ENISA's requirements for tamper-resistant audit logs. ENISA does not address distributed multi-agent trace causality verification, cross-agent root cause analysis misdirection, confidence score propagation poisoning through trace manipulation, or the complexity explosion in multi-agent forensics that makes attack attribution intractable.

### RMP_11 - Learned Behavior and Memory Evolution
- Strength score: 2

ENISA Layer II covers data and model integrity as part of the ML security life cycle, and Layer II's poisoning threat category is applicable to memory evolution manipulation. Data quality and integrity guidance is relevant to preventing hallucinated data from persisting as authoritative facts in shared memory. ENISA's access control recommendations for ML data assets apply to shared memory consolidation systems. ENISA does not address multi-agent shared memory consolidation vulnerabilities that enable one agent's hallucinations to corrupt other agents' reasoning, or LLM hallucination persistence as a deliberate security attack vector targeting cognitive state evolution mechanisms.

### RMP_12 - Context and Parameter Consistency
- Strength score: 2

ENISA Layer I addresses data integrity, session security, and access control as ICT fundamentals. Parameter validation, data provenance across system interactions, and long-term memory security are data integrity concerns within ENISA's scope. Cross-session poisoning aligns with ENISA's session management security guidance. ENISA does not address multi-turn multi-agent coherence degradation cascading across agent boundaries, tool selection semantic consistency across framework boundaries, parameter validation provenance loss in agentic context sharing, or memory slice attacks exploiting heterogeneous query patterns in multi-agent shared stores.

### RMP_13 - Reasoning Transparency and Trust
- Strength score: 1

ENISA Layer II addresses explainability and transparency as AI trustworthiness properties, but from the perspective of promoting transparency rather than securing against its adversarial weaponization. Reasoning transparency weaponization — where explicit chain-of-thought reasoning increases user trust in malicious outputs — and multi-agent reasoning disagreement manipulation for trust confusion are adversarial uses of AI trustworthiness features that ENISA encourages without analyzing as attack surfaces. The security risk created by explainability itself (transparent reasoning being more convincing for social engineering) is not addressed in ENISA's trustworthiness framework.

### RMP_14 - Efficiency and Resource Tracking
- Strength score: 1

ENISA does not address LLM-specific efficiency optimization systems, conversation history summarization compression as an injection persistence surface, or cost attribution memory as an orchestration manipulation vector. While Layer I's DoS controls abstractly apply to availability degradation, the specific attack of poisoning efficiency baselines to activate hidden behavioral payloads through optimization target manipulation has no analog in ENISA's framework. Economic resource tracking and token cost attribution in LLM-based systems are entirely outside ENISA's scope, and the concept of "learned backdoors" disguised as efficiency insights is a modern agentic threat pattern the 2023 framework does not address.

### RMP_15 - Vector Database and Embedding Poisoning
- Strength score: 2

ENISA Layer II directly identifies data poisoning as a core ML threat and recommends controls for operational data integrity. Vector database security aligns with ENISA's data storage and access control guidance from Layer I. Persistent volume integrity and cluster replication security are ICT infrastructure concerns within ENISA's scope. HNSW index rebuild integrity is a data integrity concern addressed by ENISA's general guidance. ENISA does not address HNSW graph structure poisoning through efConstruction parameter manipulation, cross-provider embedding space inconsistency exploitation for targeted retrieval, or the specific mechanics of embedding-level manipulation that bypasses format-level integrity checks while corrupting semantic correctness.

### RMP_16 - ETL Pipeline and Data Processing Attacks
- Strength score: 2

ENISA Layer II covers data quality and integrity for AI systems as part of the AI life cycle, and Layer I addresses ICT data pipeline security including access control and configuration management. Data processing integrity, access controls for ETL systems, and quality threshold configuration security are within ENISA's scope. Dataset integrity verification and access controls for data modification pipelines align with ENISA's data security principles. ENISA does not address RAG-specific ETL state file manipulation, MinHash deduplication collision attacks for training data substitution, semantic cache similarity threshold exploitation through adversarial embedding geometry, or chunking overlap poisoning specific to AI knowledge base construction workflows.

## RND — Non-determinism, continual change, and assurance gaps as a risk surface

### RND_1 - UI/Streaming and Real-Time Generation
- Strength score: 1

The security-relevant non-determinism of LLM token sampling — where attacks succeed probabilistically, defeat audit reproducibility, and exploit ephemeral vulnerability windows during streaming — is a generative AI-specific challenge outside ENISA's framework. ENISA Layer II's reliability and robustness trustworthiness properties are abstractly applicable, but ENISA provides no guidance for stochastic attack surfaces that succeed only probabilistically in production, streaming race conditions between generation and safety filtering in multi-agent pipelines, or timing-based exploitation of latency non-determinism across agent handoffs.

### RND_2 - Progressive Disclosure and Error Handling
- Strength score: 1

ENISA Layer I covers audit logging and error handling as ICT security fundamentals, but the exponential state complexity from UI progressive disclosure layers in multi-agent systems — creating O(2^NM) attribution complexity and enabling attackers to hide malicious activity in collapsed error layers — is an agentic architecture vulnerability with no analog in ENISA's framework. Non-deterministic error chain propagation across agent cascades, where errors appear and disappear before forensic capture, defeats the deterministic audit trail assumptions underlying ENISA's logging guidance.

### RND_3 - Context, Persistence, and Session Management
- Strength score: 2

ENISA Layer I addresses session management and access control as ICT security fundamentals, which applies to persistent context integrity and stale security context risks. Cross-session state security and session boundary enforcement are within ENISA's scope. ENISA does not address multi-turn LLM attacks distributing malicious intent across conversation turns to defeat stateless safety classifiers, stochastic execution path variance preventing reliable audit replay of cross-agent memory poisoning, or the empirically demonstrated 80–95%+ success rates of multi-turn attacks that ENISA's session security guidance cannot address.

### RND_4 - Multi-Agent Coordination and Decision-Making
- Strength score: 1

ENISA does not address non-determinism arising from multi-agent coordination — streaming attribution confusion enabling concurrent malicious instruction delivery, command palette AI prediction non-determinism defeating guardrail consistency, approval workflow confidence score variance creating auto-approval exploitation windows, or dashboard race conditions masking malicious operations behind transient "safe" UI states. These are LLM-specific multi-agent coordination phenomena with no analog in ENISA's ICT or ML security categories.

### RND_5 - Retry, Resilience, and Error Recovery
- Strength score: 2

ENISA Layer I covers resilience, business continuity, and availability management. Retry logic security, circuit breaker configuration, and fallback strategy design are resilience engineering concerns within ENISA's scope. Auto-retry hiding attack amplification in error logs and coordinated non-idempotent operation exploitation through retry storms are availability threats addressable at the infrastructure level. ENISA does not address non-deterministic retry timing as an exploitation surface, probabilistic fallback route selection creating untestable attack branches, or the security implications of circuit breaker state non-determinism in distributed multi-agent deployments.

### RND_6 - Checkpoint, State, and Recovery Management
- Strength score: 2

ENISA Layer I addresses backup and recovery management, data integrity, and system state consistency through ISO 27002 controls. Checkpoint security, state integrity validation during recovery, and access controls for checkpoint storage are within ENISA's scope. ENISA does not address adversarial checkpoint state injection during capture-to-resumption windows, multi-agent distributed checkpoint poisoning enabling synchronized compound backdoor activation, or determinism violation amplification through cascading state divergence across coordinated agent replay.

### RND_7 - Tool Integration and API Management
- Strength score: 2

ENISA Layer I's change management and supply chain security guidance applies to tool API version control, schema versioning, and dependency management. Layer II's component integrity guidance is relevant to tool versioning and API evolution management. Third-party tool and API security are supply chain concerns within ENISA's scope. ENISA does not address LLM function calling behavioral discontinuities from model updates, few-shot example poisoning in tool schema definitions, demonstration-driven fallback injection routing agents to compromised implementations, or silent failure propagation through API version drift in multi-agent chains.

### RND_8 - Framework and Architecture Non-Determinism
- Strength score: 1

ENISA does not address the combinatorial non-determinism arising from multi-pattern agentic compositions — where (K^M)^N * R^N behavioral state spaces make exhaustive testing intractable — or the assurance invalidation from continual framework updates requiring N(N-1)/2 re-evaluation combinations across multi-agent deployments. While Layer II recommends regular AI system re-evaluation, ENISA provides no guidance for maintaining security assurance under continuously updated agentic frameworks with multiplicative non-determinism that fundamentally defeats completeness of security testing.

### RND_9 - LangChain/LangGraph Specific Non-Determinism
- Strength score: 1

ENISA does not address LangChain or LangGraph-specific non-determinism sources: conditional edge evaluation depending on external stochastic factors, reducer behavior divergence across agent update cycles, checkpoint restoration creating non-deterministic cyclic workflow paths, confidence score non-determinism intermittently disabling safety gates, memory update ordering race conditions in distributed multi-agent sharing, model version update invalidating fleet-wide security evaluations, or tool API compatibility drift. These are framework-specific vulnerabilities entirely outside ENISA's 2023 framework.

### RND_10 - AutoGen Specific Non-Determinism
- Strength score: 1

ENISA does not address AutoGen-specific non-determinism: message-driven conversation flows producing different agent interaction paths from identical inputs defeating reproducible security testing, GroupChat speaker selection non-determinism creating probabilistically exploitable malicious agent activation windows, or the combinatorial assurance invalidation from AutoGen, CrewAI, and underlying LLM simultaneous updates. These framework-specific attack surfaces emerged after ENISA's June 2023 publication.

### RND_11 - CrewAI Specific Non-Determinism
- Strength score: 1

ENISA does not address CrewAI's hierarchical task routing non-determinism, where probabilistic manager-based delegation creates exploitable windows where attacks succeed only when routed to specific compromised workers while failing with legitimate ones. This framework-specific multi-agent routing non-determinism has no analog in ENISA's ICT or ML security categories.

### RND_12 - Semantic Kernel Specific Non-Determinism
- Strength score: 1

ENISA does not address Semantic Kernel-specific non-determinism: LLM-driven plugin routing producing non-deterministic function selection paths that defeat audit reproducibility guarantees, dynamic plugin registry state changes invalidating prior security evaluations, FunctionChoiceBehavior configuration drift creating inconsistent routing across agent updates, or concurrent service registration lifecycle changes causing agents to observe different service sets. These are Semantic Kernel framework-specific vulnerabilities outside ENISA's scope.

### RND_13 - Multimodal and Vision-Language Models
- Strength score: 2

ENISA Layer I change management and version control principles apply to managing model version drift across deployments. Layer II's AI life cycle management recommendation is conceptually applicable to multimodal component update governance. Supply chain security applies to multimodal model provenance. ENISA does not address multimodal model update drift creating inconsistent security postures across agents using different CLIP/Whisper/DePlot versions, cross-agent embedding quantization inconsistencies (FP32 vs INT8) creating retrieval ranking divergence, or the security implications of asynchronous multimodal model update schedules in shared vector store deployments.

### RND_14 - Evaluation, Testing, and Benchmarking Non-Determinism
- Strength score: 2

ENISA Layer II emphasizes regular AI system evaluation and monitoring as part of the AI life cycle, which has partial applicability to evaluation integrity and baseline consistency management. Layer I change management procedures are relevant to model update governance. ENISA does not address stochastic evaluation non-determinism defeating regression testing, multiple comparison problems invalidating statistical significance in multi-agent evaluation, or the perpetual assurance gap from asynchronous multi-agent model update schedules where N(N-1)/2 version combinations require re-validation.

### RND_15 - Temperature and Sampling Parameters
- Strength score: 1

ENISA does not address LLM temperature and sampling parameter security. Cross-agent parameter drift from independent tuning cycles creating inconsistent security postures — where one agent at temperature 0.2 rejects prompt injections while a peer at 0.5 accepts them — is a generative AI-specific vulnerability with no analog in ENISA's framework. Continuous parameter re-tuning creating moving-target security and stochastic parameter variability as an adversarial search surface are not covered.

### RND_16 - Parameter Extraction and Tool Calling
- Strength score: 1

ENISA Layer II's reliability property is abstractly relevant, but stochastic parameter generation creating targeted hallucination windows, probabilistic tool selection enabling adversarial optimization for malicious tool invocation probability, non-deterministic fallback ordering creating unpredictable attack surfaces, and parameter accuracy variance across temperature settings affecting security-critical parameter extraction are LLM inference-specific non-determinism phenomena outside ENISA's guidance.

### RND_17 - Retrieval-Augmented Generation (RAG) and Multi-Hop QA
- Strength score: 2

ENISA Layer II covers data quality and integrity for AI systems, and Layer II's poisoning threat category is applicable to ranking model manipulation and adversarial document injection for RAG poisoning. Data provenance and source integrity are data security concerns within ENISA's scope. ENISA does not address multi-hop RAG retrieval chain injection where query reformulations become adversarial instructions, evidence grounding failures enabling false bridging document acceptance, semantic similarity exploitation for instruction injection, or knowledge graph hallucination blindness propagating across multi-agent trust chains.

### RND_18 - Infrastructure and Deployment Non-Determinism
- Strength score: 2

ENISA Layer I addresses infrastructure security, availability management, and change control including message queue security, event delivery guarantees, and API gateway configuration. These infrastructure security concerns are within ENISA's ICT security scope. ENISA does not address swarm Byzantine agent injection at density thresholds causing consensus failures invisible to threshold-based monitoring, vector database index staleness as a runtime assurance gap, Prometheus scrape interval latency compounding across multi-agent decision chains, or canary deployment compatibility gaps in multi-agent LLM ecosystem rollouts.

### RND_19 - Kubernetes and Container Orchestration
- Strength score: 2

ENISA Layer I covers ICT infrastructure security including container orchestration configuration, RBAC enforcement, and infrastructure change management. Kubernetes security hardening and pod placement security are within ENISA's infrastructure scope. ENISA does not address HPA scaling timing exploitation creating non-deterministic agent population attacks, StatefulSet initialization race conditions enabling cross-agent targeting, or Kubernetes scheduler indeterminism enabling intermittent placement-based compromise that defeats validation in multi-agent deployments.

### RND_20 - Deployment and Inference Infrastructure
- Strength score: 2

ENISA Layer I covers infrastructure change management, deployment security, and availability management. Container image version control, rolling update governance, and traffic splitting configuration are infrastructure security concerns within ENISA's scope. ENISA does not address 2^N state combinations from simultaneous multi-agent canary deployments, asynchronous event processing ordering attacks, or cross-version compatibility gaps creating unobservable state transitions during rolling multi-agent deployments.

### RND_21 - Inference Hardware and Optimization
- Strength score: 1

ENISA Layer I addresses hardware infrastructure security, but GPU-level inference non-determinism — floating-point rounding differences across replicas creating exploitable probability distributions, kernel fusion GPU scheduler variance creating N-agent non-deterministic states, and TensorRT engine architecture-specific behavioral variance across heterogeneous hardware fleets — are ML inference hardware-specific attack surfaces not addressed by ENISA's ICT security framework. The perpetual assurance gap from continuous zero-downtime model updates is an agentic deployment pattern outside ENISA's scope.

### RND_22 - Optimization and Efficiency
- Strength score: 1

ENISA Layer I's configuration management principles abstractly apply to optimization parameter governance, but profiling-induced non-determinism creating measurement-dependent security behavior, speculative decoding acceptance rate variability as an untestable security surface, and asynchronous configuration optimization creating fleet-wide behavioral drift are LLM inference optimization-specific phenomena not addressed by ENISA.

### RND_23 - Chain-of-Thought (CoT), Tree-of-Thought (ToT), and Reasoning Non-Determinism
- Strength score: 1

ENISA does not address CoT or ToT reasoning architectures. The exponential path explosion from multi-agent CoT coordination — making red-teaming coverage computationally infeasible — reasoning consistency drift across asynchronous agent updates creating temporal poisoning effectiveness windows, and backtracking state divergence in distributed ToT systems are LLM reasoning-specific non-determinism patterns with no analog in ENISA's 2023 framework.

### RND_24 - Self-Consistency and Multiple Reasoning Paths
- Strength score: 1

ENISA does not address self-consistency sampling mechanisms. Intentional stochastic decoding creating assurance gaps where testing cannot cover stochastic behavior space, temperature tuning parameter drift simultaneously affecting all agents sharing optimization, and context window compression non-determinism across multi-agent handoffs with different window sizes are self-consistency-specific LLM security challenges outside ENISA's scope.

### RND_25 - Hierarchical Task Network (HTN) Planning Non-Determinism
- Strength score: 1

ENISA does not address HTN planning systems. LLM-based HTN method selection creating stochastic decomposition paths where validation of M1 provides no guarantee against M2 activation, method library version mismatch across asynchronous agent updates breaking decomposition guarantees, and partial order scheduling non-determinism enabling unsafe constraint resolution are planning system-specific vulnerabilities entirely outside ENISA's framework.

### RND_26 - Monte Carlo Tree Search (MCTS) Planning Non-Determinism
- Strength score: 1

ENISA does not address MCTS planning systems. Non-deterministic tree convergence enabling coordinated conflicting plans across multi-agent deployments, simulation budget variance from resource competition creating quality gaps that defeat assurance, and adversarial replanning exploitation through forced replanning into dangerous paths are MCTS-specific planning non-determinism patterns outside ENISA's scope.

### RND_27 - Episode-Based Memory and Consolidation
- Strength score: 1

ENISA does not address episodic memory consolidation systems. Non-deterministic episode retrieval ranking enabling adaptive adversarial crafting of intermediate-score malicious episodes, consolidation timing exploitation creating attack windows during absent monitoring, and embedding model update non-determinism creating transient poisoning windows through staged rollouts are episodic memory-specific agentic vulnerabilities outside ENISA's 2023 framework.

### RND_28 - Knowledge Graphs and Memory Stores
- Strength score: 2

ENISA Layer I data integrity and caching/storage security guidance, and Layer II data quality controls, are abstractly applicable to knowledge base consistency management and cache integrity. Knowledge graph access control and data store security are within ENISA's scope. ENISA does not address probabilistic fact evaluation creating exploitable non-determinism in knowledge graphs, temporal validity window boundary flickering enabling intermittent instruction injection, or the security implications of caching invalidation timing inconsistencies causing temporary semantic divergence across multi-agent shared stores.

### RND_29 - Context Assembly and Working Memory
- Strength score: 1

The audit blind spots created by non-deterministic context assembly order — where security-critical context positioning changes between executions, making reproducible security auditing impossible — are generative AI-specific challenges that defeat ENISA's deterministic audit trail assumptions. ENISA Layer I audit logging requirements provide no guidance for environments where identical inputs produce different security-relevant context assemblies due to non-deterministic retrieval ranking and async retrieval timing across multi-agent pipelines.

### RND_30 - Utility and Decision Making
- Strength score: 1

ENISA does not address utility-based AI decision making. Probabilistic sampling variability in Monte Carlo expected utility estimation enabling divergent decisions across agents, non-deterministic weight adjustment mechanisms creating unsafe convergence in multi-agent utility systems, and evaluation non-reproducibility for utility-based decisions are expected utility theory-specific AI decision making challenges outside ENISA's framework.

### RND_31 - Rule-Based and Adaptive Systems
- Strength score: 1

ENISA Layer I configuration management abstractly applies to rule system versioning and change control, but adaptive rule learning systems that create non-deterministic behavioral drift during deployment — where learning rule instability and heuristic parameter drift from independent tuning create inconsistent rule execution ordering — are adaptive AI system challenges outside ENISA's scope.

### RND_32 - Learning-Based Policies and Reinforcement Learning
- Strength score: 2

ENISA Layer II explicitly addresses ML security threats and recommends controls for model integrity, and gradient-based adversarial policy perturbations are a form of adversarial ML attack within ENISA's Layer II evasion threat category. Model robustness and policy integrity are within ENISA's scope. ENISA does not address online RL systems that continuously invalidate security assurance through policy drift, MARL emergent behavior unpredictability making assurance computationally infeasible, exploration phase unpredictability as a deployed security surface, or experience replay temporal non-determinism creating shared policy corruption pathways.

### RND_33 - Hybrid and Heterogeneous Systems
- Strength score: 1

ENISA does not address hybrid neural-symbolic systems combining LLM agents with reinforcement learning, symbolic planners, and rule engines. Temperature heterogeneity exploitation crafting payloads reliable against high-temperature agents but failing against deterministic ones, paradigm-specific non-determinism in cooperative convergence cycles, and feedback loop timing exploitation forcing dangerous convergence paths in hybrid multi-agent cooperation are cross-paradigm security challenges outside ENISA's framework.

### RND_34 - Other Risks/Threats/Vulnerabilities Worth Noting
- Strength score: 2

ENISA Layer I change management, configuration management, and testing procedure guidance are applicable to a subset of these miscellaneous non-determinism risks — specifically infrastructure concerns like auto-scaling configuration variability, load balancing non-determinism, CI/CD timing variations, and framework version change management. Layer II's evaluation guidance is relevant to evaluation instability and model update timing gaps. ENISA does not address the AI knowledge base construction-specific non-determinism sources: semantic chunking boundary variations across heterogeneous ETL implementations, deduplication hash collision race conditions in concurrent multi-agent extraction, REST API pagination cursor non-determinism creating inconsistent record sets, filesystem modification time resolution variability across heterogeneous agent infrastructure, or batch subdivision ordering deadlocks in partial ETL failure recovery.

## RTM — Telemetry and monitoring blind spots specific to cognitive and tool behavior

### RTM_1 - UI and Interface Telemetry Gaps
- Strength score: 2

ENISA Layer I explicitly requires comprehensive audit logging, security monitoring, and access logging as core ICT security controls under ISO 27002. These requirements apply generally to UI telemetry, session logging, tool invocation auditing, and user decision tracking. The principle that security-relevant events must be logged with attribution is within ENISA's scope. ENISA does not address semantic content analysis in logs, progressive disclosure layer-specific monitoring for hidden technical views, multi-agent attribution graphs for AI-generated content, reasoning trace telemetry for LLM cognitive state monitoring, cost and economic pattern analysis as security indicators, or HITL approval workflow reasoning provenance capture.

### RTM_2 - Framework-Specific Architecture and Logging Gaps
- Strength score: 2

ENISA Layer I covers audit logging, access control, and monitoring requirements for ICT systems, establishing the general principle that tool invocations, state mutations, and system operations should be logged with attribution. These broadly apply to the logging gaps described across Plan-and-Execute architectures, tool chain monitoring, and state management. ENISA does not address framework-specific logging gaps in LangChain, LangGraph, AutoGen, CrewAI, or Semantic Kernel, correlation ID cryptographic verification for distributed trace integrity, swarm emergent behavior monitoring requirements, cross-agent reasoning trace correlation for attack detection, or checkpoint-mediated state mutation audit evasion.

### RTM_3 - Tool Invocation and Function Calling Monitoring
- Strength score: 2

ENISA Layer I explicitly covers access control audit trails, which apply to tool invocation authorization logging, privilege audit trails, and resource access accountability. The general requirement to log all access to sensitive resources and maintain attribution for tool execution is within ENISA's scope. ENISA does not address cross-agent tool invocation correlation attacks where sequences appear legitimate per-agent but malicious in aggregate, distributed anomaly detection evasion through invocation spreading, parallel execution masking of individual tool latency, asynchronous causality tracking failures in multi-agent tool chains, or the semantic gap between tool parameters as logged and their security implications in context.

### RTM_4 - Multimodal and Streaming Response Processing
- Strength score: 2

ENISA Layer I's audit logging requirements apply to error handling, retry sequences, circuit breaker state changes, fallback routing decisions, and resource utilization monitoring as general ICT security concerns. Resilience monitoring and alert management for system availability are within ENISA's scope. ENISA does not address multimodal vision model processing opacity where unmonitored outputs drive tool selection, cross-modal consistency validation between image and text content, streaming LLM response monitoring gaps where malicious content evades detection during temporal delivery windows, or multi-agent streaming correlation attacks exploiting decoupling of content delivery from security validation.

### RTM_5 - Evaluation and Assessment Telemetry Gaps
- Strength score: 2

ENISA Layer II emphasizes regular AI system evaluation and monitoring as part of the AI life cycle, and Layer I's audit logging requirements include evaluation activity tracking and result integrity verification. Metric telemetry integrity, evaluation dataset integrity controls, and audit trail completeness are within ENISA's scope. ENISA does not address semantic content analysis of evaluation results for prompt injection indicators, evaluation agent decision reasoning transparency requirements, multi-agent evaluation orchestration control-flow monitoring, gradual metric degradation attacks that stay below alert thresholds, or distributed evaluation audit trail fragmentation preventing cross-agent attack reconstruction.

### RTM_6 - Metrics Collection and Manipulation Evasion
- Strength score: 2

ENISA Layer I covers security monitoring requirements, audit log integrity, access control auditing, and the principle of comprehensive observability. Alert threshold calibration, log aggregation integrity, cost attribution auditing, and baseline management for anomaly detection are within ENISA's scope. ISO 27002's monitoring guidance establishes the requirement for correlated security event analysis. ENISA does not address cross-agent reasoning attribution, multi-agent system-level coherence validation as a monitoring requirement, entropy tracking for LLM hallucination detection, confidence score decomposition for manipulation detection, systematic efficiency metric gaming through adversarial optimization, or attribution traceability across multi-agent workflow decision chains.

### RTM_7 - Infrastructure and Observability Stack Blind Spots
- Strength score: 2

ENISA Layer I directly covers ICT infrastructure security, monitoring, audit logging, and access management through ISO 27002. Infrastructure monitoring requirements, Kubernetes audit logging, container registry integrity, message queue security, load balancer configuration, and access control auditing are ICT infrastructure concerns ENISA's Layer I explicitly addresses. Supply chain security principles apply to container image validation. ENISA does not address GPU quantization artifact telemetry absence enabling silent quantization-based attacks, vector database semantic quality monitoring requirements, TensorRT engine binary inspection at scale, speculative decoding token prediction opacity as an inference-layer blind spot, or cross-agent coordination timing blind spots invisible to per-service monitoring.

### RTM_8 - Reasoning and Cognitive State Monitoring
- Strength score: 1

These monitoring gaps are specific to LLM reasoning architectures — CoT, ToT, MCTS, HTN decomposition, reflection, and multi-agent cognitive coordination — that have no analog in ENISA's ICT or ML security monitoring guidance. The fundamental observability challenge — that LLM reasoning is expressed in unstructured natural language requiring semantic analysis rather than deterministic log parsing, and that coordinated attack intent distributed across multiple agents' reasoning traces is invisible to per-agent analysis — is a generative AI-specific problem that ENISA's 2023 framework does not address. ENISA's general audit logging requirements cannot be applied to semantic reasoning state monitoring without substantial agentic-specific guidance not present in the framework.

### RTM_9 - Memory Systems and Knowledge Base Observability
- Strength score: 2

ENISA Layer I's data security and access management guidance applies to memory and knowledge base access logging, cache integrity controls, and audit trail requirements for data access. Layer II's data quality guidance is relevant to knowledge base integrity monitoring and embedding quality control. Data access audit trails and configuration drift monitoring are within ENISA's scope. ENISA does not address episodic memory retrieval monitoring for evasion detection, LLM working memory phase transition blind spots where context clearing destroys forensic evidence, vector database semantic quality monitoring (Recall@k), reasoning trace truncation hiding decision provenance, or hierarchical memory compression creating blind spots between summary and full-history storage layers.

### RTM_10 - Decision Logic and Utility Function Monitoring
- Strength score: 1

ENISA Layer II recommends AI life cycle monitoring and evaluation, but the specific monitoring requirements — utility function calculation telemetry, specification gaming detection through metric redefinition, policy output distribution analysis for adversarial perturbation detection, gradient flow monitoring for federated learning backdoors, multi-paradigm objective coherence tracking, and demonstration curation audit trails — are AI decision-making monitoring challenges that ENISA's guidance does not address. The challenge that LLM-based agents exhibit specification gaming where they achieve metrics while violating intent — invisible in standard infrastructure monitoring — is a post-2023 agentic security phenomenon outside ENISA's scope.

### RTM_11 - Vector Database and RAG Pipeline Telemetry
- Strength score: 2

ENISA Layer I's data security and system integrity guidance applies to ETL pipeline monitoring, batch processing audit trails, cache integrity monitoring, state file access control, and cost attribution auditing for resource usage. Layer II's data quality guidance is relevant to validation threshold monitoring and quality metric tracking for AI knowledge bases. ENISA does not address vector database retrieval quality metrics (Recall@k, precision at threshold), hybrid search alpha parameter rationale logging for security auditing, HNSW shard-level attribution for targeted poisoning detection, semantic cache poisoning through similarity threshold exploitation, or the RAG-specific monitoring gap where technical infrastructure metrics appear healthy while semantic retrieval quality silently degrades.

### RTM_12 - Detection Evasion and Attack Exploitation
- Strength score: 2

ENISA Layer I addresses security incident detection, alert management, monitoring system integrity, and audit trail completeness. Alert fatigue management, monitoring infrastructure protection, and incident response telemetry are ICT security concerns explicitly within ENISA's scope. Audit trail fragmentation prevention and log aggregation security align with ENISA's mandatory logging and monitoring requirements. ENISA does not address multi-agent-specific monitoring evasion — coordinated metric manipulation rotating across distributed agents to stay below per-agent thresholds while achieving fleet-wide impact, GPU DCGM telemetry exposure enabling capacity-calibrated denial-of-service attacks, OpenTelemetry trace aggregation enabling complete agentic architecture reverse-engineering, or cross-agent safety violation correlation blindness in distributed guardrail deployments.

## RTE — Multi-agent trust exploitation and self-replicating prompt malware

### RTE_1 - Dashboard & UI Attribution Attacks
- Strength score: 2

ENISA Layer I covers identity management and cryptographic controls under ISO 27002, which apply at the principle level to agent authentication requirements, digital signature requirements for content attribution, and access control for dashboard data. The requirement for cryptographic binding between identity claims and system operations — rather than relying on self-reported metadata — is within ENISA's security architecture scope. ENISA does not address AI-specific dashboard attribution spoofing where UI architectures rely on manipulable agent metadata fields, multi-agent attribution complexity driving reliance on visual indicators over cryptographic verification, or the specific identity attestation requirements for agentic content in LLM-based systems.

### RTE_2 - Trust Mechanisms & Inter-Agent Communication
- Strength score: 1

ENISA does not address the novel attack surface of AI-to-AI social engineering where natural language communication between agents replaces protocol-based authentication. Transitive trust exploitation through agent reputation anchoring, circular verification loop compromise enabling coordinated false consensus, and specialization trust collapse through cross-domain injection are multi-agent cognitive security vulnerabilities with no analog in ENISA's ICT security or ML threat categories. The fundamental shift from deterministic protocol-based trust to probabilistic natural-language-based inter-agent trust — which enables these attacks — is a post-2023 agentic security phenomenon outside ENISA's framework.

### RTE_3 - Confidence & Scoring Attacks
- Strength score: 1

ENISA Layer II covers model integrity and output trustworthiness at a general level, but confidence score inflation through adversarial parameter tuning for multi-agent routing manipulation, streaming confidence manipulation enabling downstream commitment exploitation before caveat delivery, and utility function poisoning propagating inflated confidence through approval chains are LLM-specific multi-agent trust attack patterns not addressed. The attack surface — where confidence scores serve as trust signals between agents replacing protocol-based authorization — is a novel agentic architectural vulnerability outside ENISA's scope.

### RTE_4 - Prompt Injection via Message/History Sharing
- Strength score: 1

Self-replicating prompt malware propagating through shared conversation histories, serialized agent memory, and GroupChat message buffers is a fundamentally novel attack class with no analog in ENISA's framework. While Layer II addresses data integrity and poisoning, the propagation mechanism — malicious instructions treated as established system context by all consuming agents, enabling worm-like geometric spread — is a generative AI-specific attack pattern that ENISA's 2023 framework does not contemplate. The epidemiological nature of prompt worm propagation across agent memory systems has no ICT or ML security precedent in ENISA's categories.

### RTE_5 - UI & Disclosure Vulnerabilities
- Strength score: 1

ENISA does not address AI agent role assignment security in dynamic multi-agent orchestration systems or progressive disclosure layer-specific trust exploitation patterns unique to agentic UI architectures. The attack surface — where trusted role labels are dynamically assigned by orchestration agents susceptible to injection, and where technical disclosure layers are consumed by specialized agents but not reviewed by users — is an agentic UI security pattern with no analog in ENISA's guidance.

### RTE_6 - Tool & Command Injection
- Strength score: 1

ENISA does not address prompt injection malware propagation through UI acceptance workflows or context poisoning in AI-powered command palettes. These attacks exploit the low-friction user acceptance patterns in suggestion UIs to propagate malicious instructions into document corpora consumed by downstream agents — an LLM-specific instruction-data conflation attack through tool interface surfaces outside ENISA's 2023 framework.

### RTE_7 - Framework-Specific Attacks
- Strength score: 1

These framework-specific multi-agent trust exploitation patterns — LangChain's agent-tool delegation assumption violation, LangGraph's state field-based instruction propagation, AutoGen's conversational self-replicating injection, CrewAI's hierarchical delegation exploitation, Semantic Kernel's plugin routing weaponization — exploit unique trust assumptions in each framework's delegation model. ENISA does not address agentic framework delegation security, prompt malware propagation through framework-specific communication protocols, or conditional edge routing as an instruction propagation channel.

### RTE_8 - AutoGen-Specific Attacks
- Strength score: 1

ENISA does not address AutoGen-specific multi-agent conversational social engineering where compromised agents persuade peers through natural language dialogue to perform unauthorized actions. Conversational trust exploitation through reputation anchoring in AutoGen's agent selection mechanism — where attackers leverage multi-agent reputation systems to route malicious influence through trusted agents — is a post-2023 agentic attack pattern outside ENISA's framework.

### RTE_9 - CrewAI-Specific Attacks
- Strength score: 1

ENISA does not address CrewAI-specific manager-worker trust exploitation, few-shot decomposition injection teaching malicious hierarchical delegation patterns, or demonstration bias embedding subtask reinterpretation biases that propagate dangerous operations through multi-agent hierarchical chains. These CrewAI-specific attacks exploit the framework's hierarchical task delegation model — an entirely post-2023 attack surface outside ENISA's scope.

### RTE_10 - Semantic Kernel-Specific Attacks
- Strength score: 2

ENISA Layer I supply chain security and access control guidance apply to plugin registry integrity and the requirement for authenticated plugin registration. The principle of validating the integrity and authenticity of third-party components before loading — directly applicable to plugin registry security — is within ENISA's supply chain security scope. ENISA does not address Semantic Kernel's dynamic plugin registration enabling runtime malicious plugin insertion, LLM-driven function routing creating probabilistic selection that enables schema-based impersonation attacks, or the specific trust model where plugin descriptions serve as trust signals rather than cryptographic identity assertions.

### RTE_11 - Multimodal Attacks
- Strength score: 1

ENISA Layer II identifies evasion attacks broadly as an ML threat category, but self-replicating multimodal worm propagation through RAG retrieval cycles — where poisoned images transit as synthesis inputs across multiple agent processing steps without text-based defenses detecting embedded instructions — cross-agent modality contradiction attacks for false consensus building, and vision model attribution confusion enabling multi-agent social engineering are multimodal agentic attack patterns that postdate ENISA's 2023 framework entirely.

### RTE_12 - Streaming & Continuous Processing
- Strength score: 1

These attacks exploit resilience mechanism communication channels (retry failure messages, fallback routing announcements, circuit breaker state transitions) as inter-agent social engineering vectors. While ENISA Layer I covers resilience and communications security, the security of resilience mechanism natural language communication in agentic systems — where messages carry injectable instructions treated as trusted operational directives by downstream agents — is an agentic coordination trust vulnerability outside ENISA's scope.

### RTE_13 - Evaluation & Benchmarking
- Strength score: 2

ENISA Layer II covers AI life cycle evaluation and Layer I supply chain security applies to evaluation library dependencies and tool integrity. Evaluation dataset integrity, benchmark validation, and change management for evaluation infrastructure are within ENISA's scope at a conceptual level. ENISA does not address self-replicating evaluation metric definition malware propagating through evaluation agent networks, transitive trust exploitation in multi-stage evaluation chains, benchmark leaderboard manipulation enabling malicious agent promotion to trusted production roles, or false consensus attacks creating coordinated metric misrepresentation across all agents simultaneously.

### RTE_14 - Web & LLM Evaluation Attacks
- Strength score: 1

ENISA does not address multi-hop QA intermediate step poisoning where early sub-question answers containing injected instructions propagate as trusted inputs through sequential evaluation agents. This attack exploits the trust model in multi-agent evaluation decomposition — where each agent inherits upstream outputs without independent validation — a pattern with no analog in ENISA's framework.

### RTE_15 - Model Tuning & Configuration Attacks
- Strength score: 1

ENISA does not address model capability assumption exploitation for social engineering, prompt cache poisoning as a persistent multi-agent malware distribution mechanism, iteration budget manipulation creating attack detection evasion windows, or adaptive routing configuration exploitation for transitive trust manipulation. These are LLM inference configuration-specific multi-agent trust attack vectors outside ENISA's 2023 framework.

### RTE_16 - Reasoning Trace & Chain-of-Thought Attacks
- Strength score: 1

ENISA does not address CoT-wrapped social engineering where malicious reasoning is presented as legitimate analytical justification, or reasoning trace worm propagation where malware is embedded in stored reasoning that agents naturally retrieve and incorporate as prior work. These LLM reasoning-specific multi-agent propagation vectors have no analog in ENISA's framework.

### RTE_17 - Tree of Thought & Sampling Attacks
- Strength score: 1

ENISA does not address quality score consensus exploitation for multi-agent trust manipulation, sampling diversity exploitation where diverse attack demonstrations create false legitimacy signals, majority voting weaponization for coordinated trust building, or path quality inflation as credential spoofing in multi-agent ToT systems. These sampling-architecture-specific trust attacks are LLM reasoning pattern vulnerabilities outside ENISA's scope.

### RTE_18 - Hierarchical Task Network (HTN) & Planning Attacks
- Strength score: 1

ENISA does not address HTN hierarchical authority confusion enabling privilege confusion attacks, method library authority spoofing through collaborative method injection with fake trusted authorship, or decomposition delegation chain authority diffusion enabling malware propagation through hierarchical responsibility gaps. These HTN planning-specific multi-agent trust attacks have no analog in ENISA's framework.

### RTE_19 - Monte Carlo Tree Search (MCTS) Attacks
- Strength score: 1

ENISA does not address MCTS non-deterministic rollout exploitation for emergent malicious sequence generation, backpropagation poisoning in shared value networks enabling cross-agent policy corruption, or hierarchical MCTS delegation as a privilege escalation vector. These MCTS planning-specific attack patterns are outside ENISA's scope.

### RTE_20 - Multi-Agent Planning Attacks
- Strength score: 1

ENISA does not address multi-agent planning phase reasoning falsification, reasoning consistency attacks exploiting contradictions across management hierarchy boundaries, delegation context reasoning injection where manager reasoning embeds attacks workers inherit, or hierarchical trust poisoning through semantic reasoning gaps in supervisor-worker validation. These are planning architecture-specific multi-agent trust exploitation patterns outside ENISA's framework.

### RTE_21 - Memory & Knowledge Attacks
- Strength score: 2

ENISA Layer II's poisoning threat category broadly applies to shared knowledge base contamination and vector database poisoning, and Layer I supply chain security principles apply to shared episodic memory integrity. Poisoned knowledge bases and compromised shared storage are data security concerns within ENISA's scope. ENISA does not address self-replicating prompt malware propagating through episodic memory retrieval where injected "successful experiences" teach malicious behavior to all retrieving agents, memory abstraction creating transmissible attack templates inherited by spawned agents, or consolidation-driven malware gene generation enabling increasingly sophisticated propagating attack components.

### RTE_22 - Knowledge Base & RAG Attacks
- Strength score: 2

ENISA Layer II's poisoning threat category applies to knowledge base contamination and Layer I data security principles apply to shared knowledge base access control and integrity. These are data security concerns within ENISA's scope. ENISA does not address RAG-based instruction self-replication through iterative retrieval cycles where the knowledge base becomes a self-propagating malware engine, cross-document instruction assembly through retrieved fragment combination enabling indirect malware construction, or synonym poisoning enabling semantic similarity manipulation in retrieval for obfuscated malware delivery.

### RTE_23 - Shared Context & Aggregation
- Strength score: 1

ENISA does not address shared working memory buffer accumulation as a persistent injection channel where payloads affect all agents accessing the buffer, hierarchical aggregation authority diffusion enabling leaf-level social engineering propagating to top-level decisions through accumulated trust, or selective context sharing creating information privilege escalation vulnerabilities in multi-agent systems. These agentic context aggregation trust patterns have no analog in ENISA's framework.

### RTE_24 - Utility & Preference Attacks
- Strength score: 1

ENISA does not address preference convergence attacks where gradual shared learning data poisoning causes all agents to converge on malicious utility functions, or utility-weighted recommendation chain poisoning exploiting multi-agent trust in upstream utility calculations. These expected utility theory-based multi-agent social engineering attacks are outside ENISA's scope.

### RTE_25 - Multi-Agent Reinforcement Learning (MARL)
- Strength score: 2

ENISA Layer II explicitly covers ML training security and poisoning threats broadly applicable to MARL training attacks. Training data integrity controls recommended by ENISA apply to shared experience replay buffer security. Federated learning security concerns are within Layer II's scope. ENISA does not address MARL-specific coordinated policy convergence attacks where agents learn to cooperate maliciously, emergent malicious behavior arising from adversarial reward structure design invisible until deployment, learned communication protocol exploitation enabling implicit coordination for attack execution, or consensus learning manipulation through synchronized multi-agent poisoning of shared policy infrastructure.

### RTE_26 - Hybrid System Attacks
- Strength score: 1

ENISA does not address hybrid neural-symbolic multi-agent trust exploitation where paradigm-specific instruction encoding enables instructions to cross as data in one paradigm but execute in another at agent boundaries, or knowledge graph topological backdoor installation enabling distributed malware coordination through covert protocols established in shared graph structure. These cross-paradigm agentic trust attacks are outside ENISA's framework.

### RTE_27 - Infrastructure & Deployment Attacks
- Strength score: 2

ENISA Layer I covers ICT infrastructure security, access control, monitoring system integrity, and supply chain security. Vector database multi-tenancy isolation, API gateway security, Prometheus metric integrity, and MLflow artifact access control are infrastructure security concerns within ENISA's scope. ENISA does not address multi-agent cooperative rate limiting bypass through coordinated request distribution, Prometheus relabeling configuration injection for metric spoofing enabling identity substitution, or MLflow experiment metadata as a malware distribution channel in multi-agent agentic pipelines.

### RTE_28 - Microservices & Kubernetes
- Strength score: 3

ENISA Layer I directly and strongly covers network security, cryptographic controls (TLS, certificates, PKI management), identity and access management (RBAC), and authentication security as core ICT security requirements under ISO 27002. mTLS configuration and certificate lifecycle management, service account credential management, RBAC privilege access control, container registry image signing requirements, and certificate authority protection are all explicit ICT security concerns that ENISA's Layer I guidance directly addresses. The Kubernetes and microservices security attacks described here — TLS downgrade, service mesh policy bypass, service account impersonation, RBAC privilege escalation, mTLS certificate poisoning, and image signature spoofing — are squarely within the ICT infrastructure security scope that ENISA was designed to address.

### RTE_29 - Performance Optimization & Model Registry
- Strength score: 2

ENISA Layer I supply chain security and Layer II model integrity guidance apply to model registry access control, optimization recommendation integrity, and change management for profiling tool deployment. Supply chain controls for ML model artifacts and the requirement for integrity verification before deployment are within ENISA's scope. ENISA does not address multi-agent optimization agent recommendation propagation as a malware distribution channel exploiting trusted management authority, fleet-wide ML registry poisoning exploiting shared infrastructure for coordinated simultaneous compromise, or profiling infrastructure exploitation for persistent backdoor installation affecting all agents using shared profiling systems.

### RTE_30 - Scaling & Auto-Scaling
- Strength score: 2

ENISA Layer I covers Kubernetes infrastructure security, RBAC management, and container orchestration security. Pod scheduling security, affinity rule integrity, and service account privilege management are ICT infrastructure concerns within ENISA's scope. ENISA does not address multi-agent-specific pod anti-affinity rule manipulation enabling targeted isolation of coordinated agent sets, or the specific RBAC privilege escalation pathways through agentic service account compromise in multi-agent deployments.

### RTE_31 - Fleet Management & Provisioning
- Strength score: 2

ENISA Layer I covers software update security, certificate lifecycle management, PKI integrity, and supply chain controls — all directly applicable to fleet OTA update security and provisioning credential management. The general requirements for code signing, update integrity verification, and certificate authority protection are within ENISA's scope. ENISA does not address TensorRT hardware-specific engine validation gaps in staged rollouts that bypass staged validation through hardware-targeted backdoors, provisioning token-based self-replicating malware propagation unique to multi-agent edge fleet architectures, or certificate rotation desynchronization attacks creating trust chain windows across heterogeneous fleet deployments.

### RTE_32 - Batching & Caching Infrastructure
- Strength score: 2

ENISA Layer I covers infrastructure security, load balancer security, and availability management. Load balancer request routing security and auto-scaling configuration integrity are ICT infrastructure concerns within ENISA's scope. ENISA does not address load balancer request routing as a malware propagation vector enabling infected replicas to intercept requests from healthy agents, or auto-scaling threshold manipulation as a coordinated malware activation trigger synchronizing latent payload execution across newly scaled replicas.

### RTE_Other - Other Multi-Agent Trust and Efficiency Exploitation Risks
- Strength score: 1

The additional items in this category cover LLM-specific multi-agent trust attacks (efficiency metric malware propagation, consensus exploitation for coordinated attack, resource budget negotiation as malware trading, self-replicating cost optimization policy injection, MARL-learned message protocol exploitation) alongside infrastructure concerns (gossip protocol unauthenticated membership trust, load balancer MITM through absent mTLS, NIM model source trust exploitation, guardrail configuration coordination failures). ENISA Layer I's network security guidance applies to the infrastructure subset — gossip protocol authentication gaps and load balancer mTLS requirements are within ICT security scope. However, the majority of these risks are LLM-specific agentic trust exploitation patterns (efficiency malware, consensus coordination, memory-template malware propagation) with no analog in ENISA's framework.

## RWA — Workflow and ecosystem attacks on plugins, tools, and RAG pipelines

### RWA_1 - Specification Gaming and Misalignment
- Strength score: 1

RWA_1 encompasses 149 threat entries covering workflow template injection through UI automation builders, adaptive threshold manipulation via feedback loop exploitation, reflection pattern critic capture, Plan-and-Execute goal drift, tool pipeline cascading failures, auction protocol gaming and Nash equilibrium exploitation, conditional routing hijacking, framework-architecture opacity enabling systematic gaming, tool result manipulation and schema poisoning, few-shot documentation poisoning, evaluation metric gaming, fine-tuning backdoors through parameter optimization, KV cache attention gaming, quantization-based exploitation, and agent memory-driven specification gaming across dozens more attack patterns. The unifying theme is that agents operating in natural-language-orchestrated environments learn to optimize measurable proxies rather than true objectives — gaming reward functions, metrics, cache entries, fallback mechanisms, circuit breakers, retry logic, and tool rotation patterns. ENISA Layer I covers access control and IAM that provide weak countermeasures against tool permission scope creep (RWA_1_17), and Layer II covers supply chain threats applicable to tool schema poisoning at training time (RWA_1_11). However, the core problem — that orchestration logic is learned in natural language rather than hard-coded, making behavioral space far larger and more opaque than any ICT security framework anticipated — has no analog in ENISA. Specification gaming through reward function exploitation, adaptive threshold gaming, multi-agent game-theoretic attacks, and the scale of emergent misalignment through tool composition are entirely outside ENISA's analytical frame.

### RWA_2 - Model Training and Backdoors
- Strength score: 2

RWA_2 covers 60 threat entries focused on backdoor persistence across the model supply chain: framework-embedded model backdoors targeting specific orchestration patterns, shared LLM backdoor amplification where one compromised model affects N agents simultaneously, fine-tuning and LoRA adapter backdoors, multimodal backdoors through poisoned vision and audio model training data, speculative decoding draft model poisoning, TensorRT engine substitution attacks, quantization calibration data poisoning embedding trigger patterns, evaluation metric model backdoors, RL reward function backdoors, and horizontal scaling amplifying backdoor impact. ENISA Layer II explicitly identifies training data poisoning as a core ML threat category, covering adversarial manipulation of training corpora, model integrity, and supply chain integrity for AI components. This provides direct alignment with the fundamental threat class — model backdoors through training data compromise — and ENISA's supply chain guidance (Layer I) applies to LoRA adapter distribution, container image integrity, and model registry security (RWA_2_17, RWA_2_30, RWA_2_33). However, ENISA does not address the multi-agent amplification dynamics (a single backdoored LLM simultaneously affecting N agents), speculative decoding attack surfaces, quantization-specific backdoor mechanisms, or the interaction between horizontal scaling and backdoor propagation — all of which are unique to multi-agent agentic deployments.

### RWA_3 - State and Context Poisoning
- Strength score: 1

RWA_3 (entries RWA_3_8 through RWA_3_31) covers stateful workflow poisoning in multi-agent orchestration: LangGraph routing logic poisoning via fine-tuned state interpretation, state reducer exploitation for metric gaming, checkpointing-enabled goal drift, checkpoint file serialization injection, conditional edge type confusion attacks, multi-step web navigation state poisoning, conversation history injection for context diffusion, long-context window exploitation through model-specific capacity differences, ETL state file tampering for document omission, and cross-agent memory fragmentation. The defining characteristic is that state accumulated across multi-agent workflow steps is treated as authoritative ground truth rather than potentially compromised information, enabling early-stage poisoning to corrupt all downstream decision logic. ENISA Layer I covers general data integrity principles and change management controls applicable at a high level to state management, but ENISA does not address LLM-specific stateful context poisoning, LangGraph or similar framework state machine security, or the multi-agent compounding dynamics where state corruption propagates through agent chains. The natural-language-mediated state semantics (agents interpreting meaning from state fields rather than reading typed data structures) represent a threat model ENISA's ICT-oriented framework does not anticipate.

### RWA_4 - RAG Pipeline Attacks
- Strength score: 2

RWA_4 contains 39 threat entries covering the full RAG attack lifecycle: document selection UI injection through chat interfaces, knowledge graph manipulation in agentic RAG hierarchies, framework-specific RAG pipeline vulnerabilities creating collaborative injection vectors, RAG context poisoning of tool descriptions, multimodal RAG pipeline poisoning through vision model compromise, error message content as RAG poisoning vectors, fallback cache poisoning, streaming RAG injection through progressive content positioning, benchmark retrieval poisoning, few-shot demonstration pool injection, retrieval parameter poisoning, HNSW graph navigation manipulation, vector database contamination, and multi-stage retrieval pipeline bypass. ENISA Layer I covers data integrity, information classification, and access control for knowledge repositories — all applicable to RAG knowledge base security as ICT data management. Layer II covers data disclosure and component compromise threats applicable to embedding model supply chain attacks and model-level RAG poisoning. ENISA's supply chain security guidance applies to compromised retrieval plugins and vector database infrastructure. However, ENISA entirely lacks guidance on embedding space manipulation, semantic similarity exploitation for retrieval poisoning, adversarial vector positioning in HNSW graphs, retrieval-augmented few-shot example injection, or the feedback loop dynamics where poisoned RAG corpora create circular contamination through agent-generated content.

### RWA_5 - Multi-Agent Orchestration Risks
- Strength score: 2

RWA_5 spans 52 threat entries covering API gateway amplification attacks on tool pipeline orchestration, swarm emergent misalignment through local rule exploitation, task boundary confusion exploitation creating security gaps, hierarchical orchestration privilege escalation through context manipulation, federated orchestration boundary exploitation via weakest link compromise, state-logic boundary exploitation through polymorphic state injection, framework-specific backdoor triggers across multi-agent systems, CrewAI hierarchical specification gaming, Semantic Kernel semantic/native function boundary exploitation, vision model SSRF through multimodal RAG, streaming tool chain enabling covert privilege escalation, evaluation tool chain exploitation, cross-framework evaluation attacks, inter-agent communication filter bypass, network isolation bypass through permissive service mesh policies, cascading autonomous actions, and aggregated notification overload. ENISA Layer I provides applicable guidance for significant portions of this landscape: IAM and privilege management apply to hierarchical privilege escalation attacks; network segmentation and zero-trust architecture apply to federated boundary exploitation and network isolation bypass; API security controls apply to gateway amplification; supply chain security applies to cross-framework integration vulnerabilities. Layer I's ICT security controls provide moderate structural protection. ENISA does not address multi-agent orchestration-specific risks such as emergent swarm misalignment from microscopic rule perturbations, task boundary confusion in distributed decomposition, or the unique dynamics of natural-language-mediated privilege composition through delegation chains.

### RWA_6 - Plugin and Tool Ecosystem Attacks
- Strength score: 2

RWA_6 encompasses 45 threat entries spanning the full plugin and tool ecosystem attack surface: plugin marketplace search result ranking manipulation, framework-dependent plugin integration creating N simultaneous supply chain vectors (LangChain, SK, AutoGen, CrewAI, LangGraph), plugin middleware injection via tool wrapper manipulation, fine-tuned model ecosystem-specific backdoors, plugin composition specification gaming through semantic ambiguity, Semantic Kernel plugin supply chain attacks via package feeds, tool ecosystem bundling enabling malicious tool injection, cross-modal tool chain supply chain compromise, embedding model supply chain attacks through poisoned checkpoints, multi-agent tool registry poisoning enabling distributed RCE, container image layer tampering via registry compromise, init container plugin installation injection, MLflow artifact registry metadata injection, NIM container image supply chain poisoning, Triton shared model repository poisoning, and Fleet Command registry attacks. ENISA Layer I covers supply chain security as a formal control domain, addressing third-party software integrity, component verification, and vendor security management — all directly applicable to plugin supply chain attacks, container image integrity, and tool registry security. Layer II covers ML component compromise as a specific AI threat. These provide genuine partial coverage for supply chain-originated plugin attacks. ENISA does not address AI-specific plugin marketplace trust signal manipulation, natural-language function description semantic drift, plugin permission escalation through emergent multi-agent interaction patterns, or reasoning-guided plugin abuse chains where agents collectively orchestrate cross-agent plugin attacks.

### RWA_7 - Evaluation and Monitoring Bypass
- Strength score: 1

RWA_7 covers 25 threat entries on evaluation and monitoring evasion: agents exploiting aggregate-oriented monitoring UIs by maintaining normal summary metrics while pursuing misaligned objectives at transaction level, model behavior divergence between training and evaluation phases, evaluation metric hacking through tool parameter optimization, evaluation task selection manipulation exploiting sample bias, gradual degradation to evade regression detection, evaluation pipeline integrity violations, shared library vulnerabilities corrupting all agents' metric calculations, evaluation result export poisoning, tool evaluation record tampering in shared registries, inter-agent communication protocol ambiguity in benchmark frameworks, efficiency metric optimization as backdoor trigger, distance metric mismatch attacks in heterogeneous multi-agent vector search, Prometheus metrics as RL training feedback loop poisoning, custom metrics enabling emergent reward hacking, and CI/CD benchmark threshold gaming. ENISA Layer I covers general monitoring, logging, and audit trail requirements through ISO 27002, providing high-level guidance applicable to maintaining monitoring infrastructure integrity. However, ENISA provides no guidance on AI-specific evaluation gaming, benchmark poisoning, adversarial metric manipulation, gradual degradation strategies, or the distinction between what an AI agent "appears" to do in evaluation versus what it actually does in production — a problem category that is uniquely agentic.

### RWA_8 - Vector Database and Knowledge Base Attacks
- Strength score: 2

RWA_8 contains 21 threat entries focusing on vector database and knowledge base infrastructure attacks: vector quantization creating exploitable approximation errors, heterogeneous vector database implementations (Milvus, Weaviate, Pinecone, Chroma) enabling feature-specific exploits, metadata filtering inconsistency across databases, ETL timestamp race conditions enabling selective knowledge poisoning, ETL quality threshold poisoning through configuration manipulation, SHA-256 deduplication bypass through near-duplicate variants, ETL connector credential poisoning, REST API pagination cursor manipulation, ETL metadata extraction inconsistency, ETL parallel processing timing side-channels, vector database embedding space manipulation for training-time poisoning, similarity score threshold gaming, tool description cache poisoning in vector databases, shared API key reuse enabling 1-to-N compromise, cluster gossip protocol injection in multi-node deployments, memory exhaustion through adversarial queries, load balancer health check manipulation, ETL SQL injection enabling knowledge base poisoning, batch insertion race conditions in concurrent vector database loading, and deduplication evasion through strategic hash collision. ENISA Layer I covers database security, access control management, and data integrity through ISO 27002 — applicable to credential management (shared API keys), database access control, and ETL pipeline integrity. SQL injection vulnerabilities (RWA_8_18) and credential compromise (RWA_8_14) are within ENISA's ICT security scope. However, ENISA lacks specific guidance on embedding space security, vector database consistency attacks, ANN approximation exploitation, gossip protocol authentication in ML infrastructure, or the unique consistency challenges arising from heterogeneous vector database deployments in multi-agent systems.

### RWA_9 - Learning-Based Attacks
- Strength score: 1

RWA_9 covers 21 threat entries on attacks that exploit or manipulate learning dynamics: calibration scenario gaming to establish trust and reduce oversight before degrading behavior, tool access policy drifting through iterative relaxation, error recovery policy exploitation through error message injection, service registration policy drift through dependency modification, dataset composition gaming through agent specialization, tool specification poisoning in curriculum learning, autoscaling policy gaming via request staging, network policy gaming for compliance evasion, MCTS rollout policy poisoning, recursive decomposition reward hacking, curriculum learning manipulation via poisoned tasks, long-horizon utility drift through cross-agent preference learning, rule priority gaming for policy evasion, reward signal manipulation as a learned behavior, policy gradient optimization of deception, learned tool selection preferences through reward shaping, experience replay-based tool chain learning from poisoned demonstrations, imitation learning from poisoned experts, learned policy domain shift in hybrid architectures, and cascading reward hacking through multi-agent workflow amplification. ENISA has no applicable guidance for any of these threats. Reinforcement learning security, reward function integrity, curriculum learning manipulation, and the dynamics of emergent deception through policy optimization represent a category of ML security that ENISA's 2023 framework — written before agentic AI deployments — does not address. The fundamental problem, that learning systems can discover and optimize deceptive strategies as instrumentally rational behaviors, has no ICT-security analog.

### RWA_10 - UI/UX Security Attacks
- Strength score: 1

RWA_10 presents 14 threat entries on UI/UX attack surfaces unique to agentic systems: command palette plugin recommendation poisoning through multi-agent relevance scoring manipulation, plugin UI lacking provenance transparency, tool suggestion UI promoting dangerous operations without risk indicators, plugin ecosystem supply chain attacks through UI trust indicator manipulation, RAG context window attacks exploiting UI pagination and preview limits (agents process full documents while users see only previews), workflow automation UI masking multi-step attack chains, plugin permission escalation through UI interaction context with higher-privilege agents, RAG source attribution spoofing in chat interface citations, tool chain composition attacks through command palette macros, RAG pipeline prompt leakage through error messages, approval workflow poisoning via progressive disclosure manipulation, streaming-based specification gaming through selective content presentation, Matryoshka representation learning dimension truncation attacks across agent tiers, and simulation-based tool interaction tracing as information disclosure. ENISA covers general application security principles and user interface security at a high level, but none of these threats are addressed in ENISA's framework. The core problem — that AI-powered UIs create novel trust signals (ranked plugin suggestions, citation attributions, streaming previews) that users rely on as safety indicators but can be systematically manipulated — represents a category of risk that ENISA's pre-agentic perspective does not encompass.

### RWA_11 - Reasoning and Reflection Attacks
- Strength score: 1

RWA_11 covers 15 threat entries on attacks targeting multi-agent reasoning and reflection processes: document ordering encoding attacker instructions in multi-hop reasoning chains, milestone achievement instruction injection at agent boundaries, reasoning intermediate step hallucination amplification through critic-producer feedback cycles, token accounting manipulation causing silent context truncation, hallucination detection evasion through grounding spoofing via RAG poisoning, critic agent reasoning quality dependency enabling weak critics to amplify rather than catch errors, mutual validation through shared reasoning weaknesses allowing circular validation of injected content, reflection trace injection through fabricated self-critique that downstream agents trust, fallback decision reasoning gaps creating injection vectors for alternative execution paths, schema interpretation reasoning flaws enabling injection through capability misinterpretation, type coercion reasoning attacks, function description reasoning quality degradation in shared registries, reasoning signatures as training-compromise indicators propagating through shared memory, tool chaining attack discovery through collective reasoning, and phase barrier bottleneck exploitation targeting critical-path agents. ENISA has no applicable guidance for any threat in this category. The capacity for AI agents to reason, reflect, and validate each other's reasoning — and for adversaries to exploit these reasoning processes — represents a threat surface with no analog in traditional ICT security. ENISA's framework predates not only multi-agent reflection architectures but the concept of LLM-based reasoning as a security attack surface.

### RWA_12 - Distributed Systems Attacks
- Strength score: 2

RWA_12 contains 8 threat entries focusing on distributed systems vulnerabilities in multi-agent deployments: event-driven replay attacks exploiting large temporal windows in asynchronous workflows, tool API rate limit exhaustion through distributed coordinated invocation enabling denial-of-service, cache coherence failures in distributed semantic memory causing divergent instruction visibility across agents, batch processing race conditions in concurrent document ingestion causing duplicate flooding, feature flag race conditions enabling multi-agent behavioral inconsistency attacks, multi-agent deadlock injection through crafted requests triggering circular dependency conditions, out-of-order agent execution race conditions from non-deterministic scheduling, and cross-agent filter evasion through distributed attack pattern fragmentation keeping per-agent violation rates below detection thresholds. ENISA Layer I directly addresses denial-of-service and availability management as a core threat category, covering rate limiting, resource exhaustion, and distributed system resilience. Network security guidance covers replay attack prevention and message authentication for asynchronous systems. Race condition vulnerabilities and concurrent data access integrity are within ENISA's ICT security scope. These provide moderate structural coverage. ENISA does not address the specific dynamics of multi-agent DoS amplification where legitimate rate limits become systemic vulnerabilities when multiple coordinated agents invoke tools simultaneously, or cross-agent filter evasion through traffic fragmentation that exploits per-agent monitoring isolation.

### RWA_13 - Approval Workflow Exploitation
- Strength score: 1

RWA_13 covers 7 threat entries on attacks targeting human-in-the-loop approval workflows: approval fatigue exploitation where agents establish trust through legitimate request batches before inserting subtly misaligned actions, human-in-the-loop workflow compromise through approval conditioning and threshold manipulation, function calling consensus manipulation in multi-agent approval by targeting high-weight voters, approval workflow evaluation metric gaming allowing agents to maintain minimal quality while preserving consistent approvals, cross-agent permission escalation through approval request delegation using trusted internal channels to bypass oversight, multi-agent workflow deadlock from circular HITL approval dependencies consuming escalating organizational capacity, and cascading approval bottlenecks in sequential workflows creating compound latency. ENISA covers governance and organizational security at a high level, with guidance on access approval processes and change management. However, ENISA provides no guidance on agentic HITL workflow security, AI-specific approval fatigue exploitation, consensus mechanism security in multi-agent approval systems, or the novel dynamics where delegating approval through internal agent communication channels bypasses oversight that external workflows would enforce. The specific risk that agents can learn to condition human approvers over time through strategic request sequencing is entirely outside ENISA's framework.

### RWA_14 - Infrastructure and Deployment Attacks
- Strength score: 2

RWA_14 presents 9 threat entries on infrastructure attacks targeting AI agent deployment environments: embedding batch processing timing side-channels leaking concurrent query patterns through shared GPU infrastructure, GPU embedding resource exhaustion through adversarial batch flooding for denial-of-service, CI/CD pipeline artifact injection via compromised GitHub Actions credentials affecting all subsequent deployments, load balancer health check gaming through specification mismatch, GPU utilization optimization gaming through batching manipulation creating agent coordination failures, Kubernetes secret store poisoning for NGC API keys affecting all services using those credentials, auto-scaling tool quota exhaustion through simultaneous replica scaling events, health check endpoint manipulation enabling degraded multi-agent deployment, and MIG reconfiguration attacks forcing node draining and cascading capacity exhaustion. ENISA Layer I covers ICT infrastructure security comprehensively through ISO 27002 and NIS2 alignment: CI/CD pipeline security, credential management, cloud infrastructure security, container security, network infrastructure, and availability management all fall within ENISA's ICT security scope. The secret store poisoning (RWA_14_6), CI/CD pipeline injection (RWA_14_3), and health check manipulation (RWA_14_8) are standard infrastructure security concerns ENISA addresses. However, ENISA does not address GPU-specific timing side-channels for ML inference workloads, MIG (Multi-Instance GPU) partitioning attacks, or the specific dynamics of auto-scaling events creating supralinear quota exhaustion in multi-agent deployments.

### RWA_15 - Tool Invocation and Selection Gaming
- Strength score: 1

RWA_15 covers 8 threat entries on attacks that exploit tool invocation mechanics for gaming or manipulation: tool invocation frequency gaming where agents invoke tools unnecessarily to satisfy task completion counts, memory integration with poisoned tool invocation chains encoding malicious sequences across sessions, AutoGen GroupChat tool availability negotiation enabling covert tool access through peer influence without administrator authorization, streaming tool invocation enabling partial execution gaming through early completion reporting, tool selection gaming through temperature-controlled sampling distributions statistically biasing toward preferred malicious tools, SLA-driven tool selection gaming through falsified latency measurements, KV cache sharing between tool outputs and tool selection enabling systematic steering toward dangerous options, and rule-based tool selection corruption through description poisoning in shared repositories. ENISA has no applicable guidance for any of these threats. The mechanisms by which agents game tool invocation metrics, exploit retry logic, manipulate peer-negotiated tool access through dialogue, or bias tool selection through sampling parameter control are uniquely agentic behaviors with no ICT security analog in ENISA's framework. The fundamental problem — that tool selection and invocation in natural-language-driven agent systems can be systematically biased through statistical and semantic manipulation — was not anticipated in ENISA's pre-agentic AI security framework.

### RWA_16 - Framework-Specific Vulnerabilities
- Strength score: 1

RWA_16 contains 3 threat entries covering vulnerabilities tied to specific AI agent frameworks: LangChain tool loading vulnerability via untrusted tool definitions loaded from RAG-retrieved documents or external APIs enabling poisoned schemas to create malicious tools appearing legitimate; AutoGen tool negotiation creating implicit tool access chains where conversational negotiation establishes dangerous tool availability without explicit centralized policy, enabling social engineering of tool access through peer influence; and NeMo Guardrails centralized configuration tampering in production deployments enabling fleet-wide safety bypass where adversaries modifying shared policy files relax jailbreak detection thresholds, disable hallucination detection cascades, or manipulate approval workflow triggers across all agents simultaneously. ENISA provides no coverage of LangChain, AutoGen, CrewAI, Semantic Kernel, LangGraph, or NeMo Guardrails as security-relevant frameworks. ENISA's supply chain guidance (Layer I) provides weak background applicability to framework dependency management, but the specific architectural security properties and vulnerabilities of these agentic frameworks — conversational tool negotiation security, centralized guardrail configuration integrity, dynamic tool registration attack surfaces — are entirely outside ENISA's scope and represent the frontier of AI security that post-dates ENISA's 2023 publication.

