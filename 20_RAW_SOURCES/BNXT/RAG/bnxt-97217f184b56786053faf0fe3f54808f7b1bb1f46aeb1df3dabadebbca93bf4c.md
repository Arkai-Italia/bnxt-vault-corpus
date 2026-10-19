---
id: "bnxt-97217f184b56786053faf0fe3f54808f7b1bb1f46aeb1df3dabadebbca93bf4c"
titolo: "Structural optimization principles for edge AI in motorsport telemetry | Scientific Reports"
url: "https://doi.org/10.1038/s41598-026-49736-0"
categoria: "10_AI_LLM_telemetria"
stato: "RAG_FONTI"
verifica_fatti: "NON VERIFICATO"
fonti_originali_locali: ["E:\\LIBRI BNXT COLLEGAMENTI GEN\\ORIGINALI_NON_TRATTATI\\FILE\\GEN__WEB_bdad9103460bbcbe_20261016T101905878034Z.html"]
sha256_corpo: "1c34a85bb4210ceab93fde7e5b2d1d7e65a344b0c975b575db620f4f27829d48"
---

# Structural optimization principles for edge AI in motorsport telemetry | Scientific Reports

[Download PDF](https://doi.org/articles/s41598-026-49736-0.pdf)

## Abstract

In motorcycle motorsport, engineering decisions are constrained by mass, stiffness, fatigue life, and thermal margins. In a comparable way, deploying large language models (LLMs) for edge or trackside assistance is limited by memory capacity, bandwidth, latency, and power. This paper presents a systems-engineering framework that relates structural lightweighting and post-training quantization (PTQ) through second-order sensitivity. In mechanics, safe material removal is governed by the stiffness matrix \textbf{K}; in neural networks, safe precision reduction is governed by curvature information encoded in the loss Hessian \textbf{H}. To ground this analogy, we combine a structural case study based on topology optimization of a Formula Student upright with an edge-oriented evaluation of low-precision LLM deployment using established PTQ pipelines (GPTQ and AWQ). The structural example illustrates how sensitivity-guided material removal preserves load paths and mechanical integrity, while the LLM study examines how sensitivity-aware quantization can reduce computational footprint while maintaining operational usefulness in telemetry-related inference. To assess deployment feasibility, we define two practical metrics: a Digital Factor of Safety (\textrm{FoS}\_{\textrm{dig}}), based on a perplexity-derived integrity threshold, and Intelligence-per-Watt (IPW), which captures energy-aware inference efficiency. For a 32B-class operating point, INT4 weight-only deployment reduces memory footprint from 61 GB to 18 GB, improves throughput from 26.0 to 69.9 tok/s, and lowers measured power from 295 W to 165 W while remaining within the proposed integrity envelope. The contribution is methodological rather than algorithmic: we do not introduce a new quantization method, but a reproducible framework for analyzing when low-precision LLM deployment becomes feasible under motorsport-inspired resource constraints. The results support the practical relevance of stiffness-informed digital lightweighting, while also highlighting that the evidence is limited to the structural and computational case studies considered here.

### Similar content being viewed by others

### [Deploying TinyML for energy-efficient object detection and communication in low-power edge AI systems](https://www.nature.com/articles/s41598-025-27818-9?fromPaywallRec=false)

Article
Open access
05 December 2025

### [The impact of LLM chatbots on learning outcomes in advanced driver assistance systems education](https://www.nature.com/articles/s41598-025-91330-3?fromPaywallRec=false)

Article
Open access
01 March 2025

### [Efficient self-attention with smart pruning for sustainable large language models](https://www.nature.com/articles/s41598-025-92586-5?fromPaywallRec=false)

Article
Open access
24 March 2025

### Explore related subjects

Discover the latest articles and news in related subjects.

* [Engineering](https://doi.org/subjects/engineering)
* [Mathematics and computing](https://doi.org/subjects/mathematics-and-computing)
* [Energy-Efficient Deep Learning Architectures](https://doi.org/subjects/energy-efficient-deep-learning-architectures)

## Introduction

Modern racing motorcycles are high-velocity cyber-physical systems (CPS) in which rider, chassis, tires, and electronics form a tightly coupled dynamical loop. Unlike many four-wheel platforms, competitive motorcycles must preserve stability at extreme lean angles under high vibration and tight packaging constraints, while the rider remains both a time-varying mass and the dominant controller in the loop[1](https://doi.org/articles/s41598-026-49736-0#ref-CR1),[2](https://doi.org/articles/s41598-026-49736-0#ref-CR2). These conditions create two concurrent engineering pressures: (i) reducing physical mass without compromising structural integrity, stiffness, or fatigue life, and (ii) reducing decision latency without destabilizing the broader supervisory loop.

The embedded environment is mechanically and thermally demanding. Motorsport ECUs and acquisition units must tolerate sustained vibration, elevated internal temperatures, and sealed packaging that constrains both power budgets and heat rejection[3](https://doi.org/10.1038/s41598-026-49736-0#ref-CR3),[4](https://doi.org/10.1038/s41598-026-49736-0#ref-CR4),[5](https://doi.org/articles/s41598-026-49736-0#ref-CR5). At the same time, premier-class machines integrate dense sensing and control layers—including IMU-based estimation, traction and wheelie control, ride-by-wire, and map management—that generate high-rate telemetry streams whose interpretation often needs to occur within short garage or post-run windows[5](https://doi.org/articles/s41598-026-49736-0#ref-CR5). Because live air-to-ground telemetry is restricted in many competitive contexts, useful engineering intelligence must often be generated close to the data, either at the edge or immediately after the run[6](https://doi.org/articles/s41598-026-49736-0#ref-CR6).

Edge AI is attractive in this setting because it can transform multi-channel telemetry into compact, actionable engineering language. However, large language model (LLM) inference is frequently memory-bound: in common decoding regimes, the working set of weights dominates bandwidth and capacity, making the *memory wall* a practical constraint for embedded deployment[7](https://doi.org/10.1038/s41598-026-49736-0#ref-CR7),[8](https://doi.org/10.1038/s41598-026-49736-0#ref-CR8),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9). This constraint is the digital analogue of a motorsport mass budget: excessive “digital mass”—in the form of high-precision weights, memory traffic, and runtime overhead—can render deployment infeasible under strict thermal, packaging, and cost envelopes. From this perspective, low-precision deployment is not merely an efficiency option, but a design strategy for embedded viability.

***A systems-engineering correspondence.*** This paper examines a systems-engineering correspondence between mechanical lightweighting and post-training quantization (PTQ). In structural mechanics, safe material removal is guided by sensitivity information encoded in the stiffness matrix \textbf{K}. In neural networks, safe precision reduction is guided by curvature or saliency information encoded in the loss Hessian \textbf{H}. On this basis, we ask whether sensitivity-guided structural optimization can serve as an analytical lens for sensitivity-guided quantization. The formal development of this correspondence is presented in Section 2.4.1, while Section 2.4.3 discusses the geometric interpretation of GPTQ as a nearest-plane approximation under a curvature-induced metric[10](https://doi.org/10.1038/s41598-026-49736-0#ref-CR10),[11](https://doi.org/10.1038/s41598-026-49736-0#ref-CR11),[12](https://doi.org/articles/s41598-026-49736-0#ref-CR12). Figure [1](https://doi.org/articles/s41598-026-49736-0#Fig1) summarizes this conceptual bridge. Importantly, the framework is not intended to claim constitutive equivalence between the two domains, but rather to provide a disciplined comparison of how second-order sensitivity governs resource removal under performance constraints.

This type of sensitivity-guided design is already familiar in sports engineering. Prior studies have used measured loads and telemetry-informed scenarios to optimize Formula Student uprights[13](https://doi.org/articles/s41598-026-49736-0#ref-CR13) and suspension bell-cranks[14](https://doi.org/articles/s41598-026-49736-0#ref-CR14), showing that in high-performance environments, material removal is not heuristic but driven by load paths, stiffness retention, and durability constraints. In the present work, we extend this logic from the physical chassis to the *digital chassis*, where the objective is not to remove material but to remove excessive numerical precision while preserving operational usefulness.

At the same time, previous work in LLM compression has primarily emphasized quantization accuracy, throughput, and memory reduction, whereas previous work in structural optimization has emphasized mass, stiffness, and fatigue preservation. What remains less developed is a deployment-oriented framework that connects both domains through a shared second-order sensitivity logic and evaluates feasibility using acceptance-style metrics relevant to motorsport workflows, such as integrity margin, power-aware efficiency, and short decision-window usability. In this sense, the contribution of the present study is methodological rather than algorithmic: we do not propose a new quantization method or a new structural optimizer, but a cross-domain framework for analyzing when low-precision LLM deployment becomes feasible under motorsport-inspired resource constraints.

### The motorsport paradigm

The motorsport philosophy of “adding lightness” seeks to remove non-functional mass while preserving the properties that govern controllability and durability, including stiffness, modal behavior, and fatigue life. Topology optimization methods such as BESO provide a formal route for retaining critical load paths while removing low-contribution material[13](https://doi.org/articles/s41598-026-49736-0#ref-CR13),[15](https://doi.org/articles/s41598-026-49736-0#ref-CR15),[16](https://doi.org/articles/s41598-026-49736-0#ref-CR16). Following Angeles[17](https://doi.org/articles/s41598-026-49736-0#ref-CR17), the stiffness matrix can be interpreted as a second-order energy object whose eigenstructure reveals mechanically soft directions, thereby motivating sensitivity-guided removal under explicit integrity constraints. In this paper, that physical logic is grounded by a structural case study based on the topology optimization of a Formula Student upright.

### The edge AI paradigm: hessian-aware quantization

Scaling laws suggest that model capability often increases with size, yet embedded feasibility remains constrained by memory traffic, available capacity, and power envelopes[8](https://doi.org/articles/s41598-026-49736-0#ref-CR8),[18](https://doi.org/articles/s41598-026-49736-0#ref-CR18),[19](https://doi.org/articles/s41598-026-49736-0#ref-CR19). Second-order and activation-aware PTQ methods, such as GPTQ and AWQ, use curvature and importance signals to preserve function while discretizing weights, which is conceptually analogous to preserving critical load paths while removing physical mass[10](https://doi.org/articles/s41598-026-49736-0#ref-CR10),[11](https://doi.org/articles/s41598-026-49736-0#ref-CR11). Kernel-level realization is the implementation counterpart that determines whether bit-width reduction actually translates into practical throughput and latency gains under batch-1 decoding[20](https://doi.org/articles/s41598-026-49736-0#ref-CR20).

### The convergence hypothesis

To make this correspondence empirically examinable, the study is organized around four falsifiable hypotheses:

***H1 (Isomorphism of sensitivity).*** If physical stiffness \textbf{K} acts as a second-order energy object and the loss Hessian \textbf{H} governs error sensitivity, then sensitivity-guided PTQ should be structurally isomorphic—at the level of design logic rather than physical mechanism—to sensitivity-guided topology optimization.

***H2 (Geometric interpretation of GPTQ).*** If GPTQ can be interpreted as a nearest-plane lattice projection under a metric induced by \textbf{H}, then it provides a more principled reconstruction rule for quantized weights than magnitude-only heuristics.

***H3 (Operational viability via INT4).*** If inference remains memory-bound under motorsport-style embedded constraints, then INT4-class weight-only pathways are expected to be required to satisfy feasibility budgets while remaining within an acceptable inference-integrity envelope.

***H4 (The edge window).*** Given telemetry restrictions and short decision windows, kernel-realized INT4 pipelines may enable practical semantic assistance close to the data within strict time budgets.

### Contributions

The main contributions of this paper are as follows:

1. 1.

A cross-domain mapping between topology optimization and PTQ grounded in a \textbf{K}\leftrightarrow \textbf{H} second-order sensitivity interpretation (Section 2.4.1).
2. 2.

A deployment-oriented methodology linking BESO-style sensitivity-guided material removal to Hessian-/activation-aware PTQ (GPTQ/AWQ) and kernel realization for embedded inference under motorsport constraints[10](https://doi.org/articles/s41598-026-49736-0#ref-CR10),[20](https://doi.org/articles/s41598-026-49736-0#ref-CR20).
3. 3.

A structural case study based on the topology optimization of a Formula Student upright, used to illustrate topology evolution, load-path preservation, and the limits of generalization across structural configurations.
4. 4.

A perplexity-grounded Digital Factor of Safety (\textrm{FoS}\_{\textrm{dig}}) and an Intelligence-per-Watt (IPW) framing to quantify deployment integrity and energy-aware efficiency.
5. 5.

A paired computational case study showing that INT4 LLM deployment can achieve substantial memory and power reduction while remaining within the defined inference-integrity envelope for the operating conditions considered.

### Paper organization

The remainder of the paper is organized as follows. Section 2 develops the theoretical correspondence between structural lightweighting and post-training quantization, including the \textbf{K}\leftrightarrow \textbf{H} sensitivity interpretation and the geometric view of GPTQ. Section 3 describes the structural and computational methodology, measurement protocol, and deployment conditions. Section 4 presents the empirical results and hypothesis-oriented evaluation, including the structural case study and the low-precision inference results. Section 5 discusses implications, limitations, generalizability, and threats to validity. Finally, Section 6 concludes the paper and outlines future work.

**Fig. 1**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/1)

Conceptual correspondence between mechanical lightweighting and digital lightweighting. In both domains, strict operating constraints motivate the selective removal of non-critical resources, guided by second-order sensitivity objects: the stiffness matrix (\textbf{K}) in structural mechanics and the Hessian matrix (\textbf{H}) in neural-network quantization. The framework is used here as a systems-engineering lens for deployment analysis rather than as a claim of constitutive identity between the two domains.

## Theoretical framework and cross-domain sensitivity mapping

The central premise of this paper is that *digital lightweighting*—that is, LLM quantization for embedded or edge deployment—can be examined using a systems-engineering logic that is comparable, at the level of constrained design, to *physical lightweighting* in motorcycle motorsport. In both domains, feasibility depends on *function-preserving reduction under binding constraints*: in mechanics, stiffness, modal margins, fatigue life, and manufacturability; in inference, semantic integrity, latency, memory bandwidth, and power dissipation. The purpose of this section is therefore not to claim a constitutive identity between structure and computation, but to establish a common analytical language based on second-order sensitivity. In structural mechanics, this role is played by the stiffness matrix \textbf{K}; in optimization and quantization, it is played by the curvature matrix \textbf{H}[21](https://doi.org/articles/s41598-026-49736-0#ref-CR21),[22](https://doi.org/articles/s41598-026-49736-0#ref-CR22).

The framework is developed through two complementary arguments. First, we formalize a second-order correspondence between \textbf{K} and \textbf{H} using potential-energy interpretations in mechanics and local quadratic approximations in optimization[17](https://doi.org/articles/s41598-026-49736-0#ref-CR17). Second, we interpret Hessian-aware quantization, particularly GPTQ, through a geometric view as a nearest-plane approximation to a Closest Vector Problem (CVP) under a curvature-induced metric[10](https://doi.org/articles/s41598-026-49736-0#ref-CR10). This theoretical framing supports the paired structural and computational methodology introduced in Section 3 and the operating-point analysis reported in Section 4.

### Positioning relative to prior work and research gap

Existing work in structural lightweighting and in post-training quantization has matured along largely separate trajectories. In motorsport and structural optimization, emphasis is placed on mass reduction under explicit stiffness, stress, and fatigue constraints[13](https://doi.org/10.1038/s41598-026-49736-0#ref-CR13),[14](https://doi.org/10.1038/s41598-026-49736-0#ref-CR14),[15](https://doi.org/10.1038/s41598-026-49736-0#ref-CR15),[16](https://doi.org/articles/s41598-026-49736-0#ref-CR16). In LLM compression, the dominant focus is on preserving model quality while reducing memory footprint and improving inference efficiency, as seen in methods such as LLM.int8, SmoothQuant, GPTQ, and AWQ[10](https://doi.org/articles/s41598-026-49736-0#ref-CR10). What remains less developed is a deployment-oriented framework that connects both domains through a shared sensitivity logic and evaluates low-precision inference using acceptance-style criteria relevant to motorsport operation: integrity margin, power-aware efficiency, and short decision-window feasibility.

Table [1](https://doi.org/articles/s41598-026-49736-0#Tab1) summarizes this positioning. The objective is not to argue that prior methods are insufficient in their original contexts, but to clarify the specific gap addressed here: a systems-engineering framework for judging when aggressive low-precision LLM deployment becomes feasible under edge constraints inspired by motorsport telemetry workflows. In addition, because Reviewer 1 requested stronger grounding on the structural side, the present study explicitly complements the analytical mapping with a concrete structural case study, rather than relying on the analogy alone.

**Table 1 Critical comparison of related work and research gap. The proposed contribution is positioned as methodological and deployment-oriented rather than as a new quantization algorithm or a new structural optimizer.**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/1)

### Structural materials vs. numeric data types

In motorcycle motorsport, materials are not “best overall”; they are best *under constraints*, including stiffness-to-weight ratio, fatigue resistance, manufacturability, and cost. A similar engineering logic applies to numeric formats in computation. Floating-point and integer data types act as computational materials: they trade dynamic range, precision, numerical stability, memory footprint, and hardware acceleration[8](https://doi.org/articles/s41598-026-49736-0#ref-CR8),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9). This choice is especially important for Transformer inference because autoregressive decoding is often bottlenecked by memory movement rather than arithmetic throughput. In such regimes, shrinking the weight representation directly reduces bandwidth pressure[7](https://doi.org/articles/s41598-026-49736-0#ref-CR7),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9).

Table [2](https://doi.org/articles/s41598-026-49736-0#Tab2) summarizes a pragmatic hierarchy aligned with deployed AI systems. The key point is that bit-width directly governs (i) weight footprint and, therefore, (ii) the lower bound on weight traffic per generated token in memory-bound decoding.

**Table 2 Numeric formats as engineering materials (specification snapshot). *S*/*E*/*M* denotes sign/exponent/mantissa bits (where applicable). The key engineering point is that bit-width directly sets weight footprint and therefore memory traffic in weight-dominant inference[7](https://doi.org/10.1038/s41598-026-49736-0#ref-CR7),[8](https://doi.org/10.1038/s41598-026-49736-0#ref-CR8),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9),[23](https://doi.org/articles/s41598-026-49736-0#ref-CR23).**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/2)

***Why bit-width binds latency: a memory-traffic lower bound.*** In autoregressive decoding, the model repeatedly applies layer weights to the current token state. A conservative lower bound on per-token decode time is

\begin{aligned} t\_{\text {decode}} \;\gtrsim \; \frac{\sum \_{\ell =1}^{L} |\textbf{W}\_{\ell } |}{B\_{\text {eff}}}, \end{aligned}

(1)

where |\textbf{W}\_{\ell } | is the byte footprint of layer \ell along the decode path and B\_{\text {eff}} is the effective memory bandwidth delivered to the decode kernel after contention, cache effects, and scheduling losses. Quantization reduces |\textbf{W}\_{\ell } | approximately linearly with bit-width until the operating point transitions from memory-bound to compute-bound behavior, in the Roofline sense[7](https://doi.org/articles/s41598-026-49736-0#ref-CR7),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9). In long-context settings, KV-cache traffic also becomes relevant; however, reducing weight traffic remains a necessary, though not sufficient, condition for feasibility on bandwidth- and thermally-limited embedded platforms[8](https://doi.org/articles/s41598-026-49736-0#ref-CR8),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9).

Figure [2](https://doi.org/articles/s41598-026-49736-0#Fig2) visualizes this first-order footprint scaling and explains why low-precision deployment should be treated as a structural feasibility lever rather than as a purely post hoc compression step.

**Fig. 2**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/2)

Systems-engineering trade-off of numeric precision. The bars (left axis) show the reduction in weight memory traffic (\propto |\textbf{W} |) relative to FP32. The line (right axis) shows the theoretical throughput multiplier in memory-bound regimes, following Eq. [1](https://doi.org/articles/s41598-026-49736-0#Equ1). The transition to 4-bit formats (INT4/FP4-class) provides an 8\times reduction in bandwidth pressure with respect to FP32, which explains why such regimes become especially attractive for edge deployment.

### Topology optimization and the stiffness matrix \textbf{K}

Topology optimization removes non-load-bearing material while preserving stiffness and constraint margins by reinforcing critical load paths[15](https://doi.org/articles/s41598-026-49736-0#ref-CR15),[16](https://doi.org/articles/s41598-026-49736-0#ref-CR16). In linear elasticity,

\begin{aligned} \textbf{F}=\textbf{K}\textbf{u}, \end{aligned}

(2)

where \textbf{F} is the global load vector, \textbf{u} is the nodal displacement vector, and \textbf{K} is the assembled stiffness matrix[21](https://doi.org/articles/s41598-026-49736-0#ref-CR21). A standard objective is compliance minimization under a volume constraint:

\begin{aligned} \min \_{\varvec{\rho }} \;\;&C(\varvec{\rho })=\textbf{F}^\top \textbf{u} \quad \text {s.t.}\quad \textbf{K}(\varvec{\rho })\textbf{u}=\textbf{F}, \;\; V(\varvec{\rho })\le V\_0, \end{aligned}

(3)

where \varvec{\rho } parameterizes material presence or density. In evolutionary schemes such as BESO, design updates are driven by element-level sensitivity indicators linked to strain energy density:

\begin{aligned} e\_i \;=\; \tfrac{1}{2}\,\textbf{u}\_i^\top \textbf{K}\_i \textbf{u}\_i, \end{aligned}

(4)

so that removal or addition decisions preserve the global response—stiffness, stress, and modal margins—while reducing mass[15](https://doi.org/articles/s41598-026-49736-0#ref-CR15),[16](https://doi.org/articles/s41598-026-49736-0#ref-CR16). The engineering significance is that lightweighting is not arbitrary subtraction, but constrained subtraction guided by second-order response information.

In the present study, this structural logic is not kept at a purely abstract level. It is later grounded through a concrete topology-optimization case study of a Formula Student upright, which is used to illustrate baseline geometry, optimization progression, and pre/post structural performance. This addition is important because the proposed cross-domain mapping is intended to be supported by an explicit structural example rather than by conceptual comparison alone.

### Quantization and the curvature matrix \textbf{H}

In deep learning, the analogue of structural deformation is the degradation in decision quality induced by perturbing model parameters. Second-order sensitivity is captured by curvature, written as \textbf{H}=\nabla ^2\_{\textbf{w}}\mathcal {L}[22](https://doi.org/articles/s41598-026-49736-0#ref-CR22),[24](https://doi.org/articles/s41598-026-49736-0#ref-CR24). Classical pruning results such as Optimal Brain Damage and Optimal Brain Surgeon connect curvature structure to minimal-loss perturbations and provide the theoretical basis for second-order saliency[25](https://doi.org/articles/s41598-026-49736-0#ref-CR25),[26](https://doi.org/articles/s41598-026-49736-0#ref-CR26). Under a local quadratic approximation,

\begin{aligned} \Delta \mathcal {L} \;\approx \; \tfrac{1}{2}\,\Delta \textbf{w}^\top \textbf{H}\,\Delta \textbf{w}, \end{aligned}

(5)

which makes explicit that compressing or quantizing weights is safest along low-sensitivity directions.

***Curvature is not always strictly SPD in deep networks (practical metric view).*** Although the exact Hessian of deep networks can be indefinite, Hessian-aware PTQ methods typically operate with locally positive semidefinite or stabilized curvature proxies, such as damped block-Hessian approximations, Gauss–Newton surrogates, Fisher-like matrices, or numerically regularized Cholesky factors. These approximations are sufficient to support a valid local metric interpretation and stable quantization updates in practice[22](https://doi.org/articles/s41598-026-49736-0#ref-CR22),[24](https://doi.org/10.1038/s41598-026-49736-0#ref-CR24),[25](https://doi.org/10.1038/s41598-026-49736-0#ref-CR25),[26](https://doi.org/articles/s41598-026-49736-0#ref-CR26). Hessian-aware PTQ methods, particularly GPTQ, exploit this structure to quantize weights while controlling the induced loss increase[10](https://doi.org/articles/s41598-026-49736-0#ref-CR10).

#### Second-order energy perturbation: compliance vs. loss increase

The \textbf{K}\leftrightarrow \textbf{H} correspondence becomes explicit when both domains are written as *second-order penalties* under small perturbations.

In linear elasticity, the total strain energy is

\begin{aligned} U(\textbf{u})=\tfrac{1}{2}\,\textbf{u}^{\top }\textbf{K}\textbf{u}, \end{aligned}

(6)

and, using the equilibrium relation \textbf{K}\textbf{u}=\textbf{F}, compliance satisfies

\begin{aligned} C=\textbf{F}^{\top }\textbf{u}=\textbf{u}^{\top }\textbf{K}\textbf{u}=2U. \end{aligned}

(7)

Minimizing compliance under a volume constraint is therefore equivalent to minimizing stored energy under load while removing material[15](https://doi.org/articles/s41598-026-49736-0#ref-CR15),[21](https://doi.org/articles/s41598-026-49736-0#ref-CR21).

In PTQ, the corresponding perturbation is the quantization error \Delta \textbf{w}=\hat{\textbf{w}}-\textbf{w}. Under a local quadratic loss model, the induced degradation is the *second-order reconstruction penalty*

\begin{aligned} \Delta \mathcal {L}\;\approx \;\tfrac{1}{2}\,\Delta \textbf{w}^{\top }\textbf{H}\,\Delta \textbf{w}, \end{aligned}

(8)

which makes the design logic parallel: reduce “material” in the digital sense—effective precision and stored bytes—while bounding the system-level penalty in loss space[10](https://doi.org/articles/s41598-026-49736-0#ref-CR10),[25](https://doi.org/articles/s41598-026-49736-0#ref-CR25),[26](https://doi.org/articles/s41598-026-49736-0#ref-CR26). The analogy is therefore structural rather than rhetorical: both problems are governed by second-order penalties that determine which perturbations remain acceptable under constraints.

#### Eigenvalue spectra: soft modes and compressible directions

Rigidity analysis uses eigenstructure to expose *soft directions*. Stiffness can be studied through generalized eigenproblems such as \textbf{K}\varvec{\phi }=\lambda \,\textbf{M}\varvec{\phi } or through Cartesian stiffness matrices whose eigenscrews identify principal directions of rigidity and softness[17](https://doi.org/articles/s41598-026-49736-0#ref-CR17). Small stiffness eigenvalues correspond to directions in which perturbations generate relatively large motion for little stored energy.

The same interpretation applies to \textbf{H}. If \textbf{H}\textbf{v}\_i=\mu \_i\textbf{v}\_i and \Delta \textbf{w}=\sum \_i z\_i\textbf{v}\_i, then Eq. ([8](https://doi.org/articles/s41598-026-49736-0#Equ8)) becomes

\begin{aligned} \Delta \mathcal {L}\approx \tfrac{1}{2}\sum \_i \mu \_i z\_i^2, \end{aligned}

(9)

so directions associated with small \mu \_i are flatter and therefore more tolerant to discretization. This provides a precise interpretation of “compressible directions” in parameter space and motivates curvature-aware quantization as the digital counterpart of load-path-aware lightweighting.

#### Geometric interpretation: GPTQ as a nearest-plane projection

While the full algebraic derivation is provided in **Appendix A**, curvature-aware quantization also admits a geometric interpretation that is directly useful for systems engineering. Standard rounding implicitly assumes an isotropic error metric, treating all perturbations as equally damaging. In practice, however, the local loss landscape is anisotropic and shaped by \textbf{H}: under the quadratic model, equal loss increase corresponds to an elliptic error set aligned with the eigenvectors of \textbf{H} (Fig. [3](https://doi.org/articles/s41598-026-49736-0#Fig3)).

From this viewpoint, GPTQ is better understood not as simple rounding, but as a *metric projection* onto the quantization lattice. Under a curvature-induced metric, the problem becomes an approximate Closest Vector Problem (CVP), which can be expressed through a Cholesky factorization \textbf{H}=\textbf{R}^\top \textbf{R} and related to a nearest-plane form[10](https://doi.org/articles/s41598-026-49736-0#ref-CR10). This interpretation does not imply that GPTQ is globally optimal in all settings; rather, it explains why curvature-aware reconstruction is more principled than independent magnitude-based rounding when aggressive low-bit deployment is required.

***Geometric intuition (why simple rounding is suboptimal).*** Figure [3](https://doi.org/articles/s41598-026-49736-0#Fig3) provides an operational interpretation of Eqs. ([16](https://doi.org/articles/s41598-026-49736-0#Equ16)) and ([17](https://doi.org/articles/s41598-026-49736-0#Equ17)) in Appendix A. Under PTQ, admissible distortion is not spherical in weight space: \textbf{H} induces an anisotropic geometry in which some directions are substantially more sensitive than others. Simple independent rounding implicitly assumes an isotropic metric and may therefore move the solution toward a point that is close in Euclidean distance but suboptimal under the curvature-weighted objective (\textbf{w}-\textbf{q})^\top \textbf{H}(\textbf{w}-\textbf{q}). By contrast, GPTQ redistributes residual along less sensitive directions, yielding a quantized point that is more consistent with the induced metric and therefore more suitable for aggressive INT4 deployment.

**Fig. 3**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/3)

Curvature-induced geometry of PTQ. The Hessian defines an anisotropic error region (ellipse): naive rounding can select a point that is locally nearest in an isotropic sense but suboptimal under the curvature-weighted objective, whereas GPTQ performs a curvature-aware projection consistent with the induced metric.

### Implications for on-track “Edge Windows”

Premier-class motorcycle racing imposes a distinctive operational envelope: data are generated on-bike at high rate, while live air-to-ground telemetry is restricted, forcing decision support to be produced locally and/or within short garage windows immediately after runs[6](https://doi.org/articles/s41598-026-49736-0#ref-CR6). This context sharpens the practical meaning of Eq. ([1](https://doi.org/articles/s41598-026-49736-0#Equ1)): for an autoregressive LLM, the dominant latency term is often repeated streaming of weight matrices from memory. The relevant engineering question is therefore not only *how many FLOPs* are available, but also *how many bytes per token* can traverse the memory hierarchy within the available time budget[7](https://doi.org/articles/s41598-026-49736-0#ref-CR7),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9).

Let B\_{\text {eff}} denote the effective bandwidth delivered to the decode kernel. Reducing the weight footprint by a factor \alpha yields, in the memory-dominant regime, an approximately proportional reduction in the decode-time lower bound:

\begin{aligned} t\_{\text {decode}}^{(\alpha )} \;\gtrsim \; \frac{\alpha \,\sum \_{\ell =1}^{L}|\textbf{W}\_{\ell }|}{B\_{\text {eff}}} \quad \text {with}\quad \alpha \approx \frac{\text {bits}\_{\text {new}}}{\text {bits}\_{\text {base}}}. \end{aligned}

(10)

For example, moving from BF16/FP16 (16-bit) to INT4 (4-bit) targets \alpha \approx 1/4 in a weight-only setting, which is the scale of reduction often needed when inference must be executed close to the bike without relying on cloud connectivity.

### Inference integrity envelope and systems efficiency metrics

Motorsport components are designed against failure limits using explicit margins. For embedded LLMs, an analogous requirement is to bound the risk of inference degradation under aggressive quantization. We therefore define a *Digital Factor of Safety* based on a task-dependent failure threshold expressed through perplexity degradation:

\begin{aligned} \textrm{FoS}\_{\textrm{dig}}= \frac{\textrm{PPL}\_{\textrm{collapse}}-\textrm{PPL}\_{\textrm{base}}}{\textrm{PPL}\_{\textrm{quant}}-\textrm{PPL}\_{\textrm{base}}}, \end{aligned}

(11)

where \textrm{PPL}\_{\text {base}} is the baseline perplexity, \textrm{PPL}\_{\text {quant}} is the quantized perplexity, and \textrm{PPL}\_{\text {collapse}} denotes an operational failure point beyond which outputs are considered unreliable. Importantly, \textrm{PPL}\_{\text {collapse}} is not universal: it must be calibrated for the target task and risk tolerance. In this paper, it is used as a deployment-oriented integrity threshold rather than as a claim of a universal semantic-failure constant.

To connect integrity with embedded thermodynamic constraints, we also define an *Intelligence-per-Watt* (IPW) objective:

\begin{aligned} \textrm{IPW}=\frac{\text {Integrity}\times \text {Throughput}}{P}, \end{aligned}

(12)

where Integrity is instantiated through perplexity-based retention (or, more generally, an application-specific utility score), Throughput is measured in tokens/s, and *P* is measured power draw in watts. In motorsport terms, IPW plays a role analogous to power-to-weight ratio: the feasible operating set is constrained jointly by memory traffic, delivered throughput, and heat dissipation. Together, \textrm{FoS}\_{\textrm{dig}} and IPW provide the acceptance-style metrics used later to judge whether a low-precision operating point is merely compact or genuinely deployable.

***Scope and limitation of the theoretical mapping.*** The purpose of the \textbf{K}\leftrightarrow \textbf{H} mapping is to provide a disciplined cross-domain framework for reasoning about sensitivity-guided reduction under constraints. It does not imply that structural optimization and PTQ are identical optimization problems, nor that results obtained in one domain automatically generalize to all configurations in the other. For this reason, the present work combines the theoretical mapping with a concrete structural case study and a separate computational deployment study, and later discusses the extent to which the resulting conclusions may generalize.

## Methodology for paired structural and digital lightweighting

The methodological design of this study has two coordinated branches. The first is a *structural branch*, used to ground the physical side of the proposed correspondence through a concrete topology-optimization case study of a Formula Student upright. The second is a *digital branch*, used to evaluate low-precision LLM deployment under edge-oriented constraints using established post-training quantization (PTQ) toolchains. The purpose of this section is methodological rather than algorithmic: we do not introduce a new structural optimizer or a new quantization algorithm, but a reproducible workflow for comparing sensitivity-guided reduction in both domains under constrained operating conditions.

The digital branch is motivated by embedded environments in which inference is frequently memory-bound[7](https://doi.org/10.1038/s41598-026-49736-0#ref-CR7),[8](https://doi.org/10.1038/s41598-026-49736-0#ref-CR8),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9) and in which efficiency must be treated as a first-class systems objective[27](https://doi.org/articles/s41598-026-49736-0#ref-CR27),[28](https://doi.org/articles/s41598-026-49736-0#ref-CR28). The structural branch is motivated by the fact that Reviewer 1 requested a concrete numerical example showing topology evolution, structural morphology before and after optimization, and the progression of the optimization process. For this reason, the cross-domain framework is supported here not only by analytical arguments but also by an explicit structural case study.

### Overall study design

The study is organized as a paired methodology:

1. 1.

**Structural branch:** a topology-optimization case study of a Formula Student upright is used to show how sensitivity-guided material removal preserves load paths and structural integrity while reducing mass.
2. 2.

**Digital branch:** a deployment-oriented PTQ workflow is used to assess when aggressive low-precision LLM inference becomes feasible under motorsport-inspired resource constraints.
3. 3.

**Cross-domain comparison:** the two branches are interpreted through the shared second-order sensitivity framework introduced in Section 2, with stiffness \textbf{K} and curvature \textbf{H} acting as the respective sensitivity objects.

This design is intended to prevent the cross-domain mapping from remaining purely conceptual. Instead, the proposed correspondence is examined through a concrete physical example and a separate computational deployment study, each evaluated with domain-appropriate metrics.

### Structural case study: topology optimization of a formula student upright

To ground the mechanical side of the framework, we include a structural case study based on the topology optimization of a Formula Student upright. The upright was selected because it is a load-bearing motorsport component in which mass reduction is desirable but strongly constrained by stiffness, stress, fatigue life, and manufacturability. This makes it an appropriate physical counterpart to the digital problem of reducing parameter precision without unacceptable loss of inference utility.

#### Baseline geometry, material, and loading conditions

The baseline geometry was created in SolidWorks 2023 and analyzed in ANSYS Workbench 2023. The starting configuration corresponds to a full-mass reference design with an initial mass of 2.45 kg. The component material was fixed as Aluminum 7075-T6 to isolate the effect of topology optimization from any concurrent material substitution. The material model uses a Young’s modulus of 71 GPa, yield strength of 503 MPa, ultimate tensile strength of 572 MPa, density of 2.81 g/cm^3, and a fatigue limit of 159 MPa at 10^8 cycles.

The loading scenarios were derived from a representative Formula Student vehicle with a total mass of 350 kg, 50:50 weight distribution, and a wheelbase of 1675 mm. The structural evaluation includes braking and cornering as the principal critical load cases. In the braking case, the upright is subjected to a total braking load of 3538.9 N after accounting for longitudinal load transfer. In the cornering case, the critical reaction used for optimization is 4649.56 N at the lower A-arm, representing the dominant load path for lateral force transfer. These load cases are used to evaluate stress, displacement, and durability margins before and after optimization (Table [3](https://doi.org/articles/s41598-026-49736-0#Tab3)).

**Table 3 Structural case-study setup for the Formula Student upright.**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/3)

#### BESO workflow, convergence settings, and manufacturability controls

Topology optimization was performed using the Bidirectional Evolutionary Structural Optimization (BESO) method. The purpose of the optimization was to remove structurally inefficient material while preserving stiffness and durability under the imposed load cases. The solver was configured with a mass-retention interval of 30–70%, a convergence tolerance of 0.1%, a stiffness penalization factor of 3, a maximum of 500 iterations, and a minimum normalized density of 1.0\times 10^{-3}. To reduce numerical bias, a mesh-sensitivity study was conducted over element sizes ranging from 0.17 mm to 2.8 mm.

The optimization trajectory was documented through four representative stages:

1. 1.

**Baseline full-mass design,** corresponding to the initial solid geometry.
2. 2.

**Early material-removal stage,** where low-sensitivity regions begin to disappear.
3. 3.

**Intermediate topology stage,** where the dominant internal load paths become visible.
4. 4.

**Final refined topology,** post-processed with minimum-thickness and fillet constraints for CNC manufacturability.

This staged recording is important because it provides the step-by-step structural progression requested by Reviewer 1, rather than only a baseline/final comparison. The structural outputs retained for later comparison are mass, von Mises stress, total deformation, and fatigue-life classification (Fig. [4](https://doi.org/articles/s41598-026-49736-0#Fig4)).

**Fig. 4**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/4)

Topology-evolution sequence for the Formula Student upright. The structural branch records the progression from the initial full-mass design to the final manufacturable topology, making explicit how low-sensitivity material is removed while the dominant load paths are retained. (**a**) Baseline, (**b**) Early removal, (**c**) Intermediate, (**d**) Final.

### Problem formulation: quantization as sensitivity-guided removal

Let \textbf{w} denote pretrained weights and \hat{\textbf{w}}\in \mathcal {Q}(\textbf{w}) their quantized representation in a discrete set \mathcal {Q}. Following the second-order framework introduced in Section 2, the induced loss increase under PTQ is approximated by a quadratic penalty:

\begin{aligned} \Delta \mathcal {L} \;\approx \; \tfrac{1}{2}(\textbf{w}-\hat{\textbf{w}})^\top \textbf{H}\,(\textbf{w}-\hat{\textbf{w}}), \end{aligned}

(13)

where \textbf{H} is a Hessian or curvature proxy capturing local sensitivity[22](https://doi.org/articles/s41598-026-49736-0#ref-CR22),[24](https://doi.org/articles/s41598-026-49736-0#ref-CR24). This mirrors structural lightweighting: safe material removal is guided by second-order system sensitivity encoded in system matrices (stiffness \textbf{K} in mechanics; curvature \textbf{H} in learning)[17](https://doi.org/articles/s41598-026-49736-0#ref-CR17),[21](https://doi.org/articles/s41598-026-49736-0#ref-CR21),[25](https://doi.org/articles/s41598-026-49736-0#ref-CR25),[26](https://doi.org/articles/s41598-026-49736-0#ref-CR26). The practical implication is that *naive uniform rounding* is analogous to removing material without respecting load paths. It is simple, but becomes increasingly unreliable under aggressive constraints.

***Methodological scope.*** Within this paper, the term “digital machining” denotes a deployment workflow rather than a new mathematical optimizer. Established PTQ operators (GPTQ, AWQ) are used as the machining tools, while the contribution of the present methodology lies in how sensitivity estimation, quantization, runtime realization, and acceptance metrics are linked into a single systems-engineering pipeline.

***Optional visualization (compile-safe).*** If a compact visual is needed to keep the mapping explicit, Fig. [5](https://doi.org/articles/s41598-026-49736-0#Fig5) provides a single illustration of the rule “high sensitivity \Rightarrow preserve” in both domains. The figure remains compile-safe even if external image files are absent.

**Fig. 5**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/5)

Visualizing the sensitivity-based correspondence. In structural optimization, high-energy regions retain material to preserve stiffness and durability margins. In PTQ, high-sensitivity regions retain precision to preserve inference utility. In both cases, reduction is guided by second-order sensitivity under hard constraints. (**a**) Structural sensitivity field (K-governed), (**b**) PTQ sensitivity field (H-governed).

### Governing equations: BESO vs. hessian-aware PTQ/pruning

The convergence proposed in this work is grounded in an alignment of governing equations rather than in a purely verbal analogy. Table [4](https://doi.org/articles/s41598-026-49736-0#Tab4) aligns the core formulation of structural lightweighting (BESO/topology optimization) with Hessian-aware PTQ and pruning. In both cases, the update step removes “material”—finite elements in one case, effective numerical precision in the other—using a *global* sensitivity model rather than a local heuristic.

**Table 4 Governing-equation alignment between structural lightweighting (BESO/topology optimization) and digital lightweighting (Hessian-aware PTQ).**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/4)

This alignment has a direct methodological implication: because the convergence claim depends on *global sensitivity* (stiffness or curvature), the pipeline prioritizes Hessian-aware PTQ (GPTQ) and activation-aware reinforcement (AWQ) over magnitude-only heuristics when operating near INT4 regimes.

### Quantization operators and process parameters

We distinguish symmetric and asymmetric (affine) quantization. For an affine quantizer, the scale *S* and zero-point *Z* define a discrete grid that covers a target dynamic range:

\begin{aligned} q&= \textrm{round}(x/S) + Z, \end{aligned}

(14)

\begin{aligned} \hat{x}&= S\,(q - Z). \end{aligned}

(15)

From a systems perspective, bit-width primarily reduces the dominant weight-traffic term in memory-bound inference (Section 2, Eq. [1](https://doi.org/articles/s41598-026-49736-0#Equ1)), while group size, scale granularity, and outlier handling control the reconstruction penalty in Eq. ([13](https://doi.org/articles/s41598-026-49736-0#Equ13)) (Table [5](https://doi.org/articles/s41598-026-49736-0#Tab5)).

**Table 5 Digital lightweighting design knobs (reporting items for reproducible PTQ).**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/5)

### Derived metrics from measured operating-point logs

To make the digital results auditable, we compute a compact set of derived metrics directly from measured operating points \{ \text {VRAM},\ \text {tok/s},\ \text {PPL},\ P\}, which are later reported in Section 4. Table [6](https://doi.org/articles/s41598-026-49736-0#Tab6) states the exact definitions used throughout the paper, including the acceptance gate \textrm{FoS}\_{\textrm{dig}} (Eq. [11](https://doi.org/articles/s41598-026-49736-0#Equ11)).

**Table 6 Derived metrics computed from measured operating-point logs.**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/6)

### Staged evaluation protocol for incremental comparison

To support an incremental interpretation of the results, the empirical evaluation is organized as a staged progression rather than as a single isolated benchmark. The structural branch is documented through the four topology-evolution stages defined in Section 3.2.2. The digital branch is documented through the staged deployment progression in Table [7](https://doi.org/articles/s41598-026-49736-0#Tab7). This separation is designed to distinguish the effects of structural reduction, precision reduction, quantization strategy, and runtime realization as clearly as the available evidence permits.

**Table 7 Staged evaluation logic used for incremental comparison in the digital branch.**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/7)

When a full runtime-only kernel ablation is not available for all variants, this is explicitly reported in the Results section as a limitation. In that case, the staged comparison remains valid as a deployment-oriented progression, even if it does not isolate every implementation factor independently.

### Production-grade PTQ toolchains

Quantization methods differ mainly in the *measurement signal* they use and in how they preserve structurally critical degrees of freedom.

***GPTQ (Hessian-aware PTQ).*** GPTQ performs layer-wise weight-only PTQ using curvature proxies that approximate sensitivity structure and compensate quantization error during discretization[10](https://doi.org/articles/s41598-026-49736-0#ref-CR10). Consistent with the geometric account developed in Section 2, GPTQ can be interpreted as an approximate nearest-plane projection under a curvature-induced metric[12](https://doi.org/articles/s41598-026-49736-0#ref-CR12).

***AWQ (Activation-aware reinforcement).*** AWQ identifies activation outliers that make a small subset of weights disproportionately important; it protects that subset while aggressively quantizing the remainder[11](https://doi.org/articles/s41598-026-49736-0#ref-CR11). In the manufacturing analogy, this is comparable to locally reinforcing high-load junctions while continuing to remove mass elsewhere.

***Contextual baselines and optional pathways.*** LLM.int8 and SmoothQuant provide widely used alternative routes based on mixed precision and activation re-scaling, respectively, and serve as contextual reference points[31](https://doi.org/articles/s41598-026-49736-0#ref-CR31),[32](https://doi.org/articles/s41598-026-49736-0#ref-CR32). We also acknowledge QAT-oriented directions and PTQ stacks tailored to more extreme activation dynamics, such as ZeroQuant[33](https://doi.org/articles/s41598-026-49736-0#ref-CR33). These methods are not part of the main empirical study unless explicitly reported in Section 4, but they contextualize the deployment choices adopted here.

### Babai nearest-plane as the core machining operator (GPTQ)

To make the methodology explicit, we state the GPTQ update as an approximate lattice decoding step under a Hessian-induced metric. In this view, the machining operator is not independent rounding, but a *nearest-plane projection* that performs sensitivity-guided reduction with correlation-aware error redistribution.

***From quadratic loss to a metric projection.*** Starting from the curvature-weighted reconstruction objective,

\begin{aligned} \hat{\textbf{w}} \;=\; \arg \min \_{\hat{\textbf{w}}\in \mathcal {Q}} (\textbf{w}-\hat{\textbf{w}})^{\top }\textbf{H}(\textbf{w}-\hat{\textbf{w}}), \end{aligned}

(16)

and factorizing the block- or row-level Hessian as \textbf{H}=\textbf{R}^{\top }\textbf{R} (e.g., via Cholesky or a stabilized equivalent), one obtains

\begin{aligned} \hat{\textbf{w}} \;=\; \arg \min \_{\hat{\textbf{w}}\in \mathcal {Q}} \Vert \textbf{R}(\textbf{w}-\hat{\textbf{w}})\Vert \_2^2 \;\;\Longleftrightarrow \;\; \arg \min \_{\hat{\textbf{w}}\in \mathcal {Q}} \Vert \textbf{y}-\textbf{R}\hat{\textbf{w}}\Vert \_2^2, \quad \textbf{y}\equiv \textbf{R}\textbf{w}, \end{aligned}

(17)

which is a closest-vector/nearest-plane problem in the lattice induced by \textbf{R}. This makes the structural nature of PTQ explicit: \textbf{H} defines the sensitivity geometry, and discretization is executed in that geometry rather than in an isotropic \ell \_2 metric.

Geometrically, this corresponds to projecting onto the quantization lattice under a Hessian-induced error ellipse (Fig. [3](https://doi.org/articles/s41598-026-49736-0#Fig3)).

***Why nearest-plane beats rounding (engineering intuition).*** Independent rounding implicitly assumes an isotropic error metric and ignores correlations between weight perturbations. By contrast, the nearest-plane procedure assigns quantized coordinates while updating the residual so that error is redistributed toward directions that are less penalized by the curvature structure. This is the digital analogue of preserving primary load paths while allowing change in structurally slack regions.

***Reproducibility checklist.*** To make GPTQ-style results comparable across platforms, the following items should be reported: curvature proxy construction, numerical damping, grouping or ordering strategy, codebook \mathcal {Q} (including scale, zero-point, and clipping), whether compensation is implemented explicitly through inverse-curvature structure, and the main deployment knobs (b\_w,g) and |\mathcal {D}\_{cal}| summarized in Table [5](https://doi.org/articles/s41598-026-49736-0#Tab5).

### Parameter-efficient adaptation as an optional extension

Where GPTQ and AWQ focus on discretization, parameter-efficient adaptation targets *domain alignment* under tight memory budgets. PRILoRA allocates rank non-uniformly across depth and prunes temporally during fine-tuning, concentrating capacity where it most affects task utility[30](https://doi.org/articles/s41598-026-49736-0#ref-CR30). QLoRA shows that low-rank adapters can be trained over quantized bases, enabling task adaptation with bounded VRAM[29](https://doi.org/articles/s41598-026-49736-0#ref-CR29). In manufacturing terms, this corresponds to adding localized reinforcements rather than globally thickening the structure. These pathways are discussed as optional extensions and are not part of the core paired validation unless explicitly evaluated in the Results section.

### Kernel-level realization: MARLIN as “Finishing”

Quantization becomes operational only if the kernel layer converts smaller weights into realized throughput. MARLIN is a mixed-precision FP16\timesINT4 inference kernel designed to reduce memory movement and improve practical latency and throughput in autoregressive decoding[20](https://doi.org/articles/s41598-026-49736-0#ref-CR20). This finishing step is essential in the present setting: without an INT4-optimized decode kernel, the theoretical reduction in |\textbf{W}\_{\ell }| may not translate into end-to-end gains in tokens/s.

### Digital lightweighting pipeline and measurement protocol

Figure [6](https://doi.org/articles/s41598-026-49736-0#Fig6) summarizes the digital pipeline logic. The measurement protocol in Table [8](https://doi.org/articles/s41598-026-49736-0#Tab8) reports the variables that determine edge-window feasibility: footprint (bytes), throughput and latency, power/energy, and utility preservation. Operational acceptance is gated by the Digital Factor of Safety (Eq. [11](https://doi.org/articles/s41598-026-49736-0#Equ11)) so that aggressive lightweighting is not accepted unless it remains within the chosen integrity envelope.

**Fig. 6**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/6)

Digital lightweighting pipeline. The workflow proceeds from sensitivity estimation to discretization, selective protection, and runtime realization. The objective is not merely to reduce bit-width, but to produce a deployable low-precision artifact that satisfies an edge-feasibility constraint.

**Table 8 Measurement protocol for the digital branch (reporting targets for edge-window feasibility).**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/8)

**Algorithm 1**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/a)

Digital lightweighting pipeline for edge-window LLM deployment.

### Reference deployment architecture for track-side edge windows

To anchor the digital branch in realistic paddock constraints, we adopt a reference architecture in which (i) high-rate sensing and deterministic ECU control coexist with (ii) restricted live telemetry, and (iii) semantic interpretation is performed locally within short garage windows[5](https://doi.org/articles/s41598-026-49736-0#ref-CR5),[6](https://doi.org/articles/s41598-026-49736-0#ref-CR6). On the compute side, emerging automotive platforms explicitly target generative-AI workloads, which makes memory traffic and unified data movement central design variables[23](https://doi.org/articles/s41598-026-49736-0#ref-CR23). The “edge window” is defined here as the short post-run interval during which the quantized model must parse the telemetry dump and answer engineering queries before debrief and setup iteration.

Figure [7](https://doi.org/articles/s41598-026-49736-0#Fig7) summarizes the operational pipeline. High-rate sensing feeds deterministic ECU/DAQ control; due to telemetry restrictions, high-fidelity data are analyzed locally after the run. The quantized LLM is therefore required to deliver decision-relevant interpretations within short garage edge windows rather than through a cloud-dependent loop.

**Fig. 7**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/7)

Operational reference architecture. **(A)** Offline digital machining: production of a verified INT4 artifact from a pretrained model and calibration data. **(B)** Online deployment: a deterministic telemetry parser serializes binary logs into a structured textual context before passing them to the quantized LLM. This preserves a clean separation between deterministic data processing and semantic reasoning.

***Telemetry ingestion: the serializer block.***

To maintain operational validity, deterministic data processing is explicitly separated from semantic inference. As shown in Fig. [7](https://doi.org/articles/s41598-026-49736-0#Fig7), raw binary telemetry is not fed directly into the LLM. Instead, a deterministic telemetry parser or serializer (typically a compiled C++ or Python module) runs on the edge-server CPU and converts high-rate binary logs into a structured textual representation, for example “*Lap 3, T4: Rear Grip=Low, TC\_Act=High*”. The quantized LLM therefore operates as a semantic reasoning engine over serialized engineering context rather than as a signal processor, preserving a clean separation of concerns and making the deployment pathway easier to validate.

### Methodological boundaries

The paired methodology is designed to support a disciplined comparison between structural and digital lightweighting, but it does not imply that both branches are evaluated with identical observables or optimization objectives. The structural branch is validated through mass, stress, deformation, and fatigue-related outputs, whereas the digital branch is validated through footprint, throughput, power, and inference-integrity metrics. Likewise, optional pathways such as PEFT are discussed only as extensions unless explicitly benchmarked. These boundaries are important to avoid overstating the extent of the cross-domain equivalence and to keep the evidence aligned with the actual case studies reported in Section 4.

## Results

This section evaluates the proposed cross-domain framework through two coordinated branches: (i) *structural lightweighting*, expressed through a topology-optimization case study of a Formula Student upright, and (ii) *digital lightweighting*, expressed through measured low-precision LLM operating points under integrity, latency, memory, and power constraints. The objective is not to claim identical physics across domains, but to show a shared *systems logic*: *sensitivity-guided reduction can preserve function while unlocking feasibility under a binding constraint* (mass in the structural domain; memory bandwidth/capacity in the digital domain)[7](https://doi.org/articles/s41598-026-49736-0#ref-CR7),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9),[15](https://doi.org/articles/s41598-026-49736-0#ref-CR15),[16](https://doi.org/articles/s41598-026-49736-0#ref-CR16).

The structural branch is reported as a concrete case study with explicit topology progression, validated final stress/deformation results, and fatigue interpretation. The digital branch is reported as *measured operating points* (VRAM footprint, throughput, power, and perplexity). Derived quantities—including retention, \textrm{FoS}\_{\textrm{dig}}, IPW, ms/token, and J/token—are reported here as fixed values computed from those measured logs using the definitions introduced in Sections 2.6 and 3.6.

\begin{aligned} \textrm{PPL}\_{\textrm{collapse}}=\kappa \,\textrm{PPL}\_{\textrm{base}}, \qquad \text {with default }\kappa =1.5 \text { unless an empirical threshold is available.} \end{aligned}

(18)

### Hypotheses evaluation summary

Table [9](https://doi.org/articles/s41598-026-49736-0#Tab9) summarizes how the empirical and analytical evidence aligns with H1–H4 (Section 1.3). The interpretation is intentionally conservative: H1 is supported at the level of *design logic and second-order sensitivity*, not as a claim of constitutive identity between mechanics and optimization (Fig. [8](https://doi.org/articles/s41598-026-49736-0#Fig8)).

**Table 9 Hypotheses H1–H4: evidence and outcome. The structural side is supported by the upright case study; the digital side is supported by measured operating points and task-grounded evaluation.**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/9)

### Structural branch results: topology evolution and validated final performance

The structural branch provides the physical anchor for the proposed correspondence. Figure [9](https://doi.org/articles/s41598-026-49736-0#Fig9) shows the optimization trajectory from the initial full-mass upright to the final manufacturable topology. The progression is important because it makes the optimization process explicit: the final geometry does not emerge from arbitrary subtraction, but from iterative removal of low-sensitivity material while the dominant load paths remain visible.

The final optimized configuration achieves a mass reduction from 2.45 kg to 1.01 kg, corresponding to a reduction of 58.75%. Under the evaluated loading scenarios, the optimized upright reaches a maximum von Mises stress of 107.52 MPa and a maximum total deformation of 0.136 mm. Relative to the adopted fatigue limit of 159 MPa, this places the final design within an infinite-life regime (>10^8 cycles), giving a fatigue-margin proxy of approximately 159/107.52 \approx 1.48.

**Fig. 8**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/8)

Final optimized upright. The optimized topology preserves the primary load-transmission regions while removing low-contribution material. The visual is intended to support interpretation of the final topology and stress distribution; the quantitative structural values used in the paper are those reported in Table [10](https://doi.org/articles/s41598-026-49736-0#Tab10).

**Table 10 Structural case-study results for the Formula Student upright. The baseline is the full-mass reference geometry; the optimized state is the final validated topology.**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/10)

These results support the physical side of the framework in a limited but concrete sense: substantial mass removal is achieved while the optimized topology remains within the prescribed structural and fatigue envelope. This is the structural counterpart to the digital question of whether aggressive precision reduction can preserve an acceptable inference-integrity envelope.

This figure provides a visual reconstruction of the topology-evolution sequence for interpretive clarity; quantitative structural values are those reported in Table [10](https://doi.org/articles/s41598-026-49736-0#Tab10).

This image is used as a visually consistent stress-distribution illustration of the optimized topology; the structural values discussed in the text correspond to the reported case-study metrics.

#### Topology progression and morphology before/after optimization

To make the optimization process explicit, Fig. [9](https://doi.org/articles/s41598-026-49736-0#Fig9) presents the sequence from the baseline geometry to the final topology. The baseline stage corresponds to the full-mass reference design. The intermediate stages show progressive elimination of low-sensitivity regions and the emergence of load-path-dominated morphology. The final stage incorporates smoothing and manufacturability-aware refinement while preserving the structural logic of the optimized form.

**Fig. 9**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/9)

Topology-evolution sequence for the Formula Student upright. The sequence illustrates the progression from the initial full-mass design to the final manufacturable topology. These visuals are included to clarify the optimization trajectory and morphological change; the quantitative structural values used in the paper are those reported in Table [10](https://doi.org/articles/s41598-026-49736-0#Tab10). (**a**) Baseline, (**b**) Early removal, (**c**) Intermediate, (**d**) Final.

This progression addresses an important practical point: the contribution of the structural case study is not only the final mass value, but the visibility of the intermediate stages through which topology optimization progressively reveals the mechanically privileged regions of the part.

#### Generalizability across structural configurations

The upright case study is used here as a concrete anchor, but the main conclusions are not tied to a single geometry. Across the structural configurations shown in Fig. [10](https://doi.org/articles/s41598-026-49736-0#Fig10), three conclusions are expected to remain stable.

**Fig. 10**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/10)

Structural configurations considered for generalizability. Three upright geometries are shown in the top row to illustrate different structural families under a shared design intent. The corresponding sensitivity fields in the bottom row highlight three robust patterns that remain stable across configurations: (i) interface and load-introduction regions remain highly sensitive and therefore tend to retain material, (ii) low-energy interior regions emerge as the primary candidates for material removal, and (iii) the final topology remains configuration-dependent because boundary conditions, load paths, packaging, and manufacturability constraints alter the local morphology. The figure therefore supports the claim that generalizability lies not in a single final shape, but in a common sensitivity-guided design logic across structural variants.

First, *interfaces and load-introduction zones tend to retain material*. Regions associated with suspension joints, bearing seats, and major reaction paths remain structurally privileged because their local sensitivity is high. Second, *material removal tends to concentrate in low-energy interior regions*, where mass can be reduced without strongly perturbing the global response. Third, *the exact final morphology remains configuration-dependent*: local topology depends on boundary conditions, load directionality, packaging, and manufacturability constraints. Accordingly, the generalizable claim of this paper is not that all optimized structures converge to the same form, but that they follow the same sensitivity-guided design logic.

This point is important for the cross-domain interpretation. The proposed \textbf{K}\leftrightarrow \textbf{H} mapping does not require identical geometries across structural cases; rather, it requires that constrained reduction be governed by a sensitivity object that distinguishes critical from non-critical regions. In that limited but relevant sense, the upright results support a broader engineering interpretation of lightweighting across structural families.

### Measured digital operating points (VRAM, throughput, integrity, and power)

Table [11](https://doi.org/articles/s41598-026-49736-0#Tab11) reports the measured digital operating points for a 32B-class setting. At the statistical level, integrity is evaluated through *measured perplexity* across operating points. We report raw measurements (VRAM, tok/s, power, perplexity) together with derived feasibility indicators: (i) perplexity retention, (ii) the Digital Factor of Safety \textrm{FoS}\_{\textrm{dig}} (Eq. [11](https://doi.org/articles/s41598-026-49736-0#Equ11), instantiated using Eq. [18](https://doi.org/articles/s41598-026-49736-0#Equ18)), and (iii) measured Intelligence-per-Watt (IPW, Eq. [12](https://doi.org/articles/s41598-026-49736-0#Equ12)). To keep the claims hardware-grounded, we also report time per token (ms/token) and energy per token (J/token), computed directly from measured throughput and measured power under batch-1 decoding.

**Table 11 Digital lightweighting operating points (measured metrics). GPTQ-INT4 yields the strongest end-to-end deployment point in the measured set.**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/11)

### Incremental deployment comparison (ablation-style summary)

Reviewer-oriented incremental interpretation is provided in Table [12](https://doi.org/articles/s41598-026-49736-0#Tab12). The table should be read as an *end-to-end staged deployment progression* rather than as a pure component-isolation study. In particular, the contribution of the low-bit runtime kernel is reflected in the final realized GPTQ-INT4 operating point, but it is not isolated here as a standalone runtime-only ablation.

**Table 12 Incremental deployment comparison across measured digital operating points. This is an end-to-end staged comparison: precision reduction, structured quantization, and final realized low-bit runtime appear cumulatively rather than as perfectly isolated factors.**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/12)

### H1: Safety-envelope correspondence (structural fatigue margin vs. digital integrity margin)

H1 concerns the analogy between function-preserving reduction in the two domains. On the structural side, the upright case study shows that the optimized design reaches a 58.75% mass reduction while remaining below the adopted fatigue threshold, with \sigma \_{\textrm{VM,max}}=107.52 MPa versus a fatigue limit of 159 MPa and a corresponding margin proxy of \approx 1.48 (Table [10](https://doi.org/articles/s41598-026-49736-0#Tab10)). On the digital side, the measured GPTQ-INT4 operating point reduces VRAM from 61 GB to 18 GB while maintaining a positive perplexity-based integrity margin under the chosen collapse threshold, with \textrm{FoS}\_{\textrm{dig}}\approx 12.05 (Table [11](https://doi.org/articles/s41598-026-49736-0#Tab11)).

The point of the comparison is not that these two margins are numerically commensurate. Rather, it is that both branches exhibit the same constrained design logic: a substantial reduction of a limiting resource is accepted only if the resulting state remains inside an explicitly defined envelope of admissibility. This is the specific sense in which H1 is supported.

### H2: Integrity preservation at low precision (perplexity retention and task-grounded reasoning)

H2 concerns integrity preservation under aggressive discretization. At the statistical level, integrity is evaluated through *measured perplexity* across operating points (Table [11](https://doi.org/articles/s41598-026-49736-0#Tab11)). GPTQ-INT4 retains \approx 96.0\% perplexity-based retention relative to BF16 while preserving a positive safety margin under the chosen collapse threshold. This is consistent with curvature- and activation-aware PTQ preserving the most critical degrees of freedom at low precision[10](https://doi.org/10.1038/s41598-026-49736-0#ref-CR10),[25](https://doi.org/articles/s41598-026-49736-0#ref-CR25),[26](https://doi.org/articles/s41598-026-49736-0#ref-CR26).

Because perplexity alone does not guarantee correctness in causal engineering reasoning, we complement it with a task-grounded synthetic evaluation.

#### Domain-specific validation: the “Track Engineer Exam” (TEE-20)

To probe H2 beyond statistical proxies, we constructed the **Track Engineer Exam (TEE-20)**, a synthetic gold-standard dataset (N=20) of setup scenarios involving coupled variables such as anti-squat, grip balance, damping, and tire thermal behavior. Each scenario requires a multi-step causal deduction rather than simple keyword matching. We compare the **base model (BF16)** against the **Hessian-optimized model (GPTQ-INT4)** and a **naive low-bit baseline (RTN-INT4)**.

TEE-20 should be interpreted as a task-grounded reasoning probe, not as a hardware-in-the-loop or on-bike closed-loop validation. Its role is to test whether aggressive low-bit deployment preserves engineering reasoning structure better than naive quantization.

**Table 13 TEE-20 evaluation results. GPTQ-INT4 retains near-baseline performance, whereas naive RTN-INT4 exhibits severe degradation and multiple critical failures.**

[Full size table](https://doi.org/articles/s41598-026-49736-0/tables/13)

Table [13](https://doi.org/articles/s41598-026-49736-0#Tab13) shows that the Hessian-optimized model preserves reasoning quality close to the BF16 baseline while naive RTN-INT4 suffers a marked degradation in both accuracy and safety-critical behavior. This result strengthens H2: not all 4-bit operating points are equivalent, and sensitivity-guided quantization matters materially for engineering use.

***Qualitative case study: safety-critical reasoning***

Figure [11](https://doi.org/articles/s41598-026-49736-0#Fig11) illustrates the operational distinction. Given identical telemetry, the naive low-bit model recommends a counter-productive front-end adjustment, whereas the Hessian-optimized model correctly diagnoses a rear-end “pump-down”-style phenomenon and proposes an appropriate damping intervention.

**Fig. 11**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/11)

TEE-20 as a task-grounded reasoning probe for setup engineering. The figure summarizes the logic of the Track Engineer Exam (TEE-20), including a representative telemetry scenario, the reasoning task posed to the models, the aggregate comparison across BF16, GPTQ-INT4, and naive RTN-INT4, and a safety-critical qualitative example. The main result is that GPTQ-INT4 preserves near-baseline reasoning quality while naive RTN-INT4 exhibits marked degradation and critical failures. The visual should be interpreted as an explanatory summary of the benchmark structure and representative outcomes.

***Additional qualitative examples***

To illustrate utility beyond aggregate metrics, two representative cases are summarized below.

### H3: Feasibility via bandwidth relief (VRAM \downarrow, throughput \uparrow)

The measured digital operating points show a *Pareto-improving* shift from BF16 to GPTQ-INT4: VRAM decreases from 61 to 18 GB while throughput increases from 26.0 to 69.9 tok/s. This is consistent with memory-bound decoding behavior and the lower bound in Eq. [1](https://doi.org/articles/s41598-026-49736-0#Equ1)[7](https://doi.org/10.1038/s41598-026-49736-0#ref-CR7),[8](https://doi.org/10.1038/s41598-026-49736-0#ref-CR8),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9). Although absolute throughput depends on kernel realization, batch regime, and context length, the observed directionality is the one expected for a bandwidth-dominated decode path.

A second important result is energetic: power decreases from 295 to 165 W while IPW improves from 0.088 to 0.407 tok/J, which corresponds to an approximately 4.62\times gain in measured intelligence-per-watt relative to BF16.

**Fig. 12**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/12)

Pareto efficiency frontier of measured digital operating points. The red shaded region marks an indicative memory-ceiling zone for typical 32 GB edge devices. BF16 provides the full-precision reference but falls outside that indicative edge envelope. The progression toward GPTQ-INT4 simultaneously improves throughput and moves the model into a more feasible deployment region.

### H4: Edge-window feasibility from throughput-to-time mapping

Because post-run edge windows are time-bounded, feasibility can be expressed as a deterministic mapping from throughput to response time. Let *T* be the number of generated tokens and \tau the measured throughput (tok/s). Then

\begin{aligned} t\_{\text {gen}} \;\approx \; \frac{T}{\tau }. \end{aligned}

(19)

At the INT4 operating point (\tau =69.9 tok/s), a 256-token generation corresponds to t\_{\text {gen}}\approx 3.66 s, and a 512-token generation corresponds to t\_{\text {gen}}\approx 7.32 s.

These times are short enough to support post-run interaction and rapid engineering summarization inside a constrained garage workflow. The result does not imply real-time closed-loop control by the LLM; rather, it shows that low-precision semantic assistance is compatible with short post-run decision windows when the deployment stack remains within the measured INT4 operating envelope.

## Discussion

### Cross-domain interpretation: one constraint logic, two substrates

The structural and digital branches of this study do not share identical physics, but they do share a common systems-engineering logic: *sensitivity-guided reduction preserves function while unlocking feasibility under a binding constraint*. In mechanics, topology optimization removes material while respecting stiffness, stress, deformation, and fatigue-related constraints through sensitivity information encoded in system matrices[15](https://doi.org/articles/s41598-026-49736-0#ref-CR15),[16](https://doi.org/articles/s41598-026-49736-0#ref-CR16). In the digital domain, INT4 post-training quantization (PTQ) removes “bit-mass” while preserving inference integrity through curvature- and activation-aware sensitivity[10](/articles/s41598-026-49736-0#ref-CR10 "Frantar, E., Ashkboos, S., Hoefler, T. & Alistarh, D. Gptq: Accurate post-training quantization for generative pre-trained transformers. In: International Conference on Learning Representations (ICLR) (2023).
                  https://arxiv.org/abs/2210.17323

The correspondence established in Section 2 should therefore be interpreted as an *analytical correspondence at the level of second-order design logic*, not as a claim of constitutive identity. In both domains, a second-order penalty is bounded under a hard resource budget: compliance or stored energy in the structural case, and loss increase or reconstruction penalty in the digital case. This is the basis on which the \textbf{K}\leftrightarrow \textbf{H} bridge becomes useful for systems engineering.

The results support this interpretation in a limited but concrete way. In the structural branch, mass is reduced from 2.45 kg to 1.01 kg while the optimized upright remains inside the adopted stress and fatigue envelope. In the digital branch, VRAM is reduced from 61 GB to 18 GB while the measured GPTQ-INT4 operating point remains within the chosen perplexity-based integrity envelope. The two branches are therefore not equivalent in mechanism, but they are comparable in how constrained removal is evaluated and accepted.

### Discussion aligned with H1–H4

***H1 (Isomorphism of sensitivity).*** H1 posits that the same second-order logic governs “safe removal” in both substrates. Mechanically, BESO and related topology-optimization methods use sensitivity information (e.g., strain-energy density) to preserve load paths while removing low-contribution mass (Eqs. [2](https://doi.org/articles/s41598-026-49736-0#Equ2)–[4](https://doi.org/articles/s41598-026-49736-0#Equ4)). In the present structural case study, that logic becomes visible in the topology-evolution sequence: the final part is not simply smaller, but more explicitly organized around the dominant transmission paths between interfaces. Digitally, PTQ uses curvature or saliency (Eq. [5](https://doi.org/articles/s41598-026-49736-0#Equ5)) to preserve model function while discretizing weights. The contribution here is not to claim identical constitutive laws, but to identify a comparable *engineering control variable*: a global second-order sensitivity structure encoded by a system matrix (\textbf{K} or \textbf{H}) that guides removal decisions.

***H2 (Geometric interpretation of GPTQ).*** H2 argues that GPTQ is principled because it implements a curvature-weighted reconstruction rule rather than independent rounding. By rewriting the PTQ objective as a metric projection (Eq. [16](https://doi.org/articles/s41598-026-49736-0#Equ16) together with the factorization \textbf{H}=\textbf{R}^\top \textbf{R} in Eq. [17](https://doi.org/articles/s41598-026-49736-0#Equ17)), the GPTQ update admits a nearest-plane/closest-vector interpretation under a Hessian-induced geometry[12](https://doi.org/articles/s41598-026-49736-0#ref-CR12). Methodologically, this matters because it clarifies why sensitivity-aware PTQ remains comparatively stable at low bit-width: reconstruction error is redistributed along correlated directions rather than accumulated in high-curvature modes.

***H3 (Operational viability via INT4).*** H3 is supported by the empirical operating-point shift reported in Section 4: moving from BF16 to GPTQ-INT4 reduces memory footprint substantially while increasing delivered throughput and reducing measured power (Tables [11](https://doi.org/articles/s41598-026-49736-0#Tab11) and [12](https://doi.org/articles/s41598-026-49736-0#Tab12); Fig. [12](https://doi.org/articles/s41598-026-49736-0#Fig12)). This Pareto-improving behavior is consistent with memory-traffic-dominated decoding: reducing weight bytes relaxes the memory-wall lower bound in Eq. [1](https://doi.org/articles/s41598-026-49736-0#Equ1)[7](https://doi.org/10.1038/s41598-026-49736-0#ref-CR7),[8](https://doi.org/10.1038/s41598-026-49736-0#ref-CR8),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9). In the motorsport edge-window context, this is not cosmetic optimization: it materially affects whether the system can remain interactive within short post-run time budgets.

***H4 (The edge window).*** H4 is strengthened by expressing feasibility deterministically as a throughput-to-time mapping (Eq. [19](https://doi.org/articles/s41598-026-49736-0#Equ19)): given a token budget *T* and a measured throughput \tau, the interaction time is bounded by t\_{\text {gen}}\approx T/\tau. This framing avoids speculative claims about “real-time” operation and ties the systems argument directly to measurable quantities. The contribution is therefore not to claim closed-loop vehicle control by the LLM, but to show that low-precision semantic assistance can fit within constrained post-run engineering workflows.

***Domain validity beyond perplexity.*** Perplexity serves as a broad statistical proxy for linguistic retention, but it does not by itself guarantee factual correctness in domain-specific reasoning. The TEE-20 results in Table [13](https://doi.org/articles/s41598-026-49736-0#Tab13) strengthen the interpretation of H2 by showing that domain-specific utility is preserved more robustly than perplexity alone might suggest: the increase in PPL from 4.82 to 5.02 is accompanied by only a modest drop in task-grounded engineering accuracy (95% \rightarrow 90%) for the Hessian-aware INT4 model, whereas naive RTN-INT4 collapses to 55% with multiple critical failures.

This is consistent with the central interpretation of the paper: the weights responsible for engineering reasoning are not uniformly expendable, and low-bit deployment remains useful only when high-sensitivity directions are preserved. The INT4 model is therefore not merely smaller; it is a structured low-precision operating point selected to retain engineering competence under a strict resource budget.

### Structural progression and what can reasonably be generalized

One of the central requests from Reviewer 1 was to clarify whether the structural conclusions extend beyond a single example. The upright case study supports a useful but limited generalization.

What appears robust across structural configurations is not a specific final shape, but a *pattern of constrained reduction*. Interfaces, bearing seats, and load-introduction zones tend to remain materially privileged because their local contribution to stiffness and load transfer is high. By contrast, low-energy internal regions are the first candidates for removal. This is the structural analogue of preserving high-curvature or high-saliency weights during aggressive quantization.

At the same time, the exact optimized morphology is configuration-dependent. Boundary conditions, load combinations, manufacturing constraints, and packaging all influence the final topology. For that reason, the present paper does not claim that the upright geometry is universally representative of all motorsport structural components. Rather, the upright is used to demonstrate the design logic by which a sensitivity field governs where reduction remains acceptable and where it does not. This is also why the generalizable claim of the paper is methodological rather than morphological.

### Inference integrity envelope: Digital Factor of Safety as an acceptance gate

Mechanical lightweighting programs reduce mass while tracking an explicit margin to failure. Analogously, PTQ reduces precision while approaching a semantic-failure boundary defined through unacceptable degradation in perplexity or task utility. The Digital Factor of Safety introduced in Section 2.6 (Eq. [11](https://doi.org/articles/s41598-026-49736-0#Equ11)) makes this integrity envelope explicit: *it quantifies the headroom between a measured operating point and a task-defined collapse threshold*.

A key methodological point is that \kappa should be treated as application-calibrated rather than universal. Accordingly, reporting the chosen \kappa and the rationale for the collapse criterion is part of reproducible reporting. Within the present dataset, \textrm{FoS}\_{\textrm{dig}} is best understood as an *acceptance-style deployment gate*: it does not prove semantic correctness, but it helps distinguish operating points that remain comfortably inside the chosen integrity envelope from those that approach unacceptable degradation.

This is directly parallel to the use of explicit structural margins. Just as a positive fatigue margin does not guarantee universal mechanical immunity under all future load cases, a positive \textrm{FoS}\_{\textrm{dig}} does not guarantee universal semantic correctness across all future prompts. In both cases, the metric is most useful as an engineering acceptance tool under stated assumptions.

### Human latency budget and cognitive ergonomics

A common objection in motorsport systems engineering is that garage analysis does not require real-time performance because the vehicle is stationary. The relevant distinction, however, is between *control latency* and *interaction latency*. The LLM is not placed inside a millisecond-scale stabilization loop; instead, it operates within the human problem-solving loop of the engineer during a constrained post-run window.

This difference is operationally important. Engineering diagnosis is rarely a single-shot query; it is typically an iterative exchange:

1. 1.

*Engineer:* “Why is the rider reporting understeer in Sector 3?”
2. 2.

*System:* “Telemetry shows front fork bottoming out at the T9 braking marker.”
3. 3.

*Engineer:* “The fork is already at maximum stiffness. What is the rear pitch doing?”
4. 4.

*System:* “Rear rebound is fast, causing rapid weight transfer and front unloading.”

Under high-latency conditions, the interaction becomes cognitively expensive. Established HCI results show that delays above a few seconds begin to disrupt flow, and delays above roughly 10 seconds push users toward attention loss or task switching[34](https://doi.org/10.1038/s41598-026-49736-0#ref-CR34),[35](https://doi.org/10.1038/s41598-026-49736-0#ref-CR35),[36](https://doi.org/articles/s41598-026-49736-0#ref-CR36). In practical terms, a 30-second response regime encourages broad, infrequent, high-confidence queries and discourages exploratory hypothesis testing. The model is then treated as a static *oracle* rather than as an interactive reasoning aid.

By contrast, the INT4 operating point reported in Section 4.8 shifts the system toward a conversational regime. At approximately 70 tok/s, a typical engineering response of 150–200 tokens can be delivered in roughly 2–3 s. This preserves the engineer’s chain of thought and supports *iterative interrogation*: the user can ask a tentative question, inspect the model’s intermediate logic, and immediately refine the next prompt. In this sense, low latency is not a vanity metric; it is a usability constraint for pit-lane viability.

### IPW and what is (and is not) supported by the current dataset

We introduced IPW as a systems metric:

\begin{aligned} \textrm{IPW}=\frac{\textrm{Integrity}\times \textrm{Throughput}}{P}, \end{aligned}

(20)

where Integrity is instantiated here through perplexity retention, Throughput is measured in tok/s, and *P* is measured power in watts. Because Section 4 reports measured \textrm{PPL} and measured power for each operating point (Table [11](https://doi.org/articles/s41598-026-49736-0#Tab11)), IPW is directly computable from observed data rather than from speculative proxies.

At the same time, IPW should not be interpreted as a universal measure of intelligence. Its role in this paper is narrower and more practical: it acts as a *decision metric* for embedded feasibility, combining integrity preservation, delivered throughput, and thermal/power cost in a single reporting variable. Within that scope, the observed joint improvement in footprint, throughput, and power supports IPW as a useful comparison metric for edge-window deployments, but not as a universal cognitive-performance index.

### Motorsport embedded environment: ECU/DAQ constraints and supervisory compatibility

To keep the contribution grounded in motorsport systems engineering rather than in generic accelerator benchmarking, we frame the deployment target as the *race ECU/DAQ stack*: a mechatronic subsystem operating under vibration, thermal, and packaging constraints while serving high-rate telemetry and supervisory reasoning. Representative ECU/DAQ classes emphasize compact integration and real-time robustness[3](https://doi.org/articles/s41598-026-49736-0#ref-CR3),[4](https://doi.org/articles/s41598-026-49736-0#ref-CR4). These conditions explain why memory bandwidth, memory capacity, and power become the binding resources of the “digital chassis,” rather than FLOPs alone.

***Telemetry as a physical signal chain.*** Motorsport DAQ stacks capture and curate high-rate sensor streams for analysis and decision support[37](https://doi.org/articles/s41598-026-49736-0#ref-CR37),[38](https://doi.org/articles/s41598-026-49736-0#ref-CR38). In the present interpretation, tensor flow through accelerator memory hierarchies behaves analogously to an engineering signal chain: bottlenecks localize where effective cross-section (bandwidth) is insufficient, consistent with the roofline and memory-wall framing[8](https://doi.org/articles/s41598-026-49736-0#ref-CR8),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9).

***Supervisory-loop compatibility.*** The quantized LLM is not positioned inside inner, kilohertz-rate control loops. Rather, it acts as a supervisory semantic layer operating on aggregated telemetry windows or post-run dumps. Its outputs are structured advisories—diagnostic hypotheses, hazard annotations, and setup-oriented recommendations—that can inform or bias existing deterministic control and analysis pipelines. This distinction is important because it bounds the claim of the paper: the LLM is compatible with supervisory engineering workflows, not proposed here as a replacement for deterministic inner-loop control.

***Why unified memory matters (platform-level implication).*** Automotive compute platforms increasingly emphasize coherent shared-memory models to reduce copy overhead between CPU and accelerators. Such features directly support lower-jitter supervisory inference by keeping the telemetry-to-model data path compact and predictable[23](https://doi.org/articles/s41598-026-49736-0#ref-CR23). In the present framework, this matters because any reduction in unnecessary data movement improves the practical viability of the edge window.

### Threats to validity and reproducibility notes

* **Hardware/kernel coupling.** Tokens/s depends on kernel choice, batch size, KV-cache policy, and memory hierarchy. Runtime kernel, decoding settings, and repeated-run variance should be reported whenever possible (cf. Table [5](https://doi.org/articles/s41598-026-49736-0#Tab5)).
* **Integrity proxy.** Perplexity retention captures statistical likelihood, not full domain utility. Task-grounded probes such as TEE-20 improve construct validity, but broader telemetry-grounded evaluations would further strengthen the claims.
* **Collapse threshold** (\kappa). \kappa should be justified empirically for the target task. Sensitivity analysis with respect to \kappa would improve robustness reporting.
* **Structural scope.** The upright case study provides a concrete physical anchor, but it is still a single structural example. It should not be interpreted as a universal bound for all motorsport components or topology-optimization workflows.
* **Structural validation scope.** The structural branch is supported by a computational case study with reported performance metrics, but it is not accompanied here by a new physical bench test or manufactured-part validation campaign. The physical evidence therefore supports the cross-domain argument at the level of computational structural design rather than as a full experimental mechanics study.
* **No closed-loop HIL validation.** The current study evaluates measured digital operating points and task-grounded reasoning preservation, but it does not include hardware-in-the-loop or on-bike closed-loop testing. The contribution should therefore be interpreted as a deployment-feasibility study rather than as a fully integrated race-vehicle validation.
* **Power measurement protocol.** Full IPW credibility benefits from explicit reporting of sampling rate, steady-state interval, instrumentation, and thermal conditions, together with J/token (E\_{\text {tok}}=P/\tau) alongside average power.

### Closing insight

Across both substrates—metal and bits—the same engineering pattern recurs: naive removal fails, whereas sensitivity-guided reduction preserves function under a strict budget. Under telemetry restrictions and short edge-window timing, quantization is best interpreted not as post hoc compression but as *structural optimization for deployment feasibility*[7](https://doi.org/articles/s41598-026-49736-0#ref-CR7),[9](https://doi.org/10.1038/s41598-026-49736-0#ref-CR9),[10](https://doi.org/10.1038/s41598-026-49736-0#ref-CR10).

Accordingly, the present work should be read as a systems-engineering feasibility study with a cross-domain analytical framing and deployment-oriented metrics, rather than as a new PTQ algorithm or as a closed-loop experimental validation of a race vehicle.

## Conclusion and future work

This paper has presented, through a systems-engineering lens, the argument that aggressive low-precision deployment of LLM-based assistance for motorcycle motorsport can be interpreted as a form of *structural optimization for deployment feasibility*, rather than as a purely post hoc compression step. The proposed convergence between topology optimization and post-training quantization (PTQ) provides a shared technical language across substrates: stiffness and load-path sensitivity in mechanics, encoded by \textbf{K}, are compared with curvature and importance sensitivity in neural networks, encoded by \textbf{H}[21](https://doi.org/articles/s41598-026-49736-0#ref-CR21),[22](https://doi.org/articles/s41598-026-49736-0#ref-CR22),[25](https://doi.org/articles/s41598-026-49736-0#ref-CR25),[26](https://doi.org/articles/s41598-026-49736-0#ref-CR26).

Across the *structural* branch, the Formula Student upright case study illustrates a sensitivity-guided lightweighting process in which mass is reduced from 2.45 kg to 1.01 kg while the final validated response remains within the adopted structural and fatigue envelope, with \sigma \_{\textrm{VM,max}}=107.52 MPa below the fatigue limit of 159 MPa. Across the *digital* branch, the measured operating points show that moving from BF16 to GPTQ-INT4 yields a Pareto-improving shift in feasibility for a 32B-class setting: VRAM decreases from 61 to 18 GB, throughput increases from 26.0 to 69.9 tok/s, power decreases from 295 to 165 W, and perplexity increases only moderately from 4.82 to 5.02 (Table [11](https://doi.org/articles/s41598-026-49736-0#Tab11)). Taken together, these results support the thesis that *sensitivity-guided reduction can preserve function while unlocking feasibility under binding constraints*—mass in the structural domain, and memory bandwidth/capacity together with thermal power in the digital domain[7](https://doi.org/articles/s41598-026-49736-0#ref-CR7),[9](https://doi.org/10.1038/s41598-026-49736-0#ref-CR9),[10](https://doi.org/10.1038/s41598-026-49736-0#ref-CR10),[15](https://doi.org/articles/s41598-026-49736-0#ref-CR15),[16](https://doi.org/articles/s41598-026-49736-0#ref-CR16),[20](https://doi.org/articles/s41598-026-49736-0#ref-CR20).

The contribution of the paper is therefore methodological rather than algorithmic. We do not propose a new quantization algorithm or a new structural optimizer; instead, we provide a cross-domain analytical framing together with deployment-oriented metrics and a reproducible evaluation workflow for judging when low-precision LLM deployment becomes feasible in motorsport telemetry contexts.

### Hypotheses revisited (H1–H4)

***H1 (Cross-substrate sensitivity isomorphism).*** The core convergence claim is that both lightweighting problems are governed by a global second-order sensitivity structure. In mechanics, \textbf{K} encodes load-path sensitivity and guides safe removal or preservation of material. In PTQ, \textbf{H} encodes loss curvature or importance and bounds safe discretization through a quadratic penalty (Eqs. [5](https://doi.org/articles/s41598-026-49736-0#Equ5) and [16](https://doi.org/articles/s41598-026-49736-0#Equ16)). The support for H1 should be interpreted at the level of *design logic*, not as a claim of constitutive identity between the two domains.

***H2 (Geometric interpretation of GPTQ under the Hessian metric).*** Hessian-aware PTQ improves over independent rounding because it minimizes reconstruction error in a curvature-induced geometry. The factorization \textbf{H}=\textbf{R}^\top \textbf{R} converts PTQ into a metric-projection/nearest-plane problem (Section 2.4.3; Eqs. [16](https://doi.org/articles/s41598-026-49736-0#Equ16)–[17](https://doi.org/articles/s41598-026-49736-0#Equ17))[10](https://doi.org/articles/s41598-026-49736-0#ref-CR10). This provides a principled rationale for why sensitivity-aware PTQ remains comparatively stable at INT4, and the TEE-20 results further indicate that this stability extends beyond perplexity into task-grounded engineering reasoning.

***H3 (Feasibility via bandwidth relief: VRAM***\downarrow ***and throughput*** \uparrow). The measured operating points show a clear shift toward deployability: BF16 \rightarrow GPTQ-INT4 reduces footprint (61 \rightarrow 18 GB), increases throughput (26.0 \rightarrow 69.9 tok/s), and reduces power (295 \rightarrow 165 W) (Tables [11](https://doi.org/articles/s41598-026-49736-0#Tab11) and [12](https://doi.org/articles/s41598-026-49736-0#Tab12); Fig. [12](https://doi.org/articles/s41598-026-49736-0#Fig12)). This is consistent with memory-wall and roofline expectations for weight-dominant decoding (Eq. [1](https://doi.org/articles/s41598-026-49736-0#Equ1))[7](https://doi.org/10.1038/s41598-026-49736-0#ref-CR7),[8](https://doi.org/10.1038/s41598-026-49736-0#ref-CR8),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9), while also showing that kernel-level realization is required for theoretical compression benefits to translate into batch-1 decode gains[20](https://doi.org/articles/s41598-026-49736-0#ref-CR20).

***H4 (Edge-window feasibility can be stated deterministically).*** With measured throughput \tau (tok/s), response time scales as t\_{\textrm{gen}}\approx T/\tau for a *T*-token generation (Eq. [19](https://doi.org/articles/s41598-026-49736-0#Equ19)). At the GPTQ-INT4 operating point, this yields concrete post-run interaction times inside the garage decision window, avoiding vague claims about speed or usability. The result supports supervisory semantic assistance within short engineering windows; it does not imply real-time closed-loop vehicle control by the LLM.

### Inference integrity envelope and IPW as embedded design constraints

For embedded deployment, feasibility requires not only speed but also an explicit integrity envelope. The Digital Factor of Safety introduced in Section 2.6 (Eq. [11](https://doi.org/articles/s41598-026-49736-0#Equ11)) expresses the headroom between a measured operating point and a task-defined collapse threshold. Complementarily, Intelligence-per-Watt (IPW) (Eq. [12](https://doi.org/articles/s41598-026-49736-0#Equ12)) operationalizes *integrity-aware functional throughput per watt* using measured power and a quantified integrity term. Under the measured logs, GPTQ-INT4 improves feasibility jointly in footprint, throughput, and power while remaining inside a high-retention regime (Table [11](https://doi.org/articles/s41598-026-49736-0#Tab11)), which supports IPW as a practical reporting and acceptance objective under strict thermal limits[27](https://doi.org/articles/s41598-026-49736-0#ref-CR27),[28](https://doi.org/articles/s41598-026-49736-0#ref-CR28),[39](https://doi.org/articles/s41598-026-49736-0#ref-CR39),[40](https://doi.org/articles/s41598-026-49736-0#ref-CR40).

At the same time, neither \textrm{FoS}\_{\textrm{dig}} nor IPW should be interpreted as universal constants of model quality. Their value lies in deployment assessment: they help determine whether a low-precision operating point is merely compact, or genuinely admissible under a given task, risk tolerance, and power envelope.

### Operational implication for premier-class motorcycle edge windows

Premier-class motorcycles are highly instrumented and support high-rate ECU/DAQ analysis[5](https://doi.org/articles/s41598-026-49736-0#ref-CR5), yet live telemetry is restricted in many competitive contexts[6](https://doi.org/articles/s41598-026-49736-0#ref-CR6). This concentrates actionable intelligence into two practical windows: (i) local edge interpretation close to the vehicle, and (ii) short garage bursts immediately after a run. Within this envelope, quantization is enabling: it moves an LLM-based semantic layer into a more embedded-feasible region by jointly reducing memory traffic and power while maintaining an explicit integrity margin.

Crucially, the LLM remains an *advisory semantic layer* for triage, explanation, summarization, and engineering reasoning. Deterministic stability, actuation, and inner-loop safety remain in the ECU/control stack. This separation bounds the claim of the paper and clarifies where low-precision LLMs are operationally useful.

### Hardware implications: toward FP4-class “materials”

As automotive and edge accelerators adopt native low-precision formats such as INT4 and FP4-class arithmetic[23](https://doi.org/articles/s41598-026-49736-0#ref-CR23), the framework developed here suggests a *certification-style* interpretation: precision formats can be treated as computational materials, and deployment should be accepted only when an explicit integrity margin (e.g., \textrm{FoS}\_{\textrm{dig}}) and an energy objective (IPW) are both satisfied.

This shifts the practical question from “Can we run FP4?” to “Under what integrity margins, workload conditions, and runtime settings is FP4-class deployment admissible?” That reframing is especially relevant for edge environments in which memory movement, thermal behavior, and human interaction latency matter as much as nominal arithmetic capability.

### Limitations and threats to validity

Several limitations bound interpretation.

First, the digital operating points characterize inference integrity and efficiency, but do not yet establish end-to-end decision utility across realistic workload mixes; future work should quantify task value under burst constraints and broader telemetry scenarios.

Second, throughput and power depend on kernel choice, batch regime, context length, KV-cache policy, and memory hierarchy; reported gains should therefore be interpreted as *operating-point evidence under stated settings*, not as universal constants[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9),[20](https://doi.org/articles/s41598-026-49736-0#ref-CR20).

Third, perplexity is a sensitive integrity signal but not a domain task. TEE-20 improves construct validity by adding task-grounded reasoning evidence, but it remains a synthetic benchmark rather than a hardware-in-the-loop or on-bike validation.

Fourth, the structural branch is grounded in a concrete computational case study of a Formula Student upright, but it remains a single structural example. It therefore supports the proposed cross-domain framework at the level of a representative case, not as a universal statement about all motorsport components or topology-optimization workflows.

Fifth, the structural branch is not accompanied here by a new manufactured-part campaign or physical bench validation. It should therefore be interpreted as a computational structural-design study rather than as a full experimental mechanics validation.

Finally, the present work does not include closed-loop HIL or on-bike testing. The manuscript should therefore be interpreted as a systems-engineering deployment-feasibility study with measured edge operating points, not as a full experimental validation of an integrated race-vehicle intelligence stack.

### Future work

1. 1.

**Telemetry-grounded task validation:** evaluate quantized agents on motorsport tasks such as chatter detection, brake-fade diagnosis, setup-delta explanation, and constraint-aware advisories under explicit edge-window latency budgets and integrity thresholds.
2. 2.

**Track-side energy accounting:** report Joules/token and IPW with repeated-run confidence intervals for each operating point, enabling more rigorous and comparable efficiency reporting[27](https://doi.org/articles/s41598-026-49736-0#ref-CR27),[28](https://doi.org/articles/s41598-026-49736-0#ref-CR28).
3. 3.

**Kernel-aware co-design:** jointly select PTQ scheme and serving kernel for the target SoC memory hierarchy and batch-1 decode regime; report the full inference stack, including runtime kernel, KV-cache strategy, and context length[8](https://doi.org/articles/s41598-026-49736-0#ref-CR8),[9](https://doi.org/articles/s41598-026-49736-0#ref-CR9),[20](https://doi.org/articles/s41598-026-49736-0#ref-CR20).
4. 4.

**Sensitivity-threshold calibration:** calibrate \kappa and task-specific collapse boundaries using domain benchmarks broader than perplexity, so that \textrm{FoS}\_{\textrm{dig}} can evolve from a conservative reporting metric into a better-grounded deployment criterion.
5. 5.

**Cross-geometry structural validation:** extend the structural branch to additional motorsport components and boundary conditions so that the generalizability of the sensitivity-guided design logic can be assessed more systematically across structural families.
6. 6.

**Hybrid routing under restricted links:** deploy a local INT4 agent for routine interpretation and invoke frontier or cloud models only for exceptional reasoning, enforcing bounded energy and predictable latency under intermittent connectivity[41](https://doi.org/articles/s41598-026-49736-0#ref-CR41),[42](https://doi.org/articles/s41598-026-49736-0#ref-CR42).
7. 7.

**Closed-loop and HIL extensions:** evaluate whether supervisory semantic assistance can be integrated safely into track-side engineering workflows with hardware-in-the-loop infrastructure, without encroaching on deterministic inner-loop control.

In premier-class motorsport, “adding lightness” is a prerequisite for feasibility. The same axiom can be extended to the digital chassis: sensitivity-guided quantization is the mechanism that makes edge-deployable semantic assistance more viable under binding memory and thermal constraints.

## Data availability

All artifacts required to reproduce the reported results, including evaluation inputs, configuration files, and benchmarking traces, are available in the Zenodo archive: https://doi.org/10.5281/zenodo.18098196. Third-party model weights must be obtained from their original providers under the corresponding licenses.

## Code availability

The source code used for quantized LLM inference, evaluation, and figure/table generation is openly available in Zenodo at <https://zenodo.org/records/18098196>.

## References

1. Cossalter, V. *Motorcycle Dynamics* 2nd edn. (Lulu.com, 2006).

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Motorcycle%20Dynamics&publication_year=2006&author=Cossalter%2CV)
2. Foale, T. Motorcycle Handling and Chassis Design: The Art and Science, 2nd edn., p. 560. Tony Foale, ??? (2006)
3. Motion Applied Ltd.: TAG-320B Electronic Control Unit (ECU) — Product information. Accessed 2026-01-27 (2026). <https://www.motionapplied.com/products/TAG-320B>
4. Motion Applied Ltd.: TAG-400i Engine Control Unit — Product summary. Accessed 2026-01-27 (2012). <https://www.motionapplied.com/documents/products/summaries/TAG-400i.pdf>
5. MotoGP: What Electronics Are Used in MotoGP Bikes? MotoGP.com blog article. Accessed: 2026-01-27 (2025). <https://www.motogp.com/en/blog-articles/what-electronics-are-used-in-motogp-bikes/823661>
6. Fédération Internationale de Motocyclisme (FIM): FIM Grand Prix World Championship Regulations (Updated 25 February 2022). Regulatory document (PDF). Accessed: 2026-01-27 (2022). <https://www.fim-moto.com/fileadmin/user_upload/Documents/2022/2022_FIM_Grand_Prix_World_Championship_Regulations._Updated_25February_2022.pdf>
7. Wulf, W. A. & McKee, S. A. Hitting the memory wall: Implications of the obvious. *ACM SIGARCH computer architecture news* **23**(1), 20–24 (1995).

[Article](https://doi.org/10.1145%2F216585.216588)
   [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Hitting%20the%20memory%20wall%3A%20Implications%20of%20the%20obvious&journal=ACM%20SIGARCH%20computer%20architecture%20news&doi=10.1145%2F216585.216588&volume=23&issue=1&pages=20-24&publication_year=1995&author=Wulf%2CWA&author=McKee%2CSA)
8. Sze, V., Chen, Y.-H., Yang, T.-J. & Emer, J. S. Efficient processing of deep neural networks: A tutorial and survey. *Proceedings of the IEEE* **105**(12), 2295–2329 (2017).

[Article](https://doi.org/10.1109%2FJPROC.2017.2761740)
   [ADS](http://adsabs.harvard.edu/cgi-bin/nph-data_query?link_type=ABSTRACT&bibcode=2017IEEEP.105.2295S)
   [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Efficient%20processing%20of%20deep%20neural%20networks%3A%20A%20tutorial%20and%20survey&journal=Proceedings%20of%20the%20IEEE&doi=10.1109%2FJPROC.2017.2761740&volume=105&issue=12&pages=2295-2329&publication_year=2017&author=Sze%2CV&author=Chen%2CY-H&author=Yang%2CT-J&author=Emer%2CJS)
9. Williams, S., Waterman, A. & Patterson, D. Roofline: an insightful visual performance model for multicore architectures. *Communications of the ACM* **52**(4), 65–76 (2009).

[Article](https://doi.org/10.1145%2F1498765.1498785)
   [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Roofline%3A%20an%20insightful%20visual%20performance%20model%20for%20multicore%20architectures&journal=Communications%20of%20the%20ACM&doi=10.1145%2F1498765.1498785&volume=52&issue=4&pages=65-76&publication_year=2009&author=Williams%2CS&author=Waterman%2CA&author=Patterson%2CD)
10. Frantar, E., Ashkboos, S., Hoefler, T. & Alistarh, D. Gptq: Accurate post-training quantization for generative pre-trained transformers. In: *International Conference on Learning Representations (ICLR)* (2023). <https://arxiv.org/abs/2210.17323>
11. Lin, J., Tang, J., Tang, H., Yang, S., Chen, X. & Han, S. Awq: Activation-aware weight quantization for on-device llm compression and acceleration. In: *Proceedings of the 6th MLSys Conference (2023)*. <https://arxiv.org/abs/2306.00978>
12. Chen, J., Shabanzadeh, Y., Crnčević, E., Hoefler, T. & Alistarh, D. The geometry of LLM quantization: GPTQ as Babai’s nearest plane algorithm. In: *The Thirteenth International Conference on Learning Representations (ICLR)* (2025). <https://doi.org/10.48550/arXiv.2507.18553>
13. Sandalcilar, H., Cagirankaya, F., Arslanoglu, M. & Dikici, B. Structural optimization of a formula student upright using the beso method. *International Journal of Automotive Science and Technology* **9**(4), 536–546 (2025). Case study on Bell Crank/Upright optimization
14. Pang, T. Y. & Fard, M. Reverse engineering and topology optimization for weight-reduction of a bell-crank. *Applied Sciences* **10**(23), 8568. <https://doi.org/10.3390/app10238568> (2020).

[Article](https://doi.org/10.3390%2Fapp10238568)
    [CAS](https://doi.org/articles/cas-redirect/1:CAS:528:DC%2BB3MXlt1Citbw%3D)
    [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Reverse%20engineering%20and%20topology%20optimization%20for%20weight-reduction%20of%20a%20bell-crank&journal=Applied%20Sciences&doi=10.3390%2Fapp10238568&volume=10&issue=23&publication_year=2020&author=Pang%2CTY&author=Fard%2CM)
15. Bendsøe, M. P. & Sigmund, O. *Topology Optimization: Theory, Methods, and Applications* (Springer, 2003).

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Topology%20Optimization%3A%20Theory%2C%20Methods%2C%20and%20Applications&publication_year=2003&author=Bends%C3%B8e%2CMP&author=Sigmund%2CO)
16. Huang, X. & Xie, Y. M. Convergent and mesh-independent solutions for the bi-directional evolutionary structural optimization (beso) method. *Finite Elements in Analysis and Design* **43**(14), 1039–1050 (2007).

[Article](https://doi.org/10.1016%2Fj.finel.2007.06.006)
    [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Convergent%20and%20mesh-independent%20solutions%20for%20the%20bi-directional%20evolutionary%20structural%20optimization%20%28beso%29%20method&journal=Finite%20Elements%20in%20Analysis%20and%20Design&doi=10.1016%2Fj.finel.2007.06.006&volume=43&issue=14&pages=1039-1050&publication_year=2007&author=Huang%2CX&author=Xie%2CYM)
17. Angeles, J. On the nature of the cartesian stiffness matrix. *Ingeniería Mecánica, Tecnología y Desarrollo* **3**(5), 163–170 (2010). SciELO México
18. Kaplan, J. et al. Scaling laws for neural language models. arXiv preprint [arXiv:2001.08361](http://arxiv.org/abs/2001.08361) (2020)
19. Hoffmann, J. et al. Training compute-optimal large language models. arXiv preprint [arXiv:2203.15556](http://arxiv.org/abs/2203.15556) (2022). The “Chinchilla” paper
20. Frantar, E. & Alistarh, D. Marlin: Mixed-precision auto-regressive parallel inference on large language models. In: Proceedings of the 30th ACM SIGPLAN Symposium on Principles and Practice of Parallel Programming (PPoPP) <https://doi.org/10.1145/3696443.3708869> (2025).
21. Bathe, K.-J. *Finite Element Procedures* (Prentice Hall, 2006).

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Finite%20Element%20Procedures&publication_year=2006&author=Bathe%2CK-J)
22. Nocedal, J. & Wright, S. J. *Numerical Optimization* 2nd edn. (Springer, 2006).

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Numerical%20Optimization&publication_year=2006&author=Nocedal%2CJ&author=Wright%2CSJ)
23. NVIDIA Corporation: NVIDIA DRIVE Thor: The Next Generation of Centralized Car Compute. Hardware reference for SoC specifications (2024). <https://www.nvidia.com/en-us/self-driving-cars/drive-platform/drive-thor/>
24. Martens, J. Deep learning via hessian-free optimization. In: ICML, vol. 27, pp. 735–742 (2010)
25. LeCun, Y., Denker, J. S. & Solla, S. A. Optimal brain damage. Advances in neural information processing systems **2** (1990). Foundational paper on using the Hessian for pruning
26. Hassibi, B. & Stork, D. G. Optimal brain surgeon and general network pruning. In: Advances in Neural Information Processing Systems, pp. 164–171. Morgan Kaufmann, San Mateo, CA (1993)
27. Strubell, E., Ganesh, A. & McCallum, A. Energy and policy considerations for deep learning in NLP. In: Proceedings of the 57th Annual Meeting of the Association for Computational Linguistics, pp. 3645–3650. Association for Computational Linguistics, Florence, Italy (2019)
28. Schwartz, R., Dodge, J., Smith, N. A. & Etzioni, O. Green ai. *Communications of the ACM* **63**(12), 54–63. <https://doi.org/10.1145/3381831> (2020).

[Article](https://doi.org/10.1145%2F3381831)
    [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Green%20ai&journal=Communications%20of%20the%20ACM&doi=10.1145%2F3381831&volume=63&issue=12&pages=54-63&publication_year=2020&author=Schwartz%2CR&author=Dodge%2CJ&author=Smith%2CNA&author=Etzioni%2CO)
29. Dettmers, T., Pagnoni, A., Holtzman, A. & Zettlemoyer, L. Qlora: Efficient finetuning of quantized llms. In: Advances in Neural Information Processing Systems (2023). <https://doi.org/10.48550/arXiv.2305.14314> . <https://arxiv.org/abs/2305.14314>
30. Benedek, N. & Wolf, L. Prilora: Pruned and rank-increasing low-rank adaptation. In: Findings of the Association for Computational Linguistics: EACL 2024, pp. 252–263. Association for Computational Linguistics, St. Julian’s, Malta (2024). <https://doi.org/10.18653/v1/2024.findings-eacl.18> . <https://aclanthology.org/2024.findings-eacl.18/>
31. Dettmers, T., Lewis, M., Belkada, Y. & Zettlemoyer, L. Llm.int8(): 8-bit matrix multiplication for transformers at scale. In: Advances in Neural Information Processing Systems (NeurIPS) (2022)
32. Xiao, G. et al. Smoothquant: Accurate and efficient post-training quantization for large language models. In: International Conference on Machine Learning (ICML) (2023)
33. Yao, Z. Zeroquant: Efficient and affordable post-training quantization for large-scale transformers. In: Advances in Neural Information Processing Systems (2022). <https://doi.org/10.48550/arXiv.2206.01861> . <https://arxiv.org/abs/2206.01861>
34. Miller, R. B. Response time in man-computer conversational transactions. *Proceedings of the AFIPS Fall Joint Computer Conference* **33**, 267–277 (1968).

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Response%20time%20in%20man-computer%20conversational%20transactions&journal=Proceedings%20of%20the%20AFIPS%20Fall%20Joint%20Computer%20Conference&volume=33&pages=267-277&publication_year=1968&author=Miller%2CRB)
35. Nielsen, J. *Usability Engineering* (Morgan Kaufmann, 1993).

[Book](https://doi.org/10.1016%2FB978-0-08-052029-2.50007-3)
    [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Usability%20Engineering&doi=10.1016%2FB978-0-08-052029-2.50007-3&publication_year=1993&author=Nielsen%2CJ)
36. Shneiderman, B. Response time and display rate in human performance with computers. *ACM Computing Surveys (CSUR)* **16**(3), 265–285 (1984).

[Article](https://doi.org/10.1145%2F2514.2517)
    [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Response%20time%20and%20display%20rate%20in%20human%20performance%20with%20computers&journal=ACM%20Computing%20Surveys%20%28CSUR%29&doi=10.1145%2F2514.2517&volume=16&issue=3&pages=265-285&publication_year=1984&author=Shneiderman%2CB)
37. Motion Applied: ATLAS: Advanced Telemetry Linked Acquisition System (Product Page). Accessed 2026-01-27. Describes ATLAS telemetry capture, distribution, display, and analysis. (2025). <https://www.motionapplied.com/products/ATLAS>
38. KX Newsroom: McLaren Applied’s ATLAS Software Adds New Analytics Capabilities with KX Partnership. Accessed 2026-01-27. Public description of ATLAS analytics capabilities and integration. (2023). <https://kx.com/>
39. Saad-Falcon, J., Narayan, A., Akengin, H. O. & Griffin, J. W. Intelligence per Watt: Measuring Intelligence Efficiency of Local AI. Key metric definition for the paper (2025).
40. Embedl: Intelligence per watt: Edge versus cloud. Technical report, Embedl AI Research <https://www.embedl.com/knowledge/intelligence-per-watt-edge-versus-cloud> (2025).
41. Chen, L. et al. Frugalgpt: How to use large language models while reducing cost and improving performance. arXiv preprint [arXiv:2305.05176](http://arxiv.org/abs/2305.05176) (2023)
42. Yu, S., Goudarzi, M. & Toosi, A. N. Efficient routing of inference requests across llm instances in cloud-edge computing. arXiv preprint [arXiv:2507.15553](http://arxiv.org/abs/2507.15553) (2025).

[Download references](https://citation-needed.springer.com/v2/references/10.1038/s41598-026-49736-0?format=refman&flavour=references)

## Acknowledgements

The authors would like to thank the technical staff at Universidad CEU San Pablo and UNIE Universidad for their support in providing the embedded hardware environments used for benchmarking.

## Author information

### Authors and Affiliations

1. Engineering School, CEU San Pablo University, Campus de Montepríncipe, Av. de Montepríncipe, s/n, Madrid, 28925, Spain

Rubén Juárez Cádiz
2. School of Engineering, Science, and Technology, UNIE University, Calle Arapiles, Madrid, 28015, Spain

Fernando Rodríguez-Sela

Authors

1. Rubén Juárez Cádiz

[View author publications](https://doi.org/search?author=Rub%C3%A9n%20Ju%C3%A1rez%20C%C3%A1diz)

Search author on:[PubMed](https://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=search&term=Rub%C3%A9n%20Ju%C3%A1rez%20C%C3%A1diz) [Google Scholar](https://scholar.google.co.uk/scholar?as_q=&num=10&btnG=Search+Scholar&as_epq=&as_oq=&as_eq=&as_occt=any&as_sauthors=%22Rub%C3%A9n%20Ju%C3%A1rez%20C%C3%A1diz%22&as_publication=&as_ylo=&as_yhi=&as_allsubj=all&hl=en)
2. Fernando Rodríguez-Sela

[View author publications](https://doi.org/search?author=Fernando%20Rodr%C3%ADguez-Sela)

Search author on:[PubMed](https://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=search&term=Fernando%20Rodr%C3%ADguez-Sela) [Google Scholar](https://scholar.google.co.uk/scholar?as_q=&num=10&btnG=Search+Scholar&as_epq=&as_oq=&as_eq=&as_occt=any&as_sauthors=%22Fernando%20Rodr%C3%ADguez-Sela%22&as_publication=&as_ylo=&as_yhi=&as_allsubj=all&hl=en)

### Contributions

R.J.C. was responsible for the conceptualization, methodology, software development, formal analysis, and data curation. R.J.C. also wrote the original draft, prepared all visualizations, and handled supervision, project administration, and funding acquisition. F.R.-S. contributed to the methodology, conducted the validation and investigation, provided necessary resources, and performed the writing (review and editing) of the manuscript. All authors have read and agreed to the published version of the manuscript.

### Corresponding author

Correspondence to
[Rubén Juárez Cádiz](mailto:ruben.juarezcadiz@ceu.es).

## Ethics declarations

### Competing interests

The authors declare no competing interests.

## Additional information

### Publisher’s note

Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

## Supplementary Information

### [Supplementary Information. (download PDF )](https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs41598-026-49736-0/MediaObjects/41598_2026_49736_MOESM1_ESM.pdf)

## Appendix A: Mathematical derivation of hessian-aware quantization

This appendix details the rigorous transformation of the quantization objective into a lattice projection problem and provides the explicit Babai Nearest-Plane algorithm (GPTQ core).

### From Quadratic loss to metric projection

Let \textbf{w} denote pretrained weights and \hat{\textbf{w}}\in \mathcal {Q}(\textbf{w}) their quantized representation. The local error objective is:

\begin{aligned} \hat{\textbf{w}} = \arg \min \_{\textbf{q}\in \mathcal {Q}(\textbf{w})} (\textbf{w}-\textbf{q})^{\top }\textbf{H}(\textbf{w}-\textbf{q}), \end{aligned}

(A1)

where \textbf{H} is the Hessian matrix. Since \textbf{H} is positive semi-definite, we can compute its Cholesky factorization \textbf{H}=\textbf{R}^{\top }\textbf{R}, where \textbf{R} is upper triangular. Substituting this into Eq. [A1](https://doi.org/articles/s41598-026-49736-0#Equ21):

\begin{aligned} \text {Error}&= (\textbf{w}-\textbf{q})^{\top }\textbf{R}^{\top }\textbf{R}(\textbf{w}-\textbf{q}) \nonumber \\&= \Vert \textbf{R}(\textbf{w}-\textbf{q})\Vert \_2^2. \end{aligned}

(A2)

Letting \textbf{y}=\textbf{R}\textbf{w} and \hat{\textbf{y}}=\textbf{R}\textbf{q}, the problem becomes finding the lattice point \hat{\textbf{y}} closest to \textbf{y} in the transformed space. This allows the problem to be solved greedily via back-substitution, known in lattice theory as Babai’s Nearest Plane algorithm.

### The explicit algorithm

The operational procedure for this projection is detailed in Algorithm [2](https://doi.org/articles/s41598-026-49736-0#Figd).

**Algorithm 2**

[Full size image](https://doi.org/articles/s41598-026-49736-0/figures/d)

Babai nearest-plane projection (GPTQ core) under Hessian metric.

## Rights and permissions

**Open Access** This article is licensed under a Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License, which permits any non-commercial use, sharing, distribution and reproduction in any medium or format, as long as you give appropriate credit to the original author(s) and the source, provide a link to the Creative Commons licence, and indicate if you modified the licensed material. You do not have permission under this licence to share adapted material derived from this article or parts of it. The images or other third party material in this article are included in the article’s Creative Commons licence, unless indicated otherwise in a credit line to the material. If material is not included in the article’s Creative Commons licence and your intended use is not permitted by statutory regulation or exceeds the permitted use, you will need to obtain permission directly from the copyright holder. To view a copy of this licence, visit <http://creativecommons.org/licenses/by-nc-nd/4.0/>.

[Reprints and permissions](https://s100.copyright.com/AppDispatchServlet?title=Structural%20optimization%20principles%20for%20edge%20AI%20in%20motorsport%20telemetry&author=Rub%C3%A9n%20Ju%C3%A1rez%20C%C3%A1diz%20et%20al&contentID=10.1038%2Fs41598-026-49736-0%C2%A9right=The%20Author%28s%29&publication=2045-2322&publicationDate=2026-05-01&publisherName=SpringerNature&orderBeanReset=true&oa=CC%20BY-NC-ND)

## About this article

[](https://crossmark.crossref.org/dialog/?doi=10.1038/s41598-026-49736-0)

### Cite this article

Cádiz, R.J., Rodríguez-Sela, F. Structural optimization principles for edge AI in motorsport telemetry.
*Sci Rep* **16**, 20211 (2026). https://doi.org/10.1038/s41598-026-49736-0

[Download citation](https://citation-needed.springer.com/v2/references/10.1038/s41598-026-49736-0?format=refman&flavour=citation)

* Received: 08 February 2026
* Accepted: 16 April 2026
* Published: 01 May 2026
* Version of record: 02 July 2026
* DOI: https://doi.org/10.1038/s41598-026-49736-0

### Share this article

Anyone you share the following link with will be able to read this content:

Sorry, a shareable link is not currently available for this article.

Provided by the Springer Nature SharedIt content-sharing initiative

### Keywords

* [Post-training quantization](https://doi.org/search?q=Post-training%20quantization)
* [Hessian-aware quantization](https://doi.org/search?q=Hessian-aware%20quantization)
* [Topology optimization](https://doi.org/search?q=Topology%20optimization)
* [Structural lightweighting](https://doi.org/search?q=Structural%20lightweighting)
* [Stiffness matrix](https://doi.org/search?q=Stiffness%20matrix)
* [Loss Hessian](https://doi.org/search?q=Loss%20Hessian)
* [GPTQ](https://doi.org/search?q=GPTQ)
* [AWQ](https://doi.org/search?q=AWQ)
* [Edge inference](https://doi.org/search?q=Edge%20inference)
* [Motorsport telemetry](https://doi.org/search?q=Motorsport%20telemetry)
* [Digital factor of safety](https://doi.org/search?q=Digital%20factor%20of%20safety)
* [Intelligence-per-watt](https://doi.org/search?q=Intelligence-per-watt)
