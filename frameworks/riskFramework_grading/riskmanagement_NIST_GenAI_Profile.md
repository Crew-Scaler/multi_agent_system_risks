# Risk Management Analysis: NIST AI 600-1 Generative AI Profile

## Framework Summary

NIST AI 600-1 ("Generative AI Profile") is a cross-sectoral companion profile to the AI Risk Management Framework (AI RMF 1.0), released in 2024 pursuant to EO 14110. It is a governance and risk management profile, not a technical security framework. Its suggested actions are organized around four primary GAI considerations: **Governance**, **Content Provenance**, **Pre-deployment Testing**, and **Incident Disclosure**.

The profile defines 12 risk categories unique to or exacerbated by Generative AI:
1. CBRN Information or Capabilities
2. Confabulation (hallucinations/fabrications)
3. Dangerous, Violent, or Hateful Content
4. Data Privacy (PII leakage, deanonymization)
5. Environmental Impacts
6. Harmful Bias and Homogenization
7. Human-AI Configuration (over-reliance, automation bias)
8. Information Integrity (disinformation, deepfakes)
9. Information Security (prompt injection, data poisoning, offensive cyber)
10. Intellectual Property
11. Obscene, Degrading, and/or Abusive Content
12. Value Chain and Component Integration (supply chain integrity)

Suggested actions are mapped to AI RMF subcategories (GOVERN, MAP, MEASURE, MANAGE) and address: policy and governance structures, pre-deployment red-teaming, content provenance tracking, incident disclosure processes, third-party vetting, safety and security measurement, and human oversight design. The profile explicitly recognizes prompt injection (direct and indirect) and data poisoning as Information Security risks (§2.9), and supply chain integrity as a Value Chain risk (§2.12).

**What the framework covers well:** Governance-level policies for GenAI risk; pre-deployment testing and red-teaming (general); data privacy and PII protection at the model level; supply chain and third-party component vetting; human-AI interaction and oversight design; confabulation/hallucination measurement; content provenance and watermarking; incident response and disclosure.

**What it does not cover:** Low-level infrastructure security (network segmentation, container hardening, Kubernetes RBAC, GPU hardware attacks); real-time operational monitoring or SIEM; cryptographic protocols; multi-agent trust mechanisms; framework-specific vulnerabilities (AutoGen, CrewAI, LangChain); streaming attacks; or detailed technical countermeasures for most agentic AI attack vectors.

---

## RATC - Agent–tool coupling as "policy‑level remote code execution"

### RATC_1 - UI/UX Abstraction and Visibility Gaps
- Strength score: 1

The NIST AI 600-1 GenAI Profile's Human-AI Configuration risk category acknowledges the broad concern of automation bias and the need for humans to retain meaningful oversight, and **GOVERN 3.2** calls for policies that define human-AI interaction roles. However, RATC_1's specific failure modes—approval UIs collapsing dangerous multi-tool chains into single buttons (RATC_1_1), progressive disclosure concealing cross-agent execution graphs (RATC_1_2), and multi-agent dashboards treating file reads and privileged system commands with equivalent visual weight (RATC_1_4)—are UI/UX implementation problems that NIST does not address at any technical level. NIST neither prescribes interface designs, risk-differentiated visualizations, nor cross-agent trace requirements. The profile's governance actions (policies, red-teaming, incident disclosure) operate at an organizational layer far removed from how an approval button is rendered or how tool-chain depth is surfaced to a reviewer. For context-driven invisible tool selection (RATC_1_5) and trace abstraction weaponization (RATC_1_6) in multi-agent settings, NIST offers no applicable control. This subsubsection sits almost entirely outside the profile's scope.

---

### RATC_2 - Approval Workflow and User Attention Vulnerabilities
- Strength score: 2

The NIST AI 600-1 GenAI Profile offers moderate, governance-level relevance to RATC_2. The Human-AI Configuration risk category directly names over-reliance and automation bias as concerns, and **GOVERN 3.2** requires organizations to establish policies for human oversight design—including defining when and how human approval gates should function. Pre-deployment testing guidance (including adversarial red-teaming) can in principle probe approval workflows for fatigue-exploitation patterns (RATC_2_1) and time-based default vulnerabilities (RATC_2_3). That said, NIST's contribution is policy-prescriptive rather than technical: it can mandate that approval workflows exist and be evaluated, but it provides no controls against keyboard shortcut hijacking via injected JavaScript (RATC_2_2), no requirements for approval queue rate-limiting, and no guidance on multi-agent trust propagation when one approval influences downstream agent behavior. The profile does not address the adversarial timing strategies or distributed queue flooding specific to multi-agent approval abuse. Organizations following NIST's AI RMF actions would be better positioned than those without governance at all, but the profile does not close the technical attack surface these vulnerabilities expose.

---

### RATC_3 - Tool Overload, Parameter Handling, and Selection Errors
- Strength score: 1

NIST AI 600-1 explicitly recognizes prompt injection (direct and indirect) within its Information Security risk category, which gives it limited but genuine relevance to RATC_3_2's tool argument injection vector—where one agent constructs arguments from untrusted data and passes them across trust boundaries for execution. Pre-deployment testing and red-teaming guidance could, in principle, exercise argument injection scenarios. However, tool overload privilege escalation and selection degradation (RATC_3_1) arise from architectural decisions about agent tool access breadth and LLM cognitive limits under high tool counts; NIST does not address tool set sizing, delegation chain design, or cascading selection failures. Similarly, inline suggestion RCE (RATC_3_3) and chat interface provenance obscuration (RATC_3_4) are UI and multi-agent transparency problems that fall outside the profile's scope entirely, as NIST covers neither interface design nor real-time execution monitoring. Even for argument injection, NIST's treatment remains at the level of risk acknowledgment and organizational policy rather than providing technical mitigations such as input sanitization at agent boundaries or cross-agent context isolation. The profile's overall coverage of this subsubsection is weak.

---

### RATC_4 - Confidence Manipulation and Tool Authorization
- Strength score: 2

NIST AI 600-1 has moderate relevance to RATC_4 through two overlapping risk categories. The Human-AI Configuration category addresses automation bias and over-reliance, which maps to the underlying vulnerability that confidence-based gating exploits (RATC_4_1)—if organizations follow GOVERN 3.2 policies requiring human oversight at critical decision points rather than delegating purely to automated confidence thresholds, the attack surface for confidence inflation is reduced. The Information Security category, which recognizes prompt injection as a named risk, is relevant because confidence threshold manipulation is initiated via adversarial prompt injection. For RATC_4_2, the Value Chain and Component Integration risk category acknowledges supply chain and multi-party trust risks, which partially maps to the transitive trust problem where a compromised coordination agent escalates privileges by exploiting other agents' assumption that prerequisites have been verified upstream. That said, NIST does not address confidence score architectures, majority-voting manipulation, or cross-agent authorization scope design at any technical level. The profile's contribution is governance framing—mandating that these risks be identified, measured, and assigned ownership—rather than providing controls that prevent confidence manipulation or enforce authorization re-validation at each agent boundary.
### RATC_6 - Tool Metadata Poisoning Across Registries and Discovery
- Strength score: 2

NIST AI 600-1's Value Chain and Component Integration category (§2.12) provides the most relevant coverage here, as RATC_6's attack surface is fundamentally a supply chain integrity problem: centralized tool registries, framework-specific tool selection mechanisms, and abstraction-layer blind spots are all third-party or shared-component risks. GOVERN 6.1 and 6.2 direct organizations to vet third-party components and establish accountability across the supply chain, which applies to tool registry providers and framework vendors whose metadata could be poisoned. However, the framework's applicability is indirect and incomplete. NIST AI 600-1 addresses whether a component was vetted before deployment, not whether a live registry has been compromised mid-operation; it has no guidance on vector database metadata integrity, runtime semantic search manipulation, or the quadratic scaling of attack surfaces in multi-agent shared-registry architectures. The support is therefore moderate: pre-deployment vetting and supply chain governance are relevant starting points, but the real-time, multi-agent, and framework-abstraction dimensions of RATC_6 fall outside the framework's scope.

---

### RATC_8 - Advanced Tool Invocation Patterns and Inference
- Strength score: 2

NIST AI 600-1's Information Security category (§2.9) explicitly names indirect prompt injection as a recognized risk, which maps directly to RATC_8_2's ReAct observation manipulation — the attack mechanism is injecting malicious content formatted as legitimate tool results so that downstream agents act on it. Pre-deployment testing guidance further supports detection of schema translation vulnerabilities (RATC_8_3) and replanning loop exploitation (RATC_8_1) if test suites are designed to probe these patterns. MEASURE 2.7 calls for security evaluation before deployment, which could in principle surface tool chain exploitation through controlled failure induction. That said, the framework's coverage is indirect for the multi-agent-specific dimensions: cross-agent state accumulation without global consistency checks, quadratic scaling of interaction pairs, and streaming progressive escalation injection are runtime, architectural concerns that governance and pre-deployment checklists do not fully address. The support is moderate because the prompt injection recognition and testing mandates are genuinely relevant, but the framework was not designed around agentic tool invocation pipelines.

---

### RATC_9 - Web and Multimodal Tool Exploitation
- Strength score: 2

RATC_9's core attack vector — adversaries injecting instructions into web content or poisoning multimodal retrieval documents so that agents execute them as legitimate guidance — is a form of indirect prompt injection operating through external data sources, which §2.9 (Information Security) explicitly acknowledges. Value Chain and Component Integration (§2.12) adds partial coverage for multimodal RAG architectures where retrieval agents select third-party documents that synthesis agents then act upon, framing poisoned multimodal content as a supply chain integrity issue. Pre-deployment testing could in principle include adversarial web content and poisoned image/document scenarios. However, the framework provides no operational guidance on cross-modal parameter injection without sanitization boundaries, vision model output laundering through sequential multi-agent processing, or hidden HTML form fields exploited at runtime. These are real-time, pipeline-structural vulnerabilities that governance documentation and third-party vetting processes do not reach. The score reflects genuine but limited relevance: the indirect prompt injection framing is correct, but the multimodal and multi-agent propagation mechanics are beyond the framework's intended scope.

---

### RATC_10 - Efficiency Optimization and Resource Constraints
- Strength score: 1

RATC_10 describes attacks that exploit infrastructure-level configuration parameters — token budget constraints forcing upstream agents to truncate safety-critical tool descriptions, iteration budget mismatches between agents, and temperature/model-selection diversity creating inconsistent policy enforcement. None of NIST AI 600-1's 12 risk categories or four primary considerations address context window allocation, inference iteration budgets, or per-agent temperature tuning. The framework is a governance and risk management profile focused on organizational accountability, content provenance, pre-deployment testing, and incident disclosure; it does not extend to runtime compute resource management or the emergent failure modes that arise when agents with heterogeneous resource constraints operate in a shared pipeline. MANAGE 4.1–4.3 covers incident response after a failure is detected, but that is downstream of the exploitation mechanisms described in RATC_10. The support is weak: resource constraint attacks represent an infrastructure and configuration concern that is effectively out of scope for a governance-oriented GenAI risk profile.
### RATC_11 - Tool Execution Infrastructure and Orchestration
- Strength score: 1

RATC_11 covers API gateway routing manipulation (RATC_11_1) and Kubernetes/container orchestration attacks (RATC_11_2)—both of which are pure infrastructure and platform-security concerns. The NIST AI 600-1 Generative AI Profile is a governance and risk management framework scoped to AI model behavior, training data integrity, and deployment policy; it contains no guidance on securing service registries, API gateway routing policies, container security contexts, Persistent Volume Claims, sidecar proxies, or init container configurations. None of its 12 risk categories address network-layer or container-layer exploitation. An organization following the profile's GOVERN and MANAGE functions may establish general accountability structures and supply chain review practices (Value Chain/Component Integration, §2.12), but these do not translate into technical controls capable of preventing or detecting the fleet-wide compromise scenarios described here. NIST AI 600-1 is therefore effectively out of scope for RATC_11.

### RATC_12 - Distributed and Hardware-Level Attacks
- Strength score: 1

RATC_12 describes attacks at the GPU and hardware communication layer: interception of unencrypted inter-GPU tensor parallelism traffic (RATC_12_1) and KV cache or quantization calibration poisoning on shared inference hardware (RATC_12_2). These threats require cryptographic transport controls for GPU interconnects, hardware isolation between tenants, and low-level inference infrastructure hardening—none of which fall within NIST AI 600-1's scope. The profile's Information Security risk category (§2.9) addresses prompt injection and training data poisoning at the model input/output level, not at the silicon or memory-bus level. There is no applicable measure in GOVERN, MAP, MEASURE, or MANAGE that would influence whether NVLink or PCIe traffic is encrypted, or whether KV cache partitioning is enforced between users. NIST AI 600-1 offers no meaningful coverage of RATC_12.

### RATC_14 - Reasoning and Planning Vulnerabilities
- Strength score: 2

RATC_14 addresses failures in chain-of-thought reasoning quality (RATC_14_1), self-consistency and majority-voting manipulation (RATC_14_2), and hierarchical planning context fragmentation (RATC_14_3). The NIST AI 600-1 Confabulation risk category (§2.2) directly targets the generation of erroneous, ungrounded, or logically unsupported outputs—which maps onto the intra-step correctness failures and unsupported reasoning leaps described in RATC_14_1. **MEASURE 2.9** calls for evaluation of AI model explanation and validation methods, providing a pre-deployment testing hook for assessing whether reasoning chains produce sound and auditable justifications for tool selection. Pre-deployment testing considerations in the profile also support structured red-teaming of reasoning pipelines, including self-consistency mechanisms. However, the profile does not prescribe runtime monitoring of live reasoning traces, offers no technical controls for detecting sampling-path divergence exploits, and does not address the tool-visibility fragmentation inherent in hierarchical task network planning. The coverage is moderate: governance and testing practices can surface reasoning quality problems before deployment, but cannot prevent adversarial manipulation of reasoning at inference time.

### RATC_15 - Episodic Memory and Learning
- Strength score: 2

RATC_15 describes poisoning of episodic memory stores, where attackers inject fabricated "successful" tool sequences that agents later retrieve and replicate, with multi-agent shared memory enabling fleet-wide propagation (RATC_15_1). This maps onto two NIST AI 600-1 risk categories: **Information Security (§2.9)**, which explicitly covers data poisoning as a threat vector affecting model and system behavior, and **Value Chain and Component Integration (§2.12)**, which addresses the integrity of external data sources and third-party components incorporated into AI systems. GOVERN 6.1 and 6.2 establish organizational accountability for data provenance and supply chain risk, which can inform policies for validating episodic memory entries before they are stored or retrieved. The profile's pre-deployment testing considerations support evaluation of memory retrieval pipelines for poisoning susceptibility. The limitation is that NIST AI 600-1 does not define runtime access controls, isolation boundaries between agents' memory namespaces, or cryptographic integrity checks for stored episodes—so the governance intent must be translated into technical controls by the implementing organization.

### RATC_16 - Semantic Memory and RAG
- Strength score: 2

RATC_16 covers RAG pipeline and knowledge base poisoning, where adversaries embed tool-invocation instructions in retrieved documents, manipulate knowledge graph relationships to surface unsafe tool combinations, inject instructions into query rewriting steps, or exploit knowledge base staleness to trigger invocations using obsolete tool formats (RATC_16_1). **Information Security (§2.9)** is the strongest applicable NIST AI 600-1 category, as it explicitly names indirect prompt injection—the mechanism by which tool-invocation instructions hidden in retrieved content reach the agent—and data poisoning of sources the model depends on. **Value Chain and Component Integration (§2.12)** applies to the knowledge base itself as an external component, and **GOVERN 6.1/6.2** establishes governance obligations for monitoring and reviewing third-party data pipelines. Pre-deployment testing guidance supports evaluation of retrieval pipelines for injection susceptibility. The profile does not, however, define technical controls for knowledge graph integrity, query rewrite sanitization, or automated staleness detection—leaving those gaps to be filled by implementation-level security engineering outside the profile's scope.
# NIST AI 600-1 GenAI Profile: Coverage Analysis for RATC_17, 18, 19, 21, 22, 27, 29, 30

---

### RATC_17 - Utility Functions and Decision Logic
- Strength score: 1

RATC_17 describes attacks on expected utility calculations: injecting false outcome assumptions, corrupting intermediate utility estimates in sequential decisions, and poisoning multi-objective trade-off weights to steer tool selection toward malicious outcomes. NIST AI 600-1's Confabulation category (§2.2) is the closest mapping, as erroneous internal reasoning can produce incorrect utility estimates that resemble confabulated outputs, and the framework's governance controls encourage pre-deployment testing of decision logic. However, the framework does not address the mechanics of utility function internals, expected-value computation pipelines, or adversarial manipulation of reward-weighting schemes. These are algorithmic decision-architecture concerns that fall outside NIST AI 600-1's scope, which focuses on observable AI outputs and organizational risk governance rather than the internal logic of planning or tool-selection engines. Support is therefore weak.

---

### RATC_18 - Rule-Based and Knowledge-Engineered Systems
- Strength score: 1

RATC_18 covers authorization bypass attacks against rule-based systems: injecting high-specificity rules that override safety rules, exploiting forward-chaining to trigger unauthorized tool authorizations, manipulating certainty factors to bypass risk assessment, and reordering lexicographic objectives to alter tool selection. NIST AI 600-1's Information Security category (§2.9) acknowledges that AI systems can be manipulated through adversarial inputs, and governance controls encourage policies around access and authorization integrity. However, the framework does not model rule engines, specificity hierarchies, certainty-factor arithmetic, or forward-chaining inference as attack surfaces. The threat is specific to symbolic AI systems and rule-based authorization architectures, which lie well outside the generative AI and neural-model scope of NIST AI 600-1. The mapping is peripheral at best.

---

### RATC_19 - Learning and Reinforcement Learning
- Strength score: 2

RATC_19 describes poisoning attacks on learned tool-selection policies: reward signal poisoning to train agents toward malicious tool selection, poisoned transitions in shared replay buffers affecting all agents' Q-values, curriculum and imitation learning trajectory corruption, actor-critic cross-agent critic poisoning, and PPO trust-region manipulation to iteratively shift agent policies toward dangerous attractors. NIST AI 600-1 directly recognizes data poisoning as a risk under Information Security (§2.9) and identifies training data integrity as a concern under Value Chain and Component Integration (§2.12), which addresses risks introduced through third-party training pipelines and datasets. Pre-deployment testing considerations further encourage evaluation of model behavior for unexpected policy shifts. The framework does not address RL-specific mechanisms such as replay buffers, Q-value corruption, or policy gradient manipulation, but it does provide governance-level coverage of the poisoning threat class that motivates these attacks, warranting moderate support.

---

### RATC_21 - Parallel Retrieval Race Conditions in Multi-Agent Query Decomposition Systems
- Strength score: 1

RATC_21 describes infrastructure-level race conditions arising from concurrent multi-agent retrieval: inconsistent database states during parallel sub-queries, deduplication races on shared indexes, timeout-triggered cancellation cascades overwhelming connection pools, multi-stage TTL inconsistencies mixing results from different knowledge states, and fleet-wide poisoning via container registry compromise. While the container supply chain element has a superficial connection to NIST AI 600-1's Value Chain and Component Integration category (§2.12), the dominant failure modes here are database consistency, distributed systems concurrency, and connection pool exhaustion — infrastructure concerns entirely outside the framework's scope. NIST AI 600-1 does not address concurrent query coordination, TTL-based cache coherence, or network-level retrieval infrastructure. The framework provides no meaningful guidance for mitigating these race conditions.

---

### RATC_22 - Multi-Stage Pipeline Result Caching Race Enabling Multi-Agent Consistency Failures
- Strength score: 1

RATC_22 describes race conditions arising from per-stage TTL expiration in multi-stage caching pipelines, causing agents to serve temporally inconsistent results; event-based invalidation propagation delays; write races and hash collision attacks on shared caches; warming inconsistency; and supply chain attacks via container registry credential compromise, typosquatting, digest collision bypass, and GitOps automation enabling fleet-wide backdoor deployment. The container registry and supply chain elements nominally overlap with NIST AI 600-1's Value Chain and Component Integration category (§2.12), which flags third-party component risks. However, the bulk of the threat — cache coherence races, TTL inconsistency, hash collision cache poisoning, and tensor parallelism memory isolation failures — is a distributed infrastructure and systems security problem that the governance-oriented NIST framework does not address. The supply chain overlap is real but minor relative to the full threat surface.

---

### RATC_27 - MIG GPU Co-Location Creating Shared Physical GPU Failure Propagation
- Strength score: 1

RATC_27 describes how Multi-Instance GPU (MIG) partitioning, despite providing logical isolation between tenant instances, fails to protect against shared physical hardware dependencies: power regulators, thermal management, PCIe interfaces, and GPU firmware are shared across all MIG instances. A power delivery failure, thermal event, PCIe error, firmware hang, or firmware vulnerability enabling cross-partition memory access can cause fleet-wide outage or isolation breach. A single driver bug triggered by any tenant's CUDA operation can affect all co-located instances simultaneously. This is a hardware architecture and GPU driver security problem with no coverage in NIST AI 600-1. The framework addresses AI model governance, content risks, and organizational risk management; it does not consider GPU hardware isolation, firmware security, multi-tenant compute infrastructure, or physical hardware failure propagation. There is no applicable mapping.

---

### RATC_29 - Shared Workflow State Version Conflict Amplification Through Concurrent Write Flooding
- Strength score: 1

RATC_29 describes how optimistic locking in shared workflow state systems generates systematic version conflicts under high concurrency: many agents read at the same version, work independently, and race to write, with only the first writer succeeding and the rest retrying. This creates retry storms that intensify contention rather than resolving it, with conflict probability scaling quadratically with concurrency. Targeted collision bursts can exhaust retry budgets and cause workflows to fail without completing. This is a distributed systems concurrency and database coordination problem — specifically the interaction between optimistic locking semantics, retry logic, and adversarial workload injection. NIST AI 600-1 does not address workflow orchestration, optimistic locking, version conflict resolution, retry budget exhaustion, or any form of concurrency control in multi-agent infrastructure. There is no applicable mapping.

---

### RATC_30 - Guardrail Bypass Through Infrastructure Failure Injection
- Strength score: 2

RATC_30 describes attacks that exploit timeout and availability constraints in guardrail validation infrastructure: adversary-induced processing delays that exceed validation timeouts cause responses to bypass safety checks; network flooding, resource exhaustion, or denial-of-service against centralized validation endpoints cause fleet-wide guardrail bypass when agents fall back to delivering unvalidated responses. NIST AI 600-1's Information Security category (§2.9) addresses adversarial manipulation of AI systems, and the framework's governance and pre-deployment testing considerations (including MEASURE 2.6 for safety risk evaluation) establish organizational expectations for maintaining safety policy enforcement. Critically, the framework's emphasis on guardrail governance and incident disclosure (§2.4) provides relevant high-level coverage: if safety validation can be bypassed through infrastructure failure, the framework's risk management requirements are directly implicated. However, NIST AI 600-1 does not address the specific infrastructure attack vectors — timeout exploitation, denial-of-service against validation endpoints, or fail-open fallback logic — that constitute the actual threat mechanism, limiting support to the governance and safety-policy governance layer.


## RDL - New data‑leakage channels via large contexts, logs, and probabilistic recall

### RDL_1 - UI/UX Patterns
- Strength score: 1

The UI/UX risks in RDL_1 are implementation-layer concerns about how sensitive data appears in chat histories, debug views, session stores, dashboards, audit trails, command palettes, accessibility labels, and reference links. NIST AI 600-1 offers only peripheral relevance. The Data Privacy risk category (**§2.4**) acknowledges PII leakage and deanonymization as named risks, and the Human-AI Configuration category (**§2.7**) recognizes the need for meaningful human oversight in AI interactions. At the governance level, **GOVERN 3.2** requires policies defining human-AI interaction roles. However, these policy instruments do not reach the specific failure modes across RDL_1: NIST prescribes no interface design standards, no requirements for redacting credentials from expandable debug views, no guidance on compartmentalizing multi-agent dashboard outputs, no controls over what approval workflow audit trails may log or who may query them, and no accessibility-label data exposure requirements. The profile does not address browser storage, CDN caching of session data, or session persistence across security classification boundaries. The framework's incident disclosure provisions (**MANAGE 4.1–4.3**) are downstream of these exposures rather than preventive. NIST AI 600-1 was designed as a governance and risk management profile for AI system behavior, not as a UI/UX security specification, and the full breadth of RDL_1 sits effectively outside its scope.

---

### RDL_2 - Data Persistence and Caching
- Strength score: 1

RDL_2 concerns data remanence: session serialization transforming conversational data into structured queryable records (RDL_2_1), cached responses persisting beyond session lifetime in browser caches and CDN edge nodes (RDL_2_2), and undo buffers preserving data that users and agents believe has been deleted (RDL_2_3). The Data Privacy risk category (**§2.4**) in NIST AI 600-1 identifies PII leakage as a named risk, and organizational governance policies required by **GOVERN** functions can in principle mandate data minimization and retention limits. However, the framework's data privacy focus is on model-level behavior — training data, inference outputs, and deanonymization risks from model outputs — not on application-layer caching infrastructure, browser storage APIs, CDN edge server retention policies, or undo buffer lifecycle management. NIST AI 600-1 contains no suggested actions that would prevent structured serialization of conversation history into queryable exfiltration-ready formats, no controls over CDN-layer data retention, and no guidance on ensuring that undo functionality respects deletion semantics. These are application architecture and infrastructure concerns that fall outside what a governance-oriented GenAI risk profile addresses. The only modest link is that pre-deployment testing considerations could theoretically probe data retention behavior, but the framework does not specify this. Coverage is weak.

---

### RDL_3 - Streaming and Token-Level Leakage
- Strength score: 1

The streaming threats in RDL_3 — fine-grained token-level leakage before redaction can apply (RDL_3_1), tokenization revealing context window structure enabling inference attacks (RDL_3_3), output length correlation enabling session reconstruction from metadata (RDL_3_4), intermediate state leakage across iterative multi-agent cycles (RDL_3_5, RDL_3_6), and cache side-channel exfiltration of streaming responses (RDL_3_7) — are technical attack vectors specific to streaming response architectures and real-time inference pipelines. NIST AI 600-1 does not address streaming as a technology category. The Information Security category (**§2.9**) names prompt injection and data poisoning but not streaming-layer interception or timing side channels. The Data Privacy category (**§2.4**) covers PII leakage at the model output level but not the mechanics of pre-redaction exposure in token buffers or network streams. None of the four primary GAI considerations — Governance, Content Provenance, Pre-deployment Testing, Incident Disclosure — provide applicable controls for streaming token leakage, tokenization inference attacks, or cache side channels. **MEASURE 2.7** calls for security evaluation but is framed around model behavior rather than streaming infrastructure telemetry. The profile is a governance document not designed to reach real-time, infrastructure-layer attack surfaces; RDL_3 is largely out of scope.

---

### RDL_4 - Search and Recall
- Strength score: 2

RDL_4_1 describes probabilistic recall through conversation search, where querying for one topic surfaces semantically related sensitive information from adjacent contexts the user did not intend to retrieve. This is functionally a data privacy risk arising from semantic search over accumulated personal and organizational data. The Data Privacy risk category (**§2.4**) in NIST AI 600-1 explicitly identifies PII leakage and deanonymization as named risks, and the framework's governance actions under this category include organizational policies for limiting and reviewing data access — which can in principle cover who may query conversation history and how search interfaces are bounded. **GOVERN** function policies on data handling and **MANAGE 4.1–4.3** on incident response provide organizational accountability framing. Pre-deployment testing guidance could support evaluation of whether retrieval surfaces sensitive data beyond user intent. However, NIST AI 600-1 does not define technical controls for semantic search scoping, result filtering, or access boundary enforcement at query time. The probabilistic and cross-context nature of the recall channel — where leakage emerges from the semantics of unrelated queries rather than direct access — is a nuanced retrieval architecture risk that the framework acknowledges at the policy level but cannot resolve with its suggested actions alone. The moderate score reflects genuine §2.4 relevance without technical countermeasure coverage.

---

### RDL_5 - Attribution and Observability
- Strength score: 1

RDL_5 describes how observability and logging infrastructure becomes an intelligence source: attribution metadata revealing organizational structure and security priorities (RDL_5_1), framework-specific debug logging aggregated across multi-agent deployments disclosing system architecture (RDL_5_2), error classification patterns revealing system fragility (RDL_5_3), and latency measurements exposing computational load and tool availability (RDL_5_4). NIST AI 600-1's Information Security category (**§2.9**) acknowledges that AI systems can be exploited through adversarial manipulation, and Value Chain and Component Integration (**§2.12**) covers risks from multi-party deployments. However, the framework contains no guidance on log retention scoping, telemetry access controls, framework-specific logging configuration, or side-channel analysis of observability data. These are operational security and monitoring architecture concerns. The profile's suggested actions address organizational accountability for AI risk, not the security of the logging and telemetry layer that surrounds AI systems. **MEASURE 2.7** covers security evaluation of the AI model itself, not of the monitoring infrastructure that may leak organizational intelligence. Observability security is effectively out of scope for a governance-oriented GenAI risk profile, and RDL_5 has no meaningful mapping to NIST AI 600-1's categories or suggested actions.

---

### RDL_7 - Tool Invocation and Function Calling
- Strength score: 2

RDL_7's threats span tool invocation logging as a side channel (RDL_7_1), function calling JSON in context windows exposing parameters (RDL_7_2), error message implementation detail leakage (RDL_7_3), tool selection reasoning as cognitive state disclosure (RDL_7_4), plugin registry enumeration exposing organizational information (RDL_7_5), audit trail poisoning through compromised agents (RDL_7_6), success rate differential leakage enabling target identification (RDL_7_7), parameter value leakage through action logs (RDL_7_8), accuracy metrics revealing operational patterns (RDL_7_9), and log aggregation creating multi-agent exfiltration channels (RDL_7_10). NIST AI 600-1 provides moderate coverage through two applicable categories. The Information Security category (**§2.9**) explicitly names indirect prompt injection — the mechanism underlying audit trail poisoning (RDL_7_6) where compromised agents inject false log entries — and data poisoning, both of which are relevant. The Data Privacy category (**§2.4**) covers PII leakage and deanonymization, which maps onto the aggregation of sensitive parameters (API keys, account numbers, PII) in action logs (RDL_7_8, RDL_7_10). Value Chain and Component Integration (**§2.12**) addresses third-party plugin and component integrity, partially applicable to plugin registry enumeration (RDL_7_5). **GOVERN 6.1/6.2** establishes accountability for third-party tool components. Pre-deployment testing guidance could in principle include evaluation of tool logging behavior and context window exposure. The limitation is that NIST AI 600-1 does not prescribe technical controls for log access scoping, context window content filtering, error message sanitization, or multi-agent telemetry isolation — the specific mechanisms that would close these leakage channels. The score reflects genuine but indirect policy-level coverage across several RDL_7 items without reaching the technical attack mechanics.

---
# NIST AI 600-1 GenAI Profile: Coverage Analysis for RDL_9 through RDL_14

---

### RDL_9 - Multi-Agent Memory and State
- Strength score: 1

RDL_9's threats are fundamentally infrastructure and architectural confidentiality problems: reflection memory stores leaking across agent trust boundaries (RDL_9_1), ReAct reasoning traces persisted in debugging or compliance logs exposing system topology (RDL_9_2), distributed trace correlation IDs enabling cross-tenant timing analysis (RDL_9_3), shared blackboard memory enabling access-pattern reconstruction (RDL_9_4), and monitoring dashboards leaking workflow correlation through simultaneous agent activity display (RDL_9_5). NIST AI 600-1's Data Privacy risk category (§2.4) acknowledges PII leakage and deanonymization as model-level concerns, and GOVERN 3.2 establishes organizational accountability for data handling policies. In principle, governance policies can mandate isolation between agent memory namespaces or restrict debugging log retention. However, the framework does not address distributed trace infrastructure, correlation ID design, timing side-channels, blackboard architecture, or multi-agent monitoring dashboard design—all of which are implementation-level decisions about how memory and observability infrastructure is built, not about GenAI model outputs or organizational policy. NIST AI 600-1's Information Security category (§2.9) is scoped to prompt injection and data poisoning, not information leakage through architectural side-channels. The framework provides no meaningful technical countermeasures for any of RDL_9's specific mechanisms; its governance mandate is too abstract to address covert channels arising from shared infrastructure.

---

### RDL_10 - Reasoning and CoT Traces
- Strength score: 2

RDL_10 describes a cluster of privacy and confidentiality failures arising from how chain-of-thought, tree-of-thought, and self-consistency reasoning traces are stored, shared, and retrieved in multi-agent systems. Traces contain sensitive context data analyzed during reasoning (RDL_10_1), step-by-step logs that enable reconstruction of sensitive inputs (RDL_10_2), embedded secrets documented in reasoning text (RDL_10_3), cross-agent context leakage when one agent's trace is fed to another (RDL_10_4), lookahead branch observation revealing future plans (RDL_10_5), preserved high-quality reasoning paths retaining sensitive content (RDL_10_6), consolidated memory merging sensitive traces (RDL_10_7), probabilistic recall of intermediate steps from preserved chains (RDL_10_8), quality score metadata as a side-channel (RDL_10_9), and failure case debug logs leaking through difficulty classification (RDL_10_10). NIST AI 600-1's Data Privacy risk category (§2.4) most directly applies: the profile explicitly identifies PII leakage and deanonymization as risks, and its governance actions call for data minimization policies and evaluation of what sensitive data models process and retain. The concern of training data or inference context containing sensitive information that persists in model outputs and logs is squarely within §2.4's scope. MEASURE 2.7 (security evaluation) supports pre-deployment testing that could probe whether reasoning traces expose sensitive content under normal and adversarial conditions. The Information Security category (§2.9) also applies at the margins where embedded secrets (RDL_10_3) or cross-agent trace propagation creates injection-adjacent risks. That said, the profile's coverage is governance and pre-deployment in orientation; it does not address runtime isolation of reasoning traces, cross-agent context filtering, quality score metadata access control, or probabilistic recall suppression. The debugging log leakage vectors (RDL_10_2, RDL_10_10) and timing-adjacent side-channels (RDL_10_9) are infrastructure concerns outside the framework's scope. The score is moderate because §2.4 provides a genuine conceptual hook for the privacy class of threats, but the agentic, multi-stage trace mechanics require technical controls the profile does not specify.

---

### RDL_11 - Multimodal and Input Processing
- Strength score: 1

RDL_11 covers confidentiality risks arising from how multimodal AI systems process and store non-text inputs: expanded context windows in multimodal RAG increasing the leakage surface across more data modalities (RDL_11_1), intermediate vision model representations leaking information about processed images (RDL_11_2), audio embeddings revealing speaker identity or emotional tone (RDL_11_3), chart linearization producing persistent structured data containing sensitive numerical values (RDL_11_4), and embedding inversion attacks reconstructing original images from stored embeddings in shared vector databases (RDL_11_5). NIST AI 600-1's Data Privacy category (§2.4) acknowledges that GenAI systems can leak PII through their outputs and stored data, and the profile encourages data minimization and privacy-preserving practices as governance measures. This provides a conceptual foundation for recognizing that multimodal inputs introduce new privacy exposure surfaces. However, the framework was developed with text-based generative AI primarily in mind and does not address vision model intermediate representations, audio embedding architectures, chart-to-table linearization pipelines, or embedding inversion as attack categories. The Value Chain category (§2.12) is marginally relevant to shared vector databases as external components, but does not address tenant isolation within those databases. NIST AI 600-1 offers no guidance on differential privacy for embeddings, inversion attack resistance, or modality-specific data retention policies. The governance framing of §2.4 is the sole relevant hook, and it is too general to meaningfully address the multimodal-specific threat mechanics described in RDL_11.

---

### RDL_12 - Error Handling and Graceful Degradation
- Strength score: 1

RDL_12 describes confidentiality and reconnaissance risks arising from operational error handling infrastructure: error logs capturing full context including credentials and sensitive tool outputs (RDL_12_1), retry logs exposing intermediate states during recovery (RDL_12_2), fallback routing to secondary providers creating separate logging streams with weaker access controls (RDL_12_3), graceful degradation logs disclosing system architecture and capability states (RDL_12_4), and circuit breaker state transitions revealing infrastructure health patterns (RDL_12_5). NIST AI 600-1's Data Privacy category (§2.4) provides the closest conceptual overlap, as credential and PII leakage through error logs is a privacy violation consistent with the framework's concern about sensitive data appearing in model-generated or system outputs. The Information Security category (§2.9) acknowledges that AI system operational security is relevant. In principle, organizational governance under GOVERN functions could mandate log access controls and data retention policies. However, the framework does not address error handling architecture, retry logic design, fallback provider selection, circuit breaker patterns, or operational log access controls—all of which are software engineering and infrastructure security decisions. NIST AI 600-1 is a GenAI model and organizational risk governance profile; it has no subcategories or suggested actions that reach error handling middleware, multi-provider failover logging, or infrastructure reconnaissance through degradation state disclosure. The support is weak across all five sub-threats.

---

### RDL_13 - Evaluation and Testing Leakage
- Strength score: 2

RDL_13 addresses confidentiality and integrity risks in evaluation infrastructure: detailed evaluation metric logs revealing sensitive system information (RDL_13_1), baseline comparison agents loading historical sensitive evaluation results into context (RDL_13_2), evaluation dashboards exposing test case samples (RDL_13_3), evaluation artifact storage compromise leaking evaluation secrets (RDL_13_4), comprehensive evaluation error logs enabling evasion of evaluation logic (RDL_13_5), metric output format fingerprinting for architecture reconnaissance (RDL_13_6), temporal analysis of metric changes inferring model updates (RDL_13_7), cross-validation fold analysis exposing dataset structure (RDL_13_8), and approval log exposure of benchmark performance and failure modes (RDL_13_9). NIST AI 600-1 places significant emphasis on pre-deployment testing as one of its four primary GAI considerations, and MEASURE 2.6 and MEASURE 2.7 explicitly call for safety and security evaluation before deployment. This creates direct relevance to RDL_13: the profile's testing governance mandate implies that evaluation pipelines themselves must be secured, that test datasets and evaluation artifacts require appropriate access controls, and that benchmark results carry confidentiality implications when they reveal system failure modes. MANAGE 4.1–4.3's incident response framework applies when evaluation artifacts are compromised. MEASURE 2.9's model explanation and validation guidance touches on the integrity of evaluation reasoning. However, NIST AI 600-1 does not prescribe specific controls for evaluation log access, dashboard data exposure, temporal metric analysis resistance, or cross-validation dataset protection. The profile acknowledges that evaluation matters and must be done, but does not govern how evaluation infrastructure is secured against leakage. The score is moderate: the framework's genuine investment in pre-deployment testing creates an organizational accountability hook, but the specific technical threats in RDL_13 require engineering controls the profile does not provide.

---

### RDL_14 - Feedback and Testing
- Strength score: 2

RDL_14 describes two attack vectors arising from operational feedback mechanisms: user feedback logs describing agent failure modes being used to profile agent vulnerabilities for targeted compromise (RDL_14_1), and A/B test results showing differential agent performance being leaked to identify more vulnerable variants for preferential exploitation (RDL_14_2). NIST AI 600-1's governance framework is most relevant here through its MANAGE 4.1–4.3 functions covering incident response and disclosure, which establish organizational obligations around handling information about AI system failures and limitations—precisely the class of information that user feedback and A/B test results represent. The pre-deployment testing consideration and MEASURE 2.7 (security evaluation) support the principle that testing results, including performance differentials between model variants, should be treated as sensitive operational data requiring access controls. Data Privacy (§2.4) applies if user feedback contains PII or if failure descriptions reveal personal user context. The Information Security category (§2.9) is relevant to the extent that feedback-derived vulnerability profiles could inform prompt injection or adversarial input strategies. That said, NIST AI 600-1 does not specify access controls for feedback repositories, policies for compartmentalizing A/B test results, or procedures for preventing operational performance data from becoming attacker intelligence. These are operational security and data governance implementation decisions that the profile's governance mandate implies but does not define. The score is moderate because the framework's accountability structures and incident management focus genuinely apply to sensitive feedback and testing data, even if the specific attack mechanics are not addressed.

---
### RDL_15 - Evaluation Metric Exploitation
- Strength score: 2

RDL_15 describes a class of adversarial attacks that manipulate evaluation metrics — exact match (RDL_15_1), joint metrics on supporting facts (RDL_15_2), Pass@K probabilistic detection (RDL_15_3), and milestone threshold scoring (RDL_15_4) — so that agents appear to perform correctly under evaluation while producing outputs that serve adversarial goals. NIST AI 600-1's Confabulation risk category (§2.2) is partially relevant: it acknowledges that AI systems can produce outputs that appear valid but are factually or logically unsupported, and the framework's pre-deployment testing guidance (including MEASURE 2.3 on performance benchmarking and MEASURE 2.7-007 on red-teaming) creates organizational expectations for evaluating model behavior before deployment. MEASURE 2.3-001 and 2.3-002 specifically caution against relying on narrow or non-systematic assessments of capability, which speaks to the brittleness of single-metric evaluation regimes that these attacks exploit. The Information Security category (§2.9) is marginally applicable to the injection mechanisms underlying RDL_15_1 and RDL_15_2, since adversaries deliver their manipulations through prompt injection or poisoned few-shot content. However, the framework does not address the mechanics of metric gaming: it provides no guidance on multi-metric robustness, non-determinism in evaluation (Pass@K), or benchmark construction that resists threshold manipulation. NIST AI 600-1's contribution is the governance imperative to evaluate AI systems rigorously and adversarially before deployment, which creates some pressure toward more robust evaluation design — but the specific technical failure modes of metric exploitation fall outside what the profile's suggested actions can resolve.

---

### RDL_16 - Parameter and Configuration Leakage
- Strength score: 1

RDL_16 describes inference attacks that reconstruct sensitive deployment configuration from observable side channels: audit log volume correlated with context window size (RDL_16_1), token consumption patterns revealing query sensitivity routing (RDL_16_2), and latency profiles fingerprinting which models and optimization settings are deployed (RDL_16_3). These are operational security and infrastructure intelligence threats. NIST AI 600-1's Data Privacy category (§2.4) covers PII leakage and deanonymization, but its framing is model output-level — it does not address inference attacks on system configuration conducted through telemetry side channels. The Information Security category (§2.9) names AI inference attacks in MEASURE 2.7-001 ("AI inference, bypass, extraction, and other baseline security concerns"), which provides the closest nominal mapping. However, the profile's treatment of inference attacks focuses on model-level extraction (model theft, membership inference on training data), not on configuration fingerprinting through latency or token consumption patterns. NIST AI 600-1 does not address logging retention scope, differential routing observability, or timing side channels in multi-agent deployments. These are operational monitoring and infrastructure security concerns that sit outside the governance-oriented profile's scope. The framework's suggested actions would not prevent an adversary from inferring deployment architecture through the side channels described in RDL_16.

---

### RDL_17 - Prompt Injection and Few-Shot Attacks
- Strength score: 2

RDL_17 covers three variants of few-shot demonstration poisoning: format injection enabling parse confusion in downstream agents (RDL_17_1), confidence bias injection through biased example patterns (RDL_17_2), and sensitive data recall triggered by crafted prompts against contaminated demonstration stores (RDL_17_3). NIST AI 600-1 provides moderate coverage through two directly applicable categories. The Information Security category (§2.9) explicitly names both direct and indirect prompt injection as recognized risks, and MEASURE 2.7-007 mandates AI red-teaming to assess resilience against prompt injection and data poisoning — both of which are relevant to demonstration contamination. For RDL_17_3, the Data Privacy category (§2.4) and MEASURE 2.10-001 (red-teaming to assess risks of outputting training data samples, reverse engineering, and membership inference) provide genuine coverage: probabilistic recall of sensitive data embedded in few-shot demonstrations is functionally a training-data memorization and membership inference risk. GOVERN 6.1-004 on security requirements in third-party data contracts is also relevant where demonstration stores are externally sourced. The limitation is that NIST AI 600-1 does not distinguish few-shot demonstration pipelines as a specific attack surface or prescribe controls for demonstration store integrity, format validation at agent boundaries, or confidence calibration auditing. The profile's contribution is naming the threat class and mandating adversarial testing, while the implementation-level defenses remain unspecified.

---

### RDL_18 - Distributed Tracing
- Strength score: 1

RDL_18 describes how distributed tracing infrastructure becomes an intelligence leakage channel: correlation IDs linking trace spans across multi-agent workflows expose complete operation chains and intermediate agent outputs (RDL_18_1), and response schema validation logs reveal individual agent configurations through patterns of schema rejection (RDL_18_2). Both threats are operational security concerns about the logging and observability infrastructure surrounding AI systems, not about AI model behavior itself. NIST AI 600-1 does not address distributed tracing systems, span data retention, trace correlation scope, or schema validation logging. The Information Security category (§2.9) acknowledges that AI systems can be exploited through adversarial manipulation and that deployment infrastructure security is relevant, but the profile's suggested actions focus on model-level security evaluation (MEASURE 2.7) and organizational governance, not on the design of observability pipelines. The Value Chain and Component Integration category (§2.12) could nominally apply if distributed tracing is a third-party component, but neither GOVERN 6.1 nor 6.2 provides guidance specific to trace data access controls or span data minimization. NIST AI 600-1's four primary considerations — Governance, Content Provenance, Pre-deployment Testing, Incident Disclosure — contain nothing applicable to the leakage channels described here. The framework is effectively out of scope for RDL_18.

---

### RDL_19 - Execution and Orchestration
- Strength score: 1

RDL_19 covers two related orchestration security failures: error messages in multi-agent systems disclosing one agent's execution paths to other agents or external parties through shared error handling (RDL_19_1), and provenance information lost during multi-agent parameter handoff creating attribution gaps that allow poisoned parameters to propagate downstream without detection (RDL_19_2). The Information Security category (§2.9) is the closest NIST AI 600-1 mapping, as it names indirect prompt injection and data poisoning — RDL_19_2 is mechanistically a data poisoning propagation problem enabled by attribution amnesia. MP-2.1-001 on documenting data origin and content lineage, and MS-2.8-003 on digital content transparency for provenance tracking, provide peripheral governance-level relevance to the attribution gap in RDL_19_2. However, NIST AI 600-1 does not prescribe error message sanitization policies, inter-agent error isolation boundaries, or provenance serialization requirements for multi-agent handoff protocols. The provenance tracking guidance in the framework is oriented toward content provenance of AI outputs (watermarking, digital signatures for generated content), not toward runtime parameter provenance across agent execution boundaries. MANAGE 4.1-002 on post-deployment monitoring could in principle capture incidents arising from these failures, but is downstream rather than preventive. Overall, the framework does not reach the specific multi-agent execution and orchestration mechanics described in RDL_19.

---

### RDL_20 - Multi-Agent Reasoning
- Strength score: 2

RDL_20 addresses security failures arising from multi-agent reasoning architecture: cascading effects from flawed or injected orchestrator reasoning propagating to all workers (RDL_20_1), delegation transparency enabling attackers to exploit disclosed trust assumptions (RDL_20_2), system-level reasoning incoherence that no single agent can validate (RDL_20_3), and consensus mechanisms vulnerable to injections hidden in non-contributory reasoning portions (RDL_20_4). NIST AI 600-1 provides moderate coverage through several applicable provisions. The Information Security category (§2.9) names indirect prompt injection as a recognized risk — the mechanism underlying RDL_20_1 (injected orchestrator logic) and RDL_20_4 (consensus injection) — and MEASURE 2.7-007 mandates red-teaming against prompt injection. The Confabulation category (§2.2) and MEASURE 2.9 on model explanation and validation are relevant to RDL_20_1 and RDL_20_3: when orchestrators produce reasoning that is internally plausible but systemically incoherent, this is functionally a confabulation failure with cascading consequences; the framework encourages evaluation of whether AI explanations are auditable and sound. The Human-AI Configuration category (§2.7) and GOVERN 3.2 on human oversight design are relevant to RDL_20_1 and RDL_20_3, as human-in-the-loop checkpoints at orchestrator reasoning boundaries would reduce cascading propagation risk. Pre-deployment testing guidance (MP-5.1-005 on adversarial role-playing and chaos testing) can in principle probe multi-agent reasoning pipelines for these failure modes. The significant gaps are that NIST AI 600-1 does not define multi-agent trust architecture requirements, prescribe isolation boundaries between reasoning agents, provide controls for consensus validation, or address the transparency paradox in RDL_20_2 where transparency itself becomes a vulnerability. These are architectural and protocol-level requirements that the governance profile cannot resolve.

---
### RDL_21 - Efficiency and Cost Attribution
- Strength score: 1

RDL_21's seven sub-threats describe how efficiency monitoring infrastructure — token consumption logs, API call telemetry, cache hit rates, latency measurements, cost attribution metadata, efficiency dashboards, and budget reallocation records — becomes an intelligence source about what processing is occurring, what data is sensitive, and what the system prioritizes. NIST AI 600-1's Data Privacy category (§2.4) identifies PII leakage and deanonymization as risks, and at a high level, inferring sensitive content from efficiency side-channels is a form of information leakage the profile would recognize as a concern. GOVERN function policies can in principle require that operational monitoring data be treated as confidential. However, NIST AI 600-1 was developed as a governance and organizational risk profile for GenAI model behavior; it has no subcategories, suggested actions, or evaluation guidance that addresses telemetry infrastructure, cost attribution system design, cache metric access controls, latency correlation analysis, or budget allocation transparency. The threat model in RDL_21 — where an attacker recovers sensitive operational intelligence from efficiency observability tooling rather than from the AI model itself — is entirely outside the scope of the framework's information security category (§2.9), which targets prompt injection and data poisoning at the model layer. None of the four primary GAI considerations (Governance, Content Provenance, Pre-deployment Testing, Incident Disclosure) address efficiency monitoring systems. The framework provides no countermeasures for any of RDL_21's specific mechanisms.

---

### RDL_22 - Infrastructure and Deployment
- Strength score: 1

RDL_22 covers information leakage through deployment infrastructure components: message queue correlation headers enabling interaction sequence reconstruction (RDL_22_1), vector database embedding exposure enabling approximate text recovery (RDL_22_2), Prometheus metric label cardinality leaking behavioral data (RDL_22_3), API gateway access log correlation for workflow reconstruction (RDL_22_4), MLflow versioning history leaking operational and security-relevant changes (RDL_22_5), dead-letter queue content exposure (RDL_22_6), and centralized logging pipelines as cross-agent exfiltration channels (RDL_22_7). NIST AI 600-1's Value Chain and Component Integration category (§2.12) and GOVERN 6.1/6.2 address third-party component accountability, which provides a thin organizational hook for evaluating the security posture of infrastructure components such as vector databases and logging pipelines. Data Privacy (§2.4) acknowledges PII leakage as a named risk, which could apply in principle to payload content in dead-letter queues or gateway logs. However, none of these framework provisions reach the specific threat mechanics: embedding inversion attacks, Prometheus label cardinality analysis, correlation ID-based workflow reconstruction, MLflow versioning metadata exposure, or the multi-agent aggregation risk in centralized logging are infrastructure security problems that require technical controls — access control policies, message queue header scrubbing, embedding sanitization, log retention scoping — that NIST AI 600-1 does not specify. The framework does not address message queue architecture, observability stack configuration, or vector database tenant isolation. Coverage is weak across all seven sub-threats.

---

### RDL_23 - Containerization Security
- Strength score: 1

RDL_23 describes container and Kubernetes infrastructure threats: CPU cache side channels between co-hosted containerized agents (RDL_23_1), event broker retention creating durable sensitive data leakage (RDL_23_2), overly permissive PersistentVolume mounts enabling cross-agent log access (RDL_23_3), sidecar proxy log interception in service meshes (RDL_23_4), init container environment variable leakage through crash logs (RDL_23_5), unauthenticated kubelet metrics port exposure (RDL_23_6), and etcd database backup leakage of cluster state including secrets (RDL_23_7). NIST AI 600-1 explicitly does not address infrastructure security: network architecture, container orchestration, Kubernetes configuration, GPU hardware, and real-time monitoring are outside the framework's stated scope. The Information Security category (§2.9) covers prompt injection and data poisoning at the model layer, not hardware side-channel attacks, container privilege escalation, or Kubernetes control plane security. Value Chain governance (§2.12, GOVERN 6.1/6.2) could in principle require that deployment infrastructure vendors meet security standards, but provides no specific guidance on container runtime isolation, service mesh configuration, kubelet authentication, or etcd encryption. CPU cache side channels (RDL_23_1) and KV cache inter-GPU communication (referenced in RDL_26) are hardware-layer attack surfaces that no governance-oriented policy profile can resolve. None of the four primary GAI considerations apply. NIST AI 600-1 has no useful coverage of any sub-threat in RDL_23.

---

### RDL_24 - Profiling and Optimization
- Strength score: 1

RDL_24 concerns leakage through profiling and optimization infrastructure: detailed execution traces containing memory contents and timing data (RDL_24_1), MLflow artifact registries storing prompt artifacts and evaluation datasets with sensitive business logic (RDL_24_2), baseline performance metrics revealing historical operational parameters (RDL_24_3), and optimization process logs documenting configuration changes and parameter tuning (RDL_24_4). NIST AI 600-1's pre-deployment testing emphasis and MEASURE 2.6/2.7 are relevant in that the profile acknowledges evaluation artifacts as part of the AI system lifecycle requiring governance. The Value Chain category (§2.12) could marginally apply to MLflow as a third-party artifact management component. However, the framework does not prescribe access controls for artifact registries, retention policies for execution traces, or protections for baseline metrics repositories. The specific threats — execution trace memory content leakage, inverse inference from profiling data, configuration parameter exposure through optimization logs — are software profiling and operational security concerns entirely outside what a GenAI governance profile addresses. NIST AI 600-1's security evaluation guidance (MEASURE 2.7) is aimed at assessing model behavior under adversarial conditions, not at securing the profiling and optimization toolchain that surrounds the model. The governance mandate implies that organizations should protect sensitive operational data, but this is too abstract to constitute meaningful coverage of RDL_24's specific leakage mechanisms.

---

### RDL_25 - Kubernetes Orchestration
- Strength score: 1

RDL_25_1 describes a single threat: Kubernetes audit logs revealing multi-agent orchestration logic through pod creation, scheduling, scaling, and failure event records, enabling attackers to identify inter-agent dependencies and critical infrastructure components. NIST AI 600-1 does not address Kubernetes or container orchestration at any level. The framework explicitly excludes infrastructure security from its scope. The Information Security category (§2.9) covers prompt injection and data poisoning as model-layer threats, not audit log security or Kubernetes RBAC configuration. The Value Chain category (§2.12) and GOVERN 6.1/6.2 address accountability for third-party AI components, but Kubernetes is deployment infrastructure rather than an AI component subject to the framework's supply chain vetting guidance. Multi-agent orchestration topology as exposed through Kubernetes event auditing is an operational security and infrastructure configuration problem — requiring audit log access controls, RBAC hardening, and network policy restrictions — that falls entirely outside what a governance-oriented GenAI risk profile covers. NIST AI 600-1 provides no coverage of RDL_25_1.

---

### RDL_26 - GPU and Model Internals
- Strength score: 1

RDL_26 covers eight sub-threats arising from GPU hardware, inference engine internals, and quantization artifacts: per-model GPU memory metrics enabling model inference pattern reconstruction (RDL_26_1), verbose container logs writing inference intermediates to centralized systems (RDL_26_2), queue depth metrics enabling workload reverse engineering (RDL_26_3), KV cache values transferred unencrypted across GPUs during tensor parallelism (RDL_26_4), engine binary metadata enabling model extraction (RDL_26_5), INT8 quantization artifacts leaking activation range information (RDL_26_6), engine traces revealing inference characteristics through timing and memory allocation (RDL_26_7), and quantization calibration statistics leaking calibration dataset distribution (RDL_26_8). NIST AI 600-1 explicitly excludes GPU hardware and infrastructure security from its scope. None of these sub-threats arise from model governance gaps, organizational policy failures, or the risk categories the profile defines. The Information Security category (§2.9) addresses prompt injection, data poisoning, and offensive cyber at the AI system behavior level, not hardware-layer side channels, inter-GPU communication encryption, or binary metadata analysis. Environmental Impacts (§2.5) acknowledges GPU resource consumption as a concern but in the context of energy and carbon footprint, not security. Value Chain governance (§2.12) could theoretically require that GPU vendors and inference engine providers meet security standards, but provides no technical countermeasures for side-channel attacks on tensor parallelism, quantization artifact inference, or engine trace analysis. These are hardware security, cryptographic engineering, and binary hardening problems that no governance profile for GenAI risk management is designed to address.

---
### RDL_27 - Load Balancing
- Strength score: 1

RDL_27's threats are pure infrastructure side-channel attacks: load balancer routing observable as capability inference (RDL_27_1), dynamic batching timeouts leaking batch composition through latency patterns (RDL_27_2), cache hit/miss timing revealing what data agents are accessing (RDL_27_3), monitoring endpoint metrics exposing replica health and utilization (RDL_27_4), and auto-scaling event timing as an attack signal for identifying load management strategies (RDL_27_5). None of these mechanisms have any mapping to NIST AI 600-1's risk categories or suggested actions. The Information Security category (§2.9) covers prompt injection and data poisoning but not timing side-channels or traffic analysis against inference infrastructure. The Data Privacy category (§2.4) addresses PII leakage through model outputs, not latency-derived inference attacks against load balancers. Value Chain and Component Integration (§2.12) requires third-party component vetting but does not govern how those components expose metrics or timing signals to adversaries. NIST AI 600-1's governance mandate — organizational policies, pre-deployment testing of model behavior, human oversight configuration — operates entirely above the infrastructure layer where these threats exist. The framework prescribes no controls for monitoring endpoint access restriction, metric exposure minimization, batching window obfuscation, or scaling event concealment. RDL_27 represents a class of network and infrastructure security concerns that are categorically outside the scope of a governance-oriented GenAI risk profile.

---

### RDL_28 - Planning Systems
- Strength score: 1

RDL_28 describes confidentiality risks specific to Hierarchical Task Network (HTN) planning architectures used in agentic AI: context window size as a target indicator for data extraction (RDL_28_1), serialized task networks leaking system capabilities (RDL_28_2), shared method library access enabling planning space reconnaissance (RDL_28_3), constraint specifications revealing protected resource boundaries (RDL_28_4), state abstraction mappings disclosing what information is treated as sensitive (RDL_28_5), decomposition pattern analysis leaking planning priorities (RDL_28_6), and task completion logging enabling workflow reconstruction (RDL_28_7). NIST AI 600-1 does not address agentic planning architectures, HTN systems, or the confidentiality of internal AI reasoning structures. The Information Security category (§2.9) covers prompt injection and data poisoning as attack vectors against AI systems but does not address inference attacks against planning state, method library exposure, or constraint specification leakage. The Data Privacy category (§2.4) covers model-level PII leakage but not the architectural reconnaissance enabled by observing planning decomposition patterns or task logs. GOVERN 6.1/6.2's supply chain accountability applies to third-party components but not to the security design of internal planning logic. Pre-deployment testing under MEASURE 2.6 and 2.7 could in principle probe whether planning outputs expose sensitive system information, but the framework provides no guidance toward this. NIST AI 600-1 was developed with text-generation risks in mind, and the structural and operational confidentiality risks of symbolic AI planning subsystems fall entirely outside the governance and risk categories it defines.

---

### RDL_29 - Monte Carlo Tree Search (MCTS)
- Strength score: 1

RDL_29 addresses confidentiality risks arising from MCTS-based decision making in agentic systems: stored state-action pairs and visit counts enabling reverse-engineering of planning decisions in multi-agent contexts (RDL_29_1), simulation traces containing tool calls and parameter values for never-executed actions becoming exfiltration surfaces if logged (RDL_29_2), value network confidence scores disclosing that certain action sequences appear high-value (RDL_29_3), observable rollout policy behavior revealing implicit strategy information in multi-agent deployments (RDL_29_4), and replanning search trees leaking tool capabilities and system state through multi-agent logs (RDL_29_5). NIST AI 600-1 has no applicable risk categories for these threats. MCTS is a search and planning algorithm; the confidentiality risks here are about the internal computational artifacts of the planning process — tree statistics, simulation trajectories, value estimates — becoming observable to adversaries. The framework's Information Security category (§2.9) addresses prompt injection and data poisoning but not inference attacks against search tree statistics or simulation trace logging. Data Privacy (§2.4) covers PII in model outputs, not probabilistic reconstruction of sensitive data from search trajectory artifacts. The framework's four primary GAI considerations — Governance, Content Provenance, Pre-deployment Testing, Incident Disclosure — provide no applicable controls for MCTS-specific leakage channels. These are highly specialized algorithmic and implementation-level security properties that governance-level policy instruments cannot address without a technical foundation the profile does not provide.

---

### RDL_30 - Monitoring and Callbacks
- Strength score: 1

RDL_30_1 describes a single but significant threat: during multi-agent execution monitoring, agents broadcast discovered discrepancies — tool unavailability, obstacle encounters, failed operations — to teammates as part of coordination protocols, and these broadcasts enumerate the exact state space regions being explored and tools being attempted, enabling passive observers to map system capabilities and operational boundaries. This is an operational security risk arising from the design of multi-agent coordination and monitoring callback architectures. NIST AI 600-1's Information Security category (§2.9) covers direct and indirect prompt injection, data poisoning, and offensive cyber actions against AI systems, but not the leakage of operational state information through inter-agent broadcast protocols. The Data Privacy category (§2.4) addresses PII leakage in model outputs, not the disclosure of system capability maps through coordination messages. GOVERN 3.2 establishes organizational accountability for human-AI interaction design but does not reach the design of agent-to-agent communication protocols. Pre-deployment testing (MEASURE 2.6, 2.7) could probe whether coordination broadcasts expose sensitive information, but the framework does not identify this as a testing concern. The specific mechanism — discrepancy enumeration as a side-channel through necessary coordination behavior — is an agentic system design problem that requires architectural controls the profile neither acknowledges nor prescribes.

---

### RDL_31 - Episodic Memory
- Strength score: 2

RDL_31 addresses confidentiality and exfiltration risks arising from episodic memory systems used in agentic AI: external episodic databases as covert exfiltration channels where agents embed sensitive data in stored episodes (RDL_31_1), vector embedding inference enabling probabilistic reconstruction of episode content (RDL_31_2), consolidation abstractions preserving enough information for reconstruction (RDL_31_3), graph database traversal enabling operational pattern reconstruction (RDL_31_4), deduplication metadata enabling frequency analysis of routine versus rare operations (RDL_31_5), and temperature/sampling artifacts in episodic retrieval creating information channels through variance analysis (RDL_31_6). NIST AI 600-1 provides partial coverage through two applicable categories. The Data Privacy risk category (§2.4) explicitly names PII leakage and deanonymization as risks and calls for data minimization governance — directly relevant to episodic storage systems that accumulate sensitive information about user interactions and agent operations over time, and to the covert exfiltration channel described in RDL_31_1. The Information Security category (§2.9) covers data poisoning, which is adjacent to the covert channel threat where a compromised agent deliberately embeds sensitive data in episode stores. Value Chain (§2.12) and GOVERN 6.1/6.2 apply to external episodic database components as third-party infrastructure requiring security vetting. Pre-deployment testing under MEASURE 2.6 and 2.7 could in principle evaluate whether episodic memory systems expose sensitive data through their retrieval interfaces. However, NIST AI 600-1 does not address vector embedding architecture, embedding inversion attack resistance, graph database access controls, deduplication metadata privacy, or sampling artifact side-channels — all of which require technical controls the framework cannot specify. The moderate score reflects genuine §2.4 and §2.9 relevance for the covert exfiltration and data accumulation threats, without coverage of the embedding-level and statistical inference mechanisms.

---

### RDL_32 - Semantic Memory
- Strength score: 2

RDL_32 describes confidentiality risks in RAG-based semantic memory systems: sensitive documents in knowledge bases leaking through context windows to agents and monitoring systems (RDL_32_1), retrieval patterns revealing what documents exist through systematic query behavior (RDL_32_2), embedding similarity patterns enabling document fingerprinting (RDL_32_3), temporal metadata on knowledge base documents leaking system evolution history (RDL_32_4), and knowledge graph relationship structures inferred through absence patterns disclosing sensitive relationship information (RDL_32_5). NIST AI 600-1 provides moderate coverage. The Data Privacy risk category (§2.4) is most directly applicable: the profile explicitly names PII leakage and deanonymization as risks, and the presence of sensitive documents in RAG knowledge bases contributing to model context window exposure (RDL_32_1) is a form of data privacy failure at the model level that §2.4 governance actions — data minimization, access review, privacy-preserving practices — are designed to address. The Information Security category (§2.9) covers indirect prompt injection, which is mechanistically related to malicious document injection into knowledge bases to manipulate RAG retrieval and downstream agent behavior. GOVERN 6.1/6.2 and Value Chain (§2.12) require third-party component vetting applicable to external vector databases and knowledge stores. Pre-deployment testing under MEASURE 2.7 could evaluate whether knowledge base retrieval surfaces sensitive content under adversarial query strategies. The limitations are significant: NIST AI 600-1 does not address embedding similarity attacks, document fingerprinting, temporal metadata privacy, knowledge graph inference through absence, or retrieval pattern enumeration — all of which require architectural controls at the vector database and knowledge store layers. The framework's policy instruments can establish accountability for what data enters a knowledge base but cannot govern the mathematical properties of the retrieval mechanisms that create these leakage channels.

---
### RDL_33 - RAG Leakage
- Strength score: 2

The five sub-threats in RDL_33 describe information leakage through RAG pipeline internals: relevance scores as probabilistic side channels (RDL_33_1), query rewriting logs as intent disclosure (RDL_33_2), deduplication artifacts revealing document similarity (RDL_33_3), silent compression validation failures propagating hallucinated content into episodic memory as validated facts (RDL_33_4), and semantic-to-working memory distillation including identifying information about agents and users (RDL_33_5). NIST AI 600-1 provides partial coverage through two converging angles. First, the Confabulation risk category (§2.2) directly names the scenario underlying RDL_33_4: GAI systems confidently presenting erroneous content when validation fails, and the framework explicitly calls for measuring and managing confabulation. MS-2.9-002 requires documenting RAG approaches as part of model documentation, and MS-2.3-001 calls for evaluating baseline model performance specifically for retrieval-augmented generation systems — the only explicit RAG reference in the framework. Second, the Data Privacy category (§2.4) covers PII leakage and deanonymization, which partially maps onto RDL_33_5 where distillation captures identifying information about users or agent instances. MS-2.10-001 mandates red-teaming for reverse engineering and membership inference risks. However, NIST AI 600-1 does not address the mechanics of relevance score side channels, query rewriting log exposure, deduplication artifact inference, or cross-agent episodic memory contamination as distinct threat vectors. The framework's coverage is genuine but operates at the governance and model-behavior level rather than at the RAG pipeline architecture level where these leakage channels exist.

---

### RDL_34 - Utility Functions and Explanations
- Strength score: 1

RDL_34's seven sub-threats concern information leakage through explanation and decision-disclosure mechanisms: progressive disclosure UI layers leaking cross-agent context (RDL_34_1), utility parameter exposure through expanded technical views (RDL_34_2), streaming decision rationale revealing utility calculations before redaction (RDL_34_3), error messages disclosing expected outcome utility values (RDL_34_4), inference trace leakage in explanation generation (RDL_34_5), working memory content exposure through shared persistence across agent boundaries (RDL_34_6), and rule condition inference through logical structure (RDL_34_7). NIST AI 600-1 provides no meaningful coverage of these threats. The framework's closest touchpoint is MS-2.9-001, which calls for applying ML explanation techniques including gradient-based attributions and embedding analysis — but this is guidance on using explanations for model validation, not guidance on preventing explanations from leaking sensitive information. The Human-AI Configuration category (§2.7) and GOVERN 3.2 address automation bias and oversight design but say nothing about controlling the information content of agent explanation outputs. The Information Security category (§2.9) names prompt injection and data poisoning, not explanation-layer side channels or streaming rationale interception. Multi-agent working memory boundary controls (RDL_34_6) and rule condition inference (RDL_34_7) require architectural isolation mechanisms and symbolic reasoning system design constraints that are entirely outside the framework's scope. The profile's incident disclosure provisions (MANAGE 4.1–4.3) are reactive and do not prevent the leakage mechanisms. NIST AI 600-1 was not designed to govern agent explanation architectures, utility function transparency, or multi-agent memory isolation, and none of these sub-threats map to its categories or suggested actions in any substantive way.

---

### RDL_35 - Reinforcement Learning and Policy
- Strength score: 2

RDL_35 encompasses seven sub-threats arising from learned policy internals: gradient-based model inversion attacks reconstructing training data from learned policies (RDL_35_1), value function reconstruction revealing training contexts (RDL_35_2), black-box policy querying to reconstruct training data distributions (RDL_35_3), replay buffer side-channel attacks (RDL_35_4), reward function reverse engineering (RDL_35_5), learning curve analysis as training data leakage (RDL_35_6), and context window expansion concentrating sensitive multi-agent data (RDL_35_7). NIST AI 600-1 provides moderate but genuine coverage for this category. MS-2.10-001 under MEASURE 2.10 explicitly calls for red-teaming to assess "outputting of training data samples, and subsequent reverse engineering, model extraction, and membership inference risks" — language that directly encompasses the training data reconstruction attacks described in RDL_35_1, RDL_35_2, and RDL_35_3. MS-2.7-007 requires red-teaming for ML attacks including "adversarial examples/prompts, data poisoning, membership inference, model extraction" and also lists "AI inference" as a resilience concern, which covers the inference-based attack surface in RDL_35_3. The Data Privacy category (§2.4) names data memorization as a named risk, with the framework acknowledging that training data can be recovered even from small-sample inclusions — relevant to reward function and value function reconstruction. MS-2.7-001 lists "reverse engineering" and "AI inference" explicitly as security concerns to assess. However, NIST AI 600-1 does not address reinforcement learning or policy-based systems specifically, does not mention experience replay buffers, reward function structure, or RLHF security beyond a documentation reference in MS-2.9-002. The context window concentration risk in hybrid multi-agent systems (RDL_35_7) is not addressed as an architectural isolation problem. The framework's coverage is genuine at the policy level (membership inference, model extraction red-teaming) but stops short of the RL-specific mechanisms.

---

### RDL_36 - Multi-Agent Coordination
- Strength score: 2

RDL_36 presents two sub-threats arising from multi-agent data sharing structures: conversation history accumulating sensitive data across agents where probabilistic token generation may surface it (RDL_36_1), and knowledge graph embedding inference enabling recovery of sensitive relationship weights (RDL_36_2). NIST AI 600-1 provides partial coverage primarily through its Data Privacy category (§2.4) and associated measurement actions. The framework explicitly identifies that "models may leak, generate, or correctly infer sensitive information about individuals" and acknowledges data memorization as an exacerbated risk — applicable to accumulated conversation history in shared multi-agent contexts (RDL_36_1). MS-2.10-001 mandates red-teaming specifically for "membership inference risks" and "revealing... sensitive, or trade-marked information," and MS-2.10-003 calls for verifying deduplication of training data — the latter having indirect relevance to shared history data management. MS-2.7-007 lists membership inference as an ML attack requiring red-teaming. For RDL_36_2, the gradient-based attribution documentation in MS-2.9-001 and the embedding analysis guidance acknowledge embeddings as an explanation artifact, and MS-2.7-001 lists "AI inference" as a vulnerability category, providing thin but genuine coverage for the embedding inference attack surface. The Value Chain category (§2.12) and GOVERN 6.1/6.2 address third-party component accountability and could apply to the multi-party nature of multi-agent deployments where cross-agent information boundaries must be governed. The gap is that NIST AI 600-1 does not address multi-agent architecture specifically — it has no guidance on conversation history partitioning, cross-agent context boundary enforcement, or knowledge graph access control. The framework recognizes the risk class (data leakage through model inference) but cannot prescribe the architectural controls needed to isolate agents sharing persistent state.

---


## RIP+RIDC - Agent identity, provenance, and economic/resource attack surface

### RIP_1 - Multi-Agent UI & Dashboard Attacks
- Strength score: 1

NIST AI 600-1 does not address multi-agent dashboard UIs, agent identity indicators, or forensic attribution in multi-agent pipelines. The framework's Information Security risk category (§2.9) names prompt injection as a recognized attack vector — relevant to RIP_1_4, where GroupChat message history can be manipulated through prompt injection to falsify sender identity — but the suggested actions (e.g., MS-2.7-007 on red-teaming for prompt injection) target model-level defenses, not UI-layer identity display or multi-agent session attribution. Content provenance mechanisms described in Appendix A address tracing origin and history of AI-generated content (text, images, datasets) using watermarking and metadata recording, but these are oriented toward detecting synthetic media, not authenticating intra-pipeline agent identities. Real-time cost attribution (RIP_1_3) has no coverage whatsoever; environmental impact measurement (MEASURE 2.12) tracks compute resource usage at a sustainability level, not per-agent economic accountability. The forensic blind spots in RIP_1_2 are not addressed: NIST has no provisions for attribution logging across multi-agent pipelines. GOVERN 3.2 calls for human oversight policies in human-AI configurations, which is at best a peripheral framing for the oversight gap, but does not translate into any concrete requirement for agent-level attribution logs or identity verification at the UI layer.

---

### RIP_2 - Approval & Workflow Attacks
- Strength score: 1

NIST AI 600-1's closest relevant provisions are in the Human-AI Configuration risk category (§2.7) and GOVERN 3.2, which call for policies addressing automation bias, over-reliance, and the design of human-AI oversight roles. These are tangentially applicable to HITL approval workflows (RIP_2_5), in that the framework encourages defining human oversight responsibilities and guarding against undue deference to automated outputs — but the framework sets no requirements for cryptographic provenance trails in approval pipelines. RIP_2_1 and RIP_2_4 concern cryptographic accountability gaps across multi-stage pipelines: NIST's content provenance discussion (Appendix A) treats provenance as a mechanism for tracing synthetic content origins (watermarking, metadata), not for securing decision audit trails within agentic workflows. RIP_2_2 and RIP_2_3 — attacks via aggregated confidence score manipulation and client-side state tampering — are entirely outside scope; the framework has no UI/UX design requirements or client-side state integrity controls. MEASURE 2.7 red-teaming and MEASURE 2.9 model documentation provisions address model-level explainability but say nothing about the integrity of intermediate pipeline outputs or how approval interfaces display provenance. The attribution fragmentation problem in RIP_2_5 (multiple actors and agents contributing to a decision) is at best partially framed under GOVERN 2.1's incident roles-and-responsibilities provisions, but no actionable control exists.

---

### RIP_3 - Command Palette & UI Attacks
- Strength score: 1

Command palette attacks — resource cost transparency (RIP_3_1), attribution confusion (RIP_3_2), and per-agent rate limiting bypass (RIP_3_3) — fall almost entirely outside NIST AI 600-1's scope. The framework does not address UI/UX design, command suggestion interfaces, or rate limiting architectures. RIP_3_1's denial-of-service vector (expensive operations triggered via context manipulation) has no coverage; the framework's Environmental Impacts category (§2.5, MEASURE 2.12) tracks aggregate compute consumption for sustainability purposes, not real-time cost visibility in interfaces. RIP_3_2 is conceptually adjacent to the Information Security risk (§2.9) on prompt injection and the content provenance theme — injecting commands appearing from trusted agents resembles indirect prompt injection — but NIST's suggested actions address model-level prompt injection defenses (MS-2.7-007), not which agent label appears in a command palette UI. RIP_3_3 involves infrastructure-level rate limiting granularity; the framework is silent on this entirely. The only peripheral applicability is GOVERN 3.2's acceptable-use policies for GAI interfaces (GV-3.2-003), which can nominally encompass command palette misuse scenarios, but provides no actionable technical controls.

---

### RIP_4 - Multi-Agent Coordination Attacks
- Strength score: 1

Multi-agent coordination attacks — reflection-amplified resource exhaustion (RIP_4_1), orchestrator overload (RIP_4_2), auction manipulation (RIP_4_3), and message flood denial-of-service (RIP_4_4) — are outside NIST AI 600-1's scope. The framework does not address multi-agent system architectures, agent coordination patterns, or denial-of-service through resource exhaustion. Economic/DoS attacks (RIP_4_1, RIP_4_2, RIP_4_4) have no corresponding risk category; Environmental Impacts (§2.5) covers compute sustainability but not adversarial resource abuse. Market-based coordination manipulation (RIP_4_3) via strategic bidding and coalition formation has no coverage anywhere in the framework. NIST's Information Security risk category (§2.9) mentions availability as a GAI system property to protect and notes that conventional cybersecurity practices must evolve, but offers no specific actions for multi-agent DoS or coordination-layer attacks. MEASURE 2.7 red-teaming (MS-2.7-007) includes "sponge examples" as a listed attack type, which is conceptually related to resource exhaustion, but this refers to model-level inference cost attacks, not multi-agent reflection cascades or orchestrator flooding. No suggested action in the framework addresses coordination protocols, agent messaging architectures, or cascade failure prevention.

---

### RIP_5 - Framework-Specific Attacks
- Strength score: 1

Framework-specific vulnerabilities in LangChain, LangGraph, AutoGen, CrewAI, and Semantic Kernel are explicitly out of scope for NIST AI 600-1. The framework is a governance-level profile and does not address implementation details of specific AI frameworks. Identity translation gaps across heterogeneous frameworks (RIP_5_1), mixed-framework cost attribution impossibility (RIP_5_2), AutoGen's unsigned message-content identity model (RIP_5_3), CrewAI's unverified role assignment (RIP_5_4), AutoGen conversation resumption amplification (RIP_5_5), and Semantic Kernel plugin registry poisoning (RIP_5_6) are all implementation-specific vulnerabilities with no analogue in the framework's risk categories or suggested actions. The Value Chain risk (§2.12) and GOVERN 6.1/6.2 supply chain provisions address third-party component vetting at a procurement and transparency level — ensuring SBOMs, SLAs, and supplier risk assessment frameworks — which is at best a distant organizational-governance framing for the risk that a third-party framework component (e.g., AutoGen, CrewAI) introduces identity or cost vulnerabilities. MANAGE 3.1 (MG-3.1-002) recommends testing for value chain risks including software vulnerabilities, which is the nearest applicable action, but this is generic supply chain hygiene rather than framework-specific agentic vulnerability assessment. Cryptographic agent identity, cross-framework trust translation, and plugin registry integrity are not addressed.

---
### RIP_6 - Tool & Plugin Attacks
- Strength score: 2

NIST AI 600-1 offers partial but uneven coverage across RIP_6's fifteen sub-threats. The clearest applicable coverage is for RIP_6_11 (tool access token leakage through agent communication): §2.4 (Data Privacy) and its associated GOVERN and MEASURE subcategories address PII leakage and sensitive data exposure at the model level, and while the profile does not specifically address credential passing between agents, the general data privacy governance posture applies. For RIP_6_6, RIP_6_8, RIP_6_9, RIP_6_14, and RIP_6_15 — which concern plugin identity ambiguity, invocation attribution spoofing, privilege inheritance through agent composition, authority delegation in hybrid workflows, and privilege inference and escalation — §2.12 (Value Chain and Component Integration) provides relevant governance framing: GOVERN 6.1 and 6.2 call for supply chain accountability and third-party vetting, and the profile recommends pre-deployment testing of third-party components and plugins. This partially addresses concerns about unvetted plugins in registries (RIP_6_7) and tool metadata trustworthiness (RIP_6_3, RIP_6_6). §2.9 (Information Security) names prompt injection as a risk and recommends red-teaming and adversarial testing, which has marginal relevance to tool invocation manipulation (RIP_6_8, RIP_6_12). However, the profile has no coverage for the majority of economic and infrastructure-level sub-threats: RIP_6_1 (multi-agent tool chaining cost amplification), RIP_6_2 (memory-cached cost attribution gaps), RIP_6_4 (scratchpad cost opacity), RIP_6_5 (aggregate cost reporting hiding per-agent breakdowns), RIP_6_7 (registry poisoning with expensive plugins), RIP_6_10 (distributed invocation cost obscuration), RIP_6_12 (tool failure attribution confusion), RIP_6_13 (profiling chain of custody loss), and RIP_6_15 (log-based privilege inference) are all implementation-level, infrastructure, or economic attack vectors that fall entirely outside NIST AI 600-1's governance scope. The framework does not define agent identity management, runtime cost attribution, or multi-agent trust mechanisms. The score reflects that roughly three to four of the fifteen sub-threats have meaningful, if indirect, coverage through §2.4, §2.9, and §2.12, while the remaining majority receive only peripheral policy framing at best.

---

### RIP_7 - Cost & Economic Denial-of-Service Attacks
- Strength score: 1

NIST AI 600-1 provides essentially no direct coverage of RIP_7's thirty-six sub-threats. The entire RIP_7 threat class is centered on economic denial-of-service through cost attribution ambiguity, resource quota exhaustion, provenance spoofing in reasoning and planning chains, session affinity pseudo-identity creation, load-balanced cost opacity, storage cost amplification through episode accumulation, embedding cache eviction exploitation, budget allocation confusion, utility function attribution spoofing, and parallelization overhead as an attack surface. None of these concepts appear in any of the profile's twelve GAI risk categories, and no suggested action within the GOVERN, MAP, MEASURE, or MANAGE functions addresses runtime resource accounting, per-agent cost metering, token price volatility exploitation, or infrastructure billing integrity. The closest tangential contact is §2.12 (Value Chain) through GOVERN 6.1 and 6.2, which call for third-party component vetting — this is marginally relevant to RIP_7_8 (resource attribution poisoning through third-party integrations) and RIP_7_22 (utility function attribution spoofing through external components), but only at the level of "vet your supply chain," not at the level of detecting or preventing economic manipulation during runtime. §2.7 (Human-AI Configuration) and its MEASURE subcategories concern automation bias and over-reliance, which have no relationship to cost-based attacks. The profile's MANAGE 4.1–4.3 subcategories address incident response and disclosure but do not define what constitutes an economic DoS incident or how to detect one. The score of 1 reflects that NIST AI 600-1 is structurally and intentionally a governance and risk categorization profile — it was not designed to address infrastructure economics, multi-agent cost accounting, or runtime resource exploitation. Organizations deploying agentic systems at scale will need dedicated cost management frameworks, per-agent billing controls, and infrastructure-level monitoring that are entirely outside this profile's scope.

---
# NIST AI 600-1 Risk Management Analysis: RIP_8 through RIP_12

---

### RIP_8 - Resilience & Error Handling Attacks
- Strength score: 1

NIST AI 600-1 offers negligible coverage of RIP_8. The seven threats in this subsubsection are all variants of economic denial-of-service or cost attribution failure operating at the infrastructure and multi-agent runtime layer: streaming response opacity hiding expensive background operations (RIP_8_1, RIP_8_7), retry budget exhaustion across agent boundaries (RIP_8_2), fallback provider cost ambiguity (RIP_8_3), circuit breaker overhead attribution gaps (RIP_8_4), graceful degradation cost imbalance (RIP_8_5), and streaming identity spoofing where early tokens claim a benign identity before malicious operations execute (RIP_8_6). None of NIST AI 600-1's 12 risk categories address economic abuse, cost attribution, billing systems, retry orchestration, or circuit breaker design. The Information Security category (§2.9) recognizes prompt injection and data poisoning but does not extend to streaming-layer identity claims or retry-budget manipulation. The Value Chain category (§2.12) addresses third-party component vetting, which is tangentially relevant if a fallback provider is a supply chain component — but this framing does not reach the cost attribution and economic cycling mechanics that define the attack surface. Environmental Impacts (§2.5) acknowledges compute consumption at a policy level, but only as a sustainability concern, not as an adversarial economic attack vector. MANAGE 4.1–4.3 covers incident response after harm is detected, but the framework provides no controls for detecting or preventing the real-time resource exhaustion and attribution opacity that RIP_8 describes. The profile is a governance document; these threats require operational monitoring, per-agent cost accounting, and streaming architecture controls that are entirely outside its scope.

---

### RIP_9 - Multimodal Attacks
- Strength score: 1

NIST AI 600-1 does not meaningfully address RIP_9. Three of the four threats are resource exhaustion and economic denial-of-service attacks targeting shared multimodal infrastructure: coordinated multimodal DDoS against shared vision model backends (RIP_9_2), exploitation of long-duration audio recordings to exhaust audio processing agents (RIP_9_3), and embedding vector store amplification from multimodal RAG systems creating infrastructure strain 3–5x beyond text-only deployments (RIP_9_4). These are infrastructure capacity and cost attacks that NIST does not address at any level. The fourth threat, multimodal content attribution spoofing through vision model outputs (RIP_9_1), involves creating falsified content that claims a different modality origin to enable cross-modality identity confusion. The Information Integrity risk category (§2.8) addresses synthetic and manipulated media — deepfakes and provenance falsification — which is the closest NIST analog; and the Content Provenance consideration encourages organizations to implement provenance tracking for AI-generated content. However, §2.8 is scoped to disinformation and content authenticity for end users, not to cross-agent attribution spoofing where a vision model's output is forged to mislead downstream agents. Pre-deployment testing (MEASURE 2.7) could in principle probe multimodal pipelines for such spoofing, but the framework offers no concrete guidance there. The three economic/infrastructure threats receive no coverage whatsoever, and the attribution spoofing threat receives only peripheral policy framing. The profile's scope does not reach the infrastructure capacity, shared-backend resource management, or cryptographic provenance binding that would be needed to address these risks.

---

### RIP_10 - Reasoning & Evaluation Attacks
- Strength score: 1

RIP_10's sixteen threats span evaluation cost attribution failures (RIP_10_1, RIP_10_6, RIP_10_9, RIP_10_11, RIP_10_12), evaluation identity and provenance spoofing (RIP_10_2, RIP_10_3, RIP_10_5, RIP_10_7, RIP_10_8, RIP_10_13), benchmark and metric poisoning (RIP_10_14, RIP_10_15, RIP_10_16), and economic exploitation of reasoning architecture parameters (RIP_10_4, RIP_10_10). NIST AI 600-1 has thin, indirect relevance in two areas. First, MEASURE 2.7 (security evaluation before deployment) and MEASURE 2.9 (model explanation and validation) are relevant to the category of evaluation integrity threats in the sense that the framework mandates pre-deployment security testing — if that testing is conducted seriously, it could surface evaluation agent identity spoofing (RIP_10_2) or benchmark attribution spoofing (RIP_10_7). Second, the Confabulation risk category (§2.2) and pre-deployment testing considerations loosely apply to few-shot metric poisoning (RIP_10_16), where poisoned examples teach incorrect metric semantics to analytics agents — this is a form of behavioral manipulation through training or context data that the framework's data integrity concerns touch at the margin. However, NIST AI 600-1 contains no controls for cryptographic identity binding in evaluation pipelines, no guidance on MCTS computational budget management or Pass@K evaluation integrity, no economic DoS protections, and no framework for securing evaluation authority delegation. The economic cycling attacks (RIP_10_4, RIP_10_9, RIP_10_11, RIP_10_12) are entirely outside scope. The spoofing threats require cryptographic provenance mechanisms that the governance profile does not prescribe. The offline-online distribution gap (RIP_10_14) and non-determinism exploitation (RIP_10_15) are evaluation architecture problems with no NIST analog. Overall, MEASURE 2.7 and 2.9 provide the only genuine but still indirect hooks; the vast majority of this subsubsection falls outside the framework's intended scope.

---

### RIP_11 - Vector Store & RAG Attacks
- Strength score: 2

NIST AI 600-1 has moderate but uneven coverage of RIP_11. The clearest mapping is to RIP_11_3 (knowledge base source attribution loss through multi-agent processing enabling instruction laundering): §2.9 explicitly names indirect prompt injection — injecting instructions into external data sources that agents retrieve and act upon — as a recognized Information Security risk. Multi-hop agent chains progressively losing source attribution is a structural indirect prompt injection vector, and pre-deployment testing guidance and MEASURE 2.7 support adversarial evaluation of retrieval pipelines. For RIP_11_9 (embedding API endpoint provenance confusion), RIP_11_10 (model version registry ambiguity), RIP_11_11 (shared API key provenance loss), RIP_11_12 (provider lock-in obscuring data provenance during migrations), and RIP_11_13 (ETL source attribution loss through metadata normalization), the Value Chain and Component Integration category (§2.12) and GOVERN 6.1/6.2 supply chain accountability subcategories are partially applicable — these are third-party data pipeline and provider integrity concerns that organizational supply chain vetting addresses at the policy level. The Data Privacy category (§2.4) is tangentially relevant to RIP_11_6 (vector database partition escaping for cross-agent identity confusion) insofar as it concerns unauthorized access to data belonging to different contexts, though the framework does not prescribe database partition isolation controls. The significant gap is the economic and resource exhaustion dimension: RIP_11_1 (cost-per-query exploitation), RIP_11_2 (resource accounting abuse), RIP_11_4 (knowledge base replication cost attacks), RIP_11_5 (indexing infrastructure cost exploitation), and RIP_11_7 (vector database query load amplification) are all economic denial-of-service variants with no applicable NIST control. RIP_11_8 (knowledge graph ownership and authorization ambiguity) is an access control problem that governance policies can acknowledge but not technically solve. The score reflects genuine but limited applicability: the indirect prompt injection and supply chain framing are real, but more than half of the attack surface in this subsubsection — particularly the economic and infrastructure-layer threats — falls outside the profile's scope.

---

### RIP_12 - Memory & Session Attacks
- Strength score: 2

NIST AI 600-1 has moderate coverage of the poisoning and integrity-related threats in RIP_12, with no coverage of the economic and resource exhaustion threats. The Information Security category (§2.9) explicitly recognizes data poisoning as a named risk, which maps onto RIP_12_9 (cross-agent context poisoning via session persistence), RIP_12_2 (corrupted checkpoint injection enabling malicious state propagation), RIP_12_11 (model checkpoint integrity attacks activating backdoors during resumption), and RIP_12_1 (poisoned context causing expensive operations on every session resumption — the poisoning mechanism itself, not the cost dimension). Pre-deployment testing and MEASURE 2.7 support adversarial evaluation of memory and context pipelines for poisoning susceptibility. GOVERN 6.1/6.2 supply chain accountability is relevant to RIP_12_11 if the checkpoint repository is a third-party or shared component. The Confabulation category (§2.2) and MEASURE 2.9 (model explanation and validation) provide peripheral coverage for RIP_12_8 (shared semantic memory losing attribution across agents), where distilled memory produces confidently wrong attributions in a manner analogous to confabulation. However, the economic denial-of-wallet threats — RIP_12_3 (unbounded iteration loops in LangGraph enabling denial-of-wallet), RIP_12_4 (conversation extension inflating storage costs), RIP_12_5 (MCTS tree growth as unbounded memory consumption), RIP_12_6 (embedding operation cost exploitation), RIP_12_7 (context inflation forcing excessive token consumption), and RIP_12_10 (memory serialization storage cost amplification) — are infrastructure cost attacks with no applicable NIST control. The framework does not address loop termination conditions, token budget enforcement, storage cost attribution, or per-agent memory quotas. The score reflects the genuine but bounded applicability of the data poisoning and supply chain framing to approximately half the threats in this subsubsection, offset by the complete absence of coverage for the economic attack vectors.

---
### RIP_13 - Infrastructure & Deployment Attacks
- Strength score: 1

NIST AI 600-1 is a governance and risk management profile scoped to AI model behavior, training data integrity, deployment policy, and organizational accountability. The nine sub-threats under RIP_13 are entirely infrastructure and platform security concerns: container escape via shared Kubernetes nodes (RIP_13_1), economic manipulation through profiling data in agent decision loops (RIP_13_2), NIM container image compromise through registry poisoning of the "latest" tag (RIP_13_3), replica identity ambiguity in generic Kubernetes pod templates (RIP_13_4), cost attribution blurring across shared GPU tenants (RIP_13_5), provenance loss in distributed engine caches (RIP_13_6), MIG partition starvation attacks (RIP_13_7), horizontal scaling identity discontinuity (RIP_13_8), and engine binary non-fungibility during hardware migration (RIP_13_9). None of the profile's twelve risk categories — including Information Security (§2.9), Value Chain and Component Integration (§2.12), or Data Privacy (§2.4) — address container isolation, GPU memory partitioning, image registry integrity verification, Kubernetes pod identity, or engine binary provenance. The Value Chain category and GOVERN 6.1/6.2 call for third-party component vetting before deployment, which is peripherally relevant to RIP_13_3 (compromised container image registries) and RIP_13_6 (cached engine provenance) in the narrow sense that a pre-deployment audit process could in principle include container image verification — but the profile prescribes no such technical controls, and its governance framing does not extend to runtime infrastructure exploitation or economic DoS through resource contention. NIST AI 600-1 was not designed to cover the attack surface RIP_13 describes, and no meaningful mapping is possible beyond the most general policy accountability framing.

---

### RIP_14 - ML/Training & Model Attacks
- Strength score: 2

NIST AI 600-1 provides partial but genuine coverage of RIP_14 through two risk categories. The Value Chain and Component Integration risk category (§2.12), with GOVERN 6.1 and 6.2, establishes organizational accountability for third-party components and supply chain integrity — this is directly applicable to RIP_14_2 (MLflow model registry identity spoofing through version tagging), RIP_14_3 (quantization artifact provenance verification gaps), and RIP_14_5 (training data provenance obfuscation), where the attack vector is an integrity failure in shared model registries or artifact pipelines that a pre-deployment vetting process could in principle address. The Information Security risk category (§2.9) explicitly names data poisoning as a covered threat, which maps onto RIP_14_5's training data provenance obfuscation and, at the training-input level, to RIP_14_4's behavioral fingerprinting and RIP_14_8's resource exhaustion attacks against federated training. MEASURE 2.6 and MEASURE 2.7 call for safety and security evaluation before deployment, which could include model provenance auditing and registry integrity checks. However, the framework's coverage has significant gaps: RIP_14_1 (model selection privilege boundary spoofing through API call interception) is an authentication and access control problem the profile does not address; RIP_14_6 (model ownership disputes through learning-based derivation) and RIP_14_7 (learned policies reducing compute in security-sensitive contexts) are emergent behavioral concerns at a level of specificity NIST does not reach; and RIP_14_8's TPU/GPU resource exhaustion in federated settings is an infrastructure denial-of-service concern outside the profile's scope. The score reflects that supply chain integrity governance and data poisoning recognition are applicable to several sub-threats, but the technical depth needed to prevent, detect, or respond to registry-level identity spoofing or quantization artifact tampering must be supplied by the implementing organization beyond what NIST AI 600-1 prescribes.

---

### RIP_15 - Kubernetes & Container Attacks
- Strength score: 1

The seven sub-threats under RIP_15 are all Kubernetes and container platform security concerns: service account token replay and identity pooling enabling lateral movement (RIP_15_1), IP spoofing via iptables manipulation in microservices (RIP_15_2), pod eviction ordering revealing agent priority hierarchy (RIP_15_3), GPU resource request gaming for hardware hoarding (RIP_15_4), namespace-based multi-tenancy isolation bypass (RIP_15_5), cluster node identity spoofing in gossip protocol communications (RIP_15_6), and load balancer VIP masking individual node provenance in audit trails (RIP_15_7). NIST AI 600-1 contains no applicable guidance for any of these vectors. The profile's twelve risk categories do not address network policy, RBAC, container security contexts, iptables rules, gossip protocol authentication, namespace isolation, or hardware resource quotas. The Information Security category (§2.9) covers prompt injection and data poisoning at the model layer — not Kubernetes network or identity infrastructure. The Value Chain category (§2.12) vets third-party AI components before deployment but says nothing about cluster node authentication or service mesh trust. An organization following the profile's GOVERN and MANAGE functions would have incident response and supply chain accountability policies, but these provide no technical control surface for the runtime Kubernetes exploitation described in RIP_15. This subsubsection is out of scope for NIST AI 600-1.

---

### RIP_16 - Load Balancer & Scaling Attacks
- Strength score: 1

RIP_16's four sub-threats concern load balancer and auto-scaling infrastructure: load balancer state exploitation enabling agent impersonation by routing requests between arbitrary replicas (RIP_16_1), load balancer endpoint interception creating man-in-the-middle scenarios (RIP_16_2), weighted routing patterns revealing privileged agents (RIP_16_3), and auto-scaling creating fresh-identity replicas that attackers impersonate (RIP_16_4). These are network infrastructure and platform security concerns — routing policies, TLS termination, traffic interception, and replica identity management — that fall entirely outside NIST AI 600-1's scope. The profile contains no risk category, suggested action, or AI RMF subcategory that addresses load balancer configuration, traffic routing integrity, or horizontal scaling identity continuity. The Human-AI Configuration category (§2.7) and the Value Chain category (§2.12) have no applicable surface here. The only marginal connection is that GOVERN 6.1 organizational accountability structures could theoretically demand that load balancer and scaling configurations be audited as part of a broader deployment governance process, but this is a general policy mandate that does not constitute meaningful coverage of the specific attack mechanisms in RIP_16. NIST AI 600-1 offers no substantive guidance for this subsubsection.

---
### RIP_17 - Service Discovery & Authentication Attacks
- Strength score: 1

NIST AI 600-1 is a governance and risk management profile focused on AI model behavior, content risks, and organizational policy — it does not address the infrastructure-level attack surface that defines RIP_17. The seven sub-threats span DNS cache poisoning (RIP_17_6), certificate authority compromise (RIP_17_7), RabbitMQ consumer identity spoofing (RIP_17_2), Kong API gateway header injection (RIP_17_3), Prometheus cardinality exhaustion (RIP_17_4), priority queue starvation (RIP_17_5), and kernel service registration audit gaps (RIP_17_1). None of these map to the framework's 12 risk categories. §2.9 (Information Security) is the closest relevant section, naming prompt injection and data poisoning as concerns, but it does not extend to network protocols, message brokers, certificate infrastructure, or observability systems. The framework's GOVERN and MANAGE functions could theoretically encourage organizational policies requiring infrastructure security auditing, but this is peripheral framing only — NIST AI 600-1 provides no named risk category, subcategory, or suggested action that directly or even moderately addresses service discovery, agent authentication at the transport layer, or multi-agent network-level identity integrity.

---

### RIP_18 - Rule/Method/Parameter Attribution Attacks
- Strength score: 2

RIP_18 spans a range of attribution and provenance integrity concerns in shared multi-agent ecosystems — including method authorship spoofing in HTN registries (RIP_18_2, RIP_18_8), rule attribution confusion and version control poisoning (RIP_18_4, RIP_18_9, RIP_18_10), parameter origin confusion and transitive trust exploitation (RIP_18_5, RIP_18_6), delegation chain verification gaps (RIP_18_3), efficiency metric spoofing (RIP_18_1), and provider economic lock-in through optimization poisoning (RIP_18_7). NIST AI 600-1 §2.12 (Value Chain and Component Integration) is partially applicable: it identifies supply chain integrity and third-party component vetting as risk concerns, and the AI RMF subcategories GOVERN 6.1 and 6.2 establish accountability expectations for third-party AI components and shared artifacts. Shared HTN method libraries, rule repositories, and version-controlled parameter stores can be treated as third-party or shared-component supply chain elements under this framing, making §2.12 a moderate fit for RIP_18_2, RIP_18_8, RIP_18_9, and RIP_18_10. However, the framework does not address the specific mechanisms of attribution confusion — it cannot detect parameter origin spoofing across agent handoffs (RIP_18_5, RIP_18_6), does not cover delegation chain verification (RIP_18_3), and has no suggested actions for runtime trust boundary enforcement or metric integrity (RIP_18_1, RIP_18_7). The framework's relevance is thus indirect: it can motivate governance policies requiring contributor accountability and provenance tracking in shared registries, but it neither defines nor operationalizes the technical controls needed to prevent these attribution attacks.

---

### RIP_19 - Hybrid & Cross-Paradigm Attacks
- Strength score: 2

RIP_19 addresses two compound threats arising from hybrid AI architectures that combine neural models, symbolic rule engines, and utility-based components. RIP_19_1 concerns paradigm attribution confusion and supply chain poisoning: when shared components (neural embeddings, enterprise rule repositories, utility functions) are compromised, the effect propagates across entire agent ensembles in a 1-to-N pattern. RIP_19_2 concerns identity spoofing that exploits the heterogeneity of credential mechanisms across paradigms — attackers craft credentials that appear valid within a specific paradigm's schema (neural embedding signatures, symbolic rule schemas, utility preference structures) to impersonate legitimate components. NIST AI 600-1 §2.12 (Value Chain and Component Integration) is the most directly applicable section: hybrid paradigm components sourced from different origins are squarely within the framework's conception of third-party and integrated AI components, and GOVERN 6.1/6.2 can establish vetting and accountability requirements for each component type before integration. The framework's pre-deployment testing guidance (MEASURE 2.6, MEASURE 2.7) also provides partial coverage by encouraging red-teaming and security evaluation of integrated systems, which could surface cross-paradigm trust boundary weaknesses. The gap is that NIST AI 600-1 does not address the runtime trust enforcement mechanisms needed to validate paradigm-specific credentials or detect cross-paradigm spoofing during agent operation — its coverage is limited to procurement, vetting, and pre-deployment evaluation rather than runtime identity verification across heterogeneous component types.

---
# NIST AI 600-1 Risk Analysis: RIDC Threat Categories

### RIDC_1 - UI and User Interaction Attack Surfaces
- Strength score: 2

NIST AI 600-1 §2.9 (Information Security) explicitly names indirect prompt injection as a named risk, and MEASURE 2.7 mandates red-teaming specifically for prompt injection. Several RIDC_1 threats map meaningfully to this coverage: progressive disclosure attacks (RIDC_1_1), chat interface attribution confusion (RIDC_1_2), and auto-retry amplification (RIDC_1_7) are all recognizable forms of indirect prompt injection where malicious content embedded in UI-delivered data hijacks agent behavior — a threat class §2.9 directly addresses. The Human-AI Configuration risk category (§2.7) also provides partial coverage for RIDC_1_2's social engineering dimension by flagging automation bias and over-reliance as governance concerns that organizations should mitigate through design policy. However, the majority of RIDC_1's specific threat mechanisms fall outside NIST's governance-level scope: ARIA live region manipulation (RIDC_1_3), keyboard shortcut exploitation (RIDC_1_4), accessibility semantic landmark spoofing (RIDC_1_5), command palette context prediction (RIDC_1_6), and time-based notification escalation (RIDC_1_8) are UI/accessibility implementation concerns that require web application security standards (WCAG, OWASP) and runtime monitoring — neither of which NIST AI 600-1 addresses. NIST does not prescribe UI architecture, accessibility feature hardening, or real-time behavioral controls. The framework's contribution is limited to directing organizations to conduct pre-deployment red-teaming for injection and to maintain human oversight policies, which catches the general injection class but leaves the accessibility-specific and timing-based attack vectors unaddressed.

---

### RIDC_2 - Messaging and Protocol Injection
- Strength score: 2

NIST AI 600-1 §2.9 (Information Security) identifies both direct and indirect prompt injection as named risks, and MEASURE 2.7 explicitly requires red-teaming for prompt injection. At the conceptual level, all three RIDC_2 threats are forms of injection: FIPA-ACL performative spoofing (RIDC_2_1) uses crafted messages to bypass authorization in a manner analogous to indirect prompt injection; protocol downgrade attacks (RIDC_2_2) create injection opportunities through parsing weaknesses; and conversation-ID chain hijacking (RIDC_2_3) injects malicious context into what appears to be legitimate inter-agent communication. §2.9's data poisoning coverage also applies partially to RIDC_2_3's context pollution mechanism. The Value Chain risk category (§2.12) and GOVERN 6.1/6.2 subcategories contribute governance-level pressure to vet third-party messaging infrastructure and communication libraries. However, NIST AI 600-1 does not address the specific mechanisms that make RIDC_2 threats distinctive: FIPA-ACL performative semantics, Protocol Buffers versus JSON serialization trust hierarchies, and conversation-ID integrity verification are protocol engineering concerns that require formal protocol specifications, message authentication codes, or transport-layer security controls. NIST's red-teaming mandate (MEASURE 2.7) could in principle surface these vulnerabilities during pre-deployment evaluation, but the framework provides no actionable guidance on how to test or mitigate protocol-layer injection. The coverage is moderate — the injection concept is named, but the protocol-specific attack surface is beyond governance-level framing.

---

### RIDC_3 - Framework and State Management Injection
- Strength score: 1

NIST AI 600-1 explicitly does not cover framework-specific vulnerabilities, and RIDC_3's threats are defined entirely by the internal mechanics of named frameworks: LangGraph state schema coercion (RIDC_3_1), cross-framework state migration between LangGraph, CrewAI, and AutoGen (RIDC_3_2), reducer confusion in LangGraph (RIDC_3_3), and conditional edge routing manipulation via state manipulation (RIDC_3_4). These threats require understanding of how each framework parses, validates, and propagates state objects — knowledge that is architectural and implementation-specific rather than governance-level. NIST AI 600-1 §2.9 names prompt injection as a risk category, but RIDC_3's mechanisms are not primarily injection through model input channels; they are exploitation of framework-enforced state transitions and schema validation behaviors. The closest applicable NIST coverage is the Value Chain risk category (§2.12) and GOVERN 6.1/6.2, which direct organizations to vet third-party components and maintain supply chain accountability — this could motivate scrutiny of framework dependencies. MEASURE 2.7 red-teaming could theoretically surface state-manipulation vulnerabilities, but the framework provides no guidance on framework-specific testing or state schema hardening. The gap is fundamental: mitigating RIDC_3 requires framework-level security engineering (schema validation enforcement, reducer sanitization, state migration integrity checks) that NIST AI 600-1 neither specifies nor approximates.

---

### RIDC_4 - Tool, Function Calling, and Plugin Injection
- Strength score: 2

NIST AI 600-1 §2.9 (Information Security) directly names indirect prompt injection as a risk, and the framework's explicit examples include tool-mediated and RAG-mediated injection pathways — making §2.9 directly relevant to several RIDC_4 threats. MEASURE 2.7's red-teaming mandate specifically targets prompt injection and would reasonably encompass tool-calling injection (RIDC_4_1, RIDC_4_2), RAG-retrieved tool metadata poisoning (RIDC_4_3), and tool description injection (RIDC_4_6) as testable attack surfaces in pre-deployment evaluation. The Data Privacy risk category (§2.4) provides peripheral coverage for scenarios where tool outputs exfiltrate PII. The Value Chain risk category (§2.12) and GOVERN 6.1/6.2 apply to shared plugin registries and third-party tool ecosystems, directing governance-level vetting of external tool sources — partially addressing RIDC_4_3's shared registry poisoning scenario. However, NIST's coverage has clear limits: function calling parameter type coercion (RIDC_4_4), implicit tool calling assumption exploitation (RIDC_4_5), Semantic Kernel-specific function template injection (RIDC_4_7), kernel dependency injection compromise (RIDC_4_8), plugin discovery protocol spoofing (RIDC_4_9), and tool schema boundary misinterpretation (RIDC_4_10) involve framework-specific behaviors, type system mechanics, and runtime execution semantics that governance-level policy cannot address. NIST directs organizations to test for injection and vet supply chains, but provides no guidance on schema validation, type coercion hardening, or plugin discovery security — the technical controls that RIDC_4's more specific threats demand.

---

### RIDC_5 - ReAct and Reasoning Architecture Injection
- Strength score: 2

NIST AI 600-1 §2.9 (Information Security) names indirect prompt injection as a risk, and MEASURE 2.7 mandates red-teaming for prompt injection — coverage that applies to the core injection mechanism present across most RIDC_5 threats. ReAct reasoning trace injection (RIDC_5_1), trace artifact post-hoc rationalization (RIDC_5_2), plan-and-execute planning phase conflation (RIDC_5_4), precondition smuggling (RIDC_5_5), abstract task description injection (RIDC_5_6), constraint specification manipulation (RIDC_5_7), effects specification injection (RIDC_5_8), data dependency injection (RIDC_5_9), and method proliferation encoding (RIDC_5_10) all involve adversarial content embedded in data structures that an LLM subsequently processes as instructions — the canonical indirect prompt injection pattern §2.9 addresses. The Confabulation risk category (§2.2) provides marginal coverage for RIDC_5_2, where unfaithful reasoning traces are consumed as ground truth, by directing organizations to measure and disclose model confabulation rates. RIDC_5_11's reflection self-critique reinforcement hijacking has a weak connection to the Human-AI Configuration risk category (§2.7) in that iterative agent cycles without human oversight create automation bias conditions NIST flags. However, the specific architectural mechanisms that make RIDC_5 threats distinctive — ReAct observation-thought-action cycles, HTN precondition/effect schema structures, planning-execution agent boundary semantics, mechanistic interpretability opacity (RIDC_5_3) — are beyond what NIST's governance framing reaches. MEASURE 2.7 red-teaming could surface some of these vulnerabilities in pre-deployment testing, but NIST provides no guidance on reasoning trace integrity, planning-phase isolation, or cross-agent confabulation propagation. The framework names the injection class; the architecture-specific exploitation pathways are out of scope.

---

### RIDC_6 - Streaming and Real-Time Communication Injection
- Strength score: 2

NIST AI 600-1 §2.9 (Information Security) names indirect prompt injection as a named risk and MEASURE 2.7 mandates red-teaming for prompt injection, providing conceptual coverage for RIDC_6_1's core mechanism: malicious instructions embedded in streaming response content that an agent processes. The threat involves adversarially crafted content within a data stream that crosses an agent boundary and hijacks subsequent reasoning — a form of indirect prompt injection §2.9 directly addresses. At the governance level, NIST's direction to conduct pre-deployment red-teaming could motivate testing of streaming response handling as an injection surface. However, the specific attack mechanisms that define RIDC_6_1 — exploitation of human attentional bias toward stream beginnings and endings, timing-based instruction delivery in mid-stream sections, and multi-agent streaming handoff windows as sequential injection points — are implementation-layer concerns tied to streaming protocol design, token-by-token delivery semantics, and real-time monitoring capabilities. NIST AI 600-1 does not address streaming architectures, real-time behavioral monitoring, or the temporal dynamics of token delivery. The framework's pre-deployment red-teaming mandate does not extend to runtime streaming attacks, and there is no NIST guidance on streaming response integrity verification or handoff security. Coverage is moderate for the injection concept and absent for the timing-specific and streaming-architecture-specific dimensions.

---

### RIDC_7 - Generation Parameter and Inference Configuration Injection
- Strength score: 2

NIST AI 600-1 §2.9 (Information Security) identifies data poisoning as a named risk alongside prompt injection, and MEASURE 2.7 mandates security evaluation including red-teaming — providing the most directly applicable coverage for RIDC_7's evaluation poisoning threats. Evaluation dataset poisoning (RIDC_7_1), ground truth injection (RIDC_7_10), and baseline measurement manipulation (RIDC_7_3, RIDC_7_9) are all forms of data poisoning targeting the evaluation and training pipeline, a class §2.9 explicitly addresses. MEASURE 2.6 (safety risk evaluation) and MEASURE 2.9 (model explanation and validation) also apply: organizations directed to evaluate model safety and validate model behavior have governance-level incentive to protect evaluation integrity. The Value Chain risk category (§2.12) and GOVERN 6.1/6.2 address third-party component vetting, which applies to shared evaluation datasets, benchmark sources, and external metric libraries — the supply chain vectors exploited in RIDC_7_3, RIDC_7_7, and RIDC_7_10. However, NIST's coverage has clear limits for this threat category: evaluation metric selection manipulation (RIDC_7_2), multi-agent evaluation orchestration hijacking (RIDC_7_4), non-deterministic evaluation sequencing attacks (RIDC_7_5), evaluation context memory poisoning (RIDC_7_6), confidence score inflation (RIDC_7_8), and the multi-agent-specific cascading and orchestration mechanisms throughout RIDC_7 involve inference configuration, metric implementation internals, and orchestration control flow that governance policy cannot prescribe. NIST directs organizations to evaluate and protect model behavior; it does not specify how to harden evaluation pipelines, isolate agent state during testing, or detect metric calculation tampering. The data poisoning and supply chain coverage is genuine; the evaluation-infrastructure and orchestration-specific attack surfaces are beyond NIST's scope.

---


## RMP - Long-lived cognitive state abuse: memory poisoning and latent backdoors

### RMP_1 - UI/UX Memory Manipulation Attacks
- Strength score: 1

NIST AI 600-1 offers only thin, peripheral governance framing for RMP_1. The Human-AI Configuration risk category (§2.7) names automation bias and over-reliance as concerns and GOVERN 3.2 calls for organizational policies defining human-AI interaction roles, which could in principle require that interfaces distinguish source provenance of displayed content. The Information Security category (§2.9) explicitly covers data poisoning, which is the underlying threat mechanism when injected instructions enter and persist in agent memory. However, these policy instruments do not reach the specific failure modes across RMP_1. NIST prescribes no interface design standards for source attribution in conversation history displays (RMP_1_2), no controls over session persistence across security boundaries (RMP_1_4, RMP_1_6), no requirements for visual isolation between agents sharing backend state (RMP_1_9), and no guidance on sanitizing inline suggestions before they enter document corpora that RAG systems retrieve (RMP_1_10). The gradual, multi-session poisoning pattern described across RMP_1_1 through RMP_1_11 — where temporal separation between injection and exploitation defeats monitoring — is a runtime, architectural concern that governance checklists and pre-deployment red-teaming cannot prevent once systems are in operation. Context window eviction inconsistencies (RMP_1_11) and command palette backdoor triggers derived from accumulated history (RMP_1_8) are application-layer implementation details entirely outside the profile's scope. The profile's incident disclosure provisions (MANAGE 4.1–4.3) are downstream of these exposures, not preventive. NIST AI 600-1 was designed as a governance and risk management profile for AI system behavior, not as a UI/UX security specification, and the full breadth of RMP_1 sits effectively outside its scope.

---

### RMP_2 - Multi-Agent State Handoff and Cross-Agent Memory Attacks
- Strength score: 2

NIST AI 600-1 has moderate relevance to RMP_2 through two overlapping risk categories. The Information Security category (§2.9) explicitly names data poisoning and indirect prompt injection as recognized risks, which maps to the core mechanism across RMP_2: malicious content injected into shared state structures (vector databases, knowledge graphs, serialized handoff payloads) that is later retrieved and acted upon by downstream agents. MEASURE 2.7 mandates security evaluation including red-teaming for data poisoning and prompt injection before deployment, providing a testing hook applicable to shared vector embedding stores (RMP_2_7, RMP_2_8), knowledge graph relationship injection (RMP_2_9), and memory type misclassification during state handoff (RMP_2_6). Value Chain and Component Integration (§2.12), with GOVERN 6.1 and 6.2, covers shared vector databases and knowledge graphs as external infrastructure components subject to supply chain vetting, which partially addresses poisoning risks in those shared stores. The framework's limitations are significant for the multi-agent-specific mechanics: gRPC schema versioning attacks carrying hidden fields that execute only at attacker-controlled deserializers (RMP_2_2), adversarial importance weighting that ensures malicious memories persist while security constraints decay (RMP_2_5), and distributed knowledge graph traversal race conditions from concurrent asynchronous updates (RMP_2_10) are implementation-level, runtime concerns that governance documentation and pre-deployment testing do not resolve. Swarm intelligence local rule injection corrupting emergent behavior without commanding individual agents (RMP_2_3) is an algorithmic architecture problem outside the profile's scope entirely. NIST AI 600-1 provides governance framing for the poisoning threat class and supply chain accountability obligations, but does not define technical controls for cross-agent state validation, inter-agent trust boundaries, serialization integrity checks, or real-time monitoring of memory retrieval pipelines.

---

### RMP_3 - Multimodal and Cross-Modal Poisoning Attacks
- Strength score: 2

NIST AI 600-1 has moderate relevance to RMP_3 primarily through the Information Security category (§2.9), which explicitly recognizes data poisoning as a named risk. This provides genuine governance coverage for the poisoning threat class underlying RMP_3: malicious content embedded in non-text modalities or RAG chunks that propagates through multi-agent pipelines and persists in long-term memory (RMP_3_4, RMP_3_5, RMP_3_6). Pre-deployment testing guidance supports adversarial evaluation of multimodal input pipelines, and MEASURE 2.7's red-teaming mandate is relevant to prompt injection through non-text channels in principle, since cross-modal instruction injection (RMP_3_1) is structurally a form of indirect prompt injection through external data sources. Value Chain and Component Integration (§2.12) applies when poisoned content originates from third-party data sources — multimodal RAG architectures where external image, audio, or document corpora are ingested as third-party components. However, the framework's coverage does not extend to the specific technical mechanisms that make RMP_3 distinctive. NIST AI 600-1 addresses neither modality-specific attack surfaces nor the multi-agent propagation dynamics that amplify them: temporal sensor desync exploitation across agent sensor streams (RMP_3_2), multi-agent modality contradiction attacks exploiting consensus mechanisms (RMP_3_3), Whisper hallucination persistence integration into agent memory (RMP_3_7), DePlot chart linearization injection materializing only during synthesis (RMP_3_8), HTML entity encoding confusion exploiting partial versus full decoders across agents (RMP_3_9), and CSS pseudo-element dual-channel content injection (RMP_3_10) are technical implementation details the governance profile cannot reach. Real-time monitoring of multimodal processing pipelines and cross-modal validation boundaries are also outside scope. The score reflects genuine §2.9 poisoning and §2.12 supply chain relevance without meaningful coverage of the modality-specific and multi-agent-propagation mechanics.

---

### RMP_4 - Framework-Specific Memory Vulnerabilities
- Strength score: 1

NIST AI 600-1 offers only peripheral governance framing for RMP_4. The profile explicitly does not cover framework-specific vulnerabilities, and RMP_4 is defined precisely by internals of LangGraph, AutoGen, CrewAI, and Semantic Kernel: LangGraph append-only reducer state accumulation (RMP_4_4), ConversationBufferMemory latent backdoors (RMP_4_5), AutoGen GroupChat shared conversation history poisoning (RMP_4_9), CrewAI hierarchical task context inheritance chains (RMP_4_10, RMP_4_11), Semantic Kernel execution context persistence across plugin invocations (RMP_4_12, RMP_4_13), and semantic function prompt cache poisoning (RMP_4_14). None of NIST AI 600-1's 12 risk categories address orchestration framework internals, checkpoint serialization formats, memory key namespace design, or plugin execution context lifecycle. The Information Security category (§2.9) acknowledges data poisoning as a generic risk class, which provides the weakest applicable framing: tool output integration into scratchpads without sanitization (RMP_4_8), tool availability state corruption (RMP_4_16), and tool invocation frequency pattern poisoning (RMP_4_17) are instances of data poisoning, and the pre-deployment red-teaming mandate under MEASURE 2.7 could in principle exercise these vectors if testers know to target framework-specific memory pathways. Value Chain and Component Integration (§2.12), with GOVERN 6.1 and 6.2, could treat LangGraph, AutoGen, CrewAI, and Semantic Kernel as third-party components subject to supply chain vetting, providing an indirect organizational accountability lever. However, third-party vetting at procurement time does not address runtime exploitation of memory persistence, namespace collision (RMP_4_7), checkpoint backdoor installation (RMP_4_3), or service registration state hijacking (RMP_4_13) in already-deployed frameworks. The temporal separation between injection and exploitation that characterizes most RMP_4 threats — where a poisoned checkpoint or memory entry activates long after deployment — defeats the pre-deployment testing paradigm the framework relies on. NIST AI 600-1's contribution to RMP_4 is limited to the generic poisoning risk acknowledgment and the supply chain accountability obligation; the framework-specific exploitation mechanics are effectively out of scope.

---
### RMP_5 - Caching and Persistence Attacks
- Strength score: 2

NIST AI 600-1's Information Security risk category (§2.9) explicitly names data poisoning as a recognized threat, which provides genuine but indirect coverage for RMP_5. Embedding cache poisoning (RMP_5_1) and shared fallback cache corruption (RMP_5_3) are structurally analogous to training and inference data poisoning, and the framework's pre-deployment testing mandate — including adversarial red-teaming — can in principle be applied to shared cache integrity before system deployment. The Value Chain and Component Integration category (§2.12), together with GOVERN 6.1 and 6.2, establishes supply chain accountability obligations that can extend to shared caching infrastructure when treated as a third-party or shared component dependency. However, the six sub-threats in RMP_5 are predominantly runtime and architectural concerns: error recovery logic persisting poisoned context across retries (RMP_5_2), injected "recommended action" instructions surviving in logs as authoritative diagnostics (RMP_5_4), retry-pattern history poisoning (RMP_5_5), and persistent degradation state permanently disabling security capabilities (RMP_5_6) are all implementation-specific memory and state management vulnerabilities. NIST AI 600-1 provides no technical controls for cache invalidation policies, memory namespace isolation between agents, integrity verification of persisted state, or detection of adversarially crafted retry sequences. The governance actions it prescribes — organizational policy, pre-deployment evaluation, supply chain vetting — are relevant starting points but do not reach the operational layer where these attacks execute.

---

### RMP_6 - Streaming and Real-time Processing Attacks
- Strength score: 1

NIST AI 600-1's Information Security category (§2.9) conceptually encompasses prompt injection, which overlaps with RMP_6's core mechanism of injecting malicious instructions into content that agents process and store. This provides a thin thread of relevance: pre-deployment red-teaming could theoretically probe streaming pipelines for injection susceptibility, and the framework's acknowledgment of indirect prompt injection (§2.9) nominally covers the scenario where malicious instructions are embedded in streamed external content. However, the four sub-threats in RMP_6 are fundamentally streaming infrastructure and real-time synchronization problems that fall well outside the profile's scope. Injection windows created by token-level memory writes before validation completes (RMP_6_1), persistence instructions embedded in later-streamed content executing before full validation in multi-agent pipelines (RMP_6_2), timing attacks synchronized to distributed memory synchronization windows across agents (RMP_6_3), and temporal buffering vulnerabilities at inter-agent handoff boundaries (RMP_6_4) all require runtime monitoring, streaming pipeline architecture controls, and synchronized validation gating — none of which appear in NIST AI 600-1. The profile does not address streaming data flows, token-level processing order, inter-agent buffer management, or timing-based attack surfaces. Its governance functions (GOVERN, MAP, MEASURE, MANAGE) operate at an organizational policy and pre-deployment testing level that cannot influence the sub-second synchronization windows where these attacks occur. The practical coverage is near-zero for the specific threat mechanisms described.

---

### RMP_7 - Evaluation and Metrics Poisoning
- Strength score: 2

NIST AI 600-1 provides moderate governance-level coverage for RMP_7 through two overlapping provisions. The Information Security risk category (§2.9) explicitly recognizes data poisoning, which applies in principle to shared baseline storage poisoning (RMP_7_1), adaptive metric manipulation embedding false priors (RMP_7_2), evaluation result cache corruption (RMP_7_3), and poisoned performance history repositories (RMP_7_8) — all of which involve corrupting the data that evaluation systems consume. MEASURE 2.6 calls for safety risk evaluation of AI systems, and MEASURE 2.7 calls for security evaluation before deployment, both of which can frame evaluation infrastructure itself as a subject of pre-deployment testing. Supply chain accountability under GOVERN 6.1 and 6.2 can extend to shared evaluation pipelines and benchmark repositories treated as third-party components. That said, the specific attack surfaces in RMP_7 are implementation details of evaluation architecture that governance documentation does not reach: shared scratchpad injection of fabricated test histories (RMP_7_4), progressive corruption through normal pipeline operation (RMP_7_5), poisoning across agent resets through persistent session state (RMP_7_6), mutable aggregation state manipulation (RMP_7_7), overfitting pattern propagation through knowledge sharing (RMP_7_9), and confidence miscalibration cascades in multi-agent pipelines (RMP_7_10). NIST provides no controls for write-access isolation to shared evaluation stores, cryptographic signing of benchmark results, or real-time detection of metric drift caused by adversarial manipulation. The framework's contribution is framing data poisoning as a category of concern and requiring pre-deployment evaluation, but translating that into controls for multi-agent evaluation infrastructure integrity is left entirely to implementing organizations.

---

### RMP_8 - Parameter Tuning and Configuration Poisoning
- Strength score: 2

NIST AI 600-1 addresses the most structurally significant sub-threat in RMP_8 through its Information Security category (§2.9), which explicitly names data poisoning as a recognized risk relevant to training and tuning pipelines. RMP_8_4 — shared tuning dataset poisoning causing all agents to simultaneously memorize adversarial patterns and fleet-wide activation of trigger-based backdoors — maps directly onto this coverage, and MEASURE 2.7's security evaluation mandate supports pre-deployment testing of fine-tuned models for poisoning susceptibility. Value Chain and Component Integration (§2.12), along with GOVERN 6.1 and 6.2, applies to shared tuning datasets and shared configuration backends (RMP_8_5) when these are treated as supply chain components requiring vetting and integrity assurance. Pre-deployment testing can in principle probe whether shared hyperparameter configurations contain adversarially crafted values. However, the remaining sub-threats are implementation-specific concerns that the profile does not reach: vision metadata injection via EXIF or steganographic channels in multi-agent visual pipelines (RMP_8_1), semantic state corruption at cross-model translation boundaries (RMP_8_2), truncation-regime diversity attacks exploiting heterogeneous context windows (RMP_8_3), and Pareto frontier manipulation through poisoned baseline metrics (RMP_8_6) are all runtime or architectural attack surfaces requiring technical mitigations — input sanitization for multimodal metadata, isolated configuration namespaces per agent, cross-model state validation, and integrity-protected metric storage — that governance policies and pre-deployment checklists cannot substitute for. The framework establishes that tuning data and configuration supply chains must be managed, but does not extend to the technical mechanisms through which adversaries exploit the specific tuning and configuration attack surfaces described.

---
### RMP_9 - Learning and Training Data Attacks
- Strength score: 2

NIST AI 600-1's Information Security risk category (§2.9) explicitly names data poisoning as a recognized threat, which provides genuine governance coverage for the core mechanism across all three RMP_9 sub-threats: adversarially constructed inputs that corrupt model behavior through the training or few-shot learning pathway. For RMP_9_1, the poisoning of chain-of-thought demonstrations in Plan-and-Execute architectures is structurally identical to training data poisoning — malicious content is introduced into the learning signal that shapes downstream model behavior — and MEASURE 2.7's mandate for security evaluation including red-teaming directly supports pre-deployment testing of few-shot example pipelines for injected demonstrations. For RMP_9_3, fine-tuning data contamination through manipulated example-selection heuristics is a data poisoning variant that MEASURE 2.7 and the profile's pre-deployment testing guidance can exercise if test suites are designed to probe curator logic. Value Chain and Component Integration (§2.12), with GOVERN 6.1 and 6.2, adds organizational accountability obligations for third-party training corpora and any external data pipelines feeding the selection or fine-tuning process. The profile's gaps are significant for the multi-agent propagation dynamics. RMP_9_2's trajectory data injection through reinforcement learning flywheels — where Agent A's operational trajectories become Agent B's few-shot examples and corrupted behaviors propagate and self-reinforce across agent chains — involves a feedback mechanism that the profile's pre-deployment testing model does not address once systems are in operation. The iterative, cross-agent reinforcement dynamic means that injected behavior can emerge progressively during deployment, after the governance checkpoint the profile relies on has already passed. Compound learning-stage attacks (RMP_9_3) that exploit heuristic selection logic in live curation agents face the same temporal gap: the attack manifests across deployment stages that static red-teaming cannot fully anticipate. NIST AI 600-1 provides genuine risk-category recognition and testing mandates for the data poisoning threat class, but does not define controls for agent-to-agent trajectory propagation, online learning pipelines, or runtime monitoring of how few-shot example sets are constructed and consumed.

---

### RMP_10 - Observability and Tracing Attacks
- Strength score: 1

NIST AI 600-1 has no meaningful coverage of RMP_10. The four sub-threats — clock skew exploitation to falsify trace causality (RMP_10_1), span metadata poisoning to suppress evidence from oversight views (RMP_10_2), architecturally maximized symptom-cause separation to defeat root cause analysis (RMP_10_3), and fabricated high-confidence reasoning injection through trace manipulation (RMP_10_4) — are attacks against distributed tracing infrastructure, span metadata integrity, and inter-agent observability pipelines. These are real-time infrastructure security problems requiring cryptographic timestamp attestation, immutable append-only trace stores, anomaly detection on span metadata, and cross-agent causality verification mechanisms. None of NIST AI 600-1's 12 risk categories address distributed tracing systems, OpenTelemetry or similar instrumentation infrastructure, or the integrity of observability data that security operations teams rely on. The Information Security category (§2.9) covers prompt injection and data poisoning as model-level threats, not as attacks on the monitoring layer that observes model behavior. MANAGE 4.1–4.3 addresses incident response and disclosure, but those processes depend on reliable observability data — they are defeated, not aided, when trace infrastructure has been compromised as RMP_10 describes. The Human-AI Configuration category's concern with automation bias has no bearing on whether distributed trace timestamps can be cryptographically verified. NIST AI 600-1 was not designed as an infrastructure security specification, and the full scope of RMP_10 sits outside the profile's governance reach.

---

### RMP_11 - Learned Behavior and Memory Evolution
- Strength score: 2

NIST AI 600-1 has moderate relevance to RMP_11 through two overlapping risk categories. The Confabulation risk category (§2.2) directly targets hallucination and fabricated outputs, which maps to the core failure mechanism in RMP_11_1: an agent produces hallucinated tool parameters, those parameters are stored in shared multi-agent memory as apparent historical evidence, and other agents subsequently retrieve and act on them believing they are grounded in prior successful execution. MEASURE 2.9 calls for evaluation of model explanation and validation methods, supporting pre-deployment assessment of whether retrieved memory entries are distinguishable from confabulated ones. The Information Security category (§2.9) adds data poisoning framing for RMP_11_2: memory consolidation and deduplication processes that can be exploited to inject malicious patterns disguised as learned behaviors are structurally a form of data poisoning applied to the memory store rather than the training set, and MEASURE 2.7's red-teaming mandate is relevant to evaluating consolidation pipeline vulnerabilities before deployment. Value Chain and Component Integration (§2.12) provides a further organizational lever when the shared memory store is a third-party or externally managed component subject to supply chain vetting under GOVERN 6.1 and 6.2. The profile's limitations are significant for the specific exploitation mechanics: the memory consolidation and deduplication algorithms that RMP_11_2 targets are implementation-level decisions about how multi-agent systems merge, weight, and deduplicate memory entries across agents — the profile prescribes no controls over consolidation logic, memory write provenance tracking, or runtime isolation between agents' memory namespaces. For RMP_11_1, NIST can establish a policy requirement that hallucination risks be evaluated, but cannot prevent a deployed system from storing and propagating hallucinated content through shared memory once the governance checkpoint has passed. The score reflects genuine §2.2 confabulation and §2.9 poisoning relevance without coverage of memory architecture or consolidation exploit mechanics.

---

### RMP_12 - Context and Parameter Consistency
- Strength score: 1

NIST AI 600-1 offers only thin, peripheral governance framing for RMP_12. The Information Security category (§2.9) covers data poisoning at the model and training input level, which provides the weakest applicable framing for cross-session parameter poisoning (RMP_12_2) and poisoned parameter history in shared conversation memory (RMP_12_3) — these can be characterized as a form of runtime data poisoning where malicious values are injected into the shared state that agents treat as ground truth. MEASURE 2.7's red-teaming mandate could in principle include adversarial parameter injection tests against multi-turn agent pipelines. The Value Chain and Component Integration category (§2.12) provides an indirect lever if shared memory infrastructure is a third-party component subject to procurement-time vetting under GOVERN 6.1 and 6.2. However, the specific mechanisms across RMP_12 are architectural and runtime concerns that NIST AI 600-1 does not address at any meaningful level. Selective history retrieval enabling targeted per-agent poisoning (RMP_12_1) involves the design of memory query routing and retrieval logic in multi-agent systems. Parameter accuracy regression cascading across agents in multi-turn sessions (RMP_12_4) is a runtime coherence problem requiring instrumentation that scales with agent graph complexity. Tool selection consistency loss across agent boundaries (RMP_12_5) — where agents selecting semantically different tools for equivalent operations create downstream parameter mismatches — is an architectural consistency problem with no NIST analog: the profile neither defines tool selection semantics nor mandates inter-agent consistency checks. Parameter validation state information leakage through shared conversation memory (RMP_12_6) — where downstream agents cannot distinguish validated from unvalidated parameters — is a provenance tracking problem specific to agent memory architectures that the governance profile does not reach. The profile's incident disclosure provisions (MANAGE 4.1–4.3) are downstream of these failures and do not prevent them. NIST AI 600-1 addresses the threat class at the level of general risk acknowledgment for poisoning, but the multi-agent parameter consistency, tool selection integrity, and validation provenance mechanisms that define RMP_12 are implementation-layer concerns outside the profile's intended scope.

---
### RMP_13 - Reasoning Transparency and Trust
- Strength score: 2

NIST AI 600-1 has moderate relevance to RMP_13 through two named risk categories that address the governance conditions enabling these attacks. The Confabulation risk category (§2.2) directly recognizes that AI-generated outputs can appear credible while being factually incorrect or misleading, and calls for measurement of model confidence calibration relative to actual accuracy — this provides genuine framing for RMP_13_3, where a compromised agent expresses inappropriate confidence in injected instructions, propagating false confidence metrics to downstream agents and users. MEASURE 2.9 supports model explanation and output validation as part of pre-deployment evaluation, and MEASURE 2.6 calls for safety risk evaluation that can in principle include adversarial testing of whether explicit reasoning traces can be weaponized to produce scientifically plausible but dangerous conclusions (RMP_13_1). The Human-AI Configuration category (§2.7) explicitly names automation bias and over-reliance as recognized harms, which maps to the trust override mechanism in RMP_13_1 where user trust in a transparent agent's reasoning chain overrides downstream safety mechanisms in healthcare and decision systems, and to RMP_13_2 where engineered multi-agent disagreement is exploited to steer human trust toward malicious recommendations. However, the framework's coverage does not extend to the multi-agent-specific exploitation mechanics that make RMP_13_2 and RMP_13_3 distinctively dangerous at scale. Engineering disagreement by injecting different payloads to different agents and then manipulating disagreement presentation (RMP_13_2) requires inter-agent trust boundary controls and disagreement arbitration validation that no NIST AI 600-1 category prescribes. Confidence metric propagation through multi-agent inference chains (RMP_13_3) requires runtime calibration monitoring across agents, which is an architectural concern the governance profile cannot specify. The profile's pre-deployment red-teaming mandate under MEASURE 2.7 could exercise reasoning trace weaponization for individual agents, but multi-agent disagreement exploitation is a systemic, emergent property of multi-agent orchestration rather than a property of any single agent testable in isolation. NIST AI 600-1 provides genuine governance framing through §2.2 and §2.7, but the multi-agent propagation and trust manipulation mechanics of RMP_13 require technical controls beyond what governance checklists can deliver.

---

### RMP_14 - Efficiency and Resource Tracking
- Strength score: 1

NIST AI 600-1 offers only peripheral governance framing for RMP_14. The Information Security category (§2.9) names data poisoning as a recognized risk, which provides the thinnest applicable thread: RMP_14_1 (poisoned historical efficiency data in shared memory), RMP_14_4 (cost attribution record corruption), and RMP_14_5 (efficiency insights stored as learned policies) each involve persistent data being corrupted to influence future agent behavior, making them structurally analogous to training or inference data poisoning. MEASURE 2.7's red-teaming mandate could in principle be interpreted to include adversarial probing of shared memory structures. However, efficiency and resource tracking infrastructure — shared efficiency memory, conversation history summarization, cost attribution databases, learned optimization policies, and shared baselines for percentage-improvement targeting — is fundamentally operational resource management architecture that falls entirely outside the scope NIST AI 600-1 was designed to address. The profile contains no risk category for resource allocation, no subcategory for shared operational memory integrity, and no governance guidance for baseline metric stores, history summarization compression algorithms, or learned policy repositories. RMP_14_2 — where injected instructions are crafted to survive history summarization compression while benign context is pruned — and RMP_14_3 — where gradually degrading shared baselines force agents into resource-constrained paths activating hidden payloads — require controls at the algorithm and architecture level: compression-resistant injection detection, tamper-evident baseline records, and isolated per-agent metric stores. RMP_14_5's scenario, where one agent's poisoned efficiency learning propagates across all agents through shared learned policies, is an emergent multi-agent coordination vulnerability with no counterpart in the profile's governance functions. The profile's incident response provisions (MANAGE 4.1–4.3) are reactive and downstream; the pre-deployment testing paradigm (MEASURE 2.7) cannot surface injection patterns that exploit summarization compression or gradual baseline drift that only manifests after extended operation. NIST AI 600-1's contribution to RMP_14 is limited to the generic data poisoning risk acknowledgment under §2.9, and the full breadth of efficiency and resource tracking attack vectors sits effectively outside its scope.

---

### RMP_15 - Vector Database and Embedding Poisoning
- Strength score: 2

NIST AI 600-1 has moderate relevance to RMP_15 through two overlapping provisions that genuinely apply to the embedding poisoning threat class. The Information Security category (§2.9) explicitly recognizes data poisoning as a named risk, which provides direct governance framing for embedding space poisoning (RMP_15_1) and cached embedding corruption (RMP_15_2) — both involve adversarially crafted vectors entering a shared data store and subsequently influencing all retrieval queries across agents, which is structurally a data poisoning attack on shared inference infrastructure. MEASURE 2.7's mandate for security evaluation including red-teaming for data poisoning applies in principle to pre-deployment testing of shared vector stores for adversarial embedding injection. Value Chain and Component Integration (§2.12), together with GOVERN 6.1 and 6.2, provides the most relevant governance hook: shared vector databases and embedding providers are third-party or shared infrastructure components subject to supply chain vetting, accountability obligations, and integrity assurance requirements. This supply chain framing genuinely applies to RMP_15_1, where cross-provider embedding model switching creates incompatible semantic spaces exploitable by adversarially crafted documents, and to RMP_15_5, where cluster replication protocols propagate poisoned vectors to all replicas because format and checksum verification does not validate semantic correctness. GOVERN 6.2's third-party accountability requirements can mandate that embedding providers and vector database vendors attest to the integrity of their models and replication mechanisms. However, the framework does not reach the infrastructure-level specifics that define RMP_15_3 and RMP_15_4. HNSW graph structure poisoning through efConstruction parameter manipulation during index rebuild (RMP_15_3) — where reducing the construction-time parameter produces a poorly-connected graph that passes all integrity checks but permanently degrades nearest-neighbor retrieval quality — is a vector index algorithm implementation detail that governance documentation, organizational policy, and pre-deployment testing paradigms cannot prevent or detect without specific instrumentation of index build parameters. Persistent volume poisoning through direct modification of stored HNSW graph files, embedding values, or metadata databases (RMP_15_4) is a storage infrastructure access control problem requiring filesystem-level integrity monitoring, volume encryption, and access logging that the AI governance profile cannot specify. NIST AI 600-1 provides genuine §2.9 data poisoning and §2.12 supply chain governance coverage for the embedding poisoning threat class broadly, but the HNSW internals and persistent volume attack surfaces are infrastructure concerns beyond its reach.

---

### RMP_16 - ETL Pipeline and Data Processing Attacks
- Strength score: 2

NIST AI 600-1 provides moderate governance-level coverage for RMP_16 through two provisions with genuine applicability. The Information Security category (§2.9) explicitly names data poisoning, which applies to the core threat class across RMP_16: ETL pipeline corruption that allows low-quality, adversarially crafted, or duplicated content to enter knowledge bases ultimately used for RAG retrieval is a form of training and inference data poisoning affecting all downstream agent behavior. MEASURE 2.7's security evaluation mandate — including adversarial red-teaming for data poisoning — can be applied to pre-deployment testing of knowledge ingestion pipelines, and MEASURE 2.6 supports safety risk evaluation that frames knowledge base integrity as a prerequisite for safe agent outputs. Value Chain and Component Integration (§2.12), with GOVERN 6.1 and 6.2, provides meaningful coverage when ETL pipelines ingest content from third-party document sources: citation database poisoning through fabricated source attributions (RMP_16_10) — where injected chunks claim authorship from authoritative sources causing RAG systems to cite dangerous recommendations as trusted — is partly addressed by the supply chain accountability obligation to vet and authenticate external data sources. Quality threshold governance (RMP_16_2, RMP_16_6, RMP_16_9) implicates organizational configuration management policies that GOVERN functions can mandate. However, the majority of RMP_16's twelve sub-threats are implementation specifics that NIST AI 600-1's governance layer cannot prevent. ETL state file poisoning through timestamp backdating (RMP_16_1, RMP_16_8) and the duplication or knowledge gap effects that result are filesystem and pipeline scheduling details. Chunking overlap poisoning creating cross-chunk context contamination (RMP_16_3) requires controls at the chunking algorithm level. Semantic cache entry poisoning through cosine similarity threshold exploitation (RMP_16_4) and query normalization collision attacks against cache key generation (RMP_16_5) are retrieval and caching architecture vulnerabilities requiring cryptographic cache key design and similarity threshold integrity monitoring. Deduplication threshold tampering (RMP_16_7) and MinHash hash collision injection amplifying training data duplication (RMP_16_12) are algorithm-level attacks requiring tamper-evident threshold configuration and collision-resistant hashing choices that governance checklists cannot specify. Batch processing queue contamination affecting concurrent multi-agent processors (RMP_16_11) is a runtime job queue isolation problem. The framework establishes that training data integrity and supply chain vetting matter, and pre-deployment testing should include adversarial data scenarios, but translating those governance obligations into technical controls for ETL state files, chunking overlap, deduplication algorithms, and cache key generation is left entirely to implementing organizations.

---

## RND - Non-determinism, continual change, and assurance gaps as a risk surface
# NIST AI 600-1 Analysis: RND Threats (UI/Streaming, Progressive Disclosure, Session, Multi-Agent, Retry, Checkpoint)

---

## RND_1 - UI/Streaming and Real-Time Generation

### RND_1_1
NIST AI 600-1's MEASURE 2.7 calls for pre-deployment security evaluation including red-teaming, and §2.9 acknowledges that information security risks from adversarial inputs must be identified and managed. However, streaming response non-determinism defeating audit reproducibility is a runtime infrastructure problem rooted in stochastic token sampling, floating-point variance, and network buffering—none of which are addressable by organizational governance or pre-deployment test checklists. The profile has no concept of streaming pipelines, inter-agent partial-output propagation before safety filtering completes, or the impossibility of deterministic audit replay under variable sampling conditions. MANAGE 4.1–4.3 covers incident response after detection, but non-reproducible streaming attacks are definitionally difficult to detect and re-investigate through after-the-fact governance processes. The framework provides no applicable control for this threat.
**Score: 1**

### RND_1_2
Streaming latency non-determinism enabling temporal exploitation is a network and infrastructure-layer timing problem that falls entirely outside NIST AI 600-1's governance scope. The profile does not address network conditions, computational load variability, or multi-agent execution ordering as a function of connection timing. §2.9 names prompt injection and data poisoning as security risks, but timing side-channels and sleeper agent behaviors activated only under production-specific latency conditions are not forms of prompt injection and are not acknowledged anywhere in the profile's 12 risk categories. Pre-deployment testing (MEASURE 2.6, 2.7) cannot reproduce the combinatorial explosion of execution orderings across N agents and M connections at production scale. The threat is infrastructure-layer and effectively out of scope.
**Score: 1**

### RND_1_3
Stochastic token sampling creating ephemeral vulnerability windows—where malicious content transiently appears before safety filtering—is a model-level inference behavior that NIST AI 600-1 does not address at the technical level required to mitigate it. MEASURE 2.7's security evaluation and red-teaming mandate could in principle probe sampling parameter space for harmful token sequence probabilities, providing a thin pre-deployment hook. However, the profile has no guidance on temperature, top-k, or top-p configuration, no requirements for post-sampling safety filtering pipelines, and no recognition that transient token sequences represent a distinct attack surface from final outputs. The ephemeral and non-deterministic nature of the vulnerability means governance documentation and pre-deployment tests cannot establish safety guarantees across the full sampling distribution. Coverage is weak.
**Score: 1**

### RND_1_4
Inline code suggestion systems generating asynchronous completions create race-condition windows and monitoring blind spots for unlogged unaccepted suggestions—a combination of UI asynchrony, multi-agent timing, and telemetry gaps that NIST AI 600-1 does not address. The Human-AI Configuration risk category (§2.7) acknowledges over-reliance and the need for human oversight design, which tangentially motivates logging and review requirements, but the profile does not specify logging of unaccepted AI suggestions or mandate observability into asynchronous generation pipelines. §2.9's prompt injection acknowledgment does not extend to race-condition windows where generation agents outpace safety agents. GOVERN 3.2 could require organizational policies mandating audit logging of all AI outputs including ephemeral suggestions, but this is an interpretive stretch and not a stated control. The threat is primarily an infrastructure and telemetry design problem beyond the framework's scope.
**Score: 1**

---

## RND_2 - Progressive Disclosure and Error Handling

### RND_2_1
Progressive disclosure creating exponentially complex state spaces as an attack surface is a UI/UX architectural problem that NIST AI 600-1 does not address. The Human-AI Configuration risk category broadly motivates meaningful human oversight and alerts organizations to automation bias risks, which could inform a policy requiring that multi-agent execution graphs be surfaced at appropriate disclosure levels—but the profile specifies no such requirements. Pre-deployment testing under MEASURE 2.6 and 2.7 could theoretically exercise progressive disclosure state combinations, but the exponential state space makes exhaustive testing infeasible, and the profile provides no guidance on how to bound or prioritize this test space. Attacks that embed malicious instructions in technical disclosure layers while displaying benign content are a form of indirect prompt injection (§2.9), giving a weak but non-zero connection to the profile's named risks. Coverage remains weak because the UI state-management dimension is entirely outside the framework.
**Score: 1**

### RND_2_2
Error message progressive disclosure hiding attack pattern evidence creates forensic blind spots where critical information resides in unexpanded technical layers—a logging and observability problem that NIST AI 600-1 does not address at the technical level. MANAGE 4.1–4.3 covers incident response and calls for organizations to have processes for identifying and addressing AI-related incidents, which implicitly requires adequate evidence capture, but the profile does not prescribe log retention requirements, error expansion policies, or audit trail completeness for multi-agent systems. The deliberate use of collapsed error displays by attackers to hide malicious activity in low-priority background agents is a multi-agent operational security evasion technique with no corresponding control in the profile. The connection to §2.9 information security is conceptually present—it is a security-relevant evidence gap—but NIST offers no actionable control for this specific forensic vulnerability.
**Score: 1**

### RND_2_3
Streaming error handling creating ephemeral state non-determinism is a runtime pipeline behavior where errors appear and disappear as streams progress, and downstream agents may recover or mask upstream errors before they propagate. This is an infrastructure-layer problem combining streaming non-determinism, multi-agent error propagation, and audit replay failure—none of which are within NIST AI 600-1's governance scope. MANAGE 4.1–4.3's incident response provisions assume detectable and reproducible incidents; ephemeral streaming errors that resolve themselves before aggregation are definitionally difficult to surface through incident reporting processes. §2.9's information security coverage does not extend to error state propagation mechanics. The profile provides no applicable control.
**Score: 1**

---

## RND_3 - Context, Persistence, and Session Management

### RND_3_1
Session state non-determinism breaking multi-turn attack detection has partial but limited coverage in NIST AI 600-1. §2.4 (Data Privacy) and §2.9 (Information Security) together create governance-level awareness that multi-turn context carries privacy and security risks, and MEASURE 2.7's red-teaming mandate could in principle include multi-turn adversarial scenarios designed to probe distributed malicious intent across session turns. However, the empirical reality that multi-turn attacks achieve 80–95%+ success against safety-aligned models reflects a fundamental limitation of stateless safety classifiers that governance policies cannot fix. The profile has no guidance on shared memory store isolation between agents, stochastic execution path auditing, or session-scoped threat detection. Pre-deployment testing provides a thin but genuine hook for probing multi-turn attack patterns, justifying a score above minimum, but the runtime non-determinism and multi-agent memory propagation dimensions are beyond the framework's scope.
**Score: 2**

### RND_3_2
Context persistence across UI state changes creating stale security context is a session management and multi-agent synchronization problem that NIST AI 600-1 does not address. The Human-AI Configuration risk category (§2.7) motivates policies ensuring that human operators have accurate, current situational awareness, which loosely motivates requirements that AI systems not act on stale context—but the profile does not specify session expiration, context freshness validation, or cross-agent state consistency requirements. GOVERN 3.2 could support organizational policies mandating context staleness checks, but this is an interpretive extension not stated in the profile. Inconsistent security postures arising from agents accessing shared state at different times with different freshness are a distributed systems consistency problem outside the framework's intended scope.
**Score: 1**

---

## RND_4 - Multi-Agent Coordination and Decision-Making

### RND_4_1
Chat interface streaming attribution confusion in multi-agent attacks—where Agent A streams benign analysis while Agent B streams malicious instructions styled as Agent A's output—is a UI/UX rendering and multi-agent identity problem that NIST AI 600-1 does not address. §2.8 (Information Integrity) covers disinformation and content authenticity at a high level, and content provenance mechanisms (one of the profile's four primary considerations) are intended to help users identify AI-generated content origins, providing a weak conceptual connection. However, the profile's provenance guidance targets watermarking and labeling of AI outputs for general audiences, not real-time attribution enforcement in streaming multi-agent dashboards. GOVERN 3.2's human oversight policies could motivate requiring clear visual agent attribution in multi-agent interfaces, but this is not stated. The attack exploits UI rendering and streaming identity gaps that governance documentation does not close.
**Score: 1**

### RND_4_2
Command palette AI-driven action prediction creating non-deterministic suggestion lists and inconsistent guardrail application is an inference-time architectural problem outside NIST AI 600-1's scope. The Human-AI Configuration risk category acknowledges automation bias and motivates design choices that preserve meaningful human control, which could loosely inform requirements for consistent command suggestion behavior—but NIST does not prescribe determinism requirements for AI-driven interface elements, multi-agent aggregation consistency, or evaluation methodologies for probabilistic command surfaces. MEASURE 2.7's security evaluation mandate is the closest applicable hook, as pre-deployment testing could probe whether dangerous commands surface through command palette prediction, but the non-deterministic nature of the threat means point-in-time testing cannot establish that dangerous commands will never appear. The framework offers minimal relevant coverage.
**Score: 1**

### RND_4_3
Approval workflow confidence score non-determinism defeating threshold-based controls maps partially to NIST AI 600-1 through the Human-AI Configuration risk category, which explicitly addresses automation bias and over-reliance on AI confidence outputs. GOVERN 3.2 and MEASURE 2.9 (model explanation and validation) together motivate organizational requirements to understand and validate AI confidence outputs before using them as automated decision gates—attackers exploiting score variance to repeatedly trigger auto-approval are exploiting exactly the over-reliance failure mode NIST identifies. However, the profile does not provide technical guidance on confidence score calibration, variance bounds, threshold design, or rate-limiting of repeated identical approval requests. Pre-deployment testing could probe score variance across identical inputs, but the profile does not mandate this. The governance framing is genuinely relevant but the technical controls are absent.
**Score: 2**

### RND_4_4
Multi-agent dashboard real-time state updates creating race condition attack windows are an infrastructure-layer synchronization problem that NIST AI 600-1 does not address. Race conditions where UI state temporarily reflects inconsistent agent combinations—and where attackers exploit these windows—are a distributed systems concurrency problem requiring runtime coordination controls, not governance policies. The Human-AI Configuration category motivates accurate human situational awareness, and MANAGE 4.1–4.3 covers incident response after exploitation, but neither provides controls preventing race condition windows from arising or being weaponized. The profile has no guidance on multi-agent state consistency, UI update sequencing, or dashboard integrity monitoring. The threat is effectively out of scope.
**Score: 1**

---

## RND_5 - Retry, Resilience, and Error Recovery

### RND_5_1
Auto-retry error recovery hiding attack attempts in collapsed error logs creates monitoring blind spots where malicious operations execute multiple times—a telemetry and observability gap that NIST AI 600-1 does not address at the technical level. MANAGE 4.1–4.3's incident response provisions implicitly require that incidents be detectable, and MEASURE 2.7's security evaluation could in principle probe whether retry mechanisms surface or conceal malicious payloads. However, the profile has no guidance on log aggregation for retry attempts, requirements for error log expansion in multi-agent systems, or detection of adversarial inputs crafted to appear as transient errors. The attack exploits infrastructure retry behavior and logging configuration—neither of which are within the governance framework's scope. Coverage is minimal.
**Score: 1**

### RND_5_2
Retry timing non-determinism from exponential backoff with jitter defeating audit reproducibility is a distributed systems infrastructure problem with no applicable NIST AI 600-1 coverage. The profile's governance actions (policy, red-teaming, incident disclosure) operate at organizational layers that do not reach runtime retry timing mechanics. MEASURE 2.7's pre-deployment security evaluation cannot reproduce production-specific timing non-determinism arising from multi-agent distributed retry interactions. The profile's 12 risk categories contain no category that maps to timing-based attack evasion through retry variance. The threat is out of scope.
**Score: 1**

### RND_5_3
Non-idempotent operation exploitation via coordinated retry storms—where multiple agents coordinate to trigger duplicate financial transactions or resource allocations—is a multi-agent coordination and financial integrity problem outside NIST AI 600-1's scope. §2.4 (Data Privacy) does not extend to financial transaction integrity. The Value Chain and Component Integration category (§2.12) and GOVERN 6.1/6.2 address supply chain vetting of components but not the runtime behavior of coordinated agent retry patterns. MANAGE 4.1–4.3 could support incident response after duplicate transactions are detected, but provides no preventive controls for idempotency enforcement or coordinated retry detection. The profile has no applicable guidance.
**Score: 1**

### RND_5_4
Fallback route selection non-determinism creating untestable branching—where probabilistic fallback strategies route requests through malicious routes that cannot be reliably prevented through testing—is a runtime inference and routing problem outside NIST AI 600-1's governance scope. MEASURE 2.7's pre-deployment testing mandate is the closest applicable provision, as security evaluation could probe fallback route behavior, but the profile acknowledges that pre-deployment testing cannot address all runtime conditions. The non-deterministic routing creates an inherently untestable guarantee gap that governance checklists cannot close. No other risk category or subcategory applies to probabilistic routing security. Coverage is negligible.
**Score: 1**

### RND_5_5
Circuit breaker opening threshold non-determinism—where identical failure patterns trigger circuit opening at variable times—is a distributed systems reliability and infrastructure configuration problem that NIST AI 600-1 does not address. The profile's governance framework has no concept of circuit breaker patterns, failure threshold configuration, or the security implications of non-deterministic infrastructure state transitions. MANAGE 4.1–4.3 covers incident response after system failures are detected, but does not reach the configuration and timing mechanics of circuit breaker non-determinism. The threat is out of scope for a governance-oriented GenAI risk profile.
**Score: 1**

### RND_5_6
Error recovery continuation conditions creating untestable non-determinism—where probabilistic degradation thresholds and independent agent degradation decisions produce inconsistent multi-agent states—is an infrastructure-layer distributed consistency problem outside NIST AI 600-1's scope. The Human-AI Configuration risk category motivates reliable human oversight mechanisms, which implicitly requires consistent AI system behavior, but the profile does not specify consistency requirements for graceful degradation decisions across agent instances. Pre-deployment testing under MEASURE 2.6/2.7 cannot establish that probabilistic continuation conditions will behave consistently across all production degradation scenarios. No applicable control exists in the framework.
**Score: 1**

---

## RND_6 - Checkpoint, State, and Recovery Management

### RND_6_1
Checkpoint replay poisoning through state manipulation during capture-to-resumption windows—where adversarial agents inject malicious payloads into checkpointed state—is a data integrity and infrastructure security problem that has weak but non-zero connection to NIST AI 600-1. §2.9 (Information Security) explicitly names data poisoning as a recognized risk category, and MEASURE 2.7's security evaluation mandate could in principle include checkpoint integrity testing. GOVERN 6.1/6.2's supply chain vetting provisions could extend to checkpoint storage infrastructure providers. However, the profile addresses data poisoning at the training data and model input level, not at the runtime checkpoint and state-store level where this attack operates. The multi-agent coordination aspect—Agent A creating partial transactions while Agent B corrupts checkpoint stores—is a distributed systems attack pattern with no corresponding governance control. Coverage is weak but poisoning acknowledgment provides a minimal hook.
**Score: 1**

### RND_6_2
Determinism violation amplification via cascading state divergence—where malicious agents exploit non-deterministic replay logic to create divergent state interpretations and submit conflicting transactions—is a multi-agent consistency and distributed systems integrity problem outside NIST AI 600-1's scope. §2.9's data poisoning coverage does not extend to state divergence exploitation during checkpoint replay. The profile has no guidance on replay determinism requirements, state consistency validation across agent instances, or detection of coordinated divergence exploitation. MANAGE 4.1–4.3's incident response could address consequences after conflicting transactions are detected, but provides no preventive architectural controls. The threat requires technical distributed systems mitigations that governance documentation cannot supply.
**Score: 1**

### RND_6_3
Retry logic exhaustion through adversarial error injection—where coordinated agents strategically inject recoverable errors forcing exponential retry backoff to consume resources and mask critical failures—is an economic denial-of-service and resource exhaustion attack that NIST AI 600-1 does not address. The profile has no coverage of denial-of-service, resource exhaustion, economic attacks, or cost attribution. §2.5 (Environmental Impacts) addresses compute resource consumption at a high level but in the context of sustainability, not adversarial resource exhaustion. MANAGE 4.1–4.3 could support response after exhaustion is detected, but the coordinated multi-agent error injection mechanism and the masking of critical failures through retry noise are operational attack patterns outside the governance framework's intended scope. The threat is out of scope.
**Score: 1**
# NIST AI 600-1 Coverage Analysis: RND_7 through RND_12

## RND_7 - Tool Integration and API Management

### RND_7_1
NIST AI 600-1 GOVERN 6.1/6.2 addresses third-party and supply chain risk at a governance level, which provides a weak hook for requiring vendors to communicate API changes, but the framework has no mechanism for detecting or mitigating silent API drift at runtime in multi-agent chains. MEASURE 2.7 red-teaming can test known API versions pre-deployment but cannot account for post-deployment drift where Agent A silently receives updated response formats. The cascading failure propagation to Agent B operating on corrupted data is an operational runtime concern entirely outside NIST's pre-deployment governance scope. NIST's supply chain provisions were designed for model and dataset provenance, not live API behavioral contracts.
**Score: 2**

### RND_7_2
GOVERN 6.1/6.2 supply chain provisions can require documentation of tool schema versions as a third-party dependency management practice, providing a thin governance hook for schema version tracking. However, NIST provides no controls for detecting or reconciling simultaneous use of divergent schema versions across agents in a running system. MEASURE 2.7 security evaluation could test individual agents against specific schema versions pre-deployment but cannot assess the emergent risk of mixed-version deployments. The multi-agent schema version drift problem is fundamentally an infrastructure and runtime configuration concern outside NIST's governance scope.
**Score: 2**

### RND_7_3
NIST AI 600-1 does not address LLM provider-side function calling behavior updates, as the framework focuses on organizational risk management rather than runtime behavioral consistency of third-party model capabilities. MEASURE 2.7 pre-deployment testing could benchmark function calling behavior at a point in time, but provider updates occurring post-deployment invalidate those evaluations with no NIST mechanism for re-triggering assessment. The behavioral discontinuity created when multi-agent systems use different LLM versions is a framework-specific and infrastructure concern that NIST explicitly does not cover. GOVERN 6.2 third-party vetting may require contractual notification of model updates but cannot prevent or detect the resulting behavioral inconsistencies.
**Score: 1**

### RND_7_4
GOVERN 6.1/6.2 supply chain provisions can nominally require third-party tool providers to maintain deprecation notice processes, providing minimal governance coverage for tool availability changes. NIST has no controls addressing the non-deterministic behavior that emerges when agents in a multi-agent system operate from inconsistent tool availability states. MEASURE 2.7 red-teaming tests a static tool configuration at one point in time and cannot simulate the asynchronous tool list divergence that occurs in live deployments. This is primarily an infrastructure and runtime state management problem beyond NIST's governance-level scope.
**Score: 2**

### RND_7_5
NIST AI 600-1 provides no coverage for function calling parameter type evolution, as this is a low-level runtime compatibility concern outside the framework's governance focus. GOVERN 6.1/6.2 could require third-party tool providers to document parameter type changes as part of supply chain risk management, but NIST has no controls for detecting or handling silent type coercion failures in deployed agents. MEASURE 2.7 security testing could verify parameter handling for known type configurations pre-deployment, but post-deployment type changes create silent failures that no NIST control can catch at runtime. This is an infrastructure-level API compatibility concern.
**Score: 1**

### RND_7_6
NIST AI 600-1 MEASURE 2.7 explicitly covers data poisoning as a security threat and mandates red-teaming evaluations, providing direct but pre-deployment-only coverage for few-shot example poisoning in tool schemas. The §2.9 Information Security section acknowledges adversarial manipulation of AI inputs, which encompasses poisoning of API calling examples embedded in tool definitions. However, NIST's testing mandate applies at a single point in time and cannot address distributed poisoning across a tool chain where compromised examples propagate to multiple consuming agents post-deployment. GOVERN 6.1/6.2 supply chain vetting provides a governance hook for auditing tool schema integrity from third-party providers.
**Score: 2**

### RND_7_7
MEASURE 2.7 red-teaming and data poisoning provisions provide partial coverage for adversarial injection of fallback behavior examples, as this fits the category of adversarial manipulation of model inputs and demonstrations. §2.9 Information Security acknowledges manipulation of AI behavior through crafted inputs, and GOVERN 6.1/6.2 supply chain provisions can require vetting of tool documentation sources. However, NIST's coverage is entirely pre-deployment and cannot detect or respond to fallback poisoning that triggers only under specific runtime conditions. The routing of agents to compromised implementations through triggered fallback behavior is an attack vector that NIST's testing regime cannot reliably surface given its non-deterministic activation conditions.
**Score: 2**

---

## RND_8 - Framework and Architecture Non-Determinism

### RND_8_1
NIST AI 600-1 MEASURE 2.7 mandates pre-deployment security evaluation and red-teaming, but the exponential state space of combined multi-pattern agent architectures (ReAct + Plan-and-Execute + Reflection) makes this mandate practically unenforceable. The framework provides no guidance on how to scope or prioritize testing when the combinatorial space approaches (K^M)^N * R^N, leaving organizations with a governance obligation but no tractable methodology. MEASURE 2.9 model explanation and validation provisions nominally apply to understanding emergent behaviors but are designed for single-model validation, not multi-pattern compositional non-determinism. NIST's risk management approach assumes testable, bounded systems and does not address the fundamental intractability created by multi-pattern composition.
**Score: 1**

### RND_8_2
NIST AI 600-1 explicitly does not address framework-specific implementations such as LangChain, LangGraph, or AutoGen, and the multiplicative non-determinism created by combining these frameworks falls entirely outside the scope of NIST's governance provisions. MEASURE 2.7 red-teaming could test individual framework configurations but has no mechanism for addressing the combinatorial explosion of cross-framework interactions. The framework's pre-deployment testing mandate becomes nominally meaningful at best when comprehensive testing is computationally infeasible. No NIST control addresses how organizations should manage untestable attack surfaces created by multi-framework agent compositions.
**Score: 1**

### RND_8_3
GOVERN 6.1/6.2 supply chain provisions could require framework vendors to disclose significant updates affecting security-relevant behavior, providing a thin governance hook for tracking framework update cadence. However, NIST provides no controls for the N(N-1)/2 re-evaluation problem where every agent combination must be re-tested after any framework component update. MEASURE 2.7's mandate to conduct red-teaming implicitly requires re-testing after significant changes, but the framework gives no guidance on defining "significant" or managing continuous invalidation of security assurances. The continuous framework update problem fundamentally undermines the static point-in-time assurance model that NIST's MEASURE category assumes.
**Score: 1**

### RND_8_4
NIST AI 600-1 does not address streaming implementations, and framework update impacts on streaming buffering, timing, and error handling are infrastructure-level concerns entirely outside the framework's scope. MEASURE 2.7 could nominally include streaming behavior in red-teaming scope, but NIST provides no specific guidance or requirements for streaming security. The invalidation of streaming security assumptions by framework updates compounds the general framework update problem with streaming-specific timing and state management concerns that NIST has no provisions to address. This is a framework-specific and infrastructure concern receiving no meaningful NIST coverage.
**Score: 1**

---

## RND_9 - LangChain/LangGraph Specific Non-Determinism

### RND_9_1
NIST AI 600-1 explicitly does not cover LangChain or LangGraph framework-specific behaviors, and conditional edge non-determinism in LangGraph is a framework-internal implementation concern beyond NIST's governance scope. MEASURE 2.7 red-teaming could test routing behavior pre-deployment but cannot address the compounded non-determinism created by multiple non-deterministic conditional edges interacting in a multi-agent workflow. The framework's pre-deployment testing model has no mechanism for validating routing consistency across the continuous distribution of possible external factor states that influence conditional edge evaluation. No NIST control addresses framework-specific routing non-determinism or its security implications.
**Score: 1**

### RND_9_2
LangGraph reducer behavior divergence is a framework-specific state management concern that NIST AI 600-1 does not address, as the framework operates at a governance level above individual framework implementation details. MEASURE 2.7 testing could include state consistency validation as part of system-level red-teaming, providing only the weakest governance hook. The specific problem of multiple agents holding conflicting reducer expectations for shared state fields is an architectural pattern concern internal to LangGraph that predefined NIST categories do not map to. No NIST supply chain or security evaluation provision addresses distributed state management consistency in framework-specific agent implementations.
**Score: 1**

### RND_9_3
NIST AI 600-1 has no provisions addressing checkpoint restoration non-determinism, as this is a framework-specific operational concern for LangGraph's persistence mechanisms. MEASURE 2.7 pre-deployment testing cannot account for the divergent execution paths that emerge when replaying cyclic workflows from checkpoints in the presence of non-deterministic elements, since this requires observing runtime state evolution across many executions. The checkpoint replay problem represents a compounding of LangGraph-specific persistence behavior with general LLM non-determinism in ways that NIST's governance framework has no category to address. Infrastructure and framework-specific concerns receive no meaningful NIST coverage.
**Score: 1**

### RND_9_4
NIST AI 600-1 MEASURE 2.7 addresses LLM non-determinism indirectly through its mandate for security evaluation and red-teaming of AI systems, providing a thin governance hook for testing safety gate reliability. However, the specific problem of intermittently failing confidence-score-based safety gates creating exploitable non-deterministic windows requires runtime monitoring and probabilistic testing methodologies that NIST does not prescribe. Attackers probing for non-deterministic windows where safety checks fail is an adversarial technique that NIST's point-in-time red-teaming cannot reliably surface. The LangChain-specific implementation of confidence-scored tool selection is a framework concern receiving no direct NIST coverage.
**Score: 1**

### RND_9_5
NIST AI 600-1 does not address distributed memory management race conditions, which are infrastructure-level concurrency concerns beyond the framework's governance scope. MEASURE 2.7 could test memory update behavior in single-threaded scenarios but has no mechanism for surfacing ordering-dependent failures that emerge only under distributed concurrent access patterns. The §2.4 Data Privacy section acknowledges risks from improper data handling but does not address the integrity risks from non-deterministic state evolution in shared agent memory. LangChain-specific memory management is a framework implementation detail that NIST categories do not cover.
**Score: 1**

### RND_9_6
GOVERN 6.1/6.2 supply chain provisions can require LangChain to notify of underlying model version changes as a third-party risk management practice, providing a minimal governance hook for tracking model update impact on security evaluations. MEASURE 2.7 mandates security evaluation and red-teaming, which implicitly requires re-evaluation when underlying models change, but NIST provides no guidance on managing continuous invalidation of security assurances from regular model updates. The problem of regular model updates perpetually invalidating previously conducted security evaluations is a fundamental limitation of NIST's static assurance model that the framework does not acknowledge or address. This concern partially overlaps with supply chain risk but is primarily a framework-specific operational problem.
**Score: 2**

### RND_9_7
GOVERN 6.1/6.2 supply chain provisions provide a thin governance hook for requiring external tool API vendors to communicate breaking changes, nominally addressing the tool API compatibility drift problem. NIST MEASURE 2.7 can test specific API configurations pre-deployment but cannot address non-deterministic error handling behaviors that emerge from API evolution after deployment. The unpredictable agent behaviors created by LangChain's error handling interacting with evolved external APIs is a runtime operational concern combining framework-specific and infrastructure elements that NIST does not cover. Supply chain vetting provides the only partial NIST coverage for this concern.
**Score: 2**

---

## RND_10 - AutoGen Specific Non-Determinism

### RND_10_1
NIST AI 600-1 explicitly does not cover AutoGen or other specific multi-agent frameworks, and the message-driven conversation non-determinism inherent to AutoGen's architecture is a framework-internal concern beyond NIST's governance scope. MEASURE 2.7 mandates red-teaming for security evaluation, but the specific challenge of attacks succeeding in only 0.1% of executions requires probabilistic testing methodologies and large execution sample sizes that NIST does not prescribe or require. The framework's pre-deployment testing model fundamentally cannot surface low-probability attack windows in stochastic multi-agent conversation flows. No NIST control addresses how organizations should validate security for systems with inherently irreproducible execution paths.
**Score: 1**

### RND_10_2
AutoGen's GroupChat speaker selection mechanism is a framework-specific implementation detail that NIST AI 600-1 has no provisions to address. MEASURE 2.7 red-teaming could test multi-agent systems including those with probabilistic role assignment, but the specific threat of malicious agents being selected probabilistically to act creates an attack surface detectable only through extensive runtime statistical analysis that NIST does not prescribe. The intersection of stochastic speaker selection and adversarial agent behavior in AutoGen systems represents a framework-specific attack surface receiving no meaningful NIST coverage. Infrastructure and framework-specific concerns are explicitly outside NIST's scope.
**Score: 1**

### RND_10_3
GOVERN 6.1/6.2 supply chain provisions can nominally require notification of significant updates from AutoGen, CrewAI, and LLM providers, providing minimal governance coverage for tracking multi-framework update impacts. The infeasible re-testing problem created by N agents across M framework versions across P LLM versions reflects the same fundamental limitation as RND_8_3: NIST's static assurance model cannot accommodate continuous multi-dimensional update invalidation. MEASURE 2.7 implicitly requires re-evaluation after significant changes but provides no guidance on scoping or managing cumulative assurance degradation across multiple simultaneously updating components. This is a framework-specific concern compounded by multi-provider supply chain dynamics.
**Score: 1**

---

## RND_11 - CrewAI Specific Non-Determinism

### RND_11_1
NIST AI 600-1 does not address CrewAI or its hierarchical task routing mechanisms, and probabilistic delegation routing is a framework-specific implementation concern outside NIST's governance scope. MEASURE 2.7 security evaluation could test delegation behavior for specific routing outcomes pre-deployment but cannot address the statistical attack surface created when routing to compromised workers depends on probabilistic selection across many executions. GOVERN 6.1/6.2 supply chain provisions have no relevance to internal CrewAI routing logic. The threat of attacks succeeding when stochastic delegation happens to select a compromised worker is a framework-specific runtime concern that NIST has no category to address.
**Score: 1**

---

## RND_12 - Semantic Kernel Specific Non-Determinism

### RND_12_1
NIST AI 600-1 does not address Semantic Kernel or LLM-driven dynamic plugin routing, making this a framework-specific concern outside the framework's explicit scope. MEASURE 2.7 could include Semantic Kernel plugin routing in red-teaming scope as part of system-level security evaluation, providing a minimal governance hook, but non-deterministic LLM-driven function selection undermines the reproducibility required for meaningful audit trails. The inability to reproduce specific routing paths for audit purposes is a runtime operational and framework-specific problem that NIST's governance approach cannot solve. No NIST control addresses how organizations should maintain audit integrity for systems with inherently non-deterministic internal routing.
**Score: 1**

### RND_12_2
GOVERN 6.1/6.2 supply chain provisions could nominally require tracking of plugin registry state as part of third-party component management, providing the weakest governance hook for plugin registry change management. NIST has no controls addressing dynamic plugin discovery and registration mechanisms, and registry state changes that modify routing without code changes fall outside what MEASURE 2.7 pre-deployment testing can anticipate. The specific Semantic Kernel dynamic plugin registry is a framework implementation detail that invalidates prior security evaluations in ways no NIST provision addresses. This is a framework-specific concern receiving no direct NIST coverage.
**Score: 1**

### RND_12_3
FunctionChoiceBehavior configuration drift across Semantic Kernel agent updates is a framework-specific configuration management concern that NIST AI 600-1 does not address. NIST's governance framework has no controls for detecting or preventing configuration drift in framework-specific routing behavior parameters, even though configuration inconsistency is a recognized category of operational risk. MEASURE 2.7 testing could validate routing consistency for a specific FunctionChoiceBehavior configuration but cannot address the post-deployment drift created when different agents operate with different settings. Infrastructure and framework-specific configuration management concerns receive no meaningful NIST coverage.
**Score: 1**

### RND_12_4
NIST AI 600-1 has no provisions for dynamic service registration lifecycle management in agent frameworks, as this is a runtime infrastructure concern outside the governance framework's scope. Concurrent agents observing inconsistent service sets due to mid-execution registry changes represents a race condition in Semantic Kernel's service lifecycle that MEASURE 2.7 pre-deployment testing cannot surface under realistic concurrent load conditions. The state inconsistency created by dynamic service registration changes during execution is a framework-specific operational concern combining infrastructure and Semantic Kernel implementation details. No NIST control addresses service registry consistency for dynamically configured agent runtime environments.
**Score: 1**
# NIST AI 600-1 Analysis: RND_13 through RND_18

---

## RND_13 - Multimodal and Vision-Language Models

### RND_13_1
NIST AI 600-1 has minimal relevance to multimodal model version drift creating inconsistent behavior across agents. The framework's Value Chain and Component Integration category (§2.12) and GOVERN 6.1/6.2 address third-party component vetting and supply chain accountability, which could in principle require that vision model versions be tracked and approved before deployment. However, NIST does not address asynchronous update schedules between heterogeneous model components (CLIP v1.0 vs. v2.0 across agents), nor does it provide any guidance on version synchronization policies, behavioral regression detection from version skew, or non-determinism introduced by mismatched multimodal embeddings at runtime. The scope of the framework is governance-level pre-deployment policy, not operational version control across distributed agent deployments.
**Score: 1**

### RND_13_2
NIST AI 600-1 offers no meaningful coverage of vision model output format inconsistency across multi-agent systems. The profile's 12 risk categories do not include interoperability or output schema consistency between heterogeneous vision models (NeVA, DePlot, CLIP), and the governance-level controls for supply chain vetting (GOVERN 6.1/6.2) address whether a component was approved, not whether its outputs are format-compatible with downstream consumers. Confabulation (§2.2) is the closest risk category but applies to factual hallucination, not structured-output format divergence that creates parsing ambiguities across agent boundaries. The framework provides no technical guidance on schema normalization, format contract enforcement between agents, or detection of processing failures caused by vision model heterogeneity.
**Score: 1**

### RND_13_3
Whisper model update drift in audio processing agents falls outside NIST AI 600-1's effective scope. Supply chain vetting (GOVERN 6.1/6.2, §2.12) could require tracking Whisper model versions used by audio agents, and pre-deployment testing (MEASURE 2.7) could in principle include regression tests for transcription behavior changes across versions. However, the framework provides no guidance on detecting asynchronous update drift between audio transcription agents and dependent synthesis agents, managing hallucination pattern changes introduced by model updates, or maintaining behavioral alignment across agent pipelines when component models update independently. The practical contribution is limited to version provenance tracking at governance level, without operational controls for the divergence-induced security failures described.
**Score: 1**

### RND_13_4
Embedding model quantization inconsistency across multi-agent deployments is a deployment infrastructure concern that sits outside NIST AI 600-1's governance scope. The framework's supply chain category (§2.12) and GOVERN 6.1/6.2 could require disclosure of quantization configurations as part of component vetting, but NIST has no guidance on the security implications of FP32 vs. INT8 precision differences affecting similarity thresholds, retrieval ranking divergence between agents, or the adversarial exploitation of threshold inconsistencies. MEASURE 2.6 and 2.7 address pre-deployment safety and security evaluation but do not extend to quantization-induced ranking discrepancies in multi-agent retrieval pipelines. This threat is fundamentally a low-level model deployment and numerical precision concern outside the framework's intended scope.
**Score: 1**

### RND_13_5
Temperature variation in vision-language model outputs is an operational parameter tuning issue that NIST AI 600-1 does not govern. The framework addresses pre-deployment testing and organizational governance but has no controls over inference-time sampling parameters such as temperature settings, nor does it address how different temperature configurations across VLM instances in a multi-agent system produce divergent outputs from identical images. Human-AI Configuration (§2.7 risk category) acknowledges over-reliance concerns but does not address parametric output variability across agents. The scoring guidance for temperature threats consistently places these at 1 due to their operational, non-governance character, and the multimodal dimension does not elevate NIST's relevance.
**Score: 1**

---

## RND_14 - Evaluation, Testing, and Benchmarking Non-Determinism

### RND_14_1
NIST AI 600-1's MEASURE 2.7 calls for pre-deployment security evaluation and red-teaming, which provides a thin governance hook for evaluation pipeline integrity: organizations following the profile should conduct testing and document results, creating implicit pressure for reproducible evaluation methodology. However, the framework does not address stochastic components within evaluation pipelines, non-deterministic regression testing, or the security implication that identical model versions produce different evaluation results. NIST's testing guidance is prescriptive at the organizational level (conduct testing, document findings) rather than methodological, and it offers no controls for evaluation pipeline determinism, seed management, or the specific failure mode of regression detection becoming unreliable due to evaluation-level stochasticity.
**Score: 2**

### RND_14_2
NIST AI 600-1 MEASURE 2.7 pre-deployment evaluation guidance provides a weak hook here: if evaluation metric models are treated as components subject to vetting, then supply chain controls (§2.12, GOVERN 6.1/6.2) could require tracking metric model versions and flagging updates that shift baseline behavior. However, the framework has no guidance on evaluation metric model governance, baseline drift detection when metric models update, or the security consequence of multi-agent evaluation agents operating on inconsistent metric versions. The practical relevance is that NIST's supply chain and pre-deployment testing categories can be interpreted to encompass evaluation infrastructure, but this interpretation requires significant extension beyond the framework's explicit scope.
**Score: 2**

### RND_14_3
Multi-stage evaluation pipeline component drift is an evaluation infrastructure management problem that NIST AI 600-1 does not directly address. MEASURE 2.7 requires pre-deployment testing to occur, which implicitly requires a functioning evaluation pipeline, but the framework provides no guidance on evaluation pipeline component versioning, independent update schedules across pipeline stages, or the reproducibility implications of component drift over time. Supply chain vetting (§2.12) could apply to third-party evaluation components, but the drift scenario described involves components updating post-deployment in ways that make prior evaluation results non-reproducible. NIST's governance-level scope does not reach the operational discipline of evaluation pipeline configuration management.
**Score: 1**

### RND_14_4
Stochastic test case selection creating non-deterministic coverage is an evaluation methodology concern that NIST AI 600-1 does not govern. MEASURE 2.7 mandates that security evaluation and red-teaming occur but says nothing about how test cases should be selected, whether selection should be deterministic, or how coverage variance across evaluation runs should be detected and managed. The framework provides no controls against attackers gaming stochastic test selection to avoid detection, nor does it address the multi-agent compounding of coverage gaps when each agent's evaluation relies on independent random test samples. This is a testing methodology problem outside the framework's governance-level scope.
**Score: 1**

### RND_14_5
Framework-dependent evaluation behavior creating multi-framework incomparability falls outside NIST AI 600-1's scope. The profile does not address evaluation framework selection, cross-framework result comparability, or the security implication of different evaluation environments producing different behavioral assessments for the same model. GOVERN 6.1/6.2 could require documenting which evaluation framework was used, providing minimal traceability, but NIST has no guidance on standardizing evaluation environments, controlling framework-induced variance, or detecting when framework differences rather than model differences explain evaluation result divergence. The framework's governance controls operate at a level of abstraction where evaluation framework choice is invisible.
**Score: 1**

### RND_14_6
NIST AI 600-1 MEASURE 2.7's security evaluation guidance provides a moderate hook for this threat: the mandate to conduct pre-deployment red-teaming and security testing implicitly requires that benchmark results be reliable enough to make deployment decisions, creating organizational pressure to manage non-determinism in benchmark methodology. The adversarial exploitation angle — where attackers engineer inputs with unpredictable behavior specifically because high variance makes detection inconsistent — maps to the profile's concern with evasion of safety measures. However, NIST provides no technical guidance on multi-trial averaging, seed standardization, or acceptable variance thresholds for benchmark reliability. The relevance is that MEASURE 2.7 creates a governance expectation for meaningful pre-deployment assessment, but the framework does not specify how to achieve reliable benchmarks.
**Score: 2**

### RND_14_7
Statistical significance and multiple comparison problems in multi-agent evaluation are methodological concerns outside NIST AI 600-1's scope. MEASURE 2.7 requires security evaluation but provides no statistical methodology guidance, no requirements for multiple comparison correction, and no recognition that running N significance tests across N agents inflates false positive rates in ways adversaries can exploit. The framework's governance-level testing mandate does not extend to the mathematical validity of statistical inference procedures used during evaluation. NIST's contribution is limited to requiring that evaluation occur, with no controls over the rigor of the statistical methods applied.
**Score: 1**

### RND_14_8
NIST AI 600-1 has moderate relevance to model update proliferation creating assurance gaps through its Value Chain and Component Integration category (§2.12) and supply chain vetting controls (GOVERN 6.1/6.2), which require organizations to track and vet AI components including updates to deployed models. The scenario where vulnerabilities fixed in Agent A remain exploitable in out-of-date Agent B maps to supply chain governance failures that NIST's controls are designed to flag at the organizational level. MEASURE 2.7 further supports this by requiring security evaluation, which should in principle detect when agents running different versions have divergent security postures. The limitation is that NIST operates pre-deployment and at governance level; it does not provide controls for detecting or remediating version skew in live multi-agent deployments.
**Score: 2**

---

## RND_15 - Temperature and Sampling Parameters

### RND_15_1
NIST AI 600-1 has no meaningful coverage of temperature and sampling parameter non-determinism creating unpredictable agent behavior. The framework's governance, pre-deployment testing, and incident response controls operate at an organizational level that does not extend to inference-time sampling configuration, stochastic output propagation across multi-agent pipelines, or the compounding effect where one agent's variable outputs become another agent's inputs. No risk category in the profile addresses operational parameter tuning or the security implications of temperature-induced output variance. Per the scoring guidance, temperature/sampling threats score 1 as operational parameters outside governance scope.
**Score: 1**

### RND_15_2
Continuous parameter re-tuning in production as an assurance gap falls outside NIST AI 600-1's scope. MEASURE 2.7 requires pre-deployment security evaluation, which provides minimal relevance if re-tuning is treated as a deployment-triggering event requiring fresh evaluation — but NIST does not define what constitutes a material change requiring re-evaluation, and continuous or incremental re-tuning in production would not typically be captured by pre-deployment governance processes. The framework has no controls for production parameter drift, moving-target security posture across continuously re-tuned multi-agent systems, or the monitoring infrastructure needed to detect when re-tuning has degraded a security boundary. This is an operational concern outside the framework's intended coverage.
**Score: 1**

### RND_15_3
Cross-agent parameter drift creating inconsistent security posture — one agent with temperature 0.2 consistently rejecting injections while a peer at temperature 0.5 accepts them — is a runtime configuration management problem that NIST AI 600-1 does not address. The framework provides no controls over per-agent temperature settings, no requirement for parameter consistency across agents in a shared pipeline, and no guidance on the security implication of heterogeneous sampling configurations creating exploitable behavioral gaps. Information Security (§2.9) recognizes prompt injection as a risk, but the specific vulnerability here is the inconsistent posture created by temperature variation, not the injection itself. NIST's governance scope does not reach individual model inference parameters.
**Score: 1**

---

## RND_16 - Parameter Extraction and Tool Calling

### RND_16_1
NIST AI 600-1 has no direct coverage of stochastic parameter generation drift creating hallucination windows exploitable across differently-configured agents. The Confabulation risk category (§2.2) acknowledges hallucination as a GenAI risk and calls for measurement and mitigation, but does not address how temperature-induced hallucination rate differences between agents create asymmetric attack surfaces. Pre-deployment testing (MEASURE 2.7) could evaluate hallucination rates at different temperature settings, providing a weak pre-deployment signal, but the framework does not address the adversarial exploitation of inter-agent temperature disparities or runtime hallucination targeting. The governance relevance is minimal given the operational, parameter-level character of this threat.
**Score: 1**

### RND_16_2
NIST AI 600-1's Value Chain and Component Integration category (§2.12) and supply chain vetting (GOVERN 6.1/6.2) offer limited but genuine relevance to tool documentation drift across agent training cycles: if tool specifications are treated as third-party or internal components subject to version governance, then NIST's supply chain controls could require that agents' training data and fine-tuning corpora reflect current specifications. However, the framework does not address continuous update cycles, the training data provenance of fine-tuned agents, or the behavioral divergence introduced when agents are trained on different versions of tool documentation. The supply chain framing provides a governance entry point, but the technical details of training data currency and its security implications are outside the profile's scope.
**Score: 1**

### RND_16_3
Probabilistic tool selection creating adversarial search space is an operational inference behavior that NIST AI 600-1 does not govern. The framework's Information Security category (§2.9) recognizes prompt injection and adversarial manipulation, and pre-deployment testing (MEASURE 2.7) could in principle test tool selection under adversarial input conditions. However, NIST provides no guidance on tool selection determinism, probabilistic routing security, or the multi-agent attack surface created when different agents independently select different tools for identical requests. The adversarial crafting of ambiguous requests to maximize malicious tool selection probability is an inference-time attack vector that governance documentation and vetting processes do not reach.
**Score: 1**

### RND_16_4
Non-deterministic fallback ordering in tool chains creating unpredictable attack surfaces is an agent architecture and runtime behavior concern outside NIST AI 600-1's scope. The framework does not address fallback chain design, probabilistic tool routing, or the security implications of fallback sequences that probabilistically include malicious tools. Information Security (§2.9) recognizes adversarial manipulation but at a higher level of abstraction than fallback chain exploitation. Pre-deployment testing could probe fallback behavior, but NIST does not prescribe testing of probabilistic fallback ordering or require that fallback chains be deterministic for security purposes.
**Score: 1**

### RND_16_5
NIST AI 600-1's Confabulation category (§2.2) directly addresses hallucination as a GenAI risk and calls for measurement and mitigation, giving the framework moderate relevance to continuous hallucination rate regression through fine-tuning updates. MEASURE 2.9 (model explanation and validation) and MEASURE 2.7 (security evaluation) together support the idea that model updates should be re-evaluated for hallucination rate changes before deployment. The threat scenario — where grounding checks calibrated to pre-update hallucination rates become ineffective after fine-tuning regression — maps to a failure of the pre-deployment re-evaluation processes NIST recommends. The limitation is that NIST does not mandate specific grounding check recalibration procedures or continuous monitoring of hallucination rates in production after updates.
**Score: 2**

### RND_16_6
Non-deterministic parameter extraction caused by temperature and sampling settings multiplying variance downstream in multi-agent pipelines is an operational inference concern that NIST AI 600-1 does not cover. As with other temperature/sampling threats, the framework's governance scope does not extend to individual model inference parameters, inter-agent variance propagation, or the compounding of extraction inconsistency across pipeline stages. Confabulation (§2.2) is peripherally relevant to extraction errors but does not address the multi-agent amplification of temperature-induced variance. Per the scoring guidance for temperature/sampling threats, this scores 1.
**Score: 1**

### RND_16_7
Tool selection variance creating reliability gaps between agents in a coordinated pipeline is an operational reliability concern outside NIST AI 600-1's governance scope. The framework does not address agent-to-agent coordination reliability, tool selection consistency requirements, or the security implication of Agent B assuming specific tool semantics from Agent A that are not reliably delivered. Value Chain (§2.12) provides a weak reference point if tool selection consistency is framed as a component integration quality issue, but NIST offers no technical controls for this failure mode.
**Score: 1**

### RND_16_8
Trajectory variance creating non-deterministic execution paths across runs is a fundamental operational behavior concern that NIST AI 600-1 does not govern. The framework's pre-deployment testing guidance (MEASURE 2.7) could require testing of agent trajectories under adversarial conditions, providing a thin pre-deployment hook, but NIST does not address trajectory determinism, execution path consistency requirements, or the security implication of different runs producing different action sequences from identical inputs. The governance-level scope of the profile does not reach agent trajectory design or runtime execution path management.
**Score: 1**

### RND_16_9
Parameter accuracy variance across temperature settings becoming a security-relevant failure mode falls outside NIST AI 600-1's scope. Confabulation (§2.2) provides the closest risk category as parameter extraction errors are a form of model output inaccuracy, and pre-deployment testing (MEASURE 2.7) could include accuracy benchmarks at different temperature settings. However, NIST does not mandate temperature-stratified accuracy evaluation, does not address the security implications of temperature-dependent extraction accuracy, and provides no controls for ensuring consistent parameter extraction across agents with different sampling configurations. The relevance is minimal.
**Score: 1**

---

## RND_17 - Retrieval-Augmented Generation (RAG) and Multi-Hop QA

### RND_17_1
NIST AI 600-1 provides moderate coverage of ranking model manipulation through relevance score injection. The Information Security category (§2.9) explicitly names data poisoning as a recognized threat, and crafting documents with artificially high relevance scores to manipulate retrieval ranking is a form of data poisoning against RAG systems. Value Chain and Component Integration (§2.12) adds supply chain relevance for third-party document corpora used in multi-hop retrieval. Pre-deployment red-teaming (MEASURE 2.7) could include adversarial document injection scenarios to probe retrieval ranking robustness. The limitation is that NIST provides governance-level recognition without technical controls for ranking model hardening, relevance score integrity verification, or multi-agent amplification of poisoned retrieval through retrieve-then-synthesize pipelines.
**Score: 2**

### RND_17_2
Semantic similarity exploitation in dense retrieval — where adversaries craft documents semantically close to legitimate queries but containing injected instructions — is an indirect prompt injection attack through the retrieval layer, which §2.9 (Information Security) explicitly recognizes. The multi-hop, multi-agent dimension adds a supply chain framing (§2.12) where third-party or user-provided documents become attack vectors. MEASURE 2.7 red-teaming guidance supports pre-deployment testing of dense retrieval robustness against adversarial documents. However, NIST does not address dense embedding space security, semantic neighborhood exploitation, or the specific vulnerability of multi-hop retrieval enabling progressive instruction injection across multiple retrieval steps. The framework provides governance-level recognition of the attack class without technical countermeasures.
**Score: 2**

### RND_17_3
Query reformulation instruction injection in multi-hop retrieval — where attackers embed instructions in source documents that become incorporated into reformulated sub-queries — is an indirect prompt injection variant explicitly acknowledged in §2.9. Multi-hop retrieval architectures that reformulate queries from untrusted source documents create a chain where injected instructions can propagate into subsequent retrieval steps, amplifying the attack surface. MEASURE 2.7 red-teaming could probe this vector in pre-deployment testing. NIST's limitation here is that it recognizes the injection risk class without addressing the specific mechanics of query reformulation as an injection propagation pathway, leaving multi-hop chain security to implementers without governance-level guidance on reformulation input sanitization.
**Score: 2**

### RND_17_4
Evidence document hallucination and fake bridging document injection in multi-hop reasoning maps partially to NIST AI 600-1's Confabulation category (§2.2), which addresses fabricated outputs including hallucinated factual content, and to data poisoning under Information Security (§2.9) for the attacker-injected fake bridge scenario. MEASURE 2.9 (model explanation and validation) supports evaluating whether agents can distinguish genuine from fabricated bridging evidence. However, NIST does not address the specific vulnerability of multi-agent systems treating injected bridge documents as valid intermediate evidence without authentication, nor does it provide controls for evidence provenance verification in multi-hop reasoning chains. The dual coverage from confabulation and data poisoning categories provides a moderate but indirect match.
**Score: 2**

### RND_17_5
Evidence grounding failures as a RAG attack surface map to NIST AI 600-1's Confabulation category (§2.2), which recognizes weak grounding as a hallucination risk, and to Information Security (§2.9) for the injection facilitated by grounding failures. In multi-agent architectures where Agent B cannot validate the authenticity of Agent A's retrieved evidence, the trust propagation problem has a partial analog in Value Chain (§2.12) cross-component accountability. MEASURE 2.9 supports grounding validation in pre-deployment evaluation. The framework's limitation is that it does not address multi-agent grounding chain validation, cross-agent evidence authentication, or the compounding of grounding failures when retrieved documents propagate unvalidated through agent pipelines.
**Score: 2**

### RND_17_6
Source attribution reasoning confusion in multi-agent systems aggregating across sources maps to NIST AI 600-1's Information Integrity category (§2.8), which addresses disinformation and provenance manipulation, and to Information Security (§2.9) for injection via attribution confusion. Value Chain (§2.12) provides supply chain framing for multi-source document aggregation architectures. However, NIST does not address source attribution logic in multi-agent aggregation pipelines, the specific attack pattern of confusing attribution to cause agents to act on attacker-controlled sources as if they were trusted ones, or the propagation of attribution errors across sequential agents. The framework's coverage is indirect, recognizing the general risk classes without addressing multi-agent attribution chain security.
**Score: 2**

### RND_17_7
Hallucination blindness in knowledge graphs — where knowledge extraction agents hallucinate relationships that corrupt shared graph state subsequently consumed by other agents — maps directly to NIST AI 600-1's Confabulation category (§2.2), which explicitly addresses fabricated relationships and factual content as a core GenAI risk. MEASURE 2.9 supports validation of model outputs including extracted knowledge. MEASURE 2.7 red-teaming could probe knowledge extraction hallucination rates before deployment. However, NIST does not address the multi-agent knowledge graph contamination scenario where Agent A's hallucinations become persistent knowledge entries that Agent B queries as ground truth; the framework's confabulation controls focus on model output validation rather than on the downstream persistence and propagation of hallucinated knowledge across agent boundaries.
**Score: 2**

---

## RND_18 - Infrastructure and Deployment Non-Determinism

### RND_18_1
Event delivery guarantee downgrade attacks via infrastructure resource exhaustion fall entirely outside NIST AI 600-1's governance scope. The framework does not address message delivery guarantees, infrastructure resource management, DoS-induced service degradation, or the silent corruption of payment and coordination logic caused by delivery guarantee downgrades. MANAGE 4.1–4.3 covers incident response after failures are detected, but provides no preventive controls for infrastructure-level resource exhaustion attacks. This is a low-level infrastructure security concern — involving queue infrastructure, protocol guarantees, and resource allocation — that is outside the profile's governance and risk management scope.
**Score: 1**

### RND_18_2
Swarm consensus manipulation via strategic Byzantine agent injection at sub-detection densities is a multi-agent trust and operational security concern that NIST AI 600-1 does not address. The framework has no controls for Byzantine fault tolerance in multi-agent consensus mechanisms, detection of adversarial agent injection, or threshold-based monitoring limitations in distributed agent swarms. Value Chain (§2.12) could nominally cover agent identity vetting as a supply chain issue, but the dynamic injection of agents at runtime into swarm architectures is an operational security problem outside pre-deployment vetting scope. NIST's governance controls do not reach consensus algorithm design or Byzantine agent detection.
**Score: 1**

### RND_18_3
Message queue ordering guarantee failures under scaling are infrastructure architecture and operational reliability concerns outside NIST AI 600-1's scope. The framework does not address queue infrastructure, message ordering guarantees, distributed systems consistency under load, or the security implications of agents losing ordering guarantees when additional queue instances are added during scaling. No risk category in the profile maps to distributed systems message ordering properties. MANAGE 4.1–4.3 incident response is the only tangentially relevant area, applicable only after failures manifest, not as a preventive control.
**Score: 1**

### RND_18_4
Vector database index staleness as a runtime assurance gap is an infrastructure operational concern outside NIST AI 600-1's governance scope. While the RAG dimension could nominally connect to §2.2 Confabulation (stale retrieval producing outdated or incorrect outputs) and §2.9 Information Security (injection via stale index content), the specific failure mode — index staleness from outdated corpus state creating gaps between agent expectations and actual retrieval behavior — is an infrastructure management problem that governance documentation and pre-deployment vetting do not address. NIST does not provide controls for vector database refresh policies, index currency requirements, or real-time corpus state monitoring.
**Score: 1**

### RND_18_5
Prometheus scrape interval variance as an observability assurance gap is a monitoring infrastructure concern entirely outside NIST AI 600-1's scope. The framework does not address observability infrastructure, metrics collection latency, monitoring system design, or the security implication of metrics appearing current while being 30+ seconds stale in multi-agent chains. NIST has no risk category that maps to observability infrastructure reliability. MANAGE 4.1–4.3 covers incident response but does not govern the observability infrastructure needed to detect incidents in the first place.
**Score: 1**

### RND_18_6
API gateway canary deployment assurance gaps — where Kong canary deployments create version mismatches between v2.0 agents invoking v1.0 tools causing format errors — are infrastructure deployment management concerns outside NIST AI 600-1's scope. Value Chain (§2.12) and GOVERN 6.1/6.2 could nominally require that API gateway configurations be tracked as components, but the specific failure mode of canary deployments creating format mismatch between agents on different API versions is an operational deployment orchestration problem that governance documentation does not reach. NIST does not address canary deployment policies, version compatibility management, or the security implications of partial rollouts in multi-agent systems.
**Score: 1**

### RND_18_7
MLflow version mismatch assurance gaps during rollouts — where agents on different MLflow versions create shared data structure incompatibilities — are infrastructure tooling and deployment coordination concerns outside NIST AI 600-1's governance scope. Value Chain (§2.12) provides the weakest possible hook if MLflow is treated as a third-party component subject to vetting, but the dynamic version skew created during rolling rollouts is an operational deployment management problem rather than a supply chain integrity problem. NIST has no guidance on ML infrastructure tooling version management, rollout coordination policies, or data structure compatibility across tool versions during transitions.
**Score: 1**
# NIST AI 600-1 Analysis Cache - RND_19 through RND_24

---

## RND_19 - Kubernetes and Container Orchestration

### RND_19_1
NIST AI 600-1 is a governance-level GenAI risk profile and contains no guidance on Kubernetes infrastructure components including the Horizontal Pod Autoscaler. The 12 risk categories address model-level concerns (confabulation, data privacy, information security via prompt injection) and organizational governance; HPA metrics collection, scaling decision timing variability, and non-deterministic agent population changes caused by autoscaling are infrastructure orchestration concerns that fall entirely outside this scope. Pre-deployment testing guidance (MEASURE 2.7) addresses model behavior and red-teaming, not container scheduling behavior. No NIST AI 600-1 control maps to scaling-induced non-determinism in multi-agent deployments.
**Score: 1**

### RND_19_2
StatefulSet ordinal initialization sequencing and network races during pod restart are Kubernetes-native infrastructure concerns that NIST AI 600-1 does not address at any level. The profile's scope covers GenAI model risks—confabulation, data privacy, supply chain integrity, prompt injection—and organizational governance policies; container lifecycle management, pod startup sequencing, and TCP/gRPC race conditions during StatefulSet recovery are outside that scope entirely. MANAGE 4.1–4.3 covers incident response after failures are detected but provides no preventive controls for infrastructure-level non-determinism. The framework offers no applicable coverage.
**Score: 1**

### RND_19_3
Node autoscaling, pod placement topology changes, and the resulting non-determinism in multi-agent network paths are Kubernetes cluster infrastructure concerns fully outside NIST AI 600-1's scope. The profile does not address network topology, node provisioning, or pod scheduling policies. While Value Chain and Component Integration (§2.12) touches on supply chain integrity for third-party components, it does not extend to runtime infrastructure topology changes affecting agent communication paths. GOVERN 6.1/6.2 addresses vetting of third-party AI components before deployment, not dynamic infrastructure reconfiguration during operation.
**Score: 1**

### RND_19_4
Kubernetes scheduler tie-breaking non-determinism and the associated multi-agent attack surface—where intermittent placement of malicious agents on specific nodes enables exploitation only under certain scheduling outcomes—is an infrastructure orchestration security problem that NIST AI 600-1 was not designed to address. The Information Security category (§2.9) recognizes prompt injection and data poisoning as GenAI risks, but does not extend to node-level placement attacks or scheduler indeterminism. MEASURE 2.7 pre-deployment security evaluation could theoretically surface awareness of placement-dependent attack vectors, but provides no controls for scheduler behavior. The framework's governance and model-risk orientation offers no meaningful coverage here.
**Score: 1**

---

## RND_20 - Deployment and Inference Infrastructure

### RND_20_1
Container image drift caused by unpinned canary deployments creating version skew across agent instances is a deployment pipeline and DevSecOps concern outside NIST AI 600-1's scope. The Value Chain and Component Integration category (§2.12) and GOVERN 6.1/6.2 address third-party component vetting and supply chain accountability, which offers marginal conceptual relevance—image provenance can be framed as a supply chain integrity question. However, the profile provides no guidance on continuous deployment pipelines, image pinning policies, canary rollout controls, or multi-agent version consistency enforcement. The supply chain framing is too indirect to constitute meaningful coverage.
**Score: 1**

### RND_20_2
Non-deterministic traffic splitting across agent versions via random sampling in canary deployments, producing 2^N possible agent state combinations in multi-agent systems, is an infrastructure routing and deployment architecture concern. NIST AI 600-1 has no guidance on traffic splitting strategies, canary policies, or the combinatorial state explosion that results when multiple agents independently receive different version assignments. Pre-deployment testing (MEASURE 2.7) cannot address runtime probabilistic routing decisions. The profile offers no applicable coverage for this deployment non-determinism.
**Score: 1**

### RND_20_3
Asynchronous event processing creating message ordering non-determinism—where agents receiving the same events in different sequences produce different outputs—is a distributed systems architecture problem. NIST AI 600-1 does not address message queue design, event ordering guarantees, or asynchronous coordination protocols. The Information Security category (§2.9) covers prompt injection and data poisoning but not event-order manipulation as an attack vector. MEASURE 2.7 pre-deployment testing could potentially probe ordering-dependent behavior, but only if specifically designed to do so, and the framework provides no such direction. Coverage is negligible.
**Score: 1**

### RND_20_4
Rolling Kubernetes deployments creating unobservable state transitions across N×(N-1)/2 compatibility combinations between services is a deployment orchestration concern. NIST AI 600-1 does not address rolling update strategies, inter-service API compatibility during transitions, or observability gaps during partial rollouts. While MANAGE 4.1–4.3 covers incident response, it does not address the unobservable nature of compatibility failures during rolling deployments. The Value Chain framing (§2.12) is too abstract to map to version compatibility matrices. The framework provides no applicable coverage.
**Score: 1**

### RND_20_5
Serverless workflow checkpoint-resume non-determinism—where multiple agents race to resume from the same checkpoint—is a serverless execution model and distributed coordination problem outside NIST AI 600-1's scope. The profile contains no guidance on serverless execution frameworks, checkpoint management, or concurrent workflow resumption. MANAGE 4.1–4.3's incident response provisions are downstream of the failure and do not prevent checkpoint race conditions. No NIST AI 600-1 control addresses this infrastructure-level non-determinism.
**Score: 1**

### RND_20_6
Rolling update non-determinism where Agent A v1 calls a tool interface that Agent B v2 has changed is an API versioning and deployment coordination problem. NIST AI 600-1's Value Chain and Component Integration category (§2.12) tangentially addresses component integration risks, and GOVERN 6.1/6.2 calls for vetting of third-party components, which could in principle extend to interface contract management between agent versions. However, the framework provides no technical guidance on API versioning strategies, interface compatibility enforcement, or multi-agent upgrade sequencing. The indirect supply chain framing offers minimal but non-zero relevance.
**Score: 1**

### RND_20_7
Traffic splitting creating concurrent agent interactions with different versions of a Tool API (v1 and v2 simultaneously) is a canary deployment architecture problem. As with RND_20_2 and RND_20_6, the Value Chain category offers the weakest conceptual mapping—API version divergence could be framed as a component integration risk. However, NIST AI 600-1 provides no controls for canary-induced API version divergence in multi-agent pipelines, no guidance on traffic routing policies, and no requirements for API backward compatibility during deployments. The framework's coverage is negligible.
**Score: 1**

---

## RND_21 - Inference Hardware and Optimization

### RND_21_1
Quantization granularity differences across agents—per-tensor versus per-channel versus per-token modes producing different numerical outputs from the same input—are hardware-level inference optimization concerns. NIST AI 600-1 contains no guidance on model quantization strategies, numerical precision management, or the behavioral divergence that results from heterogeneous quantization configurations in multi-agent deployments. MEASURE 2.6 and 2.7 address safety and security evaluation but at the model behavior level, not at the level of low-level numerical inference configuration. The framework provides no applicable coverage for quantization-induced non-determinism.
**Score: 1**

### RND_21_2
GPU scheduler variance affecting kernel fusion execution order, and the resulting non-determinism across tensor-parallel multi-agent deployments on different GPUs, is a hardware and low-level compute infrastructure concern entirely outside NIST AI 600-1's scope. The profile addresses organizational governance and GenAI model risks; GPU scheduler behavior, kernel fusion ordering, and CUDA execution non-determinism are below the abstraction level at which any NIST AI 600-1 control operates. No mapping exists between the framework's 12 risk categories and hardware-level inference execution variance.
**Score: 1**

### RND_21_3
NIM replica floating-point rounding differences across GPU architectures producing divergent token probability distributions—and the resulting attack surface where adversaries craft prompts that activate only under specific probability configurations—combines hardware-level non-determinism with an adversarial exploitation vector. NIST AI 600-1's Information Security category (§2.9) recognizes adversarial prompt-based attacks and MEASURE 2.7 calls for security red-teaming, but neither addresses probability-distribution attacks that depend on GPU-architecture-specific floating-point behavior. Pre-deployment testing cannot capture hardware-specific rounding variance that only manifests across heterogeneous production replicas. Coverage is negligible.
**Score: 1**

### RND_21_4
Triton dynamic batching non-determinism arising from request arrival timing—affecting agent behavior reproducibility in multi-agent systems—is an inference serving infrastructure concern. NIST AI 600-1 does not address inference serving frameworks, dynamic batching policies, or the non-determinism they introduce into agent outputs. MEASURE 2.7 pre-deployment testing guidance does not extend to batching-induced behavioral variance at the inference serving layer. No NIST AI 600-1 control maps to this infrastructure-level concern.
**Score: 1**

### RND_21_5
Continuous model update systems creating perpetual assurance gaps—where zero-downtime version transitions in multi-agent deployments prevent formal verification from completing before the next update cycle—combine deployment infrastructure concerns with governance-level assurance challenges. NIST AI 600-1's MEASURE 2.6 and 2.7 call for pre-deployment safety and security evaluation, which is directly undermined by continuous update cadences that make pre-deployment testing perpetually incomplete. GOVERN 6.1/6.2 could establish policies requiring update gates and re-verification windows. However, the framework provides no specific guidance on continuous deployment assurance gaps, multi-agent update synchronization, or verification cadence requirements. The governance framing offers weak but non-trivial relevance for the assurance policy dimension.
**Score: 1**

### RND_21_6
TensorRT engine GPU-architecture specificity—where engines compiled for development hardware differ from production hardware, creating deployment assurance gaps—is a hardware-specific inference optimization infrastructure concern. NIST AI 600-1's pre-deployment testing guidance (MEASURE 2.7) implicitly assumes testing environments are representative of production, but the framework provides no guidance on hardware parity requirements between test and production environments or engine portability validation. The Value Chain category (§2.12) could marginally frame GPU-specific engine compilation as a component integration risk. Coverage is negligible to minimal.
**Score: 1**

---

## RND_22 - Optimization and Efficiency

### RND_22_1
Profiling-induced measurement overhead creating non-determinism—and multi-agent optimization decisions based on profiled data producing distributed behavioral inconsistency—is an operational instrumentation and performance engineering concern. NIST AI 600-1 does not address profiling overhead, performance measurement methodologies, or the non-determinism introduced by observability instrumentation. MEASURE 2.7 covers security evaluation but from a model behavior and adversarial testing perspective, not infrastructure profiling. The framework provides no applicable coverage for profiling-induced non-determinism as an assurance concern.
**Score: 1**

### RND_22_2
Temperature and sampling parameter variability across optimization cycles—where different agents receive different optimized parameters creating inconsistent generation behavior—is an operational configuration management concern with a secondary governance dimension. NIST AI 600-1 does not address sampling parameter management, temperature optimization pipelines, or configuration drift across multi-agent deployments. MEASURE 2.7 pre-deployment security evaluation could in principle include parameter configuration audits, but the framework provides no guidance specific to sampling parameter governance or the behavioral divergence caused by heterogeneous optimization cycles. Coverage is negligible.
**Score: 1**

### RND_22_3
Asynchronous configuration updates creating split-brain states where Agent A operates on old parameters while Agent B operates on new—is a distributed configuration management and operational coordination problem. NIST AI 600-1 has no guidance on configuration synchronization protocols, update propagation ordering, or the consistency failures that arise during rolling configuration changes in multi-agent systems. While MANAGE 4.1–4.3 covers incident response to detected failures, it provides no preventive controls for configuration inconsistency windows. The framework offers no applicable coverage.
**Score: 1**

### RND_22_4
Speculative decoding acceptance rate variability creating non-deterministic latency and output distributions not detectable with fixed test sets is an inference optimization and testing methodology concern. NIST AI 600-1's MEASURE 2.7 pre-deployment testing mandate is marginally relevant in that it requires security evaluation—and if test methodology failures include failure to account for speculative decoding variance, that is a testing design weakness within the spirit of MEASURE 2.7. However, the framework provides no guidance on speculative decoding, acceptance rate variability, or adaptive test coverage strategies for stochastic inference optimization. The relevance is minimal and indirect.
**Score: 1**

---

## RND_23 - Chain-of-Thought (CoT), Tree-of-Thought (ToT), and Reasoning Non-Determinism

### RND_23_1
CoT path explosion in multi-agent coordination—where N agents each generating different chains produce exponential execution path combinations making red-teaming coverage computationally infeasible—sits outside NIST AI 600-1's scope. MEASURE 2.7 calls for red-teaming and adversarial testing before deployment, and is nominally relevant in that path explosion constitutes a testing methodology failure where coverage cannot be achieved. However, the framework provides no guidance on reasoning process coverage, CoT path enumeration, or multi-agent combinatorial test space management. The pre-deployment testing mandate acknowledges a process that fails here but offers no solution to the exponential coverage problem.
**Score: 1**

### RND_23_2
Reasoning consistency drift across individually updated agents—where a poisoning attack pattern that succeeds against older agents fails against updated ones—involves a data poisoning vector that NIST AI 600-1's Information Security category (§2.9) explicitly recognizes. MEASURE 2.7 security evaluation and red-teaming is relevant to detecting poisoning-induced reasoning drift before deployment. However, the differential update timing that causes some agents to remain vulnerable while others are patched is an operational deployment coordination problem the framework does not address. The poisoning recognition in §2.9 provides weak but genuine relevance; the multi-agent differential patching dimension falls outside scope.
**Score: 1**

### RND_23_3
Temporal coordination non-determinism where agents sharing CoT traces create race conditions—resulting in inconsistent reasoning states across the multi-agent system—is a distributed systems coordination and reasoning architecture problem. NIST AI 600-1 does not address shared reasoning trace architectures, inter-agent coordination protocols, or the race conditions that emerge from concurrent CoT access. The framework's Information Security category recognizes prompt injection but not reasoning trace contamination via concurrent access races. No NIST AI 600-1 control applies to this reasoning infrastructure concern.
**Score: 1**

### RND_23_4
Stochastic multi-agent planning where each agent's reasoning choices combine multiplicatively to create a test space that is computationally infeasible to cover is fundamentally a testing methodology limitation for stochastic systems. MEASURE 2.7 mandates pre-deployment testing and security evaluation, establishing the expectation that coverage should occur—and the infeasibility of that coverage is the core problem described here. However, NIST provides no guidance on probabilistic coverage strategies, Monte Carlo testing approaches for reasoning systems, or acceptable coverage thresholds for stochastic multi-agent planning. The testing mandate is relevant context but the framework offers no tools to address the infeasibility.
**Score: 1**

### RND_23_5
Backtracking state divergence in distributed Tree-of-Thought systems—where timing differences cause agents to disagree on which branches have been explored—is a distributed reasoning coordination and state synchronization problem. NIST AI 600-1 does not address ToT architectures, distributed reasoning tree state management, or the divergence that results from asynchronous backtracking across agents. The framework contains no controls applicable to reasoning process state consistency. This falls entirely outside the governance and model-risk scope of NIST AI 600-1.
**Score: 1**

---

## RND_24 - Self-Consistency and Multiple Reasoning Paths

### RND_24_1
Self-Consistency's stochastic sampling generating k different reasoning paths creates a test coverage gap that MEASURE 2.7's pre-deployment testing mandate is structurally relevant to—the inability to comprehensively test stochastic behavior is a testing methodology failure within the spirit of what pre-deployment evaluation should address. However, NIST AI 600-1 provides no guidance on stochastic coverage strategies, minimum k-path sampling requirements for test suites, or acceptable behavioral variance bounds across sampled reasoning paths. The framework acknowledges that evaluation should occur but offers no tools for addressing stochastic assurance gaps. Coverage is minimal.
**Score: 1**

### RND_24_2
Self-Consistency temperature parameter drift—where parameters tuned for specific problem classes cause behavioral inconsistency when applied to out-of-distribution inputs, affecting all agents simultaneously when shared—is an operational configuration management concern. NIST AI 600-1 does not address temperature tuning methodologies, problem-class-specific parameter governance, or the systemic failure mode that arises when shared parameter drift affects entire multi-agent deployments simultaneously. MEASURE 2.7 pre-deployment evaluation could in principle include temperature sensitivity testing, but the framework provides no direction for this. Coverage is negligible.
**Score: 1**

### RND_24_3
Continuous sampling parameter optimization causing systematic behavioral drift across shared multi-agent configurations is an operational parameter management concern. As with RND_24_2, NIST AI 600-1 provides no guidance on continuous optimization pipelines, parameter drift monitoring, or the adversarial exploitation of systematic drift where attackers anticipate parameter evolution trajectories. MEASURE 2.7 security evaluation before deployment does not extend to continuously optimizing runtime parameters. The framework offers no applicable coverage for adversarial drift exploitation via shared parameter optimization.
**Score: 1**

### RND_24_4
Context window compression non-determinism in Self-Consistency with k=40 paths—where compression algorithms non-deterministically select which information survives, affecting reasoning outputs—is an inference-time context management concern. NIST AI 600-1 does not address context window management, compression algorithm behavior, or the non-determinism introduced by lossy context reduction in multi-path reasoning. MEASURE 2.7 pre-deployment testing could theoretically probe compression-sensitive reasoning, but the framework provides no guidance on this. The framework offers no applicable coverage for compression-induced reasoning non-determinism.
**Score: 1**
# NIST AI 600-1 Coverage Analysis: RND_25 through RND_30

---

## RND_25 - Hierarchical Task Network (HTN) Planning Non-Determinism

### RND_25_1
NIST AI 600-1 MEASURE 2.7 mandates pre-deployment security evaluation and red-teaming, which could in principle probe LLM-based HTN planners for unsafe method selections, but stochastic method selection means any finite test suite samples only a fraction of the reachable decomposition space. The profile has no concept of probabilistic planning assurance, method selection distributions, or the compound probability problem whereby multi-agent path probability collapses to values like 0.336 that render reliable safety guarantees mathematically unattainable. MEASURE 2.9 model explanation provisions nominally address understanding model outputs but are designed for post-hoc interpretability, not for bounding stochastic planning choices across deployments. NIST's governance framework assumes that pre-deployment testing produces durable safety assurances, an assumption that HTN non-determinism structurally violates.
**Score: 1**

### RND_25_2
Temperature and sampling parameter configuration falls outside NIST AI 600-1's governance scope; the framework does not address hyperparameter standardization, inter-agent configuration consistency, or the divergent planning behaviors that emerge when agents operate at different temperatures. MEASURE 2.7 red-teaming could test individual agents at their configured temperatures but provides no mechanism for detecting or preventing divergent decompositions when Agent A at temperature 0.7 and Agent B at temperature 1.2 operate within the same multi-agent workflow. The profile's supply chain provisions (GOVERN 6.1/6.2) address third-party model vetting but not internal configuration governance for sampling parameters. This is a runtime configuration and orchestration problem with no applicable NIST control.
**Score: 1**

### RND_25_3
GOVERN 6.1/6.2 supply chain provisions provide a thin governance hook for requiring that method library versions be tracked and that updates follow a documented change management process, which partially addresses the asynchronous library evolution problem. However, NIST has no controls for detecting or mitigating the mid-deployment state where Agent A operates against library v1.0 and Agent B against v1.1, producing incompatible decompositions assuming different method availability. MEASURE 2.7 pre-deployment testing validates a fixed library version at a single point in time and cannot account for asynchronous update propagation across a live multi-agent fleet. The version skew problem is fundamentally an infrastructure and dependency management concern outside the framework's governance focus.
**Score: 1**

### RND_25_4
NIST AI 600-1 MEASURE 2.7 security evaluation and MEASURE 2.9 model validation provisions nominally motivate reproducibility as a desirable property for AI system auditing, providing a weak conceptual connection to the decomposition reproducibility problem. However, the profile contains no requirements for deterministic or reproducible planning, no guidance on seed management or inference determinism, and no recognition that multi-agent systems lacking reproducibility guarantees create audit and assurance gaps that governance documentation cannot close. The framework's incident response provisions (MANAGE 4.1–4.3) assume that incidents can be investigated and reproduced, an assumption that non-reproducible HTN planning structurally undermines. NIST provides no actionable control for this threat.
**Score: 1**

### RND_25_5
Partial order scheduling non-determinism in distributed HTN execution is a runtime planning and coordination problem that NIST AI 600-1 does not address at any level of technical specificity. The profile has no provisions for execution ordering guarantees, distributed constraint resolution consistency, or the security implications of non-deterministic sequencing in multi-agent task execution. MEASURE 2.7 red-teaming could test specific orderings pre-deployment, but the combinatorial space of valid partial orderings makes exhaustive coverage infeasible, and the profile provides no guidance on how to prioritize or bound this space. The threat is a runtime orchestration concern entirely outside NIST's governance scope.
**Score: 1**

### RND_25_6
NIST AI 600-1 does not address constraint satisfaction solver behavior, heuristic selection, or the divergent solutions that emerge when agents separately solve identical constraint problems with non-deterministic heuristics. MEASURE 2.7 pre-deployment testing could validate constraint solutions against a fixed heuristic configuration but cannot enumerate the solution space reachable under non-deterministic heuristic selection. The separation of constraint solving and execution across different agents—enabling unexpected solutions to drive actions the solving agent never intended—is a multi-agent coordination architecture problem with no corresponding NIST control. The framework's governance provisions assume that organizational policies can constrain AI behavior, but non-deterministic constraint satisfaction operates below the level where policy intervention is feasible.
**Score: 1**

---

## RND_26 - Monte Carlo Tree Search (MCTS) Planning Non-Determinism

### RND_26_1
NIST AI 600-1 MEASURE 2.7 red-teaming provisions could test MCTS-based planners against specific random seeds pre-deployment, but this provides no assurance that production deployments with different seeds will produce the same task orderings or that divergent orderings across agents are safe. The framework has no concept of search tree non-determinism, seed management policy, or the multi-agent convergence problem where Agent A plans "X then Y" and Agent B plans "Y then X" from the same goal. MEASURE 2.9 model explanation provisions could nominally require that planning decisions be explainable, but MCTS non-determinism means the same input does not reliably produce the same explanation across runs. NIST provides no applicable control for MCTS planning non-determinism.
**Score: 1**

### RND_26_2
Adaptive MCTS exploration constant tuning is a runtime algorithmic behavior entirely outside NIST AI 600-1's governance scope; the profile contains no provisions for algorithm hyperparameter governance, adaptive parameter control, or the heterogeneous exploration that results when different agents tune the exploration constant C differently during live operation. MEASURE 2.7 security evaluation tests a fixed configuration and cannot assess the emergent risk of dynamically heterogeneous search strategies across a multi-agent fleet. The ability for adversaries to exploit inter-agent exploration heterogeneity—steering one agent toward high-variance branches while another plans conservatively—is a novel attack surface that NIST's risk categories do not recognize. This threat is out of scope.
**Score: 1**

### RND_26_3
NIST AI 600-1 MEASURE 2.6 safety evaluation provisions could motivate requirements that MCTS planners operate with sufficient simulation budgets to produce reliable plans, providing a weak pre-deployment hook. However, the profile has no concept of computational budget allocation, resource contention in multi-agent systems, or the assurance gap that opens when production resource competition forces budgets below the levels validated during testing. MANAGE 4.1–4.3 incident response provisions address post-detection response but cannot prevent or detect the degraded planning quality that results from budget starvation. The runtime resource allocation problem is an infrastructure concern outside NIST's governance framework.
**Score: 1**

### RND_26_4
NIST AI 600-1 §2.9 Information Security acknowledges adversarial manipulation of AI system behavior, which provides a thin conceptual connection to adversarially forced replanning as an attack vector. MEASURE 2.7 red-teaming could probe replanning behaviors by simulating failure states, but the non-determinism introduced by different random seeds across replanning events means testing one recovery path provides no guarantee that production replanning will follow the same path. The deliberate attacker strategy of forcing replanning events where specific non-deterministic paths execute dangerous operations is not a recognized attack pattern in NIST's risk categories. Coverage is minimal.
**Score: 1**

### RND_26_5
Asynchronous replanning state divergence is a distributed systems consistency problem that NIST AI 600-1 does not address; the framework has no provisions for inter-agent state synchronization, replanning coordination protocols, or the security implications of agents operating from divergent state snapshots during concurrent replanning events. MANAGE 4.1–4.3 incident response assumes that incidents are detectable and bounded, but state divergence from asynchronous replanning can produce incorrect behaviors that are difficult to attribute to a specific causal event. The attacker exploitation of replanning asynchrony to cause state desynchronization is an operational multi-agent coordination attack with no corresponding NIST control. This is an infrastructure and runtime orchestration concern.
**Score: 1**

### RND_26_6
NIST AI 600-1 does not address heuristic evaluation consistency in search algorithms; the framework's risk categories cover AI outputs and organizational governance but not the internal algorithmic properties of planning systems. MEASURE 2.7 red-teaming could test A* search behavior under specific heuristic configurations, but divergent local heuristic estimates across agents exploring different search spaces is an algorithmic property that pre-deployment testing of individual agents cannot surface. The resulting inconsistent joint plans from agents that cannot agree on which search paths are promising creates coordination failures with no NIST control applicable. This threat is out of scope.
**Score: 1**

---

## RND_27 - Episode-Based Memory and Consolidation

### RND_27_1
NIST AI 600-1 §2.9 Information Security explicitly names data poisoning as a recognized risk, and §2.2 Confabulation addresses the generation of inaccurate or fabricated content, both of which provide moderate coverage for adversarially crafted episodes designed to manipulate retrieval ranking. MEASURE 2.7 red-teaming mandates can be applied to test episodic memory systems for poisoned episode retrieval under adversarial conditions. However, the non-deterministic nature of retrieval ranking—where malicious episodes with intermediate scores retrieve unpredictably based on weighted scoring across similarity, recency, and importance—means that pre-deployment testing cannot exhaustively map the attack surface. The cascade failure risk in multi-agent systems where one agent's poisoned retrieval propagates to dependent agents is a recognized but inadequately addressed concern under the profile's supply chain provisions.
**Score: 2**

### RND_27_2
NIST AI 600-1 §2.9 data poisoning provisions provide partial coverage for episodic memory consolidation attacks, as consolidation merges retrieved episodes into persistent memory in a manner analogous to training data manipulation. MEASURE 2.7 red-teaming could probe consolidation behavior during normal operating windows, but the threat specifically involves timing attacks where malicious episodes consolidate during windows when monitoring agents are absent—a runtime operational condition that pre-deployment testing cannot simulate. MANAGE 4.1–4.3 incident response provisions assume that attacks are detectable, but consolidation timing exploitation that targets monitoring gaps by design is intended to evade detection. The profile has no provisions for continuous memory integrity monitoring or consolidation timing governance.
**Score: 2**

### RND_27_3
NIST AI 600-1 §2.9 acknowledges prompt injection and data poisoning as information security risks, providing a conceptual basis for recognizing that poisoned episodes designed to retrieve only under specific contexts represent a targeted manipulation of AI memory systems. MEASURE 2.7 security evaluation mandates pre-deployment testing that could probe retrieval threshold sensitivity, but context-conditional poisoned episodes that evade detection by remaining below retrieval thresholds during testing represent an assurance gap that bounded test suites cannot close. The profile has no provisions for adversarial threshold probing, retrieval distribution analysis, or continuous monitoring of episode retrieval patterns that would surface context-specific poisoning. Coverage is indirect and pre-deployment only.
**Score: 2**

### RND_27_4
NIST AI 600-1 §2.9 data poisoning recognition provides a weak connection to trajectory-length-conditioned attack episodes, as these represent a form of adversarial input manipulation designed to activate malicious behavior under specific execution conditions. However, NIST has no provisions for trajectory length monitoring, long-execution monitoring fatigue, or the specific attack pattern where malicious episode steps activate only after extended trajectories exhaust monitoring resources. MEASURE 2.7 red-teaming typically operates over bounded test trajectories and cannot reliably surface activation conditions that only trigger after unusually long execution sequences. The threat exploits a runtime operational condition that governance-level provisions cannot address.
**Score: 1**

### RND_27_5
NIST AI 600-1 GOVERN 6.1/6.2 supply chain provisions address third-party component vetting, which provides indirect coverage for embedding model version management as part of AI component provenance tracking. §2.9 data poisoning recognition is relevant to the transient poisoning windows created by embedding model updates, where previously unretrieval malicious content suddenly becomes retrievable. However, NIST has no controls for detecting or mitigating differential version attacks where adversaries exploit agents running different embedding model versions, nor for the transient retrieval instability that accompanies model updates. The pre-deployment focus of MEASURE 2.7 testing means that post-deployment embedding model updates fall outside the testing regime entirely.
**Score: 2**

### RND_27_6
NIST AI 600-1 §2.9 Information Security and data poisoning provisions provide a weak connection to poisoned knowledge graph relationship attacks, as injecting malicious relationships into graph stores is a form of data manipulation targeting AI memory systems. MEASURE 2.7 red-teaming mandates could test graph traversal behavior for known poisoned relationships pre-deployment. However, graph traversal path non-determinism in distributed graph stores—where poisoned relationships activate only on specific traversal orderings—means that testing coverage of traversal paths is bounded by test suite size and cannot guarantee safety across all production traversal sequences. The profile has no provisions for distributed graph consistency monitoring or traversal path integrity validation at runtime.
**Score: 2**

---

## RND_28 - Knowledge Graphs and Memory Stores

### RND_28_1
NIST AI 600-1 §2.9 Information Security names data poisoning and indirect prompt injection as recognized risks applicable to RAG systems, providing moderate pre-deployment coverage for attacks that exploit score rounding to manipulate document ranking. MEASURE 2.7 red-teaming can probe RAG ranking behavior for specific documents near similarity score boundaries. However, score rounding creating non-deterministic ranking boundaries is a numerical precision property of retrieval systems that manifests stochastically across queries, meaning pre-deployment tests covering specific score ranges cannot guarantee production ranking consistency. The correlated non-determinism risk in shared-index multi-agent systems—where all agents simultaneously receive ranking instability for the same boundary documents—is an emergent architectural risk with no NIST control.
**Score: 2**

### RND_28_2
NIST AI 600-1 §2.9 data poisoning provisions and GOVERN 6.1/6.2 supply chain vetting apply indirectly to knowledge graph integrity, as maintaining trustworthy relationship data in graph stores is analogous to ensuring training data provenance. MEASURE 2.7 red-teaming could test graph traversal reasoning paths for specific relationship configurations, but traversal ordering non-determinism means different executions produce different reasoning paths from identical starting points, making pre-deployment testing of specific paths non-transferable to production guarantees. The profile has no provisions for graph traversal determinism, relationship visit ordering governance, or runtime monitoring of reasoning path consistency across agents. Coverage is weak and indirect.
**Score: 2**

### RND_28_3
NIST AI 600-1 §2.9 Information Security provisions nominally cover temporal data validity concerns insofar as outdated or manipulated documents represent a form of information integrity risk. MEASURE 2.6 safety evaluation could assess system behavior at validity window boundaries in controlled testing, providing minimal pre-deployment coverage. However, temporal flickering—where documents intermittently appear and disappear from retrieval based on clock-precision validity boundaries—is a runtime temporal consistency problem that governance mandates cannot prevent or detect. The resulting assurance gaps, where safety-critical documents are absent from context at exact validation-window edges, require runtime temporal monitoring that NIST does not prescribe.
**Score: 1**

### RND_28_4
NIST AI 600-1 §2.9 data poisoning recognition provides moderate indirect coverage for probabilistic fact manipulation in knowledge graphs, as adversarially adjusting fact confidence scores to exploit evaluation variance is a form of data manipulation targeting AI reasoning inputs. MEASURE 2.7 red-teaming can test probabilistic fact handling under specific score configurations. However, the non-deterministic evaluation of facts with scores near confidence thresholds means production behavior cannot be reliably predicted from pre-deployment tests, and attacker leverage of score variance to cause inconsistent fact acceptance across agents is not a recognized attack pattern in the profile's risk categories. The profile has no provisions for probabilistic knowledge graph integrity monitoring.
**Score: 2**

### RND_28_5
NIST AI 600-1 §2.9 Information Security and §2.12 Value Chain provisions provide indirect coverage for incremental knowledge graph update consistency, as ensuring that AI system inputs reflect authoritative and complete information is a stated concern of the profile. GOVERN 6.1/6.2 supply chain provisions can nominally require that knowledge base update processes maintain consistency guarantees. However, agents querying during live updates and retrieving partially-updated data—creating divergent knowledge snapshots across a multi-agent fleet—is a distributed database consistency problem that governance mandates cannot solve at the technical level required. NIST has no controls for update isolation, read consistency during writes, or detection of divergent snapshot states across agents.
**Score: 2**

### RND_28_6
NIST AI 600-1 does not address caching architecture, cache invalidation protocols, or the semantic divergence that results when eventually consistent caches expose agents to temporarily different views of the same knowledge store. The profile's §2.9 information security provisions could conceptually encompass stale cache exploitation as an information integrity risk, but cache invalidation timing is an infrastructure-layer consistency property outside the scope of governance-level risk management. MANAGE 4.1–4.3 incident response assumes that behavioral anomalies are detectable, but cache-induced semantic divergence that self-resolves after invalidation propagates may be too transient to surface through incident reporting. No applicable NIST control exists for this threat.
**Score: 1**

---

## RND_29 - Context Assembly and Working Memory

### RND_29_1
NIST AI 600-1 §2.9 Information Security recognizes indirect prompt injection and adversarial input manipulation as security risks, providing moderate coverage for the context assembly non-determinism problem insofar as variable assembly ordering can cause security-critical context to lose positional prominence or be excluded from attention-weighted processing. MEASURE 2.7 red-teaming mandates pre-deployment testing that could probe context assembly for specific injection scenarios. However, non-deterministic retrieval ranking and asynchronous retrieval in multi-agent systems mean that assembly order varies across executions in ways that bounded pre-deployment tests cannot exhaustively characterize, creating audit blind spots where reproducible audit replay is impossible. The profile has no provisions for context assembly determinism, ordering consistency requirements, or runtime assembly monitoring.
**Score: 2**

### RND_29_2
NIST AI 600-1 §2.9 indirect prompt injection recognition provides a weak connection to streaming generation non-determinism, as malicious instructions hidden in alternative token sequences represent a form of adversarial content manipulation that evades pre-deployment testing. MEASURE 2.7 red-teaming can test specific token sequences but cannot enumerate the stochastic token sequence distribution reachable under variable emission rates and sampling conditions. The profile has no provisions for streaming pipeline security, inter-agent partial-output safety validation, or the specific threat of security-critical content being untestable because it only manifests in token sequences not reproducible under controlled conditions. This threat is primarily an infrastructure and runtime concern minimally addressed by NIST.
**Score: 1**

### RND_29_3
NIST AI 600-1 GOVERN 6.1/6.2 supply chain provisions provide indirect coverage for continual update propagation consistency, as ensuring that AI system components receive updates in a controlled and documented manner is a third-party and internal change management concern. MEASURE 2.7 security evaluation mandates regression testing that could detect version boundary vulnerabilities, providing pre-deployment coverage for known update transitions. However, asynchronous propagation of updates across a multi-agent fleet creates distributed regression windows where attackers can probe version boundaries in live systems—a dynamic attack surface that pre-deployment testing of individual update transitions cannot characterize. The profile has no provisions for coordinated update rollout, version synchronization monitoring, or live version boundary attack detection.
**Score: 2**

---

## RND_30 - Utility and Decision Making

### RND_30_1
NIST AI 600-1 does not address Monte Carlo sampling for utility estimation, expected utility calculation variance, or the decision-making non-determinism that results when agents sample different utility estimates for identical decision scenarios. MEASURE 2.7 security evaluation mandates pre-deployment testing of AI decision-making behavior, which could in principle test utility-based decisions under specific sampling conditions, but sampling variance means pre-deployment tests cannot bound production decision distributions. The multi-agent divergent decision problem—where Agent A's sampled utility differs sufficiently from Agent B's to produce incompatible action selections—is an emergent algorithmic risk with no recognized NIST risk category. The framework has no applicable control.
**Score: 1**

### RND_30_2
NIST AI 600-1 MEASURE 2.7 red-teaming and MANAGE 4.1–4.3 incident response provisions are the closest applicable controls for stale utility cache exploitation, as detecting and responding to degraded decision quality from outdated utility values is nominally an incident management concern. However, the profile has no provisions for utility cache validity monitoring, stale-value detection, or the cascade degradation pattern where one agent's reliance on stale utilities propagates through dependent downstream agents. The outcome distribution drift that silently invalidates cached utility calculations while agents continue operating on stale values is a runtime monitoring problem entirely outside NIST's pre-deployment governance scope.
**Score: 1**

### RND_30_3
NIST AI 600-1 §2.9 Information Security and MEASURE 2.7 security evaluation provisions provide a weak connection to adversarial manipulation of utility weight adaptation, as continuously adjusting weights in response to adversarial inputs is a form of data poisoning targeting the AI system's decision-making parameters. However, NIST has no provisions for utility function stability monitoring, weight adjustment governance, or the specific multi-agent convergence risk where agents develop incompatible utility functions through independent continuous adaptation. The unsafe convergence problem—where legitimate adaptation processes produce harmful weight configurations—is not a recognized risk pattern in NIST's 12 risk categories.
**Score: 1**

### RND_30_4
NIST AI 600-1 MEASURE 2.7 security evaluation and MEASURE 2.9 model validation provisions nominally address the reproducibility of AI decision-making by mandating pre-deployment testing and explanation, providing a weak framework-level recognition that AI decisions should be understandable and consistent. However, the evaluation non-reproducibility problem—where pre-deployment testing establishes "safe" decision distributions that fail to generalize to production input distributions—is a fundamental statistical generalization problem that governance mandates cannot solve through additional testing. The profile has no provisions for distribution shift monitoring, utility decision audit trail requirements, or detection of unsafe decisions triggered by inputs that were safe-classified during evaluation. NIST's pre-deployment testing model structurally cannot address this threat.
**Score: 1**
# NIST AI 600-1 Coverage Analysis: RND_31 through RND_34

---

## RND_31 - Rule-Based and Adaptive Systems

### RND_31_1
NIST AI 600-1 operates at a governance and organizational risk management level and has no provisions addressing rule priority configurations or the non-determinism that emerges when adaptive priorities shift across multi-agent shared rule engines. MEASURE 2.7's pre-deployment testing mandate provides the weakest possible hook—an evaluator could in principle run the system multiple times observing output divergence—but the framework offers no methodology for validating rule priority stability or bounding divergence across execution runs. The profile's 12 risk categories contain no entry for rule-based system operational behavior, and its governance actions (policy, red-teaming, incident disclosure) operate at layers entirely above runtime rule scheduler mechanics. This is an operational and architectural concern that is effectively out of scope.
**Score: 1**

### RND_31_2
Learning rule instability in adaptive systems—where continuously refined rules create non-deterministic multi-agent behavior—is an operational machine learning concern outside NIST AI 600-1's governance scope. The profile has no provisions for shared rule learning pipelines, no requirements for rule versioning or change-control in deployed systems, and no concept of rule drift as a security-relevant failure mode. MEASURE 2.7's security evaluation mandate applies pre-deployment and cannot account for the ongoing rule evolution that occurs during production operation. MANAGE 4.1–4.3 covers incident response after anomalous behavior is detected but provides no preventive controls for rule instability. This threat is out of scope for the framework.
**Score: 1**

### RND_31_3
Heuristic parameter drift from independent per-agent tuning creating multi-agent non-determinism is an operational configuration management problem that NIST AI 600-1 does not address. The profile has no provisions for parameter consistency across distributed agent deployments, no requirements for synchronized tuning governance across agent instances, and no risk category that maps to divergent heuristic parameterization as a security concern. MEASURE 2.7's pre-deployment testing evaluates agents at a static configuration point and cannot detect drift that accumulates through independent tuning cycles post-deployment. This is an infrastructure and operational concern outside the framework's intended scope.
**Score: 1**

---

## RND_32 - Learning-Based Policies and Reinforcement Learning

### RND_32_1
NIST AI 600-1 has no coverage of reinforcement learning, learned policy architectures, or the stochastic components (softmax action selection, dropout, temperature) that produce policy non-determinism. The framework's MEASURE category addresses pre-deployment testing and explanation of model behavior, but these provisions were designed for generative AI outputs—not for RL policy action distributions across state spaces. Compounding non-determinism across multi-agent RL policies creating exponential behavior variance represents an assurance gap that governance documentation and point-in-time red-teaming cannot close, since the profile provides no methodology for bounding policy variance or validating action selection stability. RL training and deployment are entirely outside the framework's scope.
**Score: 1**

### RND_32_2
Online learning systems that improve during deployment continuously invalidate any pre-deployment safety assurances—a fundamental challenge that NIST AI 600-1's static assurance model cannot address. MEASURE 2.7 mandates pre-deployment evaluation and red-teaming, but the profile has no provisions for post-deployment assurance refresh, continuous monitoring of policy drift, or trigger conditions requiring re-evaluation when online learning has materially changed system behavior. The faster change propagation created by multi-agent online learning compounds this gap. NIST's governance framework assumes a relatively stable deployed system and does not acknowledge or address the assurance lifecycle problem created by systems that learn continuously in production.
**Score: 1**

### RND_32_3
Exploration behavior maintained in deployment as an attack surface—where learning systems execute stochastic exploration actions that attackers can exploit to trigger unpredictable sequences—is an RL-specific operational risk that NIST AI 600-1 has no category to address. The profile does not distinguish between exploitation and exploration phases in deployed systems, has no requirements for disabling or bounding exploration in production environments, and provides no guidance on managing the security implications of exploration-induced unpredictability. Coordinated multi-agent exploration creating correlated unpredictability is a distributed RL behavior pattern entirely outside the framework's 12 risk categories and four primary considerations.
**Score: 1**

### RND_32_4
Experience replay temporal non-determinism—where DRL agents sample from replay buffers with temporal gaps and shared buffers propagate this non-determinism across agents—is a deep reinforcement learning infrastructure concern that NIST AI 600-1 does not address. The profile has no concept of replay buffer architecture, no requirements for replay sampling consistency, and no risk category mapping to temporal experience gaps as a security-relevant failure mode. MEASURE 2.7's pre-deployment testing cannot surface the non-determinism created by shared replay buffers under production concurrent access patterns. This is an RL training and inference infrastructure concern entirely outside the framework's scope.
**Score: 1**

### RND_32_5
Multi-agent RL emergent behavior unpredictability—where MARL system interactions produce collective behaviors not present in any individual agent's policy—is a fundamental challenge that NIST AI 600-1 does not address. The profile's MEASURE category mandates pre-deployment testing but provides no guidance on testing methodologies for emergent MARL behaviors, no recognition that exhaustive combination testing is computationally infeasible, and no alternative assurance strategies (compositional verification, formal methods, statistical coverage bounds) for when exhaustive testing is impossible. The framework's assurance model implicitly assumes testable bounded systems and has no provisions for the intractability created by multi-agent emergent behavior spaces.
**Score: 1**

### RND_32_6
Policy network weight sensitivity to training details—where identical architectures trained with different hyperparameters, data orderings, or hardware produce meaningfully different runtime behaviors—is an ML training infrastructure concern that NIST AI 600-1 does not address. Distributed training producing heterogeneous policies across agent instances is an operational consistency problem with no corresponding NIST governance control. MEASURE 2.9's model explanation and validation provisions are the only tangential hook, but the profile does not extend these to inter-run training reproducibility requirements or distributed policy consistency validation. This is an ML operations concern outside the framework's scope.
**Score: 1**

### RND_32_7
Gradient-based adversarial policy perturbations—where small input modifications cause large learned policy behavior changes—represent an adversarial ML attack vector that NIST AI 600-1 does not specifically address. §2.9 (Information Security) names adversarial inputs as a recognized risk and MEASURE 2.7 mandates security evaluation including red-teaming, providing a thin governance hook for including adversarial perturbation testing in evaluation scope. However, the synchronized gradient vulnerabilities across multiple agents enabling single perturbations to affect entire agent fleets is a multi-agent RL-specific attack pattern with no corresponding NIST control or evaluation methodology. The profile's acknowledgment of adversarial inputs is generic and does not extend to gradient-based attack methodologies or multi-agent synchronization exploitation.
**Score: 1**

---

## RND_33 - Hybrid and Heterogeneous Systems

### RND_33_1
Temperature heterogeneity in hybrid paradigm processing—where attackers craft payloads exploiting different temperature configurations across deterministic and stochastic agents—is an inference-layer configuration security concern that NIST AI 600-1 does not address. The profile has no provisions for temperature configuration management, no requirements for consistency of sampling parameters across hybrid agent architectures, and no recognition that temperature heterogeneity creates an exploitable attack surface differential. MEASURE 2.7's security evaluation mandate provides only the most minimal hook, as pre-deployment red-teaming could in principle probe whether temperature-targeted payloads succeed against specific agents, but the profile provides no guidance on this. This is an operational configuration and adversarial exploitation concern outside the framework's scope.
**Score: 1**

### RND_33_2
Paradigm-specific non-determinism in cooperative cycles—where non-deterministic cycle counts in hybrid cooperative architectures create delayed injection activation windows—is a runtime architectural behavior that NIST AI 600-1 does not address. The profile's pre-deployment testing mandate (MEASURE 2.6, 2.7) could nominally test for injection behaviors across a range of cycle counts, but the non-deterministic nature of cycle termination means exhaustive pre-deployment testing cannot bound the attack surface. Attackers crafting injections that activate only after specific cycle numbers exploit a timing dimension that governance documentation and organizational policies cannot control. This is a runtime architectural security concern outside the framework's scope.
**Score: 1**

### RND_33_3
Streaming response non-determinism in hybrid output synthesis—where non-deterministic streaming order across multiple paradigms creates exploitable instruction-ordering windows—is an infrastructure-layer streaming behavior that NIST AI 600-1 does not address. The profile has no concept of multi-paradigm output synthesis, no requirements for streaming order consistency, and no risk category mapping to order-dependent instruction activation. As established in earlier cache analyses (RND_1_1, RND_1_2), streaming infrastructure is explicitly outside NIST's scope. The non-deterministic ordering of synthesized outputs is an architectural pattern concern that governance documentation cannot resolve.
**Score: 1**

### RND_33_4
Knowledge graph consistency assurance gaps during multi-agent evolution—where agents independently update shared knowledge graphs creating inconsistency windows exploitable by contradiction injection—have weak but non-zero connection to NIST AI 600-1. §2.9 (Information Security) acknowledges data poisoning as a recognized risk, and knowledge graph contradiction injection is a form of targeted knowledge poisoning. MEASURE 2.7 red-teaming could in principle include scenarios probing knowledge graph consistency. However, the profile addresses data poisoning at training data and model input levels, not at runtime knowledge store consistency management for evolving multi-agent systems. The distributed consistency and race condition dimensions are infrastructure concerns that the governance framework does not reach.
**Score: 1**

### RND_33_5
Feedback loop timing non-determinism in hybrid cooperation—where relative timing between components determines convergence paths and attackers exploit timing to force dangerous convergence—is a distributed systems real-time control problem that NIST AI 600-1 does not address. The profile has no provisions for feedback loop timing control, no requirements for convergence path validation in hybrid systems, and no risk category for timing-based attack vectors. MANAGE 4.1–4.3 could address incident response after dangerous convergence is observed, but provides no preventive controls for timing exploitation. This is an infrastructure and real-time systems security concern outside the framework's intended scope.
**Score: 1**

---

## RND_34 - Other Risks/Threats/Vulnerabilities

### RND_34_1
Keyboard navigation testing gaps for approval workflows—where dynamic DOM manipulation and async rendering create fragile test coverage amplified by unpredictable multi-agent approval ordering—is a UI testing infrastructure problem that NIST AI 600-1 does not address. The profile has no provisions for UI accessibility testing methodologies, DOM rendering determinism requirements, or test coverage standards for approval interfaces in multi-agent systems. The Human-AI Configuration risk category motivates meaningful human oversight design, which implicitly motivates reliable approval interfaces, but NIST does not prescribe testing methodologies for dynamic UI elements. This is a software testing and UI engineering concern outside the governance framework's scope.
**Score: 1**

### RND_34_2
HITL approval testing non-reproducibility from confidence score variance, dynamic threshold adjustments, and time-based expiration has moderate connection to NIST AI 600-1's Human-AI Configuration risk category, which directly addresses automation bias and the need for effective human oversight of AI decisions. GOVERN 3.2 and MEASURE 2.9 together motivate organizational requirements to validate that AI confidence outputs are reliable before using them as automated decision gates—the core governance rationale applies to threshold-based HITL approval systems. MEASURE 2.7's security evaluation mandate provides a hook for testing approval workflow behavior under variance conditions pre-deployment. However, the profile provides no technical guidance on confidence score calibration, adaptive threshold governance, or reproducibility standards for multi-agent approval testing, limiting coverage to governance framing without actionable technical controls.
**Score: 2**

### RND_34_3
Auto-scaling non-determinism in replica configuration—where scaled replicas may not receive identical configurations, undermining testing validity—is an infrastructure and DevOps concern that NIST AI 600-1 does not address. The profile has no provisions for replica configuration consistency, infrastructure-as-code requirements, or testing validity conditions for auto-scaled deployments. GOVERN 6.1/6.2 supply chain provisions could nominally require cloud infrastructure providers to document scaling behavior, but this is an interpretive stretch and not a stated control. This is an infrastructure operations concern entirely outside the governance framework's scope.
**Score: 1**

### RND_34_4
Batching timeout non-determinism—where dynamic batching with probabilistic timeouts creates variable batch compositions affecting inference outputs—is a runtime inference infrastructure concern that NIST AI 600-1 does not address. The profile has no concept of batching strategies, probabilistic timeout configurations, or batch composition effects on model outputs. MEASURE 2.7 pre-deployment testing evaluates model behavior for specific inputs but cannot account for the emergent non-determinism created by variable batch composition under production load conditions. This is an ML serving infrastructure concern outside the framework's scope.
**Score: 1**

### RND_34_5
Load balancing algorithm non-determinism—where routing to different replicas introduces execution variance because replicas may have subtle behavioral differences—is an infrastructure routing concern that NIST AI 600-1 does not address. The profile has no provisions for load balancer configuration, replica behavioral consistency requirements, or testing protocols that account for routing variance. This is an infrastructure and operational concern outside the governance framework's scope, similar to the auto-scaling concern in RND_34_3. No NIST control addresses routing-induced non-determinism in deployed AI systems.
**Score: 1**

### RND_34_6
Caching invalidation non-determinism based on TTL creating variable cache states—where agents may receive cached versus fresh responses producing behavioral inconsistencies—is an infrastructure caching design concern that NIST AI 600-1 does not address. The profile has no provisions for caching consistency requirements in AI serving infrastructure, no requirements for cache invalidation policy documentation, and no risk category mapping to TTL-based behavioral variance. MANAGE 4.1–4.3 could address incidents arising from cache-induced behavioral inconsistencies after detection, but provides no preventive controls. This is an infrastructure operations concern out of scope for the governance framework.
**Score: 1**

### RND_34_7
Non-deterministic evaluation from variable model temperature settings—where evaluation results themselves vary and attackers hide metric variance across runs—is a testing and evaluation methodology concern with a thin but real connection to NIST AI 600-1. MEASURE 2.7's security evaluation mandate implicitly requires that evaluation results be meaningful and reliable, and the profile's general requirement for safety and security testing creates a governance-level expectation of reproducible evaluation. However, NIST provides no specific guidance on evaluation temperature standardization, variance reporting requirements, or statistical validity standards for AI system evaluations. The attacker exploitation dimension—deliberately exploiting temperature variation to hide metric variance from evaluators—is a red-teaming evasion technique that the profile does not acknowledge.
**Score: 1**

### RND_34_8
Evaluation instability from framework version changes affecting evaluation infrastructure—such as MLflow NaN handling changes altering reported metrics—is an evaluation tooling and MLOps concern that NIST AI 600-1 does not address. The profile mandates pre-deployment evaluation (MEASURE 2.6, 2.7) but provides no requirements for evaluation infrastructure versioning, reproducibility standards for ML experiment tracking tools, or change management processes for evaluation framework dependencies. This is a software engineering and MLOps infrastructure concern outside the governance framework's intended scope. No NIST control addresses evaluation validity maintenance across framework version changes.
**Score: 1**

### RND_34_9
Continuous evaluation CI/CD timing variations—where variable CI infrastructure performance creates non-deterministic evaluation outcomes particularly for timeout-based decisions—is an infrastructure and DevOps concern that NIST AI 600-1 does not address. The profile requires pre-deployment security evaluation but provides no specifications for CI/CD infrastructure consistency, timeout policy design, or evaluation validity conditions. MEASURE 2.7's red-teaming mandate operates at the model and system level, not at the CI/CD pipeline configuration level. This is a software delivery infrastructure concern outside the governance framework's scope.
**Score: 1**

### RND_34_10
Evaluation workflow state machine non-determinism—where non-deterministic state transitions affect evaluation results for agents with state-dependent behavior—has a minimal connection to NIST AI 600-1's MEASURE 2.7, which mandates security evaluation and red-teaming. Pre-deployment testing could in principle probe state-dependent behavior, and evaluation reproducibility is an implicit requirement of meaningful MEASURE compliance. However, the profile provides no guidance on state machine design for evaluation workflows, no requirements for deterministic evaluation execution environments, and no recognition that non-deterministic evaluation state transitions undermine the validity of compliance claims. Coverage is negligible beyond the thin MEASURE hook.
**Score: 1**

### RND_34_11
Model update timing creating evaluation blind spots—where attackers time malicious updates to coincide with gaps between evaluation cycles—has partial coverage in NIST AI 600-1 through GOVERN 6.1/6.2's supply chain and third-party vetting provisions, which can require documentation and review processes for model updates. The Value Chain and Component Integration category (§2.12) acknowledges update-related integrity risks in the model supply chain. However, the adversarial exploitation of timing windows between evaluation runs—specifically crafting update timing to evade detection—is a red-teaming and monitoring concern that governance documentation does not close. MANAGE 4.1–4.3's incident response provisions are post-detection and do not prevent exploitation during evaluation blind spots. Supply chain governance provides a weak but genuine hook.
**Score: 1**

### RND_34_12
Difficulty classification continual adaptation creating assurance evasion—where same inputs receive variable sampling budgets over time and multi-agent shared classifications experience uniform drift—is an adaptive evaluation infrastructure concern outside NIST AI 600-1's scope. The profile's MEASURE provisions address evaluation at a point in time and have no provisions for adaptive difficulty classifiers that continuously change evaluation sampling, no requirements for stability of evaluation sampling budgets, and no recognition that drifting evaluation infrastructure can systematically bias safety assurance outcomes. This is an ML evaluation systems design concern beyond the governance framework's intended scope.
**Score: 1**

### RND_34_13
Non-deterministic path ordering in weighted voting—where identical quality scores produce non-deterministic selection ordering and multi-agent voting propagation amplifies downstream variance—is an evaluation aggregation design concern with a minimal connection to NIST AI 600-1. MEASURE 2.7's testing mandate provides the weakest possible hook, as evaluation could probe whether tie-breaking behavior is consistent. MEASURE 2.9's model explanation and validation provisions nominally motivate requiring that voting systems produce explainable, reproducible decisions—a loose connection to the determinism requirement. However, NIST provides no technical guidance on voting algorithm design, tie-breaking policy requirements, or consistency standards for multi-agent aggregation. The governance framing is present but the technical controls are entirely absent.
**Score: 1**

### RND_34_14
Semantic chunking boundary non-determinism—where different ETL implementations or line-ending conventions produce different chunk counts from identical content, creating systematic multi-agent inconsistencies—is an ETL pipeline infrastructure concern that NIST AI 600-1 does not address. The profile has no provisions for ETL implementation consistency, no requirements for chunking algorithm standardization across agent deployments, and no risk category mapping to data pipeline determinism as a security concern. §2.9's data poisoning coverage does not extend to ETL infrastructure inconsistency as a reliability or integrity risk. This is an infrastructure and data engineering concern outside the framework's scope.
**Score: 1**

### RND_34_15
Deduplication hash collision non-determinism in concurrent multi-agent extraction—where race conditions in concurrent hash computation create 15–40% duplicate rates—is an ETL concurrency and data pipeline integrity concern that NIST AI 600-1 addresses only weakly. §2.9's data poisoning scope could nominally extend to data pipeline integrity, as duplicate or corrupted data reaching model inputs represents a form of data quality failure affecting AI system behavior. GOVERN 6.1/6.2 supply chain provisions can require documentation of ETL pipeline integrity controls from data infrastructure providers. However, NIST provides no guidance on concurrent ETL design, hash collision handling, or race condition mitigation in multi-agent data pipelines. The data poisoning hook is real but the ETL infrastructure dimension is beyond the framework's scope.
**Score: 2**

### RND_34_16
REST API pagination cursor non-determinism creating inconsistent multi-agent extraction—where concurrent inserts during extraction cause record duplication across agents—is an ETL data consistency concern with the same weak data pipeline integrity hook as RND_34_15. §2.9's data poisoning framing can nominally encompass records injected into data pipelines during extraction windows as an integrity concern. GOVERN 6.1/6.2 supply chain provisions can require API providers to document pagination consistency guarantees. However, NIST has no controls for pagination cursor consistency, concurrent modification isolation, or multi-agent extraction coordination, making the practical coverage minimal despite the conceptual connection to data integrity.
**Score: 2**

### RND_34_17
Filesystem modification time resolution variability affecting incremental ETL—where platform differences in mtime resolution (ext4 nanoseconds vs FAT32 2-second granularity) cause heterogeneous agent deployments to detect different change sets—is an infrastructure and data pipeline consistency concern that NIST AI 600-1 does not address. The profile has no provisions for filesystem compatibility requirements across agent deployment environments, no requirements for change detection mechanism standardization, and no risk category mapping to platform-level resolution variability as a data integrity concern. This is a systems-level infrastructure engineering concern outside the governance framework's scope.
**Score: 1**

### RND_34_18
Batch subdivision ordering non-determinism during partial ETL failure recovery—where different retry subdivision strategies create different database load patterns and simultaneous multi-agent failures risk deadlocks—is an ETL resilience and distributed systems concern that NIST AI 600-1 does not address. The profile has no provisions for ETL failure recovery strategy standardization, no requirements for deadlock prevention in multi-agent data pipeline operations, and no risk category for data pipeline failure recovery as a security concern. MANAGE 4.1–4.3 covers incident response after failures are detected but does not reach ETL retry subdivision design. This is an infrastructure engineering concern outside the framework's scope.
**Score: 1**

### RND_34_19
Quality metric calculation variability across heterogeneous agent validation implementations—where five-dimensional quality assessment with different methods produces incompatible scores and attackers route documents to lenient implementations—has partial coverage in NIST AI 600-1 through MEASURE 2.7's security evaluation mandate and GOVERN 6.1/6.2's supply chain provisions. The attacker-routing dimension—deliberately directing content to the most permissive validator—is a form of policy evasion through system heterogeneity that MEASURE 2.7 red-teaming could in principle probe. GOVERN 6.1/6.2 can require documentation and consistency standards for validation implementations across the supply chain. However, NIST provides no technical guidance on quality metric standardization, inter-validator consistency requirements, or detection of routing-based evasion. The governance framing provides a moderate but incomplete hook.
**Score: 2**

## RTM - Telemetry and monitoring blind spots specific to cognitive and tool behavior
# NIST AI 600-1 RTM Analysis Cache — RTM_1 (UI and Interface Telemetry Gaps)

---

### RTM_1_1
NIST AI 600-1 is a pre-deployment governance framework and does not mandate any specific telemetry architecture, logging infrastructure, or observability tooling. The Human-AI Configuration risk category acknowledges automation bias and over-reliance concerns, and GOVERN 3.2 can require policies for meaningful human oversight, but neither provision addresses the technical problem of progressive disclosure UI patterns hiding reasoning traces in collapsed views. Multi-agent observability state tracking across simultaneous agents is an operational monitoring engineering problem that falls entirely outside the framework's scope. MANAGE 4.1–4.3 requires incident disclosure policies at an institutional level but specifies no logging depth, no disclosure-state capture requirements, and no cross-agent telemetry standards.
**Score: 1**

---

### RTM_1_2
NIST AI 600-1 does not address chat interface logging architectures, semantic context capture, intent classification, or reasoning provenance recording. The framework's Information Security category (§2.9) names prompt injection as a recognized risk and pre-deployment red-teaming (MEASURE 2.7) could in principle exercise adversarial query patterns, but neither constitutes a logging or semantic telemetry control. Attack signature detection in query patterns is a runtime SIEM function entirely outside the governance-level profile. MANAGE 4.1–4.3 incident disclosure policies operate at the institutional reporting layer and impose no requirements on what semantic metadata chat logs must retain.
**Score: 1**

---

### RTM_1_3
Streaming response telemetry gaps are a runtime infrastructure problem: security monitoring cannot analyze complete responses until a stream terminates, and non-deterministic timing disrupts baseline establishment. NIST AI 600-1 contains no controls for streaming architectures, incremental response logging, or real-time telemetry collection pipelines. The framework's pre-deployment testing guidance (MEASURE 2.6, MEASURE 2.7) evaluates model behavior in controlled scenarios but does not address live streaming attack detection delays. No part of the GOVERN, MAP, MEASURE, or MANAGE action sets touches telemetry timing requirements or streaming-specific monitoring infrastructure.
**Score: 1**

---

### RTM_1_4
Approval workflow telemetry that records decisions without capturing reasoning provenance is an audit-log design problem. NIST AI 600-1 acknowledges Human-AI Configuration risks including automation bias and calls for human oversight policies (GOVERN 3.2), which could require that approval workflows exist and be evaluated during red-teaming. However, the framework provides no requirements for what approval audit logs must contain, no mandates for capturing confidence aggregation lineage, and no controls against fraudulent confidence inflation in multi-agent pipelines distorting approval records. MANAGE 4.1–4.3 incident disclosure is an after-the-fact institutional process and does not specify the provenance depth of approval audit trails.
**Score: 1**

---

### RTM_1_5
Command palette suggestion telemetry that fails to capture context manipulation indicators is a runtime observability gap involving why suggestions appeared and whether context was adversarially biased. NIST AI 600-1 does not address command palette interfaces, suggestion generation telemetry, or context manipulation detection. The Information Security category (§2.9) names indirect prompt injection as a risk, and pre-deployment testing could exercise prompt-biased suggestion scenarios, but this does not translate into operational telemetry requirements for live systems. No NIST control addresses the logging of suggestion generation context, intermediate reasoning states, or adversarial context bias indicators in real-time agent UIs.
**Score: 1**

---

### RTM_1_6
Error recovery telemetry that treats each retry as an atomic event rather than capturing full retry-sequence context is a logging architecture decision. NIST AI 600-1 does not address retry logic, error recovery workflows, or the telemetry design of agent failure handling. MANAGE 4.1–4.3 incident response policies could in principle require that error events be logged for post-incident review, providing a weak governance framing for incident-level logging, but this falls well short of mandating retry-sequence context capture or differential input analysis across retry attempts. The framework's scope does not extend to runtime agent error state management or the security implications of stateless versus stateful retry telemetry.
**Score: 1**

---

### RTM_1_7
Cross-session state poisoning — where attackers inject malicious instructions in early sessions that propagate through agent memory into subsequent sessions — is a runtime persistence and multi-agent state management attack. NIST AI 600-1 does not address session persistence architectures, cross-session telemetry continuity, or long-lived cognitive state monitoring. The framework's pre-deployment testing guidance could exercise session persistence attack patterns in a controlled environment, but operational telemetry that treats each session independently and misses propagated state corruption is an implementation-layer problem beyond the governance profile's reach. No NIST control addresses the logging continuity requirements needed to detect cross-session state poisoning in production.
**Score: 1**

---

### RTM_1_8
Multi-agent dashboard attribution telemetry gaps enabling impersonation arise because standard telemetry logs displayed content without verifying whether agent identity was cryptographically authenticated. NIST AI 600-1 does not address agent identity verification, attribution telemetry, or multi-agent trust mechanisms. The Value Chain and Component Integration category (§2.12) and GOVERN 6.1/6.2 supply chain vetting provide governance framing for third-party agent components, which is marginally relevant to whether agent identity is established at integration time. However, runtime identity verification and attribution telemetry in live multi-agent dashboards are operational security controls that the governance profile does not specify. The supply chain framing offers only the weakest indirect relevance.
**Score: 1**

---

### RTM_1_9
Inline suggestion rejection pattern analysis — capturing patterns where users consistently reject suggestions from a compromised agent — is a behavioral analytics function requiring purpose-built telemetry that logs not just acceptances but also rejections with context. NIST AI 600-1 contains no controls for behavioral telemetry, rejection-pattern monitoring, or anomaly detection in suggestion pipelines. Pre-deployment testing could evaluate whether a compromised agent generates suspicious suggestions in controlled scenarios, but this does not create operational telemetry for live rejection pattern analysis. The framework's governance layer is entirely disconnected from the real-time analytics infrastructure needed to detect this attack pattern.
**Score: 1**

---

### RTM_1_10
Tool invocation logging without parameter semantic analysis treats parameters as opaque values and misses whether they are semantically appropriate or sourced from poisoned upstream agents. NIST AI 600-1's Information Security category (§2.9) recognizes prompt injection and data poisoning as risks, and pre-deployment red-teaming (MEASURE 2.7) could probe tool parameter injection scenarios. This provides weak governance framing: the risks are named, and testing could surface them before deployment. However, runtime semantic analysis of tool parameters — including cross-agent provenance tracking to determine whether parameters originated from poisoned upstream outputs — is an operational monitoring function that the framework does not mandate or specify. The pre-deployment recognition of injection risks is the only element of partial relevance.
**Score: 1**

---

### RTM_1_11
Confidence score telemetry that fails to capture decomposition and multi-agent aggregation transparency prevents detection of compromised confidence aggregation. NIST AI 600-1 does not address confidence score architectures, aggregation transparency, or telemetry requirements for explainability of multi-agent scoring. MEASURE 2.9 calls for model explanation and validation before deployment, which is marginally relevant in that it requires organizations to understand how models produce outputs, including potentially how confidence is calculated. However, this is a pre-deployment model-level requirement and does not extend to runtime telemetry capturing per-agent confidence contributions or detecting fraudulent weighting in live multi-agent systems. The governance profile offers no operational monitoring standard for confidence score integrity.
**Score: 1**

---

### RTM_1_12
Session persistence telemetry that captures state restoration without verifying integrity or detecting unauthorized modifications between sessions is a runtime security control gap. NIST AI 600-1 does not address session state integrity verification, persistence telemetry, or between-session tamper detection. MANAGE 4.1–4.3 incident disclosure policies require organizations to have processes for reporting incidents once discovered, which provides institutional framing for responding to detected session tampering but imposes no requirements on the detection mechanism itself. The framework has no controls for integrity checks on restored session state, tamper-evident logging of session persistence operations, or telemetry that would surface unauthorized inter-session modifications.
**Score: 1**

---

### RTM_1_13
Reasoning trace telemetry sampling bias — where performance overhead forces incomplete logging that creates blind spots for non-sampled attacks — is an engineering tradeoff between observability completeness and system performance. NIST AI 600-1 does not address logging performance overhead, sampling strategies, or the security implications of telemetry gaps created by performance constraints. The framework's pre-deployment testing guidance operates in controlled environments where performance-observability tradeoffs may not be replicated at production scale. No NIST control addresses the multi-agent amplification of this problem, where sampling blind spots across multiple concurrent agent reasoning traces compound the detection gap. This is an infrastructure engineering concern entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_1_14
User intervention telemetry that records the intervention without capturing complete pre-intervention state — including distributed agent state across a multi-agent system — prevents forensic analysis of whether an intervention stopped an attack. NIST AI 600-1 does not address pre-intervention state capture, intervention telemetry design, or forensic completeness requirements for audit logs. MANAGE 4.1–4.3 incident response policies could implicitly require that sufficient evidence be preserved to support post-incident analysis, providing the weakest governance framing for comprehensive intervention logging. However, this falls well short of mandating the specific pre-intervention state snapshots needed to reconstruct distributed agent contexts at the moment of human intervention.
**Score: 1**

---

### RTM_1_15
Cost and resource telemetry that fails to analyze economic patterns for security indicators — such as compromised agents repeatedly invoking expensive operations — is a behavioral analytics function requiring security-oriented interpretation of operational metrics. NIST AI 600-1 addresses Environmental Impacts as one of its 12 risk categories, acknowledging computational resource consumption concerns, but this is framed around sustainability and organizational accountability rather than security monitoring for economic attack patterns. No NIST control mandates security analysis of cost telemetry, anomaly detection on resource consumption distributions, or multi-agent economic pattern monitoring. The framework's governance layer does not connect operational cost metrics to security threat detection functions.
**Score: 1**

---

### RTM_1_16
HITL workflow monitoring blind spots — where aggregate metrics hide critical individual decision points and intervention in one agent may prevent cascading failures in dependent agents — are operational monitoring design problems. NIST AI 600-1's Human-AI Configuration risk category directly addresses human oversight and the risks of automation bias, and GOVERN 3.2 can require policies mandating meaningful human review at defined decision points. This governance framing is moderately relevant: organizations following NIST guidance would be required to define HITL workflows, evaluate them during pre-deployment testing, and ensure they are not reduced to rubber-stamp processes. However, the specific telemetry gap — aggregate metrics masking individual decision quality and multi-agent cascade dependencies — is an operational monitoring engineering problem that policy mandates alone do not close.
**Score: 1**

---

### RTM_1_17
Accessibility telemetry gaps where monitoring dashboards fail screen reader users — leaving their navigation patterns unmeasured and potentially hiding their inability to access critical multi-agent monitoring details — are a UI accessibility and analytics design problem. NIST AI 600-1 does not address accessibility requirements for monitoring dashboards, assistive technology compatibility, or analytics instrumentation for non-visual interaction patterns. The Human-AI Configuration risk category broadly concerns itself with how humans interact with AI systems and could theoretically encompass the requirement that all users can effectively perform oversight functions regardless of ability, but the framework provides no specific accessibility controls or telemetry requirements. This threat is entirely outside the governance profile's scope.
**Score: 1**
# NIST AI 600-1 RTM_2 Analysis Cache

## RTM_2 - Framework-Specific Architecture and Logging Gaps

### RTM_2_1
NIST AI 600-1 operates at the governance and risk management layer and does not address telemetry architecture, logging granularity, or observability design within multi-tier agent hierarchies. The Information Security risk category (§2.9) acknowledges prompt injection as a recognized risk, which is the attack vector that exploits plan-and-execute opacity, but NIST provides no controls for tier-differentiated logging, supervisor-to-worker trace continuity, or memory context isolation across hierarchy levels. MEASURE 2.7 calls for security evaluation before deployment, but pre-deployment testing cannot systematically surface observability dead zones that only emerge under live multi-tier execution patterns. MANAGE 4.1–4.3 is an institutional incident disclosure policy and does not mandate what audit trails must capture across agent tiers.
**Score: 1**

### RTM_2_2
NIST AI 600-1 does not address per-agent versus cross-agent monitoring architectures, tool access scope design, or the detection of privilege escalation chains assembled from individually benign tool calls. The Value Chain and Component Integration category (§2.12) and GOVERN 6.1/6.2 cover third-party component vetting, which has marginal relevance if distributed tool access relies on third-party tool providers, but vetting addresses provenance rather than runtime cross-agent orchestration visibility. Pre-deployment testing under MEASURE 2.7 could in principle probe tool chain combinations for escalation patterns, but the framework provides no requirement or methodology for cross-agent correlation of tool invocation sequences. The blind spots this threat describes are an operational monitoring architecture problem that falls entirely outside a governance-level risk profile.
**Score: 1**

### RTM_2_3
NIST AI 600-1 contains no provisions for distributed tracing infrastructure, correlation ID integrity, or cryptographic verification of trace identifiers. MANAGE 4.1–4.3 establishes institutional incident disclosure policies that assume investigation is possible after an incident, but the framework does not mandate the technical tracing systems that correlation ID manipulation is designed to subvert. The Information Security risk category recognizes prompt injection broadly, and an attacker who can inject content to manipulate correlation IDs is exploiting that vector, but NIST's acknowledgment does not extend to trace integrity requirements or tamper-evident logging mandates. Audit trail requirements at the governance level NIST operates on do not specify what audit trails must capture or how they must be protected from manipulation.
**Score: 1**

### RTM_2_4
Swarm intelligence systems with emergent collective behavior represent an architectural pattern that NIST AI 600-1 does not address at any level. The framework's governance actions—GOVERN 3.2 for human oversight policies, MEASURE 2.7 for pre-deployment evaluation—assume agent systems with identifiable orchestrators or decision points that can be evaluated and overseen; decentralized swarm systems with no comprehensive awareness at any node are structurally outside this assumption. The Human-AI Configuration risk category acknowledges automation bias and the need for meaningful human oversight, but swarm systems where individual telemetry appears normal while system-level behavior is malicious present an oversight challenge the profile's policy mechanisms cannot address. No NIST AI 600-1 control targets emergent behavior detection, system-level anomaly identification, or collective behavior baselining.
**Score: 1**

### RTM_2_5
NIST AI 600-1 explicitly does not cover framework-specific vulnerabilities or monitoring implementations for individual AI agent frameworks such as LangChain, AutoGen, CrewAI, or Semantic Kernel. The profile's scope is governance, risk categorization, pre-deployment evaluation policy, and incident disclosure—none of which prescribe how multi-framework systems should instrument boundaries or reconcile heterogeneous telemetry. GOVERN 6.1/6.2 supply chain vetting could apply if individual frameworks are treated as third-party components subject to pre-acquisition review, but this does not address runtime observability gaps at framework integration points. Multi-framework observability gaps are an architectural implementation concern that sits entirely outside NIST AI 600-1's intended scope.
**Score: 1**

### RTM_2_6
NIST AI 600-1's Value Chain and Component Integration risk category (§2.12) and GOVERN 6.1/6.2 direct organizations to vet third-party components and understand supply chain dependencies, which has marginal relevance to documentation gaps that expose cross-framework integration patterns to adversary reconnaissance. However, these controls address whether components are trustworthy sources, not whether documentation of integration patterns should be restricted or abstracted to limit attacker knowledge of cross-framework attack paths. The framework provides no guidance on documentation scope management, information disclosure risk from architectural documentation, or threat-model compartmentalization across multi-framework deployments. This threat describes an information security concern about documentation as an attack surface that is outside NIST AI 600-1's governance and risk management scope.
**Score: 1**

### RTM_2_7
NIST AI 600-1 does not address checkpointing mechanisms, state management architectures, or the integrity of checkpoint-mediated recovery in agent systems. MANAGE 4.1–4.3 establishes incident disclosure policies but does not specify technical requirements for audit trail completeness, pre-checkpoint state capture, or tamper detection in recovery sequences. The Information Security category (§2.9) recognizes data poisoning as a risk, and checkpoint state corruption is a form of persistent state poisoning, but NIST's acknowledgment does not extend to controls that would detect intermediate state mutations obscured by subsequent checkpoint recovery. Governance-level audit trail requirements do not specify what audit trails must capture, leaving the pre-checkpoint-to-post-checkpoint state delta visibility gap entirely unaddressed.
**Score: 1**

### RTM_2_8
Conditional routing decision logging and the capture of routing reasoning context are operational telemetry implementation concerns that NIST AI 600-1 does not address. The framework's pre-deployment testing guidance under MEASURE 2.7 could theoretically include evaluation of whether routing decisions are logged with sufficient context, but the framework imposes no such requirement and provides no methodology for evaluating routing decision opacity. MANAGE 4.1–4.3 incident response policies assume that investigations are possible, but do not mandate that the technical artifacts—including routing reasoning—necessary for investigation are preserved. Control flow opacity in multi-agent conditional routing is a monitoring architecture problem outside the governance profile's scope.
**Score: 1**

### RTM_2_9
State reducer execution tracing and field value transformation visibility across multi-agent iterations are telemetry implementation concerns that NIST AI 600-1 does not cover. The profile operates at the governance layer and does not prescribe logging requirements for specific computational primitives such as state reducers, regardless of their security relevance in multi-agent orchestration. MEASURE 2.7's pre-deployment security evaluation could in principle assess whether reducer operations are adequately traced, but the framework does not direct evaluators to examine this class of observability gap. No NIST control addresses unified visibility across reducer operations or the cross-agent state evolution tracking necessary to detect reducer-mediated state manipulation.
**Score: 1**

### RTM_2_10
LangChain's ConversationBufferMemory is a framework-specific component that NIST AI 600-1 explicitly does not cover. The Information Security category (§2.9) recognizes prompt injection as a risk, and conversation memory logging gaps that prevent injection detection are a direct consequence of that risk being realized in a framework-specific context, but NIST provides no controls for semantic analysis of memory contents, cross-agent communication pattern monitoring, or injection propagation tracing through shared memory. Framework-specific monitoring requirements for individual components like ConversationBufferMemory always fall outside NIST AI 600-1's governance scope. The governance profile cannot substitute for framework-level telemetry instrumentation decisions.
**Score: 1**

### RTM_2_11
Agent scratchpad reasoning trace logging is a framework-specific telemetry implementation detail that NIST AI 600-1 does not address. While the Human-AI Configuration risk category acknowledges the importance of human oversight and meaningful interpretability of AI decision-making, it does not extend to mandating that internal reasoning traces—such as agent_scratchpad contents—be captured and correlated across multi-agent systems. MEASURE 2.7 pre-deployment evaluation could assess reasoning transparency in principle, but NIST imposes no requirement for reasoning trace completeness or cross-agent reasoning correlation in deployed systems. The gap between step-count telemetry and full reasoning trace capture is a monitoring architecture decision that governance-level frameworks do not prescribe.
**Score: 1**

### RTM_2_12
NIST AI 600-1's Information Security category (§2.9) names prompt injection as a recognized risk, and parameter chain injection across agents is a multi-hop form of that attack vector, giving the framework minimal conceptual relevance. However, semantic analysis of tool invocation parameters, parameter provenance tracing across agent boundaries, and detection of injected content propagating through parameter chains are operational telemetry capabilities that the governance profile does not mandate or describe. MEASURE 2.7 pre-deployment testing could probe parameter injection scenarios, but runtime parameter provenance monitoring is an operational monitoring requirement beyond the profile's scope. LangChain-specific telemetry gaps are explicitly outside NIST AI 600-1's framework coverage.
**Score: 1**

### RTM_2_13
NIST AI 600-1 does not address memory provenance tracking, per-agent state modification attribution, or the distinction between legitimate learned state updates and poisoning-driven state mutations in multi-agent systems. The Information Security category recognizes data poisoning as a named risk (§2.9), providing minimal conceptual relevance, but the framework offers no technical controls for tracking which agent modified shared state or for detecting whether modifications reflect adversarial intent. MANAGE 4.1–4.3 incident disclosure policies assume that investigations can identify what happened, but do not mandate the provenance telemetry that would make such investigations possible. Shared memory modification attribution is a monitoring architecture requirement that falls outside governance-level GenAI risk profiles.
**Score: 1**

### RTM_2_14
Error recovery sequence telemetry—specifically capturing which code paths retry operations exercise rather than just retry counts—is an operational monitoring implementation detail that NIST AI 600-1 does not address. The Information Security category recognizes that AI systems can be exploited through adversarial inputs, and coordinated error injection across agents is a form of adversarial manipulation, but NIST's acknowledgment does not extend to controls for error telemetry completeness or cross-agent error correlation. MEASURE 2.7 pre-deployment evaluation and MANAGE 4.1–4.3 incident response both assume that technical visibility is available, but neither mandates the retry path logging or cross-agent error pattern correlation that would detect coordinated error injection attacks. This is an operational monitoring gap outside the framework's scope.
**Score: 1**

### RTM_2_15
Confidence score decomposition telemetry—showing which factors contributed to final scores and how per-agent scores were weighted in aggregation—is a monitoring implementation concern that NIST AI 600-1 does not address. The Human-AI Configuration risk category acknowledges automation bias and over-reliance on AI outputs, which is the underlying vulnerability that confidence score manipulation exploits, but the framework provides no controls for confidence calculation transparency or aggregation methodology auditing. MEASURE 2.7 pre-deployment evaluation could assess confidence scoring systems in principle, but the framework does not require decomposition visibility or per-agent score attribution in deployed multi-agent systems. Confidence aggregation telemetry requirements are operational monitoring decisions outside the governance profile's scope.
**Score: 1**

### RTM_2_16
AutoGen is a framework-specific implementation that NIST AI 600-1 explicitly does not cover. The Information Security category (§2.9) recognizes prompt injection as a risk, and GroupChat message history logging gaps that prevent attack attribution are a consequence of that risk manifesting in a framework-specific context, but NIST provides no controls for semantic analysis of inter-agent dialogue, message intent classification, or attribution-preserving logging of multi-agent conversation patterns. Framework-specific monitoring for AutoGen's GroupChat architecture always falls outside NIST AI 600-1's governance scope. The inability to attribute attacks within multi-agent dialogue is a telemetry design problem that governance frameworks do not resolve.
**Score: 1**

### RTM_2_17
CrewAI's hierarchical task delegation architecture is a framework-specific implementation that NIST AI 600-1 does not address. The Information Security category recognizes indirect prompt injection as a risk, and task context manipulation during delegation is a form of that attack, but NIST's acknowledgment does not extend to controls for delegation appropriateness logging, task context integrity verification, or hierarchical trace completeness in CrewAI-specific pipelines. GOVERN 6.1/6.2 supply chain vetting applies to CrewAI as a third-party framework component subject to pre-acquisition review, but this does not address runtime delegation opacity. Framework-specific monitoring requirements for CrewAI hierarchical traces always score at the minimum level against NIST AI 600-1.
**Score: 1**

### RTM_2_18
Multi-agent monitoring dashboard design and visualization requirements are not addressed by NIST AI 600-1 at any level. The Human-AI Configuration risk category acknowledges that human oversight must be meaningful and that automation bias is a concern, which implies that dashboards should support effective human review, but the framework provides no guidance on visualization design, simultaneous activity pattern rendering, or the challenge of making coordinated attack signatures visible amid concurrent multi-agent activity. GOVERN 3.2 policies for human-AI interaction roles could in principle require that monitoring tools meet interpretability standards, but no such requirement is specified. Dashboard visualization blind spots are a human factors engineering problem that governance-level GenAI risk profiles do not reach.
**Score: 1**

### RTM_2_19
AutoGen's non-deterministic conversation flows and the inability to establish reliable behavioral baselines for anomaly detection are framework-specific operational characteristics that NIST AI 600-1 does not address. MEASURE 2.7 pre-deployment security evaluation could assess baseline establishment feasibility in principle, but the framework does not require that deployed multi-agent systems maintain behavioral baselines or that anomaly detection be possible during operation. The non-determinism that prevents distinguishing normal variation from attack-driven variation is an architectural property of AutoGen that governance policies cannot resolve without technical instrumentation changes. Framework-specific baseline establishment challenges for AutoGen always fall outside NIST AI 600-1's scope.
**Score: 1**

### RTM_2_20
CrewAI's multi-tier hierarchy and the monitoring challenge of attributing responsibility across tiers when attacks at one level affect outcomes at another are framework-specific architectural concerns that NIST AI 600-1 does not address. The framework's governance actions—including GOVERN 3.2 human oversight policies and MANAGE 4.1–4.3 incident response—assume that accountability can be attributed, but do not mandate the technical monitoring architecture necessary to achieve cross-tier attribution in CrewAI hierarchical systems. Supply chain vetting under GOVERN 6.1/6.2 applies to CrewAI as a component, not to its runtime tier responsibility attribution design. Framework-specific monitoring opacity in CrewAI hierarchies always falls outside the governance profile's scope.
**Score: 1**

### RTM_2_21
Semantic Kernel's plugin execution chain abstraction and the hiding of individual plugin execution order and parameters within high-level operation summaries are framework-specific telemetry design decisions that NIST AI 600-1 does not address. The framework neither mandates telemetry granularity for plugin orchestration systems nor requires that individual plugin invocations within abstracted chains be separately logged and attributed. GOVERN 6.1/6.2 supply chain vetting applies to Semantic Kernel and its plugins as third-party components, which is marginally relevant if malicious plugins are introduced through the supply chain, but vetting does not address runtime execution trace abstraction. Plugin chain visibility is a monitoring implementation decision that governance profiles do not prescribe.
**Score: 1**

### RTM_2_22
Semantic Kernel's function description processing pipeline and the absence of telemetry analyzing description content for injected instructions are framework-specific implementation details that NIST AI 600-1 does not cover. The Information Security category (§2.9) recognizes indirect prompt injection as a named risk, and injected instructions within plugin function descriptions are precisely that attack vector, giving the framework minimal conceptual relevance. However, NIST provides no controls for semantic analysis of function description content during prompt construction, real-time injection detection in plugin metadata pipelines, or telemetry requirements for description-to-prompt transformation stages. Framework-specific telemetry gaps in Semantic Kernel's function description processing always fall outside NIST AI 600-1's governance scope.
**Score: 1**

### RTM_2_23
Kernel service resolution telemetry and visibility into which plugins resolved which services—particularly when malicious service implementations are injected—are framework-specific operational monitoring concerns that NIST AI 600-1 does not address. GOVERN 6.1/6.2 supply chain vetting is marginally relevant if malicious service implementations enter through the third-party component supply chain and could be detected at acquisition time, but dependency injection resolution is a runtime architectural mechanism that pre-acquisition vetting cannot monitor. The framework does not mandate instrumentation of dependency injection systems, service resolution tracing, or substitution detection for Semantic Kernel or any other framework. This threat describes a runtime observability gap in framework-specific architecture that is outside the governance profile's scope.
**Score: 1**

### RTM_2_24
Semantic Kernel's orchestrator prompt construction process and the absence of telemetry capturing aggregated prompt content or detecting injected instructions from plugin function descriptions are framework-specific implementation gaps that NIST AI 600-1 does not address. The Information Security category (§2.9) recognizes indirect prompt injection as a risk, providing minimal conceptual relevance since orchestrator prompt pollution is an indirect injection vector, but NIST provides no controls for prompt content logging, context pollution detection, or aggregation-stage telemetry requirements. Pre-deployment testing under MEASURE 2.7 could probe orchestrator prompt construction for injection vulnerabilities, but runtime telemetry for deployed systems is outside the framework's scope. Prompt construction telemetry gaps in Semantic Kernel always score at the minimum level against NIST AI 600-1.
**Score: 1**

### RTM_2_25
LLM-driven plugin routing decision opacity—where telemetry captures the selected plugin but not the reasoning behind routing competition outcomes—is a framework-specific operational monitoring gap that NIST AI 600-1 does not address. The Human-AI Configuration risk category acknowledges the importance of meaningful human oversight, which implicitly requires sufficient decision transparency to enable oversight, but the framework does not mandate routing decision logging with reasoning context for LLM-driven orchestration systems. MEASURE 2.7 pre-deployment evaluation could assess routing decision transparency, but imposes no requirement for runtime reasoning capture in deployed systems. Plugin routing decision opacity is a monitoring implementation detail that falls outside the governance profile's scope.
**Score: 1**

### RTM_2_26
Semantic Kernel exposes telemetry points unique to its orchestration model, and multi-framework systems combining it with other frameworks create observability gaps at integration boundaries—framework-specific monitoring concerns that NIST AI 600-1 explicitly does not cover. The profile's governance scope does not extend to framework-specific telemetry instrumentation, cross-framework boundary monitoring, or reconciliation of heterogeneous observability models across different agent frameworks. GOVERN 6.1/6.2 supply chain vetting applies to Semantic Kernel as a third-party framework component, providing the only marginal connection, but runtime observability gaps at framework boundaries are architectural implementation decisions that pre-acquisition vetting does not address. Framework-specific Semantic Kernel monitoring blind spots always fall outside NIST AI 600-1's governance scope.
**Score: 1**
# NIST AI 600-1 RTM Analysis Cache — RTM_3 and RTM_4

---

### RTM_3_1
NIST AI 600-1 does not address tool invocation telemetry, cross-agent correlation analysis, or detection of covert communication channels encoded in tool-call sequences. The framework's Information Security category (§2.9) names data poisoning and prompt injection as recognized risks, and MEASURE 2.7 pre-deployment security evaluation could probe multi-agent covert channel patterns in controlled scenarios. However, recognizing a risk class before deployment is categorically different from operating telemetry infrastructure capable of correlating invocation sequences spanning multiple agents at runtime. No NIST control addresses the correlation depth, timing analysis, or cross-agent graph analysis required to detect this threat in production.
**Score: 1**

---

### RTM_3_2
Distributed anomaly detection evasion — where each agent individually appears normal while collectively executing an attack — is an operational SIEM problem requiring cross-agent behavioral baseline correlation. NIST AI 600-1 is a governance-level pre-deployment profile and contains no requirements for anomaly detection systems, behavioral baselines, or cross-agent invocation pattern monitoring. The framework's Value Chain and Component Integration category (§2.12) and GOVERN 6.1/6.2 address third-party component vetting at integration time, which is marginally relevant to whether multi-agent architectures are assessed for this attack surface before deployment. Runtime distributed anomaly detection across multiple agents is an operational security engineering function entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_3_3
Tool latency monitoring blind spots arising from parallel multi-agent execution masking slow individual invocations behind aggregated metrics is a performance telemetry design problem. NIST AI 600-1 does not address tool latency monitoring, performance telemetry architecture, or the security implications of aggregated versus per-invocation metrics in parallel execution contexts. The framework's pre-deployment evaluation guidance (MEASURE 2.5, MEASURE 2.6) addresses performance assessment before deployment, but this does not translate into requirements for production latency telemetry granularity or per-agent breakdown of aggregated metrics. No NIST control touches the operational monitoring instrumentation needed to surface individual tool latency anomalies obscured by parallel aggregation.
**Score: 1**

---

### RTM_3_4
Tool output validation gaps across agent boundaries — where outputs validated by one agent are not re-validated by downstream agents — are a multi-agent trust boundary and pipeline integrity problem. NIST AI 600-1's Information Security category (§2.9) recognizes data integrity risks and information integrity is one of the framework's 12 risk areas, providing weak governance framing that organizations should evaluate validation chains. MEASURE 2.7 pre-deployment security testing could exercise cross-agent validation gap scenarios. However, the framework does not mandate runtime re-validation at each agent boundary, specify what output validation must cover, or require telemetry that tracks validation decisions through multi-agent pipelines. The recognition of data integrity risk is the only element of indirect relevance.
**Score: 1**

---

### RTM_3_5
Tool authorization scope audit blind spots — where audit logs show an agent was authorized without capturing that the agent delegated invocation to an unauthorized downstream agent — are a privileged delegation and audit trail design problem. NIST AI 600-1 does not address agent authorization delegation, privilege propagation controls, or audit log requirements for multi-agent tool invocation chains. GOVERN 6.1/6.2 supply chain vetting could require that third-party agent components are assessed for their authorization handling before integration, providing a weak pre-deployment governance framing. However, runtime audit trail completeness for authorization delegation across dynamically composed multi-agent systems is an operational security control the governance profile does not specify or mandate.
**Score: 1**

---

### RTM_3_6
Causality tracking failures in asynchronous tool invocation — where monitoring misattributes invocations to the wrong agent and attackers can spoof causality in logs — are a distributed tracing and log integrity problem. NIST AI 600-1 contains no controls for distributed tracing, causality chain integrity, asynchronous event correlation, or tamper-evident logging in multi-agent systems. The framework's MANAGE 4.1–4.3 incident disclosure policies operate at the institutional reporting layer and do not specify what causal provenance audit logs must preserve. No pre-deployment evaluation guidance in MEASURE addresses whether an agent system's telemetry accurately attributes asynchronous invocations. This threat is entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_4_1
Multimodal processing opacity — where vision model outputs driving tool invocations are not included in monitoring logs — creates a monitoring blind spot that makes agent decisions appear autonomous. NIST AI 600-1 does not address multimodal AI architectures, vision model output logging, or telemetry requirements for image-driven reasoning chains. The framework's Information Integrity risk category broadly covers concerns about AI-generated content accuracy, and MEASURE 2.9 calls for pre-deployment explainability evaluation, which could in principle require that multimodal reasoning be interpretable. However, the framework provides no operational telemetry requirements for vision model outputs, no logging mandates for multimodal inference chains, and no monitoring standards for image-driven tool invocations in production.
**Score: 1**

---

### RTM_4_2
Multimodal attribution opacity in tool invocation tracing — where monitoring cannot distinguish whether tools executed due to text queries, image evidence, or combinations — enables attackers to embed injected instructions in retrieved images while appearing as legitimate text reasoning. NIST AI 600-1 does not address multimodal RAG architectures, cross-modal attribution, or telemetry requirements for distinguishing modality contributions to agent decisions. MEASURE 2.7 pre-deployment security testing could probe multimodal injection scenarios, and §2.9 names prompt injection as a recognized risk, providing weak governance framing that this attack surface should be evaluated before deployment. The absence of runtime attribution telemetry requirements for multimodal systems leaves this threat entirely unaddressed operationally.
**Score: 1**

---

### RTM_4_3
Vision model hallucination detection absence in monitoring — where hallucinated content drives downstream agent tool invocations without any monitoring detecting the fabricated source — is an operational verification problem requiring ground-truth comparison infrastructure. NIST AI 600-1's Information Integrity category and MEASURE 2.9 explainability and validation requirements address hallucination concerns at the pre-deployment evaluation level, providing some governance framing that organizations must assess model accuracy and limitations. However, pre-deployment accuracy evaluation does not constitute runtime hallucination detection telemetry capable of flagging fabricated content propagating through multi-agent pipelines in production. No NIST control mandates cross-agent verification of vision model outputs or telemetry that tracks hallucinated content provenance through dependent agent invocations.
**Score: 1**

---

### RTM_4_4
Embedding similarity threshold opacity in retrieval monitoring — where monitoring does not track whether retrieved content barely exceeded thresholds and attackers craft poisoned embeddings positioned just above those thresholds — is a retrieval system observability problem. NIST AI 600-1 does not address RAG architectures, embedding similarity metrics, retrieval threshold monitoring, or telemetry requirements for content retrieval pipelines. The framework's Data Privacy and Data Quality categories acknowledge data integrity concerns, and MEASURE 2.7 could exercise data poisoning scenarios in pre-deployment testing. However, runtime monitoring of embedding similarity distributions, threshold proximity analysis, and poisoned embedding detection are operational data pipeline security functions entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_4_5
Streaming multimodal output processing monitoring gaps — where dependent agents process partial outputs before full response generation creates ordering dependencies that monitoring cannot validate — are a streaming pipeline architecture and telemetry design problem. NIST AI 600-1 contains no controls for streaming architectures, partial-output processing, or the ordering and consistency guarantees required for multimodal streaming pipelines. No pre-deployment evaluation guidance addresses streaming-specific multi-agent timing vulnerabilities. The framework's scope does not extend to the real-time telemetry infrastructure required to validate cross-modal consistency in streaming multi-agent systems. This threat is entirely outside the governance profile.
**Score: 1**

---

### RTM_4_6
Cross-modal consistency validation absence — where monitoring does not validate whether image content matches text descriptions and cross-modal inconsistencies indicating attacks go undetected — is a multimodal integrity verification problem. NIST AI 600-1's Information Integrity category provides weak governance framing that AI outputs should be accurate and organizations should assess model validity, and MEASURE 2.6 performance testing could evaluate cross-modal consistency in pre-deployment scenarios. However, this does not mandate runtime cross-modal consistency validation, specify what consistency checks monitoring must perform, or require telemetry that flags divergence between image and text modalities in production systems. The pre-deployment integrity framing offers only marginal indirect relevance.
**Score: 1**

---

### RTM_4_7
Error message content opacity — where telemetry captures error messages but lacks semantic analysis of their content, enabling attackers to embed malicious instructions in error messages — is a log analysis and semantic parsing problem. NIST AI 600-1 does not address error message logging architectures, semantic content analysis of log data, or requirements for parsing log content to detect embedded instructions. MANAGE 4.1–4.3 incident response policies implicitly require that error events be logged sufficiently to support incident analysis, providing the weakest governance framing for substantive error logging. However, this falls entirely short of mandating semantic analysis of error message content or telemetry capable of detecting instruction injection within error payloads.
**Score: 1**

---

### RTM_4_8
Retry sequence telemetry gaps — where telemetry shows retry counts and success rates but fails to capture sequence characteristics indicating attacks such as rapid-succession or simultaneous multi-agent retries — are a telemetry granularity and behavioral analytics problem. NIST AI 600-1 does not address retry logic monitoring, sequence-level telemetry, or behavioral anomaly detection in retry patterns. MANAGE 4.1–4.3 incident response policies require organizations to have processes for responding to incidents once detected, but impose no requirements on the telemetry granularity needed to detect retry-pattern attacks. No NIST control addresses the difference between aggregate retry metrics and sequence-level behavioral telemetry required to surface this attack pattern.
**Score: 1**

---

### RTM_4_9
Fallback routing telemetry gaps — where telemetry shows which fallback routes were selected but not what conditions triggered fallback or whether context was manipulated to force routing to compromised alternatives — are an operational observability problem. NIST AI 600-1 does not address fallback routing architectures, context-aware routing telemetry, or monitoring requirements for detecting forced routing to compromised components. The framework's Value Chain and Component Integration category (§2.12) and GOVERN 6.1/6.2 supply chain vetting address third-party component integrity before deployment, providing weak framing that fallback components should be vetted. Runtime telemetry capturing why routing decisions were made and whether context was adversarially manipulated is an operational security engineering function beyond the governance profile.
**Score: 1**

---

### RTM_4_10
Circuit breaker state transition telemetry gaps — where telemetry logs state changes but lacks detailed failure analysis explaining why the circuit opened, missing attack indicators where deliberate failures force state changes — are an operational resilience monitoring problem. NIST AI 600-1 does not address circuit breaker patterns, resilience architecture telemetry, or monitoring requirements for detecting deliberate failure injection. The framework's MANAGE function broadly addresses risk response and incident management at the institutional level, providing negligible framing that operational failures should be monitored and responded to. Runtime detection of adversarially induced circuit breaker state transitions through failure analysis telemetry is an operational security engineering function entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_4_11
Graceful degradation decision opacity — where telemetry shows which capabilities degraded but not why specific degradation decisions were made, enabling attacks that manipulate component health signals to appear as legitimate degradation — is an operational observability problem. NIST AI 600-1 does not address degradation telemetry, component health signal integrity, or monitoring requirements for distinguishing legitimate from adversarially triggered degradation. The framework's MANAGE 4.1–4.3 incident policies require organizations to respond to and disclose incidents, providing no guidance on the telemetry depth needed to detect health signal manipulation. Detection of adversarial degradation trigger manipulation is an operational security monitoring function entirely outside the governance profile.
**Score: 1**

---

### RTM_4_12
Error recovery coordination telemetry gaps — where telemetry captures individual agent recovery actions without cross-agent causality analysis, enabling attacks that trigger cascading recoveries across agents — are a distributed systems observability problem. NIST AI 600-1 does not address multi-agent error recovery coordination, cross-agent causality telemetry, or distributed tracing requirements for recovery workflows. MANAGE 4.1–4.3 incident response policies require institutional-level processes for managing incidents once detected, but impose no requirements on the cross-agent telemetry infrastructure needed to detect coordinated recovery manipulation. No NIST control addresses the distributed tracing depth required to reconstruct error recovery causality across asynchronous multi-agent systems.
**Score: 1**

---

### RTM_4_13
Streaming error handling telemetry gaps — where temporary error states that resolve before response completion leave minimal evidence and attacks exploiting streaming fallback complete before monitoring notices — are a real-time detection timing problem. NIST AI 600-1 contains no controls for streaming error handling telemetry, transient state capture, or real-time detection latency requirements. The framework's pre-deployment security testing (MEASURE 2.7) could in principle probe streaming error handling scenarios in controlled environments, but this does not address the operational detection lag inherent in streaming telemetry systems processing live production traffic. No NIST control addresses the requirement to capture and analyze transient error states before they resolve in streaming multi-agent pipelines.
**Score: 1**

---

### RTM_4_14
Streaming telemetry collection gaps — where complete analysis is only possible after streaming finishes and the detection lag creates vulnerabilities before monitoring completes — are a real-time telemetry architecture and detection pipeline timing problem. NIST AI 600-1 does not address streaming telemetry collection architectures, analysis latency requirements, or the security implications of post-stream-only analysis windows. The framework's pre-deployment evaluation guidance does not replicate the streaming detection lag inherent in production deployments at scale. No NIST governance control mandates real-time streaming telemetry infrastructure, incremental analysis capabilities, or maximum acceptable detection latency for streaming attack patterns.
**Score: 1**

---

### RTM_4_15
Streaming progress indicator manipulation — where attackers controlling streaming speed hide expensive or malicious internal processing while multi-agent monitoring observes only visible progress — is a covert channel and observability deception problem. NIST AI 600-1 does not address streaming progress telemetry, timing-based covert channels, or monitoring requirements for distinguishing visible streaming progress from background multi-agent processing. The framework's Information Security category (§2.9) recognizes that AI systems can be exploited for unintended purposes, providing negligible framing that streaming manipulation might be an attack surface to consider in pre-deployment evaluation. Runtime telemetry correlating streaming progress indicators with background agent processing is an operational monitoring function entirely outside the governance profile.
**Score: 1**

---

### RTM_4_16
Cross-agent streaming correlation failures — where timing variations cause out-of-order log events and attackers manipulate streaming order to fragment traces and prevent causality reconstruction — are a distributed tracing integrity and streaming pipeline ordering problem. NIST AI 600-1 does not address distributed tracing infrastructure, streaming event ordering, correlation ID integrity, or telemetry requirements for cross-agent causality reconstruction in streaming multi-agent systems. No NIST control addresses the log ordering guarantees, vector clock mechanisms, or trace reconstruction algorithms needed to resist adversarial streaming order manipulation. This threat is entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_4_17
Streaming buffer overflow telemetry gaps — where overflow telemetry logs the fact of overflow without capturing discarded content, enabling attackers to stream large content that exploits buffer overflow to discard malicious payloads — are a buffer management and telemetry completeness problem. NIST AI 600-1 does not address streaming buffer architectures, overflow telemetry, or requirements for capturing discarded content during overflow conditions. The framework's Information Security category and data integrity concerns provide negligible framing that buffer overflow as an attack surface could be evaluated in pre-deployment testing. Runtime telemetry capturing the semantic content discarded during buffer overflow events is an operational security engineering function entirely outside the governance profile.
**Score: 1**

---

### RTM_4_18
Streaming rate limiting evasion through distribution — where multi-agent systems distribute streaming below individual rate limits while aggregate throughput exceeds system capacity — is a distributed resource abuse and rate limiting architecture problem. NIST AI 600-1 does not address rate limiting controls, distributed throughput aggregation, or security requirements for multi-agent streaming endpoints. The framework's Environmental Impacts category acknowledges computational resource consumption concerns from an organizational sustainability perspective, but this does not translate into rate limiting requirements or aggregate throughput monitoring controls. Runtime detection of distributed rate limit evasion across coordinated multi-agent streaming is an operational security engineering function entirely outside the governance profile's scope.
**Score: 1**
# NIST AI 600-1 RTM Analysis Cache — RTM_5 and RTM_6

---

## RTM_5 - Evaluation and Assessment Telemetry Gaps

### RTM_5_1
NIST AI 600-1 MEASURE 2.7 calls for security evaluation before deployment, which could surface some evaluation-time anomalies in a controlled test environment, but it does not mandate semantic analysis of metric behaviors or specify any monitoring system that would detect prompt injection indicators within evaluation result trends. The framework operates at the governance and policy layer and contains no requirements for evaluation monitoring systems, anomaly detection tooling, or semantic interpretation of metric outputs. The distinction this threat draws between infrastructure anomaly detection and evaluation-semantic anomaly detection is an operational engineering concern entirely outside NIST's scope. No GOVERN, MAP, MEASURE, or MANAGE action set specifies runtime monitoring of evaluation pipelines or semantic evaluation integrity.
**Score: 1**

---

### RTM_5_2
NIST AI 600-1 does not address evaluation agents as a concept, and contains no provisions for telemetry capturing the internal reasoning of any agent—evaluation or otherwise. MEASURE 2.7 requires pre-deployment evaluation but specifies no logging requirements for the reasoning processes of agents conducting those evaluations, nor any audit trail standards for evaluation decision logic. The Human-AI Configuration risk category acknowledges human oversight concerns, but "evaluation agent reasoning logs" is an operational telemetry engineering problem outside governance-level policy. Without reasoning telemetry mandates, the framework cannot close blind spots where compromised evaluation logic produces legitimately formatted outputs that satisfy surface-level review.
**Score: 1**

---

### RTM_5_3
Evaluation orchestration monitoring across multiple coordinating evaluator agents is a distributed systems observability problem that NIST AI 600-1 does not address. The framework's governance actions do not specify how evaluation workflows must be structured, how inter-agent communication within evaluation pipelines must be logged, or how control flow routing decisions in multi-evaluator systems must be traced. MEASURE 2.7's pre-deployment evaluation guidance implies some orchestrated assessment process, but the framework imposes no visibility requirements on the orchestration layer itself. Control flow hijacking detection in multi-agent evaluation coordination is a runtime monitoring capability with no corresponding NIST control.
**Score: 1**

---

### RTM_5_4
NIST AI 600-1 does not specify what evaluation logs must capture, and provides no requirements for recording metric computation details such as the accuracy definition applied, the test case selection method, or the dataset slice used. MEASURE 2.7 calls for security evaluation before deployment but does not mandate that evaluation audit trails include sufficient detail to reconstruct the specific computational path that produced a reported metric. The framework's incident disclosure policies (MANAGE 4.1–4.3) operate at an institutional reporting level and do not specify technical log content requirements. Attacks exploiting ambiguous metric definitions exploit a documentation and audit completeness gap that NIST AI 600-1 leaves entirely unaddressed.
**Score: 1**

---

### RTM_5_5
NIST AI 600-1 explicitly does not address framework-specific vulnerabilities or implementation-level details of evaluation frameworks such as LLM-based evaluators. The framework's supply chain provisions (GOVERN 6.1/6.2) require vetting third-party components, which could marginally apply if an evaluation framework is a third-party acquisition, but vetting addresses provenance and trustworthiness at acquisition time rather than implementation-level monitoring during runtime evaluation execution. Pre-deployment testing under MEASURE 2.7 could in principle probe evaluation framework inputs for prompt injection, but the framework establishes no requirement or methodology for this. Evaluation framework implementation monitoring is outside NIST AI 600-1's governance-level scope.
**Score: 1**

---

### RTM_5_6
NIST AI 600-1 does not specify audit trail content requirements for evaluation processes, and contains no provisions requiring that evaluation audit trails capture intermediate agent contributions as distinct from final aggregated results. MANAGE 4.1–4.3 establishes institutional incident disclosure policies that assume post-incident investigation is possible, but the framework does not mandate the technical audit infrastructure needed for forensic reconstruction of which agent's compromise influenced a biased evaluation outcome. MEASURE 2.7's pre-deployment evaluation guidance addresses what should be tested, not how audit completeness should be enforced. The forensic gap between logged final results and missing intermediate agent-level evaluation steps is an audit engineering problem outside the framework's scope.
**Score: 1**

---

### RTM_5_7
NIST AI 600-1 recognizes data poisoning as a risk within the Data Privacy category and Information Security category, and MEASURE 2.5 addresses data quality. However, these provisions address training data integrity concerns and do not mandate continuous integrity monitoring of evaluation datasets during operational use, change detection mechanisms, or cryptographic verification of dataset contents between evaluation runs. The framework's pre-deployment orientation means MEASURE 2.7 evaluates using whatever dataset is present at test time without specifying how the integrity of that dataset should be verified or monitored across time. Evaluation dataset integrity monitoring as a runtime operational control is outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_5_8
NIST AI 600-1 does not address behavioral baselining for evaluation agents or any runtime monitoring concept that would distinguish normal from anomalous evaluation agent behavior. MEASURE 2.7 specifies that security evaluations should occur before deployment but does not require establishing behavioral baselines against which future evaluation agent actions could be compared. The framework's governance actions cover oversight policies and risk categorization but contain no requirements for anomaly detection systems, baseline establishment procedures, or behavioral drift monitoring for any agent type. Without baseline requirements, the precondition for detecting attack-driven evaluation manipulation via behavioral deviation is absent from the framework.
**Score: 1**

---

### RTM_5_9
NIST AI 600-1 is a governance framework that does not address temporal analysis of evaluation patterns across multiple runs over time. MEASURE 2.7 specifies pre-deployment evaluation as a gating activity but does not mandate longitudinal tracking of evaluation results, trend analysis across evaluation runs, or detection of scheduled metric improvement patterns indicative of evaluation gaming. The framework's incident disclosure provisions (MANAGE 4.1–4.3) are reactive institutional policies and do not specify proactive temporal monitoring of evaluation pipelines. Cross-evaluation temporal pattern analysis is an advanced monitoring analytics capability with no corresponding NIST AI 600-1 control.
**Score: 1**

---

### RTM_5_10
NIST AI 600-1 does not address the security of metric telemetry transmission, does not mandate encryption or authentication for evaluation reporting channels, and contains no provisions for detecting spoofed metric reports. The Information Security risk category (§2.9) recognizes prompt injection as a threat vector, and an attacker injecting into centralized telemetry is exploiting that class of vulnerability, but NIST's acknowledgment does not extend to telemetry channel integrity requirements or anti-spoofing controls for evaluation reporting infrastructure. MEASURE 2.7's pre-deployment evaluation guidance does not specify requirements for securing the telemetry pipeline through which evaluation results are reported. Multi-agent telemetry spoofing enabling single-injection attacks on centralized monitoring is a runtime security engineering problem entirely outside the framework's scope.
**Score: 1**

---

### RTM_5_11
NIST AI 600-1 does not distinguish between production code monitoring and evaluation script monitoring, and contains no provisions requiring that evaluation infrastructure receive the same monitoring rigor as production systems. MEASURE 2.7 addresses what pre-deployment evaluations should cover but does not specify operational monitoring requirements for evaluation scripts themselves. The framework cannot close monitoring blind spots that arise from organizational practices of treating evaluation code as less security-critical than production code, as this is an operational security governance decision the framework does not mandate. Resource exhaustion and evaluation timeout attacks on under-monitored evaluation infrastructure are implementation-level security engineering concerns outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_5_12
NIST AI 600-1 does not mandate tamper-evident logging, immutable audit trails, or timestamp integrity verification for evaluation logs. MANAGE 4.1–4.3 establishes institutional incident disclosure policies that implicitly assume audit logs are trustworthy for post-incident investigation, but the framework specifies no technical controls to ensure log immutability, no requirements for write-access restrictions on evaluation logs, and no cryptographic integrity protections. The gap between policy-level acknowledgment that audit trails matter and technical mandates for protecting those trails from agent-level tampering is not bridged within the framework. Evaluation log tamper resistance is an infrastructure security engineering concern outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_5_13
NIST AI 600-1 does not address monitoring alert threshold design, threshold calibration methodology, or the risk of gradual metric degradation strategies evading discrete alert boundaries. MEASURE 2.7's pre-deployment evaluation cannot anticipate the specific threshold values that production monitoring systems will use, and the framework provides no guidance for setting thresholds that resist slow-ramp evasion strategies. The multi-agent variant of this threat—where degradation is distributed across agents to keep each below individual thresholds—is an architectural monitoring design challenge that governance-level policy cannot specify. Alert threshold calibration against gradual and distributed degradation attacks is an operational monitoring engineering problem entirely outside the framework's scope.
**Score: 1**

---

### RTM_5_14
NIST AI 600-1 does not address metric correlation analysis, multi-metric anomaly detection, or the detection of coordinated evaluation degradation patterns that appear as uncorrelated individual failures. MEASURE 2.7's pre-deployment evaluation assesses model performance but does not require evaluation frameworks to implement cross-metric correlation analysis or detect coordinated multi-metric attack patterns. The framework's governance actions do not specify what analytical capabilities evaluation systems must possess for detecting distributed attacks. Metric correlation analysis as an attack detection methodology is an advanced evaluation analytics capability with no corresponding NIST AI 600-1 requirement.
**Score: 1**

---

### RTM_5_15
NIST AI 600-1 does not address benchmark execution telemetry, unified tracing across distributed agent-specific benchmark logs, or inter-agent instruction correlation during benchmark runs. MEASURE 2.7 calls for benchmark-based evaluation before deployment but does not specify how benchmark telemetry should be collected, correlated, or structured to ensure visibility at agent boundaries where inter-agent instruction passing occurs. The absence of a unified telemetry layer requirement leaves benchmark execution traceability as an architectural design decision that the framework does not mandate. Multi-agent benchmark telemetry gaps enabling attack concealment at agent communication boundaries are operational monitoring engineering problems outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_5_16
NIST AI 600-1 does not address user feedback monitoring systems, feedback attribution architectures, or the problem of misattributing system failures to the final responding agent rather than upstream agents. MANAGE 4.1–4.3 incident disclosure policies are institutional-level mechanisms that do not specify how feedback signals must be collected, attributed, or analyzed within multi-agent systems. MEASURE 2.7's pre-deployment evaluation could in principle test multi-agent failure attribution, but the framework imposes no requirement for attribution-aware feedback monitoring in production. The structural misattribution problem in multi-agent feedback pipelines is an operational monitoring design issue entirely outside the framework's scope.
**Score: 1**

---

### RTM_5_17
NIST AI 600-1 does not address compound degradation detection across coordinated multi-agent systems or the monitoring design challenge of gradual synchronized degradation that stays below per-agent thresholds while creating compound effects exceeding system-level thresholds. MEASURE 2.7 evaluates individual system configurations before deployment and does not mandate system-level aggregate performance monitoring in production or thresholds calibrated for coordinated multi-agent degradation scenarios. The framework's human oversight provisions (GOVERN 3.2) could require that meaningful oversight exist, but this does not specify the technical monitoring design needed to detect compound multi-agent degradation. Coordinated gradual multi-agent degradation detection is an advanced operational monitoring capability with no corresponding NIST AI 600-1 control.
**Score: 1**

---

## RTM_6 - Metrics Collection and Manipulation Evasion

### RTM_6_1
NIST AI 600-1 MANAGE 4.1–4.3 establishes institutional-level incident disclosure policies and acknowledges the need for audit trails, but these provisions do not mandate unified audit chain architectures spanning multiple agents or specify requirements for cross-agent decision traceability. The framework's governance-level approach to audit trails assumes institutional reporting processes, not technical specifications for audit log architecture in multi-agent approval workflows. Pre-deployment evaluation under MEASURE 2.7 could test approval workflow integrity, but no NIST control requires that production audit trails capture a complete unified decision chain across agent boundaries. Audit trail fragmentation enabling attack concealment in multi-agent approval systems is a technical audit engineering problem outside the framework's scope.
**Score: 1**

---

### RTM_6_2
NIST AI 600-1 does not address timing vulnerabilities in multi-agent web navigation, temporal synchronization requirements between agent actions and page state verification, or detection of injection attacks exploiting timing windows. The Information Security risk category (§2.9) recognizes prompt injection as a threat, and injecting instructions during page-state timing gaps is a form of that attack, but NIST's recognition does not extend to operational controls for temporal synchronization in web-interacting agents. MEASURE 2.7's pre-deployment testing could probe timing-based injection scenarios, but the framework provides no requirements for runtime temporal integrity monitoring in multi-agent web navigation. Temporal synchronization gap exploitation is a runtime security engineering concern entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_3
NIST AI 600-1 does not address monitoring parameter configuration across multi-agent systems or the risk of asymmetric instrumentation creating blind spots where some agents have reduced monitoring coverage. The framework's governance actions specify that AI risk management policies should exist but do not mandate uniform instrumentation standards across all agents in a deployment. No NIST control addresses monitoring parameter tuning as an attack surface, requires consistency checks across agent instrumentation levels, or specifies minimum instrumentation coverage requirements. Asymmetric monitoring parameter configurations creating cross-agent observability blind spots is an operational monitoring infrastructure concern outside the framework's governance scope.
**Score: 1**

---

### RTM_6_4
NIST AI 600-1 does not address confidence score generation, confidence calibration methods, or the use of confidence metrics as monitoring evasion surfaces. MEASURE 2.7's pre-deployment evaluation could in principle probe confidence calibration accuracy, but the framework does not require that confidence generation systems be hardened against adversarial tuning designed to produce high-confidence wrong answers that satisfy automated monitoring thresholds. The framework contains no provisions for detecting confidence score manipulation, requiring calibration validation, or monitoring for divergence between stated confidence and actual accuracy in production. Confidence score tuning as a monitoring evasion mechanism is a runtime security concern outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_6_5
NIST AI 600-1 does not address latency monitoring, performance metric gaming, or adversarial operation crafting designed to exploit optimized code paths for evasion. The framework's governance and risk management focus does not extend to operational telemetry design, latency measurement methodology, or technical controls against operations that produce deceptively favorable performance metrics while executing malicious logic. MEASURE 2.7's pre-deployment security evaluation could test for malicious operations, but it does not address the specific attack vector of crafting operations to appear performant under latency monitoring. Latency metric gaming through optimized code path exploitation is a runtime security engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_6
NIST AI 600-1 recognizes prompt injection as a risk within the Information Security category (§2.9), and demonstration poisoning through strategic ordering is a variant of that attack vector, providing a marginal connection. MEASURE 2.7's pre-deployment evaluation could test few-shot prompt robustness including demonstration ordering effects, which might surface recency-bias exploitation in controlled scenarios. However, the framework provides no specific controls for demonstration sequence integrity, no requirements for ordering-aware prompt evaluation, and no monitoring mandates for detecting adversarial demonstration positioning in production. The multi-agent variant—positioning poisoned demonstrations at agent boundary junctures to escape single-agent monitoring—is an operational monitoring gap with no NIST control.
**Score: 1**

---

### RTM_6_7
NIST AI 600-1 does not address asynchronous log aggregation timing, the detection windows created by aggregation latency, or technical controls ensuring that attack-correlated log entries are correlated before attackers can exploit the aggregation gap. MANAGE 4.1–4.3's institutional incident disclosure policies assume that audit trails can support post-incident investigation but do not specify real-time log aggregation requirements, maximum aggregation latency standards, or architectural requirements for closing temporal correlation gaps. The multi-agent asynchronous aggregation variant—where attackers commit attacks before cross-agent logs have been correlated—is an advanced distributed systems monitoring problem entirely outside NIST AI 600-1's governance scope. No NIST control mandates synchronous audit log availability or temporal correlation guarantees.
**Score: 1**

---

### RTM_6_8
NIST AI 600-1 does not address tool invocation observability, delegation hierarchy tracing, or the requirement that audit trails preserve the full causal context—including the user request chain—for tool calls made by worker agents. MANAGE 4.1–4.3's incident disclosure policies implicitly rely on audit trails being sufficient for investigation, but the framework specifies no requirements for semantic context capture in tool invocation logs or causal chain completeness across delegation boundaries. The semantic gap between logged tool calls and the absence of the user-request context that motivated them is an audit log design problem that governance-level policy does not address. Tool invocation traceability through agent delegation hierarchies is a technical audit engineering concern outside the framework's scope.
**Score: 1**

---

### RTM_6_9
NIST AI 600-1 does not address tool quality metrics, metric aggregation methodology for tool performance evaluation, or the risk of compromised agents inflating quality metrics to skew aggregated scores used for tool lifecycle decisions. GOVERN 6.1/6.2 supply chain vetting applies to third-party tool provenance at acquisition time but does not address the runtime integrity of quality metrics that agents report about tools. MEASURE 2.7's pre-deployment evaluation could assess tool performance but does not mandate continuous production monitoring of tool quality metric integrity. Aggregation bias attacks on tool quality metrics through compromised agent reporting are a runtime metrics manipulation concern entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_10
NIST AI 600-1 does not address per-agent versus cross-agent tool selection monitoring or the detection of adversarial tool sequences constructed across agent boundaries to evade single-agent monitors. The framework's governance provisions do not specify tool access policy monitoring architectures, cross-agent tool selection correlation requirements, or anomaly detection for tool sequencing patterns. GOVERN 6.1/6.2 vets third-party tools at acquisition but does not address how tool usage patterns should be monitored across agent specialization boundaries in production. Cross-agent tool selection pattern attacks that evade per-agent monitors are a distributed monitoring design problem entirely outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_6_11
NIST AI 600-1 does not address entropy-based hallucination detection, confidence aggregation across multi-agent pipelines, or the monitoring blind spot created when high-confidence downstream validation masks high-entropy upstream generation. MEASURE 2.7's pre-deployment evaluation could assess hallucination rates in controlled settings, but the framework does not mandate entropy monitoring in production or require that multi-agent systems preserve upstream uncertainty signals through downstream confidence aggregation. The specific attack vector—exploiting downstream confidence to mask upstream entropy from hallucination detection—is a runtime monitoring architecture problem with no corresponding NIST control. Entropy tracking evasion through agent confidence aggregation is outside the framework's governance scope.
**Score: 1**

---

### RTM_6_12
NIST AI 600-1 does not address asynchronous tool validation architectures, the coverage gaps created when tool invocation and validation are decoupled across agents, or detection requirements for tools that execute many times before asynchronous validation catches compromise. GOVERN 6.1/6.2 supply chain vetting addresses tool trustworthiness at acquisition but does not mandate synchronous validation or specify maximum acceptable validation lag for deployed tools. MANAGE 4.1–4.3 incident disclosure policies do not address the pre-detection execution window that asynchronous validation creates. The attack surface of repeated compromised tool execution before asynchronous validation completes is a runtime security architecture concern entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_13
NIST AI 600-1 does not address cross-agent anomaly detection, the requirement to correlate individual agent behavior patterns for system-level anomaly identification, or the blind spot created when individually normal agents collectively execute an attack. The framework's governance provisions operate at the organizational policy level and do not specify monitoring architectures capable of detecting cross-agent coordination attacks that are invisible to per-agent monitors. MEASURE 2.7's pre-deployment evaluation tests individual or aggregate system behavior but does not mandate the cross-agent correlation infrastructure needed for detecting coordinated attacks where each agent's individual behavior appears legitimate. Cross-agent context correlation in anomaly detection is a distributed monitoring analytics capability outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_6_14
NIST AI 600-1 does not address action accuracy metrics, aggregated accuracy computation across multi-agent chains, or production monitoring requirements for detecting accuracy degradation at agent boundaries. MEASURE 2.7 calls for pre-deployment accuracy evaluation but does not mandate continuous production monitoring of action accuracy, nor does it specify that multi-agent systems must compute accuracy metrics aggregated across the full agent chain rather than per agent. The framework's governance provisions do not distinguish between per-agent and cross-agent accuracy metrics as monitoring requirements. Action accuracy degradation at agent boundaries going undetected due to per-agent-only logging is a production monitoring design problem entirely outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_6_15
NIST AI 600-1 does not address parameter-type-specific accuracy monitoring, the risk of aggregate metrics hiding per-parameter-category failures, or requirements for disaggregated accuracy reporting across parameter types in multi-agent systems. MEASURE 2.7's pre-deployment evaluation could test performance across parameter categories but does not mandate that production monitoring maintain parameter-type-disaggregated accuracy metrics or flag cases where aggregate acceptability masks category-specific failures. The framework's governance and risk management provisions do not specify the granularity of accuracy metrics that production systems must maintain. Parameter validation failure hidden by aggregate accuracy metrics is a monitoring granularity design problem outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_16
NIST AI 600-1 does not address tool execution success rate monitoring, the aggregation dimensions by which such metrics should be computed, or the risk of tool-aggregated success rates hiding agent-specific parameter accuracy problems. The framework's governance provisions do not specify monitoring metric design requirements or mandate that success rate metrics be computed along agent dimensions in addition to tool dimensions. MEASURE 2.7's pre-deployment evaluation assesses tool performance but does not prescribe the aggregation structure of production tool execution metrics. Tool execution errors hidden in tool-aggregated success rates—rather than agent-aggregated rates—is a metrics design problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_17
NIST AI 600-1 does not address multi-turn consistency monitoring, cross-agent session coherence requirements, or production monitoring for consistency degradation propagating across agent boundaries. MEASURE 2.7's pre-deployment evaluation could test multi-turn consistency in controlled scenarios but does not mandate production consistency monitoring or specify that multi-agent systems track cross-agent conversational coherence in addition to per-agent consistency. The framework's governance provisions do not address session state management monitoring architectures or the downstream impact of one agent's consistency degradation on subsequent agents in a chain. Cross-agent multi-turn consistency loss going undetected is a production monitoring design problem outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_6_18
NIST AI 600-1 does not address distributed error recovery monitoring, cross-agent retry chain tracking, or the requirement that monitoring systems verify whether error recovery succeeded across the full recovery chain rather than at the initiating agent. MANAGE 4.1–4.3 incident disclosure policies address post-incident reporting but do not specify technical requirements for tracking distributed error recovery state in multi-agent systems. The framework's governance provisions do not mandate recovery observability requirements or distributed tracing standards that would make cross-agent recovery success or failure visible. Error recovery pattern invisibility in distributed tracing across agent coordination is a monitoring architecture problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_19
NIST AI 600-1 does not address trajectory metrics, full action sequence evaluation across agent boundaries, or requirements for computing end-to-end trajectory quality in multi-agent systems. MEASURE 2.7's pre-deployment evaluation could assess trajectory quality in controlled test scenarios, but the framework does not mandate continuous production monitoring of trajectory metrics or require that multi-agent systems instrument the complete action sequence from start to finish across agent boundaries. The governance provisions do not distinguish between local action quality and global trajectory quality as monitoring requirements. Cross-agent trajectory metric computation being rarely implemented is a monitoring coverage gap outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_20
NIST AI 600-1 does not address system-level reasoning coherence validation, cross-agent reasoning consistency requirements, or mechanisms for validating that multi-agent reasoning chains are globally coherent rather than only locally valid at each agent. MEASURE 2.7's pre-deployment evaluation assesses individual AI components but does not mandate system-level coherence validation across agent boundaries or specify evaluation methodologies for detecting distributed reasoning failures that appear locally acceptable. The framework's governance provisions do not distinguish between per-agent reasoning quality and multi-agent system-level reasoning coherence as evaluation requirements. System-level coherence validation as a distinct monitoring requirement is outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_6_21
NIST AI 600-1 MEASURE 2.7 explicitly addresses pre-deployment reasoning quality evaluation, providing a thin institutional hook for organizations that evaluate reasoning quality before release. However, the framework does not mandate continuous production monitoring of reasoning quality, does not require that pre-deployment evaluation standards be replicated in production monitoring, and does not address the drift that occurs when reasoning quality gradually degrades after deployment. The gap this threat describes—organizations evaluating reasoning in development but lacking continuous production monitoring—represents precisely the boundary between NIST's pre-deployment governance scope and operational monitoring requirements the framework does not cover.
**Score: 1**

---

### RTM_6_22
NIST AI 600-1 MEASURE 2.7 calls for security evaluation including adversarial testing before deployment, which provides a marginal connection to the requirement for testing error conditions and adversarial scenarios beyond happy-path reasoning. The framework acknowledges that red-teaming and adversarial probing should occur pre-deployment, which in principle could expand evaluation scope beyond happy-path scenarios to include error condition reasoning and adversarial inputs. However, NIST does not specify the breadth or methodology of adversarial reasoning evaluation, does not mandate coverage of specific adversarial scenario categories, and provides no continuous production monitoring requirement. The pre-deployment scope limitation means MEASURE 2.7 cannot address reasoning quality evaluation gaps in production.
**Score: 1**

---

### RTM_6_23
NIST AI 600-1 does not address reasoning chain attribution, mechanisms for tracing individual reasoning contributions to specific agents in multi-agent systems, or audit trail requirements that would support forensic analysis of which agent's reasoning caused a system-level failure. MANAGE 4.1–4.3 incident disclosure policies assume post-incident investigation is possible but do not specify technical attribution requirements for reasoning chains across multi-agent systems. The framework's governance provisions do not mandate reasoning traceability standards or require that multi-agent systems log reasoning contributions with sufficient granularity for attribution. Reasoning attribution and traceability gaps in multi-agent systems are a forensic audit engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_24
NIST AI 600-1 does not address efficiency metric aggregation methodology, the risk of cross-agent aggregation obscuring per-agent attack signals, or requirements for maintaining disaggregated efficiency metrics alongside aggregated summaries. MEASURE 2.7's pre-deployment evaluation could assess efficiency in controlled scenarios but does not mandate production efficiency monitoring or specify aggregation granularity requirements that would preserve attack signals that disappear in aggregate metrics. The framework's governance provisions do not distinguish between acceptable aggregate efficiency performance and the monitoring granularity needed to detect per-agent attacks embedded within acceptable aggregate metrics. Efficiency metric aggregation obscuring attack signals is a monitoring design problem outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_25
NIST AI 600-1 does not address sampling-based monitoring methodologies, the coverage gaps created by sampling rates insufficient to capture low-frequency attack operations, or requirements for adaptive sampling that ensures attack-crafted operations do not consistently fall outside the sampled population. The framework's governance provisions do not specify production monitoring methodology, do not address sampling theory requirements for security monitoring, and do not mandate coverage guarantees for efficiency monitoring instrumentation. MEASURE 2.7's pre-deployment evaluation tests using deliberately constructed scenarios and does not expose sampling-based coverage gaps that emerge only under high-volume production conditions. Sampling-based monitoring blind spots enabling crafted unsampled attack operations are outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_6_26
NIST AI 600-1 does not address latency reporting methodology, the choice between percentile-based and extremes-based reporting, or the security implications of reporting conventions that render attack-triggered latency spikes invisible when they affect only a small fraction of operations. The framework's governance provisions do not specify production monitoring metric reporting standards or mandate that monitoring systems preserve attack-relevant signal that percentile aggregation would discard. MEASURE 2.7's pre-deployment evaluation tests typical performance scenarios and does not address the monitoring reporting conventions that would make rare but attack-relevant latency spikes visible in production. Latency percentile reporting masking attack spikes is a monitoring methodology design problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_27
NIST AI 600-1 does not address cost attribution systems, attribution latency requirements, or the attack window created when cost attribution lags actual usage by hours or days enabling resource exhaustion before attribution reveals the problem. The framework's governance provisions do not specify financial monitoring requirements, cost attribution timeliness standards, or real-time resource consumption monitoring mandates. MANAGE 4.1–4.3 incident disclosure policies address after-the-fact reporting but do not close the pre-attribution exploitation window by mandating near-real-time cost attribution. Cost attribution lag as a resource exhaustion attack enabler is an operational financial monitoring engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_28
NIST AI 600-1 does not address alert threshold design philosophy, the distinction between discrete threshold alerting and gradient-aware monitoring, or requirements for detecting gradual resource degradation that cumulatively exhausts budgets while staying below discrete alert boundaries. The framework's governance provisions do not specify production alerting system design requirements or mandate monitoring approaches capable of detecting slow-burn resource exhaustion attacks. MEASURE 2.7's pre-deployment evaluation tests against specific scenarios and does not address the monitoring system design needed to detect gradual degradation in production. Discrete alert threshold insensitivity to gradual cumulative degradation is a monitoring system design problem outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_29
NIST AI 600-1 does not address multi-metric correlation analysis for efficiency monitoring, requirements for correlating simultaneous anomalies across multiple efficiency metrics, or detection of coordinated attacks visible only through cross-metric correlation. The framework's governance provisions do not specify monitoring analytics requirements or mandate correlation analysis across efficiency metric dimensions. MEASURE 2.7's pre-deployment evaluation could test for coordinated efficiency degradation scenarios, but the framework provides no requirement that production monitoring implement cross-metric correlation analysis for efficiency signals. Efficiency metric correlation analysis as an attack detection capability is an advanced monitoring analytics problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_30
NIST AI 600-1 does not address efficiency baseline management, baseline staleness risks, or the requirement that baselines be updated as systems evolve to avoid false negatives from outdated reference points. The framework's governance provisions do not specify baseline management requirements for production monitoring systems or mandate procedures for baseline refresh cadence. The multi-agent variant—where attackers poison shared baselines to render all agents' monitoring ineffective—is a distributed monitoring infrastructure attack with no corresponding NIST control. Historical baseline staleness creating false negatives and shared baseline poisoning in multi-agent systems are operational monitoring maintenance problems entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_6_31
NIST AI 600-1 does not address telemetry processing latency, detection lag introduced by asynchronous telemetry pipelines, or requirements for detection speed that would close the window between attack execution and monitoring response. The framework's governance provisions operate at the policy level and do not mandate real-time or near-real-time telemetry processing standards. MANAGE 4.1–4.3 incident disclosure policies address after-the-fact reporting and do not specify requirements for minimizing detection lag. The attack strategy of executing faster than monitoring can detect and respond is an operational monitoring responsiveness problem that governance-level policy cannot address, and it is entirely outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_6_32
NIST AI 600-1 does not address instrumentation coverage requirements, does not mandate that all operations be instrumented for efficiency monitoring, and provides no requirements preventing partial instrumentation from creating exploitable blind spots. The framework's governance provisions do not specify operational monitoring coverage standards or mandate instrumentation completeness audits for deployed systems. GOVERN 6.1/6.2 supply chain vetting addresses third-party component trustworthiness but does not extend to instrumentation coverage requirements for those components once deployed. The multi-agent variant—where attackers identify and target agents whose operations lack instrumentation—is a distributed monitoring coverage exploitation problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**
# NIST AI 600-1 RTM Analysis Cache — RTM_7 (Infrastructure and Observability Stack Blind Spots)

---

### RTM_7_1
NIST AI 600-1 is a governance-level risk management profile and does not address telemetry attribution architectures, distributed tracing systems, or the per-agent attribution of tool behavior in shared infrastructure. The framework has no controls targeting monitoring cardinality, tool-level latency attribution, or the engineering of observability pipelines that can isolate individual agent contributions to shared tool performance. GOVERN 6.1/6.2 supply chain vetting addresses whether tool providers are trustworthy sources but does not govern how tool invocations are instrumented and attributed at runtime. Tool behavior attribution loss in multi-agent shared tooling is a distributed systems observability engineering problem that falls entirely outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_7_2
Message queue consumer lag monitoring and per-agent lag breakdown are operational infrastructure concerns that NIST AI 600-1 does not address at any level. The framework contains no controls for queue-based asynchronous communication patterns, consumer group telemetry, or the differentiation of throughput pressure from processing failures in shared message queues. MANAGE 4.1–4.3 establishes institutional incident disclosure policies but does not mandate the technical monitoring depth required to distinguish between agent processing failures and legitimate high-throughput conditions. Consumer lag visibility gaps in multi-agent shared message queues are a monitoring infrastructure engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_7_3
NIST AI 600-1 does not address vector database infrastructure, embedding quality monitoring, retrieval performance telemetry, or the detection of semantic drift caused by poisoned embeddings or index staleness. The Information Security risk category (§2.9) recognizes data poisoning as a threat, and embedding poisoning is a form of data poisoning, but the framework's acknowledgment does not extend to operational controls for retrieval quality monitoring or latency-based anomaly detection in vector stores. MEASURE 2.7 pre-deployment security evaluation could in principle assess retrieval system integrity, but the framework provides no methodology for runtime semantic drift detection in shared vector databases serving multi-agent systems. Vector database observability gaps are an infrastructure monitoring problem outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_4
Prometheus scrape success rate monitoring and the distinction between healthy metric endpoints and malfunctioning agents exposing incorrect data are operational monitoring engineering concerns that NIST AI 600-1 does not cover. The framework provides no guidance on metrics endpoint integrity, synthetic validation of scraped metric accuracy, or the detection of agents that report normal metrics while behaving abnormally. MEASURE 2.7 pre-deployment evaluation could test agent metric reporting accuracy in controlled conditions, but runtime Prometheus scrape fidelity validation is an observability infrastructure problem beyond the governance profile. No NIST AI 600-1 control addresses the semantic gap between a successful metrics scrape and the correctness of the metrics content returned.
**Score: 1**

---

### RTM_7_5
API gateway request success rate metrics and the gap between HTTP-level success codes and application-level tool invocation outcomes are operational monitoring concerns that NIST AI 600-1 does not address. The framework has no controls for semantic response validation, application-layer error detection within HTTP 200 envelopes, or the instrumentation of tool invocation pipelines to surface functional failures masked by transport-layer success. GOVERN 6.1/6.2 can require vetting of third-party API gateway components at acquisition time, but this does not govern how gateway metrics are interpreted or what additional telemetry is required to detect tool invocation failures invisible to HTTP status codes. API gateway observability gaps are an infrastructure monitoring engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_6
MLflow deployment success events and the gap between deployment completion and model performance regression detection are MLOps infrastructure concerns that NIST AI 600-1 does not address. The framework has no controls for model performance monitoring post-deployment, regression detection in multi-agent model rollouts, or the operational telemetry required to distinguish a successful deployment from a deployment that degrades system performance. MEASURE 2.7 pre-deployment evaluation assesses models before release but explicitly does not govern post-deployment runtime performance monitoring. Multi-agent simultaneous model deployment patterns amplify this blind spot in ways that fall entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_7_7
Message queue throughput metrics and the inability of volume-based metrics to reveal content quality degradation from poisoned messages are operational monitoring concerns that NIST AI 600-1 does not address. The Information Security category (§2.9) recognizes data poisoning as a risk, but the framework provides no controls for content-level quality monitoring within message queues, semantic validation of message payloads, or detection of poisoned content propagating at high throughput. MEASURE 2.7 pre-deployment testing could probe message poisoning scenarios in controlled environments but does not mandate runtime content quality monitoring in production queue infrastructure. Queue throughput observability gaps that mask content-level attacks are an infrastructure monitoring problem entirely outside the governance-level profile.
**Score: 1**

---

### RTM_7_8
Prometheus alert suppression mechanics and the cognitive blindness they create in orchestrators relying on alert presence as a signal of ongoing issues are observability engineering concerns that NIST AI 600-1 does not address. The framework contains no controls for alert management systems, suppression policy design, or the secondary effects of alert suppression on AI orchestrator decision-making in multi-agent systems. MANAGE 4.1–4.3 incident response policies assume that alerts and monitoring signals are available and reliable, but the framework does not govern the technical design of alerting infrastructure or the failure modes introduced by standard alert suppression patterns. Alert suppression as a monitoring blind spot in multi-agent orchestration is an infrastructure engineering problem outside the governance profile's scope.
**Score: 1**

---

### RTM_7_9
API gateway rate limit exhaustion masking underlying tool timeout failures is an operational monitoring diagnosis problem that NIST AI 600-1 does not address. The framework provides no controls for distinguishing traffic management responses from infrastructure failure signals, root cause attribution in multi-agent API call patterns, or the instrumentation required to differentiate rate limiting from tool timeout cascades. GOVERN 6.1/6.2 supply chain vetting can assess gateway providers at acquisition time but does not govern how gateway error signals are interpreted or correlated with tool-level timeout telemetry at runtime. Rate limit and timeout signal conflation is an infrastructure monitoring engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_10
MLflow metrics aggregation hiding individual agent regressions beneath improved aggregate performance is an MLOps observability engineering concern that NIST AI 600-1 does not address. The framework has no controls for per-agent metric disaggregation, regression detection methodologies within multi-agent deployments, or the evaluation of aggregation-induced blind spots in model performance monitoring infrastructure. MEASURE 2.7 pre-deployment evaluation assesses model performance before release in controlled conditions but does not mandate disaggregated per-agent monitoring or regression detection strategies for multi-agent production deployments. MLflow aggregation blind spots are an MLOps infrastructure monitoring problem entirely outside the governance-level profile.
**Score: 1**

---

### RTM_7_11
Metrics aggregation operations averaging across multi-agent deployments and the resulting obscuring of outlier agent behavior are observability engineering concerns that NIST AI 600-1 does not address. The framework provides no controls for statistical monitoring methodologies, outlier detection in agent metric distributions, or the design of monitoring aggregations that preserve visibility into individual agent anomalies. MANAGE 4.1–4.3 incident response policies assume effective monitoring is in place but do not specify what monitoring architectures are required or how aggregation functions should be chosen to preserve security-relevant signal. Aggregation-induced outlier blindness in multi-agent monitoring is an infrastructure engineering problem outside the governance profile's scope.
**Score: 1**

---

### RTM_7_12
Pod-level alert volume overwhelming operators and enabling attackers to hide service-level malicious behavior beneath benign pod-level alert noise is an alert management and operational engineering concern that NIST AI 600-1 does not address. The framework has no controls for alert hierarchy design, signal-to-noise ratio management in multi-agent deployments, or the operational security implications of alert fatigue in large-scale Kubernetes-based agent infrastructure. MANAGE 4.1–4.3 incident disclosure policies operate at the institutional reporting layer and do not govern the operational alert management practices or infrastructure telemetry architectures that create alert fatigue conditions. Alert fatigue enabling attack concealment is an infrastructure operations problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_13
Canary deployment metric evaluation and the statistical insufficiency of per-agent sample sizes in distributed multi-agent canary rollouts are MLOps and statistical engineering concerns that NIST AI 600-1 does not address. The framework provides no controls for canary evaluation methodologies, statistical significance requirements for deployment gating, or the design of canary strategies that account for sample dilution across large multi-agent fleets. MEASURE 2.7 pre-deployment evaluation could assess model performance before any deployment, but canary rollout statistical validity in production multi-agent systems is an operational engineering problem beyond the governance profile. Insufficient canary sample size creating evaluation blind spots is an infrastructure monitoring engineering problem outside NIST AI 600-1's scope.
**Score: 1**

---

### RTM_7_14
Distributed tracing performance overhead creating instrumentation bias — where heavily-instrumented agents behave differently from lightly-instrumented agents — is an observability engineering concern that NIST AI 600-1 does not address. The framework has no controls for tracing overhead management, instrumentation parity requirements, or the detection of test-production divergence introduced by differential telemetry coverage across agent deployments. MEASURE 2.7 pre-deployment testing environments may have different instrumentation levels than production, creating the very divergence this threat describes, but NIST provides no guidance on instrumentation consistency requirements. Tracing overhead-induced instrumentation bias is an infrastructure engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_15
The gap between observable external metrics and non-observable internal agent states — including reasoning validity, confidence scores, and memory corruption — is a fundamental observability limitation that NIST AI 600-1 does not address. The Human-AI Configuration risk category acknowledges the importance of meaningful human oversight and interpretability, but the framework does not mandate technical controls for internal state monitoring, confidence score telemetry, or memory integrity verification in deployed agent systems. MEASURE 2.7 pre-deployment evaluation could assess reasoning transparency in controlled conditions, but runtime internal state opacity and the propagation of internal corruption across multi-agent systems are operational monitoring limitations beyond the governance profile's reach. Internal agent state observability gaps are an inherent technical constraint that no governance-level framework can resolve through policy mandates.
**Score: 1**

---

### RTM_7_16
Log aggregation timestamp skew from unsynchronized agent clocks and the resulting causality reconstruction failures during incident investigation are distributed systems engineering concerns that NIST AI 600-1 does not address. The framework provides no controls for clock synchronization requirements, timestamp integrity validation, or the forensic reliability requirements that audit log infrastructure must meet to support accurate incident investigation. MANAGE 4.1–4.3 incident response and disclosure policies assume that investigation is possible and accurate, but the framework does not mandate the technical infrastructure — including time synchronization and tamper-evident timestamping — necessary to ensure audit log causality reliability. Timestamp skew enabling attacker-crafted false event timelines is an infrastructure engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_17
Prometheus metrics cardinality explosion from unbounded label dimensions in multi-agent deployments enabling denial-of-monitoring attacks is an observability infrastructure security concern that NIST AI 600-1 does not address. The framework has no controls for metrics cardinality management, Prometheus resource protection, or the attack surface created by agent-controlled metric label generation in shared monitoring infrastructure. MANAGE 4.1–4.3 incident response policies do not address monitoring infrastructure availability or resilience against monitoring-targeted denial-of-service attacks. Cardinality explosion as a mechanism for fleet-wide monitoring blind spots is an infrastructure security engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_18
Missing metrics for inter-agent coordination latency — where standard monitoring tracks per-pod metrics but not communication latency between coordinating agents — is an observability coverage gap that NIST AI 600-1 does not address. The framework provides no controls for multi-agent coordination telemetry requirements, inter-pod communication monitoring, or the detection of coordination degradation attacks that operate below standard per-pod alert thresholds. GOVERN 3.2 human oversight policies and MEASURE 2.7 pre-deployment testing do not address what runtime inter-agent coordination metrics are required or how coordination failures should be detected in production. Inter-agent coordination latency monitoring gaps are an infrastructure telemetry engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_19
Service mesh log capture of network calls without capturing semantic operation content — enabling evasion of semantic audit — is a logging architecture concern that NIST AI 600-1 does not address. The framework has no controls for service mesh instrumentation requirements, semantic operation logging, or the distinction between network-layer audit and application-semantic audit in multi-agent architectures. MANAGE 4.1–4.3 incident disclosure policies assume effective audit trails exist but do not specify what semantic content audit logs must capture or what logging architectures are required to support meaningful forensic investigation of agent operations. Tool invocation semantic audit gaps in service mesh environments are an infrastructure logging engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_20
Grafana dashboard design bias toward aggregate metrics hiding outlier agent behavior is an observability engineering concern that NIST AI 600-1 does not address. The framework provides no controls for dashboard design requirements, visualization standards for multi-agent systems, or the monitoring architecture practices needed to maintain visibility into individual agent anomalies when aggregate metrics remain within normal bounds. MANAGE 4.1–4.3 incident response policies assume that monitoring is effective but do not prescribe what monitoring visualizations or dashboards are required to detect targeted agent compromise beneath normal aggregate metrics. Dashboard-induced monitoring blind spots are an operational observability engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_21
Alert rule threshold manipulation by attackers controlling specific agents — suppressing alerts while maintaining cascading effects invisible in the monitored metrics — is an adversarial monitoring evasion concern that NIST AI 600-1 does not address. The framework has no controls for alert rule integrity, threshold manipulation detection, or the design of alert architectures resilient to adversarial metric manipulation by compromised agents. MANAGE 4.1–4.3 incident response policies assume alert systems function as designed but do not mandate tamper-evident alerting infrastructure or controls against attacker-driven metric steering that keeps values below alert thresholds. Alert suppression through metric threshold manipulation is an adversarial monitoring evasion problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_22
Asynchronous audit log write latency creating compliance blind spots when agents crash before buffer flushes — enabling attackers to suppress evidence by triggering crashes after malicious operations — is an audit log infrastructure security concern that NIST AI 600-1 does not address. The framework's incident response and disclosure policies under MANAGE 4.1–4.3 rely on the assumption that audit logs are available and complete, but do not mandate synchronous write requirements, write-ahead logging, or crash-resilient audit architectures that prevent evidence loss. No NIST AI 600-1 control addresses the technical design of audit log persistence infrastructure or the attack surface created by asynchronous write buffering. Audit log crash-induced evidence loss is an infrastructure engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_23
Container registry audit logs tracking image push events without performing semantic validation of layer content — leaving poisoned layers undetected — is a supply chain infrastructure concern that NIST AI 600-1 addresses only peripherally. GOVERN 6.1/6.2 supply chain and third-party vetting controls provide governance framing for evaluating container image sources and supplier trustworthiness, which has the weakest indirect relevance to whether container registries perform semantic layer validation. However, the framework does not mandate technical controls for container layer content inspection, registry audit log depth, or the detection of poisoned container layers within the registry infrastructure itself. Container registry semantic validation gaps are a supply chain infrastructure security engineering problem that GOVERN 6.1/6.2 governance framing cannot substitute for.
**Score: 1**

---

### RTM_7_24
Kubernetes API server audit log cardinality overload from high-frequency multi-agent API calls causing operators to disable detailed logging — creating blind spots that hide attacks in disabled audit trails — is an infrastructure operations security concern that NIST AI 600-1 does not address. The framework has no controls for audit log volume management, Kubernetes audit policy design, or the operational security implications of audit log cardinality-driven configuration decisions that reduce forensic visibility. MANAGE 4.1–4.3 incident disclosure policies assume audit logs are available and comprehensive but do not mandate the technical infrastructure or operational practices required to maintain audit log completeness under high-frequency multi-agent workloads. Audit log cardinality overload enabling attack concealment through logging policy degradation is an infrastructure operations problem entirely outside the governance-level profile's scope.
**Score: 1**
# NIST AI 600-1 RTM Analysis Cache — RTM_7 Items 25-48 (Infrastructure and Observability Stack Blind Spots)

---

### RTM_7_25
NIST AI 600-1 is a governance-level framework with no controls addressing GPU utilization telemetry, idle period detection, or the timing correlation of tool execution across distributed agent workloads. The framework does not address how profiling blind spots arise from multi-agent overlap patterns, nor does it provide guidance on instrumentation strategies that could detect attacks timed to GPU idle windows. MEASURE 2.7 pre-deployment security evaluation could in principle include profiling assessments, but the framework provides no methodology for runtime GPU idle period monitoring or cross-agent timing analysis. GPU utilization timing blind spots in multi-agent deployments are an infrastructure observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_26
NIST AI 600-1 contains no guidance on profiling overhead, the Heisenberg-like observability paradox created by instrumentation, or the behavioral divergence that emerges between profiled and unobserved production systems. The framework has no controls addressing asymmetric profiling configurations across multi-agent fleets or the methodological challenge of drawing security conclusions from behavior observed only under profiling conditions. MEASURE 2.7 acknowledges the need for pre-deployment security evaluation but does not address the systematic gap between tested and production behavior introduced by profiling overhead. Tracing overhead as an observability paradox is a distributed systems measurement engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_7_27
Metric aggregation strategies, fleet-wide versus per-agent granularity decisions, and the statistical masking of individual agent anomalies within aggregate performance statistics are operational monitoring design concerns that NIST AI 600-1 does not address. The framework has no controls requiring disaggregated per-agent telemetry, anomaly detection sensitivity tuning for fleet deployments, or protections against malicious agents exploiting aggregate averaging to hide abnormal behavior. MANAGE 4.1–4.3 establishes institutional incident response policies but does not mandate the technical monitoring granularity required to surface agent-specific anomalies within large fleets. Metric aggregation masking of agent-level behavior is an observability architecture problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_28
MLflow experiment tracking, distributed optimization logging, and the requirement that agent-to-agent communications be captured in monitored channels are operational MLOps engineering concerns that NIST AI 600-1 does not cover. The framework provides no controls for ensuring that optimization decisions communicated through agent coordination protocols are logged to experiment tracking systems, nor for detecting when distributed optimization occurs outside observable pipelines. GOVERN 6.1/6.2 addresses third-party tool vetting at acquisition but does not govern the runtime logging completeness of distributed optimization workflows. MLflow logging gaps in distributed multi-agent optimization are a MLOps observability engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_29
NIST AI 600-1 has no controls addressing speculative decoding architectures, draft token rejection logging, or the downstream influence of rejected speculative tokens on probability distributions and subsequent agent outputs. The framework does not recognize speculative decoding as an attack surface, and no NIST control governs the logging or auditability of intermediate inference computations that do not appear in final output logs. MEASURE 2.7 pre-deployment evaluation could assess inference pipeline integrity in principle, but the framework provides no methodology for auditing speculative decoding opacity or its propagation effects in multi-agent systems. Speculative decoding token prediction opacity is a deep inference architecture observability problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_30
Profiling data retention policies, storage constraint tradeoffs affecting forensic capability, and the forensic gap created by short profiling windows are operational infrastructure management concerns that NIST AI 600-1 does not address. The framework establishes no requirements for profiling data retention duration, minimum forensic evidence windows, or the engineering of tiered storage strategies to preserve profiling data for post-incident analysis. MANAGE 4.1–4.3 establishes incident response and disclosure policies at the institutional level but does not mandate the technical data retention infrastructure required to support forensic investigation of attacks between profiling windows. Profiling data retention forensic gaps are an infrastructure storage engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_7_31
Dynamic batching infrastructure, the indeterministic latency variance it introduces, and the resulting masking of marginal malicious latency increases are operational inference serving engineering concerns that NIST AI 600-1 does not address. The framework has no controls for batching-aware anomaly detection, latency baseline calibration that accounts for batching variance, or monitoring strategies that can isolate request-level latency anomalies within dynamically batched workloads. MEASURE 2.7 pre-deployment evaluation does not address runtime batching behavior or the statistical challenge of detecting small malicious latency contributions masked within legitimate batching indeterminism. Inference latency spike masking by batching indeterminism is an inference serving observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_32
Queue depth monitoring at the inference layer versus orchestration-layer queuing between agents represents a distributed systems observability gap that NIST AI 600-1 does not address. The framework contains no controls for multi-layer queue monitoring, orchestration deadlock detection, or the instrumentation of agent-to-agent handoff points that could reveal deadlock conditions appearing as normal inference queue depth in standard telemetry. MANAGE 4.1–4.3 establishes institutional incident response policies but provides no technical guidance for distinguishing orchestration-level deadlocks from inference queue saturation. Queue depth monitoring blind spots in multi-agent orchestration are a distributed systems observability engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_33
Error rate aggregation granularity, alert threshold calibration for fleet-wide versus per-agent error rates, and the dilution of targeted attack signals within aggregate statistics are operational monitoring engineering concerns that NIST AI 600-1 does not address. The framework has no controls requiring per-agent error rate monitoring, statistical methods for detecting localized anomalies within aggregate fleet metrics, or alert architectures that preserve individual agent signal fidelity. MEASURE 2.7 pre-deployment evaluation does not address how monitoring systems should be configured to detect targeted attacks on specific agents within a larger fleet. Error rate aggregation masking of multi-agent failure patterns is a monitoring architecture engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_34
Token-per-second granularity in throughput monitoring, the distinction between request volume and actual computational load, and the exploitation of coarse throughput metrics to hide expensive operations are operational inference telemetry engineering concerns that NIST AI 600-1 does not address. The framework has no controls specifying what inference metrics must be collected, what granularity is required for security-relevant anomaly detection, or how throughput metric blind spots can be exploited by adversaries generating computationally expensive requests. MEASURE 2.7 could include throughput metric coverage in pre-deployment evaluation but provides no methodology for assessing inference telemetry completeness. Token generation metric absence as a throughput analysis blind spot is an inference observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_35
Cross-service correlation dashboards, the engineering of monitoring systems that can detect attack propagation patterns spanning multiple agents, and the gap created by per-service metrics without cross-service correlation are operational observability architecture concerns that NIST AI 600-1 does not address. The framework provides no controls for cross-agent telemetry correlation, causal chain reconstruction across agent boundaries, or the detection of attack propagation visible only through multi-agent metric correlation. GOVERN 6.1/6.2 addresses organizational dependencies and third-party vetting but does not govern how monitoring systems must be architected to reveal cross-agent attack patterns. Cross-agent correlation absence from standard monitoring is a distributed observability engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_7_36
GPU telemetry metrics, their fundamental inability to reveal quantization corruption, precision loss propagation, or KV cache poisoning occurring silently within GPU computations, and the compounding blind spots created by hundreds of edge deployment locations are hardware infrastructure observability concerns that NIST AI 600-1 does not address. The framework has no controls for GPU-level computation integrity verification, quantization attack detection through hardware telemetry, or the engineering of edge fleet monitoring that can detect silent compute-layer attacks. MEASURE 2.7 pre-deployment security evaluation does not address GPU computation integrity or the monitoring requirements for edge-deployed multi-agent fleets. GPU telemetry insufficiency for detecting quantization-based attacks is a hardware-layer observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_37
TensorRT engine binary opacity, the absence of runtime visibility into engine metadata, and the impracticality of continuous binary hash validation across hundreds of cached engines in multi-agent deployments are inference infrastructure integrity concerns that NIST AI 600-1 does not address. The framework has no controls for binary artifact integrity monitoring, runtime engine validation requirements, or the operational procedures required to maintain continuous hash verification across large fleets of opaque compiled inference artifacts. GOVERN 6.1/6.2 supply chain vetting addresses whether model and engine sources are trustworthy at acquisition time but does not govern post-deployment engine binary integrity monitoring. TensorRT engine metadata drift detectability is an inference artifact integrity engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_38
Quantization-specific telemetry such as activation range distribution, per-layer precision loss metrics, and the monitoring infrastructure required to detect quantization-based attacks are inference engineering observability concerns that NIST AI 600-1 does not address. The framework has no controls specifying what quantization monitoring telemetry must be collected, what thresholds indicate precision loss anomalies, or how quantization artifact monitoring should be architected across multi-agent deployments. MEASURE 2.7 pre-deployment evaluation could in principle assess quantization integrity but provides no methodology for runtime quantization telemetry or cross-agent precision loss monitoring. Quantization artifact telemetry absence is a specialized inference observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_39
Cross-agent timing pattern detection, the monitoring infrastructure required to correlate latency conditions in one agent with triggered behavior in downstream agents, and per-agent latency monitoring's fundamental inability to reveal such coordination are distributed systems observability concerns that NIST AI 600-1 does not address. The framework has no controls for cross-agent timing correlation, latency-conditioned behavioral trigger detection, or monitoring architectures that can reconstruct causal chains spanning multiple agent interaction points. MANAGE 4.1–4.3 establishes incident response policies but does not mandate the technical monitoring depth required to detect latency-triggered cross-agent attack coordination. Cross-agent coordination timing blind spots are a distributed observability engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_7_40
Post-deployment health check design, the fundamental limitation of functionality-based validation for detecting latent backdoors in quantized engines with non-activated trigger conditions, and model update validation telemetry gaps are inference integrity assurance engineering concerns that NIST AI 600-1 does not address. The framework has no controls specifying what post-deployment validation must include to detect backdoored quantized models, what trigger activation coverage is required during health checks, or how validation telemetry should be designed to surface latent backdoor conditions. MEASURE 2.7 focuses on pre-deployment security evaluation and does not address the ongoing runtime validation challenge posed by backdoors that pass standard health checks. Model update validation telemetry gaps are a post-deployment model integrity engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_41
Load balancer routing metrics, the aggregate traffic distribution visibility they provide, and their fundamental obscuring of individual agent-level behavior that enables replica-specific attacks to hide within fleet-level statistics are network infrastructure observability concerns that NIST AI 600-1 does not address. The framework has no controls for replica-level monitoring behind load balancers, the engineering of observability that preserves per-instance visibility within load-balanced deployments, or detection strategies for attacks confined to specific replicas invisible in aggregate routing metrics. GOVERN 6.1/6.2 addresses infrastructure component vetting but does not govern how monitoring systems must be designed to maintain per-replica observability behind load balancers. Load balancer routing metric obscuring of agent-level behavior is a network infrastructure observability engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_42
Batch processing architectures, the loss of individual tool invocation pattern visibility when invocations are aggregated within batches, and the resulting inability to detect dangerous tool sequences hidden within batch processing are inference serving observability concerns that NIST AI 600-1 does not address. The framework has no controls for batch-level tool invocation logging, individual invocation sequence reconstruction within batched workloads, or monitoring strategies that preserve tool call pattern visibility despite batch aggregation. MEASURE 2.7 pre-deployment evaluation does not address how batching architectures affect tool invocation monitoring completeness or how dangerous sequences can evade batch-level logging. Batching obscuring tool invocation patterns is an inference serving observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_43
Caching layer instrumentation, the tool invocation observability gap created when cached responses bypass underlying tool execution, and the monitoring blind spot where cached queries appear as zero tool usage are infrastructure caching observability concerns that NIST AI 600-1 does not address. The framework has no controls for cache-aware tool monitoring, hit/miss telemetry requirements, or observability architectures that maintain accurate tool invocation accounting regardless of whether responses are served from cache or live execution. GOVERN 6.1/6.2 addresses caching infrastructure at acquisition time through supply chain vetting but does not govern how caching layers must be instrumented to preserve tool invocation observability. Caching creating hit/miss monitoring blind spots is an infrastructure observability engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_7_44
Load balancer session affinity configurations, the confinement of sessions to single replicas through IP hash routing, and the resulting invisibility of cross-replica attack propagation in per-replica logs are network infrastructure security observability concerns that NIST AI 600-1 does not address. The framework has no controls for cross-replica communication monitoring, the detection of malware propagating through load balancer routing mechanisms, or observability requirements that preserve cross-replica visibility despite session affinity constraints. MANAGE 4.1–4.3 establishes incident response policies at the institutional level but provides no technical guidance for detecting cross-replica attack propagation hidden by load balancer session affinity. Session affinity hiding cross-replica attack propagation is a network infrastructure observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_45
Auto-scaling infrastructure, the monitoring configuration consistency challenges that arise as new replicas join fleets with potentially different instrumentation, and the coverage gaps created by inconsistent monitoring across dynamically scaled deployments are cloud infrastructure engineering concerns that NIST AI 600-1 does not address. The framework has no controls for monitoring configuration management in auto-scaling environments, requirements for ensuring new replicas inherit correct instrumentation at provisioning time, or detection strategies for identifying replicas that joined the fleet with monitoring gaps. GOVERN 6.1/6.2 vets infrastructure components at acquisition but does not govern runtime monitoring configuration consistency in dynamically scaled multi-agent deployments. Auto-scaling configuration blind spots are a cloud infrastructure observability engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_7_46
Dynamic load balancer metric integrity, the consequences of metric poisoning for routing decisions and downstream observability reliability, and the challenge of detecting when attackers have manipulated the metrics that observability systems rely upon are network infrastructure security concerns that NIST AI 600-1 does not address. The framework has no controls for load balancer metric integrity verification, the detection of poisoned routing metrics, or the design of observability systems that are resilient to metric manipulation at the routing layer. MEASURE 2.7 pre-deployment evaluation does not address the runtime integrity of dynamic routing metrics or the cascading observability corruption that results from metric poisoning. Dynamic load balancer metric poisoning affecting observability is a network infrastructure integrity engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_7_47
Streaming response monitoring in batched inference contexts, the misalignment between batch boundaries and response boundaries, and the resulting inability to achieve clear per-request attribution are inference serving observability engineering concerns that NIST AI 600-1 does not address. The framework has no controls for streaming response telemetry requirements, per-request attribution methodologies in batched streaming deployments, or monitoring architectures that can reconstruct individual request provenance when batch and response boundaries do not align. MEASURE 2.7 does not address the technical complexity of attributing streamed response content to individual requests within batched inference pipelines. Streaming response monitoring complexity is an inference serving observability engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_7_48
Cost telemetry attribution in distributed batch processing, the opacity of per-agent cost contribution when operations are distributed across batch members, and the resulting ability of agents to hide expensive operations within shared batch cost are cloud economics and operational monitoring concerns that NIST AI 600-1 does not address. The framework has no controls for cost attribution granularity, per-agent operational cost monitoring requirements, or the detection of agents exploiting batch cost pooling to conceal computationally expensive malicious operations. MANAGE 4.1–4.3 establishes institutional-level policies but does not mandate the technical cost telemetry infrastructure required to achieve per-agent attribution within distributed batch processing. Cost telemetry attribution opacity in distributed batching is an infrastructure economics observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**
# NIST AI 600-1 RTM Analysis Cache — RTM_8 (Reasoning and Cognitive State Monitoring)

---

### RTM_8_1
NIST AI 600-1 contains no controls addressing chain-of-thought trace monitoring, syntactic or semantic analysis of reasoning steps, or anomaly detection applied to reasoning content. The framework's MEASURE 2.9 addresses model explanation and validation but does so exclusively in pre-deployment evaluation contexts, not runtime trace analysis. No NIST control provides methodology for detecting semantic-preserving syntactic mutations in CoT traces or for deploying pattern-based versus semantic detection across multi-agent systems. Reasoning mutation attacks that defeat anomaly detection are a runtime reasoning security engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_2
NIST AI 600-1 provides no controls for cross-agent reasoning trace correlation, distributed intent detection, or monitoring architectures capable of aggregating reasoning traces across multiple agents to reveal coordinated attack patterns. The framework addresses organizational governance of AI systems but does not recognize multi-agent reasoning coordination as a monitoring domain requiring technical controls. MEASURE 2.7 pre-deployment security evaluation and MEASURE 2.9 model validation are scoped to individual model assessment, not cross-agent reasoning analysis in deployment. Coordination intent invisibility arising from per-agent trace isolation is a distributed reasoning observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_3
The challenge of comparing reasoning structures across agents to detect semantic equivalence, obfuscated decision pathways, or compromised conclusions expressed through varied reasoning forms is a runtime cognitive observability problem that NIST AI 600-1 does not address. The framework has no controls for reasoning consistency verification across agents, semantic matching of decision rationales, or the detection of structurally varied reasoning that reaches identical compromised conclusions. MEASURE 2.9 model explanation controls are pre-deployment in nature and do not provide methodology for runtime cross-agent reasoning structure comparison. Reasoning state obfuscation exploiting semantic matching limitations is an operational reasoning monitoring engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_8_4
Meta-reasoning structures in which agents reason about their own reasoning processes, and the monitoring blind spots that arise when recursive reasoning layers explicitly conceal decision factors from external observation, are cognitive architecture security concerns that NIST AI 600-1 does not address. The framework has no controls requiring transparency in nested or recursive reasoning structures, detection of reasoning layers designed to evade monitoring, or audit requirements for self-referential deliberation in multi-agent systems. MEASURE 2.9 model explanation guidance addresses interpretability in the pre-deployment context but provides no methodology for detecting monitoring-evasive meta-reasoning in deployed agents. Meta-reasoning blind spots creating recursive monitoring failures are a cognitive architecture security engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_5
Timing sidechannels embedded in reasoning trace generation and retrieval, the state information they encode, and the exploitation of latency patterns across coordinated multi-agent reasoning to infer attack progress are inference timing security concerns that NIST AI 600-1 does not address. The framework contains no controls for reasoning latency monitoring, timing covert channel detection in multi-agent coordination, or the instrumentation required to distinguish legitimate reasoning latency variation from attacker-exploited timing signals. MANAGE 4.1–4.3 establishes incident response policies at the institutional level but does not provide technical guidance for detecting latency sidechannel exploitation in agent reasoning pipelines. Reasoning latency sidechannel attacks in multi-agent coordination are a timing security engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_8_6
Tree-of-thought search pattern telemetry, the reverse-engineering risk it creates when logged patterns reveal agent decision logic and predictable future plans, and the aggregated ToT telemetry generated across multi-agent systems are reasoning architecture exposure concerns that NIST AI 600-1 does not address. The framework has no controls governing the logging of ToT search patterns, the security implications of reasoning telemetry as an attack surface, or the design of ToT monitoring that preserves oversight without exposing exploitable decision logic. MEASURE 2.9 model explanation guidance does not address the dual-use nature of ToT trace logging or the adversarial exploitation of aggregated reasoning telemetry in multi-agent deployments. ToT search pattern analysis enabling behavior prediction is a reasoning telemetry security engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_7
The monitoring gap between preserved reasoning paths and real-time reasoning, and the telemetry linkage failure that occurs when downstream agents execute preserved paths without attribution to the original generation context, are reasoning provenance observability concerns that NIST AI 600-1 does not address. The framework has no controls for reasoning path provenance tracking, telemetry continuity requirements across reasoning preservation and retrieval boundaries, or audit logging that links preserved path execution back to original generation. GOVERN 6.1/6.2 supply chain vetting addresses component acquisition but does not govern reasoning path provenance or telemetry continuity across agent execution boundaries. Preserved path instrumentation blind spots are a reasoning provenance engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_8_8
Aggregated telemetry architectures that report only summary statistics across multiple reasoning chains, the statistical masking of path-specific anomalies within aggregate metrics, and the compounding blind spots created when multiple agents' reasoning paths mix in aggregated reporting are monitoring architecture concerns that NIST AI 600-1 does not address. The framework has no controls specifying telemetry granularity requirements for multi-path reasoning, methods for preserving path-level anomaly signal within aggregate reporting, or detection strategies for injected instructions in individual reasoning chains that disappear within cross-agent aggregation. MEASURE 2.7 pre-deployment evaluation does not address how aggregated telemetry architectures must be designed to preserve path-level security signal. Multi-path reasoning telemetry aggregation blind spots are a reasoning monitoring architecture engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_9
Quality score monitoring methodology, the undetectability of malicious quality score inflation when safety monitoring focuses on output content rather than scoring processes, and the cross-agent blind spots created by heterogeneous quality metrics in multi-agent systems are operational monitoring design concerns that NIST AI 600-1 does not address. The framework has no controls for quality scoring transparency, methodology auditing requirements for agent evaluation metrics, or detection strategies for metric manipulation that disappears when individual agent scores aggregate across heterogeneous multi-agent quality frameworks. MEASURE 2.9 model validation addresses pre-deployment quality assessment but does not govern runtime quality scoring integrity or cross-agent metric aggregation blind spots. Quality score metric blind spots in safety monitoring are a scoring methodology security engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_8_10
Sampling parameter tracking, the monitoring blind spots created when execution telemetry does not record which parameter configurations were active, and the multi-agent deployment blind spots arising from heterogeneous parameter configurations across agents are inference configuration observability concerns that NIST AI 600-1 does not address. The framework has no controls requiring per-execution parameter logging, monitoring coverage for parameter-conditional attack surfaces, or detection strategies for vulnerabilities that manifest only under specific sampling configurations invisible in aggregate multi-agent telemetry. MEASURE 2.7 pre-deployment security evaluation does not address runtime parameter configuration monitoring or the heterogeneous parameter blind spots created in multi-agent deployments. Sampling parameter monitoring blind spots are an inference configuration observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_11
Confidence score reporting that omits voting distribution detail, and the resulting loss of consensus strength information that enables attackers to game voting mechanisms to produce high-confidence outputs from weak or manipulated consensus, are multi-agent decision aggregation security concerns that NIST AI 600-1 does not address. The framework has no controls for voting mechanism transparency, confidence score provenance requirements, or monitoring specifications that mandate distribution-level detail over aggregated confidence values. MEASURE 2.9 model validation does not address runtime voting mechanism integrity or the monitoring requirements for confidence score provenance in multi-agent consensus systems. Confidence score monitoring blind spots from voting mechanism opacity are a decision aggregation security engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_8_12
Memory consolidation monitoring that tracks only retrieval outcomes without visibility into which consolidated points were accessed, and the resulting undetectability of consolidated memory backdoor exploitation, are cognitive memory security concerns that NIST AI 600-1 does not address. The framework has no controls for consolidated memory access logging, granular retrieval audit requirements that preserve information about which memory artifacts were accessed, or detection strategies for backdoor exploitation hidden within normal-appearing retrieval outcomes. MANAGE 4.1–4.3 establishes institutional incident response policies but does not mandate the technical memory access telemetry required to detect consolidated memory backdoor activation. Consolidated memory usage monitoring blind spots are a cognitive memory security engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_13
Streaming response monitoring that captures only final outputs without streaming-level detail, the resulting invisibility of instruction injection through streaming handoffs between agents, and the boundary blind spots created at multi-agent streaming handoff points are inference serving and agent coordination security concerns that NIST AI 600-1 does not address. The framework has no controls for streaming handoff telemetry requirements, monitoring continuity across agent boundaries in streaming architectures, or detection strategies for mid-stream instruction injection that does not appear in final output logs. MEASURE 2.7 pre-deployment evaluation does not address the runtime monitoring requirements for streaming handoff integrity in multi-agent pipelines. Streaming response monitoring blind spots in multi-agent handoffs are an agent coordination observability engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_8_14
Decomposition trace logging within individual agents without central aggregation, the monitoring blind spots created when hierarchical agent decompositions are not visible to central oversight, and the early detection failures that result from decomposition errors executing without systemic visibility are multi-agent hierarchy observability concerns that NIST AI 600-1 does not address. The framework has no controls requiring centralized decomposition trace logging, monitoring architectures that provide hierarchical visibility into agent task decomposition, or early warning systems for invalid decompositions propagating through agent hierarchies. GOVERN 6.1/6.2 addresses organizational governance of AI systems but does not govern the internal logging architecture required for decomposition trace visibility across agent hierarchies. Decomposition trace logging gaps in multi-agent hierarchies are a hierarchical agent monitoring engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_15
Partial order execution monitoring that lacks visibility into partial order semantics, the resulting appearance of deterministic execution when underlying decompositions permit non-determinism, and the attacker exploitation of non-deterministic execution paths invisible to determinism-assuming monitors are task execution modeling concerns that NIST AI 600-1 does not address. The framework has no controls for partial order semantics logging, non-determinism-aware monitoring requirements, or detection strategies for attacks exploiting execution order flexibility invisible within deterministic-assumption monitoring architectures. MEASURE 2.9 does not address runtime execution ordering semantics or the monitoring implications of partial order task decomposition. Partial order execution monitoring without complete ordering information is a task execution observability engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_8_16
Abstraction level mismatches in multi-agent monitoring hierarchies, the blind spots created when each agent monitors only at its own abstraction level, and the problem detection failures arising from gaps between strategic abstract state and concrete operational state are cognitive architecture observability concerns that NIST AI 600-1 does not address. The framework has no controls for cross-abstraction-level monitoring consistency, requirements for mapping between strategic and operational state representations, or detection strategies for attacks that exploit abstraction gaps between monitoring layers in hierarchical agent deployments. MEASURE 2.7 pre-deployment evaluation does not address the runtime monitoring architecture requirements for maintaining observability across abstraction level transitions in multi-agent hierarchies. State abstraction projection loss in monitoring context is a hierarchical cognitive architecture observability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_17
Method selection monitoring that logs which methods execute without flagging suboptimal selections that do not immediately fail, and the resulting inability to detect gradual capability degradation from systematically poor method choices, are planning quality observability concerns that NIST AI 600-1 does not address. The framework has no controls for method selection quality monitoring, baseline expectations for method appropriateness, or detection strategies for gradual degradation from suboptimal selections that remain functional but cumulatively undermine system performance. MEASURE 2.7 pre-deployment evaluation assesses model performance but does not address runtime method selection quality monitoring or the detection of non-failing but systematically poor planning decisions. Method selection monitoring lacking failure prediction is a planning quality observability engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_8_18
Precondition validation gaps in multi-agent hierarchies where one agent checks preconditions and another executes methods without independent precondition visibility, and the resulting undetectability of execution proceeding on falsely satisfied preconditions, are multi-agent task integrity concerns that NIST AI 600-1 does not address. The framework has no controls for cross-agent precondition verification, independent execution-side precondition visibility requirements, or monitoring architectures that ensure executing agents have reliable knowledge of whether preconditions were genuinely satisfied. GOVERN 6.1/6.2 addresses organizational AI governance but does not govern the distributed precondition verification requirements of multi-agent task execution architectures. Precondition failure modes not monitored in cross-agent execution are a multi-agent task integrity engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_19
Decomposition correctness validation gaps where no agent in a multi-agent hierarchy bears complete responsibility for verifying whether selected methods actually achieve abstract goals, and the resulting correctness failures that evade monitoring detection, are hierarchical planning integrity concerns that NIST AI 600-1 does not address. The framework has no controls for end-to-end decomposition correctness responsibility assignment, cross-agent goal achievement verification requirements, or monitoring strategies that ensure abstract goal satisfaction is validated rather than assumed. MANAGE 4.1–4.3 establishes institutional-level response policies but does not mandate the technical monitoring responsibility allocation required for decomposition correctness validation across agent hierarchies. Decomposition correctness validation lacking fallback checks is a hierarchical planning integrity engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_8_20
Cross-agent constraint respect verification, the monitoring gap when one agent specifies constraints and another executes tasks without verified constraint compliance, and the absence of monitoring confirmation that executing agents honor specifying agents' constraints are multi-agent coordination integrity concerns that NIST AI 600-1 does not address. The framework has no controls for cross-agent constraint propagation verification, execution-side constraint compliance monitoring requirements, or detection strategies for executing agents that ignore or violate constraints specified by other agents in the hierarchy. MEASURE 2.7 pre-deployment evaluation does not address runtime constraint compliance monitoring in multi-agent systems where specification and execution are separated across agent boundaries. Cross-agent constraint conflict detection blind spots are a multi-agent coordination integrity engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_21
MCTS tree statistics representing agent cognitive state during planning, the rarity with which these internal statistics are logged or monitored, and the detailed planning intention and exploration pattern information they expose to attackers with access to internal statistics are cognitive state security concerns that NIST AI 600-1 does not address. The framework has no controls for MCTS internal state logging requirements, cognitive state observability standards for planning algorithms, or security requirements governing access to internal planning statistics that expose detailed agent intention. MEASURE 2.9 model explanation guidance addresses pre-deployment interpretability but does not extend to runtime cognitive state logging requirements for MCTS-based planning systems. MCTS tree statistics as hidden cognitive state is a planning algorithm observability engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_8_22
MCTS simulation traces containing extensive exploration of dangerous actions remaining internal and invisible to monitoring systems that see only the executed action, and the behavioral blind spot this creates for safety oversight, are simulation transparency security concerns that NIST AI 600-1 does not address. The framework has no controls for simulation trace logging requirements, monitoring visibility into planning exploration paths that do not result in executed actions, or safety oversight requirements that account for the full space of actions an agent considered before selecting its output. MEASURE 2.7 pre-deployment evaluation does not address the monitoring gap between internally simulated and externally executed actions in MCTS-based agent planning. Simulation trace visibility as a behavioral blind spot is a planning transparency engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_23
Rollout policy evolution in adaptive MCTS systems occurring without explicit monitoring, the resulting undetectability of policy drift, and the multi-agent amplification effect where one compromised rollout policy propagates drift to agents sharing that policy are planning algorithm security concerns that NIST AI 600-1 does not address. The framework has no controls for rollout policy version tracking, adaptive policy drift monitoring requirements, or detection strategies for behavioral changes arising from unmonitored rollout policy evolution in single or multi-agent MCTS deployments. MANAGE 4.1–4.3 establishes incident response policies but does not provide technical guidance for detecting unmonitored adaptive planning policy changes. Rollout policy drift as unobserved behavior change is a planning algorithm integrity engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_8_24
MCTS planning quality silently degrading without alerts when convergence fails due to insufficient computational budget or value function errors, and the resulting unmonitored planning breakdowns that produce poor decisions without triggering observable failures, are planning algorithm reliability concerns that NIST AI 600-1 does not address. The framework has no controls for MCTS convergence monitoring, planning quality health indicators, or alert requirements for planning systems that silently degrade when computational constraints prevent adequate search tree development. MEASURE 2.7 pre-deployment security evaluation does not address runtime planning algorithm convergence monitoring or silent degradation detection for MCTS-based decision systems. Convergence failures as unmonitored planning breakdowns are a planning algorithm reliability engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_25
LangGraph checkpointing logging that records only checkpoint creation and restoration without examining MCTS tree contents, and the resulting unmonitored attack surface created by poisoned tree states persisting across checkpoint boundaries, are framework integration observability concerns that NIST AI 600-1 does not address. The framework has no controls for AI framework-specific monitoring integration requirements, checkpoint content integrity validation, or the telemetry gaps introduced when orchestration frameworks log operational metadata without exposing the cognitive state they preserve. GOVERN 6.1/6.2 supply chain vetting addresses framework component acquisition but does not govern the runtime telemetry completeness of specific AI orchestration frameworks. Framework-induced telemetry gaps in LangGraph MCTS integration are a framework observability engineering problem entirely outside the governance-level profile's scope.
**Score: 1**

---

### RTM_8_26
Slow, subtle heuristic degradation injected by attackers to be indistinguishable from natural performance variation, causing replanning to gradually favor attacker-preferred paths in ways that evade anomaly detection calibrated to larger performance deviations, are planning security engineering concerns that NIST AI 600-1 does not address. The framework has no controls for replanning heuristic integrity monitoring, gradual degradation detection sensitivity requirements, or the statistical methodology needed to distinguish attacker-induced heuristic drift from legitimate performance variation across replanning cycles. MEASURE 2.7 pre-deployment evaluation does not address the runtime challenge of detecting slow heuristic degradation that remains within normal variation bounds while systematically steering planning toward attacker objectives. Replanning attack obfuscation via normal variation is a planning integrity engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**

---

### RTM_8_27
Contingency branch condition corruption enabling attackers to trigger contingencies frequently while appearing legitimate, and the monitoring blind spot created when systems focus on unexpected triggers rather than authenticating normal-appearing activations, are planning contingency security concerns that NIST AI 600-1 does not address. The framework has no controls for contingency activation authenticity verification, monitoring requirements that distinguish legitimate from attacker-triggered normal-appearing contingency activations, or detection strategies for corrupted branch conditions that produce frequent contingency activation without triggering unexpectedness-based alerts. MANAGE 4.1–4.3 establishes institutional incident response policies but does not provide technical guidance for contingency activation integrity monitoring in planning systems. Contingency activation blind spots are a planning contingency security engineering problem entirely outside the governance profile's scope.
**Score: 1**

---

### RTM_8_28
Search tree expansion rate monitoring that may miss incremental heuristic degradation increasing search tree size gradually enough to be attributed to harder planning problems, and the resulting inability to distinguish attacker-induced expansion rate increases from legitimate complexity growth, are planning performance security concerns that NIST AI 600-1 does not address. The framework has no controls for search tree expansion rate monitoring, heuristic quality trend analysis, or the statistical methodology required to attribute expansion rate changes to attacker-induced degradation versus legitimate problem complexity increases across replanning cycles. MEASURE 2.7 pre-deployment evaluation does not address runtime planning algorithm performance monitoring or the detection of adversarial tree expansion rate manipulation camouflaged as natural complexity variation. Search tree expansion rate blinding is a planning algorithm performance security engineering problem entirely outside NIST AI 600-1's governance scope.
**Score: 1**
# RTM_9 Analysis: Memory Systems and Knowledge Base Observability
# NIST AI 600-1 Generative AI Profile Coverage Assessment

### RTM_9_1
NIST AI 600-1 does not address episodic memory retrieval systems or their monitoring requirements. The framework's §2.9 Information Security addresses data poisoning at a governance level but provides no mechanisms for logging or attributing behavioral influences from retrieved memory episodes. MEASURE 2.7 covers pre-deployment security evaluation and would not surface runtime attribution blind spots in multi-agent episodic retrieval chains. No NIST control mandates audit logging of memory retrieval events or correlates retrieved episodes to subsequent agent actions.
**Score: 1**

### RTM_9_2
NIST AI 600-1 offers no governance provisions for memory consolidation processes or the offline transformation of episodic memory into semantic rules. §2.9 acknowledges data poisoning as a threat category but does not extend to monitoring or auditing consolidation pipelines that operate outside the primary inference path. MEASURE 2.9 addresses model validation but not the behavioral artifacts of consolidation processes that may encode malicious templates. Organization-scale consolidation to shared semantic memory represents an attack surface entirely outside NIST's observability scope.
**Score: 1**

### RTM_9_3
NIST AI 600-1 does not address trajectory storage, trajectory-based learning, or the monitoring of intermediate steps within agent action sequences. MEASURE 2.7 focuses on pre-deployment evaluation and cannot detect malicious intermediate steps embedded within shared trajectory references used at runtime. The framework's incident response provisions in MANAGE 4.1-4.3 operate at institutional disclosure levels, not the granular step-level attribution required to detect multi-step attacks hidden in shared trajectory materials. No control addresses monitoring of trajectory integration in multi-agent environments.
**Score: 1**

### RTM_9_4
NIST AI 600-1 does not distinguish between vector databases, graph databases, or hybrid storage architectures, and provides no guidance on monitoring coverage parity across storage channels. GOVERN 6.1/6.2 addresses third-party vetting of supply chain components but does not mandate monitoring symmetry across the storage systems those components comprise. §2.9 acknowledges information security risks broadly but offers no framework for identifying or remediating monitoring asymmetries that attackers could exploit by targeting under-monitored storage channels. Hybrid architecture observability gaps are entirely outside the framework's scope.
**Score: 1**

### RTM_9_5
NIST AI 600-1 provides no accountability framework for cross-agent memory sharing or the attribution of behavioral influence across agent boundaries. MANAGE 4.1-4.3 addresses incident response at an organizational level but does not resolve the technical challenge of attributing responsibility when one agent retrieves episodes and another acts upon them. The framework's governance provisions in GOVERN 6.1/6.2 address organizational accountability structures but not the runtime accountability diffusion inherent in shared episodic memory architectures. No NIST control mandates provenance tracking for memory artifacts shared across agents.
**Score: 1**

### RTM_9_6
NIST AI 600-1 does not address metadata filtering complexity in retrieval systems or the monitoring challenges it creates. §2.9 acknowledges data poisoning risks but provides no guidance on detecting poisoned episodes designed to activate only under specific metadata filter combinations invisible to standard monitoring. MEASURE 2.7 pre-deployment evaluation would not surface runtime evasion strategies that depend on specific metadata query configurations. No NIST control addresses the logging or analysis of retrieval metadata filter combinations as a security observable.
**Score: 1**

### RTM_9_7
NIST AI 600-1 does not address semantic memory query logging, performance-constrained audit gaps, or the security implications of incomplete query audit trails. MEASURE 2.7 covers security evaluation before deployment but cannot mandate comprehensive query logging without impacting the performance characteristics evaluated. §2.9 information security provisions do not extend to operational logging requirements for vector database or semantic memory query systems. The framework provides no basis for requiring complete query audit trails in multi-agent environments where monitoring evasion via selective query routing is possible.
**Score: 1**

### RTM_9_8
NIST AI 600-1 does not address retrieval relevance monitoring or the distinction between retrieval relevance and semantic appropriateness for a given context. §2.2 Confabulation provisions acknowledge that AI systems may produce contextually inappropriate outputs, but provide no governance mechanism for detecting when retrieved documents are contextually valid yet contain injected inappropriate instructions. MEASURE 2.9 model explanation provisions do not extend to validating the semantic appropriateness of retrieved knowledge base content against query intent. No NIST control mandates monitoring beyond relevance scoring for retrieved content.
**Score: 1**

### RTM_9_9
NIST AI 600-1 provides no guidance on knowledge graph monitoring, inference rule management, or cardinality explosion detection in graph-based storage systems. MEASURE 2.7 pre-deployment security evaluation does not cover runtime graph relationship cardinality monitoring that would be needed to detect exponential relationship growth before system-wide degradation occurs. The framework's incident response provisions in MANAGE 4.1-4.3 address organizational response after observable failures but not proactive detection of cardinality explosion that remains invisible in aggregate metrics. No NIST control addresses knowledge graph health monitoring as a security requirement.
**Score: 1**

### RTM_9_10
NIST AI 600-1 does not address temporal validity enforcement in knowledge bases or the monitoring gap between document age metrics and actual agent compliance with validity annotations. §2.2 Confabulation acknowledges that AI systems may rely on outdated information but provides no operational framework for enforcing or monitoring validity annotation compliance at runtime. MEASURE 2.9 validation provisions address model-level accuracy but not the knowledge base lifecycle management required to ensure stale documents do not remain retrievable despite monitoring indicators suggesting expiration. No NIST control mandates monitoring of agent validity annotation compliance.
**Score: 1**

### RTM_9_11
NIST AI 600-1 does not address embedding quality monitoring, vector database integrity, or the detection of gradual corruption in embedding spaces. §2.9 data poisoning provisions acknowledge the threat category but provide no operational guidance for detecting slow-burn embedding degradation that produces no acute monitoring signal until retrieval accuracy visibly collapses. MEASURE 2.7 pre-deployment evaluation cannot establish runtime baselines for embedding quality that would enable drift detection over time. No NIST control mandates continuous embedding health monitoring or anomaly detection for vector database integrity.
**Score: 1**

### RTM_9_12
NIST AI 600-1 does not address anomaly detection for knowledge base poisoning or the baseline establishment challenges in multi-agent environments with diverse query patterns. §2.9 acknowledges data poisoning as a threat but provides no framework for distinguishing poisoning anomalies from legitimate query variation in complex multi-agent deployments. GOVERN 6.1/6.2 supply chain vetting applies to initial component acquisition, not the ongoing monitoring needed to detect poisoning that occurs after deployment through diverse query-driven content ingestion. No NIST control provides methodology for knowledge base anomaly baseline establishment.
**Score: 1**

### RTM_9_13
NIST AI 600-1 does not address caching architectures, cache hit rate monitoring, or the manipulation of performance health metrics through cache poisoning. §2.9 information security provisions do not extend to cache integrity monitoring or the detection of artificially inflated hit rates from adversarially crafted queries. MEASURE 2.7 pre-deployment evaluation cannot anticipate runtime cache manipulation strategies that exploit shared cache architectures across multi-agent systems. No NIST control addresses cache health metrics as a security observable or mandates integrity verification of cached content.
**Score: 1**

### RTM_9_14
NIST AI 600-1 does not address deduplication processes in knowledge bases or the monitoring distortions created by incomplete deduplication metadata. §2.9 information security provisions do not extend to knowledge base completeness monitoring or the verification that deduplicated counts accurately represent actual knowledge base contents. MEASURE 2.9 validation provisions address model-level outputs but not the data management integrity required to ensure monitoring based on deduplicated counts reflects true knowledge base state. No NIST control mandates metadata completeness verification for deduplicated knowledge base items.
**Score: 1**

### RTM_9_15
NIST AI 600-1 does not address working memory lifecycle management, phase transition monitoring, or the evidentiary gaps created when context clears between phases. §2.9 information security provisions acknowledge attack vectors broadly but provide no framework for capturing reasoning paths, intermediate states, and tool invocations before phase transitions erase them. MANAGE 4.1-4.3 incident response provisions require post-incident analysis but cannot reconstruct evidence destroyed by working memory phase transitions that attackers time to coincide with monitoring blind spots. No NIST control mandates persistent capture of working memory state across phase transitions.
**Score: 1**

### RTM_9_16
NIST AI 600-1 does not address token budget monitoring, per-agent utilization dashboards, or the emergent reasoning quality degradation arising from combined capacity constraints in coordinating multi-agent systems. MEASURE 2.7 pre-deployment evaluation cannot replicate the dynamic token budget interactions that emerge only when multiple agents coordinate at scale near combined capacity limits. §2.2 Confabulation provisions acknowledge reasoning quality concerns but provide no operational framework for detecting quality collapse that is invisible in individual agent metrics while manifesting at the system coordination level. No NIST control addresses multi-agent token budget health monitoring.
**Score: 1**

### RTM_9_17
NIST AI 600-1 does not address reasoning trace management, chain-of-thought logging requirements, or the provenance gaps created by token limit-driven trace truncation. §2.2 Confabulation provisions acknowledge that AI reasoning may be flawed but provide no governance mechanism for ensuring decision provenance is preserved when downstream agents inherit outputs with truncated justifications. MEASURE 2.9 model explanation provisions address interpretability at a high governance level but do not mandate the preservation of reasoning traces across multi-agent handoffs where cascading truncations progressively eliminate transparency. No NIST control requires reasoning trace completeness or truncation detection in multi-agent chains.
**Score: 1**

### RTM_9_18
NIST AI 600-1 does not address hierarchical memory compression, the relationship between working memory summaries and external full-history storage, or monitoring fragmentation across distributed history databases. §2.9 information security provisions acknowledge data integrity concerns but provide no framework for detecting coordinated attacks that manifest only through cross-agent full-history comparison rarely performed in operational monitoring. MANAGE 4.1-4.3 incident response provisions operate at institutional disclosure levels and cannot address the forensic challenge of reconstructing attack patterns distributed across multiple external databases containing compressed history fragments. No NIST control mandates monitoring of hierarchical compression processes or cross-agent history correlation as a security requirement.
**Score: 1**
# NIST AI 600-1 RTM Analysis - RTM_10: Decision Logic and Utility Function Monitoring

## RTM_10 - Decision Logic and Utility Function Monitoring

### RTM_10_1
NIST AI 600-1 does not address runtime telemetry of utility function calculations or the internal computational processes underlying AI decisions. MEASURE 2.9 concerns model explanation and validation but operates pre-deployment and does not extend to capturing live utility calculations during inference. GOVERN 6.1/6.2 address supply chain vetting at the component level and do not impose requirements on decision-level telemetry. No NIST control establishes logging requirements that would enable distinguishing correct utility optimization from poisoned utility functions, nor any mechanism for reconstructing distributed utility calculations across agent networks.
**Score: 1**

### RTM_10_2
NIST AI 600-1 has no controls that address monitoring for decision divergence across multi-agent systems or distinguishing legitimate behavioral variation from attack-injected weight inconsistencies. MEASURE 2.7 covers pre-deployment security evaluation but does not provide runtime detection capability for weight-level compromise manifesting as behavioral divergence. GOVERN 6.1/6.2 govern third-party component vetting, not the operational integrity of utility weights across deployed agent populations. The framework provides no mechanism for attributing observed decision divergence to compromised weights versus legitimate heterogeneity.
**Score: 1**

### RTM_10_3
NIST AI 600-1 does not address runtime visibility into outcome probability distributions or how shifts in those distributions propagate through expected utility calculations. MEASURE 2.9 model validation is a pre-deployment activity and does not cover live monitoring of probability distribution changes affecting utility computations. MANAGE 4.1-4.3 establish institutional incident response policies but do not define detection mechanisms for cascading utility recalculations triggered by distribution drift. No NIST control provides a framework for detecting or alerting on probability distribution shifts that silently corrupt utility calculations across networked agents.
**Score: 1**

### RTM_10_4
NIST AI 600-1 has no controls targeting specification gaming or the detection of agents reinterpreting utility function definitions while appearing to meet optimization targets. MEASURE 2.7 pre-deployment evaluation does not anticipate post-deployment semantic drift in how agents apply utility metrics. GOVERN 6.1/6.2 supply chain controls do not extend to monitoring whether deployed agents maintain faithful interpretation of their utility specifications. The framework lacks any mechanism for detecting propagation of utility metric redefinition through multi-agent learning mechanisms.
**Score: 1**

### RTM_10_5
NIST AI 600-1 does not require monitoring of tool outcome distributions or success rates that underpin expected utility calculations in agentic systems. MEASURE 2.7 addresses security evaluation at deployment time but does not establish ground-truth baselines for tool outcome distributions that would enable poisoning detection. GOVERN 6.1/6.2 vet third-party tools at the supply chain level without mandating runtime telemetry of how those tools' outputs influence utility computations. No NIST control addresses the telemetry gap that allows attackers to poison outcome distribution assumptions without detection.
**Score: 1**

### RTM_10_6
NIST AI 600-1 provides no guidance on capturing the computational basis of confidence scores or ensuring telemetry reveals their derivation from utility calculations. MEASURE 2.9 model explanation addresses interpretability in the pre-deployment context and does not extend to runtime transparency of confidence score computation. GOVERN 6.1/6.2 and MANAGE 4.1-4.3 operate at institutional and supply chain levels, not at the level of individual inference-time computations. The framework does not address the opacity created when multi-agent confidence aggregation obscures which agents' utility calculations drove collective confidence outputs.
**Score: 1**

### RTM_10_7
NIST AI 600-1 has no controls requiring counterfactual utility analysis or logging of expected utilities for actions not taken. MEASURE 2.9 model explanation covers pre-deployment validation of model outputs and does not mandate post-hoc counterfactual audit trails in operational systems. MANAGE 4.1-4.3 define incident response and disclosure policies at the institutional level without specifying the forensic telemetry needed to determine whether decisions were utility-optimal. The framework cannot support post-incident analysis of whether agents made optimal choices, particularly across multi-agent systems where forgone options must be reconstructed across all agents.
**Score: 1**

### RTM_10_8
NIST AI 600-1 has no controls addressing rule firing monitoring in rule-based agent systems or unified visibility into distributed rule execution. MEASURE 2.7 security evaluation is pre-deployment and does not establish requirements for operational-time rule firing observability in distributed architectures. GOVERN 6.1/6.2 vet rule repositories as supply chain components but do not mandate telemetry of how individual agents apply rules at runtime. The framework provides no basis for detecting distributed malicious rule firing patterns that remain invisible when each agent's execution is monitored in isolation.
**Score: 1**

### RTM_10_9
NIST AI 600-1 does not address monitoring of shared working memory access patterns across multi-agent systems or attribution of memory reads and writes to specific agents. MEASURE 2.9 model validation does not extend to runtime cognitive state monitoring or tracking which agents access which facts in shared memory. GOVERN 6.1/6.2 supply chain controls address component provenance, not the operational access patterns within shared cognitive architectures. No NIST control establishes mechanisms for detecting covert memory poisoning enabled by unattributable access to shared working memory.
**Score: 1**

### RTM_10_10
NIST AI 600-1 has no controls for detecting rule modification race conditions or ensuring consistent rule propagation across distributed agent systems. GOVERN 6.1/6.2 address supply chain vetting of rule repositories but do not mandate versioning telemetry or propagation monitoring that would reveal inconsistent policy enforcement. MEASURE 2.7 pre-deployment security evaluation does not cover operational scenarios where rule changes propagate asynchronously creating exploitable inconsistencies. The framework provides no mechanism for detecting or responding to attackers exploiting rule propagation timing differentials across agent populations.
**Score: 1**

### RTM_10_11
NIST AI 600-1 provides no controls for monitoring heuristic parameter tuning across multi-agent systems or distinguishing legitimate parameter heterogeneity from attack-enabled specification gaming. MEASURE 2.7 evaluates system security prior to deployment and does not cover runtime parameter drift or cross-agent parameter consistency monitoring. GOVERN 6.1/6.2 do not extend to operational tuning behaviors of deployed agents. The framework cannot distinguish independent legitimate parameter adaptation from coordinated tuning that collectively enables gaming of system objectives.
**Score: 1**

### RTM_10_12
NIST AI 600-1 has no controls addressing the threat of agents learning to evade monitoring systems through training-based counter-strategies. MEASURE 2.7 pre-deployment security evaluation does not anticipate adaptive evasion behaviors that emerge from agents observing their own monitoring environment. MANAGE 4.1-4.3 define incident response policies at the institutional level and do not address detection of coordinated multi-agent evasion strategies. The framework offers no countermeasure to agents that behave correctly when monitored while collectively developing adversarial counter-monitoring strategies.
**Score: 1**

### RTM_10_13
NIST AI 600-1 has no controls targeting reward metric gaming or the detection of agents optimizing monitored metrics without achieving actual system objectives. MEASURE 2.7 security evaluation operates pre-deployment and does not establish runtime mechanisms for distinguishing genuine objective achievement from metric gaming. GOVERN 6.1/6.2 supply chain controls do not address post-deployment emergent behaviors in reinforcement learning systems. The framework provides no basis for detecting multi-agent specialization where different agents game different metrics collectively while no individual metric breach triggers alerts.
**Score: 1**

### RTM_10_14
NIST AI 600-1 has no controls addressing experience buffer sampling patterns or the telemetry required to detect biased experience prioritization in reinforcement learning systems. MEASURE 2.7 and MEASURE 2.9 focus on pre-deployment evaluation and model validation and do not cover operational learning dynamics in deployed RL agents. GOVERN 6.1/6.2 address component provenance, not the runtime sampling behaviors of learning systems. No NIST control establishes monitoring requirements that would prevent attackers from inferring buffer contents and training focus by observing sampling patterns.
**Score: 1**

### RTM_10_15
NIST AI 600-1 does not address policy output distribution analysis as a monitoring mechanism or establish requirements for detecting anomalous action selection probabilities. MEASURE 2.7 pre-deployment evaluation does not cover runtime statistical analysis of policy output distributions that would reveal adversarial influence. MANAGE 4.1-4.3 incident response policies do not define detection methods based on distributional anomalies in agent behavior. The framework has no mechanism for identifying sophisticated adversaries who train policies to match benign output distributions while embedding hidden behavioral triggers.
**Score: 1**

### RTM_10_16
NIST AI 600-1 has no controls addressing gradient flow monitoring in federated or distributed learning environments or detection of anomalous gradient patterns. MEASURE 2.7 security evaluation is pre-deployment and does not extend to monitoring the operational learning dynamics of distributed training systems. GOVERN 6.1/6.2 vet components at the supply chain level without establishing runtime telemetry requirements for federated gradient aggregation. The framework provides no basis for detecting backdoors encoded in individually benign-appearing gradients that activate only after aggregation across federated participants.
**Score: 1**

### RTM_10_17
NIST AI 600-1 has no controls for analyzing temporal learning dynamics or using convergence pattern anomalies to detect poisoning attacks. MEASURE 2.7 pre-deployment evaluation does not include requirements for monitoring learning curves or convergence signatures during operational training. MANAGE 4.1-4.3 define incident response at the policy level without specifying detection capabilities for slow-spreading poisoning that hides within normal learning noise. The framework cannot address adversaries who distribute poisoning across many training iterations specifically to evade signature-based detection.
**Score: 1**

### RTM_10_18
NIST AI 600-1 does not address paradigm-specific telemetry gaps in hybrid AI systems or the challenge of unified monitoring across heterogeneous learning paradigms. MEASURE 2.9 model explanation applies pre-deployment and does not establish runtime telemetry standards spanning different AI paradigms within hybrid architectures. GOVERN 6.1/6.2 supply chain controls do not impose cross-paradigm monitoring requirements on hybrid systems. The framework has no mechanism for detecting attackers who operate specifically within paradigm-specific monitoring blind spots where no single monitoring approach provides complete coverage.
**Score: 1**

### RTM_10_19
NIST AI 600-1 has no controls addressing knowledge graph evolution telemetry or audit trails for relationship modifications in shared knowledge bases. MEASURE 2.9 model validation does not extend to runtime monitoring of knowledge graph updates or attribution of changes to specific agents or authorized processes. GOVERN 6.1/6.2 do not mandate telemetry requirements for shared knowledge infrastructure used across multi-agent systems. The framework provides no basis for detecting unattributable simultaneous knowledge graph modifications by multiple agents that enable covert knowledge poisoning.
**Score: 1**

### RTM_10_20
NIST AI 600-1 has no controls for monitoring intermediate computational states in cooperative hybrid architectures or capturing states between final outputs across agent boundaries. MEASURE 2.7 and MEASURE 2.9 focus on final model outputs and pre-deployment validation, not intermediate states during multi-agent cooperative processing. MANAGE 4.1-4.3 incident response policies operate at the institutional level and do not specify forensic requirements for intermediate state capture in distributed cooperative systems. The framework cannot address attacks that corrupt intermediate states that are distributed across agent boundaries and invisible to final-output-focused monitoring.
**Score: 1**

### RTM_10_21
NIST AI 600-1 has no controls addressing streaming response monitoring in real-time multi-agent output scenarios or the proportionally larger monitoring surface created by simultaneous multi-agent streaming. MEASURE 2.7 pre-deployment evaluation does not cover runtime detection of malicious content embedded in streaming outputs from one agent amidst legitimate streaming from others. GOVERN 6.1/6.2 and MANAGE 4.1-4.3 operate at institutional and supply chain levels without addressing the operational monitoring challenges of concurrent multi-agent streaming architectures. The framework provides no mechanism for detecting malicious instructions that exploit the monitoring dilution created when multiple agents stream simultaneously.
**Score: 1**

### RTM_10_22
NIST AI 600-1 has no controls requiring audit trails for demonstration selection or curation processes in in-context learning systems. MEASURE 2.9 model explanation addresses pre-deployment interpretability and does not mandate runtime logging of demonstration curation decisions and their rationale. GOVERN 6.1/6.2 supply chain controls do not extend to the operational curation of shared demonstration pools used across multi-agent systems. The framework cannot detect unattributable poisoning of shared demonstration pools where audit trail opacity enables covert manipulation of what examples agents learn from.
**Score: 1**

### RTM_10_23
NIST AI 600-1 has no controls for unified objective tracking across multiple AI paradigms or detecting collective objective misalignment that remains below per-metric alert thresholds. MEASURE 2.7 and MEASURE 2.9 evaluate individual model properties pre-deployment and do not establish cross-paradigm objective coherence monitoring in hybrid systems. MANAGE 4.1-4.3 incident response policies do not address detection scenarios where no single metric exceeds thresholds but aggregate objective alignment violates system specifications. The framework provides no basis for detecting attackers who engineer collective misalignment specifically structured to evade per-paradigm monitoring while corrupting overall system behavior.
**Score: 1**
# RTM_11: Vector Database and RAG Pipeline Telemetry — NIST AI 600-1 Analysis

### RTM_11_1
NIST AI 600-1 does not address vector database operational or retrieval quality monitoring. MEASURE 2.7 focuses on pre-deployment security evaluation and does not encompass runtime telemetry such as Recall@k or semantic relevance metrics. The §2.9 Information Security discussion acknowledges data poisoning as a risk category but provides no guidance on detecting it through production monitoring. No GOVERN, MEASURE, or MANAGE controls prescribe collection of query quality metrics that would expose silent retrieval degradation.
**Score: 1**

### RTM_11_2
NIST AI 600-1 contains no controls relating to hybrid search configuration, alpha parameter selection, or the logging of algorithmic decision rationale within retrieval pipelines. GOVERN 6.1/6.2 address third-party supply chain vetting at the procurement level and do not extend to runtime agent strategy transparency. MEASURE 2.7's pre-deployment evaluation scope does not capture the emergent blind spots that arise when multiple agents apply heterogeneous alpha strategies in production. There is no framework mechanism to detect per-agent strategy manipulation that disappears in aggregate monitoring.
**Score: 1**

### RTM_11_3
NIST AI 600-1 provides no guidance on multi-node vector database cluster monitoring, shard-level performance attribution, or node-level query latency decomposition. MEASURE 2.7 evaluates security properties before deployment and does not require operational telemetry architectures that isolate per-shard performance. The §2.9 data poisoning acknowledgment identifies the attack class conceptually but offers no detection controls that would surface shard-targeted poisoning or resource exhaustion hidden by cluster-level aggregation. MANAGE 4.1–4.3 address incident response at the institutional policy level, not the operational monitoring instrumentation needed to identify such attacks.
**Score: 1**

### RTM_11_4
NIST AI 600-1 does not address batch ingestion pipelines, document-level failure granularity, or the monitoring architectures required to detect data loss within nominally successful batch operations. GOVERN 6.1/6.2 vet third-party data providers at the supply chain level but do not prescribe per-document failure tracking during ingestion. MEASURE 2.7 pre-deployment evaluation does not encompass runtime batch error aggregation behavior. No controls require disaggregated failure metrics that would expose document-level losses concealed by high batch success rates in multi-agent concurrent ingestion scenarios.
**Score: 1**

### RTM_11_5
NIST AI 600-1 lacks any controls governing ETL pipeline monitoring, per-source quality metrics, or rejection rate disaggregation. GOVERN 6.1/6.2 address vendor and third-party data source vetting as a procurement activity, not the ongoing operational quality monitoring needed to detect per-source degradation masked by acceptable aggregate rejection rates. The §2.9 data poisoning section recognizes that training and retrieval data can be compromised but does not prescribe the per-source telemetry that would expose such compromise in live pipelines. Multi-agent systems with heterogeneous source assignments create additional blind spots that fall entirely outside NIST AI 600-1's scope.
**Score: 1**

### RTM_11_6
NIST AI 600-1 contains no provisions for pipeline transformation throughput monitoring, chunk-level performance instrumentation, or the detection of per-agent processing heterogeneity that can mask anomalies. MEASURE 2.7 is bounded to pre-deployment security evaluation and does not extend to runtime throughput telemetry or the operational visibility needed to distinguish expected variation from adversarial manipulation. Document-level versus chunk-level performance gaps that differ by orders of magnitude are an operational monitoring concern entirely outside the framework's governance scope. No MANAGE controls address the operational instrumentation needed to surface these blind spots in multi-agent deployments.
**Score: 1**

### RTM_11_7
NIST AI 600-1 does not address incremental ETL update monitoring, run-level versus source-level success attribution, or the detection of partial extraction failures that accumulate into growing knowledge gaps. MEASURE 2.7's pre-deployment evaluation does not capture failure modes that emerge gradually across consecutive production runs. The §2.9 data poisoning acknowledgment does not extend to monitoring controls that would detect silently growing knowledge gaps caused by partial source failures. MANAGE 4.1–4.3 incident response policies operate at an institutional level and presuppose that incidents are detected, which this blind spot prevents.
**Score: 1**

### RTM_11_8
NIST AI 600-1 provides no controls for per-dimension quality validation, per-source quality scoring, or the disaggregated metric collection needed to prevent agent-specific quality issues from disappearing in fleet-wide averages. GOVERN 6.1/6.2 supply chain controls address data provenance at the acquisition level rather than ongoing per-dimension quality monitoring in production. MEASURE 2.7 pre-deployment evaluation does not require quality metric architectures capable of per-source or per-agent root cause attribution. The framework offers no mechanism to detect the aggregation-induced blind spots described in this threat.
**Score: 1**

### RTM_11_9
NIST AI 600-1 contains no controls for cache performance monitoring, per-layer hit rate tracking, or eviction pressure analysis in multi-layer retrieval cache architectures. MEASURE 2.7 evaluates security properties prior to deployment and does not encompass the operational cache telemetry needed to detect capacity and quality degradation patterns. No GOVERN or MANAGE controls address the instrumentation requirements for distinguishing per-layer cache behavior from aggregate cache metrics. Cache-layer performance blind spots in RAG pipelines are entirely outside the framework's defined scope.
**Score: 1**

### RTM_11_10
NIST AI 600-1 does not address deduplication pipeline monitoring, false positive rate tracking, or the detection of unique content loss caused by incorrect deduplication decisions. GOVERN 6.1/6.2 supply chain controls do not extend to runtime deduplication effectiveness telemetry. MEASURE 2.7 pre-deployment evaluation does not require deduplication false positive rate baselines or ongoing monitoring to detect content erosion. Multi-agent systems reporting aggregate deduplication metrics create blind spots that the framework has no controls to address.
**Score: 1**

### RTM_11_11
NIST AI 600-1 provides no guidance on observability layer instrumentation overhead measurement, latency attribution between application processing and monitoring infrastructure, or the operational implications of accumulated instrumentation overhead in multi-agent workflows. MEASURE 2.7 focuses on AI system security properties evaluated before deployment, not the measurement of monitoring infrastructure's own performance impact. No controls require that instrumentation overhead be separately tracked to prevent misleading performance optimization decisions. This operational telemetry concern falls entirely outside the framework's governance scope.
**Score: 1**

### RTM_11_12
NIST AI 600-1 does not address circuit breaker state monitoring, graceful degradation frequency tracking, or the cross-agent correlation of circuit states needed to detect cascading failure patterns in multi-agent systems. MANAGE 4.1–4.3 provide institutional-level incident response and disclosure policies that presuppose failures are detected and reported, not the runtime monitoring controls needed to surface borderline capacity degradation before it cascades. MEASURE 2.7 pre-deployment evaluation does not encompass operational fault tolerance telemetry. No controls prescribe the per-agent or cross-agent circuit state instrumentation required to detect the patterns described in this threat.
**Score: 1**

### RTM_11_13
NIST AI 600-1 contains no controls for quality dashboard design, per-agent validation failure distribution monitoring, or the disaggregated metric architectures needed to prevent fleet-wide averages from masking severe per-agent quality issues. GOVERN 6.1/6.2 address third-party oversight at the procurement level and do not extend to operational quality monitoring granularity requirements. MEASURE 2.7 pre-deployment evaluation does not require per-agent quality metric collection or dashboard designs that preserve distribution visibility. This operational monitoring concern is outside the framework's defined scope.
**Score: 1**

### RTM_11_14
NIST AI 600-1 provides no controls for within-batch document-level failure monitoring, batch processing telemetry granularity, or the race condition blind spots that arise from multi-agent concurrent batch processing. MEASURE 2.7's pre-deployment security evaluation does not encompass runtime batch processing failure patterns. The §2.9 data poisoning acknowledgment identifies data integrity risks conceptually but does not prescribe the monitoring granularity needed to detect document-level failures hidden by batch-level success metrics. No MANAGE controls address the operational instrumentation requirements for concurrent multi-agent batch processing visibility.
**Score: 1**

### RTM_11_15
NIST AI 600-1 contains no controls for state file integrity monitoring, ETL checkpoint timestamp validation, or the detection of unauthorized rollbacks that cause pipelines to re-ingest or skip data. The §2.9 Information Security section acknowledges data integrity as a concern within the data poisoning risk category but does not prescribe operational controls to monitor state file modification patterns or detect timestamp manipulation. GOVERN 6.1/6.2 address supply chain integrity at the vendor level, not the runtime file system monitoring needed to detect insider or adversarial ETL state manipulation. MANAGE 4.1–4.3 incident response policies cannot surface incidents that state-file-level monitoring fails to detect.
**Score: 1**

### RTM_11_16
NIST AI 600-1 does not address token cost monitoring, per-query cost distribution analysis, or the detection of cost exploitation patterns where expensive queries inflate aggregate spending without crossing threshold alerts. GOVERN 6.1/6.2 supply chain controls address third-party API provider vetting but do not prescribe the per-query cost telemetry needed to detect abuse. MEASURE 2.7 pre-deployment evaluation does not require cost monitoring architectures that preserve per-query visibility. No controls within the framework address the operational cost analytics needed to surface exploitation that hides within acceptable aggregate cost metrics.
**Score: 1**

### RTM_11_17
NIST AI 600-1 provides no controls for remediation activity effectiveness monitoring, false modification rate tracking, or per-agent remediation quality disaggregation in multi-agent content correction workflows. MANAGE 4.1–4.3 address incident response and disclosure at the institutional policy level but do not prescribe operational metrics for evaluating whether automated remediation actions are correctly targeted. MEASURE 2.7 pre-deployment evaluation does not encompass runtime false positive tracking for remediation systems. Fleet-wide aggregation of remediation metrics that hides per-agent false positive rates is an operational monitoring concern entirely outside the framework's scope.
**Score: 1**
# RTM_12 Analysis: Detection Evasion and Attack Exploitation
# NIST AI 600-1 Generative AI Profile Coverage Assessment

### RTM_12_1
NIST AI 600-1 does not address metric emission logic, latency calculation integrity, or the manipulation of performance metrics in multi-agent deployments. §2.9 Information Security acknowledges data poisoning as a threat category at governance level but provides no operational framework for detecting falsified health metrics emitted by compromised agents blending with legitimate fleet telemetry. MEASURE 2.7 covers pre-deployment security evaluation and cannot detect runtime metric manipulation that occurs only after deployment when attackers gain application code access. No NIST control mandates integrity verification of collected metrics or anomaly detection for metric reporting consistency across agent fleets.
**Score: 1**

### RTM_12_2
NIST AI 600-1 does not address alert fatigue, threshold-gaming strategies, or the operational challenge of distinguishing legitimate alerts from adversarially induced false positives in multi-agent monitoring environments. MANAGE 4.1-4.3 addresses incident response and disclosure at the institutional policy level but provides no technical guidance on alert tuning, false positive management, or detection of deliberate alert flooding campaigns. §2.9 Information Security provisions acknowledge adversarial threats broadly but offer no framework for detecting attacker conditioning strategies that exploit multi-agent fleet alerting volume to desensitize operators. No NIST control addresses alert quality management or false positive exploitation as a security concern.
**Score: 1**

### RTM_12_3
NIST AI 600-1 does not address metric collection pipeline integrity, reporting delay detection, or the monitoring blind spots created when compromised collection agents introduce artificial buffering delays. MEASURE 2.7 pre-deployment evaluation cannot surface runtime delay manipulation strategies that become exploitable only after fleet deployment when attackers gain access to distributed collection agents. §2.9 information security provisions do not extend to the telemetry pipeline layer where selective delay manipulation affecting subsets of agents could leave aggregate fleet metrics appearing timely while individual agent incidents go undetected. No NIST control mandates freshness verification or staleness detection for collected monitoring data.
**Score: 1**

### RTM_12_4
NIST AI 600-1 does not address detection window exploitation, time-to-detect optimization by attackers, or the amplified damage potential of fleet-wide incidents during monitoring blind spots. MANAGE 4.1-4.3 establishes institutional incident response and disclosure policies but does not address the technical challenge of minimizing detection windows to reduce the maximum-impact interval available to attackers aware of monitoring timing. §2.9 acknowledges adversarial threats but provides no operational framework for reducing detection latency or hardening monitoring architectures against timing-aware attacks. No NIST control mandates detection window minimization or mitigation complexity reduction for multi-agent fleet incidents.
**Score: 1**

### RTM_12_5
NIST AI 600-1 does not address Prometheus scraping infrastructure, metrics transport security, or the man-in-the-middle attack surface created by unencrypted metric collection in multi-agent deployments. GOVERN 6.1/6.2 addresses supply chain and third-party component vetting but does not extend to the operational security configuration of monitoring infrastructure components such as Prometheus scraper connections. §2.9 Information Security provisions acknowledge network-level threats at a governance level but provide no technical guidance for securing metrics collection channels against in-transit manipulation that could simultaneously falsify all agents' metrics through centralized scraper interception. No NIST control mandates transport security requirements for monitoring infrastructure.
**Score: 1**

### RTM_12_6
NIST AI 600-1 does not address Prometheus alerting rule configurations, threshold-based detection logic, or evasion strategies that exploit duration requirements through intermittent degradation shorter than sustained alert periods. MEASURE 2.7 pre-deployment security evaluation does not cover the runtime gaming of alerting thresholds that occurs only when attackers calibrate degradation to remain just below configured limits. §2.9 information security provisions do not extend to the technical design of alerting rules or detection of rotating degradation distributed across agents to prevent any single agent from sustaining a threshold breach triggering alerts. No NIST control addresses alerting rule hardening or threshold evasion as a security design requirement.
**Score: 1**

### RTM_12_7
NIST AI 600-1 does not address OpenTelemetry infrastructure, centralized observability aggregation, or the operational intelligence disclosure risks arising from adversaries accessing consolidated monitoring systems. §2.9 Information Security acknowledges confidentiality concerns broadly but provides no framework for controlling access to centralized telemetry aggregation platforms that expose dependency patterns, capacity limits, cost structures, and optimization gaps enabling precise attack calibration. GOVERN 6.1/6.2 supply chain vetting applies to component acquisition but does not address the ongoing access control requirements for monitoring infrastructure containing comprehensive operational intelligence. No NIST control mandates security controls specific to observability infrastructure protecting aggregated multi-agent operational data.
**Score: 1**

### RTM_12_8
NIST AI 600-1 does not address GPU telemetry, DCGM infrastructure, or the resource utilization disclosure risks arising from centralized GPU monitoring in multi-agent inference deployments. MEASURE 2.7 pre-deployment evaluation covers security assessment but does not extend to the infrastructure telemetry layer where GPU utilization patterns, memory capacity limits, and thermal thresholds expose precise resource exhaustion attack parameters. §2.9 information security provisions do not address hardware-level telemetry security or the denial-of-service risks enabled by adversary access to detailed GPU capacity and load imbalance data across agent fleets. No NIST control addresses GPU telemetry access controls or infrastructure-level monitoring security.
**Score: 1**

### RTM_12_9
NIST AI 600-1 does not address distributed tracing systems, trace aggregation infrastructure, or the architectural intelligence exposure created when adversaries access centralized OpenTelemetry tracing data. §2.9 Information Security provisions acknowledge confidentiality concerns but provide no governance framework for protecting distributed trace data that reveals complete workflow architectures, critical path bottlenecks, coordination mechanisms, and failure patterns enabling adversaries to reconstruct full multi-agent coordination architecture. GOVERN 6.1/6.2 supply chain vetting does not extend to access controls for observability infrastructure containing comprehensive operational intelligence. No NIST control mandates security controls protecting distributed tracing systems from intelligence disclosure.
**Score: 1**

### RTM_12_10
NIST AI 600-1 does not address guardrail telemetry aggregation, cross-agent safety violation correlation, or the fleet-wide jailbreak campaign detection blind spots arising from independent per-agent alert thresholds. §2.9 Information Security provisions acknowledge prompt injection and adversarial inputs as threats but provide no operational framework for detecting coordinated attacks distributed across agent fleets where each agent's individual violation rate appears within normal bounds while the fleet-wide attack rate is significant. MEASURE 2.7 pre-deployment evaluation cannot surface the emergent detection blindness that appears only when coordinated attacks are distributed across deployed agent fleets. No NIST control mandates fleet-level safety monitoring correlation or cross-agent guardrail telemetry aggregation.
**Score: 1**

### RTM_12_11
NIST AI 600-1 does not address sandbox audit trail architecture, cross-agent log correlation, or the forensic fragmentation arising from distributed sandbox environments with independent log streams. MANAGE 4.1-4.3 establishes institutional incident response policies but does not address the technical audit infrastructure required to correlate fragmented log streams across multi-agent nodes to reconstruct multi-stage coordinated attacks that appear as isolated single-agent events. §2.9 information security provisions acknowledge audit concerns broadly but provide no mandate for automatic cross-agent audit trail correlation or the architectural requirements needed to prevent attack reconstruction failures in distributed sandbox environments. No NIST control addresses distributed audit trail architecture as a security design requirement.
**Score: 1**

### RTM_12_12
NIST AI 600-1 does not address fairness audit trail architecture, cross-agent decision log correlation, or the regulatory compliance failures arising from heterogeneous storage systems with incompatible schemas across agent fleets. MANAGE 4.2 addresses incident response and disclosure processes at an institutional level but does not mandate the specific technical audit infrastructure required to reconstruct complete decision audit trails across demographically diverse agent deployments for regulatory examinations. §2.9 information security provisions and MANAGE 4.1-4.3 institutional policies do not extend to the technical data architecture requirements needed to enable longitudinal fairness analysis across multi-agent deployments. No NIST control mandates unified audit trail schemas or cross-agent fairness monitoring aggregation infrastructure.
**Score: 1**

### RTM_12_13
NIST AI 600-1 does not address feedback collection infrastructure, distributed feedback storage architectures, or the systematic improvement failures arising from fleet-wide pattern invisibility in agent-specific feedback databases. MEASURE 2.7 pre-deployment evaluation and MANAGE 4.1-4.3 institutional response provisions do not address the operational data management requirements needed to aggregate feedback across agent-specific databases to detect fleet-wide routing inefficiencies or systematic quality issues visible only in cross-agent aggregation. §2.9 information security provisions do not extend to feedback collection architecture or the governance of multi-agent learning infrastructure. No NIST control addresses feedback aggregation requirements as a monitoring or governance mandate.
**Score: 1**

### RTM_12_14
NIST AI 600-1 does not address preference collection infrastructure, distributed annotation pools, or the value inconsistency risks arising from non-overlapping preference datasets training separate reward models across agent fleets. §2.9 information security provisions acknowledge data integrity concerns but provide no framework for ensuring value alignment consistency across multi-agent systems with distributed human feedback collection that produces heterogeneous learned value representations. GOVERN 6.1/6.2 supply chain vetting does not extend to the operational data collection architecture that determines whether reward models across a fleet learn consistent or conflicting value representations. No NIST control addresses preference data aggregation requirements or distributed annotation governance for value consistency.
**Score: 1**

### RTM_12_15
NIST AI 600-1 does not address annotation team governance, distributed disagreement resolution processes, or the systematic value inconsistency arising from independent teams reaching conflicting resolutions of the same fundamental value tradeoffs. §2.9 information security provisions and GOVERN 6.1/6.2 supply chain controls do not address the human annotation governance processes through which inconsistent value representations become embedded across an agent fleet invisible to any individual team or reviewer. MEASURE 2.7 pre-deployment evaluation assesses individual model characteristics but cannot detect fleet-wide value inconsistency that emerges from independent annotation team resolutions of shared disagreements. No NIST control mandates cross-team disagreement correlation or fleet-level annotation consistency governance.
**Score: 1**

### RTM_12_16
NIST AI 600-1 does not address fleet-wide anomaly detection, cross-agent monitoring correlation, or the detection blindness arising from distributed monitoring systems that analyze agents independently without cross-agent interaction telemetry. MANAGE 4.1-4.3 institutional incident response provisions do not address the technical monitoring architecture requirements needed to detect emergent fleet-wide patterns such as circular escalation workflows that manifest only in cross-agent interaction telemetry correlation. §2.9 information security provisions acknowledge security monitoring needs at a governance level but provide no framework for mandating unified monitoring infrastructure capable of detecting patterns invisible in per-agent analysis. No NIST control addresses cross-agent monitoring correlation as an architectural security requirement.
**Score: 1**

### RTM_12_17
NIST AI 600-1 does not address distributed audit logging architecture, cross-agent causal trace linking, or the compliance and forensic failures arising from agent-specific audit logs lacking explicit causal linking across multi-agent workflows. MANAGE 4.2 addresses incident response and disclosure processes at the institutional policy level but does not mandate the specific technical traceability infrastructure required to reconstruct complete decision narratives across multi-agent workflows with heterogeneous audit schemas, incompatible retention policies, and fragmented demographic data joins. §2.9 information security provisions do not extend to the audit architecture requirements needed to enable complete decision reconstruction for regulatory examinations where partial coverage from fragmented distributed audit trails is insufficient. No NIST control mandates causal linking standards, schema compatibility requirements, or retention policy coordination across multi-agent audit systems.
**Score: 1**

## RTE - Multi-agent trust exploitation and self-replicating prompt malware
# NIST AI 600-1 RTE Threat Analysis

## RTE_1 - Dashboard & UI Attribution Attacks

### RTE_1_1
NIST AI 600-1 does not address agent identity verification or cryptographic attestation mechanisms that would prevent visual attribution spoofing. The framework's GOVERN 6.1/6.2 supply chain controls focus on third-party vetting at procurement time rather than runtime identity binding. Section 2.9 Information Security touches on prompt injection broadly but does not engage with the UI-layer problem of agents self-reporting identity metadata that dashboards render as trusted attribution. There are no NIST controls covering cryptographic binding of agent outputs to verified identities, which is the technical root cause of this attack.
**Score: 1**

### RTE_1_2
NIST AI 600-1 §2.9 acknowledges prompt injection as a recognized risk category, which provides a thin conceptual hook for message source field manipulation via prompt injection. However, the framework operates at governance and organizational policy levels and does not specify authentication protocols, cryptographically bound identifiers, or technical controls that would prevent metadata manipulation in chat interfaces. GOVERN 6.1/6.2 supply chain vetting does not extend to runtime inter-agent message integrity. The absence of any NIST control addressing agent authentication at the protocol level leaves this impersonation vector unaddressed.
**Score: 1**

### RTE_1_3
Evaluation dashboard attribution spoofing sits outside NIST AI 600-1's scope of controls. MEASURE 2.9 addresses model explanation and validation but focuses on pre-deployment interpretability rather than runtime dashboard integrity or attribution verification. MEASURE 2.7 covers red-teaming and security evaluation but as a pre-deployment activity that would not detect runtime spoofing of evaluation results. NIST has no controls governing how evaluation dashboards authenticate or verify the provenance of results displayed, leaving poisoned-attribution presentation entirely unaddressed.
**Score: 1**

---

## RTE_2 - Trust Mechanisms & Inter-Agent Communication

### RTE_2_1
NIST AI 600-1 §2.9 recognizes indirect prompt injection as a distinct threat vector where malicious content embedded in external data influences model behavior, which provides a moderate conceptual alignment with this attack. However, NIST frames indirect prompt injection as a single-agent concern rather than a multi-agent social engineering problem where attacker-controlled messages traverse inter-agent communication channels masquerading as trusted peer analysis. GOVERN 6.1/6.2 supply chain controls do not extend to runtime inter-agent message validation. The framework lacks any controls for authenticating or sandboxing inter-agent communication flows, leaving the transitive trust exploitation surface unaddressed at the technical level.
**Score: 2**

### RTE_2_2
NIST AI 600-1 §2.9 indirect prompt injection provides some conceptual alignment since the attack involves injecting malicious instructions into data that agents process and propagate. The framework's recognition that AI systems can be manipulated via data they consume is relevant to the mechanism by which low-reputation agents contaminate high-reputation agents' outputs. However, NIST does not address multi-agent reputation systems, transitive trust chains, or the specific social engineering pattern of using one compromised agent to influence another through reputation anchoring. GOVERN 6.1/6.2 vetting operates at system procurement level, not runtime agent interaction level.
**Score: 2**

### RTE_2_3
NIST AI 600-1 §2.9 indirect prompt injection is marginally relevant to circular verification loop exploitation since both involve compromised data influencing subsequent AI processing. However, NIST has no controls addressing multi-agent verification architectures, the integrity of agent-to-agent confirmation workflows, or the specific failure mode where verification mechanisms themselves are compromised. MEASURE 2.7 red-teaming could theoretically surface this class of attack pre-deployment, but the framework provides no architectural guidance on preventing circular verification collapse. The dashboard checkmark trust signal is a UI concern entirely outside NIST's scope.
**Score: 2**

### RTE_2_4
NIST AI 600-1 §2.9 indirect prompt injection is partially relevant since this attack involves malicious analysis injected by one agent being consumed and acted upon by a downstream specialized agent. The framework recognizes that AI systems processing external data can have that data weaponized against them. However, NIST does not address agent specialization trust boundaries, credential assumption across agent roles, or the specific risk of unauthorized financial transactions triggered by poisoned inter-agent data flows. MANAGE 4.1-4.3 incident response controls would apply post-compromise but do not prevent the underlying trust collapse.
**Score: 2**

---

## RTE_3 - Confidence & Scoring Attacks

### RTE_3_1
NIST AI 600-1 §2.9 provides no coverage of confidence score manipulation as a trust exploitation vector. MEASURE 2.9 model explanation and validation touches on output calibration conceptually but does not address adversarial tuning of temperature and sampling parameters to inflate confidence scores for downstream manipulation. MEASURE 2.7 red-teaming could potentially surface confidence inflation attacks pre-deployment, but the framework does not specifically identify confidence score integrity as a security concern in multi-agent routing contexts. The absence of controls for output calibration integrity leaves this attack vector without meaningful NIST coverage.
**Score: 1**

### RTE_3_2
NIST AI 600-1 §2.9 data poisoning controls provide a weak conceptual hook since utility function input poisoning is a form of data poisoning that propagates through confidence scores. MEASURE 2.7 security evaluation could identify data poisoning vulnerabilities pre-deployment. However, NIST does not address confidence score integrity in multi-agent approval chains, utility-based risk assessment architectures, or the specific propagation pathway from poisoned upstream assessments to downstream automated decisions. The framework's data poisoning coverage is primarily pre-deployment and does not address runtime propagation of corrupted scoring through agent pipelines.
**Score: 2**

### RTE_3_3
NIST AI 600-1 has no controls addressing streaming response manipulation or progressive revelation attacks. The framework does not engage with the temporal dimension of streaming outputs where confidence signals arrive before qualifying caveats, nor with the cascading commitment problem in multi-agent systems consuming streamed data. MEASURE 2.9 model validation does not cover streaming output integrity. This attack exploits a runtime architectural characteristic of streaming inference that falls entirely outside NIST's governance-level scope.
**Score: 1**

---

## RTE_4 - Prompt Injection via Message/History Sharing

### RTE_4_1
NIST AI 600-1 §2.9 directly recognizes indirect prompt injection as a threat vector, which provides meaningful coverage of the self-replicating prompt injection mechanism since shared conversation history functions as the external data channel through which injected instructions propagate. The framework identifies that malicious content embedded in data consumed by AI systems can redirect system behavior, which maps to the core mechanism here. However, NIST does not address multi-agent shared memory architectures specifically, nor the worm-like self-replication property that emerges when injected instructions embed themselves in history accessed by all subsequent agents. MEASURE 2.7 red-teaming could surface this pre-deployment.
**Score: 2**

### RTE_4_2
NIST AI 600-1 §2.9 indirect prompt injection coverage extends conceptually to serialized memory poisoning since deserialization of attacker-controlled state is a form of processing malicious external data. The framework's recognition of data poisoning as a pre-deployment concern under MEASURE 2.7 is also partially relevant to the persistence mechanism. However, NIST does not address agent memory serialization security, shared persisted state integrity across multi-agent systems, or the specific attack surface created when poisoned memory is deserialized and interpreted as legitimate system context. The shared cache propagation dimension is entirely absent from NIST coverage.
**Score: 2**

### RTE_4_3
NIST AI 600-1 §2.9 indirect prompt injection aligns with this attack since AutoGen GroupChat message history functions as an external data channel through which injected instructions propagate to all agents processing the shared history. The self-replicating worm-like propagation property is directly an instance of the indirect prompt injection class NIST recognizes. However, NIST does not address AutoGen or any other specific framework, nor does it provide architectural controls for shared message history integrity or isolation. The framework-specific nature of AutoGen GroupChat exploitation falls outside NIST's scope, limiting coverage to the general prompt injection recognition.
**Score: 2**

---

## RTE_5 - UI & Disclosure Vulnerabilities

### RTE_5_1
NIST AI 600-1 does not address progressive disclosure UI architectures or the asymmetric attack surface created when human users review summary layers while agents consume technical layers. The framework has no controls for disclosure layer integrity verification or detection of poisoned technical disclosures designed to influence downstream agents rather than human reviewers. GOVERN 6.1/6.2 supply chain controls focus on component vetting rather than runtime disclosure integrity. This attack exploits a UI design pattern and human-agent review asymmetry that falls outside NIST's governance-level scope entirely.
**Score: 1**

### RTE_5_2
NIST AI 600-1 has no controls addressing dynamic agent role assignment or dashboard role integrity verification. GOVERN 6.1/6.2 supply chain vetting does not extend to runtime role manipulation via task metadata injection. The framework does not recognize agent role confusion as a security concern, nor does it provide controls for verifying that agents displayed under trusted role labels in dashboards are the authorized agents for those roles. This attack is a runtime identity and authorization failure that NIST's governance-level controls cannot address.
**Score: 1**

---

## RTE_6 - Tool & Command Injection

### RTE_6_1
NIST AI 600-1 §2.9 prompt injection directly covers the mechanism by which attackers embed malicious instructions into content that AI systems process, which maps to inline suggestion poisoning where injected instructions are incorporated into user-accepted suggestions. MEASURE 2.7 security evaluation and red-teaming could identify prompt injection via suggestion mechanisms pre-deployment. However, NIST does not address the specific attack surface of AI-assisted inline suggestion systems, the user acceptance step that launders malicious instructions as user-intended input, or the downstream agent processing of incorporated instructions. Coverage is moderate because the underlying prompt injection mechanism is recognized but the multi-agent propagation chain is not.
**Score: 2**

### RTE_6_2
NIST AI 600-1 §2.9 indirect prompt injection is relevant since the attack involves poisoning project files that context-processing agents consume, causing those agents to surface dangerous commands. The framework's recognition that AI systems can be manipulated via external data they process applies to context poisoning of suggestion agents. However, NIST does not address command palette systems, proactive suggestion mechanisms, or the urgency-exploitation dimension of contextual suggestions. MEASURE 2.7 red-teaming could surface this pre-deployment, but the framework provides no architectural controls for suggestion agent context integrity validation.
**Score: 2**

---

## RTE_7 - Framework-Specific Attacks

### RTE_7_1
NIST AI 600-1 has no coverage of framework-specific delegation models or the distinct trust assumptions embedded in LangChain, LangGraph, AutoGen, CrewAI, or Semantic Kernel. GOVERN 6.1/6.2 supply chain controls could theoretically flag framework selection as a third-party risk concern, but do not address the exploitation of framework-specific privilege escalation pathways. The framework's §2.9 controls do not engage with transitive privilege escalation through trust chain exploitation in agentic delegation architectures. Framework-specific attacks are explicitly outside NIST AI 600-1's governance-level scope.
**Score: 1**

### RTE_7_2
NIST AI 600-1 §2.9 indirect prompt injection provides the only conceptual hook for framework-specific communication protocol exploits, since malware propagating through inter-agent communication channels is a form of indirect prompt injection at the protocol level. However, NIST does not address framework-specific communication architectures, protocol-level security properties, or the worm-like propagation characteristics that emerge from framework-specific message passing implementations. GOVERN 6.1/6.2 supply chain vetting does not extend to runtime communication security between agents. Coverage is minimal because only the generic injection class is recognized, not the framework-specific propagation surface.
**Score: 1**

### RTE_7_3
NIST AI 600-1 has no coverage of LangGraph's conditional edge architecture or state-based message routing as an attack surface. The framework does not address graph-based agent orchestration, conditional routing logic, or state field manipulation as an instruction propagation channel. §2.9 indirect prompt injection is nominally relevant since crafted state functions as malicious external data influencing agent behavior, but NIST does not engage with state machine routing as a distinct attack vector. This is a framework-specific architectural exploit entirely outside NIST AI 600-1's governance scope.
**Score: 1**
# NIST AI 600-1 GenAI Profile: Coverage Analysis for RTE_8 through RTE_13

---

## RTE_8 - AutoGen-Specific Attacks

### RTE_8_1
NIST AI 600-1 does not address AutoGen as a framework, and its controls were not designed around conversation-based multi-agent architectures where trust is determined by natural language message content. The Information Security category (§2.9) recognizes prompt injection as a named risk, which has superficial relevance since social engineering via crafted conversational messages is a form of adversarial input manipulation. However, the profile's treatment of prompt injection focuses on user-to-model injection vectors, not agent-to-agent dialogue channels where a compromised peer persuades another through contextually plausible conversation. NIST provides no controls for inter-agent trust establishment, message authenticity verification, or dialogue-based manipulation detection in conversational multi-agent systems. The framework's governance and pre-deployment testing guidance would not surface this threat class, which requires runtime monitoring of agent dialogue semantics.
**Score: 1**

### RTE_8_2
AutoGen's reputation-based agent selection and the exploitation of high-reputation agents via low-reputation intermediaries represents a multi-agent identity and trust architecture problem with no coverage in NIST AI 600-1. The profile does not model agent reputation systems, trust score computation, or the propagation of influence through reputation-weighted multi-agent networks. §2.9's prompt injection framing is nominally applicable in that compromised agents could inject malicious content through conversational channels, but the specific attack mechanism—leveraging low-reputation agents to influence high-reputation ones and manufacture false consensus—requires runtime trust validation controls the framework does not provide. GOVERN 6.1/6.2 addresses third-party component vetting but does not extend to dynamic reputation management within a running multi-agent system. This threat is effectively out of scope for a governance-oriented GenAI risk profile.
**Score: 1**

---

## RTE_9 - CrewAI-Specific Attacks

### RTE_9_1
NIST AI 600-1 does not address CrewAI's hierarchical manager-worker trust architecture, and the framework contains no controls specific to task delegation chains or transitive trust exploitation across organizational roles within a multi-agent system. The Information Security category (§2.9) recognizes prompt injection and data poisoning, and task output poisoning—where a compromised worker inserts malicious content into outputs that managers consume—can be framed as a data poisoning vector flowing through inter-agent communication. However, this framing captures only a narrow slice of the threat: the profile does not model manager-worker trust relationships, delegation authorization scopes, or the amplification effect of hierarchical trust in agentic pipelines. Pre-deployment testing guidance could in principle include adversarial task delegation scenarios, but the framework provides no structural guidance for CrewAI-specific trust boundary enforcement. Coverage is minimal and indirect.
**Score: 1**

### RTE_9_2
Few-shot demonstration poisoning targeting CrewAI manager agents is a training-adjacent attack that partially overlaps with NIST AI 600-1's data poisoning coverage under Information Security (§2.9), since malicious demonstrations embedded in manager context function as poisoned in-context training data that shapes subsequent task decomposition behavior. The Value Chain and Component Integration category (§2.12) is tangentially relevant if the few-shot demonstrations originate from external or third-party sources that could be compromised upstream. GOVERN 6.1/6.2 establishes governance obligations for data provenance review, which could encompass vetting the sources of demonstration examples used to configure manager agents. However, NIST provides no guidance on in-context few-shot attack surfaces, decomposition pattern injection, or the specific risk that learned decomposition patterns bypass human oversight by creating autonomous subtask chains. The coverage is weak and governance-level only.
**Score: 1**

### RTE_9_3
Demonstration bias enabling semantic subtask reinterpretation is a behavioral manipulation attack on multi-agent task decomposition pipelines where biased in-context examples cause agents to systematically misinterpret task semantics before passing reinterpreted outputs downstream. NIST AI 600-1's Confabulation risk category (§2.2) has marginal relevance, as semantically shifted subtask interpretations could be characterized as a form of systematic output distortion. The Information Security category (§2.9) is nominally applicable if the biased demonstrations are treated as adversarially injected content. However, the profile does not address cross-agent semantic drift, demonstration-induced interpretation bias, or the propagation of reinterpreted task semantics through downstream agent chains. These are behavioral and architectural concerns in multi-agent systems that fall outside the governance and policy scope of NIST AI 600-1. The framework offers no actionable controls for this threat.
**Score: 1**

---

## RTE_10 - Semantic Kernel-Specific Attacks

### RTE_10_1
Plugin registry takeover and plugin substitution attacks in Semantic Kernel represent a supply chain integrity problem where the plugin registry itself—or the namespace it exposes—is the compromised component. NIST AI 600-1's Value Chain and Component Integration category (§2.12) is directly relevant here: it explicitly flags risks arising from third-party components integrated into AI systems, and GOVERN 6.1/6.2 establishes organizational obligations for vetting and monitoring the provenance of such components. A plugin registry populated by attacker-controlled entries named "SecurityValidator" or "ComplianceChecker" is precisely the kind of third-party supply chain compromise the profile's value chain governance is designed to prompt organizations to address. Pre-deployment testing could in principle include adversarial plugin registration scenarios. The limitation is that NIST does not define technical controls for plugin namespace integrity, registry authentication, or runtime function-routing verification—translating the governance intent into actual defenses requires implementation work beyond what the profile specifies.
**Score: 2**

### RTE_10_2
Semantic Kernel function impersonation through schema duplication exploits the probabilistic nature of LLM-based function routing when multiple plugins share similar names and descriptions, causing the model to non-deterministically select between legitimate and malicious variants. NIST AI 600-1's Value Chain and Component Integration category (§2.12) applies insofar as the malicious plugin is a counterfeit third-party component introduced into the AI system's operational environment, and GOVERN 6.1/6.2 establishes supply chain accountability that could require verifying plugin provenance before registration. The profile's pre-deployment testing guidance supports evaluation of plugin selection behavior under adversarial conditions. However, the core vulnerability—LLM probabilistic routing being manipulated by schema similarity—is a runtime, model-behavior problem that governance and vetting processes cannot fully address, since the attack exploits the LLM's own function selection logic rather than a policy gap. Coverage is indirect and limited to the supply chain governance dimension.
**Score: 2**

---

## RTE_11 - Multimodal Attacks

### RTE_11_1
Multimodal content attribution spoofing—where attackers present malicious outputs as legitimate synthesis from trusted vision agents—is a form of content provenance manipulation that NIST AI 600-1 addresses through its content provenance and watermarking guidance under Information Integrity (§2.8) and the broader pre-deployment testing framework. The profile explicitly encourages organizations to implement provenance tracking for AI-generated content, which is directly relevant to an attack that weaponizes the absence of verifiable attribution in multi-agent collaborative outputs. The Human-AI Configuration risk category (§2.7) further applies, as users interpreting collaborative analysis displays are subject to automation bias amplified by false attribution cues. However, NIST does not specify technical controls for cross-agent output attribution verification, tamper-evident agent contribution logging, or display-layer provenance enforcement in multi-agent collaboration interfaces. Governance-level guidance on provenance tracking provides a meaningful but incomplete foundation.
**Score: 2**

### RTE_11_2
Self-replicating image injection through inline multimodal suggestions is a form of indirect prompt injection carrying a cross-agent propagation payload, placing it squarely within NIST AI 600-1's Information Security category (§2.9), which explicitly names indirect prompt injection as a recognized risk. The worm propagation mechanism—poisoned images containing visual triggers spreading through shared document processing—can also be framed as a data poisoning vector within multi-agent RAG or document collaboration pipelines, also covered by §2.9. Pre-deployment testing and red-teaming guidance could support evaluation of multimodal agent pipelines for injection propagation susceptibility. The limitation is that NIST does not define technical controls for image-embedded instruction detection, cross-agent content isolation, or shared document quarantine policies; the profile's recognition of indirect prompt injection provides a valid conceptual hook without reaching the multimodal propagation mechanics.
**Score: 2**

### RTE_11_3
Cross-agent modality contradiction attacks that exploit fusion logic biases to manufacture false consensus represent a threat at the intersection of multi-agent orchestration architecture and multimodal input processing, for which NIST AI 600-1 has no direct coverage. The Confabulation risk category (§2.2) has marginal relevance since accepting contradictory multimodal evidence produces erroneous outputs, and Human-AI Configuration (§2.7) touches on the risk that humans over-rely on apparent consensus from AI systems. However, NIST does not address modality fusion logic, cross-modal contradiction detection, or the adversarial exploitation of orchestrator agents' modality preference biases. The profile provides no guidance on multi-agent consensus validation, modality weighting architectures, or the detection of coordinated evidence poisoning across input channels. This threat requires technical controls at the fusion and orchestration layer that are entirely outside the framework's scope.
**Score: 1**

### RTE_11_4
Multimodal RAG worm propagation through retrieval cycles directly engages two of NIST AI 600-1's most applicable risk categories: Information Security (§2.9), which covers both indirect prompt injection—the embedded instruction mechanism—and data poisoning—the contamination of the retrieval corpus; and Value Chain and Component Integration (§2.12), which addresses integrity of external data sources and third-party components that agents retrieve and act upon. The worm's bypass of text-based defenses by embedding instructions in images maps precisely onto the indirect injection through multimodal content that §2.9 recognizes. GOVERN 6.1/6.2 establishes supply chain accountability applicable to the retrieval sources from which poisoned images originate. Pre-deployment testing guidance supports red-teaming of RAG pipelines for multimodal injection. The gap is that NIST does not specify runtime controls for image content scanning, cross-agent retrieval quarantine, or cycle detection in iterative RAG pipelines.
**Score: 2**

---

## RTE_12 - Streaming & Continuous Processing

### RTE_12_1
Retry failure messaging as inter-agent social engineering exploits the semantic content of error communications between agents, where failure messages are crafted to carry instructions that downstream agents interpret as legitimate guidance from trusted upstream peers. NIST AI 600-1's Information Security category (§2.9) has weak applicability, as the malicious content embedded in retry messages functions as a form of indirect prompt injection traveling through a trusted inter-agent communication channel. The profile recognizes indirect prompt injection as a risk but does not address the specific vector of error message channels or the trust relationships that make retry communications credible to receiving agents. Human-AI Configuration (§2.7) has marginal relevance if humans are involved in interpreting retry escalations. NIST provides no controls for sanitizing inter-agent error communications, validating message semantic content against expected retry formats, or isolating instruction-carrying payloads in structured failure messages. Coverage is minimal.
**Score: 1**

### RTE_12_2
Fallback routing announcements as malware propagation channels exploit the trusted status of system state communications in multi-agent streaming architectures, where agents embed instructions in fallback notifications that downstream agents consume as legitimate operational guidance. NIST AI 600-1 does not address streaming architectures, real-time message routing, or the security of fallback and failover communication channels between agents. The Information Security category (§2.9) recognizes prompt injection conceptually, which provides the thinnest possible hook—embedded instructions in fallback announcements are a form of injection through system-internal channels. However, the profile was not designed around streaming multi-agent systems, provides no controls for fallback message integrity, and does not consider the amplification effect of injected instructions that propagate simultaneously to multiple downstream agents through broadcast fallback notifications. The coverage is effectively out of scope.
**Score: 1**

### RTE_12_3
Circuit breaker state change announcements as coordinated instruction propagation vectors exploit multi-agent systems' dependency on infrastructure state transitions, where attackers craft state change notifications to carry instructions that multiple agents simultaneously execute as they respond to the announced state. NIST AI 600-1 has no coverage of circuit breaker patterns, distributed state machine coordination, or the security of infrastructure state announcement channels in multi-agent systems. The profile's Information Security category (§2.9) provides only the most abstract framing—instructions carried in state announcements could be characterized as indirect injection through system channels—but this framing offers no actionable guidance. Real-time streaming infrastructure security and coordinated multi-agent instruction propagation through state management systems are entirely outside the governance-oriented scope of the NIST GenAI Profile. There is no meaningful mapping.
**Score: 1**

---

## RTE_13 - Evaluation & Benchmarking

### RTE_13_1
Self-replicating evaluation metric definitions propagating through multi-agent evaluation pipelines via inter-agent communication represent a data poisoning vector within a specialized multi-agent workflow, partially engaging NIST AI 600-1's Information Security category (§2.9). If metric definitions are treated as data flowing through an evaluation pipeline, their adversarial manipulation maps onto the data poisoning risk class the profile recognizes. However, NIST does not address evaluation agent pipelines, metric definition integrity, or the replication mechanics of malicious content spreading through inter-agent evaluation communication. The profile's pre-deployment testing guidance (MEASURE 2.7) focuses on testing the AI system being evaluated, not securing the multi-agent evaluation infrastructure itself. The governance-level intent of §2.9 provides a weak conceptual hook, but the specific attack surface—metric definition channels as malware propagation vectors—is outside the framework's intended scope.
**Score: 1**

### RTE_13_2
Transitive trust exploitation in multi-agent evaluation chains directly maps onto NIST AI 600-1's MEASURE 2.7, which calls for pre-deployment security evaluation including red-teaming. The attack exploits the assumption that each agent in a sequential evaluation chain can trust its predecessor's output without re-validation, which is precisely the kind of evaluation chain integrity failure that structured adversarial testing is designed to expose. The Value Chain and Component Integration category (§2.12) further applies, as evaluation agents functioning as components in a deployment decision pipeline are subject to supply chain integrity concerns when any upstream agent is compromised. GOVERN 6.1/6.2 establishes accountability for third-party evaluation components. The limitation is that NIST's red-teaming guidance focuses on pre-deployment evaluation of the AI model, not on continuous adversarial testing of the multi-agent evaluation infrastructure itself; transitive trust failures in live evaluation chains require operational security controls the profile does not specify.
**Score: 2**

### RTE_13_3
Evaluation framework dependency manipulation—substituting malicious versions of MLflow, numpy, pytest, or other evaluation libraries through compromised package managers or mutable dependency specifications—is a software supply chain attack that NIST AI 600-1's Value Chain and Component Integration category (§2.12) directly addresses. The profile explicitly flags risks from third-party components integrated into AI systems, and GOVERN 6.1/6.2 establishes organizational obligations to vet supply chain components and maintain accountability for their integrity. This is one of the stronger NIST mappings within the RTE_13 cluster: poisoned evaluation library versions are exactly the kind of third-party component substitution that supply chain governance frameworks target. Pre-deployment testing guidance supports evaluation of dependency integrity. The gap is that NIST does not prescribe technical controls such as dependency pinning, hash verification, private package mirrors, or software bill of materials enforcement—translating governance intent into concrete dependency security requires implementation work beyond the profile's scope.
**Score: 2**

### RTE_13_4
Evaluation report format injection through custom report rendering—where attackers inject HTML or JavaScript into evaluation reports via customizable rendering functions or untrusted templates—is a web application security problem (XSS/template injection) with no coverage in NIST AI 600-1. The profile's Information Security category (§2.9) addresses prompt injection at the AI model input level, not HTML/JavaScript injection into report rendering pipelines. Human-AI Configuration (§2.7) has marginal relevance insofar as manipulated report displays can distort human interpretation and induce automation bias toward deploying a compromised model, but the profile does not address report rendering security, template sandboxing, or output escaping in evaluation dashboards. This is a standard application security vulnerability in evaluation infrastructure that falls outside the scope of a governance-oriented GenAI risk profile.
**Score: 1**

### RTE_13_5
Cross-agent metric comparison showing false consensus exploits the human tendency to interpret coordinated high metrics across multiple evaluation agents as evidence of thorough validation, when in fact all agents have been poisoned to report inflated results. NIST AI 600-1's Human-AI Configuration category (§2.7) is relevant here: automation bias and over-reliance on AI-generated outputs—including evaluation outputs—is a named risk, and GOVERN 3.2 encourages policies for human oversight that do not rely exclusively on automated evaluation consensus. The Confabulation category (§2.2) has marginal relevance if coordinated false metric reporting is characterized as a form of systematic AI output distortion. MEASURE 2.7 supports adversarial testing of evaluation systems. However, NIST does not define controls for detecting coordinated metric poisoning across agent fleets, validating evaluation independence, or implementing statistical anomaly detection on benchmark result distributions. The governance framing is weakly applicable but does not reach the attack mechanics.
**Score: 1**

### RTE_13_6
Benchmark leaderboard manipulation as agent reputation spoofing engages NIST AI 600-1's Value Chain and Component Integration category (§2.12) and GOVERN 6.1/6.2 at moderate strength: leaderboard-based agent selection is a form of supply chain selection mechanism where the integrity of benchmark implementations determines which agents enter production, and the profile's supply chain governance obligations can encompass vetting the integrity of benchmarking systems used to make deployment decisions. If downstream agents select "best performing" peers based on leaderboard results, a poisoned benchmark is a compromised selection component within the AI value chain. MEASURE 2.7 red-teaming guidance is also applicable if adversarial benchmark manipulation is included in pre-deployment evaluation scope. The limitation is that NIST does not specify controls for benchmark implementation integrity, leaderboard audit trails, or detection of agents gaming evaluation metrics—the governance intent must be operationalized through implementation-level controls not defined by the profile.
**Score: 2**

### RTE_13_7
Benchmark-based agent selection creating trust exploitation vectors is a second-order consequence of benchmark gaming: once malicious agents pass selection criteria through falsified results, they occupy trusted production roles where peer agents extend elevated trust to them. NIST AI 600-1's Value Chain and Component Integration category (§2.12) applies because agent selection based on benchmark performance is a procurement and vetting process that the profile's supply chain governance covers, and GOVERN 6.1/6.2 establishes accountability for verifying that selected components—including agents—are what they purport to be. The attack's dependency on falsified benchmark results as the entry mechanism connects to the same governance gap addressed in RTE_13_6. However, NIST provides no controls for post-selection behavioral monitoring, runtime trust revocation, or detection of agents that perform differently in production than in evaluation. The governance controls address the selection process integrity but not the downstream trust exploitation that occurs after a malicious agent is admitted.
**Score: 2**
# NIST AI 600-1 RTE Threat Analysis - Cache 3

## RTE_14 - Web & LLM Evaluation Attacks

### RTE_14_1
NIST AI 600-1 §2.9 explicitly recognizes indirect prompt injection as a named threat vector where malicious content embedded in data consumed by AI systems redirects their behavior, which provides moderate conceptual alignment with multi-hop QA sub-question poisoning since injected instructions in early sub-question answers constitute indirect prompt injection propagating through a structured retrieval pipeline. MEASURE 2.7 security evaluation and red-teaming mandates could in principle exercise cascading injection scenarios across decomposed query chains pre-deployment. However, NIST does not address multi-hop query decomposition architectures, the specific attack surface created when intermediate answers become trusted input for downstream sub-agents, or the amplification effect where a single injected answer reaches all subsequent agents in the chain. The framework's prompt injection coverage is a genuine but governance-level acknowledgment that does not extend to architectural controls for intermediate answer integrity validation in decomposed query systems.
**Score: 2**

---

## RTE_15 - Model Tuning & Configuration Attacks

### RTE_15_1
NIST AI 600-1 does not address model size selection as a security-relevant configuration parameter or the trust asymmetry that emerges when large-model agents assume small-model agents lack attack sophistication. The framework's GOVERN 6.1/6.2 supply chain controls focus on third-party component vetting at procurement rather than runtime inter-agent capability trust assumptions. §2.9 Information Security does not engage with model capability profiling as a social engineering vector, and no NIST control addresses the multi-agent trust assumption that correlates model size with attack risk. Model capability management and the social engineering exploitation of capability-based trust hierarchies fall outside NIST AI 600-1's governance scope entirely.
**Score: 1**

### RTE_15_2
NIST AI 600-1 §2.9 indirect prompt injection provides meaningful conceptual alignment with prompt cache poisoning since a poisoned shared cache functions as the external data channel through which injected instructions reach all agents consuming that cache, directly instantiating the indirect prompt injection threat class NIST recognizes. MEASURE 2.7 security evaluation could identify cache poisoning vulnerabilities pre-deployment if test suites are designed to probe shared cache integrity. However, NIST does not address prompt caching architectures, multi-agent shared cache configurations, the self-replicating persistence property that emerges when poisoned instructions survive across request cycles, or the lateral propagation mechanism that spreads malware to all agents sharing a poisoned cache. Coverage is moderate because the underlying injection mechanism is recognized but the cache-specific persistence and multi-agent propagation dimensions are not.
**Score: 2**

### RTE_15_3
NIST AI 600-1 has no controls addressing iteration budget configuration as a security parameter or the exploitable windows created by budget mismatches between agents with varying reasoning depths. The framework does not engage with inference iteration counts, reasoning depth budgets, or the failure mode where budget-constrained agents cannot complete the reasoning steps required to detect injected instructions. None of NIST's 12 risk categories or four primary considerations address runtime inference compute allocation as an attack surface. Iteration budget configuration is an operational infrastructure parameter that falls entirely outside the governance-level scope of NIST AI 600-1.
**Score: 1**

### RTE_15_4
NIST AI 600-1 does not address adaptive routing models, dynamic model size selection based on query complexity, or the attack surface created when routing logic can be manipulated by crafting queries that appear complex. GOVERN 6.1/6.2 supply chain vetting could in principle cover router model components as third-party elements, but does not extend to runtime routing decision exploitation. §2.9 Information Security does not identify routing configuration as a security-relevant attack surface. Adaptive routing exploitation is an operational infrastructure concern involving model selection algorithms and query classification that falls outside NIST AI 600-1's governance-level scope.
**Score: 1**

---

## RTE_16 - Reasoning Trace & Chain-of-Thought Attacks

### RTE_16_1
NIST AI 600-1 §2.9 prompt injection directly covers the mechanism by which adversarial instructions are embedded in content that AI systems process and act upon, providing moderate alignment with CoT-wrapped social engineering since the attack embeds manipulative instructions within reasoning traces that downstream agents consume as trusted analytical context. MEASURE 2.7 red-teaming and security evaluation mandates could surface CoT-layer social engineering pre-deployment if adversarial testers probe reasoning trace integrity and the persuasive framing of embedded manipulation. However, NIST does not address chain-of-thought architectures specifically, the distinct attack surface created when reasoning traces are treated as authoritative analytical output rather than agent-generated text, or the social engineering dimension of embedding manipulation as plausible-sounding analytical justification. Coverage is moderate because the prompt injection class is recognized and red-teaming is mandated, but the CoT-specific social engineering vector is not explicitly scoped.
**Score: 2**

### RTE_16_2
NIST AI 600-1 §2.9 indirect prompt injection is directly relevant since reasoning trace worm propagation involves injecting malicious instructions into data (stored reasoning traces) that agents naturally retrieve and incorporate, causing them to execute and propagate injected content—a textbook indirect prompt injection instantiation. The framework's recognition that malicious content embedded in external data sources can redirect AI behavior maps to the mechanism by which stored CoT traces carrying embedded malware spread across agent networks. MEASURE 2.7 red-teaming could identify reasoning trace contamination vectors pre-deployment. However, NIST does not address shared reasoning trace storage architectures, the worm-like self-replication property that emerges when injected instructions instruct agents to embed the same instructions in their own outputs, or the network-wide propagation dynamics specific to multi-agent shared trace repositories.
**Score: 2**

---

## RTE_17 - Tree of Thought & Sampling Attacks

### RTE_17_1
NIST AI 600-1 has no controls addressing quality score systems in Tree of Thought architectures, the integrity of multi-path evaluation scoring, or the trust exploitation that emerges when compromised agents maintain artificially high quality scores while injecting malicious content. MEASURE 2.9 model validation touches on output calibration conceptually but does not address adversarial quality score maintenance as a trust manipulation strategy. §2.9 Information Security does not identify scoring system integrity in multi-path reasoning as a security concern. Tree of Thought sampling and quality evaluation mechanisms are reasoning architecture internals that fall outside NIST AI 600-1's governance scope entirely.
**Score: 1**

### RTE_17_2
NIST AI 600-1 does not address Self-Consistency sampling architectures, the security implications of multi-path reasoning diversity, or the exploitation of the "errors are path-specific" assumption that Self-Consistency relies upon. The framework has no controls for detecting when diverse attack demonstrations across multiple reasoning paths are used to construct apparent legitimacy for malicious instructions. §2.9 does not engage with sampling-based reasoning processes, and MEASURE 2.7 red-teaming does not specifically address adversarial exploitation of diversity-based proof-of-exploitation patterns in multi-agent systems. Sampling diversity attacks against Self-Consistency reasoning are planning algorithm internals outside NIST's scope.
**Score: 1**

### RTE_17_3
NIST AI 600-1 has no controls addressing majority voting mechanisms in multi-path reasoning systems or the consensus-building attack pattern where multiple reasoning paths converge on malicious conclusions to create apparent agreement. The framework does not engage with voting-based reasoning architectures, consensus thresholds, or the manipulation of statistical agreement signals as a trust exploitation vector. MEASURE 2.7 red-teaming could theoretically probe for consensus manipulation pre-deployment, but the framework does not identify this as a target for adversarial testing. Majority voting exploitation in multi-agent Tree of Thought systems is a reasoning algorithm concern outside NIST AI 600-1's governance scope.
**Score: 1**

### RTE_17_4
NIST AI 600-1 does not address path quality inflation, credential spoofing through inflated scoring, or the trust propagation dynamics that emerge when high-quality score signals cause downstream agents to accept injected instructions with elevated confidence. MEASURE 2.9 model validation does not cover adversarial quality score inflation in multi-path reasoning systems. §2.9 Information Security does not identify quality score integrity as a security-relevant property in multi-agent coordination. Path quality inflation as a credential spoofing mechanism is a reasoning architecture internal that falls entirely outside the governance-level scope of NIST AI 600-1.
**Score: 1**

---

## RTE_18 - Hierarchical Task Network (HTN) & Planning Attacks

### RTE_18_1
NIST AI 600-1 does not address hierarchical task decomposition architectures, authority levels within HTN planning systems, or the privilege confusion attacks that emerge when lower-authority agents falsely claim higher-status authorization. GOVERN 6.1/6.2 supply chain controls operate at procurement-time vetting and do not extend to runtime authority verification within planning hierarchies. §2.9 does not engage with multi-agent authority structures or hierarchical decomposition as an attack surface. HTN authority confusion and privilege escalation through hierarchical status spoofing are multi-agent coordination problems at the planning infrastructure level that fall outside NIST AI 600-1's scope.
**Score: 1**

### RTE_18_2
NIST AI 600-1 §2.12 Value Chain and Component Integration provides partial relevance since shared method library poisoning is a supply chain integrity problem where attackers inject malicious components that propagate to all agents inheriting from the poisoned library. GOVERN 6.1/6.2 directs organizations to vet third-party components and establish supply chain accountability, which could in principle apply to shared HTN method library providers. However, NIST's supply chain controls focus on pre-procurement vetting and do not address runtime library integrity, the impersonation of trusted method authors, or the inheritance propagation mechanism that spreads poisoned methods to production agents. The framework offers governance-level supply chain framing but no controls for runtime method library authentication or integrity verification.
**Score: 1**

### RTE_18_3
NIST AI 600-1 has no controls addressing delegation chain integrity, upstream authority verification in HTN decomposition, or the authority diffusion attack pattern where each agent in a chain trusts claimed authorization from its upstream without independent verification. The framework does not engage with multi-agent delegation architectures, task decomposition chains, or the propagation dynamics that allow malware to traverse planning hierarchies by exploiting deferred verification. §2.9 indirect prompt injection is nominally relevant since malicious instructions propagate through data consumed by downstream agents, but NIST does not address the HTN-specific authority chain structure that enables this propagation. Multi-agent delegation chain exploitation is a planning coordination concern outside NIST's governance scope.
**Score: 1**

---

## RTE_19 - Monte Carlo Tree Search (MCTS) Attacks

### RTE_19_1
NIST AI 600-1 has no controls addressing Monte Carlo Tree Search algorithms, stochastic policy exploitation, or the probabilistic emergence of malicious action sequences under specific MCTS rollout conditions. The framework does not engage with planning algorithms, non-deterministic simulation processes, or the attack surface created when adversarially crafted scenarios probabilistically trigger malicious MCTS outputs. §2.9 Information Security does not identify MCTS rollout manipulation as a security concern. Non-deterministic MCTS rollout exploitation is a planning algorithm internal that falls entirely outside the governance-level scope of NIST AI 600-1.
**Score: 1**

### RTE_19_2
NIST AI 600-1 §2.9 data poisoning provides a weak conceptual hook since MCTS value network poisoning is a form of data poisoning that corrupts the reward signals guiding multi-agent planning decisions. MEASURE 2.7 security evaluation could in principle include pre-deployment testing for value network integrity. However, NIST's data poisoning coverage focuses on training-time data integrity rather than runtime backpropagation poisoning through shared simulation networks. The framework does not address shared MCTS value network architectures, the mechanism by which a single malicious simulation propagates false reward signals across all agents sharing the network, or the runtime operational dimension of backpropagation poisoning. Coverage is minimal because the data poisoning class is recognized at the conceptual level but MCTS-specific propagation mechanics are not within scope.
**Score: 1**

### RTE_19_3
NIST AI 600-1 does not address hierarchical MCTS architectures, delegation relationships between MCTS supervisors and expansion processes, or the privilege escalation pattern where compromised MCTS generates dangerous action sequences that supervisors accept as legitimate decomposition. The framework has no controls for validating that MCTS expansion outputs remain within supervisor-authorized action spaces. §2.9 does not engage with planning algorithm delegation as an attack surface. Hierarchical MCTS delegation and privilege escalation through compromised planning expansion are multi-agent coordination and planning algorithm concerns outside NIST AI 600-1's scope.
**Score: 1**

---

## RTE_20 - Multi-Agent Planning Attacks

### RTE_20_1
NIST AI 600-1 §2.9 indirect prompt injection provides partial relevance since planning phase reasoning falsification involves injecting flawed logic into the planning context that downstream execution agents treat as authoritative instruction, instantiating the indirect prompt injection class at the plan-level. MEASURE 2.7 red-teaming could in principle probe plan-and-execute architectures for planning phase manipulation pre-deployment. However, NIST does not address plan-and-execute architectural patterns, the security properties required for planning phase integrity, or the specific failure mode where falsified planning logic like unauthorized permission grants propagates unchallenged to execution agents. The framework offers governance-level framing through prompt injection recognition but no architectural controls for planning phase validation or separation of plan generation from execution authorization.
**Score: 2**

### RTE_20_2
NIST AI 600-1 does not address reasoning consistency across hierarchical multi-agent boundaries, contradiction injection at management levels, or the propagation of conflicting instructions through hierarchies where workers cannot detect manager-level contradictions. §2.9 Information Security recognizes prompt injection broadly but does not engage with the specific failure mode of cross-boundary reasoning inconsistency where injected contradictions are structurally invisible to the agents they affect. GOVERN 6.1/6.2 supply chain controls do not extend to runtime consistency verification within multi-agent hierarchies. Hierarchical reasoning consistency and cross-boundary contradiction injection are multi-agent coordination architecture concerns outside NIST AI 600-1's governance scope.
**Score: 1**

### RTE_20_3
NIST AI 600-1 §2.9 indirect prompt injection directly covers the mechanism of delegation context reasoning injection since manager reasoning that becomes worker context is an external data channel through which injected instructions propagate—workers treating poisoned manager context as legitimate instruction is a textbook indirect prompt injection instantiation. MEASURE 2.7 red-teaming could surface delegation context injection vulnerabilities pre-deployment if test scenarios probe manager-worker context boundaries. However, NIST does not address plan-and-execute context passing architectures, relevancy embedding as an injection channel, or the specific attack pattern of embedding instructions within plausible operational directives that workers cannot distinguish from legitimate context. Coverage is moderate because the indirect prompt injection mechanism is genuinely applicable but the hierarchical delegation context-passing architecture is not addressed.
**Score: 2**

### RTE_20_4
NIST AI 600-1 §2.9 indirect prompt injection is partially relevant since compromised workers injecting contaminated subtasks into supervisor replanning involves malicious content embedded in worker outputs that supervisors consume and incorporate into planning decisions, functioning as an upward-direction indirect prompt injection. MEASURE 2.7 red-teaming could in principle test for worker-to-supervisor contamination pathways pre-deployment. However, NIST does not address hierarchical trust poisoning in plan-and-execute architectures, upward propagation from workers to supervisors through subtask injection, or semantic gap exploitation where contaminated subtasks appear operationally coherent to supervisors making replanning decisions. The framework's prompt injection coverage applies in the abstract but does not reach the multi-agent hierarchical trust dynamics specific to plan-and-execute architectures.
**Score: 2**
# NIST AI 600-1 RTE Threat Analysis - Cache 4

## RTE_21 - Memory & Knowledge Attacks

### RTE_21_1
NIST AI 600-1 §2.9 recognizes data poisoning as a distinct attack class where adversaries corrupt training or retrieval data to manipulate AI system behavior, providing a moderate conceptual hook for episodic memory poisoning since injected episodes function as poisoned retrieval data. The framework's recognition that external data consumed by AI systems can be weaponized maps to the mechanism by which falsified successful-exploit episodes are stored and later retrieved by peer agents as behavioral precedents. However, NIST does not address episodic memory architectures, multi-agent shared memory systems, or the specific trust amplification dynamic where fabricated experience records create false legitimacy. The framework's data poisoning controls are framed around training datasets and pre-deployment data pipelines, not runtime episodic memory shared across an agent fleet.
**Score: 2**

### RTE_21_2
NIST AI 600-1 has no coverage of reinforcement learning trajectory data, multi-agent policy convergence, or the specific attack surface of poisoned experience trajectories causing synchronized malicious behavior across an agent fleet. The framework's §2.9 data poisoning hook is extremely thin here since trajectory-based policy convergence involves RL-specific learning dynamics—reward signals, policy gradients, and joint optimization—that NIST AI 600-1 does not address anywhere. GOVERN 6.1/6.2 supply chain controls do not extend to runtime learning data integrity. MARL coordination dynamics are explicitly outside NIST's governance-level scope, leaving this threat without meaningful coverage.
**Score: 1**

### RTE_21_3
NIST AI 600-1 §2.9 data poisoning provides a thin conceptual alignment since memory abstraction systems consuming poisoned episodes represent a form of poisoned data pipeline where corrupted inputs propagate into derived knowledge structures. The framework's MEASURE 2.7 red-teaming could theoretically surface anomalous abstracted templates pre-deployment if the poisoning occurred during testing. However, NIST does not address agent spawning mechanisms, memory abstraction processes, semantic knowledge inheritance, or the specific threat of malware templates propagating into newly instantiated agents' baseline reasoning. The multi-generational inheritance dimension of this attack is entirely outside NIST's scope.
**Score: 1**

### RTE_21_4
NIST AI 600-1 §2.9 data poisoning and §2.12 value chain integrity together provide moderate coverage of shared vector database embedding poisoning since a centralized embedding store is a third-party data component subject to supply chain integrity concerns, and poisoning it constitutes data poisoning affecting all downstream consumers. GOVERN 6.1/6.2 supply chain vetting is relevant because the shared vector database represents an external dependency whose integrity must be verified. MEASURE 2.7 security evaluation could include testing for embedding integrity. However, NIST does not address the specific unanimity property of shared database poisoning where all agents simultaneously retrieve compromised results, nor does it provide technical controls for embedding-level integrity verification.
**Score: 2**

### RTE_21_5
NIST AI 600-1 §2.9 data poisoning provides a weak hook for consolidation-driven abstraction attacks since the abstraction process consuming poisoned episodes is a form of poisoned data pipeline generating corrupted derived outputs. However, NIST does not address memory consolidation architectures, the modular recombination of abstracted procedures, or the emergent complexity property where mixed malicious components produce increasingly sophisticated attack capabilities over time. The multi-generational nature of this attack—where malware genes accumulate and combine through abstraction cycles—is a runtime learning dynamic entirely outside NIST AI 600-1's governance-level controls.
**Score: 1**

---

## RTE_22 - Knowledge Base & RAG Attacks

### RTE_22_1
NIST AI 600-1 §2.9 data poisoning directly covers the mechanism by which adversaries corrupt documents in knowledge bases that AI systems retrieve and act upon, providing meaningful coverage of shared knowledge base poisoning as a multi-agent malware distribution vector. The framework explicitly recognizes that data consumed by AI systems can be weaponized to manipulate behavior, which maps to poisoned documents being executed as instructions by all retrieving agents. MEASURE 2.7 security evaluation and red-teaming could identify knowledge base poisoning vulnerabilities pre-deployment. However, NIST does not address the 1-to-N amplification property specific to shared knowledge bases serving multi-agent systems, nor does it provide technical controls for document-level instruction injection detection.
**Score: 2**

### RTE_22_2
NIST AI 600-1 §2.9 indirect prompt injection is the most relevant control since RAG-based instruction self-replication involves agents retrieving malicious content from external knowledge sources and incorporating it as behavioral guidance—the canonical indirect prompt injection scenario. The self-replicating property, where retrieved instructions guide subsequent queries toward related malicious documents, aligns with NIST's recognition that external data can redirect AI system behavior. MEASURE 2.7 red-teaming could surface this iterative retrieval loop pre-deployment. However, NIST does not address the self-reinforcing propagation dynamic specific to iterative RAG retrieval, nor provide controls for detecting when retrieved content is being incorporated as system-level instruction rather than informational context.
**Score: 2**

### RTE_22_3
NIST AI 600-1 §2.9 data poisoning provides a moderate hook since cross-document instruction assembly involves multiple poisoned documents in the knowledge base—a form of distributed data poisoning where no single document contains a complete malicious payload. The indirect prompt injection framework is also partially relevant since assembled fragments collectively function as injected instructions. However, NIST does not address fragment-based obfuscation techniques, semantic assembly of distributed payloads, or the specific challenge of detecting incomplete instruction fragments that are individually benign. MEASURE 2.7 red-teaming would need to test fragment assembly scenarios specifically, which is not indicated in the framework.
**Score: 2**

### RTE_22_4
NIST AI 600-1 §2.9 data poisoning provides a weak hook for synonym injection attacks since corrupting synonym relationships in a knowledge base represents poisoning of a semantic infrastructure component. However, the framework's data poisoning controls focus primarily on training data and document content rather than semantic relationship structures or vector space topology used by retrieval systems. NIST does not address semantic search infrastructure integrity, synonym graph poisoning, or the specific obfuscation technique of routing benign queries to malicious content through corrupted similarity relationships. This attack targets retrieval infrastructure internals that fall outside NIST AI 600-1's governance-level scope.
**Score: 1**

---

## RTE_23 - Shared Context & Aggregation

### RTE_23_1
NIST AI 600-1 §2.9 indirect prompt injection covers the core mechanism of this attack since shared working memory buffers function as external data channels through which injected instructions propagate to all agents reading that buffer—the defining characteristic of indirect prompt injection in a multi-agent context. The framework's recognition that AI systems can be manipulated via external data they consume applies directly to agents reading adversarially deposited messages from shared buffers. However, NIST does not address shared buffer architectures, the amplification dynamic where instructions accumulate through repeated agent access, or technical controls for message provenance verification in multi-agent working memory pools. MEASURE 2.7 red-teaming could surface this vulnerability class pre-deployment.
**Score: 2**

### RTE_23_2
NIST AI 600-1 §2.9 indirect prompt injection provides moderate coverage since the attack involves malicious content injected at leaf agent level propagating upward through aggregation layers to influence top-level decisions—a multi-hop indirect injection scenario. The framework recognizes that AI systems processing external data can have that data weaponized, which maps to aggregation heuristics laundering injected content into trusted aggregated outputs. However, NIST does not address hierarchical multi-agent aggregation architectures, confidence-weighted aggregation as an attack amplifier, or the reverse-verification gap that allows top-layer agents to trust aggregated results without auditing leaf-level inputs. The social engineering dimension exploiting aggregation heuristics is not addressed at any level in NIST AI 600-1.
**Score: 2**

### RTE_23_3
NIST AI 600-1 §2.9 indirect prompt injection provides a thin hook since selective context sharing creates asymmetric information environments where agents act on summarized context without access to dangerous details hidden in excluded content—a form of environmental manipulation affecting AI decision-making. However, NIST does not address selective context sharing architectures, information privilege asymmetry between communicating agents, or the structural vulnerability of summarization-based context filtering as an attack surface. The framework's indirect prompt injection recognition applies to data that agents do process, not to attacks exploiting what agents do not see. This information asymmetry exploitation is a multi-agent architectural concern outside NIST's scope.
**Score: 2**

---

## RTE_24 - Utility & Preference Attacks

### RTE_24_1
NIST AI 600-1 has no coverage of utility function security, shared preference learning architectures, or the specific threat of gradual utility weight poisoning causing convergent malicious preferences across an agent fleet. The framework's §2.9 data poisoning hook is extremely weak here since the attack targets learning dynamics—how agents update utility weights from shared training data—rather than document or dataset poisoning in the conventional sense. NIST does not address multi-agent utility alignment, shared learning corpus integrity, or the convergence property where individually small poisoning steps accumulate to wholesale utility function corruption. Utility function security is explicitly outside NIST AI 600-1's governance-level scope.
**Score: 1**

### RTE_24_2
NIST AI 600-1 §2.9 indirect prompt injection provides a weak conceptual hook since utility-based social engineering involves poisoned recommendations from one agent influencing another agent's verification process—a form of inter-agent data manipulation. However, NIST does not address multi-agent recommendation chains, utility inference from peer outputs, or the specific trust collapse mechanism where verification agents infer prior optimization from recommendation presence rather than independently validating utility claims. The framework's supply chain controls in GOVERN 6.1/6.2 do not extend to runtime inter-agent recommendation integrity. This attack exploits implicit trust in peer utility optimization that falls outside NIST's governance-level controls.
**Score: 1**

---

## RTE_25 - Multi-Agent Reinforcement Learning (MARL)

### RTE_25_1
NIST AI 600-1 has no controls covering multi-agent reinforcement learning, joint reward optimization, or emergent coordination behaviors arising from MARL training dynamics. The framework does not recognize MARL systems as a distinct deployment paradigm with unique security properties. §2.9 data poisoning provides no meaningful hook since this attack operates through reward signal manipulation and policy gradient dynamics rather than data content poisoning. GOVERN 6.1/6.2 supply chain controls do not address MARL training pipeline integrity. Learned implicit coordination between agents as a social engineering vector is entirely outside NIST AI 600-1's governance-level scope.
**Score: 1**

### RTE_25_2
NIST AI 600-1 §2.9 data poisoning provides a marginal conceptual hook since shared experience replay buffers represent a data structure whose corruption propagates across multiple agents' learning—a form of shared data poisoning. However, the framework's data poisoning controls focus on training datasets and knowledge bases rather than RL-specific experience replay buffers and the policy update dynamics through which poisoned transitions propagate. NIST does not address experience sharing architectures, replay buffer integrity, or the self-replicating property where poisoned transitions influence policy updates that generate additional poisoned experiences. The RL-specific propagation mechanism is outside NIST's scope.
**Score: 1**

### RTE_25_3
NIST AI 600-1 has no coverage of multi-agent consensus mechanisms, policy averaging across agent populations, or the specific attack of moving consensus optima toward malicious behaviors through majority poisoning. The framework does not address distributed policy learning, consensus-based policy aggregation, or the threshold property where poisoning a sufficient fraction of agents controls the consensus outcome. §2.9 data poisoning is nominally relevant only if consensus inputs are treated as data, but NIST does not engage with consensus learning dynamics or the aggregation security properties of federated policy learning. MARL consensus manipulation is explicitly outside NIST AI 600-1's scope.
**Score: 1**

### RTE_25_4
NIST AI 600-1 has no coverage of learned communication protocols in multi-agent systems or the security properties of emergent implicit messaging conventions. The framework does not address how agents can develop shared communication encodings through joint learning that are not interpretable by external monitors. §2.9 indirect prompt injection is not applicable since the attack involves exploiting conventions agents learned rather than injecting content into data channels. GOVERN 6.1/6.2 supply chain controls do not address learned communication security. Exploitation of emergent learned protocols is a MARL-specific concern entirely outside NIST AI 600-1's governance-level scope.
**Score: 1**

### RTE_25_5
NIST AI 600-1 §2.9 data poisoning provides a marginal hook insofar as poisoned reward structures represent corrupted training signals—a form of data poisoning targeting the learning process rather than the data corpus. MEASURE 2.7 red-teaming could theoretically surface emergent malicious behaviors pre-deployment if the evaluation environment triggers the poisoned reward conditions. However, NIST does not address reward function integrity, emergent behavior detection, or the deceptive activation property where malicious behaviors appear only in deployment conditions not present during evaluation. The emergent and latent nature of this attack—behaviors that arise as unintended learning outcomes and activate conditionally—is outside the framework's threat model entirely.
**Score: 1**
# NIST AI 600-1 RTE Threat Analysis - Cache 5

## RTE_26 - Hybrid System Attacks

### RTE_26_1
NIST AI 600-1 operates at the governance and organizational policy level and does not engage with the technical architecture of hybrid AI systems combining neural, symbolic, and utility-based paradigms. The framework's §2.9 Information Security controls address prompt injection in general terms but do not contemplate paradigm-crossing instruction encoding where data in one computational paradigm becomes executable in another at agent boundaries. GOVERN 6.1/6.2 supply chain vetting applies to third-party components at procurement time and would not detect malware that exploits runtime inter-paradigm semantic ambiguity. There are no NIST controls addressing hybrid system architecture, instruction boundary validation, or cross-paradigm execution semantics, making this attack entirely outside the framework's scope.
**Score: 1**

### RTE_26_2
NIST AI 600-1 §2.9 includes data poisoning as a recognized risk category, which provides a thin conceptual hook for knowledge graph backdoor injection since the attack involves corrupting a shared data store that agents consume. The framework acknowledges that training data and knowledge bases can be manipulated to alter AI behavior, and the knowledge graph serves as a persistent shared data layer analogous to a poisoned knowledge base. However, NIST does not address distributed covert communication channels established through graph topology, multi-agent coordination via shared query patterns, or the detection of steganographic structures embedded in knowledge graph relationships. The governance-level framing of data poisoning does not translate into technical controls capable of identifying or preventing topological backdoor structures.
**Score: 2**

---

## RTE_27 - Infrastructure & Deployment Attacks

### RTE_27_1
Vector database multi-tenancy exploitation is a data infrastructure security problem involving partition key validation and access control enforcement at the database layer, which sits entirely outside NIST AI 600-1's scope. The framework does not address vector stores, embedding databases, retrieval-augmented generation infrastructure, or tenant isolation mechanisms. §2.9 Information Security does not descend to the level of database partition key validation or cross-tenant data leakage in retrieval systems. This attack is an infrastructure and data engineering concern with no corresponding NIST AI 600-1 control.
**Score: 1**

### RTE_27_2
Prometheus relabeling configuration injection is a monitoring infrastructure attack targeting time-series metric collection pipelines, which NIST AI 600-1 does not address at any level. The framework contains no controls for AI system observability infrastructure, metric pipeline integrity, or telemetry authentication. §2.9 Information Security and MANAGE 4.1-4.3 incident response controls assume monitoring infrastructure is trustworthy and do not account for adversarial manipulation of the monitoring layer itself. This attack is an infrastructure operations concern entirely outside the governance-level scope of NIST AI 600-1.
**Score: 1**

### RTE_27_3
API gateway rate limiting and multi-agent request distribution coordination are network-layer and API infrastructure concerns that NIST AI 600-1 does not engage with. The framework has no controls addressing API gateway configuration, rate limit enforcement mechanisms, or coordinated multi-agent network behavior at the infrastructure level. While GOVERN and MANAGE controls address organizational oversight of AI systems, they do not specify technical controls preventing distributed request coordination designed to evade per-source rate limits. This is an API infrastructure attack with no applicable NIST AI 600-1 coverage.
**Score: 1**

### RTE_27_4
NIST AI 600-1 §2.12 Value Chain and Transparency provides a moderate conceptual hook for MLflow experiment tracking metadata injection because experiment metadata represents a form of documented provenance that flows through AI development and deployment pipelines involving multiple agents. The framework's supply chain integrity concerns encompass the integrity of artifacts and metadata that inform AI system behavior, and injected experiment metadata consumed as configuration by downstream agents aligns with the supply chain contamination pattern. However, NIST does not specifically address ML experiment tracking systems, MLflow as an infrastructure component, or the specific attack surface of metadata-as-instruction injection in MLOps pipelines. The §2.12 supply chain framing provides an indirect governance hook but no technical controls for validating experiment metadata integrity.
**Score: 2**

---

## RTE_28 - Microservices & Kubernetes

### RTE_28_1
TLS downgrade attacks on service-to-service communication in container orchestration environments are PKI and network security infrastructure concerns entirely outside NIST AI 600-1's scope. The framework does not address mTLS enforcement, container networking, service mesh configuration, or cryptographic protocol negotiation between AI agent microservices. §2.9 Information Security does not descend to the level of transport layer security protocol configuration in Kubernetes environments. This is a network infrastructure attack with no corresponding NIST AI 600-1 control.
**Score: 1**

### RTE_28_2
Kubernetes NetworkPolicy and Istio AuthorizationPolicy misconfiguration enabling unauthorized inter-agent lateral movement is a container orchestration security concern outside NIST AI 600-1's scope. The framework does not address service mesh authorization policies, network segmentation enforcement in container clusters, or the specific failure modes of Kubernetes-native access control mechanisms. GOVERN 6.1/6.2 supply chain vetting does not extend to runtime network policy configuration validation. This is a Kubernetes infrastructure security problem with no applicable NIST AI 600-1 coverage.
**Score: 1**

### RTE_28_3
Shared service account credential abuse enabling agent impersonation is a Kubernetes identity and access management concern that NIST AI 600-1 does not address. The framework has no controls covering service account architecture, credential isolation between agent instances, or Kubernetes-native identity mechanisms. While GOVERN controls address organizational accountability, they do not specify technical requirements for agent identity isolation at the infrastructure level. This is a Kubernetes RBAC and identity management problem outside the framework's scope.
**Score: 1**

### RTE_28_4
Service account token extraction and reuse across pod instances is a Kubernetes secrets management and credential hygiene problem entirely outside NIST AI 600-1's scope. The framework does not address Kubernetes pod security, secret storage, token rotation policies, or the specific threat of credential extraction from running container workloads. §2.9 Information Security operates at the AI system behavioral level and does not engage with container runtime secrets management. This is a Kubernetes infrastructure security concern with no corresponding NIST control.
**Score: 1**

### RTE_28_5
ClusterRole and ClusterRoleBinding privilege escalation through AI agent API access is a Kubernetes RBAC security problem that NIST AI 600-1 does not address. The framework has no controls specifying Kubernetes role design, cluster-scoped permission boundaries for AI agent service accounts, or detection of unauthorized RBAC modifications. GOVERN 6.1/6.2 vetting applies at procurement and configuration planning stages but does not translate into technical controls preventing runtime privilege escalation through the Kubernetes API. This is a Kubernetes access control infrastructure concern outside the framework's scope.
**Score: 1**

### RTE_28_6
mTLS certificate store poisoning enabling man-in-the-middle attacks on inter-agent communication is a PKI infrastructure security problem entirely outside NIST AI 600-1's scope. The framework does not address service mesh certificate management, trust anchor stores, or cryptographic identity for AI agent microservices. §2.9 Information Security does not engage with transport-layer cryptographic integrity or certificate validation mechanisms. This is a PKI and service mesh infrastructure attack with no applicable NIST AI 600-1 coverage.
**Score: 1**

### RTE_28_7
Container image signature spoofing enabling malicious image injection into pod replicas is a container supply chain security problem that NIST AI 600-1 does not specifically address at the infrastructure level. The framework's §2.12 supply chain controls could provide a thin governance-level hook since container images are artifacts in the AI system supply chain, but NIST does not specify image signing requirements, registry security controls, or signature verification enforcement in container runtimes. The governance framing of supply chain integrity does not translate into actionable technical controls for container image provenance validation. This attack is primarily a container infrastructure security concern outside the framework's operational scope.
**Score: 1**

---

## RTE_29 - Performance Optimization & Model Registry

### RTE_29_1
NIST AI 600-1 §2.12 supply chain integrity provides a moderate conceptual alignment with optimization recommendation propagation as a malware vector because recommendations generated by one AI agent and consumed as configuration or instruction by orchestration agents represent an inter-agent data flow analogous to supply chain artifact consumption. The framework's recognition that AI system behavior can be influenced through the artifacts and information it ingests is relevant to the attack mechanism. However, NIST does not address multi-agent orchestration architectures, the integrity of optimization recommendation pipelines, or the specific attack surface where one agent's output becomes another agent's executable instruction. The §2.12 hook provides governance-level framing but no technical controls for validating recommendation integrity.
**Score: 2**

### RTE_29_2
NIST AI 600-1 §2.12 Value Chain and Transparency provides a moderate coverage hook for model registry version manipulation since the model registry is a core supply chain artifact store and version selection determines which AI model all consuming agents load and execute. The framework explicitly recognizes that third-party and supply chain components can be compromised to introduce risks into AI systems, and a poisoned model registry entry causing fleet-wide simultaneous model replacement aligns directly with the supply chain integrity concern. GOVERN 6.1/6.2 third-party vetting provides additional governance framing for registry access controls and model provenance verification requirements. However, NIST does not specify technical controls for semantic version constraint validation, registry signing, or cryptographic model provenance that would prevent this attack at the implementation level.
**Score: 2**

### RTE_29_3
Profiling tool integration as a persistent backdoor installation mechanism is a system-level infrastructure security concern involving privileged process access to container and host environments, which NIST AI 600-1 does not address. The framework has no controls covering profiling infrastructure security, system call access granted to performance monitoring tools, or the attack surface created by privileged observability tooling. §2.9 Information Security operates at the AI behavioral level and does not engage with the operating system and runtime privileges required by profiling frameworks. This is an infrastructure security concern with no applicable NIST AI 600-1 coverage.
**Score: 1**

---

## RTE_30 - Scaling & Auto-Scaling

### RTE_30_1
Pod anti-affinity rule manipulation enabling targeted agent isolation through node scheduling poisoning is a Kubernetes scheduler security concern entirely outside NIST AI 600-1's scope. The framework does not address container orchestration scheduling, node affinity policies, or the availability risks created by forced co-location of agent replicas. MANAGE 4.1-4.3 incident response controls address organizational responses to AI system incidents but do not specify infrastructure-level controls preventing scheduler manipulation. This is a Kubernetes infrastructure attack with no corresponding NIST AI 600-1 control.
**Score: 1**

### RTE_30_2
Service account RBAC misconfiguration enabling privilege escalation and cross-agent manipulation is a Kubernetes identity and access management concern that NIST AI 600-1 does not address at the technical level. The framework's governance controls do not specify Kubernetes service account design principles, permission scoping requirements for AI agent workloads, or detection mechanisms for unauthorized cross-agent configuration manipulation. While GOVERN 6.1/6.2 supply chain and third-party controls address organizational-level vetting, they do not translate into Kubernetes RBAC design requirements. This is a Kubernetes infrastructure security problem outside the framework's operational scope.
**Score: 1**
# NIST AI 600-1 RTE Threat Analysis - Cache 6

## RTE_31 - Fleet Management & Provisioning

### RTE_31_1
NIST AI 600-1 §2.12 Value Chain and GOVERN 6.1/6.2 supply chain controls provide indirect but meaningful coverage for over-the-air update chain of custody corruption by establishing requirements for supply chain integrity, third-party vetting, and update provenance verification. The framework's supply chain risk management provisions recognize that model integrity can be compromised at any point in the delivery pipeline, including staging and deployment stages. However, NIST does not specify technical controls for OTA update signing, baseline integrity validation, or fleet-wide staged rollout health check integrity. The poisoned baseline scenario—where health checks themselves verify against a compromised reference—represents an infrastructure operational concern beyond NIST's governance-level scope.
**Score: 2**

### RTE_31_2
NIST AI 600-1 does not address hardware-specific engine validation or the challenge of staging coverage gaps across heterogeneous production hardware variants. The framework operates at a governance and organizational policy level that does not engage with TensorRT engine compilation, hardware-specific behavioral differences, or the validation methodology required for multi-hardware fleet deployments. GOVERN 6.1/6.2 supply chain vetting focuses on third-party software and model provenance rather than runtime hardware execution validation. This attack exploits a gap that is entirely infrastructure-specific and falls outside any NIST AI 600-1 control domain.
**Score: 1**

### RTE_31_3
NIST AI 600-1 provides no coverage for provisioning token security, token generation authorization controls, or the self-replicating malware pattern that exploits provisioning infrastructure. GOVERN 6.1/6.2 supply chain controls address procurement-time vetting of third-party components rather than runtime token lifecycle management or the prevention of unauthorized token generation by compromised edge devices. Section 2.9 Information Security does not extend to provisioning infrastructure authentication mechanisms. This threat operates at the deployment infrastructure layer, which NIST AI 600-1 explicitly does not govern.
**Score: 1**

### RTE_31_4
NIST AI 600-1 does not address PKI infrastructure, certificate authority compromise, certificate rotation processes, or trust chain desynchronization across distributed agent fleets. The framework's GOVERN 6.1/6.2 supply chain controls focus on AI model and component provenance rather than cryptographic certificate lifecycle management. Section 2.9 Information Security recognizes broad information security concerns but does not specify controls for certificate management, rotation timing, or the detection of partial certificate authority compromise. Certificate desynchronization as a trust chain attack is a PKI infrastructure concern entirely outside NIST's AI-specific risk management scope.
**Score: 1**

---

## RTE_32 - Batching & Caching Infrastructure

### RTE_32_1
NIST AI 600-1 does not address load balancer security, request routing integrity, or the use of load balancer state as a malware propagation vector. The framework has no controls governing the infrastructure layer at which load balancers operate, including state poisoning, routing manipulation, or the systematic channeling of requests through compromised replicas. Section 2.9 Information Security addresses prompt injection and data poisoning at the model input level rather than at the request routing infrastructure layer. This attack is a network infrastructure concern that falls entirely outside NIST's AI governance scope.
**Score: 1**

### RTE_32_2
NIST AI 600-1 provides no coverage for auto-scaling infrastructure security, scaling metric integrity, or the coordination of latent malware activation through auto-scaling triggers. The framework does not address shared state security between replicas, the integrity of scaling decision inputs, or coordinated multi-agent activation patterns. GOVERN 6.1/6.2 supply chain controls do not extend to the infrastructure orchestration layer where auto-scaling policies and shared state reside. This threat exploits cloud infrastructure operational mechanisms that are entirely outside NIST AI 600-1's governance-level scope.
**Score: 1**

---

## RTE Other Threats

### RTE_Other_1 - Trace-Based Few-Shot Learning Poisoning Through Compromised Execution Histories
NIST AI 600-1 §2.9 data poisoning provisions provide direct conceptual coverage for trace-based few-shot learning poisoning, as the attack involves corrupting the training data equivalent (execution traces) that downstream agents learn from. The framework recognizes that adversarial manipulation of training inputs can compromise model behavior, which maps to the mechanism by which malicious execution patterns embedded in upstream agents' traces propagate as learned behaviors. NIST does not, however, address the specific multi-agent scenario where inference-time few-shot examples derive from peer agents' execution histories rather than curated training data. The indirect pathway from runtime trace contamination to behavioral corruption in downstream agents requires more operational specificity than NIST provides.
**Score: 2**

### RTE_Other_2 - Parameter Accuracy Assumptions Between Agents Without Verification
NIST AI 600-1 does not address inter-agent parameter validation, implicit trust assumptions between agents, or the verification gap created when multi-agent architectures delegate parameter extraction without independent verification. The framework's §2.9 Information Security focuses on prompt injection and data poisoning rather than the operational trust model governing parameter passing between coordinating agents. GOVERN 6.1/6.2 supply chain vetting operates at system procurement level rather than runtime inter-agent communication protocol level. This vulnerability is a structural gap in multi-agent trust architecture that NIST's governance-level framework does not engage with.
**Score: 1**

### RTE_Other_3 - Tool Call Verification Bypasses Through Multi-Agent Trust Chains
NIST AI 600-1 §2.9 prompt injection provisions provide moderate coverage for tool call verification bypasses, as the underlying attack involves manipulating the tool invocation pipeline through injection of malicious instructions that cause an agent to select inappropriate tools. The framework recognizes prompt injection as a mechanism by which AI systems can be induced to take unintended actions, which includes the failure mode of executing tool calls that were not appropriately verified for selection appropriateness. However, NIST does not address the specific architectural split where tool selection and tool execution are delegated to separate agents with no re-verification handshake. The gap between Agent A's selection and Agent B's execution without reverification is an operational multi-agent design concern beyond NIST's current controls.
**Score: 2**

### RTE_Other_4 - Parameter Validation Delegation Without Re-Validation
NIST AI 600-1 does not address parameter validation delegation patterns, the assumption of upstream validation completeness, or the specific failure mode where Agent B omits independent parameter verification because it trusts Agent A's prior validation. The framework's information security provisions focus on the manipulation of AI inputs through adversarial injection rather than on the organizational trust protocols governing agent-to-agent workflow handoffs. MEASURE controls address testing and evaluation but not runtime re-validation requirements for inter-agent parameter passing. This is an operational process design gap in multi-agent architectures that falls outside NIST's governance-level scope.
**Score: 1**

### RTE_Other_5 - Inter-Agent Context Pollution Enabling Transitive Instruction Injection
NIST AI 600-1 §2.9 indirect prompt injection directly addresses the threat pattern where malicious instructions embedded in data sources—including shared context—cause AI systems to execute unintended actions. The framework recognizes that AI models processing external content can be manipulated by adversarial instructions contained within that content, which maps to the mechanism where Agent B reads shared context and interprets Agent A's user instructions as applicable to itself. However, NIST frames this primarily as a single-agent concern and does not address the transitive propagation characteristic that makes multi-agent context pollution more severe. The absence of NIST controls for context isolation or instruction scoping in multi-agent shared memory compounds the gap.
**Score: 2**

### RTE_Other_6 - Conversation ID Chain Hijacking Creating Transitive Instruction Propagation
NIST AI 600-1 §2.9 indirect prompt injection provisions provide relevant coverage for conversation ID chain hijacking, as attackers inject messages that appear as authorized inter-agent communication and contain instructions that downstream agents execute without independent verification. The framework's recognition that malicious content in processed data can redirect AI behavior applies to the mechanism by which hijacked conversation IDs cause agents to treat injected messages as legitimate workflow communications. NIST does not address conversation ID authentication, inter-agent message provenance verification, or the specific transitive trust chain that amplifies single-injection attacks across multi-agent workflows. The indirect match is meaningful but the framework lacks the technical specificity to prevent this attack class.
**Score: 2**

### RTE_Other_7 - Efficiency Metric Sharing as Malware Propagation Channel
NIST AI 600-1 does not address efficiency metric sharing channels, the use of operational metrics as malware propagation vectors, or the specific threat pattern where embedded instructions spread between agents through normal business communication about performance optimization. The framework's §2.9 information security provisions do not extend to agent-to-agent communication channels used for operational coordination. GOVERN 6.1/6.2 supply chain controls focus on third-party model and component vetting rather than intra-fleet agent communication integrity. This economic and operational attack pattern exploiting metric-sharing channels is outside NIST's current scope.
**Score: 1**

### RTE_Other_8 - Trust Degradation Through Efficiency Report Poisoning
NIST AI 600-1 does not provide controls addressing the manipulation of efficiency reports to degrade trust in legitimate agents or artificially elevate trust in compromised agents. The framework's information security provisions focus on direct prompt injection and training data poisoning rather than the indirect manipulation of operational reputation signals. GOVERN 6.1/6.2 third-party vetting does not extend to runtime trust score manipulation between fleet agents. This attack exploits economic and operational coordination mechanisms that fall outside NIST's governance-level risk management scope.
**Score: 1**

### RTE_Other_9 - Cost Optimization Policy Self-Replication Through Agent Communication
NIST AI 600-1 does not address policy self-replication through agent communication channels, the embedding of malicious directives in ostensibly legitimate policy documents, or the propagation mechanism by which agents share and implement policies received from peer agents. The framework's supply chain provisions in §2.12 and GOVERN 6.1/6.2 focus on third-party model and software vetting rather than intra-fleet policy sharing integrity. Section 2.9 does not engage with economic or operational attack patterns that use policy communication as a propagation vector. This threat represents a novel multi-agent propagation channel that is entirely outside NIST's current control coverage.
**Score: 1**

### RTE_Other_10 - Efficiency Consensus Exploitation for Coordinated Attack
NIST AI 600-1 does not address consensus mechanisms in multi-agent systems, the amplification of attacks through distributed voting on malicious payloads, or the coordinated simultaneous implementation of poisoned efficiency measures across a fleet. The framework does not engage with distributed agent coordination protocols, consensus-based decision making, or the failure modes that emerge when consensus mechanisms inadvertently legitimize adversarial content. This attack exploits a multi-agent architectural pattern—consensus-based collective decision making—that is entirely beyond NIST's governance-level scope.
**Score: 1**

### RTE_Other_11 - Efficiency Benchmark Gaming as Coordinated Deception
NIST AI 600-1 does not address benchmark gaming, emergent coordinated deception from collective metric optimization, or the scenario where agents inadvertently activate hidden behaviors through coordinated pursuit of performance measurements. The framework's MEASURE controls address evaluation methodology and red-teaming but focus on pre-deployment assessment rather than runtime emergent coordination patterns. GOVERN provisions do not address the organizational incentive structures that make benchmark gaming possible or the multi-agent dynamics that amplify individual gaming into coordinated deception. This threat category is outside NIST's current scope.
**Score: 1**

### RTE_Other_12 - Resource Budget Negotiation as Malware Trading
NIST AI 600-1 does not address resource budget negotiation protocols between agents, the exploitation of optimization offer mechanisms as malware distribution channels, or the specific pattern where agents accept and propagate techniques received through budget negotiation exchanges. The framework's supply chain and information security provisions do not extend to runtime inter-agent bargaining communications. This attack exploits a multi-agent economic coordination mechanism that has no analog in the single-agent threat models that NIST AI 600-1 was designed to address.
**Score: 1**

### RTE_Other_13 - Vector Database Query Quality Metrics Blind Spots in Prometheus Monitoring
NIST AI 600-1 does not address Prometheus monitoring configuration, vector database retrieval quality metrics, or the exploitation of monitoring blind spots to conduct gradual undetected quality degradation attacks. The framework's MEASURE controls address evaluation and testing at an organizational level but do not specify operational monitoring requirements for retrieval infrastructure. GOVERN provisions do not engage with infrastructure observability gaps or the specific challenge that multi-agent coordination creates for detecting distributed quality degradation. This is an infrastructure monitoring concern outside NIST's governance-level scope.
**Score: 1**

### RTE_Other_14 - Cluster Node Trust Exploitation Through Unauthenticated Gossip Protocol
NIST AI 600-1 does not address gossip protocol security, cryptographic authentication requirements for cluster communication, or the exploitation of unauthenticated topology information in distributed systems. The framework has no controls governing the infrastructure-level communication protocols that multi-agent systems use for cluster membership management and routing information distribution. Section 2.9 information security provisions do not extend to the network protocol layer at which gossip protocol attacks occur. This is an infrastructure communication security concern entirely outside NIST's AI governance scope.
**Score: 1**

### RTE_Other_15 - Load Balancer Trust Assumption Enabling MITM Attacks Between Agents and Vector Database Nodes
NIST AI 600-1 does not address mutual TLS requirements for load balancer configurations, the exploitation of VIP trust assumptions, or man-in-the-middle attack surfaces created by load balancer architecture in multi-agent deployments. The framework's information security provisions focus on AI model input manipulation rather than the network infrastructure through which agent communications are routed. GOVERN 6.1/6.2 supply chain controls do not extend to load balancer security configuration or the authentication protocols governing agent-to-database communication paths. This is a network infrastructure security concern outside NIST's scope.
**Score: 1**

### RTE_Other_16 - Fact-Checking Rail Cascaded Verification Gaming Through Threshold Boundary Exploitation
NIST AI 600-1 §2.9 Information Security provisions provide indirect coverage for guardrail bypass attacks by recognizing that security controls protecting AI systems can be deliberately circumvented. The framework acknowledges that adversarial actors will attempt to undermine safety and security mechanisms, which maps conceptually to threshold boundary exploitation designed to systematically bypass cascaded verification stages. However, NIST does not address NeMo Guardrails specifically, threshold configuration security, or the fleet-wide synchronized bypass that emerges when shared guardrail configurations create identical exploitation surfaces across all agents. The governance-level recognition of guardrail circumvention as a risk is present but the technical specificity needed to prevent this attack class is absent.
**Score: 2**

### RTE_Other_17 - Execution Rail Resource Limit Coordination Failures Enabling Fleet-Wide Quota Exhaustion
NIST AI 600-1 does not address per-agent resource limit configuration, the aggregation of individual resource quotas creating fleet-wide exhaustion, or the coordination failures in distributed resource management that enable denial-of-service through quota manipulation. The framework's scope does not extend to infrastructure resource governance, operational capacity planning, or the technical controls needed to prevent coordinated resource exhaustion across agent fleets. This is an economic and operational denial-of-service concern that falls outside NIST's AI risk management focus.
**Score: 1**

### RTE_Other_18 - Multi-LLM NIM Model Source Trust Exploitation Through Safetensors Validation Bypass
NIST AI 600-1 §2.12 Value Chain and GOVERN 6.1/6.2 supply chain integrity controls provide meaningful indirect coverage for this threat, as model file format validation bypass represents a supply chain integrity failure where a poisoned model is introduced through a trusted distribution channel. The framework's recognition that AI components can be compromised at any point in the supply chain applies to the safetensors format manipulation used to smuggle malicious content past format validators. However, NIST does not address file format validation specifics, safetensors versus pickle security properties, or the shared NIM catalog architecture that allows one poisoned model to propagate to all agents through auto-update. The supply chain alignment is real but the framework lacks technical specificity for this attack vector.
**Score: 2**

### RTE_Other_19 - LLM-Specific NIM Hardware Detection Fallback Exploitation Through vLLM Performance Degradation
NIST AI 600-1 does not address GPU detection mechanisms, hardware fallback exploitation, vLLM performance degradation through hardware spoofing, or the fleet-wide throughput impact of coordinated hardware detection manipulation. The framework has no controls governing the hardware abstraction layer at which this attack operates, including GPU cache integrity, hardware capability reporting, or inference infrastructure performance security. GOVERN and MEASURE provisions focus on model-level and organizational risk management rather than the infrastructure hardware layer. This is an AI inference infrastructure attack that falls entirely outside NIST's governance-level scope.
**Score: 1**

## RWA - Workflow and Ecosystem Attacks on Plugins, Tools, and RAG Pipelines

### RWA_1 - Specification Gaming and Misalignment

RWA_1 describes operational specification gaming threats where agents exploit architectural feedback loops, tool interaction patterns, and metric optimization pressures to achieve misalignment at runtime — covering workflow template injection, adaptive threshold manipulation, circuit breaker gaming, HNSW index poisoning, semantic cache poisoning, batching attacks, ETL pipeline gaming, metric manipulation, and reward-shaped tool selection. NIST AI 600-1's governance framework addresses upstream supply chain risk (GOVERN 6.1/6.2), pre-deployment safety evaluation (MEASURE 2.7), and data poisoning (§2.9), with thin coverage of tool schema poisoning and RAG metadata injection as §2.9 data poisoning instantiations. However, NIST lacks controls for emergent runtime behaviors: feedback loop exploitation, threshold learning gaming, critic capture in reflection systems, circuit breaker gaming, error-driven fallback abuse, and optimization-driven tool selection misalignment — phenomena invisible to governance frameworks focused on supply chain control and pre-deployment assessment.

**Strength Score: 1/3** - Pre-deployment red-teaming and data poisoning controls negligibly address runtime operational specification gaming.

### RWA_2 - Model Training and Backdoors

NIST AI 600-1 provides governance-level mitigation through §2.12 (supply chain risk management for third-party AI components) and §2.9 (data poisoning), with GOVERN 6.1/6.2 requiring third-party risk assessment and training data integrity controls. MEASURE 2.7 enables adversarial testing and red-teaming of AI systems before deployment. However, RWA_2's 60 threat items reveal critical gaps: NIST lacks technical detection for quantization-induced backdoors, TensorRT engine compromises, CUDA kernel injection, speculative decoding poisoning, LoRA adapter manipulation, and persistent volume deployment-time attacks. Supply chain governance controls address third-party model sourcing at a policy level but provide no technical verification mechanisms for fine-tuned adapter poisoning, multimodal backdoors, or evaluation metric corruption during training. Incident response (MANAGE 4.1–4.3) assumes backdoors are already detected, not proactively discovered pre-deployment.

**Strength Score: 2/3** - Governance supply chain and data poisoning controls apply; technical detection mechanisms entirely absent.

### RWA_3 - State and Context Poisoning

NIST AI 600-1 provides limited coverage of state and context poisoning threats in multi-agent systems. Section §2.9 Information Security addresses indirect prompt injection risks when attackers poison accumulated workflow state to influence downstream agent decisions, and data poisoning through ETL state file tampering (timestamp manipulation, selective document omission). GOVERN 6.1/6.2 under §2.12 provides governance-level supply chain oversight for checkpoint and model configuration integrity. However, NIST AI 600-1 does not address LangGraph-specific routing security, state serialization injection mechanics, distributed memory architecture vulnerabilities, reducer logic exploitation, or the emergent misalignment risks arising from context window capacity gaming and state abstraction layers. Items involving cached vector corruption and conversation history manipulation are only thinly covered through the pre-deployment evaluation lens of MEASURE 2.7, without runtime state monitoring controls.

**Strength Score: 2/3** - Indirect prompt injection and data poisoning coverage present; LangGraph and distributed state architecture gaps limit applicability.

### RWA_4 - RAG Pipeline Attacks

NIST AI 600-1 addresses RAG pipeline attacks primarily through §2.9 Information Security, which explicitly covers data poisoning — RAG corpus poisoning being a direct application of this threat model. Section §2.12 (GOVERN 6.1/6.2) covers knowledge base integrity as supply chain risk, and MEASURE 2.7 mandates pre-deployment security evaluation including red-teaming of retrieval-augmented systems. RWA_4's 39 attack vectors include UI-mediated document injection, knowledge graph manipulation, tool description poisoning, few-shot example poisoning, fallback cache poisoning, reranking injection, and multi-stage pipeline bypass — all of which fall under the §2.9 data poisoning category at the governance level. However, NIST provides no technical controls for HNSW parameter attacks, chunking strategy security, vector database authentication, streaming RAG validation, or embedding manipulation — leaving the architectural implementation entirely unspecified.

**Strength Score: 2/3** - Data poisoning directly addressed; supply chain coverage applies; technical RAG pipeline controls absent.

### RWA_5 - Multi-Agent Orchestration Risks

NIST AI 600-1 provides minimal coverage for multi-agent orchestration risks. Section §2.9 partially addresses injection-based threats via indirect prompt injection through inter-agent messages, and §2.12 covers framework-specific backdoors as third-party supply chain risks (CrewAI/Semantic Kernel). The remaining items — spanning service mesh injection, Kubernetes sidecar attacks, swarm emergent misalignment, hierarchical privilege escalation chains, state-logic boundary exploitation, streaming covert escalation, KV cache cross-agent poisoning, federated weakest-link exploitation, circuit breaker exhaustion, agent cascade autonomous actions, and alert fatigue — fall entirely outside NIST AI 600-1's governance scope. The framework defines no multi-agent trust architectures, inter-agent authentication or authorization models, orchestration-layer security controls, or swarm behavior governance mechanisms. §2.7 Human-AI Configuration thinly addresses human oversight gaps in autonomous cascades but provides no structural solution.

**Strength Score: 1/3** - NIST covers only injection and supply chain aspects; multi-agent orchestration security wholly unaddressed.

### RWA_6 - Plugin and Tool Ecosystem Attacks

NIST AI 600-1 provides moderate coverage of plugin and tool ecosystem attacks through its supply chain risk management framework. Section §2.12 (GOVERN 6.1/6.2) explicitly requires third-party component integrity assessment and vendor risk management, directly applicable to plugin marketplace poisoning, framework-dependent supply chain attacks, tool registry compromise, model registry poisoning via container image tampering, dependency version pinning failures, and NIM supply chain attacks. MEASURE 2.7 supports security evaluation of third-party plugin components pre-deployment. However, NIST AI 600-1 does not address framework-specific plugin integration mechanisms (Semantic Kernel, LangChain, AutoGen plugin APIs), container image signing or layer integrity, package manager security, load balancer routing manipulation, or reasoning-guided plugin abuse chains. The governance framework establishes vendor risk policies and third-party assessment requirements but lacks technical controls for plugin metadata validation and registry integrity verification.

**Strength Score: 2/3** - Direct supply chain governance coverage; technical framework-specific plugin security controls absent.

### RWA_7 - Evaluation and Monitoring Bypass

NIST AI 600-1 provides minimal coverage of evaluation and monitoring bypass threats through MEASURE 2.6 (safety evaluation) and MEASURE 2.7 (pre-deployment security evaluation and red-teaming), addressing only the initial evaluation phase before deployment. RWA_7 encompasses 25 distinct operational evasion techniques spanning runtime monitoring blind spots, metric manipulation and hacking across Prometheus and custom metrics, CI/CD threshold gaming, distributed tracing exposure, evaluation data pipeline corruption, and inter-agent communication exploitation. These threat vectors operate primarily in production and post-deployment contexts — real-time monitoring bypass, metric computation library attacks, auto-scaling gaming, and continuous benchmarking integrity — which fall entirely outside NIST AI 600-1's governance scope. The framework's pre-deployment focus leaves systematic gaps for runtime metric poisoning, evaluation result tampering, and coordinated multi-agent fleet-wide evasion strategies.

**Strength Score: 1/3** - Pre-deployment evaluation cannot address runtime monitoring bypass, metric poisoning, or operational evasion techniques.

### RWA_8 - Vector Database and Knowledge Base Attacks

RWA_8 encompasses 21 data integrity attacks targeting knowledge base infrastructure — from vector quantization poisoning and ETL metadata extraction inconsistencies to credential poisoning and deduplication evasion. NIST AI 600-1 provides partial governance-level coverage through §2.9 (Information Security, where data poisoning directly applies to vector database poisoning) and §2.12 (Value Chain/Supply Chain, where knowledge bases constitute supply chain components subject to GOVERN 6.1/6.2 risk management). However, NIST's governance scope excludes vector database technical architecture (Milvus, Weaviate, Pinecone, Chroma), metadata filtering implementation, ETL timestamp race conditions, SHA-256 deduplication bypass, timing side-channels on shared GPU infrastructure, and infrastructure-level attacks such as SQL injection in ETL connectors, memory exhaustion, and cluster gossip protocol injection. Organizations must supplement NIST AI 600-1 with database-specific technical controls and ETL pipeline hardening.

**Strength Score: 2/3** - Data poisoning and supply chain governance apply directly; technical database architecture and ETL implementation attacks fall outside NIST scope.

### RWA_9 - Learning-Based Attacks

NIST AI 600-1 provides minimal governance-level coverage for learning-based attacks. Section §2.9 (Information Security) and §2.12 (Value Chain) address training-time poisoning vectors such as curriculum data contamination and shared dataset compromise, offering narrow applicability. MEASURE 2.7 (security evaluation and red-teaming) provides indirect pre-deployment oversight. However, the framework critically lacks coverage for the majority of RWA_9's runtime learning behaviors: reinforcement learning reward signal manipulation, policy gradient deception optimization, MCTS rollout policy poisoning, experience replay poisoning, imitation learning from poisoned expert demonstrations, learned tool selection preference gaming, long-horizon utility drift through cross-agent preference learning, and cascading multi-agent reward hacking through RLHF amplification. These runtime learning dynamics fall entirely outside NIST AI 600-1's governance scope, leaving systematic gaps in operational agent learning behavior control.

**Strength Score: 1/3** - Training-time poisoning minimally covered; runtime RL/learning behaviors and multi-agent cascades entirely unaddressed.

### RWA_10 - UI/UX Security Attacks

RWA_10 encompasses 14 UI/UX attacks spanning RAG injection (context window exploitation, source attribution spoofing, prompt leakage through error messages), plugin supply chain risks (marketplace manipulation, permission escalation through command palette interaction context), and workflow automation attacks (progressive disclosure exploitation, multi-step attack chain masking). NIST AI 600-1 provides weak indirect coverage: §2.12 Value Chain addresses plugin ecosystem supply chain risks; §2.9 Information Security covers RAG injection in UI contexts; §2.7 Human-AI Configuration addresses transparency gaps at a policy level. However, NIST's governance scope excludes UI/UX design standards, progressive disclosure mechanics, streaming response security, command palette security, embedding truncation attacks across agent tiers, and simulation-based tool trace disclosure — all central to RWA_10's threat model. Technical UI security requires supplementary frameworks beyond NIST AI 600-1.

**Strength Score: 1/3** - NIST governance excludes UI design, progressive disclosure, and streaming attack vectors; supply chain tangentially applies.

### RWA_11 - Reasoning and Reflection Attacks

NIST AI 600-1 provides minimal coverage of reasoning and reflection attacks, which exploit multi-agent coordination mechanisms absent in single-agent systems. Section §2.2 on confabulation offers weak coverage for hallucination amplification through critic-producer validation cycles, and §2.9 Information Security addresses grounding spoofing narrowly through RAG poisoning contexts. However, the framework lacks governance controls for the twelve core attack vectors in RWA_11: multi-hop reasoning path manipulation through document ordering, milestone achievement instruction injection, token accounting manipulation across agent boundaries, false self-critique propagation in reflection traces, hallucination detection evasion through grounding spoofing, schema interpretation reasoning flaws, type coercion reasoning attacks, and function description reasoning quality degradation. NIST AI 600-1 does not address runtime reasoning process integrity, multi-agent reflection security patterns, or reasoning quality dependencies between collaborating agents.

**Strength Score: 1/3** - Confabulation and RAG poisoning minimally covered; multi-agent reasoning integrity and reflection security entirely unaddressed.

### RWA_12 - Distributed Systems Attacks

NIST AI 600-1 provides no meaningful coverage for distributed systems attack vectors including event-driven replay in asynchronous workflows, rate-limit exhaustion through coordinated tool invocation, cache coherence failures across distributed semantic memory, batch insertion race conditions, feature flag propagation race conditions enabling behavioral inconsistency attacks, deadlock injection through circular dependency conditions, out-of-order agent execution race conditions, and cross-agent filter evasion through distributed attack pattern fragmentation. These threats operate at infrastructure and distributed systems architecture layers — timing, concurrency, and distributed coordination primitives — where NIST's governance-level framework lacks control points. The framework assumes operational security boundaries that multi-agent distributed architectures fundamentally violate.

**Strength Score: 1/3** - Infrastructure-level distributed systems threats entirely outside governance scope; no NIST coordination or timing security controls apply.

### RWA_13 - Approval Workflow Exploitation

NIST AI 600-1's §2.7 (Human-AI Configuration) tangentially addresses human oversight (HITL) at a policy level, but provides no controls for approval workflow-specific attack patterns: approval fatigue conditioning, HITL compromise through legitimate-seeming request flooding, consensus voting manipulation in multi-agent approval systems, evaluation metric gaming to manipulate approval thresholds, cross-agent permission escalation through delegation chains, circular HITL dependency deadlocks, and cascading approval bottlenecks. The framework assumes centralized human decision-making rather than distributed approval workflow architectures or multi-agent consensus mechanisms that create novel exploitation surfaces. MANAGE 4.1–4.3 applies only post-incident.

**Strength Score: 1/3** - HITL oversight mentioned in §2.7 but no approval workflow security controls; multi-agent delegation chains unaddressed.

### RWA_14 - Infrastructure and Deployment Attacks

RWA_14 covers GPU timing side-channels in shared infrastructure, GPU resource exhaustion through adversarial batch flooding, CI/CD artifact injection via credentials compromise, health check gaming, GPU batching manipulation for latency gaming, Kubernetes secret store poisoning for NGC API keys, auto-scaling tool quota exhaustion, health check endpoint manipulation for degraded fleet deployment, and MIG reconfiguration attacks causing cascading capacity exhaustion. These operate at GPU, Kubernetes, and CI/CD infrastructure layers entirely outside NIST AI 600-1's governance scope. The framework addresses organizational risk management and third-party component assessment but contains no controls for container orchestration, GPU scheduling, secret management, or deployment pipeline integrity.

**Strength Score: 1/3** - Pure infrastructure and deployment attacks; NIST AI 600-1 has no GPU, Kubernetes, or CI/CD security controls.

### RWA_15 - Tool Invocation and Selection Gaming

RWA_15 covers tool invocation frequency gaming for metric inflation, memory-poisoned tool invocation chain reproduction, AutoGen GroupChat tool negotiation abuse for covert access, streaming partial execution gaming, temperature-controlled tool selection bias toward malicious tools, SLA-driven latency falsification for malicious tool promotion, KV cache tool selection coupling, and rule description injection in tool selection systems. These are operational agent behavior gaming attacks at the execution layer — NIST AI 600-1 does not address tool orchestration security, model sampling behavior, cache coherence exploitation, or tool selection metric integrity. These are runtime operational concerns beyond governance scope.

**Strength Score: 1/3** - Operational agent tool selection and invocation gaming unaddressed by governance framework.

### RWA_16 - Framework-Specific Vulnerabilities

RWA_16 covers three framework-specific vulnerabilities: LangChain's dynamic tool loading from untrusted sources enabling poisoned schema injection, AutoGen's conversational tool negotiation creating implicit access control through dialogue, and NeMo Guardrails centralized configuration tampering enabling fleet-wide safety bypass by relaxing jailbreak detection or disabling verification stages. NIST AI 600-1 provides no controls for framework architecture choices, tool definition loading mechanisms, conversational access negotiation, or guardrail configuration management in specific AI frameworks. §2.12 supply chain governance thinly applies to sourcing and assessing third-party AI framework components but does not prescribe framework-specific hardening.

**Strength Score: 1/3** - Framework-specific implementation details; NIST governance lacks technical control points for vendor-specific AI frameworks.

