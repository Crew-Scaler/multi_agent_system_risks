# Category 3.9 [NEW]: Distributed and Hardware-Level Agent Attacks [RATC_11, RATC_12, RATC_13]

*Alignment: Significantly expands GenAI Domain 3. The RATC domain (38 categories) represents the largest MAS-specific expansion, covering framework-specific tool vulnerabilities, infrastructure attacks, and reasoning exploits not present in any GenAI framework.*

**Description:** Multi-agent production deployments using shared GPU infrastructure, container orchestration, and distributed inference introduce hardware-level attack surfaces absent in single-agent deployments.

## Key Risks

- 3.9.1 KV Cache Cross-User Poisoning via Shared GPU: Multi-user inference on shared GPUs with KV cache optimization enables one agent's context to contaminate another agent's cached attention values with fabricated data. [NoFW]
- 3.9.2 Tensor Parallelism Communication Interception: Attackers intercept or manipulate tensor data flowing between GPU nodes during distributed inference, altering model outputs without modifying weights. [NoFW]
- 3.9.3 Kubernetes Container Security Context Privilege Escalation: Attackers with write access to container security contexts remove filesystem protections, enabling persistent modification of agent execution environments. [NoFW]
- 3.9.4 Horizontal Scaling Distributed Tool Abuse: By distributing tool invocation requests across multiple replicas, attackers coordinate attacks that exceed per-replica rate limits and detection thresholds while remaining invisible to per-instance monitoring. [NoFW]
- 3.9.5 Load Balancing Replica-Specific Tool Abuse: Attackers exploit IP-hash load balancing or session affinity to route requests consistently to specific replicas, enabling targeted exploitation of replica-specific state. [NoFW]
- 3.9.6 GPU Resource Contention on Shared Accelerator Hardware Creating Cross-Agent Denial of Service: Production multi-agent inference platforms sharing GPU hardware through NVIDIA Multi-Process Service (MPS) rely on percentage-based compute allocation providing soft rather than hard isolation. An attacker compromising one agent container can manipulate GPU workloads to bypass MPS throttling limits and monopolize physical GPU compute, degrading all co-located agents simultaneously. A single agent monopolizing one GPU degrades three to six co-located agents; repeated across a fleet of GPUs the impact cascades to dozens of agents simultaneously—a blast radius exceeding 100% of fleet capacity because each GPU's degradation affects multiple agents. GPU VRAM shared under MPS is additionally vulnerable to exhaustion attacks that force CUDA_ERROR_OUT_OF_MEMORY conditions for co-located agents. Single-agent deployments with exclusive GPU access eliminate this cross-agent contention surface entirely. [NoFW]
