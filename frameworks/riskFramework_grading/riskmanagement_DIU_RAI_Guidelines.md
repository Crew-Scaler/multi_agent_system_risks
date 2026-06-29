# Risk Management Analysis: DIU Responsible AI Guidelines

## Framework Overview

The Defense Innovation Unit (DIU) Responsible AI (RAI) Guidelines operationalize the DoD's five Ethical Principles for AI—**Responsible**, **Equitable**, **Traceable**, **Reliable**, and **Governable**—across a three-phase AI lifecycle: Planning, Development, and Deployment. The framework is delivered as structured worksheets guiding AI vendors, DoD stakeholders, and program managers. The Development worksheet addresses five lines of inquiry: (1) manipulation of data models, (2) system performance monitoring, (3) output verification, (4) audit mechanisms, and (5) governance roles. The Deployment worksheet requires continuous evaluation procedures throughout the AI system's lifecycle.

The framework is fundamentally an ethical governance and oversight instrument, not a cybersecurity or adversarial ML defense framework. Assessments below are made against this honest baseline.

---

## RATC - Agent–tool coupling as "policy‑level remote code execution"

### RATC_1 - UI/UX Abstraction and Visibility Gaps
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires transparency of AI data, methods, and decisions, and the development worksheet includes audit mechanisms as a line of inquiry. In principle, these provisions could encourage some consideration of how tool invocations are surfaced to human reviewers. However, the framework does not address UI/UX design for multi-agent systems, tool chain visibility across agent boundaries, or the security implications of progressive disclosure patterns that hide dangerous multi-agent execution sequences. The DIU guidelines are an ethical oversight framework, not a UI security or HCI security standard, and they offer no concrete guidance on preventing tool chain abstraction from concealing multi-agent execution complexity or risk from human approvers.

### RATC_2 - Approval Workflow and User Attention Vulnerabilities
- Strength score: 1

The DIU RAI Guidelines' "Governable" and "Responsible" principles emphasize appropriate human judgment over AI decisions and human accountability for AI outcomes, which could broadly be interpreted to include proper design of approval workflows. However, the framework provides no guidance on preventing approval fatigue in high-volume multi-agent environments, keyboard shortcut hijacking via injected JavaScript, or time-based default exploits that force unauthorized approvals during coverage gaps. These specific UI manipulation and psychological exploitation vectors require dedicated security engineering controls not addressed by an ethical AI oversight framework.

### RATC_3 - Tool Overload, Parameter Handling, and Selection Errors
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle requires systems to perform as intended with clear performance metrics, and the development worksheet includes output verification as a line of inquiry. This could encourage testing of tool selection accuracy under varying load conditions. However, the framework does not address multi-agent privilege escalation through tool overload, argument injection across trust boundaries where LLM generation probabilistically introduces malicious payloads, or compounding parameter extraction errors in chained multi-agent calls. These are technical attack vectors requiring dedicated security engineering beyond the scope of an ethical governance framework.

### RATC_4 - Confidence Manipulation and Tool Authorization
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle includes system performance monitoring and output verification, which could in principle include validation of model confidence scores. The "Traceable" principle's audit mechanisms might surface anomalous confidence patterns. However, the framework offers no specific guidance on detecting adversarially inflated confidence scores designed to bypass approval gates, securing authorization scopes across multi-agent specializations, or preventing precondition validation bypass through transitive trust in agent chains. These are adversarial ML threats requiring specialized countermeasures beyond this framework's scope.

### RATC_6 - Tool Metadata Poisoning Across Registries and Discovery
- Strength score: 1

The DIU RAI Guidelines do not address software supply chain security, tool registry integrity, or framework-specific tool selection security. The "Reliable" principle could in theory require verification that tool behavior matches specifications, but the framework provides no guidance on defending against centralized vector database metadata poisoning, few-shot parameter injection in tool descriptions, or framework abstraction leakage enabling hidden tool chain exploitation across multi-agent systems. The framework is focused on ethical use rather than security of AI development infrastructure.

### RATC_8 - Advanced Tool Invocation Patterns and Inference
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires audit mechanisms that could capture unusual tool invocation sequences, and "Reliable" performance monitoring might detect unexpected behaviors over time. However, the framework offers no guidance on securing Plan-and-Execute replanning loops against iterative tool chain exploitation, defending ReAct pattern observation formats against injection via poisoned upstream outputs, or hardening cross-framework tool schema translation boundaries against parameter manipulation. Attacks exploiting multi-agent replanning and streaming injection require dedicated AI security controls not addressed by this framework.

### RATC_9 - Web and Multimodal Tool Exploitation
- Strength score: 1

The DIU RAI Guidelines' "Responsible" and "Reliable" principles could encourage general consideration of potential harms from web-facing AI agents and data integrity in retrieved content. However, the framework provides no guidance on defending against prompt injection via adversarially crafted web content, vision model output laundering through multi-step multi-agent processing, or cross-modal parameter injection in multi-agent RAG systems where multiple modalities contribute parameter segments without sanitization boundaries. These are specialized adversarial attack vectors requiring domain-specific security controls beyond this framework's scope.

### RATC_10 - Efficiency Optimization and Resource Constraints
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle broadly requires systems to perform as intended, which could encompass some consideration of behavior under resource constraints. However, the framework does not address adversarial exploitation of token budget constraints to truncate safety-critical tool descriptions in upstream agents, iteration budget manipulation across multi-agent systems with different limits, or temperature tuning exploitation to create policy inconsistency across model variants within a fleet. These highly technical adversarial strategies require specialized AI security research and testing not encompassed by an ethical oversight framework.

### RATC_11 - Tool Execution Infrastructure and Orchestration
- Strength score: 1

The DIU RAI Guidelines have no applicable coverage of API gateway routing policy attacks, Kubernetes container security configurations, persistent volume claim exploitation, sidecar proxy interception, or init container injection in multi-agent AI deployments. These are infrastructure and DevSecOps security concerns requiring network security, container hardening, and supply chain controls that fall entirely outside the scope of an ethical AI framework focused on governance, fairness, and responsible use principles.

### RATC_12 - Distributed and Hardware-Level Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of tensor parallelism communication security across GPUs, KV cache isolation vulnerabilities in multi-user inference, GPU quantization calibration dataset poisoning, or multi-tenant memory isolation failures. These hardware-level and distributed system security concerns require specialized GPU security engineering, cryptographic protections for inter-GPU communication, and hardware memory isolation guarantees that are entirely outside the scope of this ethical AI framework.

### RATC_14 - Reasoning and Planning Vulnerabilities
- Strength score: 1

The DIU RAI Guidelines' "Traceable" and "Reliable" principles could encourage logging and output verification of AI reasoning processes, which might help detect anomalous reasoning patterns. However, the framework offers no guidance on defending against poisoned chain-of-thought traces that embed malicious tool-selection justifications appearing independently sound, self-consistency majority voting manipulation through convergent path injection, or hierarchical task network planning tool visibility fragmentation. These adversarial attacks on AI reasoning require specialized AI safety research not encompassed by this framework.

### RATC_15 - Episodic Memory and Learning
- Strength score: 1

The DIU RAI Guidelines' development worksheet explicitly includes "manipulation of data models" as a line of inquiry, which is broadly relevant to episodic memory poisoning attacks. However, the framework treats data model integrity as an ethical governance concern—not as a security engineering discipline—and provides no specific guidance on preventing adversarial episode injection into shared episodic memory stores, detecting malicious action records disguised as successful resolutions, or blocking trajectory abstraction that converts poisoned episodes into dangerous procedural workflows propagated across multi-agent systems.

### RATC_16 - Semantic Memory and RAG
- Strength score: 1

The DIU RAI Guidelines' "Reliable" and "Traceable" principles encourage data quality and transparency, and the development worksheet's data model manipulation line could include some consideration of knowledge base integrity. However, the framework provides no specific guidance on preventing knowledge graph relationship poisoning that causes unsafe tool combinations to appear recommended, query rewriting instruction injection affecting all agents' transformations, or staleness-based failures from outdated RAG documentation. These are specialized vector database and RAG security concerns requiring dedicated controls beyond the framework's scope.

### RATC_17 - Utility Functions and Decision Logic
- Strength score: 1

The DIU RAI Guidelines do not address adversarial manipulation of AI utility functions, outcome assumption injection causing agents to miscalculate expected utility, or multi-objective utility weight poisoning. The "Reliable" principle broadly requires correct system behavior, but the framework provides no security engineering guidance for protecting AI decision logic against targeted manipulation. These threats require formal verification and adversarial testing of decision-making models beyond the scope of an ethical guidelines document.

### RATC_18 - Rule-Based and Knowledge-Engineered Systems
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle could broadly encompass testing that rule-based systems behave correctly under unexpected inputs. However, the framework provides no guidance on preventing rule injection attacks that override safety rules through specificity hierarchies, forward chaining exploitation via injected facts, certainty factor manipulation to bypass risk assessment, or lexicographic objective reordering through targeted rule injection. Securing rule-based AI authorization systems requires formal security analysis of inference engines entirely outside this framework's scope.

### RATC_19 - Learning and Reinforcement Learning
- Strength score: 2

The DIU RAI Guidelines' development worksheet explicitly identifies "manipulation of data models" as one of five key lines of inquiry, providing direct applicability to RL tool selection poisoning through reward signal manipulation. This line of inquiry prompts stakeholders to examine how data models can be corrupted, which encompasses adversarial reward poisoning, curriculum learning poisoning, and imitation learning trajectory corruption. Additionally, the "Reliable" principle's system performance monitoring requirements could detect behavioral drift from actor-critic cross-agent poisoning or proximal policy optimization trust region manipulation. While the framework does not prescribe specific RL security controls, its explicit attention to data model manipulation and governance roles offers moderate relevance to managing the risk of RL-based tool selection being corrupted through adversarial training signals.

### RATC_21 - Parallel Retrieval Race Conditions in Multi-Agent Query Decomposition Systems
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle requires AI systems to perform correctly under operational conditions, which could in principle include testing for concurrent access failures. However, the framework provides no guidance on database state consistency during parallel retrieval, shared index deduplication races causing duplicates or missed documents, connection pool exhaustion from cascading timeouts, multi-stage cache TTL conflicts producing mixed knowledge states, or container supply chain vulnerabilities in model registries. These are distributed systems engineering and infrastructure security concerns entirely outside the scope of an ethical AI framework.

### RATC_22 - Multi-Stage Pipeline Result Caching Race Enabling Multi-Agent Consistency Failures
- Strength score: 1

Like RATC_21, the DIU RAI Guidelines' "Reliable" principle could theoretically include requirements for data consistency in AI pipelines, but the framework provides no guidance on event-based cache invalidation race conditions propagating across agent fleets, hash collision attacks on shared caching infrastructure poisoning entries for multiple agents, cache warming inconsistency, or GitOps automation enabling fleet-wide backdoor deployment through model serving registry compromise. The supply chain and infrastructure security dimensions of these threats are entirely outside the framework's scope.

### RATC_27 - MIG Instance Co-Location Creating Shared Physical GPU Failure Propagation
- Strength score: 1

The DIU RAI Guidelines have no coverage of GPU hardware security, Multi-Instance GPU partitioning vulnerabilities, firmware-level cross-partition memory access breaches, PCIe-level failure propagation across MIG instances, or shared GPU driver stack vulnerabilities enabling fleet-wide outages. These hardware isolation and security concerns require GPU security engineering, hardware-level isolation guarantees, and firmware vulnerability management that are entirely beyond the scope of an ethical AI framework focused on governance and responsible use.

### RATC_29 - Shared Workflow State Version Conflict Amplification Through Concurrent Write Flooding
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle broadly requires that AI systems maintain correct operation, but the framework provides no guidance on optimistic locking strategies for multi-agent shared state, version conflict management under high concurrency, retry storm mitigation where conflicts intensify rather than resolve, or targeted collision bursts designed to exhaust workflow retry budgets causing task failures. These are distributed systems and database engineering concerns requiring dedicated technical controls well beyond the scope of an ethical guidelines framework.

### RATC_30 - Guardrail Bypass Through Infrastructure Failure Injection Preventing Safety Validation Execution
- Strength score: 2

The DIU RAI Guidelines' "Governable" and "Responsible" principles most directly relate to this threat, as they emphasize the ability to monitor, oversee, and control AI systems and ensure that humans remain accountable for AI outputs throughout the system's lifecycle. The deployment worksheet's continuous evaluation procedures could include monitoring for guardrail availability and effectiveness. The framework's insistence on human oversight provides governance-level rationale for designing guardrail infrastructure that fails safely—rejecting rather than passing through unvalidated responses when validation endpoints are unavailable. However, the framework does not prescribe specific technical controls for preventing DoS-induced timeout exploitation against centralized guardrail validation endpoints, making its support indirect and policy-level only.

---

## RDL - New data‑leakage channels via large contexts, logs, and probabilistic recall

### RDL_1 - UI/UX Patterns
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires transparency of AI decisions and the development worksheet includes audit mechanisms as a line of inquiry. In principle, these provisions could encourage mindful handling of sensitive data surfaced through multi-agent UIs. However, the framework offers no specific guidance on preventing chat interfaces from accumulating credentials and PII across agent sessions without adequate access controls, securing progressive disclosure debug views that aggregate inter-agent authentication tokens and system internals, managing session persistence that mixes data from agents operating at different classification levels, or limiting command palette histories from producing cross-agent behavioral reconnaissance artifacts. These are UI security and data minimization engineering concerns not addressed by an ethical oversight framework.

### RDL_2 - Data Persistence and Caching
- Strength score: 1

The DIU RAI Guidelines provide no guidance on securing serialized multi-agent session data against structured query exfiltration, managing cache retention policies for composite multi-agent responses that outlive session permissions, or ensuring distributed undo buffers across multi-agent systems comply with data minimization and right-to-be-forgotten requirements. The "Traceable" principle encourages audit trails but does not address preventing persistent sensitive data from accumulating in session caches or recovery buffers where it exceeds the original session's authorization scope.

### RDL_3 - Streaming and Token-Level Leakage
- Strength score: 1

The DIU RAI Guidelines have no applicability to streaming token-level data leakage, streaming-based context window inference attacks revealing tokenization patterns across multi-agent systems, output length correlation for session reconstruction through metadata analysis, intermediate state exposure in multi-agent cycling workflows, or covert data exfiltration through streaming cache side channels. These highly technical information security concerns require network-level controls, streaming content filtering, and timing attack mitigations entirely beyond the scope of an ethical AI framework.

### RDL_4 - Search and Recall
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle emphasizes transparency but does not address probabilistic recall attacks via conversation search indexes that cross-contaminate agent domains without security boundaries. Preventing semantic similarity searches from surfacing confidential HR, security, or financial content through association with unrelated financial queries requires access-controlled search boundaries and security domain separation that this ethical governance framework does not prescribe.

### RDL_5 - Attribution and Observability
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires audit trails and attribution of AI decisions, which creates a tension with the RDL_5 threats: these threats arise precisely because attribution logs document ecosystem-level agent usage patterns, user role mappings, and agent specialization data that become high-value intelligence targets for competitive espionage and social engineering. The framework encourages creating comprehensive attribution records for accountability but provides no guidance on securing them against exfiltration or limiting the organizational intelligence they disclose to adversaries.

### RDL_7 - Tool Invocation and Function Calling
- Strength score: 1

The DIU RAI Guidelines' development worksheet explicitly includes "audit mechanisms" as a line of inquiry, broadly encompassing logging of tool invocations. However, this provision encourages creating such audit trails for governance accountability rather than securing them against leakage. The framework offers no guidance on preventing tool parameter logs from exposing API keys, account numbers, or PII; securing function calling JSON in shared agent context windows; preventing tool selection reasoning from leaking agent decision-making profiles; or detecting audit trail poisoning where compromised agents omit malicious invocations from compliance logs.

### RDL_9 - Multi-Agent Memory and State
- Strength score: 1

The DIU RAI Guidelines have no coverage of cross-agent reflection memory becoming unauthorized covert channels between security domains, ReAct reasoning trace forensic reconstruction enabling attack graph derivation, distributed trace cross-tenant correlation leaking workflow patterns through timing metadata, or blackboard shared memory access pattern analysis enabling workflow reconstruction without direct content access. These are specialized agentic AI memory security concerns requiring memory access controls, trace sanitization, and multi-tenant isolation entirely outside the scope of an ethical oversight framework.

### RDL_10 - Reasoning and CoT Traces
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle values transparency of AI reasoning processes, but the RDL_10 threats arise from the security risks created by that very transparency: stored CoT traces contain the sensitive context they analyzed, intermediate steps enable reconstruction of sensitive inputs, reasoning-embedded secrets persist in shared coordination contexts, and preserved high-quality reasoning paths accumulate as long-lived leakage vectors across agent boundaries. The framework encourages traceability for accountability without providing guidance on how to prevent that traceability from becoming a multi-agent data leakage channel.

### RDL_11 - Multimodal and Input Processing
- Strength score: 1

The DIU RAI Guidelines have no coverage of vision model intermediate representation leakage, audio embedding privacy risks in shared vector stores, chart linearization creating machine-readable numerical data extraction from protected images, or embedding inversion attacks reconstructing sensitive medical imagery or confidential diagrams from shared multimodal vector databases. These are specialized multimodal AI security concerns requiring domain-specific embedding access controls and representation isolation entirely beyond this framework's scope.

### RDL_12 - Error Handling and Graceful Degradation
- Strength score: 1

The DIU RAI Guidelines do not address securing centralized multi-agent error logs containing sensitive data from all agents' error states, preventing fallback routing to secondary providers with weaker log retention policies from creating data leakage paths, or limiting capability disclosure through graceful degradation telemetry and circuit breaker state logs that reveal infrastructure fragility and system architecture to adversaries. These technical error handling security practices require engineering controls not encompassed by an ethical governance framework.

### RDL_13 - Evaluation and Testing Leakage
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle requires performance monitoring and output verification, which could broadly encompass evaluation practices. However, the framework provides no guidance on securing evaluation metric logs from fingerprinting proprietary algorithms, protecting evaluation datasets from reconstruction through multi-agent dashboard access, preventing temporal analysis of benchmark result changes from revealing update patterns and failure modes, or securing evaluation artifacts against exfiltration by adversaries seeking to craft evaluation-evading attacks.

### RDL_14 - Feedback and Testing
- Strength score: 1

The DIU RAI Guidelines do not address protecting user feedback logs from being systematically analyzed to profile agent failure modes for targeted compromise, or preventing A/B test result storage from revealing which agent variants are more susceptible to specific attack patterns. These are operational security concerns for AI systems that require specific data governance and access control engineering beyond the ethical framework's scope.

### RDL_15 - Evaluation Metric Exploitation
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle broadly requires correct system performance but provides no guidance on preventing adversarial exploitation of Exact Match metrics through semantic paraphrasing, joint metric evasion via selective fabricated fact injection, Pass@K inconsistency exploitation through probabilistically activating attack instructions that evade detection, or milestone scoring threshold manipulation enabling conditional attack execution. These require specialized AI evaluation security and adversarial robustness controls entirely outside the framework's scope.

### RDL_16 - Parameter and Configuration Leakage
- Strength score: 1

The DIU RAI Guidelines have no coverage of context window logging size differentials as information leakage channels, token consumption pattern analysis inferring query sensitivity distributions from cost-optimized model routing, or latency fingerprinting of deployed AI configurations enabling infrastructure reconnaissance. These are infrastructure-level timing and resource consumption side channel attacks requiring normalization and isolation controls entirely outside the scope of an ethical AI framework.

### RDL_17 - Prompt Injection and Few-Shot Attacks
- Strength score: 1

The DIU RAI Guidelines do not address adversarial few-shot demonstration poisoning embedding instructions in output format definitions, generation pattern injection through confidence-biased examples, or probabilistic recall of sensitive data from prompt demonstrations. These are adversarial attacks on AI model behavior through prompt engineering that require input sanitization, demonstration validation, and prompt security engineering not addressed by an ethical governance framework.

### RDL_18 - Distributed Tracing
- Strength score: 1

The DIU RAI Guidelines have no coverage of distributed tracing leakage through correlation ID analysis revealing complete multi-agent operation chains, or schema validation log side channels enabling attackers to fingerprint agent types and customize attacks. These are infrastructure telemetry security concerns requiring tracing system access controls and data minimization in span payloads entirely outside the ethical framework's scope.

### RDL_19 - Execution and Orchestration
- Strength score: 1

The DIU RAI Guidelines do not address execution path disclosure through aggregated multi-agent error propagation where each agent adds diagnostic context creating comprehensive execution traces, or parameter provenance tracking loss creating attribution amnesia where compromised upstream outputs propagate malicious parameters through agent chains without detectability. These require orchestration security engineering controls not addressed by an ethical framework.

### RDL_20 - Multi-Agent Reasoning
- Strength score: 1

The DIU RAI Guidelines' "Reliable" and "Traceable" principles could broadly encourage validation of AI reasoning quality. However, the framework does not address orchestrator reasoning failures cascading across all worker agents executing flawed coordination logic, transparency paradoxes enabling attacks when supervisor reasoning explicitly reveals delegation patterns, or system-level reasoning incoherence where each agent reasons locally that a task is safe but the system-level outcome is globally unsafe with no cross-boundary coherence validation. These require cross-agent reasoning security frameworks beyond the ethical guidelines' scope.

### RDL_21 - Efficiency and Cost Attribution
- Strength score: 1

The DIU RAI Guidelines have no coverage of efficiency log exploitation inferring sensitive operational patterns from token spikes and API call anomalies, cost attribution metadata leakage revealing data sensitivity through processing cost differentials, cache performance metric analysis identifying high-value frequently-accessed data targets, or efficiency dashboard exposure providing infrastructure architecture reconnaissance. These operational telemetry security concerns require access controls and metric anonymization not encompassed by an ethical governance framework.

### RDL_22 - Infrastructure and Deployment
- Strength score: 1

The DIU RAI Guidelines do not address message queue header correlation ID leakage enabling workflow reconstruction, vector database embedding exposure enabling approximate text recovery through inversion attacks, Prometheus label cardinality leakage enabling temporal workflow correlation, API gateway log correlation for complete user workflow reconstruction, MLflow artifact versioning metadata disclosure, dead-letter queue content exposure, or centralized logging pipeline compromise enabling fleet-wide data exfiltration. These are infrastructure security engineering concerns entirely outside the framework's scope.

### RDL_23 - Containerization Security
- Strength score: 1

The DIU RAI Guidelines have no coverage of CPU cache side channels between containerized agents on shared physical hosts, event stream retention creating durable leakage exposure, persistent volume permission escalation enabling cross-agent log exfiltration, sidecar proxy access log interception of inter-agent plaintext communication, init container environment variable leakage through crash-induced error logs, kubelet unauthenticated metrics port exposure, or etcd database backup containing all cluster secrets and pod environment variables. These container and Kubernetes security engineering concerns are entirely outside the scope of an ethical AI framework.

### RDL_24 - Profiling and Optimization
- Strength score: 1

The DIU RAI Guidelines provide no guidance on securing execution traces containing sensitive data processed during profiling, protecting MLflow artifact registries storing model versions and prompt artifacts with sensitive business logic, securing baseline comparison behavioral records from revealing user request patterns, or controlling access to performance optimization logs disclosing system capabilities and infrastructure constraints. These require data governance and access control engineering specific to AI development pipelines not addressed by this framework.

### RDL_25 - Kubernetes Orchestration
- Strength score: 1

The DIU RAI Guidelines have no coverage of Kubernetes event audit logs leaking multi-agent orchestration topology through correlated pod scaling and failure patterns, revealing inter-agent dependencies and identifying critical agents whose failures trigger cascading restarts. This container orchestration security concern requires Kubernetes audit log hardening and role-based access controls entirely outside the ethical framework's scope.

### RDL_26 - GPU and Model Internals
- Strength score: 1

The DIU RAI Guidelines have no coverage of GPU memory utilization metric leakage enabling agent activity inference, verbose inference engine logging exposing token generation intermediates across multi-agent logging infrastructure, queue depth metric workload reconstruction, KV cache side-channel attacks through unencrypted inter-GPU tensor parallelism communication, engine binary metadata model extraction attacks, quantization artifact analysis inferring input domain, GPU memory forensics through engine traces, or calibration dataset distribution leakage through quantization statistics. These hardware and model serving security concerns are entirely outside the scope of an ethical governance framework.

### RDL_27 - Load Balancing
- Strength score: 1

The DIU RAI Guidelines have no coverage of load balancer routing decisions as observable side channels revealing system load and capability differentials, batching window timing as latency-based information leakage, cache hit/miss pattern analysis as content access disclosure, load balancer metrics endpoint exposure for replica targeting, or auto-scaling event timing as attack signal for inferring system capacity and configuration. These are infrastructure timing side channel concerns requiring network-level controls outside the framework's scope.

### RDL_28 - Planning Systems
- Strength score: 1

The DIU RAI Guidelines do not address context window size as a leakage capacity indicator enabling targeted data extraction attacks against large-context agents, planning history disclosure through task network serialization in inter-agent coordination, method library reconnaissance via shared registry access revealing complete planning capability space, constraint specification leakage revealing resource trust boundaries, state abstraction mapping disclosure revealing information classification priorities, or task completion log timeline analysis enabling workflow reconstruction. These are specialized AI planning system security concerns not addressed by this ethical framework.

### RDL_29 - Monte Carlo Tree Search (MCTS)
- Strength score: 1

The DIU RAI Guidelines have no coverage of MCTS tree statistics as implicit planning memory leakage encoding sensitive state-action-reward history in shared context, simulation trajectory logs as probabilistic recall attack surfaces for attempted-but-not-executed actions, value network confidence score disclosure revealing strategic assessments, rollout policy behavior analysis as implicit strategy information leakage, or replanning trajectory logs enabling infrastructure topology reconstruction through shared multi-agent log access. These are specialized AI search algorithm security concerns entirely outside the framework's scope.

### RDL_30 - Monitoring and Callbacks
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle includes performance monitoring requirements, but the framework does not address discrepancy leakage through monitoring callbacks where agents broadcasting discovered obstacles, tool unavailability, or failure states to teammates inadvertently enumerate the exact state space regions being explored and tools being attempted, creating operational intelligence leakage through collaborative monitoring protocols. This requires secure multi-agent communication design not addressed by an ethical governance framework.

### RDL_31 - Episodic Memory
- Strength score: 1

The DIU RAI Guidelines' development worksheet includes "manipulation of data models" as a line of inquiry, which could broadly touch on episodic memory integrity. However, the framework provides no specific guidance on preventing episodic memory from serving as a covert data exfiltration mechanism via external storage, defending against embedding vector inference attacks reconstructing episode content from shared vector stores, securing consolidation abstractions that preserve enough detail for reconstruction, or detecting graph relationship traversal reconstructing operational sequences and decision patterns. These require specialized memory security controls beyond the framework's scope.

### RDL_32 - Semantic Memory
- Strength score: 1

The DIU RAI Guidelines have no coverage of semantic memory context window leakage where retrieved RAG documents expose sensitive knowledge base contents to agents and monitoring systems, knowledge base document enumeration through systematic retrieval pattern analysis, embedding similarity fingerprinting for document content inference, temporal metadata leakage revealing system evolution history, or knowledge graph structure inference through systematic absence-of-relationship analysis. These are RAG system security concerns requiring access-controlled retrieval and semantic search hardening not addressed by this framework.

### RDL_33 - RAG Leakage
- Strength score: 1

The DIU RAI Guidelines do not address RAG relevance score exploitation as probabilistic leakage channels for inferring document content through cross-query similarity analysis, query rewriting log disclosure of operational intent and agent reasoning processes, deduplication artifact leakage revealing content similarity relationships, compression validation failures causing hallucinated summaries to propagate as credible knowledge in multi-agent pipelines, or semantic-to-working memory distillation as a privacy breach channel where episodic agent-identifying information leaks into shared semantic memory accessible to all agents. These are RAG pipeline security concerns requiring specific technical mitigations.

### RDL_34 - Utility Functions and Explanations
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires transparency in AI decision-making, which creates a fundamental tension with the RDL_34 threats: progressive disclosure UIs, streaming decision rationale, and explanation generation required for accountability become leakage vectors disclosing utility parameters, rule structures, and working memory content in multi-agent systems. The framework encourages this transparency for governance purposes but provides no guidance on preventing utility parameter disclosure through expanded technical views, streaming explanation leakage of probability assumptions before redaction is possible, or working memory content exposure through shared persistence in multi-agent deployments.

### RDL_35 - Reinforcement Learning and Policy
- Strength score: 1

The DIU RAI Guidelines' development worksheet's "manipulation of data models" line could broadly encompass concern for RL training data integrity. However, the framework provides no guidance on preventing gradient-based model inversion attacks reconstructing training data from shared gradients in multi-agent systems, value function queries reconstructing which states appeared in training, policy behavior querying for black-box training data distribution reconstruction, experience replay buffer side-channel attacks, reward function reverse engineering from observed agent behavior, or context window expansion data leakage in hybrid agent coordination concentrating sensitive multi-agent data in shared vulnerable context windows.

### RDL_36 - Multi-Agent Coordination
- Strength score: 1

The DIU RAI Guidelines have no coverage of multi-agent conversation trace leakage through shared history where probabilistic token generation may surface sensitive data from accumulated inter-agent interactions, or knowledge graph edge weight leakage via embedding inference where weighted relationships encoding sensitive associations are reconstructable from embeddings trained on the graph. These are specialized multi-agent coordination security concerns requiring shared context isolation and embedding access controls entirely outside the framework's scope.

---

## RIP+RIDC - Agent identity, provenance, and economic/resource attack surface

### RIP_1 - Multi-Agent UI & Dashboard Attacks
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires that AI data, methods, and decisions be transparent and that audit mechanisms exist, which conceptually encourages attribution logging and visibility into agent actions. However, the framework does not address cryptographic agent identity verification in multi-agent dashboard UIs, fine-grained attribution forensics for tracing malicious decisions through multi-agent pipelines, real-time per-agent cost attribution for economic accountability, or prevention of social engineering through GroupChat speaker identity spoofing. These require UI security engineering and cryptographic provenance controls not provided by an ethical governance framework.

### RIP_2 - Approval & Workflow Attacks
- Strength score: 2

The DIU RAI Guidelines' "Traceable" principle and the development worksheet's explicit "audit mechanisms" line of inquiry directly relate to approval workflow provenance. The framework requires transparency of AI decisions and audit trails documenting decision processes, which corresponds to establishing cryptographic provenance trails across multi-agent approval pipelines. The "Responsible" principle holds personnel accountable for AI-assisted decisions, reinforcing the need for unambiguous decision attribution. However, the framework does not prescribe cryptographic signature requirements for inter-agent approval records, provide specific guidance on preventing confidence score aggregation manipulation through selective agent compromise, or address tamper-evident logging implementations to prevent client-side approval record modification.

### RIP_3 - Command Palette & UI Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of command palette cost visibility gaps enabling economic denial-of-service through context-manipulated expensive operation suggestions, agent identity confusion in suggestion attribution systems, or rate limiting bypass through agent identity switching in multi-agent command systems. The "Responsible" principle could broadly encourage user awareness of AI action costs, but the framework provides no specific technical guidance on cost transparency, attribution controls, or rate limiting policies for multi-agent UI systems.

### RIP_4 - Multi-Agent Coordination Attacks
- Strength score: 1

The DIU RAI Guidelines do not address reflection-amplified resource exhaustion through exponential inter-agent feedback loops, orchestrator resource exhaustion via coordination request flooding, auction manipulation through strategic bidding and Sybil coalition formation, or broadcast amplification denial-of-service in event-driven multi-agent publish-subscribe networks. These are distributed systems security and economic denial-of-service concerns requiring specific architectural controls, resource quotas, and coordination security mechanisms entirely outside the scope of an ethical AI governance framework.

### RIP_5 - Framework-Specific Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of cross-framework identity model gaps enabling impersonation through identity translation failures, framework-dependent cost attribution opacity enabling denial-of-wallet attacks, AutoGen conversational identity spoofing without cryptographic binding, CrewAI role-based impersonation, or Semantic Kernel plugin registry token consumption amplification. These are framework-specific security engineering concerns requiring cryptographic identity binding and cost attribution controls at the AI framework level, not addressed by an ethical oversight framework.

### RIP_6 - Tool & Plugin Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of economic attacks through multi-agent tool chaining cost amplification, tool invocation cost attribution gaps enabling free-rider resource exhaustion through memory sharing, tool access token leakage through inter-agent communication, plugin execution identity ambiguity through kernel-level delegation, privilege inheritance and escalation through agent composition chains, or tool execution authority transitive exploitation in hybrid workflows. These require dedicated security engineering for tool authorization, credential isolation, and cost attribution in multi-agent architectures, far outside this framework's scope.

### RIP_7 - Cost & Economic Denial-of-Service Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of the extensive range of economic denial-of-service and cost attribution attacks catalogued in this subsubsection, including resource attribution ambiguity in statistical analysis, reasoning amplification cost attacks, provenance gaps in multi-step reasoning chains, MCTS computational budget exhaustion, episode attribution spoofing, storage expansion attacks, budget allocation as authorization confusion, utility function resource exhaustion, persistent volume claim contamination, multi-connector credential provenance confusion, and ETL incremental update provenance gaps. The "Responsible" principle broadly requires consideration of potential harms, but the framework provides no economic security controls, resource quota management, or cost attribution mechanisms.

### RIP_8 - Resilience & Error Handling Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of streaming response resource consumption opacity enabling economic denial-of-service, retry budget attribution failures across multi-agent boundaries enabling retry pool exhaustion, fallback provider cost attribution ambiguity, circuit breaker resource consumption attribution gaps, or streaming attribution spoofing through progressive identity claims. These resilience and error handling security concerns require dedicated economic controls and identity verification mechanisms not encompassed by an ethical AI governance framework.

### RIP_9 - Multimodal Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of multimodal content attribution spoofing through vision model output, vision model resource exhaustion through coordinated multimodal denial-of-service, audio processing resource exploitation through long-duration content monopolizing shared Whisper infrastructure, or embedding vector store resource amplification through multimodal scale enabling similarity search overload. These are specialized multimodal AI system resource security concerns requiring domain-specific controls entirely outside the framework's scope.

### RIP_10 - Reasoning & Evaluation Attacks
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle requires performance monitoring and output verification, which could broadly encompass evaluation infrastructure. However, the framework provides no guidance on preventing evaluation cost attribution failures enabling economic denial-of-service, defending against evaluation agent identity spoofing in decision displays, protecting evaluation artifact provenance from tampering, preventing excessive evaluation cycle triggering through false alarms, countering benchmark attribution spoofing, managing MCTS computational budget exhaustion in shared resource pools, or defending against multi-pattern cost amplification through ReAct-Reflection interaction exploitation. These evaluation and resource security concerns require specialized controls.

### RIP_11 - Vector Store & RAG Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of cost-per-query economic abuse, resource consumption accounting evasion, knowledge base source attribution loss through multi-agent processing enabling instruction laundering, vector database partition escaping for cross-agent identity fraud, query load amplification through synchronized poisoned retrieval, knowledge graph ownership authorization ambiguity, embedding API endpoint provenance confusion enabling malicious endpoint redirection, model version registry ambiguity enabling semantic drift attacks, shared API key provenance loss eliminating fine-grained audit capability, or ETL metadata normalization stripping attribution fields. These are RAG and vector database security engineering concerns entirely outside the framework's scope.

### RIP_12 - Memory & Session Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of session persistence context pollution enabling long-duration cross-agent resource drain, checkpoint attribution ambiguity in multi-agent workflow forensics, unbounded iteration loop economic cycling attacks, memory persistence cost overhead denial-of-wallet, MCTS tree growth memory exhaustion in shared pools, semantic memory embedding cost exploitation, working memory context inflation cascading across agent hops, shared semantic memory trojanized solution propagation, cross-agent context poisoning through shared session persistence, memory serialization storage cost amplification, or model checkpoint integrity attacks activating dormant backdoors upon resumption.

### RIP_13 - Infrastructure & Deployment Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of container escape enabling agent-to-agent attacks on shared Kubernetes nodes, infrastructure cost manipulation through profiling data poisoning, NIM container image provenance ambiguity through floating tag exploitation, replica identity loss through generic pod template anonymity, GPU infrastructure cost attribution blurring across multi-agent deployments, TensorRT engine cache provenance loss enabling backdoor injection, MIG partition resource contention starvation, horizontal scaling replica identity ambiguity, or non-fungible engine binary identity spoofing through hardware migration. These are infrastructure and container security engineering concerns entirely outside this framework's scope.

### RIP_14 - ML/Training & Model Attacks
- Strength score: 1

The DIU RAI Guidelines' development worksheet includes "manipulation of data models" as a line of inquiry, providing broad relevance to training data integrity. However, the framework provides no guidance on preventing model selection as economic privilege boundary spoofing, MLflow model registry identity spoofing through semantic versioning range exploitation, quantization artifact provenance verification gaps in shared registries, behavioral fingerprinting-based identity forgery, training data provenance obfuscation through learned weight opacity, model ownership disputes through learning-based derivation chains, or training resource acquisition attacks preventing legitimate agents from updating policies.

### RIP_15 - Kubernetes & Container Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of Kubernetes service account identity pooling vulnerabilities enabling token replay and lateral movement, IP spoofing through iptables manipulation for inter-agent identity forgery, pod eviction priority-based agent importance disclosure, GPU resource request gaming for hardware hoarding, namespace multi-tenancy isolation bypass, vector database gossip protocol cluster node identity spoofing, or load balancer VIP masking eliminating node-level audit attribution. These are Kubernetes and container orchestration security concerns entirely outside the ethical framework's scope.

### RIP_16 - Load Balancer & Scaling Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of load balancer identity abstraction enabling agent impersonation through routing manipulation, load balancer endpoint interception for man-in-the-middle attacks, weighted load balancing privilege asymmetry disclosure through traffic pattern analysis, or auto-scaling triggered identity discontinuity enabling service impersonation through fresh replica state. These are infrastructure network security concerns requiring load balancer hardening and identity continuity mechanisms not addressed by an ethical AI framework.

### RIP_17 - Service Discovery & Authentication Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of kernel service registration provenance audit gaps, message queue consumer identity spoofing through tag reassignment, API gateway consumer identity header injection for rate limit bypass, Prometheus label cardinality explosion through unbounded agent identity provisioning, message queue priority starvation as economic denial-of-service, DNS cache poisoning affecting multi-agent service discovery, or certificate authority compromise enabling fleet-scale agent identity spoofing. These are network and infrastructure security concerns requiring cryptographic identity management far outside this framework's scope.

### RIP_18 - Rule/Method/Parameter Attribution Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of efficiency metric spoofing for agent identity deception, HTN method library authorship spoofing in shared registries, hierarchical delegation chain identity verification gaps, rule attribution confusion through multi-source aggregation, parameter origin confusion enabling poisoned parameter relay from compromised agents appearing as trusted sources, parameter source transitive trust boundary exploitation, provider economic lock-in through efficiency contract poisoning, method contributor accountability gaps in distributed registries, rule version control poisoning, or rule authorship spoofing through description manipulation. These require cryptographic attribution and provenance controls in AI planning systems.

### RIP_19 - Hybrid & Cross-Paradigm Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of paradigm attribution confusion in hybrid systems where poisoning shared paradigm components enables 1-to-N compromise across the agent ensemble, or identity spoofing through paradigm-specific credential mechanisms where different credential types across neural, symbolic, and utility paradigms create cross-paradigm authentication confusion. These are specialized hybrid AI system security concerns requiring paradigm-aware identity management and cryptographic binding not addressed by an ethical governance framework.

### RIDC_1 - UI and User Interaction Attack Surfaces
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires transparency of AI decisions and methods, which could discourage UI patterns that hide agent actions from oversight. However, the framework provides no specific guidance on preventing progressive disclosure layers from serving as multi-tier injection points for backend agents, securing chat attribution against social engineering through identity confusion in multi-agent contexts, defending ARIA live regions from persistent context injection, or blocking accessibility feature exploitation as out-of-band instruction channels. These are specialized prompt injection and UI security engineering concerns not addressed by an ethical oversight framework.

### RIDC_2 - Messaging and Protocol Injection
- Strength score: 1

The DIU RAI Guidelines have no coverage of FIPA-ACL performative spoofing enabling inter-agent control flow manipulation through semantic gaps between message intent and content, protocol language downgrade attacks exploiting heterogeneous serialization format weaknesses, or conversation-ID chain hijacking for cascading context pollution across multi-agent hierarchical orchestration. These are inter-agent communication protocol security concerns requiring message authentication, semantic validation, and conversation integrity mechanisms entirely outside the scope of an ethical AI framework.

### RIDC_3 - Framework and State Management Injection
- Strength score: 1

The DIU RAI Guidelines have no coverage of framework-enforced trust boundary violations through state schema coercion where malicious instructions pass framework validation but execute as control flow in downstream agents, cross-framework state migration enabling transitive instruction propagation as data becomes code during framework transitions, state schema field injection via reducer confusion creating instructions surviving in some agents' views, or conditional edge routing hijacking through state field manipulation. These are AI framework security engineering concerns requiring framework-level validation and sanitization controls not addressed by an ethical governance framework.

### RIDC_4 - Tool, Function Calling, and Plugin Injection
- Strength score: 1

The DIU RAI Guidelines have no coverage of memory-driven tool selection manipulation through conversation history injection, tool-calling schema instruction boundary collapse enabling "tool output laundering," RAG-retrieved tool metadata poisoning propagating to all agents querying shared registries, function calling parameter injection via type coercion across agent chains, implicit tool calling assumption violations between multi-agent boundaries, plugin-function metadata injection through decorator descriptions, semantic function template injection via variable interpolation, native function parameter type coercion as service dependency hijacking, plugin discovery protocol spoofing through schema ambiguity, or tool schema evasion through boundary misinterpretation. These are technical prompt injection controls requiring framework-level security engineering.

### RIDC_5 - ReAct and Reasoning Architecture Injection
- Strength score: 1

The DIU RAI Guidelines have no coverage of ReAct reasoning trace injection enabling cascading vulnerability through shared agent traces, unfaithful trace reuse as post-hoc rationalization injection, mechanistic interpretability opacity creating cross-agent blind spots where Agent B cannot validate Agent A's reasoning integrity, Plan-and-Execute planning phase conflation attacks embedding malicious instructions in persistent execution plans, HTN precondition smuggling, abstract task description as implicit instruction channels, constraint specification ambiguity enabling ordering manipulation, effect specification injection, causal link manipulation, method library proliferation hiding malicious decompositions, or reflection self-critique reinforcement hijacking amplifying injections through iterative generate-reflect-refine cycles. These require dedicated prompt injection defenses in AI reasoning architectures.

### RIDC_6 - Streaming and Real-Time Communication Injection
- Strength score: 1

The DIU RAI Guidelines have no coverage of streaming response manipulation through timing attacks that embed malicious instructions in middle sections of long responses, exploiting multiple inter-agent streaming handoff windows where one agent's streaming output becomes another's input before full sanitization is possible. These streaming injection vulnerabilities require real-time content validation and injection detection controls in multi-agent streaming pipelines not addressed by an ethical AI governance framework.

### RIDC_7 - Generation Parameter and Inference Configuration Injection
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle broadly requires performance monitoring and output verification. However, the framework provides no guidance on preventing evaluation dataset poisoning through adversarial production log injection propagating across all agents sharing evaluation datasets, defending against evaluation metric selection manipulation through poisoned natural language metric definitions, baseline measurement manipulation through agent substitution in multi-agent evaluation, evaluation orchestration control-flow hijacking through instruction injection into agent outputs, test dataset ordering attacks through non-deterministic parallel evaluation sequencing, evaluation context memory poisoning through persistent agent state pollution, metric calculation hijacking through compromised custom metric functions, or benchmark ground truth injection corrupting agent-to-agent trust relationships. These require evaluation security engineering controls not addressed by this framework.

---

## RMP - Long‑lived cognitive state abuse: memory poisoning and latent backdoors

### RMP_1 - UI/UX Memory Manipulation Attacks
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires transparency of AI data, methods, and decisions, and the development worksheet includes audit mechanisms as a line of inquiry. These provisions conceptually encourage maintaining clear records of AI cognitive state changes. However, the framework provides no specific guidance on preventing gradual session persistence poisoning through incremental injection across multi-agent systems, securing conversation history displays against provenance opacity that enables memory poisoning, defending against latent backdoor activation through context reference links, managing session state revalidation across multi-agent resumption, or detecting context window eviction-based security constraint removal. These are specialized AI memory security engineering concerns not addressed by an ethical governance framework.

### RMP_2 - Multi-Agent State Handoff and Cross-Agent Memory Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of API gateway routing manipulation for cross-agent tool delegation, gRPC schema versioning backward-compatibility exploitation for hidden malicious field injection, swarm intelligence local rule injection for emergent malicious collective behavior, cross-type memory poisoning via retrieval mechanism exploitation during agent handoffs, adversarial importance weighting for selective malicious memory retention, memory type misclassification through handoff serialization, semantic anchor embedding injection causing co-retrieval of malicious context, cross-agent vector embedding poisoning, knowledge graph relationship injection through agent coordination, or distributed graph traversal race conditions enabling inconsistent decision-making. These are specialized agentic AI memory security engineering concerns entirely outside this framework's scope.

### RMP_3 - Multimodal and Cross-Modal Poisoning Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of cross-modal instruction injection embedding malicious content in non-text modalities to bypass text safety filters, temporal sensor desync exploitation creating phantom consensus in multi-agent sensor fusion, adversarial cross-modal contradiction attacks manipulating consensus mechanisms, distributed memory poisoning through corrupted perception modules propagating as collective false beliefs, vision model output integration creating cross-session instruction persistence, multimodal RAG chunk poisoning hiding instructions in modality gaps, audio transcript memory corruption through Whisper hallucination persistence, DePlot chart linearization instruction injection, or CSS pseudo-element dual-channel content injection enabling selective agent execution. These are specialized multimodal AI security concerns entirely outside this framework's scope.

### RMP_4 - Framework-Specific Memory Vulnerabilities
- Strength score: 1

The DIU RAI Guidelines have no coverage of the extensive framework-specific memory vulnerabilities catalogued here, including cross-framework state poisoning through memory architecture mismatch transformation, distributed session resumption creating cross-framework memory corruption propagation, checkpoint persistence enabling temporally separated backdoor activation, ConversationBufferMemory long-term backdoor injection, agent scratchpad poisoning for reasoning history fabrication, memory key namespace collision enabling fleet-wide context injection, unsanitized tool output integration into persistent memory, AutoGen GroupChat shared history creating synchronized fleet compromise, CrewAI context inheritance deterministic propagation paths, Semantic Kernel service registration poisoning, or semantic function prompt cache enabling persistent response injection. These require framework-specific security hardening beyond the scope of an ethical oversight framework.

### RMP_5 - Caching and Persistence Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of shared embedding cache poisoning creating fleet-wide persistent latent backdoors, error recovery payload persistence through retry cycles where poisoned diagnostic context activates malicious instructions, fallback state caching establishing long-term cognitive corruption across agent boundaries, error message storage as backdoor activation vectors through "recommended action" instructions appearing as system diagnostics, retry state history manipulation embedding instructions in failure modes, or graceful degradation configuration persistence permanently disabling security capabilities. These require cache security and persistence validation engineering not addressed by an ethical governance framework.

### RMP_6 - Streaming and Real-time Processing Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of streaming context accumulation as persistent memory injection where mid-stream malicious instructions are stored before complete validation, progressive disclosure in streaming responses hiding persistence instructions in later-streamed content that executes before full stream validation, streaming token timing for distributed memory corruption exploiting synchronization windows, or conversation history injection through streaming response buffering at multi-agent handoff boundaries. These streaming AI security engineering concerns require real-time content validation and injection detection controls not addressed by this framework.

### RMP_7 - Evaluation and Metrics Poisoning
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle requires performance monitoring and output verification, broadly encompassing evaluation integrity. However, the framework provides no specific guidance on preventing shared baseline storage poisoning causing all downstream evaluation agents to compare against false reference points, adaptive evaluation metric learned patterns as latent backdoors, shared evaluation result cache poisoning through replay attacks, evaluation scratchpad poisoning with fabricated prior test records, progressive evaluation dataset corruption through distributed processing chains, or benchmark performance history poisoning creating fleet-wide pervasive latent backdoors through fabricated tool performance records. These require evaluation infrastructure security controls not addressed by this framework.

### RMP_8 - Parameter Tuning and Configuration Poisoning
- Strength score: 1

The DIU RAI Guidelines have no coverage of image metadata instruction injection via EXIF/XMP content exploited during vision model multi-agent processing, model-specific memory architecture tuning creating cross-model state corruption vectors during model swapping, context window truncation diversity enabling persistent targeted instructions across heterogeneous-window agent fleets, shared tuning dataset poisoning causing simultaneous fleet-wide memorization of adversarial patterns, hyperparameter configuration backend poisoning affecting all agents fleet-wide, or Pareto frontier configuration poisoning through baseline metric corruption causing malicious configurations to be permanently adopted as optimal. These require specialized AI system configuration security controls.

### RMP_9 - Learning and Training Data Attacks
- Strength score: 2

The DIU RAI Guidelines' development worksheet explicitly identifies "manipulation of data models" as a line of inquiry, directly applicable to the learning and training data attacks in this subsubsection. Few-shot CoT demonstration injection, adversarial trajectory data injection in learning flywheels, and fine-tuning data contamination through selection heuristic poisoning all fundamentally attack the data models used to train and guide agent behavior. The framework encourages examining how data models can be manipulated and establishes governance roles for ensuring data integrity, which provides moderate support for managing these risks. However, the framework does not prescribe specific technical controls for detecting poisoned training data, validating few-shot examples, or securing learning flywheels against adversarial trajectory injection.

### RMP_10 - Observability and Tracing Attacks
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires audit trails and transparency of AI decisions, which conceptually encourages robust observability infrastructure. However, the framework does not address trace timestamp clock skew exploitation for causality corruption in distributed multi-agent tracing, span metadata poisoning to hide critical tool invocations from filtering-based oversight, multi-agent attack architectures that exploit cross-agent root cause analysis cognitive difficulty, or confidence score injection through trace manipulation propagating false high-confidence reasoning across agent boundaries. These require cryptographic trace integrity and distributed observability security controls not addressed by this framework.

### RMP_11 - Learned Behavior and Memory Evolution
- Strength score: 1

The DIU RAI Guidelines have no coverage of hallucinated tool parameter persistence in shared multi-agent memory causing other agents to repeat hallucinations as grounded historical facts, or memory consolidation and deduplication exploitation causing evolved consolidated memory to contain injected instructions disguised as learned patterns that propagate as collective learned behavior across multi-agent systems. These require specialized AI memory lifecycle security controls not addressed by an ethical governance framework.

### RMP_12 - Context and Parameter Consistency
- Strength score: 1

The DIU RAI Guidelines have no coverage of memory slice attacks where heterogeneous query patterns across multi-agent systems enable targeted poisoning delivering different malicious memories to different agents, long-term cross-session memory poisoning amplified by multi-agent shared user memory, poisoned parameter history persistence in shared conversation memory propagating extraction errors as ground truth, parameter accuracy regression cascading across multi-agent sessions, tool selection consistency loss across agent boundaries creating parameter mismatches, or parameter validation state information loss in shared memory eliminating provenance of validated versus unvalidated parameters. These require multi-agent memory integrity and provenance controls not addressed by this framework.

### RMP_13 - Reasoning Transparency and Trust
- Strength score: 2

The DIU RAI Guidelines' "Traceable" principle directly addresses reasoning transparency, requiring that AI methods and decisions be transparent and auditable. The framework explicitly values explainability of AI decisions to enable human oversight. This directly relates to the weaponization of reasoning transparency: the Traceable principle's requirements could, if properly implemented, encourage defenses against reasoning trace injection by demanding verification that transparent reasoning actually reflects internal computation. The "Responsible" principle further requires human accountability for AI outcomes, which includes critically evaluating AI reasoning rather than being socially engineered by apparently rigorous logic. However, the framework does not prescribe specific technical defenses against reasoning confidence calibration manipulation or multi-agent reasoning disagreement exploitation for trust confusion.

### RMP_14 - Efficiency and Resource Tracking
- Strength score: 1

The DIU RAI Guidelines have no coverage of efficiency memory accumulation as persistent injection vectors where poisoned historical data in shared memory corrupts all agents' resource allocation decisions, conversation history summarization lossy compression as injection persistence, baseline metric degradation as silent attack activation triggering hidden payloads, cost attribution memory poisoning corrupting orchestration routing decisions, or efficiency insight persistence as learned backdoors embedding malicious optimization shortcuts as persistent policies. These are AI system operational security concerns requiring dedicated monitoring and validation controls not addressed by an ethical governance framework.

### RMP_15 - Vector Database and Embedding Poisoning
- Strength score: 1

The DIU RAI Guidelines have no coverage of embedding space poisoning through cross-provider model switching exploiting incompatible semantic spaces in shared vector databases, shared embedding cache corruption with adversarially crafted dimension-valid but semantically poisoned vectors, HNSW graph structure poisoning through efConstruction parameter manipulation during index rebuild causing persistent retrieval degradation, persistent volume-level vector database file modification creating durable corruption across agent restarts, or cluster replication protocol poisoning automatically propagating corrupted vectors to all replicas within seconds. These require vector database security engineering and integrity validation controls entirely outside the framework's scope.

### RMP_16 - ETL Pipeline and Data Processing Attacks
- Strength score: 1

The DIU RAI Guidelines' development worksheet's "manipulation of data models" line of inquiry could broadly encompass concern for ETL data pipeline integrity. However, the framework provides no specific guidance on preventing ETL state file timestamp backdating for persistent incremental update corruption, quality validation threshold degradation through incremental configuration drift, chunking overlap poisoning creating cross-chunk context contamination, adversarial embedding injection exploiting similarity thresholds to poison multiple queries simultaneously, query normalization collision attacks creating false cache key associations, deduplication logic manipulation through threshold tampering, citation database poisoning with fabricated authoritative source attributions, batch processing queue contamination, or MinHash fuzzy deduplication hash collision attacks enabling fleet-wide training data substitution. These require specialized data pipeline security engineering controls beyond this framework's scope.

---

## RND - Non‑determinism, continual change, and assurance gaps as a risk surface

### RND_1 - UI/Streaming and Real-Time Generation
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle requires AI systems to perform as intended and the deployment worksheet requires continuous evaluation. These provisions conceptually support requiring deterministic or reproducible behavior for safety-critical decisions. However, the framework provides no specific guidance on addressing streaming non-determinism defeating audit reproducibility, latency variability enabling sleeper agent activation only under production-specific timing conditions, stochastic sampling creating ephemeral malicious content windows before safety filtering completes, or inline suggestion race conditions between generation and filtering in distributed multi-agent pipelines. These require technical AI system engineering controls not provided by an ethical oversight framework.

### RND_2 - Progressive Disclosure and Error Handling
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires transparency and audit mechanisms, which conceptually encourages comprehensive logging of agent behaviors. However, the framework provides no guidance on managing exponentially complex progressive disclosure state spaces that make exhaustive validation impossible, preventing forensic blind spots where critical attack evidence resides in unexpanded technical layers, or addressing ephemeral error state non-determinism in multi-agent cascades where upstream errors are masked before propagating. These require specialized AI system observability and testing engineering not addressed by this framework.

### RND_3 - Context, Persistence, and Session Management
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle could encompass requirements for consistent AI behavior across sessions, and "Traceable" could require auditable session contexts. However, the framework does not address multi-turn attacks distributing malicious intent across conversation turns to evade stateless safety classifiers (achieving 80-95%+ success against safety-aligned models), or session persistence across UI state changes creating stale security context divergence across agents accessing state at different times. These require specialized multi-turn security controls not addressed by an ethical governance framework.

### RND_4 - Multi-Agent Coordination and Decision-Making
- Strength score: 1

The DIU RAI Guidelines have no coverage of chat interface streaming attribution confusion enabling simultaneous malicious instruction injection styled as trusted agent output, command palette context prediction non-determinism creating inconsistent guardrails across identical starting states, approval workflow confidence score non-determinism enabling repeated requests until auto-approval thresholds are met, or multi-agent dashboard race condition attack windows during brief periods of inconsistent state display. These require deterministic UI and coordination security engineering not addressed by this framework.

### RND_5 - Retry, Resilience, and Error Recovery
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle broadly requires system reliability, which could include error recovery. However, the framework provides no guidance on preventing hidden attack amplification through collapsed auto-retry error logs, exponential backoff timing non-determinism defeating audit reproducibility, non-idempotent operation exploitation via coordinated multi-agent retry storms, non-deterministic fallback route selection enabling untested malicious routing paths, circuit breaker threshold non-determinism, or graceful degradation decision non-determinism creating inconsistent agent security postures. These require resilience security engineering not addressed by this framework.

### RND_6 - Checkpoint, State, and Recovery Management
- Strength score: 1

The DIU RAI Guidelines have no coverage of checkpoint replay poisoning through adversarial state manipulation during capture-to-resumption windows, determinism violation amplification via cascading state divergence exploiting non-deterministic replay logic, or coordinated retry exhaustion attacks strategically injecting recoverable errors to mask critical failures while consuming resources. These are distributed systems security engineering concerns requiring cryptographic checkpoint integrity and deterministic recovery guarantees not addressed by an ethical AI framework.

### RND_7 - Tool Integration and API Management
- Strength score: 1

The DIU RAI Guidelines have no coverage of tool API drift creating cascading silent failures in multi-agent Plan-and-Execute architectures, tool schema version drift causing simultaneous multi-version operation failures, LLM function calling behavioral discontinuity from provider updates across agents using different versions, tool availability changes creating non-deterministic behavior from mismatched tool lists, function calling parameter type evolution creating silent type mismatch failures, or few-shot API calling example poisoning creating distributed vulnerabilities through unsafe learned defaults. These require API compatibility management engineering not addressed by this framework.

### RND_8 - Framework and Architecture Non-Determinism
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle requires systems to perform as intended, which conceptually supports managing behavioral non-determinism in AI systems. However, the framework provides no guidance on addressing combinatorial non-determinism from multi-pattern agent compositions where total state space makes exhaustive testing intractable, cross-framework multiplicative non-determinism creating infeasible comprehensive testing, or continual framework and model weight updates invalidating prior security evaluations across N(N-1)/2 agent-framework combinations. These require specialized AI assurance engineering not addressed by an ethical governance framework.

### RND_9 - LangChain/LangGraph Specific Non-Determinism
- Strength score: 1

The DIU RAI Guidelines have no coverage of LangGraph-specific non-determinisms including conditional edge evaluation variability from external factors, reducer behavior divergence across agents with different update expectations, checkpoint restoration path divergence in cyclic workflows, confidence score non-determinism defeating tool selection safety gates, memory update race conditions from concurrent multi-agent writes, model version update invalidating fleet-wide security evaluations, or tool API compatibility drift creating unpredictable error-triggered behaviors. These are framework-specific engineering concerns entirely outside this framework's scope.

### RND_10 - AutoGen Specific Non-Determinism
- Strength score: 1

The DIU RAI Guidelines have no coverage of AutoGen's message-driven architecture producing different conversation flows across identical inputs, preventing security testing from establishing unreachability of dangerous execution paths, GroupChat speaker selection non-determinism enabling probabilistic malicious agent selection exploitation, or multi-framework update cumulative assurance gaps requiring re-evaluation across infeasible agent×framework×LLM version combinations. These are framework-specific security engineering concerns outside this framework's scope.

### RND_11 - CrewAI Specific Non-Determinism
- Strength score: 1

The DIU RAI Guidelines have no coverage of CrewAI's hierarchical task routing non-determinism where identical delegations route differently across executions, enabling attacks that succeed only when routed to compromised workers to evade security testing that cannot cover all probabilistic routing outcomes. This is a framework-specific non-determinism concern entirely outside the scope of an ethical AI governance framework.

### RND_12 - Semantic Kernel Specific Non-Determinism
- Strength score: 1

The DIU RAI Guidelines have no coverage of Semantic Kernel's LLM-driven function routing producing non-deterministic plugin invocation paths defeating audit reproducibility, dynamic plugin registry state changes invalidating prior security evaluations without code changes, FunctionChoiceBehavior configuration drift across agents producing inconsistent routing, or service registration lifecycle non-determinism from concurrent agents observing different service sets during registration changes. These are framework-specific security concerns outside this framework's scope.

### RND_13 - Multimodal and Vision-Language Models
- Strength score: 1

The DIU RAI Guidelines have no coverage of multimodal model version drift creating inconsistent semantic spaces across agents using different model versions with shared storage, vision model output format inconsistencies across heterogeneous agent deployments creating processing ambiguities, Whisper model update drift creating behavior divergence between audio processing and dependent synthesis agents, embedding quantization inconsistency affecting similarity threshold behavior across agent fleets, or vision-language model temperature variation producing different outputs from identical images. These are multimodal AI engineering concerns outside this framework's scope.

### RND_14 - Evaluation, Testing, and Benchmarking Non-Determinism
- Strength score: 2

The DIU RAI Guidelines' "Reliable" principle explicitly requires performance monitoring, output verification, and systematic evaluation of AI systems, and the deployment worksheet mandates continuous evaluation procedures throughout the AI lifecycle. These requirements directly confront the challenges described in this subsubsection: the framework's mandate for reliable performance creates governance pressure for addressing non-deterministic evaluation results that defeat regression testing, managing evaluation metric model updates that shift baselines, and handling framework-dependent evaluation incomparability. The "Responsible" principle further requires accountable AI decision-making, which requires reproducible performance assessment. However, the framework does not prescribe specific technical methods for testing stochastic AI systems, managing multiple comparison problems in multi-agent statistical significance testing, or ensuring continuous evaluation keeps pace with multi-agent update proliferation creating assurance gaps.

### RND_15 - Temperature and Sampling Parameters
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle requires consistent AI behavior, which could broadly support requirements for managing sampling parameter variability. However, the framework provides no guidance on managing temperature-induced non-determinism across agent chains, continuous parameter re-tuning creating moving-target security assurance, or cross-agent parameter drift creating inconsistent security postures where high-temperature agents accept injections that low-temperature agents reject. These require AI system configuration management and behavioral consistency engineering not addressed by this framework.

### RND_16 - Parameter Extraction and Tool Calling
- Strength score: 1

The DIU RAI Guidelines have no coverage of stochastic parameter generation drift creating hallucination windows targeted by adversaries seeking high-entropy agents, tool documentation drift across agent training cycles creating knowledge inconsistencies, probabilistic tool selection creating adversarial search spaces for maximizing malicious tool selection probability, non-deterministic fallback ordering including malicious tool paths, continuous hallucination rate regression through updates outpacing grounding check recalibration, or trajectory variance creating non-deterministic execution paths that compound across multi-agent chains. These require AI system reliability engineering not addressed by this framework.

### RND_17 - Retrieval-Augmented Generation (RAG) and Multi-Hop QA
- Strength score: 1

The DIU RAI Guidelines have no coverage of ranking model manipulation through adversarially crafted high-relevance documents, semantic similarity exploitation in dense retrievers for multi-hop instruction injection, query reformulation injection enabling cross-agent instruction propagation, fake bridging document injection corrupting multi-agent multi-hop reasoning, weak evidence grounding enabling downstream agent trust in unverified poisoned content, source attribution confusion propagating through multi-agent aggregation, or knowledge graph hallucination blindness creating persistent corrupted relationship entries. These are RAG security engineering concerns outside this framework's scope.

### RND_18 - Infrastructure and Deployment Non-Determinism
- Strength score: 1

The DIU RAI Guidelines have no coverage of event delivery guarantee downgrade attacks through infrastructure resource exhaustion, Byzantine swarm agent injection at critical consensus threshold densities causing emergent failures invisible to monitoring, message queue ordering assurance loss under scaling, vector database index staleness creating invisible knowledge state gaps, Prometheus scrape interval latency creating compounded metric lag in multi-agent decision chains, API gateway canary deployment version mismatch across agent tool invocations, or MLflow version mismatch during rollouts creating heterogeneous data structure incompatibilities. These are infrastructure engineering concerns outside this framework's scope.

### RND_19 - Kubernetes and Container Orchestration
- Strength score: 1

The DIU RAI Guidelines have no coverage of HPA scaling behavior non-determinism creating variable agent population dynamics, StatefulSet ordinal initialization race conditions during coordinated agent restarts, network topology non-determinism from node autoscaling affecting multi-agent co-location patterns, or Kubernetes scheduler indeterminism enabling intermittent attacker-controlled pod placement that validation cannot reproduce under rare scheduling conditions. These are container orchestration engineering concerns entirely outside this framework's scope.

### RND_20 - Deployment and Inference Infrastructure
- Strength score: 1

The DIU RAI Guidelines have no coverage of container image drift from unpinned canary deployments creating multi-agent version skew, non-deterministic traffic splitting creating 2^N state combinations across N simultaneously updating agents, asynchronous event processing ordering non-determinism producing different agent results from identical events received in different orders, unobservable state transitions during rolling deployments creating incompatible agent interface combinations, serverless workflow checkpoint resume races, or rolling update non-determinism creating interface version mismatches across agent populations. These are deployment engineering concerns outside this framework's scope.

### RND_21 - Inference Hardware and Optimization
- Strength score: 1

The DIU RAI Guidelines have no coverage of quantization granularity non-determinism creating inconsistent inference behavior across agents using different quantization modes, GPU scheduler kernel fusion execution order variance, NIM replica floating-point rounding differences enabling probability-based attacks requiring specific probability distributions, dynamic batching indeterminism from request arrival timing, continuous model update perpetual assurance gaps where formal verification cannot complete before the next update, or hardware-specific TensorRT engine variations creating deployment-to-production behavioral mismatches. These are hardware and inference engineering concerns outside this framework's scope.

### RND_22 - Optimization and Efficiency
- Strength score: 1

The DIU RAI Guidelines have no coverage of profiling-induced non-determinism from measurement overhead creating behavioral differences between monitored and unmonitored execution, temperature and sampling parameter variability across optimization cycles creating agent behavioral divergence, asynchronous configuration updates causing agents to operate on different parameter versions simultaneously, or speculative decoding acceptance rate variability creating non-deterministic latency and output distributions not observable with fixed test sets. These are AI system optimization engineering concerns outside this framework's scope.

### RND_23 - Chain-of-Thought (CoT), Tree-of-Thought (ToT), and Reasoning Non-Determinism
- Strength score: 1

The DIU RAI Guidelines have no coverage of CoT path explosion in multi-agent coordination making red-teaming coverage computationally infeasible, reasoning consistency drift across individually updated agents creating version-specific poisoning vulnerabilities, temporal coordination race conditions where Agent A's incomplete reasoning is retrieved by Agent B, stochastic multi-agent planning creating exponentially untestable combined state spaces, or concurrent ToT backtracking state divergence from different branch exploration timing across distributed agent trees. These require specialized AI assurance testing methodologies not addressed by this framework.

### RND_24 - Self-Consistency and Multiple Reasoning Paths
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle could broadly support requirements for behavioral consistency, but the framework provides no guidance on managing Self-Consistency's intentional stochastic decoding creating assurance gaps for identical inputs, temperature parameter drift affecting all agents simultaneously when shared parameters change, continuous sampling optimization causing systematic adversarial drift where attackers exploit accumulating behavioral changes, or context window compression algorithm non-determinism at Self-Consistency scale creating variable information survival across agent handoffs. These require AI system reliability engineering not addressed by this framework.

### RND_25 - Hierarchical Task Network (HTN) Planning Non-Determinism
- Strength score: 1

The DIU RAI Guidelines have no coverage of probabilistic HTN decomposition method selection creating assurance gaps where testing one method provides no guarantee production selects the same method, temperature divergence across HTN planning agents producing incompatible decompositions, continual method library evolution creating version mismatch gaps across asynchronously updated agents, lack of decomposition reproducibility across multi-agent independent planners, partial order scheduling non-determinism in distributed execution, or constraint satisfaction heuristics producing divergent solutions. These are AI planning system engineering concerns outside this framework's scope.

### RND_26 - Monte Carlo Tree Search (MCTS) Planning Non-Determinism
- Strength score: 1

The DIU RAI Guidelines have no coverage of MCTS non-determinism from random seeding causing divergent planning trees across coordinated multi-agent systems, dynamic MCTS exploration constant adaptation creating heterogeneous exploration strategies, simulation budget variance from multi-agent resource competition forcing insufficient planning quality, replanning non-determinism exploitable by forcing dangerous recovery paths, asynchronous replanning state desynchronization attacks, or heuristic evaluation inconsistency creating distributed search space divergence. These are AI planning algorithm engineering concerns outside this framework's scope.

### RND_27 - Episode-Based Memory and Consolidation
- Strength score: 1

The DIU RAI Guidelines have no coverage of non-deterministic episode retrieval ranking enabling adaptive attacks that probe for unpredictable high-malicious-episode retrieval windows, consolidation timing non-determinism creating attack escalation windows when specific agents are absent, retrieval threshold sensitivity creating context-specific poisoning that avoids detection under most query conditions, trajectory length variability enabling attack activation only during long trajectories when monitoring is exhausted, embedding model update non-determinism creating transient poisoning windows during staged rollouts, or graph traversal path non-determinism enabling selective poisoning along specific exploration paths. These are specialized AI memory system engineering concerns outside this framework's scope.

### RND_28 - Knowledge Graphs and Memory Stores
- Strength score: 1

The DIU RAI Guidelines have no coverage of RAG result ranking non-determinism from score rounding creating correlated retrieval failures across all agents sharing ranking functions, knowledge graph traversal ordering non-determinism producing different multi-agent reasoning paths from identical graph states, temporal document validity window boundary flickering creating intermittent behavior depending on query timing, probabilistic knowledge fact evaluation non-determinism enabling adversarial variance exploitation, incremental knowledge base update partial-state divergence across querying agents, or cache invalidation timing creating temporary semantic divergence between cached and fresh data consumers. These are knowledge store engineering concerns outside this framework's scope.

### RND_29 - Context Assembly and Working Memory
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires auditable AI decisions, but the framework provides no guidance on addressing multi-agent context assembly non-determinism from variable retrieval ranking making reproducible auditing impossible, streaming generation token sequence variability across agent handoffs making comprehensive attack path testing infeasible, or distributed regression gaps from asynchronous multi-agent update propagation creating emergent untested behaviors at version boundaries. These require deterministic context management engineering not addressed by this framework.

### RND_30 - Utility and Decision Making
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle requires correct system decision-making, but the framework provides no guidance on managing Monte Carlo sampling variability in expected utility calculations causing divergent agent decisions from identical inputs, stochastic outcome distribution changes invalidating cached utility estimates while agents continue using stale values, non-deterministic utility weight adjustment mechanisms creating unsafe convergence in multi-agent shared optimization, or evaluation non-reproducibility preventing reliable identification of which utility-based decisions are safe. These require AI decision system engineering not addressed by this framework.

### RND_31 - Rule-Based and Adaptive Systems
- Strength score: 1

The DIU RAI Guidelines have no coverage of rule priority instability creating non-deterministic execution order where identical requests trigger different rule sequences across multi-agent systems with tunable priorities, learning rule instability from continuous shared rule refinement creating shifting security postures, or heuristic parameter drift through independent agent tuning cycles creating behavioral divergence that attackers exploit by crafting inputs exploiting specific agents' parameter states. These are adaptive AI system engineering concerns outside this framework's scope.

### RND_32 - Learning-Based Policies and Reinforcement Learning
- Strength score: 1

The DIU RAI Guidelines' development worksheet's "manipulation of data models" and the "Reliable" principle's performance monitoring requirements are tangentially relevant to RL policy non-determinism. However, the framework provides no guidance on managing learned policy stochastic components creating assurance gaps from identical-state variable outputs, online learning continuous drift defeating validation in deployment, exploration phase unpredictability creating dangerous behavioral gaps, experience replay temporal non-determinism from variable historical context, multi-agent RL emergent behavior unpredictability requiring computationally infeasible comprehensive coverage, training detail-sensitive weight heterogeneity across distributed training, or synchronized gradient adversarial perturbations enabling single attacks to affect multiple agents simultaneously.

### RND_33 - Hybrid and Heterogeneous Systems
- Strength score: 1

The DIU RAI Guidelines have no coverage of temperature heterogeneity across hybrid paradigms enabling attackers to selectively inject into high-temperature agents while failing against deterministic agents, cooperative cycle count non-determinism enabling attacks that activate only after specific convergence cycles, hybrid output synthesis streaming order non-determinism enabling order-specific instruction triggering, knowledge graph consistency assurance gaps during multi-agent concurrent evolution, or feedback loop timing non-determinism enabling attackers to force dangerous convergence paths through timing manipulation. These are hybrid AI system engineering concerns outside this framework's scope.

### RND_34 - Other Risks/Threats/Vulnerabilities worth noting
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle broadly supports requirements for consistent and testable AI system behavior, which conceptually encourages addressing the diverse non-determinism concerns catalogued in this subsubsection. However, the framework provides no specific technical guidance on the 19 distinct non-determinism failure modes enumerated here, including keyboard navigation testing fragility in multi-agent approval workflows, HITL approval non-reproducibility from adaptive confidence thresholds, auto-scaling replica configuration variability, batching timeout non-determinism, load balancing algorithm routing randomness, evaluation instability from framework version changes, CI/CD timing variations affecting timeout-based decisions, model update timing creating evaluation blind spot windows, difficulty classification continual drift creating adversarial resource budget evasion, semantic chunking boundary non-determinism across heterogeneous line ending implementations, deduplication hash collision race conditions in concurrent multi-agent extraction, pagination cursor non-determinism during concurrent REST API extraction, filesystem modification time resolution variability across OS platforms, batch subdivision ordering non-determinism during ETL failure recovery, or quality metric calculation variability across heterogeneous validation implementations. These require specialized AI system engineering controls entirely beyond this framework's scope.

## RTM - Telemetry and monitoring blind spots specific to cognitive and tool behavior

### RTM_1 - UI and Interface Telemetry Gaps
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires that AI decisions be auditable, and the development worksheet asks about audit mechanisms, but neither provides any technical guidance on the UI and interface telemetry gaps catalogued in RTM_1. The framework does not address monitoring blind spots in multi-agent UI layers, approval workflow telemetry deficiencies, human-computer interaction logging gaps, frontend rendering divergence from backend state, accessibility interface monitoring, HITL interface state capture failures, cross-agent approval step provenance loss, or any of the 17 distinct interface observability weaknesses. These require specialized instrumentation engineering entirely outside this governance worksheet's scope.

### RTM_2 - Framework-Specific Architecture and Logging Gaps
- Strength score: 1

The DIU RAI Guidelines' development worksheet asks about audit mechanisms and system performance monitoring in broad terms, but the framework provides no guidance on the 26 framework-specific architecture and logging gaps in RTM_2. Blind spots such as LangChain callback logging incompleteness, AutoGen multi-agent conversation recording gaps, LangGraph checkpoint-to-checkpoint state opacity, CrewAI role assignment audit deficiencies, cross-framework message translation logging loss, orchestration framework version drift creating behavioral divergence, or framework-specific telemetry schema incompatibility are all implementation-level engineering concerns entirely outside the scope of a principles-based responsible AI guidelines framework.

### RTM_3 - Tool Invocation and Function Calling Monitoring
- Strength score: 1

The DIU RAI Guidelines have no coverage of tool invocation and function calling monitoring blind spots. Threats such as function call parameter logging incompleteness, tool return value audit gaps, nested tool call provenance loss, parallel tool execution interleaving opacity, tool timeout and retry telemetry deficiencies, or cross-agent tool call attribution failures require dedicated instrumentation solutions not addressed by the "Responsible," "Traceable," "Reliable," "Equitable," and "Governable" ethical principles or the worksheet-based assessment methodology of this framework.

### RTM_4 - Multimodal and Streaming Response Processing
- Strength score: 1

The DIU RAI Guidelines provide no guidance on multimodal and streaming response monitoring challenges. Blind spots in image processing pipeline observability, audio transcription intermediate state logging, streaming token sequence audit capture, multimodal input-output correlation tracking, cross-modal consistency verification gaps, or streaming response interception and injection monitoring in multi-agent handoffs are implementation-level telemetry engineering concerns entirely outside this governance framework's scope. The framework's principles and worksheets do not address monitoring at the level of data modality or streaming protocol specifics.

### RTM_5 - Evaluation and Assessment Telemetry Gaps
- Strength score: 2

The DIU RAI Guidelines have meaningful indirect relevance to evaluation and assessment telemetry gaps. The "Reliable" DoD AI principle explicitly requires AI systems to perform with expected accuracy and resilience throughout their lifecycle, and the deployment worksheet mandates continuous evaluation procedures. The development worksheet's "system performance monitoring" and "output verification" lines of inquiry directly encourage capturing evaluation telemetry. However, the framework does not specify how to implement continuous evaluation instrumentation, address benchmark gaming through evaluation metric manipulation, close gaps in A/B test telemetry for multi-agent deployments, detect evaluation dataset contamination, or ensure evaluation reproducibility under non-determinism. Its contribution is to mandate that evaluation monitoring must exist and be continuously applied, without technical guidance on closing the specific telemetry gaps RTM_5 catalogues.

### RTM_6 - Metrics Collection and Manipulation Evasion
- Strength score: 1

The DIU RAI Guidelines' "Reliable" and "Governable" principles conceptually support robust metrics collection and organizational accountability for AI performance, but the framework provides no guidance on the 32 distinct metrics manipulation and evasion techniques in RTM_6. Threats such as tool invocation observability reduction through delegation hierarchies, metrics poisoning through agent confidence aggregation bias, cross-agent context correlation blindness in anomaly detection, efficiency metric aggregation obscuring attack signals, sampling-based monitoring blind spots enabling attacker timing, latency percentile reporting masking attack spikes, historical baseline staleness creating false negatives, or telemetry processing latency as a detection evasion window require technical telemetry engineering defenses entirely beyond this governance framework's scope.

### RTM_7 - Infrastructure and Observability Stack Blind Spots
- Strength score: 1

The DIU RAI Guidelines have no coverage of the 48 infrastructure and observability stack blind spots catalogued in RTM_7. Technical threats such as tool behavior attribution loss in multi-agent shared tool calls, Prometheus scrape success masking agent-level failures, API gateway request success rates hiding semantic tool failures, MLflow deployment success hiding model performance regressions, Prometheus alert suppression creating cognitive behavior blindness, Kubernetes API server audit log cardinality overload, distributed tracing performance overhead creating instrumentation bias, observable metrics obscuring non-observable internal agent states, or log aggregation timestamp skew preventing accurate causality reconstruction require infrastructure-level monitoring engineering entirely outside the scope of a principles-based DoD responsible AI governance framework.

### RTM_8 - Reasoning and Cognitive State Monitoring
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires that AI reasoning be auditable, which creates a high-level organizational mandate to monitor cognitive state, but the framework provides no technical guidance on the 28 reasoning and cognitive state monitoring blind spots in RTM_8. Specific threats such as reasoning mutation defeating syntactic anomaly detection, coordination intent invisibility in distributed multi-agent reasoning, meta-reasoning structures concealing decision factors from monitoring, tree-of-thought search pattern analysis enabling behavior prediction, MCTS tree statistics as hidden cognitive state, rollout policy drift as unmonitored behavior change, or cross-agent constraint conflict detection blind spots require specialized AI monitoring instrumentation that far exceeds the worksheet-based assessment approach of this framework.

### RTM_9 - Memory Systems and Knowledge Base Observability
- Strength score: 1

The DIU RAI Guidelines have no coverage of memory system and knowledge base observability challenges. Blind spots such as episodic memory retrieval as monitoring evasion, memory consolidation process opacity hiding learning attacks, trajectory integration hiding multi-step attacks within legitimate-appearing problem-solving, hybrid storage architecture creating asymmetric monitoring coverage, working memory phase transitions creating evidence-erasing monitoring gaps, token budget visualization creating false confidence, reasoning trace truncation hiding decision provenance, or hierarchical compression observation opaqueness creating semantic monitoring failures are knowledge system engineering concerns entirely outside the scope of the principles and worksheet methodology of this framework.

### RTM_10 - Decision Logic and Utility Function Monitoring
- Strength score: 1

The DIU RAI Guidelines' development worksheet asks about "output verification" and "audit mechanisms," which tangentially implies monitoring decision outputs, but the framework provides no guidance on the 23 decision logic and utility function monitoring blind spots in RTM_10. Technical threats such as utility function calculation telemetry gaps making poisoned utility functions undetectable, weight-driven decision divergence appearing as normal variation, probability distribution shift impact on utility decisions, specification gaming through utility metric redefinition, reward metric gaming as learned monitoring evasion, gradient flow monitoring limitations for distributed learning, or knowledge graph evolution telemetry opacity require AI decision system monitoring engineering entirely beyond this governance framework's scope.

### RTM_11 - Vector Database and RAG Pipeline Telemetry
- Strength score: 1

The DIU RAI Guidelines have no coverage of vector database and RAG pipeline telemetry blind spots. Technical monitoring gaps such as vector database query quality metrics absence in production monitoring, hybrid search alpha parameter selection rationale invisibility, cluster shard-level query performance attribution gaps, batch ingestion error rate aggregation hiding document-level failures, ETL quality rejection metrics aggregation hiding per-source failures, cache hit rate manipulation as performance monitoring bypass, deduplication effectiveness tracking blind spots, or fault tolerance monitoring gaps hiding cascading failure patterns are infrastructure and data pipeline engineering concerns entirely outside the scope of a principles-based responsible AI governance framework.

### RTM_12 - Detection Evasion and Attack Exploitation
- Strength score: 1

The DIU RAI Guidelines' "Governable" and "Traceable" principles create organizational mandates for AI oversight and audit, which conceptually encourage robust monitoring infrastructure, but the framework provides no guidance on the 17 detection evasion and attack exploitation blind spots in RTM_12. Specific threats such as monitoring metric manipulation masking multi-agent performance degradation, alert fatigue exploitation through false positive flooding, time-to-detect exploitation through metric reporting delay manipulation, Prometheus metrics collection manipulation enabling man-in-the-middle falsification, alert rule threshold gaming through coordinated degradation across agent boundaries, cross-agent safety violation correlation blindness from independent guardrail telemetry, distributed sandbox audit trail fragmentation preventing cross-agent attack correlation, or fairness audit trail fragmentation preventing discrimination root cause analysis require security engineering for monitoring infrastructure far beyond this governance worksheet framework's scope.

## RTE - Multi-agent trust exploitation and self-replicating prompt malware

### RTE_1 - Dashboard & UI Attribution Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of dashboard and UI attribution attacks. Threats such as multi-agent dashboard attribution spoofing through visual similarity, agent impersonation through message source field manipulation, and evaluation dashboard attribution spoofing exploiting self-reported metadata rather than cryptographic attestation are UI security engineering concerns entirely outside the scope of this principles-based governance framework. The framework's five DoD AI ethical principles and worksheet-based lifecycle assessment do not address authentication binding between agent identities and UI representations, nor any of the cryptographic or protocol-level controls needed to prevent attribution spoofing.

### RTE_2 - Trust Mechanisms & Inter-Agent Communication
- Strength score: 1

The DIU RAI Guidelines' "Governable" principle requires meaningful human control over AI systems, which at a very high level encourages oversight of inter-agent communications, but the framework provides no guidance on the specific inter-agent trust exploitation threats in RTE_2. Threats such as agent-to-agent communication displayed as trusted analysis bypassing human oversight, transitive trust exploitation via agent reputation anchoring, inter-agent trust exploitation via circular verification loops where both primary and verifying agents are compromised, or agent specialization trust collapse through credential assumption are multi-agent architecture security concerns requiring technical controls well beyond this governance worksheet's scope.

### RTE_3 - Confidence & Scoring Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of confidence score manipulation attacks. Threats such as confidence score inflation through temperature and sampling parameter tuning enabling trust manipulation, confidence score poisoning through utility-based risk assessment function manipulation, and streaming response confidence manipulation through progressive revelation exploit technical AI system internals that the framework's ethical principles and worksheets do not address. The framework asks whether output verification mechanisms exist but provides no guidance on preventing confidence score-based social engineering in multi-agent trust chains.

### RTE_4 - Prompt Injection via Message/History Sharing
- Strength score: 1

The DIU RAI Guidelines have no coverage of self-replicating prompt injection attacks through shared conversation history, agent memory serialization, or framework-specific message history mechanisms such as AutoGen GroupChat. These threats exploit the architectural property that multi-agent systems share context across agent boundaries, enabling malicious instructions to propagate geometrically across agent networks. The framework's "Responsible" and "Governable" principles broadly support safe AI behavior but provide no technical guidance on prompt injection prevention, input sanitization, or context isolation controls needed to address these threats.

### RTE_5 - UI & Disclosure Vulnerabilities
- Strength score: 1

The DIU RAI Guidelines have no coverage of multi-agent trust escalation through progressive disclosure layer poisoning or agent dashboard role confusion through dynamic agent assignment. These threats exploit information asymmetry in UI architectures where specialized agents contribute to different disclosure layers and roles are dynamically assigned through manipulable orchestration decisions. The framework's ethical principles and worksheet methodology do not address UI security architecture, dynamic role assignment security, or the trust indicators displayed to users in multi-agent collaboration interfaces.

### RTE_6 - Tool & Command Injection
- Strength score: 1

The DIU RAI Guidelines have no coverage of prompt injection malware propagation via inline suggestions or command palette agent suggestion manipulation through context poisoning. These threats exploit the trusted authority of agent-generated recommendations in editing and command interfaces, embedding instructions that downstream agents execute as user-intended content. The development worksheet asks about output verification but provides no technical guidance on sanitizing agent-generated suggestions, validating inline recommendations, or preventing command injection through context poisoning in multi-agent tool interfaces.

### RTE_7 - Framework-Specific Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of framework-specific trust exploitation and prompt malware propagation through LangChain, LangGraph, AutoGen, CrewAI, or Semantic Kernel delegation architectures. Threats such as transitive privilege escalation through framework trust chain exploitation, framework-specific communication protocol exploits enabling worm-like propagation, and conditional edge message passing as instruction propagation channels in LangGraph are implementation-level security concerns requiring technical controls specific to each framework's architecture. These fall entirely outside the scope of a principles-based DoD responsible AI governance framework.

### RTE_8 - AutoGen-Specific Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of AutoGen-specific social engineering threats. Conversational AI-to-AI manipulation enabling peer agents to be persuaded through natural language dialogue to perform unauthorized actions, and agent reputation exploitation in AutoGen's conversation-based selection creating false consensus from compromised agents, are framework-specific vulnerabilities requiring technical countermeasures within AutoGen's architecture. The framework's ethical principles and worksheet assessments do not extend to agentic framework implementation security.

### RTE_9 - CrewAI-Specific Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of CrewAI-specific attack vectors. Manager-worker trust exploitation through task delegation, few-shot decomposition pattern injection in CrewAI manager agents teaching dangerous subtask patterns, and demonstration bias enabling semantic subtask reinterpretation that propagates harmful operations through hierarchical chains are all implementation-specific security concerns for the CrewAI hierarchical agent framework. The framework's governance worksheets assess AI system behavior at a high organizational level without addressing agentic framework architecture security.

### RTE_10 - Semantic Kernel-Specific Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of Semantic Kernel-specific attack vectors including plugin registry takeover enabling substitution of malicious plugins under legitimate names and function impersonation through schema duplication causing probabilistic routing to malicious plugin variants. These threats exploit Semantic Kernel's dynamic plugin registration and LLM-based function selection architecture. The framework's principles-based approach and worksheet methodology do not address plugin security, registry integrity, or function routing manipulation in AI orchestration frameworks.

### RTE_11 - Multimodal Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of multimodal trust exploitation attacks. Threats such as multimodal content attribution spoofing in agent collaboration displays, self-replicating image injection through inline multimodal suggestions spreading to all agents processing shared documents, cross-agent modality contradiction attacks manufacturing false consensus through exploitable weighting heuristics, and multimodal worm propagation through RAG retrieval cycles bypassing text-based defenses are specialized multi-agent security threats combining vision, audio, and language modalities. These require technical multimodal security controls entirely outside this governance framework's scope.

### RTE_12 - Streaming & Continuous Processing
- Strength score: 1

The DIU RAI Guidelines have no coverage of streaming and continuous processing as social engineering and malware propagation channels. Threats such as retry failure messaging as inter-agent social engineering embedding instructions in error communications, fallback routing announcements weaponized as malware propagation channels, and circuit breaker status announcements as coordinated instruction propagation mechanisms exploit multi-agent error coordination protocols as attack surfaces. These are streaming and resilience pattern security concerns entirely outside the scope of the ethical principles and lifecycle assessment worksheets of this framework.

### RTE_13 - Evaluation & Benchmarking
- Strength score: 1

The DIU RAI Guidelines' "Reliable" principle requires AI systems to perform accurately, and the deployment worksheet mandates continuous evaluation, which conceptually encourage evaluation integrity. However, the framework provides no guidance on the seven evaluation and benchmarking attack vectors in RTE_13. Threats such as self-replicating evaluation metric definitions injected through agent communication, transitive trust exploitation in multi-agent evaluation chains, evaluation framework version poisoning through dependency manipulation, evaluation report format injection, cross-agent metric comparison showing false consensus, benchmark leaderboard manipulation as agent reputation spoofing, and benchmark-based agent selection creating trust exploitation vectors require evaluation security engineering not addressed by this governance framework.

### RTE_14 - Web & LLM Evaluation Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of intermediate question answering output poisoning in decomposed multi-hop query architectures. This attack injects instructions into early sub-question answers in chain-of-thought decomposed query pipelines, propagating malicious directives to later execution stages. The framework's evaluation and output verification worksheet questions do not address query decomposition security, multi-hop reasoning pipeline poisoning, or the injection vectors specific to retrieval-augmented and decomposed question answering systems.

### RTE_15 - Model Tuning & Configuration Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of model tuning and configuration exploitation for multi-agent trust attacks. Threats such as model size selection as a vector for social engineering by exploiting capability-trust correlations, prompt cache poisoning for persistent malware establishment across shared multi-agent infrastructure, iteration budget exploitation creating detection windows in multi-agent trust chains, and adaptive routing configuration manipulation for transitive trust exploitation are model configuration security concerns requiring technical controls beyond the governance principles and worksheet methodology of this framework.

### RTE_16 - Reasoning Trace & Chain-of-Thought Attacks
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires AI reasoning to be auditable and understandable, which creates an organizational mandate relevant to chain-of-thought integrity, but the framework provides no technical guidance on reasoning trace attacks. Threats such as CoT-wrapped social engineering embedding manipulation within ostensibly legitimate analytical reasoning and reasoning trace as worm propagation vector weaving malware into stored chain-of-thought explanations that agents naturally retrieve and incorporate require AI security engineering controls for reasoning systems entirely outside this framework's scope.

### RTE_17 - Tree of Thought & Sampling Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of tree-of-thought and sampling attacks exploiting multi-agent trust. Threats such as quality score consensus enabling trust exploitation in RASC voting systems, sampling diversity enabling multi-agent proof-of-exploitation through demonstrated attacks across multiple reasoning paths, majority voting as consensus building for transitive trust manipulation, and path quality inflation as credential spoofing for propagating instructions with enhanced trust are AI reasoning system security concerns entirely outside the scope of a principles-based responsible AI governance framework.

### RTE_18 - Hierarchical Task Network (HTN) & Planning Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of HTN and planning attack vectors for multi-agent trust exploitation. Threats such as hierarchical authority confusion enabling privilege confusion attacks where lower-authority agents claim strategic status, method library authority spoofing through collaborative method injection establishing malware with equal standing to trusted methods, and decomposition delegation chain authority diffusion enabling malware propagation where each agent trusts upstream authority without verification are HTN planning architecture security concerns requiring technical controls beyond this governance framework's scope.

### RTE_19 - Monte Carlo Tree Search (MCTS) Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of MCTS-specific attack vectors in multi-agent trust exploitation. Threats such as non-deterministic rollout exploitation enabling emergent malicious behaviors, backpropagation poisoning in shared multi-agent value networks propagating false reward signals through all agents, and hierarchical MCTS delegation as privilege escalation where compromised worker MCTS produces dangerous sequences beyond supervisor authorization are planning algorithm security concerns entirely outside the scope of the DoD responsible AI ethical principles and worksheet methodology.

### RTE_20 - Multi-Agent Planning Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of multi-agent planning attack vectors. Threats such as planning phase reasoning falsification enabling injection of directives into plan-and-execute architectures, reasoning consistency loss across hierarchical boundaries enabling contradiction injection at management level, delegation context reasoning injection embedding attacks in manager reasoning inherited as worker context, and plan-and-execute hierarchical trust poisoning where compromised workers inject subtasks supervisors accept without verification are multi-agent planning architecture security concerns requiring technical controls beyond this governance framework's scope.

### RTE_21 - Memory & Knowledge Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of memory and knowledge system attacks for multi-agent malware propagation. Threats such as episodic memory as a trust bridge for worm propagation through shared experience stores, trajectory-based policy convergence enabling coordinated attack through poisoned collective experience, memory abstraction as attack template generation for agent spawning, shared vector database embedding poisoning for unanimous retrieval corruption across all querying agents, and consolidation-driven abstraction creating malware "genes" that agents assemble during reasoning are knowledge system security concerns entirely outside the scope of this governance worksheet framework.

### RTE_22 - Knowledge Base & RAG Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of knowledge base and RAG pipeline attack vectors for multi-agent malware distribution. Threats such as shared knowledge base as 1-to-N malware propagation vector through document poisoning, RAG-based instruction self-replication through iterative retrieval cycles where malicious documents guide subsequent fetches, cross-document instruction assembly from retrieved fragments composing executable malware, and synonym injection enabling instruction obfuscation through poisoned semantic similarity relationships require RAG pipeline security engineering entirely outside this governance framework's scope.

### RTE_23 - Shared Context & Aggregation
- Strength score: 1

The DIU RAI Guidelines have no coverage of shared context and aggregation attack vectors. Threats such as message-passing injection through context accumulation in shared working memory pools functioning as persistent propagation channels, hierarchical aggregation authority diffusion enabling distributed social engineering where high-confidence leaf-level injections propagate through aggregation trust chains, and message selective context sharing creating information privilege escalation where summarization hides dangerous details from receiving agents are multi-agent context architecture security concerns entirely outside the scope of a principles-based responsible AI governance framework.

### RTE_24 - Utility & Preference Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of utility and preference manipulation in multi-agent trust exploitation. Threats such as preference convergence attacks where gradual poisoning of shared learning data causes all agents to converge to malicious utility functions, and utility-based social engineering through recommendation chain poisoning where upstream agents' utility assessments are trusted by downstream agents without independent verification require AI preference learning security controls beyond the ethical principles and lifecycle worksheets of this governance framework.

### RTE_25 - Multi-Agent Reinforcement Learning (MARL)
- Strength score: 1

The DIU RAI Guidelines have no coverage of MARL-specific attack vectors. Threats such as multi-agent learned coordination as social engineering where poisoned reward structures cause agents to develop implicit malicious agreements, self-replicating reward signal injection through shared experience replay buffers enabling policy poisoning propagation, consensus learning manipulation through synchronized poisoning of majority agents, learned message protocol exploitation in conversational agent learning, and emergent malicious behavior through multi-agent policy evolution where dangerous behaviors arise as unintended consequences of learned coordination are reinforcement learning security concerns entirely outside this governance framework's scope.

### RTE_26 - Hybrid System Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of hybrid system attacks for multi-agent trust exploitation. Threats such as paradigm-specific instruction encoding for agent-to-agent malware propagation exploiting semantic differences between neural, symbolic, and utility-based paradigms where instructions appear as data in one paradigm but become executable when transiting agent boundaries, and knowledge graph topological backdoor enabling distributed malware coordination through covert communication channels established via injected graph relationships are hybrid AI architecture security concerns entirely outside the scope of this principles-based governance framework.

### RTE_27 - Infrastructure & Deployment Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of infrastructure and deployment attack vectors exploiting multi-agent trust. Threats such as vector database multi-tenancy exploitation for cross-agent context leakage through partition key confusion, Prometheus relabeling configuration injection for metric spoofing causing compromised agent metrics to appear as trusted agent data, API gateway rate limiting bypass through cooperative multi-agent request distribution, and MLflow experiment tracking metadata injection for malware distribution disguised as legitimate experiment results are infrastructure security engineering concerns entirely outside the scope of a principles-based responsible AI governance framework.

### RTE_28 - Microservices & Kubernetes
- Strength score: 1

The DIU RAI Guidelines have no coverage of microservices and Kubernetes-specific attack vectors in multi-agent deployments. Threats such as inter-service communication authentication bypass via TLS downgrade, service mesh authorization policy bypassing through misconfiguration enabling lateral movement between agents, agent identity spoofing via shared service account credentials, service account token reuse across agent instances, ClusterRole privilege escalation through agent API access, mutual TLS certificate poisoning in service mesh, and registry image signature spoofing for pod replica poisoning are container orchestration and zero-trust networking security concerns entirely outside the scope of a governance worksheet framework.

### RTE_29 - Performance Optimization & Model Registry
- Strength score: 1

The DIU RAI Guidelines have no coverage of performance optimization and model registry attack vectors. Threats such as performance optimization recommendation propagation as malware vector where optimization agents' recommendations propagate malicious directives as legitimate operational guidance, model registry version selection manipulation causing all querying agents to simultaneously load compromised models through shared registry infrastructure, and profiling tool integration as persistent backdoor installation mechanism leveraging elevated privileges to affect the entire multi-agent fleet are deployment infrastructure security concerns requiring technical controls entirely beyond this governance framework's scope.

### RTE_30 - Scaling & Auto-Scaling
- Strength score: 1

The DIU RAI Guidelines have no coverage of scaling and auto-scaling attack vectors in multi-agent trust exploitation. Threats such as pod anti-affinity rule manipulation enabling targeted agent isolation by forcing high-availability replicas to co-locate defeating resilience objectives, and RBAC privilege escalation within agent service accounts enabling leveraging of misconfigured permissions to manipulate other agents' configurations are Kubernetes infrastructure security concerns entirely outside the scope of the DoD responsible AI ethical principles and worksheet-based assessment methodology.

### RTE_31 - Fleet Management & Provisioning
- Strength score: 1

The DIU RAI Guidelines have no coverage of fleet management and provisioning attack vectors. Threats such as over-the-air update chain of custody corruption poisoning the baseline model reference used for staged health checks, distributed engine validation weakness in staged rollouts enabling backdoored TensorRT engines that pass staging but activate on production hardware variants, provisioning token compromise enabling self-replicating malware that programmatically generates additional tokens across newly provisioned hardware, and certificate rotation desynchronization creating trust chain attack windows are edge fleet deployment security engineering concerns entirely outside this governance framework's scope.

### RTE_32 - Batching & Caching Infrastructure
- Strength score: 1

The DIU RAI Guidelines have no coverage of batching and caching infrastructure attack vectors for multi-agent trust exploitation. Threats such as load balancer as malware propagation vector through systematic routing of requests through compromised replicas and auto-scaling threshold manipulation enabling coordinated malware activation where scaling events trigger simultaneous activation of latent malware across all new instances are infrastructure orchestration security concerns requiring technical controls entirely beyond the ethical principles and lifecycle assessment worksheets of this governance framework.

### RTE_33 - Other Risks/Threats/Vulnerabilities Worth Noting
- Strength score: 1

The DIU RAI Guidelines have no coverage of the additional multi-agent trust exploitation threats catalogued in this section, including trace-based few-shot learning poisoning through compromised execution histories, parameter accuracy assumptions between agents without verification, tool call verification bypasses through multi-agent trust chains, parameter validation delegation without re-validation, inter-agent context pollution enabling transitive instruction injection, conversation ID chain hijacking, efficiency metric sharing as malware propagation channel, trust degradation through efficiency report poisoning, cost optimization policy self-replication through agent communication, efficiency consensus exploitation for coordinated attack, cluster node trust exploitation through unauthenticated gossip protocol, load balancer trust assumption enabling MITM between agents and vector database nodes, fact-checking rail cascaded verification gaming, execution rail resource limit coordination failures enabling fleet-wide quota exhaustion, multi-LLM NIM model source trust exploitation through safetensors validation bypass, and LLM-specific NIM hardware detection fallback exploitation through vLLM performance degradation. These are specialized multi-agent infrastructure and trust architecture security concerns entirely outside the scope of a principles-based responsible AI governance framework.

## RWA - Workflow and ecosystem attacks on plugins, tools, and RAG pipelines

### RWA_1 - Specification Gaming and Misalignment
- Strength score: 2

The DIU RAI Guidelines have meaningful indirect relevance to specification gaming and misalignment. The "Reliable" DoD AI principle explicitly requires AI systems to perform their intended functions with appropriate accuracy and resilience, which directly addresses the concern that agents must behave as specified rather than gaming metrics. The "Governable" principle requires that AI systems permit human override and intervention, creating a governance layer that could catch emergent misalignment before it compounds. The development worksheet's "output verification" line of inquiry encourages organizations to confirm that AI outputs match intended behavior rather than metric proxies. However, the framework provides no technical guidance on the 149 specific gaming vectors in RWA_1, including adaptive threshold manipulation, plan-and-execute goal drift, auction protocol gaming, conditional routing hijacking, tool result manipulation, circuit breaker gaming, quantization-based attack vectors, MCTS reward exploitation, or PII detection circumvention. Its contribution is an organizational mandate that misalignment should be detected and corrected, without specifying how.

### RWA_2 - Model Training and Backdoors
- Strength score: 2

The DIU RAI Guidelines' development worksheet explicitly asks about "manipulation of data models," which directly applies to the training-time backdoor injection and data poisoning threats catalogued in RWA_2. This makes the framework's relevance more concrete than for purely operational threats: organizations implementing this worksheet should explicitly consider whether training datasets, fine-tuning corpora, benchmark datasets, LoRA adapters, embedding models, or calibration datasets have been manipulated. The "Reliable" principle further requires monitoring for performance degradation that backdoors could cause post-activation. However, the framework provides no technical guidance on the 60 specific backdoor mechanisms in RWA_2, including framework-embedded model assumption triggers, fine-tuned tool-calling behavior backdoors, CUDA kernel injection, speculative decoding draft model poisoning, or quantization calibration data manipulation. Its worksheet question establishes the right organizational question to ask without providing implementation controls.

### RWA_3 - State and Context Poisoning
- Strength score: 1

The DIU RAI Guidelines have no coverage of multi-agent state and context poisoning attacks. Threats such as decision path manipulation via stateful context poisoning in orchestration systems, LangGraph routing logic poisoning via fine-tuned state interpretation, checkpoint-enabled goal drift through state isolation exploitation, conditional edge logic injection through state field type confusion, reducer custom logic exploitation for cross-agent state corruption, stateful web navigation context poisoning, ETL state file poisoning for persistent extraction manipulation, or cross-agent context loss through distributed memory fragmentation are workflow state engineering concerns requiring technical controls entirely outside this governance framework's scope.

### RWA_4 - RAG Pipeline Attacks
- Strength score: 2

The DIU RAI Guidelines' development worksheet's "manipulation of data models" line of inquiry has indirect but meaningful applicability to RAG pipeline attacks. RAG corpora, knowledge graphs, and retrieval indices are data sources that feed AI model behavior, and an organization implementing this worksheet question should consider whether those knowledge sources could be poisoned or manipulated. The "Reliable" principle requires AI performance across the full operational lifecycle, including the retrieval quality that RAG systems provide. However, the framework provides no technical guidance on the 39 specific RAG attack vectors in RWA_4, including knowledge graph manipulation across agent hierarchies, retrieval parameter tuning enabling poisoned document propagation, HNSW graph navigation manipulation through layer-based poisoning, hybrid search alpha parameter poisoning, citation verification bypass through adversarial source attribution spoofing, or lazy retrieval timing exploitation. Its contribution is an organizational question that implicitly covers knowledge base integrity without specifying technical RAG security controls.

### RWA_5 - Multi-Agent Orchestration Risks
- Strength score: 1

The DIU RAI Guidelines' "Governable" principle requires human control over AI systems and the development worksheet asks about "governance roles," which at a very high level encourages oversight of AI orchestration architectures. However, the framework provides no guidance on the 52 specific multi-agent orchestration threats in RWA_5, including API gateway amplification attacks, swarm emergent misalignment through local rule exploitation, task boundary confusion exploitation, hierarchical orchestration privilege escalation, federated orchestration boundary exploitation via weakest link compromise, state-logic boundary exploitation, inter-agent communication filter bypass, cross-agent filesystem sharing through volume mount misconfiguration, fairness constraint bypass through request routing, or cascading autonomous actions creating uncontrolled operational commitment. These are orchestration architecture security engineering concerns entirely outside this governance framework's scope.

### RWA_6 - Plugin and Tool Ecosystem Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of plugin and tool ecosystem attack vectors. Threats such as plugin marketplace search result ranking manipulation, framework-dependent plugin integration enabling supply chain attacks, plugin middleware injection via tool wrapper manipulation, fine-tuned model adaptation creating ecosystem-specific backdoors, plugin supply chain attacks via package feed compromise, function description semantic drift enabling cross-plugin exploitation, NIM container image supply chain poisoning, model repository persistent volume poisoning for multi-model ecosystems, load balancer routing enabling tool enumeration, or reasoning-guided plugin abuse chain orchestration are tool ecosystem security engineering concerns entirely outside the scope of this principles-based responsible AI governance framework.

### RWA_7 - Evaluation and Monitoring Bypass
- Strength score: 2

The DIU RAI Guidelines have meaningful indirect relevance to evaluation and monitoring bypass. The "Reliable" principle explicitly requires AI systems to perform accurately throughout their lifecycle, and the deployment worksheet mandates continuous evaluation procedures, which creates an organizational imperative that evaluation and monitoring must be robust and trustworthy. Adversarial evaluation bypass directly undermines the continuous monitoring the deployment worksheet mandates. However, the framework provides no technical guidance on the 19 specific bypass techniques in RWA_7, including model behavior divergence between training and evaluation phases, evaluation task selection manipulation, regression detection evasion through gradual degradation, evaluation data pipeline integrity violations, metric computation library vulnerabilities, Prometheus metrics manipulation for RL reward poisoning, distance metric mismatch attacks in heterogeneous vector search, or metric manipulation coordination through reasoning traces. Its contribution is mandating that evaluation exist and be continuous without specifying how to protect that evaluation infrastructure.

### RWA_8 - Vector Database and Knowledge Base Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of vector database and knowledge base infrastructure attack vectors. Technical threats such as vector database quantization for embedding space poisoning, metadata filtering inconsistency across vector database implementations creating cross-agent validation blind spots, ETL timestamp race conditions creating temporal knowledge gaps, SHA-256 deduplication bypass through hash-distinct variants of poisoned documents, REST API pagination cursor manipulation in shared ETL extractors, batch insertion race conditions in concurrent vector database loading, or SQL injection enabling multi-agent knowledge base poisoning through ETL connector vulnerabilities are data infrastructure security engineering concerns entirely outside the scope of a principles-based responsible AI governance framework.

### RWA_9 - Learning-Based Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of learning-based attack vectors exploiting multi-agent AI training dynamics. Threats such as learning/calibration scenario manipulation where agents game monitoring phases to reduce oversight, tool access policy drifting through iterative privilege relaxation, curriculum learning manipulation via poisoned intermediate tasks, MCTS rollout policy poisoning during training, cascading reward hacking through multi-agent workflow amplification where each agent's reward model misinterprets prior agents' exploitation as legitimate signal, or cascading policy conflicts in multi-agent workflows creating execution deadlocks from incompatible authorization decisions are AI training and learning security engineering concerns requiring technical controls entirely outside this governance framework's scope.

### RWA_10 - UI/UX Security Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of UI/UX security attack vectors in multi-agent AI systems. Threats such as command palette plugin recommendation poisoning through compromised relevance scoring, plugin UI integration lacking provenance transparency enabling uninformed approvals, RAG context window attacks exploiting UI pagination and preview limits that hide malicious instructions beyond visible content, workflow automation UI masking multi-step attack chains that appear atomic, plugin permission escalation through command palette interaction context, RAG source attribution spoofing in chat interface citations, or Matryoshka representation learning dimension truncation attacks exploiting dimensional inconsistency across agent tiers are user interface and user experience security concerns entirely outside the scope of this governance worksheet framework.

### RWA_11 - Reasoning and Reflection Attacks
- Strength score: 1

The DIU RAI Guidelines' "Traceable" principle requires AI reasoning to be understandable and auditable, which creates a conceptual mandate for reasoning integrity, but the framework provides no technical guidance on the 15 reasoning and reflection attack vectors in RWA_11. Threats such as multi-hop reasoning path manipulation through document ordering, hallucination amplification through mutual reinforcement in multi-agent critic-producer patterns, hallucination detection evasion through grounding spoofing via RAG poisoning, critic agent reasoning quality dependency enabling injections validated by deficient critics, reflection trace injection through fabricated self-critique that downstream agents accept as verified, type coercion reasoning through weak intra-step logic, or phase barrier bottleneck exploitation targeting single-agent critical path stalling require AI reasoning system security controls entirely beyond this governance framework's scope.

### RWA_12 - Distributed Systems Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of distributed systems attack vectors specific to multi-agent AI deployments. Threats such as event-driven replay attacks on asynchronous workflows exploiting large temporal accumulation windows, tool API rate limiting exhaustion through coordinated distributed invocation, cache coherence failure in distributed semantic memory causing selective instruction activation, batch processing race conditions in concurrent document ingestion enabling duplicate flooding, feature flag race conditions enabling multi-agent behavioral inconsistency attacks, multi-agent deadlock injection through crafted requests triggering circular dependency conditions, out-of-order agent execution race conditions exploiting non-deterministic scheduling, or cross-agent filter evasion through distributed attack pattern fragmentation are distributed systems security engineering concerns entirely outside the scope of this governance worksheet framework.

### RWA_13 - Approval Workflow Exploitation
- Strength score: 2

The DIU RAI Guidelines have meaningful relevance to approval workflow exploitation. The "Governable" principle explicitly requires that AI systems support human control and the ability to intervene in AI operations, which directly addresses the integrity of approval mechanisms. The development worksheet's "audit mechanisms" and "governance roles" lines of inquiry encourage organizations to assess whether human oversight processes function as intended and whether accountability structures are sound. These governance-layer questions are directly applicable to the approval fatigue exploitation, HITL workflow compromise, function calling consensus manipulation, and approval delegation-based privilege escalation catalogued in RWA_13. However, the framework provides no technical guidance on countermeasures for approval workflow gaming, approval request timing manipulation, batch poisoning through approval conditioning, consensus vote weight exploitation, or multi-agent deadlock from circular HITL approval dependencies. Its contribution is establishing organizational requirements for human control and audit that implicitly require resilient approval processes.

### RWA_14 - Infrastructure and Deployment Attacks
- Strength score: 1

The DIU RAI Guidelines have no coverage of AI infrastructure and deployment attack vectors. Threats such as embedding batch processing timing side-channels in shared GPU infrastructure, GPU embedding resource exhaustion through adversarial batch flooding, CI/CD pipeline artifact injection via compromised build credentials, load balancer health check gaming via specification mismatch, Kubernetes secret store poisoning enabling fleet-wide credential compromise, auto-scaling tool quota exhaustion through replica flood scaling events, or MIG reconfiguration attacks forcing coordinated node draining and cascading capacity exhaustion are infrastructure and deployment security engineering concerns entirely outside the scope of a principles-based responsible AI governance framework.

### RWA_15 - Tool Invocation and Selection Gaming
- Strength score: 1

The DIU RAI Guidelines have no coverage of tool invocation and selection gaming attacks. Threats such as tool invocation frequency gaming via observation manipulation causing over-invocation to satisfy task count metrics, LangChain memory poisoning with malicious invocation chains that propagate across agent boundaries, GroupChat tool availability negotiation enabling covert tool access through social engineering, streaming tool invocation enabling partial execution gaming where agents report completion based on incomplete execution, SLA-driven tool selection gaming through latency falsification causing agents to prefer malicious tools with falsified performance metrics, or KV cache sharing creating cross-agent tool-selection coupling vectors are tool security engineering concerns requiring technical controls entirely beyond this governance framework's scope.

### RWA_16 - Framework-Specific Vulnerabilities
- Strength score: 1

The DIU RAI Guidelines have no coverage of framework-specific vulnerability exploitation in multi-agent AI deployments. Threats such as LangChain tool loading vulnerabilities via untrusted tool definitions loaded from RAG-retrieved documents or external APIs, AutoGen tool negotiation creating implicit tool access chains through social engineering of conversational tool availability, and NeMo Guardrails configuration tampering in centralized deployments enabling fleet-wide safety bypass by relaxing jailbreak detection thresholds or disabling hallucination detection cascades are AI framework security engineering concerns entirely outside the scope of the DoD responsible AI ethical principles and worksheet-based assessment methodology.

