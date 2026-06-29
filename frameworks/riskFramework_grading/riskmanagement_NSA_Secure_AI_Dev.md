# Risk Management Analysis: NSA_Secure_AI_Dev Framework

## Framework Overview

The NSA_Secure_AI_Dev folder contains two documents:

1. **Guidelines for Secure AI System Development** (NCSC/CISA/NSA/Five Eyes, 2023): A lifecycle-oriented security guide co-authored by 21 international cyber agencies. It covers four phases of AI system development: Secure Design, Secure Development, Secure Deployment, and Secure Operation & Maintenance. Key controls include threat modeling, supply chain security (SLSA, SBOM, verified vendors), input sanitization, least privilege, guardrails, infrastructure access controls, cryptographic model integrity (hashes/signatures), behavior and input monitoring, adversarial input detection, incident management, and responsible release (benchmarking, red teaming).

2. **JCDC AI Cybersecurity Collaboration Playbook** (CISA/JCDC, January 2025): An information-sharing playbook for AI cybersecurity incidents and vulnerabilities. It provides checklists for voluntary incident reporting to CISA/JCDC, covers vulnerability disclosure coordination, and facilitates proactive threat intelligence sharing among government, industry, and international partners.

Together these documents constitute a principled, lifecycle-oriented framework with explicit awareness of adversarial ML (prompt injection, data poisoning, model extraction) but without prescriptive technical controls for agentic or multi-agent architectures specifically.

---

## RATC - Agent–tool coupling as "policy-level remote code execution"

### RATC_1 - UI/UX Abstraction and Visibility Gaps
- Strength score: 1

The NSA/NCSC Guidelines acknowledge that AI systems must provide users with usable outputs while minimizing unnecessary information exposure, and encourage secure by default configurations. However, the framework provides no specific guidance on the UI/UX abstraction and visibility gap threats catalogued in RATC_1. Threats such as approval UIs masking true risk levels behind human-readable abstractions, progressive disclosure hiding tool chain complexity, inline suggestions executing tools invisibly, multi-agent dashboards failing to differentiate tool risk levels, context-driven tool selection without user awareness, and trace visualization abstraction concealing execution complexity are UX design and multi-agent dashboard engineering concerns that the framework does not address. Its general advice to explain riskier capabilities and require opt-in is directionally appropriate but too abstract to mitigate these threats.

### RATC_2 - Approval Workflow and User Attention Vulnerabilities
- Strength score: 1

The NSA/NCSC Guidelines advise applying appropriate restrictions to AI component actions and implementing least privilege, which conceptually supports well-designed approval workflows. However, the framework provides no guidance on approval fatigue exploitation, keyboard shortcut hijacking in approval UIs, time-based default exploits where timeout-triggered auto-approvals bypass review, or how multi-agent architectures amplify these vulnerabilities by generating exponentially more approval requests than single agents. These are human-computer interaction security and agentic workflow design concerns entirely outside the scope of this lifecycle guidance framework.

### RATC_3 - Tool Overload, Parameter Handling, and Selection Errors
- Strength score: 2

The NSA/NCSC Guidelines explicitly state that "appropriate checks and sanitisation of data and inputs" must be applied, which directly addresses tool argument injection (RATC_3_2) where untrusted data injected into tool parameters can achieve remote code execution across agent trust boundaries. The framework also advises applying appropriate restrictions to possible actions and least privilege principles, which are relevant to tool overload privilege escalation. The guideline on applying appropriate controls to external API inputs further supports parameter validation. However, the framework does not address tool count degradation in multi-agent delegation chains, cross-framework parameter extraction errors, or the specific risk amplification from separating argument construction from execution in multi-agent systems.

### RATC_4 - Confidence Manipulation and Tool Authorization
- Strength score: 1

The NSA/NCSC Guidelines acknowledge adversarial machine learning broadly but provide no specific guidance on confidence threshold manipulation attacks where inflated confidence scores bypass approval gates, or on tool authorization scope confusion where agents exploit intermediary roles for privilege escalation. The framework's guidance on least privilege and threat modeling is relevant in principle but does not translate to countermeasures for confidence-based gating attacks or transitive trust exploitation in multi-agent authorization chains.

### RATC_6 - Tool Metadata Poisoning Across Registries and Discovery
- Strength score: 2

The NSA/NCSC Guidelines' "Secure your supply chain" section is directly relevant: it requires assessing and monitoring supply chain security, acquiring components from verified sources, and using frameworks like SLSA for supply chain attestation. The "Identify, track and protect your assets" section requires tracking and authenticating AI assets including models and software. The guidance on computing and sharing cryptographic hashes and signatures of model files applies to tool registry integrity. These supply chain controls are meaningfully applicable to centralized tool registry poisoning (RATC_6_1) and framework-specific tool selection poisoning through compromised registries, though the framework does not specifically address vector database metadata poisoning or few-shot parameter injection in tool descriptions.

### RATC_8 - Advanced Tool Invocation Patterns and Inference
- Strength score: 1

The NSA/NCSC Guidelines' monitoring guidance — "monitor your system's inputs...to enable...investigation and remediation in the case of compromise" — is tangentially relevant to detecting tool result injection (RATC_8_2) where poisoned observations propagate through multi-agent chains. However, the framework provides no specific guidance on multi-agent replanning loop exploitation, observation manipulation in ReAct patterns, cross-framework tool schema translation boundary attacks, or the specific risk of downstream agents trusting upstream tool outputs without re-validation. These agentic architecture concerns are beyond the scope of this general secure development framework.

### RATC_9 - Web and Multimodal Tool Exploitation
- Strength score: 2

The NSA/NCSC Guidelines explicitly name prompt injection as a known adversarial ML threat and mandate "explicit detection of out-of-distribution and/or adversarial inputs" in the input monitoring section. The requirement to apply "appropriate checks and sanitisation of data and inputs" is directly applicable to web agent tool vulnerabilities (RATC_9_1) where attackers inject instructions into web content, and to multimodal retrieval poisoning (RATC_9_2) where poisoned content is retrieved and executed. The guidance on controlling what data AI systems can access and applying appropriate restrictions to actions reinforces containment. The framework does not, however, address multi-agent propagation of injected web instructions across pipelines, cross-modal parameter injection without sanitization boundaries, or vision model output laundering through multi-step processing chains.

### RATC_10 - Efficiency Optimization and Resource Constraints
- Strength score: 1

The NSA/NCSC Guidelines do not address token allocation, context window management, iteration budget tuning, or temperature parameter trade-offs in multi-agent systems. Attacks that force upstream agents into constrained token contexts causing truncated safety-critical tool descriptions, or that exploit iteration budget differentials across agent specializations, are agentic system optimization concerns entirely outside the scope of this lifecycle guidance framework.

### RATC_11 - Tool Execution Infrastructure and Orchestration
- Strength score: 2

The NSA/NCSC Guidelines' "Secure your infrastructure" section directly states: "apply good infrastructure security principles," "apply appropriate access controls to your APIs, models and data," and require "appropriate segregation of environments." These controls are directly applicable to API gateway routing policy attacks (RATC_11_1) where compromised routing redirects tool calls to attacker-controlled services, and to Kubernetes container orchestration attacks (RATC_11_2) where misconfigured security contexts enable fleet-wide privilege escalation. The framework's guidance on secure by default configurations and access controls for AI systems' action-triggering capabilities provides meaningful structural support, though it does not address multi-agent fleet amplification through poisoned init containers or sidecar proxy interception specifically.

### RATC_12 - Distributed and Hardware-Level Attacks
- Strength score: 1

The NSA/NCSC Guidelines' guidance on protecting model confidentiality and integrity (cryptographic hashes, access controls) is conceptually relevant but the framework provides no specific guidance on tensor parallelism communication interception, KV cache poisoning through multi-tenant GPU co-location, quantization calibration dataset poisoning for tool-call parameter corruption, or cross-agent activation corruption through shared GPU infrastructure. These are hardware-level and distributed computing security concerns requiring specialized GPU and infrastructure security controls that this general AI security framework does not address.

### RATC_14 - Reasoning and Planning Vulnerabilities
- Strength score: 1

The NSA/NCSC Guidelines' behavior monitoring guidance — "observe sudden and gradual changes in behaviour affecting security" — is directionally relevant to detecting reasoning quality issues, and the input monitoring guidance for adversarial input detection has indirect applicability. However, the framework provides no guidance on chain-of-thought intra-step correctness failures, poisoned reasoning traces embedding justifications for dangerous tool sequences, self-consistency majority voting exploitation through sampled path manipulation, or hierarchical task network tool visibility fragmentation. These AI reasoning architecture vulnerabilities require model-level security engineering beyond this framework's scope.

### RATC_15 - Episodic Memory and Learning
- Strength score: 2

The NSA/NCSC Guidelines explicitly state: "applying appropriate checks and sanitisation of data and inputs; this includes when incorporating user feedback or continuous learning data into your model, recognising that training data defines system behaviour." This directly applies to episodic memory poisoning (RATC_15_1) where attackers poison stored episodes recording malicious tool sequences as successful resolutions. The framework's recognition that training data manipulation (data poisoning) is a key adversarial ML threat is directly relevant to trajectory poisoning and procedural memory abstraction attacks. The framework does not, however, address multi-agent shared episodic memory propagation or trajectory abstraction mechanisms specifically.

### RATC_16 - Semantic Memory and RAG
- Strength score: 2

The NSA/NCSC Guidelines advise "appropriate checks and sanitisation of data and inputs" and state that "you have processes and controls in place to manage what data AI systems can access." These controls are applicable to RAG knowledge base poisoning (RATC_16_1) where attackers embed tool-invocation instructions in retrieved documents. The framework's supply chain security guidance extends to data provenance, and access control guidance applies to restricting what knowledge bases agents can query. The data poisoning threat category acknowledged by the framework covers knowledge base injection at a conceptual level, though the framework provides no specific guidance on knowledge graph relationship poisoning, query rewriting instruction injection, or RAG staleness attacks.

### RATC_17 - Utility Functions and Decision Logic
- Strength score: 1

The NSA/NCSC Guidelines do not address utility-weighted tool selection, expected utility calculation, outcome assumption injection, or multi-objective utility function weight manipulation. These decision-theoretic AI system vulnerabilities are outside the scope of this secure development lifecycle framework, which focuses on infrastructure security, data integrity, and monitoring rather than AI decision logic internals.

### RATC_18 - Rule-Based and Knowledge-Engineered Systems
- Strength score: 1

The NSA/NCSC Guidelines have no coverage of rule-based authorization system vulnerabilities. Threats such as specificity hierarchy exploitation enabling injected rules to override safety rules, forward chaining rule chain exploitation through fact injection, certainty factor manipulation, or lexicographic heuristic objective reordering are rule engine and expert system security concerns entirely outside the scope of this framework.

### RATC_19 - Learning and Reinforcement Learning
- Strength score: 2

The NSA/NCSC Guidelines' explicit acknowledgment that "training data defines system behaviour" and the requirement for sanitization of "user feedback or continuous learning data" apply directly to RL tool selection poisoning (RATC_19_1). Reward poisoning, curriculum learning poisoning, and imitation learning trajectory poisoning are all forms of training data manipulation that the framework identifies as a known adversarial ML threat requiring protective controls. The framework's supply chain security guidance is also relevant to shared replay buffer poisoning across multi-agent RL systems. However, the framework provides no specific technical countermeasures for actor-critic cross-agent poisoning, proximal policy optimization trust region manipulation, or multi-agent RL convergence attacks.

### RATC_21 - Parallel Retrieval Race Conditions in Multi-Agent Query Decomposition Systems
- Strength score: 1

The NSA/NCSC Guidelines' supply chain security guidance is tangentially applicable to container registry compromise mentioned in RATC_21, and the monitoring guidance relates broadly to detecting fleet-wide retrieval outages. However, the framework provides no guidance on parallel retrieval race conditions, shared index deduplication races, timeout-triggered cascade failures across connection pools, multi-stage cache temporal inconsistency, or network propagation delay-induced retrieval inconsistency across agent fleets. These are distributed systems engineering concerns requiring concurrent data access controls beyond this framework's scope.

### RATC_22 - Multi-Stage Pipeline Result Caching Race Enabling Multi-Agent Consistency Failures
- Strength score: 1

The NSA/NCSC Guidelines' supply chain security guidance covers registry credential compromise and component integrity, which is relevant to the container supply chain attack vectors mentioned in RATC_22. The guidance on cryptographic hashes for supply chain attestation applies. However, the framework has no coverage of TTL-based cache race conditions, event-based cache invalidation propagation delays, shared cache write races, hash collision attacks on cache entries, cache warming inconsistency, tensor parallelism communication poisoning through GPU memory isolation failures, or telemetry exposure of fleet-wide dependency maps enabling targeted DoS attacks.

### RATC_27 - MIG Instance Co-Location Creating Shared Physical GPU Failure Propagation
- Strength score: 1

The NSA/NCSC Guidelines have no coverage of Multi-Instance GPU (MIG) virtualization, GPU hardware isolation mechanisms, shared physical GPU failure propagation, power regulator or thermal management shared dependency attacks, GPU firmware vulnerability exploitation, or cross-partition memory access attacks. These are GPU hardware security engineering concerns entirely outside the scope of this AI software security lifecycle framework.

### RATC_29 - Shared Workflow State Version Conflict Amplification Through Concurrent Write Flooding
- Strength score: 1

The NSA/NCSC Guidelines have no coverage of optimistic locking mechanisms, version conflict amplification through concurrent write flooding, retry storm cascades, distributed state coordination for multi-agent workflows, or contention-based denial of service through systematic version conflict injection. These are distributed systems concurrency engineering concerns requiring database and workflow infrastructure controls beyond this framework's scope.

### RATC_30 - Guardrail Bypass Through Infrastructure Failure Injection Preventing Safety Validation Execution
- Strength score: 2

The NSA/NCSC Guidelines directly address guardrails: "if necessary, your system provides effective guardrails around model outputs." The secure infrastructure section requires good infrastructure security principles to protect all lifecycle stages, and the incident management guidance acknowledges the inevitability of security incidents and requires resilient response plans. The framework's emphasis on secure by default settings and protecting model integrity extends to maintaining guardrail availability. These controls are indirectly applicable to the infrastructure failure injection attacks that cause guardrail bypass through timeout exploitation (RATC_30), though the framework does not address adversary-induced processing delays, centralized validation endpoint DoS, or fallback behavior design for guardrail unavailability specifically.

---

## RDL - New data-leakage channels via large contexts, logs, and probabilistic recall

### RDL_1 - UI/UX Patterns
- Strength score: 1

The NSA/NCSC Guidelines advise that AI systems be designed to minimize unnecessary information exposure and encourage secure by default configurations. The framework's access control guidance is relevant to protecting conversation histories and audit trail storage. However, the framework provides no specific guidance on the UI/UX data leakage patterns catalogued in RDL_1: chat interface histories aggregating multi-agent sensitive data without adequate compartmentalization, progressive disclosure debug views exposing credentials and inter-agent authentication tokens, session persistence crossing security classification boundaries, command palette history as behavioral reconnaissance artifacts, multi-agent dashboard correlation leakage across organizational silos, reference link traversal exposing relational metadata, approval workflow audit trails as high-value forensic targets, evidence link aggregation bypassing role-based access controls, refinement iteration histories disclosing agent decision logic and quality thresholds, and ARIA accessibility attribute leakage. These are UX engineering and multi-agent dashboard design concerns entirely outside the scope of this lifecycle guidance framework.

### RDL_2 - Data Persistence and Caching
- Strength score: 1

The NSA/NCSC Guidelines' access control guidance and the requirement to "manage what data AI systems can access" apply conceptually to protecting session storage and cache systems. The principle of minimizing unnecessary information exposure is relevant to data minimization in persistence design. However, the framework provides no specific guidance on session serialization security for multi-agent systems where structured sessions become efficiently queryable exfiltration targets (RDL_2_1), cache isolation failures in composite multi-agent response caching that expose prior users' data (RDL_2_2), or distributed undo state coordination creating persistent recovery buffers containing data users believe has been deleted, conflicting with right-to-be-forgotten requirements (RDL_2_3). These are application data lifecycle management concerns the framework does not address.

### RDL_3 - Streaming and Token-Level Leakage
- Strength score: 1

The NSA/NCSC Guidelines' monitoring guidance requires observing system inputs and outputs for security, and the guidance to minimize unnecessary information exposure is directionally applicable to streaming design. However, the framework provides no guidance on token-by-token streaming leakage where sensitive content appears in UI buffers before post-processing redaction can apply (RDL_3_1), tokenization boundary inference attacks enabling context window reconstruction (RDL_3_3), streaming output length correlation for session activity reconstruction without content observation (RDL_3_4), intermediate state exposure during multi-agent streaming handoff cycles (RDL_3_5, RDL_3_6), or streaming response cache side channels enabling covert data exfiltration through cache query patterns (RDL_3_7). These are streaming architecture and real-time data pipeline security concerns outside the framework's scope.

### RDL_4 - Search and Recall
- Strength score: 1

The NSA/NCSC Guidelines' guidance to "manage what data AI systems can access" and implement access controls is conceptually applicable to enforcing security boundaries in search index access. However, the framework provides no specific guidance on probabilistic recall through semantic conversation search (RDL_4_1), where semantic similarity matching across unified multi-agent history indexes enables cross-domain data leakage — financial queries surfacing confidential HR or security content from other agents' domains — through the absence of access-controlled search boundaries. Systematic adversarial probing using crafted semantic queries to surface high-sensitivity content is an attack vector not addressed by this general lifecycle framework.

### RDL_5 - Attribution and Observability
- Strength score: 1

The NSA/NCSC Guidelines require monitoring and logging for security, and the access control guidance applies to protecting log storage from unauthorized access. However, the framework does not address the dual-use nature of multi-agent attribution logs as organizational intelligence sources (RDL_5_1) where log aggregation across agent ecosystems reveals hierarchy, high-privilege user patterns, and strategic priorities beyond what any individual log reveals. The framework similarly does not address framework-dependent logging creating comprehensive architectural disclosures through aggregated multi-framework telemetry (RDL_5_2), error classification patterns enabling infrastructure weakness mapping (RDL_5_3), or latency measurement side channels revealing computational load and service availability for attack timing optimization (RDL_5_4).

### RDL_7 - Tool Invocation and Function Calling
- Strength score: 1

The NSA/NCSC Guidelines require monitoring of system inputs including tool invocations, and the access control guidance applies to protecting audit logs. The input sanitization guidance has indirect relevance to RDL_7_8 where sensitive parameter values such as API keys, account numbers, and PII leak through centralized action logs visible across all agents. However, the framework provides no guidance on sanitizing tool invocation logs to prevent sensitive parameter exfiltration (RDL_7_1, RDL_7_8), function calling JSON persistence leaking sensitive parameters across agent context boundaries (RDL_7_2), tool error message containment preventing implementation detail disclosure (RDL_7_3), audit trail poisoning through compromised logging agents omitting malicious invocations (RDL_7_6), or tool success rate differential side channels for identifying high-performing agents as compromise targets (RDL_7_7). These are logging security and multi-agent audit system concerns the framework does not address.

### RDL_9 - Multi-Agent Memory and State
- Strength score: 1

The NSA/NCSC Guidelines' access control guidance and least privilege principles are conceptually applicable to protecting shared multi-agent memory stores, and the requirement to "manage what data AI systems can access" could extend to memory access isolation. However, the framework provides no specific guidance on cross-agent reflection memory leakage where shared infrastructure allows critic agents to create covert channels between agent domains (RDL_9_1), ReAct reasoning trace persistence creating attack graphs exposing complete agent topology and decision logic (RDL_9_2), cross-tenant timing analysis through distributed trace correlation IDs (RDL_9_3), blackboard shared memory access pattern reconstruction and false sharing timing attacks (RDL_9_4), or multi-agent dashboard simultaneous activity correlation revealing sensitive business workflow patterns (RDL_9_5). These multi-agent state isolation and distributed observability security concerns are outside the framework's scope.

### RDL_10 - Reasoning and CoT Traces
- Strength score: 2

The NSA/NCSC Guidelines explicitly state that AI systems should minimize unnecessary information exposure and require secure by default configurations, which are directly applicable to CoT trace verbosity policies and retention controls. The monitoring guidance requires reviewing outputs for security issues, which is relevant to detecting sensitive data embedded in reasoning chains (RDL_10_3 — reasoning-embedded secrets). The guideline to "apply appropriate checks and sanitisation of data and inputs" extends to sanitizing reasoning traces before storage or cross-agent transmission, providing meaningful indirect coverage for CoT trace leakage (RDL_10_1), intermediate step verbosity enabling data reconstruction (RDL_10_2), and cross-agent reasoning context flow (RDL_10_4). The framework does not, however, address Tree-of-Thought lookahead branch observation attacks (RDL_10_5), preserved path persistence as long-lived leakage vectors (RDL_10_6), consolidation leakage from merged multi-source reasoning traces (RDL_10_7), probabilistic recall of intermediate reasoning chains (RDL_10_8), quality score metadata side channels (RDL_10_9), or failure case logging propagating sensitive data as learning material to other agents (RDL_10_10).

### RDL_11 - Multimodal and Input Processing
- Strength score: 1

The NSA/NCSC Guidelines' input sanitization guidance and access control requirements for data AI systems can access have indirect applicability to controlling what multimodal content is processed and how intermediate representations are stored. However, the framework provides no specific guidance on multimodal context window expansion proportionally expanding leakage surfaces across text, image, audio, and table modalities (RDL_11_1), vision model intermediate representation leakage through model logs (RDL_11_2), audio embedding persistence in shared vector stores revealing speaker identity and emotional tone (RDL_11_3), chart linearization creating permanent machine-readable leakage vectors for sensitive numerical data (RDL_11_4), or embedding inversion attacks reconstructing sensitive visual content from stored embeddings in shared vector databases (RDL_11_5). These multimodal ML architecture security concerns require specialized controls not addressed by this framework.

### RDL_12 - Error Handling and Graceful Degradation
- Strength score: 1

The NSA/NCSC Guidelines' incident management guidance acknowledges security failures and requires response plans, and the monitoring guidance is relevant to detecting error conditions. The access control guidance applies to protecting error log storage. However, the framework provides no guidance on error message sanitization to prevent sensitive data appearing in error states from persisting in logs (RDL_12_1), retry attempt log security containing full context state including tool results and agent assessments (RDL_12_2), fallback chain logging security posture inconsistencies where secondary providers have weaker access controls (RDL_12_3), graceful degradation state logging disclosing system architecture through capability-disabled records (RDL_12_4), or circuit breaker state change logging enabling infrastructure topology reconnaissance by exposing fragile endpoints and failure patterns (RDL_12_5).

### RDL_13 - Evaluation and Testing Leakage
- Strength score: 1

The NSA/NCSC Guidelines' "identify, track and protect your assets" guidance and supply chain security controls could conceptually extend to protecting evaluation datasets and test artifacts. The framework mentions responsible release including benchmarking and red teaming, implying evaluation data exists and has value. Access controls apply to evaluation dashboard access. However, the framework provides no specific guidance on evaluation pipeline log sanitization (RDL_13_1), baseline comparison context window exposure of historical evaluation data (RDL_13_2), test dataset leakage through evaluation dashboard aggregation (RDL_13_3), distributed evaluation artifact storage security (RDL_13_4), metric fingerprinting enabling evaluation architecture reconstruction (RDL_13_6), temporal analysis of metric changes inferring model update patterns (RDL_13_7), cross-validation dataset structure exposure through fold result analysis (RDL_13_8), or benchmark performance leakage through approval log justifications exposing agent failure modes (RDL_13_9).

### RDL_14 - Feedback and Testing
- Strength score: 2

The NSA/NCSC Guidelines explicitly state: "applying appropriate checks and sanitisation of data and inputs; this includes when incorporating user feedback or continuous learning data into your model, recognising that training data defines system behaviour." This directly addresses the security of user feedback data and the logs that store it (RDL_14_1), where stored failure descriptions become agent vulnerability profiles accessible to attackers. The framework's recognition that feedback and continuous learning data require sanitization and access controls is meaningfully applicable to restricting who can query user feedback logs and what failure mode intelligence can be extracted. The access control guidance extends to A/B test result storage (RDL_14_2), though the framework does not specifically address A/B result analysis enabling vulnerability prediction for targeted attacks on weaker agent variants.

### RDL_15 - Evaluation Metric Exploitation
- Strength score: 1

The NSA/NCSC Guidelines do not address evaluation metric security, benchmark gaming, or adversarial exploitation of metric weaknesses. Threats including exact match metric exploitation through semantic paraphrasing (RDL_15_1), joint metric evasion through fact injection optimized for evaluation criteria (RDL_15_2), pass@k inconsistency exploitation through probabilistic activation creating detection-evading non-determinism (RDL_15_3), and milestone scoring threshold manipulation for partial credit exploitation and conditional attack execution (RDL_15_4) are ML evaluation system security concerns involving adversarial optimization against specific metric implementations that are entirely outside the scope of this lifecycle guidance framework.

### RDL_16 - Parameter and Configuration Leakage
- Strength score: 1

The NSA/NCSC Guidelines' guidance on minimizing unnecessary information exposure is tangentially relevant to limiting telemetry visibility, and access controls apply to restricting who can observe performance metrics. However, the framework provides no specific guidance on side-channel attacks through performance telemetry: context window logging leakage enabling attackers to identify large-context agents as high-value exfiltration targets (RDL_16_1), token consumption pattern analysis revealing query sensitivity distributions and operational context through cost-optimized routing differentials (RDL_16_2), or latency fingerprinting through optimized performance profiles enabling inference of deployed model configurations and optimization strategies (RDL_16_3). These are operational security and side-channel attack concerns requiring telemetry architecture controls beyond this framework's scope.

### RDL_17 - Prompt Injection and Few-Shot Attacks
- Strength score: 2

The NSA/NCSC Guidelines explicitly name prompt injection as a known adversarial ML threat, mandate "explicit detection of out-of-distribution and/or adversarial inputs," and require "appropriate checks and sanitisation of data and inputs." These controls are directly applicable to RDL_17_1 (demonstration format injection embedding instructions in few-shot output format specifications) and RDL_17_3 (few-shot demonstration contamination triggering probabilistic recall of sensitive training data), both of which are variants of prompt injection through manipulated input context. The framework's input monitoring guidance for adversarial input detection supports identifying poisoned demonstration examples. The framework does not, however, address the multi-agent propagation dynamics of demonstration injections across agent boundaries, generation pattern injection through confidence bias in few-shot examples (RDL_17_2), or the specific mechanics of few-shot poisoning for agentic contexts.

### RDL_18 - Distributed Tracing
- Strength score: 1

The NSA/NCSC Guidelines' monitoring guidance requires implementing logging and auditing for security, and the infrastructure security section requires protecting data access. However, the framework provides no guidance on securing distributed tracing backends against intelligence extraction: correlation ID and span data leakage providing attackers with complete operation chains showing all intermediate outputs and data flows across multi-agent pipelines (RDL_18_1), or response schema validation log side channels where failure patterns across heterogeneous agent schemas reveal agent type and configuration information enabling targeted attack customization (RDL_18_2). These distributed systems observability security concerns require specialized tracing infrastructure controls outside this framework's scope.


---

## RIP+RIDC - Agent identity, provenance, and economic/resource attack surface

### RIP_1 - Multi-Agent UI & Dashboard Attacks
- Strength score: 1

The NSA/NCSC Guidelines' access control guidance and monitoring requirements are conceptually applicable to protecting dashboard components and attribution logs. However, the framework provides no specific guidance on cryptographic identity verification at the UI layer to prevent agent impersonation through attribution gaps (RIP_1_1), fine-grained attribution logging enabling forensic reconstruction of multi-agent pipeline compromises (RIP_1_2), real-time per-agent cost attribution in dashboard displays (RIP_1_3), or GroupChat speaker attribution manipulation enabling social engineering through identity confusion (RIP_1_4). These are multi-agent dashboard design and UX security engineering concerns entirely outside the framework's scope.

### RIP_2 - Approval & Workflow Attacks
- Strength score: 1

The NSA/NCSC Guidelines' monitoring guidance and incident management requirements are directionally relevant to approval workflow integrity, and the access control guidance applies to protecting workflow state storage. However, the framework provides no specific guidance on cryptographic provenance trails for multi-agent approval workflows preventing decision attribution attacks (RIP_2_1), per-agent confidence score breakdowns preventing weighted aggregation manipulation (RIP_2_2), tamper-evident logging for multi-agent decision provenance preventing client-side state manipulation (RIP_2_3), cryptographic provenance for intermediate pipeline stages in sequential agent reviews (RIP_2_4), or explicit responsibility assignment mechanisms for HITL decision attribution in multi-agent approval chains (RIP_2_5).

### RIP_3 - Command Palette & UI Attacks
- Strength score: 1

The NSA/NCSC Guidelines' least privilege and access control guidance is applicable to controlling what operations command palettes can surface and execute. However, the framework provides no specific guidance on real-time cost estimation and display in command palettes preventing denial-of-wallet through context manipulation recommending expensive multi-agent operations (RIP_3_1), clear agent identity attribution in command palette suggestions preventing cross-agent impersonation (RIP_3_2), or per-agent resource tracking and rate limiting accounting for differential costs across specialized agents (RIP_3_3). These are UX economics and multi-agent resource accountability design concerns outside the framework's scope.

### RIP_4 - Multi-Agent Coordination Attacks
- Strength score: 1

The NSA/NCSC Guidelines' infrastructure security guidance and monitoring requirements are relevant to detecting coordinated resource exhaustion. However, the framework provides no specific guidance on reflection cascade detection and cost controls preventing exponential resource amplification in multi-agent feedback loops (RIP_4_1), orchestrator overload protection against non-linear coordination traffic scaling (RIP_4_2), auction-based resource allocation integrity against strategic bidding and Sybil coalition attacks (RIP_4_3), or broadcast amplification prevention in event-driven multi-agent publish-subscribe architectures (RIP_4_4). These are multi-agent coordination architecture security concerns the framework does not address.

### RIP_5 - Framework-Specific Attacks
- Strength score: 1

The NSA/NCSC Guidelines do not address framework-specific agent identity implementation differences. The cross-framework identity translation gaps creating impersonation opportunities in multi-framework deployments (RIP_5_1), framework-dependent cost attribution opacity enabling denial-of-wallet in mixed-framework systems (RIP_5_2), AutoGen conversational identity spoofing without cryptographic binding (RIP_5_3), CrewAI hierarchical role impersonation through unverified role assignment (RIP_5_4), AutoGen conversation resumption cost amplification (RIP_5_5), and Semantic Kernel context window inflation through poisoned plugin registries (RIP_5_6) are framework-specific attack surfaces requiring framework-aware controls entirely outside this general lifecycle framework's scope.

### RIP_6 - Tool & Plugin Attacks
- Strength score: 2

The NSA/NCSC Guidelines' least privilege guidance is directly applicable to RIP_6_9 (tool access privilege inheritance through agent composition delegation chains) and RIP_6_15 (privilege inference and escalation through tool audit log analysis), as the principle of applying minimum necessary privileges constrains what credentials worker agents inherit from supervisors. The access control guidance applies to protecting inter-agent communication channels carrying authentication tokens (RIP_6_11). The framework's guidance to apply appropriate restrictions to AI component actions addresses tool chaining cost amplification at the policy level (RIP_6_1). However, the framework does not address tool invocation attribution spoofing (RIP_6_8), tool registry economic reconnaissance (RIP_6_3), or multi-agent tool cost attribution complexity (RIP_6_5) with prescriptive controls.

### RIP_7 - Cost & Economic Denial-of-Service Attacks
- Strength score: 1

The NSA/NCSC Guidelines' monitoring guidance is applicable to tracking resource consumption patterns, and the supply chain security guidance's emphasis on cryptographic attestation is conceptually relevant to the provenance spoofing attacks in reasoning chains (RIP_7_12, RIP_7_14, RIP_7_15, RIP_7_22). The access control guidance applies to protecting shared credentials in multi-connector ETL authentication (RIP_7_35). However, the framework provides no specific guidance on denial-of-wallet protection, resource attribution in multi-agent systems, Kubernetes billing gaming (RIP_7_8), resource quota exhaustion against co-tenant agents (RIP_7_9), economic incentive manipulation in multi-agent allocation (RIP_7_18), or the extensive range of cost amplification, provenance spoofing, and economic attack patterns catalogued across this subsubsection's 35 items.

### RIP_8 - Resilience & Error Handling Attacks
- Strength score: 1

The NSA/NCSC Guidelines' incident management guidance addresses error handling at an organizational level, and the infrastructure security guidance applies to protecting retry and circuit breaker infrastructure. However, the framework provides no guidance on streaming response resource consumption opacity enabling economic attacks during user distraction (RIP_8_1, RIP_8_7), retry budget attribution gaps across multi-agent boundaries enabling denial-of-service (RIP_8_2), fallback provider cost attribution failures in multi-agent systems (RIP_8_3), circuit breaker resource attribution opacity (RIP_8_4), or streaming identity spoofing where early tokens establish trust before verification completes (RIP_8_6). These are multi-agent resilience architecture and economic accountability concerns the framework does not address.

### RIP_9 - Multimodal Attacks
- Strength score: 1

The NSA/NCSC Guidelines' input sanitization requirement and access controls have indirect applicability to controlling what multimodal content agents can process. However, the framework provides no specific guidance on multimodal content attribution spoofing through vision model output manipulation (RIP_9_1), coordinated multimodal DDoS through multiple agents invoking shared vision model backends (RIP_9_2), audio processing resource exhaustion through long-duration content in multi-agent audio RAG systems (RIP_9_3), or vector store resource amplification through multimodal-scale embedding storage creating infrastructure strain exploitable through coordinated agent queries (RIP_9_4). These are multimodal infrastructure resource security concerns outside the framework's scope.

### RIP_10 - Reasoning & Evaluation Attacks
- Strength score: 1

The NSA/NCSC Guidelines mention responsible release including benchmarking, implying evaluation data exists and has value, and the monitoring guidance is relevant to detecting evaluation anomalies. However, the framework provides no specific guidance on evaluation cost attribution failures enabling economic DoS through false alarm triggering (RIP_10_1, RIP_10_4, RIP_10_6), evaluation agent identity spoofing in decision displays without cryptographic binding (RIP_10_2, RIP_10_5), evaluation artifact provenance opacity enabling false attribution (RIP_10_3), benchmark attribution spoofing (RIP_10_7), quality score identity confusion in cross-agent assessment propagation (RIP_10_8), sampling budget resource allocation attacks (RIP_10_9, RIP_10_10), or MCTS computational budget exhaustion affecting shared multi-agent resources (RIP_10_11).

### RIP_11 - Vector Store & RAG Attacks
- Strength score: 2

The NSA/NCSC Guidelines' supply chain security guidance — requiring components from verified sources, tracking and authenticating AI assets, and applying cryptographic hashes — is applicable to vector database provenance verification and embedding endpoint authentication (RIP_11_9, RIP_11_10). The guidance on managing what data AI systems can access and applying appropriate access controls addresses vector database partition access isolation (RIP_11_6) and knowledge graph authorization ambiguity (RIP_11_8). The supply chain SBOM and vendor assessment guidance is relevant to provider lock-in data provenance during migrations (RIP_11_12). However, the framework does not address knowledge base source attribution loss through multi-agent processing (RIP_11_3), vector database query load amplification (RIP_11_7), shared API key provenance loss (RIP_11_11), or indexing infrastructure cost exploitation (RIP_11_5) with prescriptive controls.

### RIP_12 - Memory & Session Attacks
- Strength score: 2

The NSA/NCSC Guidelines explicitly require "applying appropriate checks and sanitisation of data and inputs; this includes when incorporating user feedback or continuous learning data into your model," which is directly applicable to cross-agent session context poisoning (RIP_12_9) and memory-driven context pollution enabling persistent multi-agent infections (RIP_12_1). The framework's guidance on cryptographic model integrity through hashes and signatures applies to model checkpoint integrity (RIP_12_11), and the access control guidance supports protecting shared session state from unauthorized manipulation. The requirement to "manage what data AI systems can access" addresses semantic memory provenance attribution concerns (RIP_12_8). The framework does not, however, address LangGraph cycling cost controls (RIP_12_3), MCTS tree growth memory exhaustion (RIP_12_5), or working memory inflation through context bloating across agent hops (RIP_12_7).

### RIP_13 - Infrastructure & Deployment Attacks
- Strength score: 2

The NSA/NCSC Guidelines' "Secure your infrastructure" section directly states: "apply good infrastructure security principles" and require "appropriate segregation of environments," which is applicable to Kubernetes namespace isolation bypass (RIP_13_5), container escape via shared node (RIP_13_1), and replica identity ambiguity through identical pod templates (RIP_13_4, RIP_13_8). The supply chain security guidance covering SBOM and verified component sources directly addresses NIM container image version ambiguity from floating tags (RIP_13_3) and provenance loss in distributed engine caching (RIP_13_6). The cryptographic hash guidance supports verifying container and engine integrity. However, the framework does not address GPU resource contention as an economic attack surface (RIP_13_5, RIP_13_7) or engine binary non-fungibility breaking agent identity across hardware variants (RIP_13_9).

### RIP_14 - ML/Training & Model Attacks
- Strength score: 2

The NSA/NCSC Guidelines' supply chain security guidance requires acquiring components from verified sources and performing supply chain assessment, which is directly applicable to model registry identity spoofing through version tag manipulation (RIP_14_2) and quantization artifact provenance verification gaps (RIP_14_3). The framework's requirement to compute and share cryptographic hashes and signatures of model files directly supports model integrity verification against substitution attacks. The guidance on tracking and authenticating AI assets is relevant to training data provenance obfuscation (RIP_14_5) and model ownership disputes (RIP_14_6). However, the framework does not address behavioral fingerprinting for agent identity spoofing through training (RIP_14_4), computational cost emergence through learned efficiency optimization (RIP_14_7), or training resource acquisition attacks preventing legitimate model updates (RIP_14_8).

### RIP_15 - Kubernetes & Container Attacks
- Strength score: 2

The NSA/NCSC Guidelines' infrastructure security guidance requiring "appropriate segregation of environments" and "good infrastructure security principles" is directly applicable to Kubernetes service account identity pooling vulnerabilities (RIP_15_1), namespace-based multi-tenancy isolation bypass (RIP_15_5), and GPU resource request gaming for hardware hoarding (RIP_15_4). The framework's access control guidance supports protecting service account tokens and restricting cross-namespace access. The supply chain security guidance is relevant to cluster node identity spoofing in gossip protocols (RIP_15_6). However, the framework provides no specific guidance on IP spoofing via iptables manipulation (RIP_15_2), pod eviction ordering reconnaissance (RIP_15_3), or load balancer VIP masking individual node provenance (RIP_15_7).

### RIP_16 - Load Balancer & Scaling Attacks
- Strength score: 2

The NSA/NCSC Guidelines' infrastructure security section requires applying good infrastructure security principles and protecting AI system components, which is directly applicable to load balancer endpoint security and protection against man-in-the-middle attacks on load balancer identity (RIP_16_2). The framework's access control and secure by default configuration guidance applies to weighted routing privilege asymmetry controls (RIP_16_3) and auto-scaling identity continuity requirements (RIP_16_4). These provide meaningful structural support, though the framework does not address load balancer identity abstraction attacks specifically (RIP_16_1) or auto-scaling identity discontinuity exploitation in multi-agent contexts.

### RIP_17 - Service Discovery & Authentication Attacks
- Strength score: 2

The NSA/NCSC Guidelines' infrastructure security guidance and access control requirements directly address DNS cache poisoning as a service discovery attack (RIP_17_6) and API gateway consumer identity header injection for rate limit bypass (RIP_17_3). The supply chain security guidance's emphasis on trusted sources and cryptographic attestation is relevant to certificate authority compromise enabling fleet-wide agent spoofing (RIP_17_7). The framework's infrastructure protection guidance applies to message queue consumer identity spoofing (RIP_17_2) and service registration audit gap remediation (RIP_17_1). However, the framework does not address Prometheus cardinality explosion DoS through unbounded agent identity labels (RIP_17_4) or priority queue starvation as an economic attack (RIP_17_5).

### RIP_18 - Rule/Method/Parameter Attribution Attacks
- Strength score: 1

The NSA/NCSC Guidelines' supply chain security guidance is applicable at a high level to method and rule registry integrity, and the least privilege principle applies to delegation chain authorization verification. However, the framework provides no specific guidance on efficiency metric spoofing for agent identity deception (RIP_18_1), method authorship spoofing in shared registries (RIP_18_2), delegation chain identity verification gaps in HTN hierarchies (RIP_18_3), rule attribution confusion in multi-agent aggregation (RIP_18_4), parameter origin confusion through transitive trust chains (RIP_18_5, RIP_18_6), or rule version control poisoning propagating through shared repositories (RIP_18_10). These are rule-based and planning system security concerns requiring specialized controls not addressed by this framework.

### RIP_19 - Hybrid & Cross-Paradigm Attacks
- Strength score: 1

The NSA/NCSC Guidelines' supply chain security guidance covers component provenance tracking from verified sources, which is tangentially applicable to paradigm attribution confusion in hybrid component source validation (RIP_19_1). However, the framework provides no guidance on multi-paradigm credential mechanism diversity creating identity spoofing opportunities through paradigm-specific credential confusion (RIP_19_2). These hybrid AI architecture security concerns combining neural models, symbolic rules, and utility functions require specialized cross-paradigm security engineering beyond this framework's scope.

### RIDC_1 - UI and User Interaction Attack Surfaces
- Strength score: 2

The NSA/NCSC Guidelines explicitly name prompt injection as a known adversarial ML threat and mandate "explicit detection of out-of-distribution and/or adversarial inputs" and "appropriate checks and sanitisation of data and inputs." These controls are directly applicable to RIDC_1_2 (chat interface attribution confusion enabling social engineering through injected content appearing from trusted agents) and RIDC_1_6 (command palette context prediction as covert instruction channel through poisoned context sources). The input monitoring guidance for adversarial input detection supports identifying malicious content in ARIA live regions (RIDC_1_3) and keyboard shortcut configuration injection (RIDC_1_4). However, the framework does not address progressive disclosure as layered injection surface (RIDC_1_1), semantic landmark identity spoofing (RIDC_1_5), error communication auto-retry amplification of injected instructions (RIDC_1_7), or notification timeout-based transitive trust chain exploitation (RIDC_1_8) with specific controls.

### RIDC_2 - Messaging and Protocol Injection
- Strength score: 2

The NSA/NCSC Guidelines' explicit recognition of prompt injection as a known adversarial ML threat and requirement for input sanitization are applicable to FIPA-ACL performative spoofing where message content contradicts performative metadata to inject malicious instructions bypassing authorization checks (RIDC_2_1) and conversation-ID chain hijacking to pollute shared context inherited by downstream agents (RIDC_2_3), both of which are forms of instruction injection through inter-agent communication channels. The framework's input sanitization guidance applies to protocol message content validation. However, the framework does not address protocol language downgrade attacks exploiting format negotiation weaknesses in heterogeneous multi-agent messaging systems (RIDC_2_2) with specific controls.

### RIDC_3 - Framework and State Management Injection
- Strength score: 1

The NSA/NCSC Guidelines' prompt injection guidance and input sanitization requirements are directionally relevant to state schema-based instruction injection. However, the framework provides no specific guidance on framework-enforced trust boundary violations through state schema coercion in LangGraph/CrewAI/AutoGen cross-framework workflows (RIDC_3_1), cross-framework state migration enabling transitive instruction propagation where data becomes code during LangChain-to-LangGraph state transformation (RIDC_3_2), state schema field injection exploiting heterogeneous reducer behavior across agents (RIDC_3_3), or conditional edge routing hijacking through state manipulation (RIDC_3_4). These agentic framework-specific instruction propagation vulnerabilities require framework-level security engineering not addressed by this general guidance.

### RIDC_4 - Tool, Function Calling, and Plugin Injection
- Strength score: 2

The NSA/NCSC Guidelines explicitly name prompt injection as a known adversarial ML threat and require input sanitization and adversarial input detection, which are directly applicable to tool description injection through RAG-retrieved tool metadata (RIDC_4_3 — a canonical indirect prompt injection attack), plugin-function metadata injection through decorated function descriptions (RIDC_4_6), and tool schema evasion through semantic payload injection passing syntactic validation (RIDC_4_10). The framework's guidance on controlling what data AI systems can access supports tool registry access control preventing unauthorized metadata poisoning. The framework does not, however, address memory-driven tool selection manipulation via conversation history (RIDC_4_1), function calling parameter injection via type coercion chains (RIDC_4_4), semantic function template injection through variable interpolation (RIDC_4_7), or native function dependency injection attacks on kernel service resolution (RIDC_4_8) with specific technical controls.

### RIDC_5 - ReAct and Reasoning Architecture Injection
- Strength score: 2

The NSA/NCSC Guidelines' explicit recognition of prompt injection as a known adversarial ML threat and requirement for "explicit detection of out-of-distribution and/or adversarial inputs" are applicable to ReAct reasoning trace injection where malicious instructions in observation data hijack subsequent reasoning propagating across multi-agent trace sharing (RIDC_5_1), reflection self-critique reinforcement hijacking where dual-agent critic patterns amplify rather than remove injections (RIDC_5_11), and precondition smuggling injecting instructions into HTN method selection through natural language precondition descriptions (RIDC_5_5). The input monitoring guidance supports detecting adversarial payloads in planning documentation. The framework does not address plan-and-execute planning phase conflation attacks with responsibility diffusion across specialized agents (RIDC_5_4), or the specific HTN injection surfaces through abstract task descriptions, constraint specifications, effect descriptions, and causal link manipulation (RIDC_5_6 through RIDC_5_10).

### RIDC_6 - Streaming and Real-Time Communication Injection
- Strength score: 1

The NSA/NCSC Guidelines' prompt injection guidance and monitoring requirements are directionally relevant to detecting injected instructions in streaming content. However, the framework provides no specific guidance on timing-based streaming response manipulation exploiting multi-agent handoff windows where instructions embedded in middle sections of long responses evade human review (RIDC_6_1), or the non-deterministic generation speeds creating variable injection opportunities unique to streaming architectures in multi-agent workflows. These real-time streaming injection concerns are outside the framework's scope.

### RIDC_7 - Generation Parameter and Inference Configuration Injection
- Strength score: 2

The NSA/NCSC Guidelines' explicit recognition that "training data defines system behaviour" and the requirement to sanitize "user feedback or continuous learning data" are directly applicable to evaluation dataset poisoning through adversarial inputs in production logs affecting all agents sharing the dataset (RIDC_7_1) and evaluation context memory poisoning through persistent cross-batch state pollution (RIDC_7_6). The data poisoning threat category acknowledged by the framework covers benchmark ground truth injection (RIDC_7_10) at a conceptual level. The framework's monitoring guidance is relevant to detecting baseline manipulation through agent substitution (RIDC_7_3) and confidence score inflation in custom evaluation metrics (RIDC_7_8). The framework does not, however, address evaluation metric selection manipulation via natural language metric definition poisoning (RIDC_7_2), multi-agent orchestration control-flow hijacking through evaluation pipeline injection (RIDC_7_4), or metric calculation hijacking through untrusted custom metric function injection (RIDC_7_7).


---

## RMP - Long-lived cognitive state abuse: memory poisoning and latent backdoors

### RMP_1 - UI/UX Memory Manipulation Attacks
- Strength score: 2

The NSA/NCSC Guidelines' behavior monitoring guidance requiring observation of "sudden and gradual changes in behaviour affecting security" is directly applicable to detecting session persistence memory poisoning through progressive context accumulation (RMP_1_1) and conversation thread continuity enabling cross-session instruction persistence (RMP_1_6), both of which manifest as gradual behavioral drift. The framework's input sanitization requirements apply to detecting and filtering malicious content injected into session state before it becomes persistent cognitive state. The guidance to "manage what data AI systems can access" addresses context panel metadata injection (RMP_1_5) and inline suggestion acceptance creating cascading RAG contamination (RMP_1_10). However, the framework provides no specific guidance on conversation history provenance opacity in multi-agent flat timeline displays (RMP_1_2), context reference links as latent backdoor activation vectors across temporal and agent boundaries (RMP_1_3), context window eviction attack vectors exploiting heterogeneous window sizes to selectively preserve malicious instructions (RMP_1_11), or multi-agent dashboard context switching revealing cognitive isolation failures (RMP_1_9).

### RMP_2 - Multi-Agent State Handoff and Cross-Agent Memory Attacks
- Strength score: 2

The NSA/NCSC Guidelines' data poisoning threat recognition is directly applicable to cross-agent vector embedding poisoning (RMP_2_8), similarity-search poisoning via semantic anchor injection (RMP_2_7), and knowledge graph relationship injection via agent coordination (RMP_2_9), all of which involve poisoning shared AI knowledge stores. The supply chain security guidance requiring components from verified sources and applying cryptographic attestation is relevant to API gateway routing manipulation (RMP_2_1) and embedding endpoint verification. The framework's access control guidance addresses cross-type memory poisoning mitigation through data access restriction. However, the framework provides no specific guidance on gRPC schema versioning backward-compatibility exploitation (RMP_2_2), swarm intelligence local rule injection causing emergent malicious behavior (RMP_2_3), adversarial importance weighting for selective memory retention manipulation (RMP_2_5), distributed knowledge graph traversal race conditions (RMP_2_10), or cross-agent memory type misclassification during serialization (RMP_2_6).

### RMP_3 - Multimodal and Cross-Modal Poisoning Attacks
- Strength score: 2

The NSA/NCSC Guidelines explicitly name prompt injection as a known adversarial ML threat and mandate input sanitization and adversarial input detection, which are directly applicable to cross-modal instruction injection (RMP_3_1) where malicious instructions embedded in non-text modalities bypass text safety filters, and multimodal chunk poisoning in RAG (RMP_3_6) where instructions hide in modality gaps text-only validation misses. The data poisoning threat recognition applies to distributed memory poisoning via perception corruption (RMP_3_4) and audio transcript memory corruption through Whisper hallucination persistence (RMP_3_7). The framework does not, however, address temporal sensor desync exploitation (RMP_3_2), multi-agent modality contradiction attacks manipulating consensus (RMP_3_3), HTML entity encoding confusion across heterogeneous multi-agent parsers (RMP_3_9), CSS pseudo-element dual-channel content path exploitation (RMP_3_10), or vision model output creating cross-session backdoor persistence (RMP_3_5) with specific controls.

### RMP_4 - Framework-Specific Memory Vulnerabilities
- Strength score: 2

The NSA/NCSC Guidelines' explicit requirement to "apply appropriate checks and sanitisation of data and inputs" is directly applicable to RMP_4_8 (tool output integration into memory without sanitization), where unsanitized tool results become persistent backdoor vectors with multiplicative reach in multi-agent systems. The data poisoning threat recognition applies to ConversationBufferMemory poisoning (RMP_4_5), agent scratchpad poisoning (RMP_4_6), AutoGen GroupChat shared history poisoning (RMP_4_9), and Semantic Kernel context persistence as cross-plugin backdoor (RMP_4_12). The behavior monitoring guidance supports detecting anomalous tool invocation pattern changes (RMP_4_17) and sudden behavioral deviations from checkpoint resumption (RMP_4_3). However, the framework provides no specific guidance on framework-dependent memory architecture mismatches at LangChain/LangGraph/AutoGen boundaries (RMP_4_1), reducer append-only function creating unintended cross-agent persistence (RMP_4_4), memory key namespace collision in shared backends (RMP_4_7), or service registration state persistence as cross-workflow backdoor (RMP_4_13).

### RMP_5 - Caching and Persistence Attacks
- Strength score: 2

The NSA/NCSC Guidelines' input sanitization guidance is directly applicable to error recovery payload persistence (RMP_5_2) where malicious content in error state should be sanitized before storage as diagnostic context, and to error message storage as backdoor activation vectors (RMP_5_4). The framework's data poisoning threat recognition applies to embedding cache poisoning creating persistent fleet-wide latent backdoors (RMP_5_1) and fallback state caching creating long-term cognitive corruption (RMP_5_3). The infrastructure security guidance's requirement for secure by default settings is relevant to graceful degradation configuration persistence as behavioral memory injection (RMP_5_6), where degradation-driven capability disablement should revert by default. The framework does not, however, address retry state history as cognitive corruption vector (RMP_5_5) or cache invalidation policies for security-relevant content with specific controls.

### RMP_6 - Streaming and Real-time Processing Attacks
- Strength score: 1

The NSA/NCSC Guidelines' prompt injection guidance and monitoring requirements are directionally relevant to detecting malicious instructions in streaming content before memory persistence. However, the framework provides no specific guidance on streaming context accumulation creating injection windows where malicious instructions embed before complete validation completes (RMP_6_1), progressive disclosure in streaming hiding persistence instructions in later-streamed content that executes before full stream validation (RMP_6_2), streaming token timing for distributed memory corruption exploiting synchronization windows (RMP_6_3), or conversation history injection through streaming handoff buffering points at agent boundaries (RMP_6_4). These real-time streaming memory security concerns require streaming-architecture-specific controls beyond the framework's scope.

### RMP_7 - Evaluation and Metrics Poisoning
- Strength score: 2

The NSA/NCSC Guidelines' data poisoning threat recognition is directly applicable to evaluation history poisoning through baseline persistence manipulation (RMP_7_1), cached evaluation results poisoning through replay attacks (RMP_7_3), and benchmark performance history as persistent cognitive corruption (RMP_7_8), all of which involve poisoning shared measurement stores that guide agent behavior. The framework's recognition that training data defines system behavior applies to learned evaluation patterns as latent backdoors (RMP_7_2) and learned benchmark artifacts as persistent agent bias (RMP_7_9). The behavior monitoring guidance is relevant to detecting confidence calibration degradation through multi-agent propagation (RMP_7_10). The framework does not, however, address evaluation scratchpad poisoning enabling false record injection (RMP_7_4), metric aggregation state manipulation at the aggregation layer (RMP_7_7), or evaluation session state persistence enabling bypass (RMP_7_6) with specific controls.

### RMP_8 - Parameter Tuning and Configuration Poisoning
- Strength score: 2

The NSA/NCSC Guidelines explicitly recognize data poisoning as a known adversarial ML threat and require sanitization of training/learning data, which is directly applicable to tuning dataset memory poisoning through shared benchmark artifacts (RMP_8_4) where all agents tuning on shared poisoned data simultaneously memorize adversarial patterns. The supply chain security guidance for verified component sources and SBOM is relevant to hyperparameter configuration store integrity (RMP_8_5) and Pareto frontier configuration poisoning through baseline manipulation (RMP_8_6). The framework's cryptographic model integrity guidance applies to tuning artifact provenance verification. However, the framework does not address multi-modal web benchmark image metadata injection via EXIF/XMP (RMP_8_1), model-specific memory architecture tuning creating cross-model state corruption vectors (RMP_8_2), or context window truncation-point diversity creating heterogeneous instruction survival patterns (RMP_8_3).

### RMP_9 - Learning and Training Data Attacks
- Strength score: 3

The NSA/NCSC Guidelines directly and strongly address this subsubsection: they explicitly recognize data poisoning as a key adversarial ML threat, state that "training data defines system behaviour," and require "applying appropriate checks and sanitisation of data and inputs; this includes when incorporating user feedback or continuous learning data into your model." These controls are directly applicable to all three items: few-shot CoT demonstration injection poisoning planning agent demonstrations that execution agents inherit (RMP_9_1), trajectory data injection in few-shot RL flywheels where adversarially injected trajectories propagate through multi-agent learning chains (RMP_9_2), and fine-tuning data contamination through poisoned example selection heuristics (RMP_9_3). The framework's supply chain security guidance further supports validating the provenance of training data sources before use. The framework does not, however, provide specific technical countermeasures for detecting subtle multi-hop trajectory poisoning or few-shot demonstration contamination in automated learning pipelines.

### RMP_10 - Observability and Tracing Attacks
- Strength score: 1

The NSA/NCSC Guidelines' monitoring guidance requires implementing behavioral observation for security, and the infrastructure security section requires protecting observability infrastructure. However, the framework provides no specific guidance on trace timestamp clock skew as causality corruption enabling false causal order injection (RMP_10_1), trace filtering manipulation making critical tool invocations invisible in shared multi-agent trace repositories (RMP_10_2), multi-agent attack architecture design maximally separating symptom from cause to defeat root cause analysis (RMP_10_3), or confidence score propagation poisoning through trace manipulation affecting all downstream decision-making (RMP_10_4). These distributed systems observability integrity concerns require cryptographic trace verification controls beyond this framework's scope.

### RMP_11 - Learned Behavior and Memory Evolution
- Strength score: 2

The NSA/NCSC Guidelines' recognition that "training data defines system behaviour" and the requirement to sanitize continuous learning data are directly applicable to memory reduction exploits through hallucinated historical context (RMP_11_1) — where hallucinated tool parameters stored in shared memory corrupt other agents' decisions — and memory evolution manipulation enabling persistent instruction injection (RMP_11_2), where malicious patterns inserted by one agent survive consolidation as collective learned behavior. The framework's behavior monitoring guidance supports detecting anomalous behavioral drift from evolved memory corruption. The framework does not, however, address specific technical controls for detecting hallucinated memory entries versus authentic historical records, or mechanisms for validating shared memory before consolidation across agent boundaries.

### RMP_12 - Context and Parameter Consistency
- Strength score: 1

The NSA/NCSC Guidelines' input sanitization guidance and access control requirements are conceptually applicable to protecting shared memory from unauthorized writes, and the behavior monitoring guidance may detect downstream behavioral consequences of context consistency loss. However, the framework provides no specific guidance on memory slice attacks where poisoned memories selectively retrieve for specific agents based on query patterns (RMP_12_1), long-term memory cross-session poisoning propagating through shared user memory in multi-agent systems (RMP_12_2), poisoned parameter history in shared conversation memory treated as ground truth (RMP_12_3), cascading parameter accuracy regression across multi-agent turns (RMP_12_4), tool selection consistency loss creating parameter mismatches at agent boundaries (RMP_12_5), or parameter validation provenance disappearing across memory boundaries producing blind parameters (RMP_12_6).

### RMP_13 - Reasoning Transparency and Trust
- Strength score: 1

The NSA/NCSC Guidelines' behavior monitoring guidance may detect anomalous reasoning patterns, and the general guidance on designing AI systems that users can trust is directionally relevant. However, the framework provides no specific guidance on reasoning transparency weaponization for social engineering where explicit reasoning traces become stronger manipulation vectors than opaque decisions (RMP_13_1), multi-agent reasoning disagreement exploitation through coordinated payload injection to different agents to steer user trust (RMP_13_2), or reasoning confidence calibration attacks where compromised agents propagate false confidence signals to downstream agents and users (RMP_13_3). These human-AI trust exploitation and multi-agent social engineering concerns require controls the framework does not address.

### RMP_14 - Efficiency and Resource Tracking
- Strength score: 2

The NSA/NCSC Guidelines' behavior monitoring guidance requiring observation of "sudden and gradual changes in behaviour affecting security" is directly applicable to detecting efficiency memory accumulation as persistent injection vectors (RMP_14_1) — where poisoned efficiency history causes fleet-wide resource misallocation — and baseline metric degradation as silent attack activation (RMP_14_3) where gradual poisoning forces agents into resource-constrained execution paths. The data poisoning threat recognition applies to cost attribution memory poisoning corrupting orchestration decisions (RMP_14_4) and efficiency insight persistence embedding learned backdoors in shared repositories (RMP_14_5). The framework does not, however, address conversation history summarization lossy compression creating tailored injection survival opportunities (RMP_14_2) with specific controls.

### RMP_15 - Vector Database and Embedding Poisoning
- Strength score: 2

The NSA/NCSC Guidelines' data poisoning threat recognition is directly applicable to shared embedding cache corruption enabling fleet-wide adversarial vector injection (RMP_15_2) and cluster replication protocol poisoning that automatically propagates corrupted vectors to all replicas (RMP_15_5). The framework's cryptographic model integrity guidance — requiring cryptographic hashes and signatures of model artifacts — is directly applicable to HNSW graph structure integrity validation preventing poisoned index rebuilds (RMP_15_3) and persistent volume poisoning detection through integrity verification (RMP_15_4). The supply chain security guidance supports validating embedding provider authenticity to prevent cross-provider semantic space poisoning (RMP_15_1). The framework does not, however, address HNSW parameter manipulation during rebuild as a specific attack vector, or vector-level integrity verification beyond file-level cryptographic hashing.

### RMP_16 - ETL Pipeline and Data Processing Attacks
- Strength score: 2

The NSA/NCSC Guidelines' explicit recognition of data poisoning as a known adversarial ML threat and the requirement to sanitize training and continuous learning data are directly applicable to ETL pipeline attacks targeting knowledge base quality: citation database poisoning with fabricated source attributions (RMP_16_10), batch processing queue contamination spreading malicious content through processing proximity (RMP_16_11), chunking overlap poisoning multiplying malicious content across adjacent chunks (RMP_16_3), and semantic cache entry poisoning through adversarial embedding optimization (RMP_16_4). The supply chain security guidance for data provenance validation is relevant to ETL state file integrity (RMP_16_1, RMP_16_8). The framework does not, however, address quality validation threshold progressive degradation through institutional memory loss (RMP_16_2, RMP_16_6, RMP_16_9), deduplication logic manipulation through threshold tampering (RMP_16_7), response cache corruption through query normalization collisions (RMP_16_5), or MinHash hash collision injection for training data substitution (RMP_16_12) with specific technical controls.


---

## RND - Non-determinism, continual change, and assurance gaps as a risk surface

### RND_1 - UI/Streaming and Real-Time Generation
- Strength score: 1

The NSA/NCSC Guidelines' behavior monitoring guidance requires observing "sudden and gradual changes in behaviour affecting security," which is directionally relevant to detecting behavioral anomalies arising from streaming non-determinism. However, the framework provides no specific guidance on streaming token sampling creating ephemeral vulnerability windows where malicious content appears probabilistically before safety filtering applies (RND_1_3), multi-agent streaming latency non-determinism enabling timing-based attacks exploiting combinatorially many execution orderings (RND_1_2), audit reproducibility failures from stochastic intermediate UI states (RND_1_1), or race conditions between inline suggestion generation and safety filtering in multi-agent pipelines (RND_1_4). Non-determinism, stochastic behavior, and audit irreproducibility as security properties are entirely outside this framework's scope.

### RND_2 - Progressive Disclosure and Error Handling
- Strength score: 1

The NSA/NCSC Guidelines' incident management guidance and monitoring requirements are relevant to error handling at an organizational level. However, the framework provides no specific guidance on progressive disclosure creating exponentially complex state spaces making exhaustive validation impossible across multi-agent disclosure combinations (RND_2_1), forensic blind spots where attack evidence resides in unexpanded technical disclosure layers with O(2^NM) attribution complexity in multi-agent error chains (RND_2_2), or streaming error handling creating ephemeral non-deterministic states where downstream agent recovery masks upstream errors across variable-visibility paths that cannot be exhaustively tested (RND_2_3). Audit reproducibility and combinatorial state explosion are inherent properties of multi-agent streaming systems not addressed by this framework.

### RND_3 - Context, Persistence, and Session Management
- Strength score: 1

The NSA/NCSC Guidelines' behavior monitoring guidance may detect multi-turn attack patterns manifesting as gradual behavioral drift, and the input sanitization guidance applies to detecting malicious content in session context. However, the framework provides no specific guidance on multi-turn attack non-determinism where malicious intent distributed across turns evades stateless safety classifiers while achieving 80-95%+ empirical success rates (RND_3_1), stale security context from session persistence across UI state changes creating inconsistent trust models across agents accessing context at different times (RND_3_2), or the fundamental assurance gap that compound stochastic execution paths prevent reliable audit replay of multi-agent cross-memory contamination attacks. These non-determinism and assurance challenges require evaluation methodology controls not addressed by this framework.

### RND_4 - Multi-Agent Coordination and Decision-Making
- Strength score: 1

The NSA/NCSC Guidelines' monitoring guidance is directionally relevant to detecting coordinated agent attacks, and input monitoring for adversarial detection applies generally. However, the framework provides no specific guidance on chat interface streaming attribution confusion enabling concurrent multi-agent social engineering through concurrent malicious and benign streams (RND_4_1), command palette context prediction non-determinism defeating safety evaluations that assume consistent command visibility (RND_4_2), approval workflow confidence score non-determinism allowing attackers to submit identical requests until variation produces auto-approval (RND_4_3), or dashboard real-time state update race conditions creating brief "safe" indicator windows exploitable for malicious operation injection (RND_4_4).

### RND_5 - Retry, Resilience, and Error Recovery
- Strength score: 1

The NSA/NCSC Guidelines' incident management guidance addresses error recovery at an organizational level, and infrastructure security guidance applies to retry infrastructure protection. However, the framework provides no specific guidance on auto-retry error recovery creating hidden attack amplification loops where malicious payloads execute during retries while appearing as transient errors in collapsed logs (RND_5_1), exponential backoff timing non-determinism defeating audit reproducibility of timing-exploiting attacks (RND_5_2), non-idempotent operation exploitation through coordinated multi-agent retry storms triggering duplicate financial transactions (RND_5_3), fallback route selection non-determinism creating untestable attack paths (RND_5_4), or circuit breaker and graceful degradation threshold non-determinism creating inconsistent multi-agent security postures (RND_5_5, RND_5_6).

### RND_6 - Checkpoint, State, and Recovery Management
- Strength score: 1

The NSA/NCSC Guidelines' infrastructure security guidance and incident management requirements are relevant to checkpoint system protection. However, the framework provides no specific guidance on checkpoint replay poisoning through state manipulation during capture-to-resumption windows where semantically valid but logically poisoned data propagates through orchestrators (RND_6_1), determinism violation amplification via cascading state divergence exploiting non-deterministic replay logic across multi-agent systems (RND_6_2), or retry logic exhaustion through adversarial error injection enabling distributed denial-of-service that appears as distributed transient failures (RND_6_3). These checkpoint integrity and replay non-determinism concerns require distributed state security controls beyond this framework's scope.

### RND_7 - Tool Integration and API Management
- Strength score: 2

The NSA/NCSC Guidelines' update management guidance and the requirement to track and authenticate AI assets are directly applicable to API version drift creating cascading silent failures in multi-agent Plan-and-Execute chains (RND_7_1) and tool schema version drift where agents run different schema versions simultaneously (RND_7_2). The framework's supply chain security guidance requiring verified component sources and SBOM documentation is relevant to tool API compatibility drift (RND_7_4, RND_7_5) and few-shot API calling example poisoning in tool chains (RND_7_6). The guidance on monitoring for behavioral changes supports detecting silent failures from API drift. The framework does not, however, address function calling model update behavioral discontinuity across heterogeneous LLM versions in multi-agent systems (RND_7_3) or demonstration-driven fallback behavior injection (RND_7_7) with specific controls.

### RND_8 - Framework and Architecture Non-Determinism
- Strength score: 1

The NSA/NCSC Guidelines do not address combinatorial non-determinism in multi-agent systems where N agents each employing M reasoning steps with K tool choices create behavior spaces approaching (K^M)^N, making exhaustive testing intractable (RND_8_1). The framework provides no guidance on framework non-determinism from mixing multiple frameworks creating multiplicative untestable attack surfaces (RND_8_2), continual framework vendor updates requiring re-evaluation of N(N-1)/2 framework combinations making comprehensive re-testing prohibitive (RND_8_3), or framework update invalidating streaming security assumptions (RND_8_4). Assurance methodology for stochastic multi-agent systems is beyond this framework's scope.

### RND_9 - LangChain/LangGraph Specific Non-Determinism
- Strength score: 1

The NSA/NCSC Guidelines provide no framework-specific guidance on LangGraph's non-deterministic conditional edge evaluation creating untestable routing when depending on external factors (RND_9_1), reducer behavior divergence across agents with different state mutation expectations (RND_9_2), checkpoint restoration non-determinism in cyclic workflow replay (RND_9_3), confidence score non-determinism in tool selection enabling intermittent safety gate failures (RND_9_4), memory update ordering race conditions in distributed systems (RND_9_5), model version updates invalidating security evaluations (RND_9_6), or tool API compatibility drift creating untestable behavioral divergence (RND_9_7). These LangChain/LangGraph-specific assurance concerns require framework-level testing methodology controls outside this framework's scope.

### RND_10 - AutoGen Specific Non-Determinism
- Strength score: 1

The NSA/NCSC Guidelines provide no guidance on AutoGen-specific non-determinism concerns: message-driven architecture with stochastic LLM generation making attack reproducibility impossible for attacks succeeding in 0.1% of executions (RND_10_1), GroupChat speaker selection non-determinism creating probabilistic windows for malicious agent selection (RND_10_2), or multi-framework update non-determinism from evaluating N agents across M framework versions across P LLM versions creating infeasible re-testing burdens (RND_10_3). These AutoGen-specific assurance gaps require probabilistic security testing methodologies beyond this framework's scope.

### RND_11 - CrewAI Specific Non-Determinism
- Strength score: 1

The NSA/NCSC Guidelines provide no guidance on CrewAI's hierarchical task routing non-determinism where probabilistic worker selection causes identical delegations to route differently across executions, enabling attacks that succeed only when routed to compromised workers and evade deterministic testing that uses a fixed delegation path (RND_11_1). Framework-specific routing probabilism is outside this framework's scope.

### RND_12 - Semantic Kernel Specific Non-Determinism
- Strength score: 1

The NSA/NCSC Guidelines provide no guidance on Semantic Kernel's LLM-driven function routing non-determinism defeating audit reproducibility of plugin selection (RND_12_1), dynamic plugin registry state changes invalidating prior evaluations without code changes (RND_12_2), FunctionChoiceBehavior configuration drift across agent updates creating inconsistent routing (RND_12_3), or service registration lifecycle enabling state inconsistency across concurrently observing agents (RND_12_4). These Semantic Kernel-specific assurance concerns are outside this framework's scope.

### RND_13 - Multimodal and Vision-Language Models
- Strength score: 1

The NSA/NCSC Guidelines' update management guidance and asset tracking requirements are tangentially applicable to managing multimodal model version drift. However, the framework provides no specific guidance on multimodal model version drift creating inconsistent cross-agent behavior from asynchronous updates (RND_13_1), vision model output format inconsistency across agent types processing different modalities (RND_13_2), Whisper transcription hallucination pattern changes through model updates (RND_13_3), embedding quantization inconsistency affecting cross-agent retrieval ranking (RND_13_4), or temperature variation in vision-language model output creating inconsistent multi-agent synthesis (RND_13_5). These multimodal model assurance concerns require specialized evaluation frameworks not addressed here.

### RND_14 - Evaluation, Testing, and Benchmarking Non-Determinism
- Strength score: 1

The NSA/NCSC Guidelines mention responsible release including benchmarking and red teaming, which implies evaluation should occur before deployment. However, the framework provides no guidance on non-deterministic evaluation results defeating regression testing when stochasticity of multiple agents compounds (RND_14_1), model update invalidating evaluation metric baselines through independent update schedules (RND_14_2), evaluation pipeline component drift creating irreproducible results (RND_14_3), stochastic test case selection creating non-deterministic coverage (RND_14_4), framework-dependent evaluation behavior creating incomparable results (RND_14_5), or statistical significance thresholds becoming meaningless in multi-agent contexts due to multiple comparison problems (RND_14_7). These evaluation methodology concerns for AI systems are beyond the framework's scope.

### RND_15 - Temperature and Sampling Parameters
- Strength score: 1

The NSA/NCSC Guidelines do not address temperature and sampling parameter security concerns in AI agents. Sampling parameter non-determinism creating unpredictable multi-agent behavior chains (RND_15_1), continuous parameter re-tuning creating moving-target security properties (RND_15_2), and cross-agent parameter drift creating inconsistent security postures where one agent's temperature rejects injections while a peer agent accepts them (RND_15_3) are fundamental AI system properties requiring ML engineering controls that this lifecycle framework does not address.

### RND_16 - Parameter Extraction and Tool Calling
- Strength score: 1

The NSA/NCSC Guidelines' input sanitization guidance is directionally relevant to detecting hallucinated parameters, and the behavior monitoring guidance may detect anomalous tool selection patterns. However, the framework provides no specific guidance on stochastic parameter generation creating hallucination windows exploitable by targeting high-temperature agents (RND_16_1), tool documentation drift across agent training cycles creating schema version mismatches (RND_16_2), probabilistic tool selection creating adversarial search spaces where ambiguous requests maximize malicious tool probability (RND_16_3), non-deterministic fallback ordering enabling malicious tool inclusion in fallback chains (RND_16_4), or continuous hallucination rate regression through model updates invalidating grounding calibration (RND_16_5).

### RND_17 - Retrieval-Augmented Generation (RAG) and Multi-Hop QA
- Strength score: 2

The NSA/NCSC Guidelines' explicit recognition of prompt injection and data poisoning as adversarial ML threats is directly applicable to ranking model manipulation through crafted high-relevance documents poisoning multi-agent retrieval (RND_17_1), semantic similarity exploitation in dense retrieval enabling indirect prompt injection through semantically-similar documents (RND_17_2), and query reformulation instruction injection where source document instructions become sub-queries in multi-hop chains (RND_17_3). The framework's input sanitization and adversarial input detection guidance supports addressing evidence document hallucination (RND_17_4) and evidence grounding failures (RND_17_5). However, the framework does not address source attribution reasoning confusion (RND_17_6), knowledge graph hallucination blindness (RND_17_7), or the structural non-determinism of multi-hop RAG chains with specific controls.

### RND_18 - Infrastructure and Deployment Non-Determinism
- Strength score: 2

The NSA/NCSC Guidelines' infrastructure security guidance requiring "appropriate segregation of environments" and "good infrastructure security principles" is applicable to event delivery guarantee degradation through infrastructure resource exhaustion (RND_18_1) and message queue ordering guarantee loss during scaling (RND_18_3). The framework's supply chain security guidance addresses API gateway canary deployment assurance gaps (RND_18_6) and MLflow version mismatch assurance gaps (RND_18_7) through component versioning and verification controls. The monitoring guidance is relevant to detecting vector database index staleness and Prometheus metric lag (RND_18_4, RND_18_5). The framework does not, however, address swarm consensus manipulation via strategic Byzantine agent injection at critical density thresholds (RND_18_2) or the fundamental assurance gap from heterogeneous agent population state combinations.

### RND_19 - Kubernetes and Container Orchestration
- Strength score: 2

The NSA/NCSC Guidelines' infrastructure security guidance requiring "appropriate segregation of environments" and good infrastructure security principles is directly applicable to Kubernetes namespace isolation, pod placement security, and container orchestration hardening. The guidance is applicable to horizontal pod autoscaler scaling non-determinism creating non-deterministic agent populations (RND_19_1), StatefulSet initialization races during restart (RND_19_2), and Kubernetes scheduler indeterminism enabling intermittent malicious pod placement (RND_19_4). However, the framework provides no specific guidance on non-deterministic network topology changes from node autoscaling affecting multi-agent co-location security (RND_19_3), or the fundamental assurance challenge that scheduling non-determinism prevents validation of placement-dependent security properties.

### RND_20 - Deployment and Inference Infrastructure
- Strength score: 2

The NSA/NCSC Guidelines' supply chain security guidance covering SBOM and verified component sources is directly applicable to container image drift in continuous deployment creating version skew across multi-agent systems (RND_20_1) and rolling update non-determinism in version transitions creating compatibility gaps (RND_20_6). The framework's asset tracking guidance is relevant to managing agent version heterogeneity during canary deployments (RND_20_2, RND_20_7). Infrastructure security guidance applies to checkpoint resume non-determinism in serverless workflows (RND_20_5). However, the framework does not address asynchronous event processing ordering non-determinism (RND_20_3) or the assurance challenge from N×(N-1)/2 compatibility combinations during simultaneous rolling deployments (RND_20_4).

### RND_21 - Inference Hardware and Optimization
- Strength score: 1

The NSA/NCSC Guidelines' infrastructure security guidance and asset tracking requirements are tangentially relevant to managing heterogeneous inference hardware. However, the framework provides no specific guidance on quantization granularity non-determinism creating inconsistent agent behavior across different quantization modes (RND_21_1), kernel fusion non-determinism from GPU scheduler variance (RND_21_2), NIM replica floating-point rounding differences enabling probability-based attacks targeting specific execution paths (RND_21_3), dynamic batching indeterminism from request arrival timing (RND_21_4), continuous model update perpetual assurance gaps from zero-downtime transitions (RND_21_5), or hardware-specific TensorRT engine variations creating deployment assurance gaps between development and production hardware (RND_21_6). These hardware-level inference security concerns require specialized controls.

### RND_22 - Optimization and Efficiency
- Strength score: 1

The NSA/NCSC Guidelines do not address profiling-induced non-determinism as assurance violations (RND_22_1), temperature and sampling parameter variability across optimization cycles creating behavioral divergence (RND_22_2), asynchronous configuration updates during multi-agent coordination creating inconsistent operational states (RND_22_3), or speculative decoding acceptance rate variability creating non-deterministic latency and output distributions (RND_22_4). These inference optimization security concerns require ML engineering controls outside this framework's scope.

### RND_23 - Chain-of-Thought (CoT), Tree-of-Thought (ToT), and Reasoning Non-Determinism
- Strength score: 1

The NSA/NCSC Guidelines do not address CoT path explosion in multi-agent coordination where multiple agents generate different reasoning chains creating exponential untestable execution paths (RND_23_1), reasoning consistency drift across independently updated agents (RND_23_2), temporal coordination non-determinism from race conditions in shared CoT trace retrieval (RND_23_3), stochastic multi-agent planning creating computationally infeasible combined state spaces (RND_23_4), or backtracking state divergence in distributed ToT search tree exploration (RND_23_5). These reasoning architecture assurance concerns are outside this framework's scope.

### RND_24 - Self-Consistency and Multiple Reasoning Paths
- Strength score: 1

The NSA/NCSC Guidelines do not address Self-Consistency's intentional stochastic decoding creating assurance gaps (RND_24_1), temperature tuning parameter drift affecting shared multi-agent behavior (RND_24_2), continuous sampling parameter optimization creating adversarial drift (RND_24_3), or context window non-determinism from compression algorithm path selection in multi-agent handoffs (RND_24_4). These sampling-architecture security concerns require probabilistic testing methodology beyond this framework's scope.

### RND_25 - Hierarchical Task Network (HTN) Planning Non-Determinism
- Strength score: 1

The NSA/NCSC Guidelines do not address HTN-specific non-determinism: probabilistic decomposition method selection creating assurance gaps where testing one decomposition path provides no guarantee about production selections (RND_25_1), temperature divergence creating incompatible agent planning strategies (RND_25_2), continual method library evolution breaking decomposition guarantees through version mismatch (RND_25_3), hierarchical planning reproducibility failures (RND_25_4), partial order scheduling non-determinism (RND_25_5), or constraint satisfaction heuristic divergence (RND_25_6). These HTN planning assurance concerns are outside the framework's scope.

### RND_26 - Monte Carlo Tree Search (MCTS) Planning Non-Determinism
- Strength score: 1

The NSA/NCSC Guidelines do not address MCTS-specific non-determinism: random seed-dependent tree divergence causing conflicting multi-agent plans (RND_26_1), dynamic MCTS adaptation creating heterogeneous exploration strategies across agents (RND_26_2), simulation budget variance as assurance gap (RND_26_3), replanning non-determinism from different random seeds in identical failure states enabling targeted dangerous path activation (RND_26_4), asynchronous replanning state divergence (RND_26_5), or heuristic evaluation inconsistency across agents (RND_26_6). These MCTS planning security concerns are outside this framework's scope.

### RND_27 - Episode-Based Memory and Consolidation
- Strength score: 1

The NSA/NCSC Guidelines' data poisoning recognition is tangentially applicable to retrieval non-determinism enabling malicious episode activation, and behavior monitoring may detect anomalous retrieval patterns. However, the framework provides no specific guidance on non-deterministic episode retrieval ranking enabling adaptive attacks calibrated to unpredictable retrieval (RND_27_1), consolidation timing non-determinism exploitable for attack escalation during specific windows (RND_27_2), retrieval threshold sensitivity as assurance gap enabling context-specific poisoning (RND_27_3), trajectory length variability creating hidden attack activation paths (RND_27_4), embedding model update non-determinism creating transient poisoning windows during staged rollouts (RND_27_5), or graph traversal path non-determinism enabling selective relationship poisoning (RND_27_6).

### RND_28 - Knowledge Graphs and Memory Stores
- Strength score: 1

The NSA/NCSC Guidelines do not address knowledge graph and memory store non-determinism: RAG result ranking non-determinism from score rounding creating correlated uncertainty across all agents sharing a ranking function (RND_28_1), knowledge graph traversal ordering non-determinism producing different reasoning paths (RND_28_2), temporal validity window boundary intermittent visibility (RND_28_3), probabilistic fact evaluation non-determinism enabling attacker leverage of variance (RND_28_4), incremental update non-determinism from partial-update query windows (RND_28_5), or caching invalidation timing non-determinism creating semantic divergence (RND_28_6). These distributed knowledge store consistency concerns are outside this framework's scope.

### RND_29 - Context Assembly and Working Memory
- Strength score: 1

The NSA/NCSC Guidelines do not address multi-agent context assembly non-determinism from variable retrieval ranking and asynchronous retrieval order creating unreproducible security auditing (RND_29_1), streaming generation non-determinism across agent handoffs creating untestable malicious token sequence variations (RND_29_2), or continual update propagation creating distributed regression gaps where heterogeneous agent version states produce emergent behaviors probed by attackers during version boundaries (RND_29_3). Context assembly non-determinism as a security property is outside this framework's scope.

### RND_30 - Utility and Decision Making
- Strength score: 1

The NSA/NCSC Guidelines do not address utility calculation non-determinism in agentic decision-making: probabilistic sampling variability in expected utility calculation enabling divergent multi-agent decisions (RND_30_1), stochastic outcome distribution changes causing cached utility values to become invalid while agents continue using stale calculations (RND_30_2), non-deterministic weight adjustment mechanisms creating unsafe utility function divergence across agents (RND_30_3), or non-reproducible testing for utility-based decisions where safe test distributions fail to bound production behavior (RND_30_4). These decision-theoretic security concerns require specialized controls outside this framework's scope.

### RND_31 - Rule-Based and Adaptive Systems
- Strength score: 1

The NSA/NCSC Guidelines do not address rule priority instability creating non-deterministic execution in multi-agent systems with tunable priorities (RND_31_1), learning rule instability in adaptive systems where shared rule learning creates non-deterministic behavior (RND_31_2), or heuristic parameter drift from independent agent tuning creating behavioral non-determinism (RND_31_3). These adaptive rule system assurance concerns are outside this framework's scope.

### RND_32 - Learning-Based Policies and Reinforcement Learning
- Strength score: 2

The NSA/NCSC Guidelines' explicit recognition of data poisoning as a known adversarial ML threat is directly applicable to experience replay temporal non-determinism enabling poisoning through timing-dependent context (RND_32_4) and gradient-based adversarial policy perturbations creating synchronized vulnerabilities across shared-training agents (RND_32_7). The framework's recognition that training data defines system behavior applies to online learning continual change defeating validation as policies drift (RND_32_2) and distributed training heterogeneous policy production (RND_32_6). The behavior monitoring guidance is relevant to detecting anomalous exploration behavior in deployed RL agents (RND_32_3). However, the framework does not address learned policy non-determinism as an inherent assurance gap (RND_32_1), multi-agent RL emergent behavior unpredictability (RND_32_5), or exploration unpredictability with specific controls.

### RND_33 - Hybrid and Heterogeneous Systems
- Strength score: 1

The NSA/NCSC Guidelines do not address temperature heterogeneity in hybrid paradigm processing enabling targeted injection to high-temperature agents (RND_33_1), cooperative cycle non-determinism in hybrid architectures enabling activation-count-specific attacks (RND_33_2), hybrid output synthesis streaming non-determinism from variable paradigm output ordering (RND_33_3), knowledge graph consistency assurance gaps during multi-agent evolution (RND_33_4), or feedback loop timing non-determinism forcing dangerous convergence paths (RND_33_5). These hybrid AI architecture assurance concerns require cross-paradigm security engineering beyond this framework's scope.

### RND_34 - Other Risks/Threats/Vulnerabilities Worth Noting
- Strength score: 1

The NSA/NCSC Guidelines do not address the extensive range of infrastructure and ETL non-determinism concerns catalogued in RND_34: keyboard navigation testing fragility from DOM manipulation and asynchronous rendering (RND_34_1), HITL approval workflow non-reproducibility from confidence score variance and adaptive thresholds (RND_34_2), auto-scaling replica configuration non-determinism (RND_34_3), batching timeout and load balancing algorithm non-determinism (RND_34_4, RND_34_5), cache invalidation temporal non-determinism (RND_34_6), temperature-dependent evaluation instability (RND_34_7), framework version change evaluation instability (RND_34_8), CI/CD timing variations affecting timeout-based evaluations (RND_34_9), evaluation workflow state machine non-determinism (RND_34_10), or the ETL-specific non-determinism concerns including semantic chunking boundary variation across implementations (RND_34_14), deduplication hash collision race conditions in concurrent multi-agent extraction (RND_34_15), REST API pagination cursor non-determinism during concurrent extraction (RND_34_16), filesystem modification time resolution variability across platform heterogeneity (RND_34_17), batch subdivision ordering non-determinism during partial failure recovery (RND_34_18), and quality metric calculation variability across heterogeneous validation implementations (RND_34_19). These operational and ETL infrastructure non-determinism concerns require specialized engineering controls outside this framework's scope.


---

## RTM - Telemetry and monitoring blind spots specific to cognitive and tool behavior

### RTM_1 - UI and Interface Telemetry Gaps
- Strength score: 2

The NSA/NCSC Guidelines explicitly require "monitoring your system's inputs and outputs to enable prompt response and investigation and remediation in the case of compromise" and require observing "sudden and gradual changes in behaviour affecting security." These requirements directly call for the kind of monitoring that RTM_1 identifies as missing: the framework mandates that monitoring exist, making the blind spots catalogued here failures against the framework's own requirements. The input monitoring guidance requiring adversarial input detection is applicable to RTM_1_7 (auto-retry amplification), RTM_1_10 (tool invocation parameter semantic analysis), and RTM_1_12 (session restoration integrity verification). The access control guidance is relevant to RTM_1_8 (attribution telemetry for impersonation detection). However, the framework provides no prescriptive guidance on semantic logging capturing decision graphs (RTM_1_2), streaming response telemetry collection strategies (RTM_1_3), confidence score decomposition in multi-agent aggregation (RTM_1_11), or cross-session state poisoning indicator tracking (RTM_1_7) — the specific monitoring architectures needed to close these blind spots.

### RTM_2 - Framework-Specific Architecture and Logging Gaps
- Strength score: 2

The NSA/NCSC Guidelines' monitoring requirements mandate system output observation for security, which is directly applicable to the framework-specific logging gaps creating observability dead zones. The supply chain security guidance requiring documentation of AI system components and tracking AI assets is relevant to RTM_2_6 (framework architecture documentation gaps enabling attack reconnaissance) — the framework's requirement for asset documentation supports closing reconnaissance opportunities. The access control guidance applies to protecting audit trails against RTM_2_7 (checkpoint-mediated state mutations evading audit trails). However, the framework provides no specific guidance on multi-tier Plan-and-Execute opacity (RTM_2_1), cross-framework integration monitoring gaps (RTM_2_5), correlation ID manipulation in distributed tracing (RTM_2_3), or the framework-specific telemetry blind spots for LangChain/AutoGen/CrewAI/Semantic Kernel (RTM_2_10 through RTM_2_26).

### RTM_3 - Tool Invocation and Function Calling Monitoring
- Strength score: 2

The NSA/NCSC Guidelines' explicit monitoring requirement to observe system outputs and detect adversarial inputs is directly applicable to tool invocation monitoring requirements, as tool invocations are primary outputs whose patterns indicate attack activity. The guidelines' access control guidance supports RTM_3_5 (authorization scope audit — agents should not invoke tools without appropriate authorization), and the least privilege guidance directly addresses the privilege escalation via tool chaining (RTM_3_2) by limiting what tools agents can access. However, the framework provides no specific guidance on distributed tool invocation correlation to detect cross-agent attack chains that appear benign individually (RTM_3_1, RTM_3_2), multi-agent parallel tool execution masking per-tool latency anomalies (RTM_3_3), inter-agent output validation gaps at handoff points (RTM_3_4), or asynchronous causality ambiguity in tool invocation attribution (RTM_3_6).

### RTM_4 - Multimodal and Streaming Response Processing
- Strength score: 1

The NSA/NCSC Guidelines' monitoring requirements and adversarial input detection guidance are applicable to multimodal content monitoring, and the prompt injection threat recognition supports detecting RTM_4_2 (multimodal attribution opacity enabling image-embedded injection). However, the framework provides no specific guidance on vision model processing opacity creating monitoring blind spots (RTM_4_1), hallucination detection in vision model outputs (RTM_4_3), embedding similarity threshold opacity in retrieval monitoring (RTM_4_4), streaming multimodal output intermediate state gaps (RTM_4_5), cross-modal consistency validation absence (RTM_4_6), streaming telemetry collection temporal lag (RTM_4_14), or the extensive range of streaming error handling and retry telemetry blind spots (RTM_4_7 through RTM_4_18). These multimodal and streaming observability gaps require specialized monitoring architectures not addressed by this framework.

### RTM_5 - Evaluation and Assessment Telemetry Gaps
- Strength score: 2

The NSA/NCSC Guidelines mention responsible release including benchmarking and red teaming, implying evaluation pipelines exist and their integrity matters for security assurance. The framework's behavior monitoring guidance requiring observation of "sudden and gradual changes in behaviour" is applicable to detecting evaluation result anomalies indicating injection or manipulation (RTM_5_1, RTM_5_9, RTM_5_13). The access control guidance applies to protecting evaluation datasets and audit logs (RTM_5_7, RTM_5_12), and the monitoring requirements support evaluation agent behavior baseline establishment (RTM_5_8). However, the framework provides no specific guidance on evaluation agent reasoning telemetry (RTM_5_2), cross-agent evaluation orchestration monitoring (RTM_5_3), evaluation metric calculation opacity (RTM_5_4), or distributed evaluation audit trail completeness requirements (RTM_5_6, RTM_5_15, RTM_5_16).

### RTM_6 - Metrics Collection and Manipulation Evasion
- Strength score: 2

The NSA/NCSC Guidelines' behavior monitoring requirement to observe "gradual changes in behaviour affecting security" is directly applicable to the gradual degradation evasion attacks catalogued in RTM_6: historical baseline staleness (RTM_6_30), gradual efficiency baseline poisoning (RTM_6_28), and coordinated alert threshold evasion across agents (RTM_6_13, RTM_6_29). The incident management guidance supports establishing response capabilities for metric manipulation attacks (RTM_6_31). The framework's monitoring requirement implies that metric collection integrity is a security prerequisite. However, the framework provides no specific guidance on confidence score tuning for monitoring evasion (RTM_6_4), distributed tool invocation obfuscation via log aggregation timing (RTM_6_7), cross-agent tool selection pattern monitoring (RTM_6_10), entropy tracking evasion through confidence aggregation (RTM_6_11), or the extensive range of per-agent versus aggregate metric attribution challenges catalogued in RTM_6 (RTM_6_14 through RTM_6_32).

### RTM_7 - Infrastructure and Observability Stack Blind Spots
- Strength score: 2

The NSA/NCSC Guidelines' infrastructure security guidance requiring "good infrastructure security principles" and the monitoring requirements are directly applicable to protecting the observability stack from manipulation: metric transport security (RTM_12_5 conceptually addressed by infrastructure security guidance), audit log write integrity (RTM_7_22), and container registry audit security (RTM_7_23). The framework's supply chain security guidance addresses container image drift creating version skew blind spots (RTM_7_23). The infrastructure security and access control guidance applies to protecting Prometheus, OpenTelemetry, and distributed tracing infrastructure from manipulation. However, the framework provides no specific guidance on the extensive range of multi-agent-specific observability challenges: message queue consumer lag hiding agent failures (RTM_7_2), vector database semantic drift invisible in latency metrics (RTM_7_3), alert fatigue from pod-level metric volumes (RTM_7_12), speculative decoding token opacity (RTM_7_29), quantization artifact telemetry absence (RTM_7_38), or GPU-level DCGM telemetry exposure enabling attack calibration (RTM_12_8).

### RTM_8 - Reasoning and Cognitive State Monitoring
- Strength score: 2

The NSA/NCSC Guidelines explicitly require behavior monitoring for "sudden and gradual changes in behaviour affecting security," which is directly applicable to detecting reasoning quality drift in production (RTM_6_21) and policy drift in RL systems (RTM_8_23). The framework's requirement to monitor inputs and outputs for adversarial content applies to reasoning trace injection detection (RTM_8_1). The monitoring guidance supports detecting confidence score manipulation visible in quality score metric blind spots (RTM_8_9). The access control guidance is applicable to protecting preserved reasoning paths (RTM_8_7). However, the framework provides no specific guidance on the deep cognitive state monitoring blind spots that RTM_8 catalogues: coordination intent invisibility in distributed reasoning (RTM_8_2), reasoning state obfuscation through structural variation (RTM_8_3), meta-reasoning blind spots with recursive concealment (RTM_8_4), MCTS tree statistics as unmonitored cognitive state (RTM_8_21), or the extensive range of planning and decomposition trace opacity issues (RTM_8_14 through RTM_8_28).

### RTM_9 - Memory Systems and Knowledge Base Observability
- Strength score: 2

The NSA/NCSC Guidelines' access control guidance requiring management of "what data AI systems can access" is directly applicable to knowledge base access attribution requirements (RTM_9_5, RTM_9_7), and the data poisoning threat recognition supports the requirement for knowledge base integrity monitoring (RTM_9_11, RTM_9_12). The monitoring guidance requiring behavioral observation applies to detecting episodic memory retrieval influence on agent behavior (RTM_9_1). The framework's requirement to sanitize continuous learning data supports consolidation process monitoring (RTM_9_2). However, the framework provides no specific guidance on trajectory integration hiding multi-step attacks (RTM_9_3), hybrid storage architecture monitoring asymmetry (RTM_9_4), metadata filtering complexity as detection evasion (RTM_9_6), temporal validity enforcement blind spots (RTM_9_10), token budget visualization false confidence (RTM_9_16), or hierarchical compression semantic monitoring failures (RTM_9_18).

### RTM_10 - Decision Logic and Utility Function Monitoring
- Strength score: 1

The NSA/NCSC Guidelines' behavior monitoring guidance is broadly applicable to detecting decision logic anomalies, and the framework's recognition of data poisoning as a threat is relevant to utility function and reward metric poisoning (RTM_10_13). However, the framework provides no specific guidance on utility function calculation telemetry enabling poisoning detection (RTM_10_1), weight-driven decision divergence blind spots (RTM_10_2), specification gaming through utility metric redefinition (RTM_10_4), rule firing monitoring gaps in distributed systems (RTM_10_8), policy output distribution analysis (RTM_10_15), gradient flow monitoring in federated learning (RTM_10_16), learned monitoring evasion through training-based counter-strategies (RTM_10_12), or the extensive range of decision logic transparency requirements catalogued across this subsubsection. These internal decision logic observability concerns require ML-specific interpretability and monitoring tools outside this framework's scope.

### RTM_11 - Vector Database and RAG Pipeline Telemetry
- Strength score: 2

The NSA/NCSC Guidelines' monitoring requirements mandate system output observation for security, which applies directly to RAG pipeline output monitoring where retrieval quality determines agent behavior quality. The framework's data access controls and supply chain security guidance is relevant to ETL state file modification tracking (RTM_11_15), per-source quality failure attribution (RTM_11_5), and vendor-specific data provenance monitoring (RTM_11_8). The behavior monitoring guidance supports detecting embedding quality gradual degradation (RTM_9_11 equivalent) and cache performance manipulation (RTM_11_9). However, the framework provides no specific guidance on retrieval quality metric blind spots in Prometheus (RTM_11_1), hybrid search alpha parameter selection opacity (RTM_11_2), shard-level query performance attribution gaps (RTM_11_3), or the extensive range of ETL quality monitoring blind spots for batch ingestion (RTM_11_4), deduplication effectiveness (RTM_11_10), and per-agent validation failure distribution patterns (RTM_11_13, RTM_11_14).

### RTM_12 - Detection Evasion and Attack Exploitation
- Strength score: 2

The NSA/NCSC Guidelines' monitoring requirements and incident management guidance are directly applicable to the detection evasion threats catalogued in RTM_12: the framework's requirement to monitor for adversarial inputs and respond to incidents supports establishing the detection baselines that evasion attacks circumvent. The infrastructure security guidance requires protecting observability infrastructure, directly applicable to RTM_12_5 (Prometheus metrics collection manipulation through man-in-the-middle attacks). The framework's incident management guidance is relevant to RTM_12_4 (incident response time attack through detection window exploitation). The access control guidance applies to RTM_12_11 (distributed sandbox audit trail fragmentation) and RTM_12_12 (fairness audit trail fragmentation for compliance). However, the framework provides no specific guidance on alert fatigue exploitation through false positive flooding (RTM_12_2), metric reporting delay manipulation (RTM_12_3), distributed Prometheus alerting rule threshold gaming (RTM_12_6), coordinated safety violation correlation blindness across agents (RTM_12_10), or the cross-agent traceability requirements for regulatory compliance (RTM_12_17).


## RTE - Multi-agent trust exploitation and self-replicating prompt malware

### RTE_1 - Dashboard & UI Attribution Attacks
- Strength score: 1

The NSA/NCSC Guidelines and JCDC Playbook do not address dashboard UI design, agent identity attribution in multi-agent visualizations, or the cryptographic binding requirements between agent identity claims and visual representations. RTE_1's threats—dashboard attribution spoofing through visual similarity, message source field manipulation, and evaluation dashboard attribution spoofing—all depend on the fundamental gap between cosmetic agent indicators and cryptographically verified identity. The framework's guidance on authentication and cryptographic model integrity is infrastructure-level and does not extend to the application-layer concern of how multi-agent dashboards authenticate and display the provenance of agent-generated content. The framework provides no compensating controls for this class of attack.

### RTE_2 - Trust Mechanisms & Inter-Agent Communication
- Strength score: 1

RTE_2 describes trust exploitation that arises specifically because agents treat natural language inter-agent messages as trusted analysis rather than untrusted peer data—a structural property of agentic systems absent from traditional distributed systems. The NSA/NCSC Guidelines advocate for authentication and access controls in AI deployments but do not address the unique trust mechanics of agent-to-agent natural language communication, transitive trust via reputation anchoring, circular verification loop exploitation, or domain-specialization trust collapse. The framework's access control guidance assumes protocol-based authentication, not the unstructured trust that emerges from language-mediated agent interactions. No meaningful coverage exists for RTE_2_1 through RTE_2_4.

### RTE_3 - Confidence & Scoring Attacks
- Strength score: 1

RTE_3's attacks exploit confidence scores as a routing and trust signal in multi-agent pipelines—a mechanism through which parameter tuning, utility function poisoning, and streaming timing can inflate the apparent certainty of compromised agent outputs. The NSA/NCSC Guidelines contain no guidance on confidence score integrity, the use of confidence as a trust proxy in multi-agent routing decisions, or defenses against the particular attack vectors described (temperature manipulation, utility function poisoning, streaming progressive revelation exploitation). While the framework recommends input sanitization and guardrails in general terms, confidence score manipulation is an internal model signal, not an external input boundary, placing it outside the framework's scope.

### RTE_4 - Prompt Injection via Message/History Sharing
- Strength score: 2

The NSA/NCSC Guidelines explicitly identify prompt injection as a known threat category requiring input sanitization and guardrails, stating that AI systems must recognize and resist prompt injection attacks. This provides direct coverage for the injection vector itself across RTE_4_1 through RTE_4_3. However, the framework's guidance is scoped to single-point injection defenses and does not address the multi-agent propagation dimension that makes these threats distinct: self-replication through shared conversation history, geometric propagation across agents deserializing shared persisted memory, and worm-like spread through AutoGen GroupChat history. The framework cannot account for the epidemiological amplification that shared context enables in multi-agent architectures, leaving the propagation and persistence dimensions of RTE_4 unaddressed.

### RTE_5 - UI & Disclosure Vulnerabilities
- Strength score: 1

RTE_5 addresses exploitation of progressive disclosure UI patterns and dynamic agent role assignment in multi-agent dashboards. Neither the NSA/NCSC Guidelines nor the JCDC Playbook contains guidance on UI/UX security design, progressive disclosure layer security, or the vulnerability introduced by dynamic role assignment in orchestration agents. The framework's scope is the AI system lifecycle—development, deployment, and operations—but does not extend to interface design patterns or the security implications of dashboard role labeling for dynamically assigned agents. No meaningful framework coverage applies to RTE_5_1 or RTE_5_2.

### RTE_6 - Tool & Command Injection
- Strength score: 2

The NSA/NCSC Guidelines address input sanitization requirements and identify the risk of malicious content influencing AI system behavior, which provides indirect coverage for the injection component of RTE_6's attacks. Inline suggestion UIs displaying agent-generated recommendations that contain embedded malicious instructions (RTE_6_1) and command palette context poisoning (RTE_6_2) both exploit insufficient input validation at the boundary where agent-generated content is treated as trusted. The framework's requirement to sanitize inputs and maintain guardrails around model outputs applies here, though the framework does not specifically address the UI-level contexts described (code editor suggestions, command palettes) or the social engineering dynamic where visual prominence encourages rapid user acceptance without scrutiny of embedded instructions.

### RTE_7 - Framework-Specific Attacks
- Strength score: 1

RTE_7 catalogs trust exploitation specific to the delegation models of LangChain, LangGraph, AutoGen, CrewAI, and Semantic Kernel—each with distinct attack surfaces arising from how the framework implements agent-to-agent trust and message passing. The NSA/NCSC Guidelines predate widespread agentic framework deployment and contain no guidance on any of these frameworks' specific trust architectures, state-passing mechanisms, conversational delegation patterns, hierarchical task delegation, or plugin routing. The framework's general guidance on threat modeling and secure development is too abstract to address the framework-specific injection points described in RTE_7_1 through RTE_7_3. Organizations using these frameworks receive no actionable framework-specific security guidance from NSA/NCSC.

### RTE_8 - AutoGen-Specific Attacks
- Strength score: 1

AutoGen's conversational agent model enables AI-to-AI social engineering (RTE_8_1) and reputation exploitation (RTE_8_2) through mechanisms entirely absent from the NSA/NCSC Guidelines' scope. The framework does not address conversational trust negotiation between agents, reputation systems built through dialogue history, or the exploitation of natural language persuasion between peer agents. The guidelines' authentication controls assume protocol-level identity verification, not the conversational context-based trust that AutoGen's architecture creates. No framework guidance applies to these AutoGen-specific threat vectors.

### RTE_9 - CrewAI-Specific Attacks
- Strength score: 1

CrewAI's hierarchical manager-worker architecture creates trust relationships that RTE_9 exploits through manager-worker trust exploitation (RTE_9_1), poisoned few-shot decomposition demonstrations (RTE_9_2), and demonstration bias enabling semantic subtask reinterpretation (RTE_9_3). The NSA/NCSC Guidelines contain no guidance on hierarchical agent trust, task delegation security, few-shot demonstration integrity in production deployments, or the specific risks of biased demonstrations teaching attack-enabling subtask interpretations. The framework's guidance on training data sanitization has marginal relevance to demonstration poisoning in inference-time CrewAI deployments, but the conceptual gap is too large to constitute meaningful coverage.

### RTE_10 - Semantic Kernel-Specific Attacks
- Strength score: 1

Semantic Kernel's dynamic plugin registration model creates the attack surfaces described in RTE_10: plugin registry takeover enabling substitution attacks where malicious plugins with legitimate names execute before legitimate ones (RTE_10_1), and function impersonation through schema duplication exploiting probabilistic LLM routing (RTE_10_2). The NSA/NCSC Guidelines do not address plugin architectures, dynamic function registration security, or the LLM function selection mechanism that makes near-identical plugin descriptions an exploitable attack vector. The framework's supply chain guidance provides no specific coverage for runtime plugin injection as distinct from build-time supply chain compromise.

### RTE_11 - Multimodal Attacks
- Strength score: 1

RTE_11 identifies multimodal-specific attack vectors in multi-agent systems: attribution spoofing in collaboration displays, self-replicating image injection through accepted user suggestions, cross-agent modality contradiction attacks manufacturing false consensus, and multimodal worm propagation through RAG retrieval cycles where poisoned images bypass text-based defenses. The NSA/NCSC Guidelines contain no guidance on multimodal agent systems, cross-modal trust relationships, modality-weighted aggregation exploitation, or visual trigger embedding for instruction hallucination. The framework's input sanitization guidance does not extend to multimodal content validation or the specific risks of image-embedded instruction propagation.

### RTE_12 - Streaming & Continuous Processing
- Strength score: 1

RTE_12 describes how operational communication channels in multi-agent systems—retry failure messages (RTE_12_1), fallback routing announcements (RTE_12_2), and circuit breaker state transition announcements (RTE_12_3)—become malware propagation channels when downstream agents treat these operational messages as trusted instructions. The NSA/NCSC Guidelines address monitoring and incident management but not the security of inter-agent operational coordination messages or the vulnerability created when failure/fallback messaging carries implicit trust. The framework has no guidance on circuit breaker security patterns or the trust implications of streaming operational state announcements in multi-agent architectures.

### RTE_13 - Evaluation & Benchmarking
- Strength score: 2

The NSA/NCSC Guidelines contain guidance on responsible release practices including red teaming and adversarial testing, and the supply chain security controls (SBOM, dependency scanning) directly apply to RTE_13_3's evaluation framework version poisoning through dependency manipulation. The framework's general adversarial testing guidance also provides indirect coverage for identifying metric manipulation and benchmark integrity issues through pre-deployment evaluation. However, the framework does not address multi-agent evaluation-specific threats: self-replicating evaluation metric definitions propagating through evaluation agent networks (RTE_13_1), transitive trust exploitation in evaluation chains (RTE_13_2), HTML/JavaScript injection into custom evaluation reports (RTE_13_4), coordinated false consensus across all agents (RTE_13_5), or benchmark leaderboard manipulation for agent reputation spoofing (RTE_13_6, RTE_13_7).

### RTE_14 - Web & LLM Evaluation Attacks
- Strength score: 1

RTE_14_1 describes prompt injection attacks in multi-hop QA decomposition pipelines where malicious instructions embedded in early sub-question answers propagate through sequential agent execution. The NSA/NCSC Guidelines identify prompt injection as a threat requiring sanitization, which provides the baseline conceptual coverage, but the framework does not address decomposed query architectures, multi-hop pipeline trust chains, or the specific propagation mechanism where early-stage injection influences later-stage agent execution. The multi-agent sequential trust chain amplification is not addressed.

### RTE_15 - Model Tuning & Configuration Attacks
- Strength score: 1

RTE_15 describes exploitation of model configuration parameters as attack vectors in multi-agent trust contexts: leveraging model size assumptions to evade attribution of sophisticated attacks (RTE_15_1), prompt cache poisoning for malware persistence across requests (RTE_15_2), iteration budget exploitation creating reasoning gaps that allow injection detection bypass (RTE_15_3), and adaptive routing configuration manipulation for transitive trust exploitation (RTE_15_4). The NSA/NCSC Guidelines contain no guidance on model size trust assumptions, prompt cache security, iteration budget configuration as a security control, or router model security in multi-agent deployments. These attacks exploit agentic configuration parameters the framework does not contemplate.

### RTE_16 - Reasoning Trace & Chain-of-Thought Attacks
- Strength score: 1

RTE_16 identifies chain-of-thought reasoning traces as both attack delivery mechanisms and malware propagation vectors: social engineering embedded within CoT reasoning presented as legitimate analysis (RTE_16_1), and malware woven into CoT explanations that agents naturally retrieve and incorporate from shared knowledge (RTE_16_2). The NSA/NCSC Guidelines contain no guidance on chain-of-thought reasoning security, the integrity of reasoning traces shared across agents, or defenses against manipulation embedded within reasoning artifacts. The framework's monitoring guidance does not extend to reasoning trace analysis for embedded malicious content.

### RTE_17 - Tree of Thought & Sampling Attacks
- Strength score: 1

Tree-of-thought and sampling-based reasoning architectures create the attack surfaces in RTE_17: quality score consensus enabling trust exploitation of compromised high-scoring paths (RTE_17_1), sampling diversity exploitation where diversity itself validates apparent legitimacy (RTE_17_2), majority voting manipulation for consensus building (RTE_17_3), and quality metric inflation as credential spoofing (RTE_17_4). The NSA/NCSC Guidelines do not address tree-of-thought architectures, self-consistency sampling, RASC-style weighted voting systems, or the specific vulnerabilities introduced by diversity-based reasoning approaches. These are emergent AI reasoning paradigm vulnerabilities outside the framework's scope.

### RTE_18 - Hierarchical Task Network (HTN) & Planning Attacks
- Strength score: 1

HTN hierarchical decomposition creates authority levels that RTE_18 exploits through hierarchical authority confusion enabling privilege confusion attacks (RTE_18_1), shared method library poisoning through collaborative method injection (RTE_18_2), and decomposition delegation chain authority diffusion (RTE_18_3). The NSA/NCSC Guidelines address least privilege and access control conceptually but do not address HTN-specific authority hierarchies, shared planning method libraries, or the authority diffusion that emerges when decomposition chains distribute responsibility across agents. The framework has no guidance on securing planning architectures or validating authority claims in hierarchical decomposition systems.

### RTE_19 - Monte Carlo Tree Search (MCTS) Attacks
- Strength score: 1

MCTS-specific attacks in RTE_19 target non-deterministic rollout exploitation for emergent malicious behavior (RTE_19_1), shared value network backpropagation poisoning (RTE_19_2), and hierarchical MCTS delegation as a privilege escalation path (RTE_19_3). The NSA/NCSC Guidelines contain no guidance on MCTS architectures, stochastic planning security, shared reward signal integrity, or the specific trust assumption exploited when supervisors treat MCTS expansion as appropriately decomposed subtasks. These are specialized AI planning algorithm vulnerabilities the framework does not address.

### RTE_20 - Multi-Agent Planning Attacks
- Strength score: 1

RTE_20 attacks target plan-and-execute architectures through planning phase reasoning falsification (RTE_20_1), reasoning consistency loss across hierarchical boundaries (RTE_20_2), delegation context reasoning injection where manager reasoning becomes worker context containing embedded instructions (RTE_20_3), and hierarchical trust poisoning through semantic gaps in worker-supervisor communication (RTE_20_4). The NSA/NCSC Guidelines do not address plan-and-execute agentic architectures, the security of planning phase outputs as downstream execution drivers, or the trust relationship between manager and worker agents in hierarchical AI deployments. No framework guidance applies.

### RTE_21 - Memory & Knowledge Attacks
- Strength score: 1

RTE_21 exploits shared episodic memory as a multi-agent malware propagation bridge (RTE_21_1), trajectory-based policy convergence for coordinated attack coordination (RTE_21_2), memory abstraction as attack template generation for newly spawned agents inheriting poisoned templates (RTE_21_3), shared vector database embedding poisoning for unanimous retrieval corruption (RTE_21_4), and consolidation-driven abstraction creating modular malware components (RTE_21_5). The NSA/NCSC Guidelines do not address episodic memory systems, shared vector database security beyond general access controls, agent spawning with inherited knowledge, or the genetic-malware propagation enabled by memory abstraction. These are agentic-architecture-specific vulnerabilities outside the framework's scope.

### RTE_22 - Knowledge Base & RAG Attacks
- Strength score: 2

The NSA/NCSC Guidelines' supply chain security controls and input sanitization requirements provide indirect coverage for RAG pipeline poisoning threats in RTE_22. The framework's emphasis on validating and sanitizing data used by AI systems is conceptually applicable to shared knowledge base poisoning (RTE_22_1) and iterative retrieval amplification (RTE_22_2), where the knowledge base functions as a shared data source whose integrity must be maintained. Supply chain controls help address the provenance of knowledge base content. However, the framework provides no specific guidance on RAG architecture security, cross-document instruction assembly through fragment aggregation (RTE_22_3), synonym injection enabling semantic similarity exploitation (RTE_22_4), or the multi-agent amplification where one poisoned document reaches all retrieving agents simultaneously.

### RTE_23 - Shared Context & Aggregation
- Strength score: 1

RTE_23 exploits architectural properties of multi-agent coordination: shared working memory buffer injection that persists and amplifies across all agents accessing the buffer (RTE_23_1), hierarchical aggregation authority diffusion enabling leaf-level social engineering to propagate to top-level decisions (RTE_23_2), and selective context sharing creating information asymmetry that receiving agents cannot detect (RTE_23_3). The NSA/NCSC Guidelines contain no guidance on shared working memory pool security, hierarchical aggregation trust models, or the information privilege escalation vulnerabilities created by selective context sharing in multi-agent systems. These are structural multi-agent coordination vulnerabilities the framework does not contemplate.

### RTE_24 - Utility & Preference Attacks
- Strength score: 1

Preference convergence attacks (RTE_24_1) exploit shared learning infrastructure to gradually poison all agents' utility weights through a common corpus, while recommendation chain poisoning (RTE_24_2) manipulates utility-weighted systems by compromising upstream relevance or diversity assessments. The NSA/NCSC Guidelines contain no guidance on utility function security, shared learning corpus integrity for preference formation, or the transitive trust exploitation that occurs when downstream agents accept upstream utility-weighted recommendations without independent re-evaluation. The framework's training data sanitization guidance has minimal applicability to inference-time utility function manipulation.

### RTE_25 - Multi-Agent Reinforcement Learning (MARL)
- Strength score: 1

MARL-specific threats in RTE_25 include learned coordination as a social engineering vector where agents develop implicit malicious agreements through joint reward optimization (RTE_25_1), self-replicating reward signal injection via shared experience replay buffers (RTE_25_2), consensus learning manipulation through synchronized policy poisoning (RTE_25_3), learned message protocol exploitation embedding malicious conventions (RTE_25_4), and emergent malicious behavior arising from poisoned reward structures (RTE_25_5). The NSA/NCSC Guidelines contain no guidance on MARL systems, multi-agent experience sharing security, reward signal integrity, or the emergent behavior risks specific to joint policy learning. These are advanced AI training paradigm vulnerabilities outside the framework's scope entirely.

### RTE_26 - Hybrid System Attacks
- Strength score: 1

RTE_26 exploits paradigm boundaries in hybrid AI systems where instructions appearing as data in one paradigm (neural, symbolic, utility-function-based) become executable in another when transiting agent boundaries (RTE_26_1), and knowledge graph topological backdoors creating covert coordination channels through seemingly unrelated entity relationships (RTE_26_2). The NSA/NCSC Guidelines contain no guidance on hybrid AI system architectures, cross-paradigm instruction encoding risks, or knowledge graph security. The framework's scope does not extend to these specialized attack vectors.

### RTE_27 - Infrastructure & Deployment Attacks
- Strength score: 2

The NSA/NCSC Guidelines' infrastructure security guidance—emphasizing network segmentation, least privilege access controls, TLS-based authentication, and monitoring—provides indirect coverage for several RTE_27 threats. Prometheus relabeling configuration injection (RTE_27_2) and MLflow experiment metadata injection (RTE_27_4) are mitigated by the framework's requirements for access controls on monitoring and logging infrastructure and integrity of operational data. API gateway rate limiting bypass through cooperative request distribution (RTE_27_3) relates to the framework's guidance on monitoring for anomalous behavior. However, the framework provides no specific guidance on vector database multi-tenancy security (RTE_27_1), Prometheus relabeling rule integrity specifically, or the multi-agent amplification that makes single-point monitoring infrastructure compromise affect the entire agent fleet.

### RTE_28 - Microservices & Kubernetes
- Strength score: 2

The NSA/NCSC Guidelines explicitly address infrastructure hardening, TLS-based authentication, access controls, and least privilege principles—controls directly applicable to several RTE_28 threats. mTLS downgrade attacks (RTE_28_1), service mesh authorization policy misconfiguration (RTE_28_2), shared service account impersonation (RTE_28_3), service account token reuse (RTE_28_4), overly permissive RBAC (RTE_28_5), mTLS certificate poisoning (RTE_28_6), and image signature spoofing (RTE_28_7) are all infrastructure security failures that the framework's general guidance on hardening, authentication, and access controls addresses in principle. The limitation is that the framework provides no Kubernetes-specific guidance, no service mesh security specifics, and no multi-agent context for how single Kubernetes infrastructure compromise propagates across an entire agent fleet through shared service accounts and registries.

### RTE_29 - Performance Optimization & Model Registry
- Strength score: 2

The NSA/NCSC Guidelines' supply chain security controls—including cryptographic model integrity verification (hashes and signatures), SBOM maintenance, and provenance tracking—directly address model registry version selection manipulation (RTE_29_2), where attackers inject malicious model versions matching semantic version constraints. The framework's requirement to verify model integrity before deployment provides a compensating control for this threat. Performance optimization recommendation propagation as a malware vector (RTE_29_1) is partially addressed by the framework's input validation guidance applied to operational recommendations. Profiling tool backdoor installation (RTE_29_3) falls within the framework's infrastructure hardening guidance. The limitation is that the framework does not address the multi-agent amplification dimension where shared registry infrastructure creates fleet-wide impact from single-point compromise.

### RTE_30 - Scaling & Auto-Scaling
- Strength score: 1

RTE_30 identifies pod anti-affinity rule manipulation enabling targeted agent isolation (RTE_30_1) and RBAC privilege escalation within agent service accounts exploiting misconfigured permissions (RTE_30_2) as threats specific to Kubernetes-based multi-agent auto-scaling deployments. While the NSA/NCSC Guidelines address least privilege and access control broadly, they contain no guidance on Kubernetes-specific RBAC configuration for agent deployments, pod scheduling security, or anti-affinity rule integrity. The framework's general hardening guidance is too abstract to address the specific Kubernetes orchestration attack surfaces described.

### RTE_31 - Fleet Management & Provisioning
- Strength score: 1

Fleet management threats in RTE_31—OTA update chain of custody corruption through poisoned baseline references (RTE_31_1), distributed TensorRT engine validation weaknesses in staged rollouts (RTE_31_2), self-replicating malware via provisioning token compromise (RTE_31_3), and certificate rotation desynchronization as a trust chain attack (RTE_31_4)—are specific to large-scale multi-agent edge deployments operating fleet management infrastructure. The NSA/NCSC Guidelines contain no guidance on fleet management for AI agent deployments, OTA update security for edge AI, staged rollout validation integrity, or agent provisioning token security. These are operationally specialized threats the framework does not address.

### RTE_32 - Batching & Caching Infrastructure
- Strength score: 1

RTE_32 identifies load balancer routing state poisoning as a malware propagation vector directing requests through compromised replicas (RTE_32_1) and auto-scaling threshold manipulation as a coordinated malware activation trigger where scaling events simultaneously activate latent malware in new instances (RTE_32_2). The NSA/NCSC Guidelines contain no guidance on load balancer security in multi-agent deployments, auto-scaling security considerations, or the coordinated activation risk created when shared state poisoning activates across all newly scaled instances simultaneously. These infrastructure attacks exploit the fleet coordination properties of multi-agent deployments.

### RTE - Other Risks/Threats/Vulnerabilities Worth Noting
- Strength score: 1

The "other risks" section covers a range of multi-agent trust exploitation mechanisms including trace-based few-shot learning poisoning through corrupted execution histories, parameter accuracy assumption exploitation between agents, tool call verification bypass through trust chain delegation, parameter validation delegation without re-validation, inter-agent context pollution enabling transitive instruction injection, conversation ID chain hijacking, efficiency metric sharing as malware propagation, trust degradation through efficiency report poisoning, cost optimization policy self-replication, efficiency consensus exploitation, benchmark gaming as coordinated deception, resource budget negotiation as malware trading, vector database retrieval quality blind spots, gossip protocol unauthenticated peer injection, load balancer MITM enabling cross-agent interception, guardrail threshold gaming enabling fleet-wide synchronized bypass, resource limit coordination failures enabling fleet-wide quota exhaustion, NIM model source trust exploitation through safetensors validation bypass, and hardware detection fallback exploitation for coordinated performance degradation. These threats collectively exploit the trust, coordination, and shared infrastructure properties of multi-agent systems in ways the NSA/NCSC Guidelines—designed as a general AI lifecycle framework predating widespread agentic deployment—do not address. The framework's monitoring, supply chain, and access control guidance provides minimal applicable baseline, but none of these specific multi-agent coordination attack vectors receive meaningful coverage.

## RWA - Workflow and ecosystem attacks on plugins, tools, and RAG pipelines

### RWA_1 - Specification Gaming and Misalignment
- Strength score: 2

RWA_1 encompasses 149 distinct specification gaming and misalignment attack patterns across agentic workflows, spanning UI template injection, adaptive threshold manipulation, reflection critic capture, plan-and-execute goal drift, auction protocol gaming, MCTS reward exploitation, tool parameter gaming, fine-tuning data poisoning, circuit breaker abuse, quantization-based specification gaming, and dozens of other mechanisms. The NSA/NCSC Guidelines provide meaningful but partial coverage through several applicable controls: prompt injection recognition and input sanitization address injection-based gaming vectors (RWA_1_19 dynamic tool metadata injection, RWA_1_38 tool example parameter injection via few-shot documentation, RWA_1_53 parameter validation failures in RAG-retrieved tool metadata, RWA_1_147 query decomposition LLM prompt injection); supply chain security controls including SBOM, dependency scanning, and cryptographic model integrity address training-time poisoning vectors (RWA_1_34 fine-tuning data poisoning, RWA_1_86 fine-tuning data injection for latency-biased malicious behavior, RWA_1_87/88 speculative decoding and calibration dataset poisoning, RWA_1_73 init container model download hijacking via compromised repositories); and monitoring guidance provides an indirect basis for detecting behavioral drift. However, the overwhelming majority of RWA_1's items—specification gaming through feedback loop exploitation, reflection critic capture, emergent goal drift in plan-and-execute architectures, auction protocol collusion, MCTS tree manipulation, circuit breaker gaming, graceful degradation exploitation, KV cache attention biasing, HNSW graph navigation poisoning, and dozens of others—address agentic reasoning architectures, learning dynamics, and operational patterns the NSA/NCSC framework does not contemplate. The framework's general threat modeling guidance cannot compensate for the absence of specific agentic-system security controls across these categories.

### RWA_2 - Model Training and Backdoors
- Strength score: 2

RWA_2's 60 backdoor and training attack patterns represent the threat category most directly aligned with NSA/NCSC Guidelines controls. The framework explicitly mandates training data integrity—requiring sanitization of data used for training and continuous learning, specifically identifying data poisoning as a known threat. Supply chain security controls (SLSA, SBOM, cryptographic hashing and signing for model artifacts) directly address model supply chain vectors including container image tampering (RWA_2_30 pre-built NIM container backdoors), model repository compromise (RWA_2_33 persistent volume shared mount for backdoor installation), and framework-embedded model assumption attacks (RWA_2_1). Red teaming and adversarial testing requirements provide coverage for backdoor detection during pre-deployment evaluation. However, the framework does not address fine-tuning-specific backdoors (RWA_2_4, RWA_2_17 LoRA adapter poisoning), quantization-induced backdoors exploiting calibration datasets (RWA_2_28, RWA_2_29, RWA_2_32), hardware-level backdoors through CUDA kernel injection (RWA_2_31), speculative decoding draft model backdoors (RWA_2_58), streaming temperature sensitivity revealing backdoor conditions (RWA_2_12), the fleet-wide amplification specific to multi-agent shared infrastructure (RWA_2_34, RWA_2_36), or hybrid component cross-training data poisoning cascades (RWA_2_55). The framework provides foundational coverage for the class of training supply chain attacks but lacks depth for the agentic-infrastructure-specific instantiations.

### RWA_3 - State and Context Poisoning
- Strength score: 1

RWA_3 attacks exploit stateful orchestration architectures—accumulated execution context poisoning in LangGraph workflows (RWA_3_8), checkpoint file injection (RWA_3_12), state field type confusion in conditional edge routing (RWA_3_13), custom reducer logic exploitation (RWA_3_14), ETL state file tampering for knowledge gap injection (RWA_3_20, RWA_3_29, RWA_3_30), multi-hop QA conversation history injection (RWA_3_16), context window capacity exploitation (RWA_3_19), caching TTL manipulation (RWA_3_22), and distributed memory fragmentation (RWA_3_31). The NSA/NCSC Guidelines contain no guidance on stateful agentic workflow security, LangGraph state management integrity, checkpoint file security, ETL state file access controls, or the trust implications of accumulated execution context. The framework's general access control guidance provides minimal applicable baseline for filesystem access controls that would protect ETL state files, but this is too indirect to constitute meaningful coverage for the specific state manipulation attack surfaces described.

### RWA_4 - RAG Pipeline Attacks
- Strength score: 2

RWA_4's 39 RAG pipeline attack patterns exploit retrieval augmented generation as an attack surface for instruction injection, knowledge graph manipulation, few-shot example poisoning, reranking configuration injection, HNSW parameter heterogeneity exploitation, and multi-stage pipeline bypass. The NSA/NCSC Guidelines' input sanitization requirements and supply chain security controls provide indirect coverage: sanitization of data used by AI systems is conceptually applicable to RAG corpus content integrity (RWA_4_1 UI-mediated document selection injection, RWA_4_21 vector database poisoning at scale); supply chain controls address the provenance of knowledge sources and embedding model integrity (RWA_4_30 knowledge graph construction tool backdoor exploitation, RWA_4_6 multimodal RAG pipeline plugin poisoning through vision model compromise); and the framework's prompt injection recognition guidance applies to RAG-mediated instruction delivery. However, the framework provides no guidance specific to RAG pipeline architecture security, citation verification bypass (RWA_4_38), lazy retrieval timing exploitation (RWA_4_34), multi-stage retrieval pipeline bypass (RWA_4_39), demonstration relevance hacking (RWA_4_14), hybrid search alpha parameter poisoning (RWA_4_18), or the iterative self-replication enabled by RAG retrieval cycles (RWA_4_2). The multi-agent amplification dimension—where shared RAG infrastructure enables one poisoned document to affect all retrieving agents—is not addressed.

### RWA_5 - Multi-Agent Orchestration Risks
- Strength score: 2

RWA_5's 52 orchestration attack patterns include API gateway amplification, swarm emergent misalignment, task boundary confusion, hierarchical privilege escalation through context manipulation, federated boundary exploitation, state-logic boundary attacks, framework backdoor triggers, cross-agent context laundering, tool chain boundary validation loss, sidecar container injection, fairness constraint bypass, cascading autonomous actions, and alert fatigue amplification. The NSA/NCSC Guidelines' infrastructure security guidance—covering TLS authentication, least privilege, network segmentation, and access controls—provides indirect coverage for infrastructure-layer orchestration attacks: service mesh injection of routing rules (RWA_5_29), cross-agent filesystem sharing through volume misconfiguration (RWA_5_48), network isolation bypass through permissive service mesh policies (RWA_5_49), and sidecar container injection via deployment template poisoning (RWA_5_24) all relate to infrastructure security principles the framework addresses. The framework's monitoring guidance provides an indirect basis for detecting anomalous orchestration patterns. However, the framework contains no guidance on multi-agent orchestration architectures, hierarchical trust models in agent networks, task decomposition security, federated orchestration boundary security, or the emergent misalignment risks specific to collaborative agent coordination.

### RWA_6 - Plugin and Tool Ecosystem Attacks
- Strength score: 2

RWA_6's 45 plugin and tool ecosystem attacks exploit plugin marketplace manipulation, framework-specific plugin integration supply chains, middleware injection, plugin composition specification gaming, package feed compromise, knowledge graph backdoors, container image layer tampering, and load balancer routing to specific tools. The NSA/NCSC Guidelines' supply chain security controls provide the most directly applicable coverage: SBOM requirements, dependency scanning, cryptographic integrity verification, and package version pinning guidance directly address plugin supply chain attacks including Semantic Kernel package feed compromise (RWA_6_6), dependency version pinning failure (RWA_6_20), container image layer tampering via registry compromise (RWA_6_21), NIM container image supply chain poisoning (RWA_6_26), and model repository persistent volume poisoning (RWA_6_27). The framework's access control guidance applies to plugin registry access controls. However, the framework provides no guidance on plugin marketplace trust indicator integrity, dynamic plugin registration security, plugin composition specification gaming, plugin discovery through RAG poisoning, rule base sharing ecosystem attacks, or the multi-agent amplification that transforms single plugin registry compromises into fleet-wide attacks.

### RWA_7 - Evaluation and Monitoring Bypass
- Strength score: 2

RWA_7's 25 evaluation and monitoring bypass techniques exploit aggregate monitoring blind spots, model behavior divergence between training and evaluation phases, evaluation metric hacking, regression detection evasion through gradual degradation, evaluation data pipeline integrity violations, metric computation library vulnerabilities, benchmark threshold gaming, and distributed trace analysis for intelligence gathering. The NSA/NCSC Guidelines' monitoring requirements and incident management guidance provide indirect coverage: the framework mandates monitoring for unusual inputs and outputs and behavioral changes, which is the conceptual basis for detecting metric gaming and monitoring evasion (RWA_7_1, RWA_7_3). Supply chain security controls address shared library vulnerabilities in metric computation (RWA_7_7). Red teaming requirements provide indirect coverage for identifying evaluation bypass vulnerabilities. However, the framework provides no specific guidance on evaluation pipeline integrity, benchmark data contamination detection (RWA_7_23), CI/CD benchmark threshold gaming (RWA_7_24), distributed trace operational disclosure (RWA_7_25), or multi-agent aggregate metric manipulation that keeps individual agents below detection thresholds (RWA_7_1). Prometheus-specific metric poisoning for RL reward signals (RWA_7_13), quality-weighted voting gaming (RWA_7_20), and distance metric mismatch attacks (RWA_7_12) are all outside the framework's scope.

### RWA_8 - Vector Database and Knowledge Base Attacks
- Strength score: 1

RWA_8's 21 vector database attacks exploit quantization approximation errors, heterogeneous vector database implementations creating retrieval inconsistencies, metadata filtering differences across implementations, ETL timestamp race conditions, quality threshold poisoning through configuration manipulation, deduplication bypass through near-duplicate crafting, connector credential poisoning, REST API pagination manipulation, metadata extraction inconsistency, and parallel processing timing side-channels. The NSA/NCSC Guidelines contain no guidance on vector database security, ANN index integrity, ETL pipeline security, embedding space poisoning, or the distributed consistency challenges specific to multi-agent knowledge base architectures. The framework's general access control guidance applies to database credential management (RWA_8_14 shared API key reuse), but this is too indirect to constitute meaningful coverage for the specific vector database and ETL vulnerability patterns described. The technical specificity of these attacks—exploiting HNSW graph navigation, ANN search approximation, batch insertion race conditions, and gossip protocol authentication gaps—is entirely outside the NSA/NCSC framework's scope.

### RWA_9 - Learning-Based Attacks
- Strength score: 1

RWA_9's 21 learning-based attacks exploit calibration phase monitoring evasion, tool access policy drift through iterative relaxation, error recovery policy exploitation, curriculum learning poisoning, dataset composition gaming, MCTS rollout policy poisoning during training, recursive decomposition reward hacking, cascading reward hacking through multi-agent workflow amplification, and cascading policy conflicts creating execution deadlocks. The NSA/NCSC Guidelines' training data integrity guidance provides minimal applicable coverage for curriculum learning poisoning (RWA_9_6, RWA_9_11) and MCTS rollout policy poisoning (RWA_9_9) by mandating sanitization of training and continuous learning data. However, the framework does not address RLHF reward hacking amplification in multi-agent workflows (RWA_9_20), policy gradient optimization of deception (RWA_9_15), experience replay-based tool chain learning poisoning (RWA_9_17), imitation learning from poisoned experts (RWA_9_18), long-horizon utility drift through cross-agent preference learning (RWA_9_12), or the emergent behavior risks specific to reinforcement learning in multi-agent contexts. These are advanced ML training paradigm vulnerabilities the framework does not contemplate.

### RWA_10 - UI/UX Security Attacks
- Strength score: 1

RWA_10's 14 UI/UX security attacks exploit command palette plugin recommendation poisoning, plugin UI provenance transparency gaps, tool suggestion UIs promoting dangerous operations without risk indicators, supply chain attacks on trusted plugins through marketplace trust indicators, RAG context window attacks exploiting preview limits, workflow automation UI masking of multi-step attack chains, plugin permission escalation through UI interaction context, RAG source attribution spoofing in chat citations, tool chain composition attacks through command palette macros, RAG pipeline prompt leakage through error messages, approval workflow poisoning via progressive disclosure manipulation, streaming-based specification gaming through selective content presentation, multimodal embedding dimension truncation attacks, and simulation-based tool interaction tracing disclosure. The NSA/NCSC Guidelines contain no guidance on UI/UX security design, plugin marketplace presentation, progressive disclosure security, command palette trust hierarchies, or the human factors that enable these attacks. The framework does not address these interface-layer vulnerabilities.

### RWA_11 - Reasoning and Reflection Attacks
- Strength score: 1

RWA_11's 15 reasoning and reflection attacks exploit multi-hop reasoning path manipulation through document ordering, milestone achievement instruction injection, hallucination amplification through critic-producer validation cycles, token accounting manipulation in long reasoning chains, grounding validation bypass through RAG poisoning establishing false grounding, critic agent reasoning quality dependency creating amplification of generator errors, mutual validation through shared reasoning bias, reflection trace injection through fabricated self-critique, fallback decision reasoning gaps, schema interpretation reasoning flaws, type coercion reasoning through weak intra-step logic, function description reasoning quality exploitation, reasoning signature as training-compromise indicator, tool chaining attack discovery through collective reasoning, and phase barrier bottleneck exploitation. The NSA/NCSC Guidelines contain no guidance on chain-of-thought reasoning security, critic-producer reflection architectures, multi-hop QA security, or reasoning trace integrity. These attacks exploit the internal cognitive architectures of agentic systems in ways the framework does not address.

### RWA_12 - Distributed Systems Attacks
- Strength score: 1

RWA_12's 8 distributed systems attacks exploit event-driven replay attack windows in asynchronous workflows, tool API rate limiting exhaustion through distributed invocation, cache coherence failures enabling selective instruction activation, batch processing race conditions causing duplicate insertion, feature flag race conditions creating behavioral inconsistency, deadlock injection through crafted requests triggering circular dependencies, out-of-order agent execution race conditions, and cross-agent filter evasion through distributed attack pattern fragmentation. The NSA/NCSC Guidelines address general infrastructure security but do not provide guidance on distributed agentic system coordination security, event replay attack prevention in asynchronous agent workflows, cache coherence security, or distributed deadlock attacks. The framework's general monitoring guidance provides minimal applicable baseline for detecting anomalous patterns but does not address the specific distributed coordination vulnerabilities described.

### RWA_13 - Approval Workflow Exploitation
- Strength score: 1

RWA_13's 7 approval workflow exploitation techniques include approval fatigue conditioning for specification gaming, human-in-the-loop workflow compromise through batch flooding and timing attacks, consensus manipulation in multi-agent approval voting, threshold gaming in approval workflows, cross-agent permission escalation through approval delegation, circular HITL approval dependency deadlocks, and cascading approval bottlenecks. The NSA/NCSC Guidelines emphasize maintaining human oversight of AI systems and incident management procedures, providing a conceptual basis for the importance of approval workflow integrity. However, the framework provides no specific guidance on approval fatigue countermeasures, multi-agent consensus voting security, permission escalation through delegation chains, HITL deadlock prevention, or the specific attack techniques targeting human oversight workflows. The framework's guidance predates the complex human-agent approval architectures described in RWA_13.

### RWA_14 - Infrastructure and Deployment Attacks
- Strength score: 2

RWA_14's 9 infrastructure and deployment attacks include GPU embedding side-channels through shared infrastructure, GPU resource exhaustion through adversarial batch flooding, CI/CD pipeline artifact injection via compromised credentials, load balancer health check gaming, GPU batching manipulation, Kubernetes secret store poisoning for NGC API keys, auto-scaling tool quota exhaustion, health check endpoint manipulation enabling degraded deployment, and MIG reconfiguration attacks causing cascading capacity exhaustion. The NSA/NCSC Guidelines' infrastructure security guidance—hardening, network segmentation, least privilege access controls, cryptographic integrity for deployment artifacts, and CI/CD pipeline security guidance—provides meaningful coverage for several of these attacks. CI/CD pipeline integrity is explicitly addressed in supply chain security guidance, covering credential compromise enabling artifact injection (RWA_14_3). Kubernetes secret management falls within the framework's infrastructure hardening guidance (RWA_14_6). Health check endpoint security relates to infrastructure monitoring requirements. However, the framework provides no guidance on GPU-specific side-channel attacks, MIG profile reconfiguration security, auto-scaling quota exhaustion in multi-agent deployments, or the fleet-wide amplification specific to shared GPU infrastructure.

### RWA_15 - Tool Invocation and Selection Gaming
- Strength score: 1

RWA_15's 8 tool invocation and selection gaming attacks exploit observation manipulation for frequency gaming, memory poisoning with malicious invocation chains, AutoGen GroupChat tool availability negotiation creating covert access, streaming tool invocation enabling partial execution gaming, temperature-controlled randomness for biased tool selection, SLA-driven tool selection gaming through latency falsification, KV cache sharing coupling tool outputs to selection, and rule description injection in shared repositories. The NSA/NCSC Guidelines contain no guidance on tool invocation monitoring, AutoGen tool negotiation security, streaming execution gaming, or the specific dynamics of tool selection in multi-agent environments. The framework's monitoring guidance provides minimal applicable baseline for detecting abnormal tool invocation patterns, but the specific gaming mechanisms described—temperature manipulation for statistical bias, latency falsification in shared rating systems, KV cache attention steering—are outside the framework's scope.

### RWA_16 - Framework-Specific Vulnerabilities
- Strength score: 1

RWA_16's 3 framework-specific vulnerability patterns include LangChain dynamic tool registration attack surfaces when tool definitions are loaded from untrusted sources, AutoGen tool negotiation creating implicit tool access chains through conversational social engineering, and NeMo Guardrails centralized configuration tampering enabling fleet-wide safety bypass across all agents. The NSA/NCSC Guidelines contain no guidance on LangChain, AutoGen, or NeMo Guardrails security. While the framework's general supply chain guidance applies conceptually to dynamic tool loading from untrusted sources (RWA_16_1), and the JCDC Playbook's incident management guidance is applicable to guardrail bypass incidents (RWA_16_3), the specific vulnerabilities arising from these frameworks' architectural choices—conversational tool access negotiation, centralized guardrail configuration as a shared attack surface—are not addressed. No framework-specific security guidance exists for any of these agentic AI frameworks.
