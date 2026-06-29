# Category 2.2: Evasion and Adversarial Examples

**Description:** Attackers manipulate model inputs at inference time with subtle perturbations that cause the model to produce incorrect, attacker-desired outputs while the manipulation is invisible or imperceptible to human observers. These attacks exploit the learned decision boundaries of ML models.

## Frameworks

NIST_RMF, DoD_Cyber, MITRE, ENISA, NSA_Deploy, CDAO

## Key Risks

- 2.2.1 White-box evasion attacks: Attackers with full model knowledge use gradient optimization (e.g., FGSM, PGD) to create adversarial examples that change classification labels [E] — NIST_RMF
- 2.2.2 Black-box evasion attacks: Attackers use query access or final decision labels to craft adversarial examples without direct model access [E] — NIST_RMF, DoD_Cyber
- 2.2.3 Universal evasion perturbations: Small perturbations that cause misclassification on most inputs and transfer across model architectures [E] — NIST_RMF
- 2.2.4 Physically realizable adversarial attacks: Adversarial perturbations printed or applied to physical objects fool AI systems in real-world deployment (e.g., adversarial road signs) [E] — NIST_RMF, ENISA
- 2.2.5 Evasion of ML monitoring: Attackers craft inputs that cause the AI's outputs to appear normal to monitoring systems while achieving adversarial goals [E] — DoD_Cyber, MITRE
- 2.2.6 Accuracy-robustness tradeoff: AI systems optimized for accuracy tend to be less robust to adversarial attacks, creating an inherent tension without a known solution [E] — NIST_RMF
- 2.2.7 Multimodal adversarial vulnerability: Single-modality attacks can still compromise multimodal models; combining modalities does not automatically improve robustness [E] — NIST_RMF
- 2.2.8 Gradient-Based Adversarial Policy Perturbations: Learned policies are vulnerable to adversarial perturbations that cause targeted misbehavior. Synchronized gradient vulnerabilities across agents sharing architectures enable single perturbations to affect multiple agents simultaneously, amplifying adversarial impact across the entire coordinated policy fleet. [NoFW]
  - Tianyang Duan et al.. 2025. Rethinking Adversarial Attacks in Reinforcement Learning from Policy Distribution Perspective. In *2025 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP 2025), Hyderabad, India, April 2025*. https://doi.org/10.1109/ICASSP49660.2025.10890540
    - no strong evidence found
  - Wu Yichao, Wang Yirui, Ding Panpan, Wang Hailong, Zhu Bingqian, and Liu Chun. 2026. Enhancing Security in Deep Reinforcement Learning: A Comprehensive Survey on Adversarial Attacks and Defenses. In *Neurocomputing (Elsevier)*. https://doi.org/10.1016/j.neucom.2026.S0925231226009008
    - "These attacks distort the policy gradient update direction by introducing inducive bias into the reward signal while keeping the environment dynamics unchanged."
