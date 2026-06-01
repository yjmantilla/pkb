---
title: "DiscoverPhysics: Benchmarking LLMs for Out-of-the-Box Scientific Thinking"
source: "https://arxiv.org/html/2605.26087v1"
author:

created: 2026-06-01
description:
tags:
  - "clippings"
  - "citable"
  - "llms"
---
Matt L. Wiemann   
Princeton University  
ms0821@princeton.edu  
&Lindsay M. Smith <sup>1</sup>   <sup>2</sup>  
Princeton University  
lindsay.smith@princeton.edu  
&Peter Melchior  
Princeton University  
&Siddharth Mishra-Sharma  
Boston University  
&Andrew Gordon Wilson  
New York University  
&Pavel Izmailov  
New York University  
Carolina Cuesta-Lázaro  
Flatiron Institute  
Institute for Advanced Studies Equal contribution (order by coinflip).Work done during internship at NYU/Polymathic AI.

###### Abstract

Frontier LLMs now perform strongly across a wide range of physics evaluations, but it is hard to disentangle genuine reasoning from recall of established science. We introduce DiscoverPhysics, an interactive benchmark that asks a LLM agent to discover the laws of motion of a simulated world whose physics deliberately deviates from our own. We construct 22 worlds governed by, among others, screened and fractional-power gravity, multi-species couplings, hidden dark-matter-like particles, non-coordinate-free physics, and time-varying interactions. Each world is generated on demand by an N-body simulator, for which the agent proposes several rounds of experiments, observes raw trajectory data, and ultimately submits both a natural-language explanation of the world’s physics and a Python implementation of the inferred law. Because solving a world requires the agent to design informative experiments and revise its hypotheses, the benchmark probes long-horizon reasoning over an experimental history. We evaluate submissions along two complementary axes: trajectory MSE on held-out particles and an LLM-judged explanation score following an expert-written rubric assessing conceptual understanding of each world. Across eleven frontier models, we find that the strongest agents pass only half of the worlds and consistently fail on those where latent structure must be uncovered. Open-source models lag substantially behind commercial models, both in their ability to design informative experiments and in extracting conclusions from the data. We further find that good predictive accuracy does not guarantee high explanation quality and that conceptual understanding depends on hypothesis refinement through well-chosen experiments.

[DiscoverPhysics](https://github.com/SampsonML/DiscoverPhysics/) [Leaderboard](https://sampsonml.github.io/DiscoverPhysicsLeaderboard/)

![Refer to caption](https://arxiv.org/html/2605.26087v1/x1.png)

Figure 1: Schematic of the DiscoverPhysics benchmarking pipeline. Our benchmark does not store data products, instead data is generated on demand by a N-body simulator from the equations specified by the world definition. We then generate an initial set of particle trajectories and feed this, as well as the agent prompt about task and experiment instructions, to an LLM. The LLM agent is tasked with discovering the physical laws of the world it is operating in and can propose experiments to be run by the N-body simulator or perform parameter fitting on proposed laws. The agent may experiment for up to n rounds, after which a final law and text explanation must be submitted, both of which are then scored to determine if the agent passed.

## 1 Introduction

Large language models (LLMs) already excel at a wide range of specialized tasks like competitive math, coding and graduate-level scientific knowledge. But these represent only a fraction of the skills needed for scientific discovery in the physical sciences, which also requires generating hypotheses, designing experiments, updating beliefs based on noisy observations, and sometimes giving up preconceived notions entirely. Existing evaluations cover slices of the discovery skill space. Knowledge benchmarks measure what a model has learned; reasoning benchmarks measure inference over given inputs; symbolic regression benchmarks measure the recovery of known equation families from fixed datasets. Recent interactive physics benchmarks come closer, but they typically present canonical laws with shifted parameters and a known set of relevant variables, leaving the conceptual part of scientific discovery largely unprobed.

We introduce DiscoverPhysics, an extensible benchmark of simulated worlds whose physics is deliberately non-canonical: short-range exponentially screened forces, fractional Laplacians, hidden particle species, non coordinate-free physics, time-varying couplings, and extra dimensions are some of the examples. Agents interact with each world by placing test particles, observing their trajectories under noise, and iteratively refining their hypothesis about the underlying law. Because the laws governing these worlds are not standard, the agent must identify which features of the observed dynamics are relevant and build mechanistic models that, ideally, predict yet unseen data in this world. The worlds are curated rather than procedurally generated, but they deliberately span a diverse set of non-standard force laws, designed to probe whether LLM agents can discover unusual equations of motion through experimentation and altering their initial guesses.

Our benchmark also seeks to define evaluation metrics for scientific discovery. Predictive accuracy, often the single performance metric for ML tasks, alone does not capture what makes a scientific theory valuable. It also needs to provide a conceptual explanation of a phenomenon that generalizes beyond the observations that motivated it [^6]. The history of the natural sciences is full of examples where empirical models fit the data long before the underlying principle was identified. Alchemy attempted to explain the transformation of substances before modern notions of chemistry and elements existed. In physics, Lorentz transformations were derived empirically to explain electromagnetic phenomena before Einstein recognized they follow from a single principle: the invariance of the speed of light. Similarly, energy conservation was long observed in mechanical systems long before Noether’s theorem revealed why: conservation laws arise from continuous symmetries. These insights, not just the specific form of the equations, are what allow strong theories to generalize far beyond the initial experiments. DiscoverPhysics therefore evaluates agents along two axes. Trajectory MSE measures how well the agent’s discovered law predicts the dynamics on held-out test particles. An explanation score, computed by an LLM judge using a human-written rubric, measures whether the agent has correctly identified the conceptual features of the world that explain those dynamics.

Our contributions are:

- We introduce DiscoverPhysics, an extensible benchmark of simulated worlds with non-canonical physics that probes scientific discovery through active experimentation, including the design of informative experiments and the iterative refinement of hypotheses.
- We define a dual evaluation framework that assesses predictive accuracy (trajectory MSE) and explanatory adequacy (LLM-judged explanation score), capturing whether agents fit the data, identify the underlying principles, or both.
- We benchmark 11 frontier and open-source LLMs and find that the strongest agents pass only about half of the worlds, consistently failing on those that require uncovering latent structure such as hidden particle species or extra dimensions.
- We show that predictive accuracy and conceptual understanding can decouple: the model achieving the lowest trajectory MSE is not the one with the highest explanation score, and benefits less from additional experimental rounds, which we read as a tendency to fit data without revising its conceptual picture.
- We show that open-source models substantially lag frontier models, both in designing informative experiments and in extracting conclusions from the resulting data, with their performance largely unchanged between guided and randomized experimentation.
- We release the simulator, public world definitions, and evaluation framework to enable evaluation of future LLMs and community extensions to new worlds.<sup>1</sup>

## 2 Related work

Scientific discovery. LLMs are employed to accelerate scientific research, from probabilistic frameworks for model discovery [^18] to coding agents that evolve algorithms for open but targeted scientific problems [^12]. Systems like The AI Scientist [^9] take a most ambitious framing, automating the full research lifecycle including ideation, experimentation, and writeup. These works aim to establish that LLMs can meaningfully participate in scientific workflows, but their evaluation typically targets paper-level quality judged by LLMs rather than recovery of a ground-truth mechanism, leaving open the question of how well models perform discovery against an objective standard.

Knowledge benchmarks. Static benchmarks of scientific knowledge have served as a primary measure of LLM capabilities in technical domains. GPQA [^13] provides 448 expert-validated, "Google-proof" graduate-level questions in physics, chemistry, and biology, with a rigorous validation procedure in which domain PhDs reach 65 – 74% accuracy. Current state-of-the-art LLMs achieve over 90% accuracy. However, strong performance on GPQA reflects what a model has learned about known science, but not what it can discover.

Reasoning benchmarks. A complementary line of work targets reasoning rather than memorized knowledge. ARC [^5] introduces few-shot puzzles explicitly designed to resist memorization through novel task generation. While humans solve ARC tasks with near-perfect accuracy, AI systems show wide performance variation, making it one of the most demanding tests of general reasoning. Whilst ARC measures reasoning, scientific discovery requires reasoning from experimentation in noisy environments, where the relevant features must themselves be inferred.

Symbolic regression and equation discovery. A long line of work evaluates the recovery of physical equations from data. Recently, LLM-based approaches such as LLM-SR [^16] use language models as priors to accelerate equation discovery, and LLM-SRBench [^17] evaluates these capabilities while avoiding memorization of known physics. More recent agentic frameworks such as KeplerAgent [^19] infer physical structure such as symmetries from data and translate it into constraints, restricting the search space of downstream symbolic regression algorithms. Our goals depart from symbolic regression more fundamentally: scientific discovery is not merely about retrieving an equation that fits the data. Symbolic regression methods often interpolate observations through complex functional forms, which bear little resemblance to the generative mechanism. Even when a concise equation is recovered, the act of discovery then depends a human scientists to recognize the conceptual content of the underlying physics. Our benchmark evaluates conceptual understanding in addition to trajectory MSE, asking not only whether the agent produces a law that predicts the dynamics but whether it has identified the structural features that explain them.

Interactive scientific discovery. A growing body of work evaluates the scientific capabilities of LLMs through interactive worlds. DiscoveryWorld [^7] introduces 120 scientific tasks in a text-based simulator and proposes an evaluation scheme that separates task completion from explanatory knowledge graded against gold references. Other benchmarks evaluate data-driven hypothesis search [^10], agent performance on tasks from peer-reviewed publications [^4], closed-loop experimentation in biology [^15] [^11], and causal-graph discovery via interventions [^2]. [^14] further analyze agent reasoning traces across eight scientific domains and find that evidence is ignored in 68% of traces and refutation-driven belief revision occurs in only 26%, even when agents arrive at correct answers. This suggests that outcome-based metrics miss systematic reasoning failures. While these works establish that interactive evaluation is essential, none directly tests whether an agent recovers a ground-truth generative mechanism in a setting where the underlying physics is genuinely unusual.

Physics discovery benchmarks. The closest precedents to our work specifically target physics. NewtonBench [^21] introduce 324 tasks across 12 physics domains via alterations of canonical laws. They report that frontier models reach roughly 30% accuracy on complex tasks, with sperformance halving under minimal observational noise. PhysGym [^3] provides 97 interactive physics problems with controlled prior-knowledge levels, allowing the contribution of a model’s prior physical knowledge to be cleanly separated from its discovery ability. Gravity-Bench [^8], focuses on agentic discovery in two-body gravitational dynamics with budget-limited observations. In all these benchmarks, the structure of the discovery problem is largely given. NewtonBench shifts the parameters of canonical laws while preserving their functional family, PhysGym exposes named controllable variables and asks for closed-form relationships in those variables, and Gravity-Bench restricts to two-body dynamics where the relevant degrees of freedom are evident. Real scientific breakthroughs often rely on identifying which variables matter, recognizing hidden degrees of freedom, and distinguishing signal from noise. Our worlds are designed to probe these capabilities.

## 3 The DiscoverPhysics Benchmark

This benchmark suite evaluates whether a LLM agent can infer the physical laws governing a simulated world through active experimentation. Because all trajectories are generated at run-time by a simulator, the benchmark depends on no static data products and can be extended to arbitrary force laws, noise levels, and particle counts. Each evaluation pairs an agent with a world whose force law is hidden, an experimental interface for proposing and running experiments, and two final metrics: predictive accuracy on held-out trajectories and explanatory agreement with a human-written rubric.

### 3.1 Overview

The unit of evaluation is a single session in which the agent is given a fixed budget of experimentation rounds to discover the hidden law of one world. Within this budget, the agent can freely alternate between running experiments and proposing candidate laws. At the end of the budget it must commit to a final answer.

A single experimental round works as follows. The agent submits a list of test particles, each specified by an initial position, initial velocity, charge, and a list of measurement times. The simulator integrates these particles under the world’s hidden law together with any pre-existing particles in the world, and returns the requested positions and velocities at each measurement time. The agent may also propose a candidate law and request a parameter fit against the trajectories observed so far.

Once the round budget is exhausted, the agent submits two final outputs: a short natural-language explanation of the world’s physics and a Python implementation of the inferred law. These are then scored on trajectory MSE against held-out test particles and on explanation quality judged against a human-written rubric (see Section 3.3).

The interactive experimentation loop (visualized in Figure 1) is central to the benchmark because the problems are not solvable as pure curve fitting. The agent must decide which regions of phase space to probe, which quantities to vary, and whether to intervene by changing charges, placing probes at short or long range, or repeating experiments at different evaluation times.

To isolate experimental design from reasoning over existing data, we provide an optional randomized mode in which the per-round particle initial conditions are sampled from a uniform grid rather than chosen by the agent. The agent still observes the resulting trajectories and proposes laws as usual, so any performance gap between the two modes can be attributed to the agent’s choice of experiments.

Every discovery session opens with the same universal prompt:

<svg id="S3.SS1.p7.pic1" height="6960.31" overflow="visible" version="1.1" viewBox="0 0 600 6960.31" width="600"><g style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,6960.31) matrix(1 0 0 -1 0 0)" fill="#000000" stroke="#000000" stroke-width="0.4pt"><g style="--ltx-fill-color:#868686;" fill="#868686" fill-opacity="1.0"><path style="stroke:none" d="M 0 0 L 0 6960.31 L 600 6960.31 L 600 0 Z"></path></g><g style="--ltx-fill-color:#ECECEC;" fill="#ECECEC" fill-opacity="1.0"><path style="stroke:none" d="M 1.38 1.38 L 1.38 6937.37 L 598.62 6937.37 L 598.62 1.38 Z"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 10.86 6945.38)"><foreignObject style="--ltx-fg-color:#FFFFFF;--ltx-fo-width:41.79em;--ltx-fo-height:0.69em;--ltx-fo-depth:0.19em;" width="578.29" height="12.3" transform="matrix(1 0 0 -1 0 9.61)" overflow="visible" color="#FFFFFF"><span id="S3.SS1.p7.pic1.1.1.1.1.1" style="width:36.34em;"><span id="S3.SS1.p7.pic1.1.1.1.1.1.1"><span id="S3.SS1.p7.pic1.1.1.1.1.1.1.1">Universal Prompt</span></span> </span></foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 10.86 6918.29)"><foreignObject style="--ltx-fg-color:#000000;--ltx-fo-width:41.79em;--ltx-fo-height:0.69em;--ltx-fo-depth:499.2em;" width="578.29" height="6917.04" transform="matrix(1 0 0 -1 0 9.61)" overflow="visible" color="#000000"><span id="S3.SS1.p7.pic1.2.2.2.1.1" style="width:40.88em;"><span id="S3.SS1.p7.pic1.2.2.2.1.1.1"><span id="S3.SS1.p7.pic1.2.2.2.1.1.1.1">“You are an expert physicist and AI research scientist tasked with discovering scientific laws in a simulated universe. Your goal is to propose experiments, analyze the data they return, and ultimately deduce the underlying scientific law. Please note that the laws of physics in this universe may differ from those in our own. You can perform experiments to gather data, but you must follow the protocol strictly.”</span></span></span></foreignObject></g></g></svg>

Thereafter we provide a short, world-specific protocol describing the JSON schema for experiments and how to invoke the parameter-fitting tool. Crucially, the agent receives no information about the world’s force law, field equation, source coupling, or the existence of hidden particle types. All of these must be inferred from the trajectories returned by the simulator.

Final output. For evaluation, the models must give a final output that includes both a short plain-text explanation of the physical laws that govern the world and a physical law in the form of a Python function definition to evolve particles according to the discovered law. Along with the function definition, the agent may present up to five parameters, which will then be fit to the observable data before the final code is executed. We show an example output below, where we exclude the majority of the python code for brevity. Full example outputs for each world are in Appendix E.

[⬇](data:text/plain;base64,ZGVmIGRpc2NvdmVyZWRfbGF3KHBvczEsIHBvczIsIHAxLCBwMiwgdmVsb2NpdHkyLCBkdXJhdGlvbiwgKipwYXJhbXMpOgogICAgIiIiUGFydGljbGUgMiBpcyBhY2NlbGVyYXRlZCB0b3dhcmQgcGFydGljbGUgMSBieSBhIHJhZGlhbCBmb3JjZSBwZXIgdW5pdCBhbG9uZyByXiB0aGF0IGNvbWJpbmVzIGEgc3RhdGljIHNvdXJjZSB0ZXJtIEcqcDEsIGEgcmFkaWFsLXZlbG9jaXR5IHRlcm0gKGFuYWxvZ291cyB0byBhbiBhZHZlY3RpdmUvcmV0YXJkYXRpb24gY291cGxpbmcpLCBhbmQgYSBjZW50cmlwZXRhbC1saWtlIHRhbmdlbnRpYWwga2luZXRpYy1lbmVyZ3kgdGVybSwgYWxsIHNjYWxlZCBieSAxL3IgYW5kIGluZGVwZW5kZW50IG9mIHAyLiIiIgogICAgLi4uCiAgICByZXR1cm4gdHJhamVjdG9yaWVzCgpkZWYgZml0X3BhcmFtZXRlcnMoKToKICAgIHJldHVybiB7CiAgICAgICAgIkciOiAgICAgeyJpbml0IjogMS4wLCAiYm91bmRzIjogWzAuMSwgNS4wXX0sCiAgICAgICAgLi4uCiAgICB9)

def discovered\_law(pos1, pos2, p1, p2, velocity2, duration, \*\*params):

"""Particle 2 is accelerated toward particle 1 by a radial force per unit along r^ that combines a static source term G\*p1, a radial-velocity term (analogous to an advective/retardation coupling), and a centripetal-like tangential kinetic-energy term, all scaled by 1/r and independent of p2."""

...

return trajectories

def fit\_parameters():

return {

"G": {"init": 1.0, "bounds": \[0.1, 5.0\]},

...

}

Agentic experimentation and tool use. Importantly, we consider the agents to be an active participant in these worlds. As such, we provide the ability to run experiments, which are critical to determine the laws of the world. The experiments are conducted via a N-body simulator we provide. Each experiment is defined by JSON-formatted text, which specifies initial conditions for the controllable particles plus a list of measurement times. The simulator integrates from $t=0$ (or an optional start\_time) and returns positions and velocities at each requested time. Trajectories from every experiment are appended to a per-run CSV log. The agent can use an optional tool that performs a least-squares fit of the parameters of its current candidate law against this CSV log using scipy.optimize. We use the same tool to evaluate the final MSE. The fit results are returned in the form of a text prompt which is given to the science agent at the beginning of the next round.

### 3.2 Physics simulator and world construction

The worlds are defined by their pairwise force laws, evolved by an N-body simulator with interchangeable integrators: RK4, 4th- and 6th-order Yoshida [^20], Dormand-Prince (jax.experimental.ode.odeint) [^1], symplectic Euler (kick-drift), and leapfrog (velocity-Verlet). This lets the user (but not the agent) trade simulation precision against compute. We use Yoshida4 by default because it is symplectic, 4th-order accurate, and fast under JIT compilation. A small softening term is added to avoid numerical instabilities.

Each particle carries a kinematic state: position $\mathbf{r}_{i}$ and velocity $\mathbf{v}_{i}$, together with a set of generalized charges $\mathbf{q}_{i}$ that determine how it couples to forces. (In physics, a charge is any intrinsic property that sets a particle’s response to a given interaction: e.g. mass is the charge of the gravitational force). The pairwise force takes the general form

$$
\mathbf{F}_{i\leftarrow j}\;=\;F_{\mathrm{mag}}\!\left(r_{ij},\,\mathbf{q}_{i},\,\mathbf{q}_{j},\,t\right)\,\hat{\mathbf{r}}_{ij},
$$

where $r_{ij}=\|\mathbf{r}_{i}-\mathbf{r}_{j}\|$ and $\hat{\mathbf{r}}_{ij}$ is the unit separation vector.

Letting $\mathbf{q}_{i}$ be a vector rather than a scalar lets a single world contain multiple particle species with distinct couplings. For instance, three species with distinct pairwise interactions. The explicit time dependence accommodates time-modulated couplings, as in our oscillator world, where the coupling strength periodically reverses sign. In our implementation we decompose $\mathbf{q}_{i}\equiv(s_{i},c_{i})$ into a source charge $s_{i}$ controlling how strongly particle $i$ generates the field, and a response charge $c_{i}$ controlling how strongly it feels the field generated by others. This lets a single world contain pure sources ($c_{i}=0$), neutral test probes ($s_{i}=0$), and ordinary particles that both source and respond ($s_{i}=c_{i}$) without separate code paths. When $s_{i}=c_{i}$ for all particles, the formalism reduces to a standard symmetric pairwise interaction.

Because the simulator evaluates pairwise interactions directly, its computational cost scales as $\mathcal{O}(N^{2})$. For the modest particle counts used in our worlds (typically $N\lesssim 35$), the computational cost is low. The benefit of this design is flexibility: arbitrary, non-standard interaction laws can be specified without solving a corresponding PDE. Optionally, Gaussian noise can be added to simulator outputs, controlled by a CLI parameter; the noise level used in our main results (Table 1, Figure 2) is $\sigma=0.05$, representing $5\%$ observation noise compared to the test particle variance, while held-out test trajectories used at evaluation time are noise-free.

A full description of the worlds’ governing principles and parameters is given in Appendix C.

### 3.3 Evaluation metrics

Trajectory MSE. Once the agent submits its final law, we fit its free parameters to the data the agent has already observed and roll out the resulting law on a set of held-out test particles. The trajectory MSE is the mean squared Euclidean distance between true and predicted positions across these held-out trajectories. The held-out trajectories are noise-free. We report the geometric mean across worlds to reduce the influence of occasional large outliers.

Explanation score. We present each physics world with a rubric of human-labeled explanation scores from $0\to 10$. An example of a rubric is given in Appendix D. After the science-agent presents its final text summary of the physical laws of the world we use a separate LLM (always claude-opus-4-6, which is not in our reported baselines) to compare the given explanation to the human written rubric and provide a score. The explanation score is a proxy for the conceptual understanding of the system.

Worlds pass@ $k$. We report the expected worlds passed from $k$ independent attempts as pass@ $k$. A world is considered a pass if the per-trajectory normalized MSE is below 0.1 ($10\%$ error) and the explanation score is $\geq 0.9$. The MSE normalization is done by calculating the variance of the test particle trajectories and dividing the MSE accordingly to account for the difference in total particle travel in different worlds. To calculate pass@ $k$ we randomly sample without replacement from each of the 5 attempts per model, $k$ times per world, and a single passing attempt per world is sufficient to count the world as passed. For instance, pass@3 is computed by randomly sampling 3 seeds per world, recording how many worlds had at least one pass. We repeat the sampling 1,000 times to calculate the expectation value and standard errors, which are reported in Table 1.

Held out worlds. We publicly release the setup and evaluation rubrics for 11 public worlds. However, to ensure the validity of this benchmark, we keep the remaining 11 worlds private. All results are shows over the full suite of 22 worlds, so we redact the names of the private worlds in all figures and results.

Table 1: Results of the baseline testing of 11 LLM models on the DiscoverPhysics benchmark, reporting the mean explanation score, per-particle mean position error, and the expected percentage of worlds pass@ $k$ as a percentage.

<table><tbody><tr><th>Model</th><td><math><semantics><mrow><mo>⟨</mo> <mtext>Explanation</mtext> <mo>⟩</mo></mrow> <annotation>\langle\text{Explanation}\rangle</annotation></semantics></math></td><td>norm <math><semantics><mrow><mo>⟨</mo> <mtext>MSE</mtext> <mo>⟩</mo></mrow> <annotation>\langle\text{MSE}\rangle</annotation></semantics></math></td><td><math><semantics><mrow><mi>pass</mi> <mo></mo><mi>@</mi> <mo></mo><mn>1</mn></mrow> <annotation>\rm{pass}@1</annotation></semantics></math></td><td><math><semantics><mrow><mi>pass</mi> <mo></mo><mi>@</mi> <mo></mo><mn>2</mn></mrow> <annotation>\rm{pass}@2</annotation></semantics></math></td><td><math><semantics><mrow><mi>pass</mi> <mo></mo><mi>@</mi> <mo></mo><mn>3</mn></mrow> <annotation>\rm{pass}@3</annotation></semantics></math></td><td><math><semantics><mrow><mi>pass</mi> <mo></mo><mi>@</mi> <mo></mo><mn>4</mn></mrow> <annotation>\rm{pass}@4</annotation></semantics></math></td><td><math><semantics><mrow><mi>pass</mi> <mo></mo><mi>@</mi> <mo></mo><mn>5</mn></mrow> <annotation>\rm{pass}@5</annotation></semantics></math></td></tr><tr><th colspan="8">Proprietary</th></tr><tr><th>claude-opus-4-7</th><td><math><semantics><mrow><mn>0.61</mn> <mo>±</mo><mn>.03</mn></mrow> <annotation>\mathbf{0.61\pm.03}</annotation></semantics></math></td><td><math><semantics><msubsup><mn>0.01</mn><mn>.004</mn><mn>.006</mn></msubsup> <annotation>0.01^{.006}_{.004}</annotation></semantics></math></td><td><math><semantics><mrow><mn>26.4</mn> <mo>±</mo><mn>.3</mn></mrow> <annotation>\mathbf{26.4\pm.3}</annotation></semantics></math></td><td><math><semantics><mrow><mn>36.8</mn> <mo>±</mo><mn>.2</mn></mrow> <annotation>\mathbf{36.8\pm.2}</annotation></semantics></math></td><td><math><semantics><mrow><mn>43.1</mn> <mo>±</mo><mn>.2</mn></mrow> <annotation>\mathbf{43.1\pm.2}</annotation></semantics></math></td><td><math><semantics><mrow><mn>47.2</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>\mathbf{47.2\pm.1}</annotation></semantics></math></td><td><math><semantics><mn>50.0</mn> <annotation>\mathbf{50.0}</annotation></semantics></math></td></tr><tr><th>claude-sonnet-4-6</th><td><math><semantics><mrow><mn>0.28</mn> <mo>±</mo><mn>.02</mn></mrow> <annotation>0.28\pm.02</annotation></semantics></math></td><td><math><semantics><msubsup><mn>2.8</mn> <mn>2.6</mn> <mn>10.4</mn></msubsup> <annotation>2.8^{10.4}_{2.6}</annotation></semantics></math></td><td><math><semantics><mrow><mn>1.8</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>1.8\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>3.7</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>3.7\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>5.5</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>5.5\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>7.2</mn> <mo>±</mo><mn>.05</mn></mrow> <annotation>7.2\pm.05</annotation></semantics></math></td><td><math><semantics><mn>9.1</mn> <annotation>9.1</annotation></semantics></math></td></tr><tr><th>claude-haiku-4-5</th><td><math><semantics><mrow><mn>0.23</mn> <mo>±</mo><mn>.02</mn></mrow> <annotation>0.23\pm.02</annotation></semantics></math></td><td><math><semantics><msubsup><mn>3.1</mn> <mn>1.4</mn> <mn>2.6</mn></msubsup> <annotation>3.1^{2.6}_{1.4}</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td></tr><tr><th>gpt-5.5</th><td><math><semantics><mrow><mn>0.59</mn> <mo>±</mo><mn>.03</mn></mrow> <annotation>0.59\pm.03</annotation></semantics></math></td><td><math><semantics><msubsup><mn>0.002</mn><mn>.001</mn><mn>.001</mn></msubsup> <annotation>\mathbf{0.002}^{.001}_{.001}</annotation></semantics></math></td><td><math><semantics><mrow><mn>21.7</mn> <mo>±</mo><mn>.2</mn></mrow> <annotation>21.7\pm.2</annotation></semantics></math></td><td><math><semantics><mrow><mn>27.6</mn> <mo>±</mo><mn>.2</mn></mrow> <annotation>27.6\pm.2</annotation></semantics></math></td><td><math><semantics><mrow><mn>30.8</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>30.8\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>33.7</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>33.7\pm.1</annotation></semantics></math></td><td><math><semantics><mn>36.4</mn> <annotation>36.4</annotation></semantics></math></td></tr><tr><th>gpt-5.4</th><td><math><semantics><mrow><mn>0.29</mn> <mo>±</mo><mn>.03</mn></mrow> <annotation>0.29\pm.03</annotation></semantics></math></td><td><math><semantics><msubsup><mn>6.7</mn> <mn>4.5</mn> <mn>13.9</mn></msubsup> <annotation>6.7^{13.9}_{4.5}</annotation></semantics></math></td><td><math><semantics><mrow><mn>0.9</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>0.9\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>1.7</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>1.7\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>2.7</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>2.7\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>3.6</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>3.6\pm.1</annotation></semantics></math></td><td><math><semantics><mn>4.5</mn> <annotation>4.5</annotation></semantics></math></td></tr><tr><th colspan="8">Open-source</th></tr><tr><th>gpt-oss-120b</th><td><math><semantics><mrow><mn>0.22</mn> <mo>±</mo><mn>.02</mn></mrow> <annotation>0.22\pm.02</annotation></semantics></math></td><td><math><semantics><msubsup><mn>1.6</mn> <mn>0.9</mn> <mn>1.9</mn></msubsup> <annotation>1.6^{1.9}_{0.9}</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td></tr><tr><th>gpt-oss-20b</th><td><math><semantics><mrow><mn>0.14</mn> <mo>±</mo><mn>.02</mn></mrow> <annotation>0.14\pm.02</annotation></semantics></math></td><td><math><semantics><msubsup><mn>0.7</mn> <mn>0.3</mn> <mn>0.5</mn></msubsup> <annotation>0.7^{0.5}_{0.3}</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td></tr><tr><th>Qwen3-235B-A22B</th><td><math><semantics><mrow><mn>0.24</mn> <mo>±</mo><mn>.02</mn></mrow> <annotation>0.24\pm.02</annotation></semantics></math></td><td><math><semantics><msubsup><mn>1.0</mn> <mn>0.5</mn> <mn>0.9</mn></msubsup> <annotation>1.0^{0.9}_{0.5}</annotation></semantics></math></td><td><math><semantics><mrow><mn>1.0</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>1.0\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>1.8</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>1.8\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>2.7</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>2.7\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>3.7</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>3.7\pm.1</annotation></semantics></math></td><td><math><semantics><mn>4.5</mn> <annotation>4.5</annotation></semantics></math></td></tr><tr><th>Qwen3.5-397B-A17B</th><td><math><semantics><mrow><mn>0.33</mn> <mo>±</mo><mn>.03</mn></mrow> <annotation>0.33\pm.03</annotation></semantics></math></td><td><math><semantics><msubsup><mn>0.1</mn><mn>.04</mn><mn>.07</mn></msubsup> <annotation>0.1^{.07}_{.04}</annotation></semantics></math></td><td><math><semantics><mrow><mn>4.5</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>4.5\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>7.3</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>7.3\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>8.6</mn> <mo>±</mo><mn>.01</mn></mrow> <annotation>8.6\pm.01</annotation></semantics></math></td><td><math><semantics><mn>9.1</mn> <annotation>9.1</annotation></semantics></math></td><td><math><semantics><mn>9.1</mn> <annotation>9.1</annotation></semantics></math></td></tr><tr><th>Kimi-K2.5</th><td><math><semantics><mrow><mn>0.29</mn> <mo>±</mo><mn>.02</mn></mrow> <annotation>0.29\pm.02</annotation></semantics></math></td><td><math><semantics><msubsup><mn>0.8</mn><mn>.4</mn> <mn>1.1</mn></msubsup> <annotation>0.8^{1.1}_{.4}</annotation></semantics></math></td><td><math><semantics><mrow><mn>2.6</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>2.6\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>4.8</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>4.8\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>6.8</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>6.8\pm.1</annotation></semantics></math></td><td><math><semantics><mrow><mn>8.2</mn> <mo>±</mo><mn>.1</mn></mrow> <annotation>8.2\pm.1</annotation></semantics></math></td><td><math><semantics><mn>9.1</mn> <annotation>9.1</annotation></semantics></math></td></tr><tr><th>Llama-3.3-70B</th><td><math><semantics><mrow><mn>0.10</mn> <mo>±</mo><mn>.01</mn></mrow> <annotation>0.10\pm.01</annotation></semantics></math></td><td><math><semantics><msubsup><mn>14.1</mn> <mn>10.1</mn> <mn>35.8</mn></msubsup> <annotation>14.1^{35.8}_{10.1}</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td><td><math><semantics><mn>0.0</mn> <annotation>0.0</annotation></semantics></math></td></tr></tbody></table>

![Refer to caption](https://arxiv.org/html/2605.26087v1/x2.png)

Figure 2: Performance metrics of the suite of 11 tested LLM models. From left to right, we show a Pareto plot of evaluation score as a function of normalized MSE, expected pass@k as a function of k attempts, and expected pass@3 as a function of model release date.

## 4 Experiments

We present the results of benchmarking eleven pretrained LLMs acting as science agents, using the API-default reasoning settings for gpt-5.5 (medium) and claude-opus-4-7 (high), which are roughly at the mid-point of the reasoning effort spectrum; higher reasoning effort could potentially yield higher scores on both models, and the numbers reported here should be read as a lower bound at default settings rather than a ceiling. API-default sampling temperature is used as well. For each of the 22 physical worlds, we gather results across five seeds, presenting the mean and bootstrapped standard error for all reported results with 5,000 bootstrap resamples.

Model performance comparison. In Table 1 we show the full results for our benchmark over a series of open and proprietary models for the mean explanation score, normalized MSE, and the expected worlds passed with $k$ attempts. This data is visualized in Figure 2. claude-opus-4-7 performs best in terms of explanation score for each world and the expected pass rate at all $k$ attempts, while gpt-5.5 has the overall lowest MSE.

Guided vs random experimentation. In Figure 3 we compare the performance of gpt-5.5 and claude-opus-4-7 as a function of experimental rounds. We see qualitative differences between the two models. For the guided experiments, claude-opus-4-7 shows monotonic improvement across all 3 performance metrics as the number of experimental rounds increases, with the final pass rate at $k=3$, and evaluation score higher than gpt-5.5, however with a higher mean MSE. In contrast, gpt-5.5 initially far exceeds the performance of claude-opus-4-7 at the lower round budgets of 2, 4, and 8 in both explanation score and MSE, but sees no significant improvement in the pass rate or explanation score after round 4, while MSE still continue to decrease. Notably, at round budgets of 2, 4, and 8, the judge rates gpt-5.5’s explanations strictly higher than claude-opus-4-7s, with opus-4-7 only surpassing gpt-5.5 at 16 rounds on the explanation axis. A judge biased toward its own model family would be unlikely to produce this pattern across three of four round budgets.

We also compare agent-guided and randomly selected experiments at each round. In the LLM-guided experiments, the agent chooses the positions of the particles it tests in each experimental round. In the random experiments, the positions are chosen at random. The agent still sees the results of the time evolution of the particles from the simulator. This ablation determines whether the agents are capable of choosing insightful experiments to aid their analytic capabilities. Figure 3 indicates that claude-opus-4-7 uses its experiments more wisely and learns more from new data than gpt-5.5. We note due to the unavailability of the exact amount of reasoning/thinking tokens used by the OpenAI models, we did not perform an analysis of performance as a function of tokens.

![Refer to caption](https://arxiv.org/html/2605.26087v1/x3.png)

Figure 3: Performance metrics of claude-opus-4-7 and gpt-5.5 as a function of allowed experimental rounds. We show (from left to right) the expected pass rate at k = 3 k=3, explanation score averaged over all worlds, and normalized MSE averaged over all worlds. We also show the results of random experimentation in the cross-hatched colorbars for both models.

World difficulty. Figure 5 shows a heatmap of the mean explanation score across five attempts for every physics world. The worlds are ordered from easiest to hardest based on median explanation scores for the strongest model. While the oscillator world proved most difficult for most models, gpt-5.5 and claude-opus-4-7 perform well on this test, while they struggle more than others on the coulomb world, demonstrating that their explanatory power is not uniformly higher.

Noise ablation. Figure 4 shows a noise ablation on two worlds (Yukawa and Ether) for our top models. Increasing observational noise generally reduces the explanation score and increases MSE, but with notable per-world differences. Yukawa contains only 2 particles versus Ether’s 26, so Gaussian noise degrades the Yukawa signal much faster, the explanation scores collapse beyond $20\%$ noise. Surprisingly, claude-opus-4-7 on Ether retains high explanation scores even at $50\%$ noise. In the output logs we see for these trials the model recognises the noise and designs experiments that mitigate its effect; we include a sample response in Appendix B. Note that this ablation adds Gaussian noise to both position and velocity, whereas the main results in Table 1 and Figure 2 use position-only noise; we chose the harsher regime to stress-test the ablation, and expect model rankings to be preserved under either convention.

![Refer to caption](https://arxiv.org/html/2605.26087v1/x4.png)

Figure 4: Noise ablations over the Yukawa (top) and Ether (bottom) worlds for claude-opus-4-7 and gpt-5.5. We show the evaluation score, and normalised MSE as a function of observation noise added as a fraction of total trajectory variance (as in the MSE normalization scaling).

![Refer to caption](https://arxiv.org/html/2605.26087v1/x5.png)

Figure 5: Top: Mean explanation score per model (rows) and world (columns). The cell values show the mean explanation score between 0 → 1 0\\to 1, averaged over all 5 seeds for each model. Bottom: Distribution of explanation scores by world, pooled across all models and seeds. Each violin shows the score distribution for one world; the horizontal bar marks the median. Worlds are sorted left to right by from easiest to hardest based on claude-opus-4-7 performance.

## 5 Discussion

The benchmark presented here is designed to reveal the progress and the limitations of current LLMs to perform open-ended discovery of physical laws through experimentation. The strongest models we tested, claude-opus-4-7 and gpt-5.5, pass about half of the worlds in the benchmark, a decidedly non-trivial achievement given the complexity of the task. These frontier models represent a substantial improvement over their predecessors, not just in reasoning over given data but in actively designing informative experiments. As shown in Figure 3, claude-opus-4-7 and gpt-5.5 outperform their own randomized-experiment baselines, with claude-opus-4-7 in particular showing monotonic gains in pass rate and explanation score with increasing round budget. This widening gap between guided and random experimentation indicates that the benchmark rewards genuine long-horizon reasoning over an experimental history.

On the other hand, even the strongest models fail to solve the more difficult worlds, which are characterized by important latent structure (e.g. three particle species, dark matter, and extra dimensions). As we show with several LLM-proposed explanations and discovered laws in Appendix E, failure is caused primarily by insufficient experimentation (see Figures 6 and 7 for an instructive example, where not probing enough measurement times to find the time-varying force law leads to a failure). A more systematic per-run analysis of every failure by the two strongest agents, organised along seven capability axes, is given in Appendix F.

We find a significant skill gap between proprietary and open-source models. Open-source models show little difference between guided and random experiments, suggesting they do not meaningfully exploit their experimental budget. The most performant open model, Qwen-3.5, solves only about $10\%$ of the worlds in five attempts and scores low on both trajectory MSE and explanation relative to the proprietary variants.

Our dual evaluation also reveals meaningful differences amongst the strongest proprietary models. gpt-5.5 achieves the lowest trajectory MSEs usually without achieving the highest explanation scores. It also gains less explanation score through multiple experiment rounds compared to claude-opus-4-7, which we interpret as a tendency to lock in a candidate law early and refine its parameters rather than revise its conceptual picture, i.e. fitting the data well without necessarily understanding it. The capability-gap decomposition in Appendix F is consistent with this picture: gpt-5.5 failures concentrate on considering the right kind of law in the first place, whereas claude-opus-4-7 failures spread more across experimental design and self-monitoring.

#### Limitations and future extensions.

DiscoverPhysics is limited in world generation and evaluation, both of which suggest future extensions that would increase its usefulness. The worlds, while non-canonical, are deliberately curated rather than genuinely novel: the underlying physics is unusual but designed by us, and the systems are small in scale, with few particles, no instrumental systematics, and no observational realism beyond a small amount of Gaussian noise. The set of worlds is also still modest in size. Although the simulator is built to accept arbitrary force laws, extending the suite requires human effort to design conceptually interesting worlds and write scoring rubrics for them.

Our evaluation methodology has limitations as well. The explanation score relies on a single LLM judge (claude-opus-4-6); ideally this would be averaged over multiple judge models to reduce any single-model bias. The fact that the judge ranks gpt-5.5 above claude-opus-4-7 on MSE and rates them comparably at low round budgets (Figure 3) is reassuring evidence against naive Claude-on-Claude favoritism, but does not fully resolve the concern. The pass/fail thresholds we use (10% normalized MSE, 0.9 explanation score) are also somewhat arbitrary; we mitigate this by reporting mean explanation scores and normalized MSE alongside pass@k, so that aggregate trends do not hinge on a particular cutoff.

Finally, all models show pass@5 substantially larger than pass@1, which suggests that reinforcement learning could meaningfully improve scores on this task. We caution, however, that this is evidence RL can improve performance on DiscoverPhysics specifically, not that the resulting capabilities would generalize to scientific discovery more broadly. Assessing such transfer would require further work.

## 6 Conclusion

We present the benchmark DiscoverPhysics to test the abilities of agentic models to discover the physical laws governing simulated worlds, whose physics deviates considerably from standard textbook cases. The agent has to propose experiments over several rounds that are carried out with a black-box N-body simulator, with the goal of describing, in words and Python code, the physical principles the simulated worlds follow. We assess the model performance with a dual evaluation scheme, which comprises a conventional predictive accuracy metric as well as an explanation score from a LLM that judges the proposed physical explanation against a expert human description of the true underlying physical relations. This dual assessment reflects the goal of physics research to decode through experiments the latent principles of our world in ways that generalize beyond existing observations. To our knowledge, DiscoverPhysics is the strongest test of how well LLMs perform this ill-defined, iterative research process of hypothesis generation and refinement on the basis of self-proposed experiments. We find that the largest, most recent LLMs perform best and discover the correct explanation for about 50% of the worlds within five attempts. However, they employ different research strategies. While gpt-5.5 finds excellent predictive accuracy quickly by adopting flexible governing equations, claude-opus-4-7 is able to refine its conceptual guesses and therefore find more accurate explanations for the most unconventional worlds, demonstrating that the act of designing insightful experiments is critical for discovery in the physical sciences.

## Acknowledgments and Disclosure of Funding

The authors thank Polymathic AI for funding and support.

## References

## Appendix A Example experimentation process

In Figures 6 and 7 we show two examples of the experimentation process for Claude Opus 4.7 on the oscillator world, differing solely by the random seed we used for the agent. Both figures show a summary of a full run on this world with a maximum of 16 possible experimentation rounds.

In Figure 6, the agent decided to submit its final discovered law after four rounds. In round 1, the agent proposes an initial sweep of probe particle positions and times. It notices from the results of round 1 that the velocities of the probes are oscillating in time. It then decides to carefully probe the time-dependent force causing this oscillation in order to find the frequency and structure of the force in round 2. It submits much longer times to measure the probes at, and then in round 3, it analyzes these results and concludes that there must be explicit time-dependence in the force law, and proposes a fitting request to verify this. In round 4, it analyzes the results of the fit, is confident that it got the free parameters correct, and then submits its final answer.

In Figure 7, we contrast the successful experimentation in Figure 6 with a failed attempt just by changing the random seed for sampling. Similarly, in round 1 the agent proposes an initial sweep of probe particle positions and times. It guesses a $\frac{1}{r}$ force law due to the behavior of the particles at short times. It then decides to submit experiments at very short timescales, where the time-dependent behavior is not detectable. In round 3, it submits an MSE fitting request, but there was an error and the fit did not go through. It chooses to continue to probe even smaller timescales in round 4 and continues to trust its “discovered” force law of $\frac{1}{r}$. In round 5, it is confident that it got the law correct, and then submits its final answer.

![Refer to caption](https://arxiv.org/html/2605.26087v1/x7.png)

Figure 6: A summary of the experimentation process for an example run of Claude Opus 4.7 on the oscillator world. The model is ultimately successful in discovering the underlying force law due to long timescale experiments. We highlight the naive initial experiment, the “aha moment” of the model after it analyzes the naive experiment, the clever experimentation (choosing a timescale for its experiments that allows for the detection of the time-dependent force), and the finalization of its discovered law. The visualization of the model’s experiments are shown on the right. r ( t ) r(t) represents the position of the particles in the simulation, with the noisy observations of the model’s proposed probe particles marked by an x, the ground-truth force law shown with solid lines, and the agent’s proposed final law shown with dashed lines. The final evaluation is shown on held-out test particles the model never sees.

![Refer to caption](https://arxiv.org/html/2605.26087v1/x8.png)

Figure 7: A summary of the experimentation process for an example run of Claude Opus 4.7 on the oscillator world. The model fails to discover the true underlying force law. We highlight the naive initial experiment, the false discovery of the model early on, and the smaller timescales in experimentation (missing the detection of the time-dependent force), and the finalization of its discovered law. The visualization of the model’s experiments are shown on the right. r ( t ) r(t) represents the position of the particles in the simulation, with the noisy observations of the model’s proposed probe particles marked by an x, the ground-truth force law shown with solid lines, and the agent’s proposed final law shown with dashed lines. The final evaluation is shown on held-out test particles the model never sees.

## Appendix B Sample agent noise response

A sample response from claude-opus-4-7 on the Ether world at an observational noise level of $50\%$. The model clearly identifies noise as a potential issue, then determines the best experiments to run to minimize its effect on the results.

<svg id="A2.p2.pic1" height="2346.59" overflow="visible" version="1.1" viewBox="0 0 600 2346.59" width="600"><g style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,2346.59) matrix(1 0 0 -1 0 0)" fill="#000000" stroke="#000000" stroke-width="0.4pt"><g style="--ltx-fill-color:#868686;" fill="#868686" fill-opacity="1.0"><path style="stroke:none" d="M 0 0 L 0 2346.59 L 600 2346.59 L 600 0 Z"></path></g><g style="--ltx-fill-color:#ECECEC;" fill="#ECECEC" fill-opacity="1.0"><path style="stroke:none" d="M 1.38 1.38 L 1.38 2322.11 L 598.62 2322.11 L 598.62 1.38 Z"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 10.86 2330.89)"><foreignObject style="--ltx-fg-color:#FFFFFF;--ltx-fo-width:41.79em;--ltx-fo-height:0.75em;--ltx-fo-depth:0.25em;" width="578.29" height="13.84" transform="matrix(1 0 0 -1 0 10.38)" overflow="visible" color="#FFFFFF"><span id="A2.p2.pic1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1" style="width:36.34em;"><span id="A2.p2.pic1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1"><span id="A2.p2.pic1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1.1">claude-opus-4-7 | noise level <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="50\%"><semantics><mrow><mn>50</mn> <mo>%</mo></mrow> <annotation encoding="application/x-tex">50\%</annotation></semantics></math> | Round 2 response</span></span> </span></foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 10.86 2302.26)"><foreignObject style="--ltx-fg-color:#000000;--ltx-fo-width:41.79em;--ltx-fo-height:0.75em;--ltx-fo-depth:165.6em;" width="578.29" height="2301.78" transform="matrix(1 0 0 -1 0 10.38)" overflow="visible" color="#000000"><span id="A2.p2.pic1.53.53.53.53.53.53.53.53.53.53.53.52.52.52.52.52.52" style="width:41.79em;"><span id="A2.p2.pic1.53.53.53.53.53.53.53.53.53.53.53.52.52.52.52.52.52.52"><span id="A2.p2.pic1.8.8.8.8.8.8.8.8.8.8.8.7.7.7.7.7.7.7.7"><span id="A2.p2.pic1.8.8.8.8.8.8.8.8.8.8.8.7.7.7.7.7.7.7.7.7">“Looking at the data more carefully now. The probe at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(20,0)"><semantics><mrow><mo stretchy="false">(</mo><mn>20</mn><mo>,</mo><mn>0</mn><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(20,0)</annotation></semantics></math> with mass <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="1"><semantics><mn>1</mn> <annotation encoding="application/x-tex">1</annotation></semantics></math> — at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="t=0.5"><semantics><mrow><mi>t</mi> <mo>=</mo> <mn>0.5</mn></mrow> <annotation encoding="application/x-tex">t=0.5</annotation></semantics></math> it’s near <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(24.5,-4.1)"><semantics><mrow><mo stretchy="false">(</mo><mn>24.5</mn><mo>,</mo><mrow><mo>−</mo> <mn>4.1</mn></mrow><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(24.5,-4.1)</annotation></semantics></math> and <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(20.7,0.8)"><semantics><mrow><mo stretchy="false">(</mo><mn>20.7</mn><mo>,</mo><mn>0.8</mn><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(20.7,0.8)</annotation></semantics></math> in two trials. So at short times, there’s significant noise (<math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sigma"><semantics><mi>σ</mi> <annotation encoding="application/x-tex">\sigma</annotation></semantics></math> maybe <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sim 3\text{--}4"><semantics><mrow><mo>∼</mo> <mrow><mn>3</mn> <mo lspace="0em" rspace="0em"></mo><mtext>–</mtext> <mo lspace="0em" rspace="0em"></mo><mn>4</mn></mrow></mrow> <annotation encoding="application/x-tex">\sim 3\text{--}4</annotation></semantics></math>?). At later times the positions vary wildly between identical experiments — large noise.</span></span> <span id="A2.p2.pic1.9.9.9.9.9.9.9.9.9.9.9.8.8.8.8.8.8.8.8"><span id="A2.p2.pic1.9.9.9.9.9.9.9.9.9.9.9.8.8.8.8.8.8.8.8.1">Wait — looking again. Two identical experiments give very different positions at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="t=20"><semantics><mrow><mi>t</mi> <mo>=</mo> <mn>20</mn></mrow> <annotation encoding="application/x-tex">t=20</annotation></semantics></math>. That suggests either:</span></span> <span id="A2.I1"><span id="A2.I1.i1" style="list-style-type:none;">1. <span id="A2.I1.i1.p1"><span id="A2.I1.i1.p1.1"><span id="A2.I1.i1.p1.1.1">Huge observation noise (</span><math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sigma\sim"><semantics><mrow><mi>σ</mi> <mo>∼</mo></mrow> <annotation encoding="application/x-tex">\sigma\sim</annotation></semantics></math> <span id="A2.I1.i1.p1.1.2">several units)</span></span> </span></span><span id="A2.I1.i2" style="list-style-type:none;">2. <span id="A2.I1.i2.p1"><span id="A2.I1.i2.p1.1"><span id="A2.I1.i2.p1.1.1">The system itself is chaotic</span></span> </span></span></span><span id="A2.p2.pic1.14.14.14.14.14.14.14.14.14.14.14.13.13.13.13.13.13.13.13"><span id="A2.p2.pic1.14.14.14.14.14.14.14.14.14.14.14.13.13.13.13.13.13.13.13.5">Let me focus on velocities, which may be less noisy. Actually for the probe at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(20,0)"><semantics><mrow><mo stretchy="false">(</mo><mn>20</mn><mo>,</mo><mn>0</mn><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(20,0)</annotation></semantics></math>, <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="v=0"><semantics><mrow><mi>v</mi> <mo>=</mo> <mn>0</mn></mrow> <annotation encoding="application/x-tex">v=0</annotation></semantics></math>, at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="t=0.5"><semantics><mrow><mi>t</mi> <mo>=</mo> <mn>0.5</mn></mrow> <annotation encoding="application/x-tex">t=0.5</annotation></semantics></math> in run 1: <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="v\approx(-0.06,-1.16)"><semantics><mrow><mi>v</mi> <mo>≈</mo> <mrow><mo stretchy="false">(</mo><mrow><mo>−</mo> <mn>0.06</mn></mrow><mo>,</mo><mrow><mo>−</mo> <mn>1.16</mn></mrow><mo stretchy="false">)</mo></mrow></mrow> <annotation encoding="application/x-tex">v\approx(-0.06,-1.16)</annotation></semantics></math>. Run 2: <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="v\approx(1.20,0.01)"><semantics><mrow><mi>v</mi> <mo>≈</mo> <mrow><mo stretchy="false">(</mo><mn>1.20</mn><mo>,</mo><mn>0.01</mn><mo stretchy="false">)</mo></mrow></mrow> <annotation encoding="application/x-tex">v\approx(1.20,0.01)</annotation></semantics></math>. Different!</span></span> <span id="A2.p2.pic1.17.17.17.17.17.17.17.17.17.17.17.16.16.16.16.16.16.16.16"><span id="A2.p2.pic1.17.17.17.17.17.17.17.17.17.17.17.16.16.16.16.16.16.16.16.3">That’s chaotic behavior, but with <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="v=0"><semantics><mrow><mi>v</mi> <mo>=</mo> <mn>0</mn></mrow> <annotation encoding="application/x-tex">v=0</annotation></semantics></math> initial and almost no time, the velocity shouldn’t change much from systematic forces in <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="0.5"><semantics><mn>0.5</mn> <annotation encoding="application/x-tex">0.5</annotation></semantics></math> s unless force is large. Means the noise must be on positions: <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sigma\approx 2\text{--}3"><semantics><mrow><mi>σ</mi> <mo>≈</mo> <mrow><mn>2</mn> <mo lspace="0em" rspace="0em"></mo><mtext>–</mtext> <mo lspace="0em" rspace="0em"></mo><mn>3</mn></mrow></mrow> <annotation encoding="application/x-tex">\sigma\approx 2\text{--}3</annotation></semantics></math> units? And velocity reports are also noisy.</span></span> <span id="A2.p2.pic1.26.26.26.26.26.26.26.26.26.26.26.25.25.25.25.25.25.25.25"><span id="A2.p2.pic1.26.26.26.26.26.26.26.26.26.26.26.25.25.25.25.25.25.25.25.9">Let me check the ring orbiters. Particle 1 starts at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(5,0)"><semantics><mrow><mo stretchy="false">(</mo><mn>5</mn><mo>,</mo><mn>0</mn><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(5,0)</annotation></semantics></math> with <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="v=(0,2.8209)"><semantics><mrow><mi>v</mi> <mo>=</mo> <mrow><mo stretchy="false">(</mo><mn>0</mn><mo>,</mo><mn>2.8209</mn><mo stretchy="false">)</mo></mrow></mrow> <annotation encoding="application/x-tex">v=(0,2.8209)</annotation></semantics></math>. For Keplerian orbit <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="GM=v^{2}r=39.77"><semantics><mrow><mrow><mi>G</mi> <mo lspace="0em" rspace="0em"></mo><mi>M</mi></mrow> <mo>=</mo> <mrow><msup><mi>v</mi> <mn>2</mn></msup> <mo lspace="0em" rspace="0em"></mo><mi>r</mi></mrow> <mo>=</mo> <mn>39.77</mn></mrow> <annotation encoding="application/x-tex">GM=v^{2}r=39.77</annotation></semantics></math>. Let’s check if this ring is in circular orbit in run 1 at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="t=20"><semantics><mrow><mi>t</mi> <mo>=</mo> <mn>20</mn></mrow> <annotation encoding="application/x-tex">t=20</annotation></semantics></math>: particle 1 should be back near start (period if Kepler: <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="T=2\pi r/v=2\pi\cdot 5/2.82=11.13"><semantics><mrow><mi>T</mi> <mo>=</mo> <mrow><mrow><mn>2</mn> <mo lspace="0em" rspace="0em"></mo><mi>π</mi> <mo lspace="0em" rspace="0em"></mo><mi>r</mi></mrow> <mo>/</mo> <mi>v</mi></mrow> <mo>=</mo> <mrow><mrow><mrow><mn>2</mn> <mo lspace="0em" rspace="0em"></mo><mi>π</mi></mrow> <mo lspace="0.222em" rspace="0.222em">⋅</mo> <mn>5</mn></mrow> <mo>/</mo> <mn>2.82</mn></mrow> <mo>=</mo> <mn>11.13</mn></mrow> <annotation encoding="application/x-tex">T=2\pi r/v=2\pi\cdot 5/2.82=11.13</annotation></semantics></math> s, so at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="t=20"><semantics><mrow><mi>t</mi> <mo>=</mo> <mn>20</mn></mrow> <annotation encoding="application/x-tex">t=20</annotation></semantics></math> should be about <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="1.8"><semantics><mn>1.8</mn> <annotation encoding="application/x-tex">1.8</annotation></semantics></math> periods around, angle <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="=20\cdot v/r=11.28"><semantics><mrow><mo>=</mo> <mrow><mrow><mn>20</mn> <mo lspace="0.222em" rspace="0.222em">⋅</mo> <mi>v</mi></mrow> <mo>/</mo> <mi>r</mi></mrow> <mo>=</mo> <mn>11.28</mn></mrow> <annotation encoding="application/x-tex">=20\cdot v/r=11.28</annotation></semantics></math> rad <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="=1.8"><semantics><mrow><mo>=</mo> <mn>1.8</mn></mrow> <annotation encoding="application/x-tex">=1.8</annotation></semantics></math> rev).</span></span> <span id="A2.p2.pic1.36.36.36.36.36.36.36.36.36.36.36.35.35.35.35.35.35.35.35"><span id="A2.p2.pic1.36.36.36.36.36.36.36.36.36.36.36.35.35.35.35.35.35.35.35.10">Looking at reported position for particle 1 at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="t=20"><semantics><mrow><mi>t</mi> <mo>=</mo> <mn>20</mn></mrow> <annotation encoding="application/x-tex">t=20</annotation></semantics></math> in run 1: <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(1.28,2.43)"><semantics><mrow><mo stretchy="false">(</mo><mn>1.28</mn><mo>,</mo><mn>2.43</mn><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(1.28,2.43)</annotation></semantics></math>. Should be at angle <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="11.28"><semantics><mn>11.28</mn> <annotation encoding="application/x-tex">11.28</annotation></semantics></math> rad <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\bmod 2\pi=4.997"><semantics><mrow><mo>mod</mo> <mn>2</mn> <mi>π</mi> <mo>=</mo> <mn>4.997</mn></mrow> <annotation encoding="application/x-tex">\bmod 2\pi=4.997</annotation></semantics></math> rad <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="=286^{\circ}"><semantics><mrow><mo>=</mo> <msup><mn>286</mn> <mo>∘</mo></msup></mrow> <annotation encoding="application/x-tex">=286^{\circ}</annotation></semantics></math>, so <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\cos=0.286"><semantics><mrow><mi>cos</mi> <mo>=</mo> <mn>0.286</mn></mrow> <annotation encoding="application/x-tex">\cos=0.286</annotation></semantics></math>, <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sin=-0.958"><semantics><mrow><mi>sin</mi> <mo>=</mo> <mrow><mo>−</mo> <mn>0.958</mn></mrow></mrow> <annotation encoding="application/x-tex">\sin=-0.958</annotation></semantics></math>, expected <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(1.43,-4.79)"><semantics><mrow><mo stretchy="false">(</mo><mn>1.43</mn><mo>,</mo><mrow><mo>−</mo> <mn>4.79</mn></mrow><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(1.43,-4.79)</annotation></semantics></math>. Hmm, doesn’t match closely. But position is on a circle <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sim r=5"><semantics><mrow><mo>∼</mo> <mi>r</mi> <mo>=</mo> <mn>5</mn></mrow> <annotation encoding="application/x-tex">\sim r=5</annotation></semantics></math>? <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sqrt{1.28^{2}+2.43^{2}}=2.75"><semantics><mrow><msqrt><mrow><msup><mn>1.28</mn> <mn>2</mn></msup> <mo>+</mo> <msup><mn>2.43</mn> <mn>2</mn></msup></mrow></msqrt> <mo>=</mo> <mn>2.75</mn></mrow> <annotation encoding="application/x-tex">\sqrt{1.28^{2}+2.43^{2}}=2.75</annotation></semantics></math>. Way too small.</span></span> <span id="A2.p2.pic1.53.53.53.53.53.53.53.53.53.53.53.52.52.52.52.52.52.52.53"><span id="A2.p2.pic1.53.53.53.53.53.53.53.53.53.53.53.52.52.52.52.52.52.52.53.1">Hmm. So either the orbit isn’t circular gravity, or noise is enormous and position reports are very corrupted.</span></span> <span id="A2.p2.pic1.41.41.41.41.41.41.41.41.41.41.41.40.40.40.40.40.40.40.40"><span id="A2.p2.pic1.41.41.41.41.41.41.41.41.41.41.41.40.40.40.40.40.40.40.40.5">Let me think: the velocities for the ring orbiters are different too — <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="v=2.82"><semantics><mrow><mi>v</mi> <mo>=</mo> <mn>2.82</mn></mrow> <annotation encoding="application/x-tex">v=2.82</annotation></semantics></math> is set up but at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="t=0"><semantics><mrow><mi>t</mi> <mo>=</mo> <mn>0</mn></mrow> <annotation encoding="application/x-tex">t=0</annotation></semantics></math> isn’t given. Let me check at <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="t=0.5"><semantics><mrow><mi>t</mi> <mo>=</mo> <mn>0.5</mn></mrow> <annotation encoding="application/x-tex">t=0.5</annotation></semantics></math>: particle 1 reported velocity <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(-1.59,5.09)"><semantics><mrow><mo stretchy="false">(</mo><mrow><mo>−</mo> <mn>1.59</mn></mrow><mo>,</mo><mn>5.09</mn><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(-1.59,5.09)</annotation></semantics></math>. That’s pretty different from initial <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(0,2.82)"><semantics><mrow><mo stretchy="false">(</mo><mn>0</mn><mo>,</mo><mn>2.82</mn><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(0,2.82)</annotation></semantics></math>. So actually large changes happen.</span></span> <span id="A2.p2.pic1.53.53.53.53.53.53.53.53.53.53.53.52.52.52.52.52.52.52.54"><span id="A2.p2.pic1.53.53.53.53.53.53.53.53.53.53.53.52.52.52.52.52.52.52.54.1">This is hard to disentangle with noise. Let me try: a single very heavy probe far away (so its motion is slow) with zero velocity, look at very short times, and use repeated runs to average.</span></span> <span id="A2.p2.pic1.52.52.52.52.52.52.52.52.52.52.52.51.51.51.51.51.51.51.51"><span id="A2.p2.pic1.52.52.52.52.52.52.52.52.52.52.52.51.51.51.51.51.51.51.51.11">Actually let me think differently. The noise <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sigma"><semantics><mi>σ</mi> <annotation encoding="application/x-tex">\sigma</annotation></semantics></math> seems large — maybe <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sim 3"><semantics><mrow><mo>∼</mo> <mn>3</mn></mrow> <annotation encoding="application/x-tex">\sim 3</annotation></semantics></math> on positions? Position <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(20,0)"><semantics><mrow><mo stretchy="false">(</mo><mn>20</mn><mo>,</mo><mn>0</mn><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(20,0)</annotation></semantics></math> goes to <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="(24.57,-4.13)"><semantics><mrow><mo stretchy="false">(</mo><mn>24.57</mn><mo>,</mo><mrow><mo>−</mo> <mn>4.13</mn></mrow><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">(24.57,-4.13)</annotation></semantics></math> in <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="0.5"><semantics><mn>0.5</mn> <annotation encoding="application/x-tex">0.5</annotation></semantics></math> s. If force were <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sim 1/r^{2}"><semantics><mrow><mo>∼</mo> <mrow><mn>1</mn> <mo>/</mo> <msup><mi>r</mi> <mn>2</mn></msup></mrow></mrow> <annotation encoding="application/x-tex">\sim 1/r^{2}</annotation></semantics></math> with <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="GM=40"><semantics><mrow><mrow><mi>G</mi> <mo lspace="0em" rspace="0em"></mo><mi>M</mi></mrow> <mo>=</mo> <mn>40</mn></mrow> <annotation encoding="application/x-tex">GM=40</annotation></semantics></math>, accel <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="=40/400=0.1"><semantics><mrow><mo>=</mo> <mrow><mn>40</mn> <mo>/</mo> <mn>400</mn></mrow> <mo>=</mo> <mn>0.1</mn></mrow> <annotation encoding="application/x-tex">=40/400=0.1</annotation></semantics></math>, so displacement in <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="0.5"><semantics><mn>0.5</mn> <annotation encoding="application/x-tex">0.5</annotation></semantics></math> s <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="=0.0125"><semantics><mrow><mo>=</mo> <mn>0.0125</mn></mrow> <annotation encoding="application/x-tex">=0.0125</annotation></semantics></math> — utterly invisible against noise <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="\sigma\sim 3"><semantics><mrow><mi>σ</mi> <mo>∼</mo> <mn>3</mn></mrow> <annotation encoding="application/x-tex">\sigma\sim 3</annotation></semantics></math>.</span></span> <span id="A2.p2.pic1.53.53.53.53.53.53.53.53.53.53.53.52.52.52.52.52.52.52.52"><span id="A2.p2.pic1.53.53.53.53.53.53.53.53.53.53.53.52.52.52.52.52.52.52.52.1">The strategy must be: very long durations with large signal, and use many trials to average. Let me check if the ring (orbiters) are actually in a closed circular orbit, by averaging the radius across all <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline" data-latex="20"><semantics><mn>20</mn> <annotation encoding="application/x-tex">20</annotation></semantics></math> orbiters at various times.”</span></span></span></span></foreignObject></g></g></svg>

## Appendix C Description of physics worlds

Worlds use one of four JSON configs. All of them are sent to the N-body simulator, where they are processed in the same way. The exact JSON keys for each config are defined in a prompt and substituted into the universal template via a {world\_instructions} placeholder.

Two-particle ($p_{1},p_{2}$). Particle 1 is a fixed source at the origin; particle 2 is a mobile probe. The agent sets $p_{1}$ (which the simulator maps to the source coupling of particle 1), $p_{2}$ (the inertia of particle 2), $\mathbf{r}_{2}(0)$, $\mathbf{v}_{2}(0)$, and the measurement times. gravity, yukawa, fractional, oscillator, extra\_dimensions, and coulomb\_easy all use this set-up.

Probe topologies. three\_species (30 fixed background particles in three hidden species + 5 neutral probes; 35 total) and dark\_matter (20 visible + 10 hidden dark sources + 5 probes; 25 exposed to the agent, 35 total) accept the agent’s 5 probe positions and velocities only. The probes have $s_{i}=0$ (do not source the gravitational field) but $c_{i}\neq 0$ (do feel it).

Anchor + ring + probes (with masses). ether and hubble use 26 particles: a heavy central anchor (the only field source, $Q=50$), 20 ring orbiters with masses cycling through species $\{1,2,4\}$, and 5 probes whose positions, velocities, and masses (charges) the agent sets. These worlds are designed to test whether agents can disentangle a central field-mediated force from a body force supplied by space itself.

Symmetric multi-body. species (6 particles, two hidden source species), circle (1 center + 10 ring particles, all sourcing the field) place the agent in control of every particle’s initial state (just radius and tangential velocity for the constrained circle world) with no fixed background.

### C.1 Simple gravity: gravity

$1/r$ (logarithmic) attractive force in a 2D world.

### C.2 Screened potential: yukawa

Yukawa potential: Short-range exponentially suppressed force, screening length $\lambda$.

### C.3 Fractional gravitation with 2 particles: fractional

Fractional Laplacian $-(-\nabla^{2})^{\alpha}$.

### C.4 Fractional gravitation with ring of particles: circle

Fractional Laplacian $-(-\nabla^{2})^{\alpha}$.

### C.5 Multi-species simulation: three\_species

3 hidden classes (one repulsive) + 5 neutral probes.

### C.6 Dark matter (invisible mass): dark\_matter

Hidden (from agent) dark halo.

### C.7 Luminiferous ether: ether

Central law + a global preferred-direction drift.

### C.8 Hubble flow: hubble

Central law + radial Hubble flow $H(\mathbf{r})$ (position-dependent).

### C.9 Oscillator: oscillator

Sinusoidally varying coupling ($G(t)=G_{0}\cos(\omega t+\varphi)$) that periodically reverses sign.

### C.10 Extra dimensions: extra\_dimensions

$1/r$ to $1/r^{2}$ transition (2d+1 dimensions, reuses gravity world setup) with Kaluza-Klein image sums.

### C.11 Coulomb force: coulomb\_easy

$1/r^{2}$ attractive force.

## Appendix D Example rubric

We provide an example of our scoring rubric for the worlds. This rubric is for the yukawa world. Scores range from 0 to 10, with 0 being the worst possible score and 10 the best.

| Score | Criterion |
| --- | --- |
| 10 | Identifies a static screened / Helmholtz operator, $\nabla^{2}\phi-\frac{\phi}{\lambda^{2}}=\text{source}$, or an equivalent Yukawa form; names or describes exponential suppression at long range with screening length within roughly $1$ – $4$ (ground truth $\lambda=2$); correctly identifies $p_{1}$ as source coupling and $p_{2}$ as inertia. |
| 7–9 | Identifies screened / Yukawa-like behaviour qualitatively, such as short-range attraction and long-range suppression, but misses a quantitative detail, for example $\lambda$ is badly off or not estimated, or muddles the roles of $p_{1}$ and $p_{2}$. |
| 4–6 | Recognises a static attractive field but treats it as a plain Laplacian or generic power law and misses the screening structure, or asserts screening without any characterisation of its scale. |
| 1–3 | Uses the wrong operator family, such as a time-evolving or wave operator, or gives no distance-dependent suppression despite the clear experimental signature. |
| 0 | Empty or irrelevant. |

## Appendix E Example outputs

We present a range of example outputs that are gathered after a round is finished, with different models and scores shown. The output consists of a discovered law in Python, an evaluation conducted by fitting the Python code with SciPy, a text explanation of the discovered physical law, and the LLM-judge’s evaluation of the agent’s text explanation (both in words and a numerical score out of 10). We show one example for each of the 11 public worlds.

### E.1 gravity

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZGVmIGRpc2NvdmVyZWRfbGF3KHBvczEsIHBvczIsIHAxLCBwMiwgdmVsb2NpdHkyLCBkdXJhdGlvbiwgKipwYXJhbXMpOgogICAgIiIiCiAgICAyRCBsb2dhcml0aG1pYy1wb3RlbnRpYWwgaW50ZXJhY3Rpb246IHBhcnRpY2xlIDEgKGhlbGQgZml4ZWQgYXQgb3JpZ2luKSBzb3VyY2VzCiAgICBhIDJEIENvdWxvbWIvZ3Jhdml0YXRpb25hbCBmaWVsZCB3aG9zZSBwb3RlbnRpYWwgZ3Jvd3MgYXMgbG4ociksIHRoZSBuYXR1cmFsCiAgICBHcmVlbidzIGZ1bmN0aW9uIG9mIHRoZSAyRCBMYXBsYWNpYW4uIFRoZSBwcm9iZSBwYXJ0aWNsZSAyIGZlZWxzIGFuIGF0dHJhY3RpdmUKICAgIHJhZGlhbCBhY2NlbGVyYXRpb24gYSA9IC0ocDEvKDIgcGkgcDIpKSAqIHJfdmVjIC8gcl4yLgogICAgIiIiCiAgICBpbXBvcnQgbnVtcHkgYXMgbnAKICAgIGZyb20gc2NpcHkuaW50ZWdyYXRlIGltcG9ydCBzb2x2ZV9pdnAKICAgIGsgPSBwYXJhbXMuZ2V0KCJrIiwgMS4wLygyLjAqbnAucGkpKQogICAgeDAsIHkwID0gcG9zMlswXSwgcG9zMlsxXQogICAgdngwLCB2eTAgPSB2ZWxvY2l0eTJbMF0sIHZlbG9jaXR5MlsxXQogICAgZGVmIHJocyh0LCB5KToKICAgICAgICB4LCB5eSwgdngsIHZ5ID0geQogICAgICAgIHIyID0geCp4ICsgeXkqeXkKICAgICAgICBpZiByMiA8IDFlLTEyOgogICAgICAgICAgICByZXR1cm4gW3Z4LCB2eSwgMC4wLCAwLjBdCiAgICAgICAgY29lZiA9IC1rICogcDEgLyBwMiAvIHIyCiAgICAgICAgcmV0dXJuIFt2eCwgdnksIGNvZWYqeCwgY29lZip5eV0KICAgIHNvbCA9IHNvbHZlX2l2cChyaHMsICgwLjAsIGR1cmF0aW9uKSwgW3gwLCB5MCwgdngwLCB2eTBdLAogICAgICAgICAgICAgICAgICAgIG1ldGhvZD0iUks0NSIsIHJ0b2w9MWUtNywgYXRvbD0xZS05KQogICAgcmV0dXJuIFtzb2wueVswLC0xXSwgc29sLnlbMSwtMV1dLCBbc29sLnlbMiwtMV0sIHNvbC55WzMsLTFdXQoKZGVmIGZpdF9wYXJhbWV0ZXJzKCk6CiAgICByZXR1cm4geyJrIjogeyJpbml0IjogMC4xNTkxNTQ5NDMwOSwgImJvdW5kcyI6IFswLjA1LCAwLjVdfX0KCj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpFdmFsdWF0aW9uOgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KICBbZml0IHVzaW5nIDQvMTQgdHJhaW5pbmcgdHJhamVjdG9yaWVzIChjYXA6IDQgdHJhaiDDlyA1IHRpbWVzKV0KICBGaXR0ZWQgcGFyYW1ldGVyczogaz0wLjE5MDcKICBUcmFpbmluZy1zZXQgbG9zczogMC4wMDc0NDQg4oaSIDAuMDA2ODk5CiAgQ2FzZSAxOiBtZWFuX3BhcnRpY2xlX21zZSA9IDAuMDY3MQogIENhc2UgMjogbWVhbl9wYXJ0aWNsZV9tc2UgPSAwLjIwMzIKICBDYXNlIDM6IG1lYW5fcGFydGljbGVfbXNlID0gMC4wMTkyCgogIE1lYW4gcGFydGljbGUgTVNFOiAwLjA5NjUKICBNYXggIHBhcnRpY2xlIE1TRTogMS4wNzUzCiAgUmVzdWx0OiBQQVNTCgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KRXhwbGFuYXRpb24gZXZhbHVhdGlvbjoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkFnZW50IGV4cGxhbmF0aW9uOiBQYXJ0aWNsZSAxIHNpdHMgYXQgdGhlIG9yaWdpbiBhbmQgc291cmNlcyBhIDJEIGxvZ2FyaXRobWljIHBvdGVudGlhbCDPhihyKSA9IChwMS8oMs+AKSkgbG4ocikg4oCUIHRoZSBHcmVlbidzIGZ1bmN0aW9uIG9mIHRoZSBMYXBsYWNpYW4gaW4gdHdvIGRpbWVuc2lvbnMuIFBhcnRpY2xlIDIgY291cGxlcyB0byB0aGlzIGZpZWxkIHRocm91Z2ggaXRzIGluZXJ0aWEgcDIsIGV4cGVyaWVuY2luZyBhbiBhdHRyYWN0aXZlIHJhZGlhbCBmb3JjZSBGID0gLXAxLygyz4AgcikgKCpAJFxoYXR7cn0kQCopIGFuZCBoZW5jZSBhY2NlbGVyYXRpb24gYSA9IC1wMS8oMs+AIHAyKSDCtyAoKkAkXGhhdHtyfSRAKikvci4gVGhpcyBpcyB0aGUgZ2VudWluZSAyRCBhbmFsb2d1ZSBvZiBOZXd0b25pYW4gZ3Jhdml0eSAoZm9yY2Ug4oidIDEvciByYXRoZXIgdGhhbiAxL3LCsiksIGFuZCB0aGUgcHJlZmFjdG9yIDEvKDLPgCkgaXMgdGhlIHVuaXZlcnNhbCAyRCBHcmVlbidzLWZ1bmN0aW9uIGNvbnN0YW50LgoKICBFeHBsYW5hdGlvbiBzY29yZTogMS4wMCAgKHJhdyAxMC4wLzEwKQogIEp1ZGdlIHJlYXNvbmluZzogVGhlIHN0dWRlbnQncyBkZXNjcmlwdGlvbiBpcyBlc3NlbnRpYWxseSBwZXJmZWN0LiBUaGV5IGNvcnJlY3RseSBpZGVudGlmeSB0aGUgc3RhdGljIDJEIFBvaXNzb24vTGFwbGFjaWFuIGVxdWF0aW9uLCB0aGUgbG9nYXJpdGhtaWMgR3JlZW4ncyBmdW5jdGlvbiwgdGhlIDEvciBmb3JjZSBsYXcgKG5vdCAxL3LCsiksIHAxIGFzIHRoZSBzb3VyY2UgY291cGxpbmcgc3RyZW5ndGgsIGFuZCBwMiBhcyB0aGUgaW5lcnRpYSBvZiBwYXJ0aWNsZSAyLiBUaGUgaW5jbHVzaW9uIG9mIHRoZSAxLygyz4ApIHByZWZhY3RvciBpcyBhbiBhY2N1cmF0ZSBhZGRpdGlvbmFsIGRldGFpbC4gQWxsIHBoeXNpY2FsIGNvbnRlbnQgbWF0Y2hlcyB0aGUgZ3JvdW5kIHRydXRoIHByZWNpc2VseS4KCjxzY29yZT4xMDwvc2NvcmU+)

\============================================================

Discovered law:

\============================================================

def discovered\_law(pos1, pos2, p1, p2, velocity2, duration, \*\*params):

"""

2D logarithmic-potential interaction: particle 1 (held fixed at origin) sources

a 2D Coulomb/gravitational field whose potential grows as ln(r), the natural

Green’s function of the 2D Laplacian. The probe particle 2 feels an attractive

radial acceleration a = -(p1/(2 pi p2)) \* r\_vec / r^2.

"""

import numpy as np

from scipy.integrate import solve\_ivp

k = params.get("k", 1.0/(2.0\*np.pi))

x0, y0 = pos2\[0\], pos2\[1\]

vx0, vy0 = velocity2\[0\], velocity2\[1\]

def rhs(t, y):

x, yy, vx, vy = y

r2 = x\*x + yy\*yy

if r2 < 1e-12:

return \[vx, vy, 0.0, 0.0\]

coef = -k \* p1 / p2 / r2

return \[vx, vy, coef\*x, coef\*yy\]

sol = solve\_ivp(rhs, (0.0, duration), \[x0, y0, vx0, vy0\],

method="RK45", rtol=1e-7, atol=1e-9)

return \[sol.y\[0,-1\], sol.y\[1,-1\]\], \[sol.y\[2,-1\], sol.y\[3,-1\]\]

def fit\_parameters():

return {"k": {"init": 0.15915494309, "bounds": \[0.05, 0.5\]}}

\============================================================

Evaluation:

\============================================================

\[fit using 4/14 training trajectories (cap: 4 traj $\times$ 5 times)\]

Fitted parameters: k=0.1907

Training-set loss: 0.007444 $\to$ 0.006899

Case 1: mean\_particle\_mse = 0.0671

Case 2: mean\_particle\_mse = 0.2032

Case 3: mean\_particle\_mse = 0.0192

Mean particle MSE: 0.0965

Max particle MSE: 1.0753

Result: PASS

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: Particle 1 sits at the origin and sources a 2D logarithmic potential $\varphi$ (r) = (p1/(2 $\pi$)) ln(r) --- the Green’s function of the Laplacian in two dimensions. Particle 2 couples to this field through its inertia p2, experiencing an attractive radial force F = -p1/(2 $\pi$ r) $\hat{r}$ and hence acceleration a = -p1/(2 $\pi$ p2) $\cdot$ $\hat{r}$ /r. This is the genuine 2D analogue of Newtonian gravity (force $\propto$ 1/r rather than 1/r²), and the prefactor 1/(2 $\pi$) is the universal 2D Green’s-function constant.

Explanation score: 1.00 (raw 10.0/10)

Judge reasoning: The student’s description is essentially perfect. They correctly identify the static 2D Poisson/Laplacian equation, the logarithmic Green’s function, the 1/r force law (not 1/r²), p1 as the source coupling strength, and p2 as the inertia of particle 2. The inclusion of the 1/(2 $\pi$) prefactor is an accurate additional detail. All physical content matches the ground truth precisely.

<score>10</score>

Listing 1: Final output after giving the science agent a budget of 16 rounds of experiments for a gravity world with Claude Opus 4.7 as the science agent.

### E.2 yukawa

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZnJvbSBzY2lweS5pbnRlZ3JhdGUgaW1wb3J0IHNvbHZlX2l2cAppbXBvcnQgbnVtcHkgYXMgbnAKCmRlZiBkaXNjb3ZlcmVkX2xhdyhwb3MxLCBwb3MyLCBwMSwgcDIsIHZlbG9jaXR5MiwgZHVyYXRpb24sICoqcGFyYW1zKToKICAgICIiIgogICAgUGFydGljbGUgMiBleHBlcmllbmNlcyBhIGNlbnRyYWwgZm9yY2UgZnJvbSBwYXJ0aWNsZSAxLgogICAgVGhlIGZvcmNlIG1hZ25pdHVkZSBzY2FsZXMgd2l0aCB0aGUgcmF0aW8gcDEvcDIgYW5kCiAgICBkZWNheXMgYXMgdGhlIGludmVyc2UgMS41IHBvd2VyIG9mIGRpc3RhbmNlLgogICAgVGhpcyBjcmVhdGVzIHN0YWJsZSBvcmJpdHMgd2l0aCBwZXJpb2QtZGlzdGFuY2UKICAgIHJlbGF0aW9uc2hpcHMgZGlzdGluY3QgZnJvbSBpbnZlcnNlLXNxdWFyZSBncmF2aXR5LgogICAgIiIiCiAgICBrID0gcGFyYW1zLmdldCgiayIsIDAuMTE4KQoKICAgIGRlZiByaHModCwgc3RhdGUpOgogICAgICAgIHgsIHksIHZ4LCB2eSA9IHN0YXRlCiAgICAgICAgciA9IG5wLnNxcnQoeCoqMiArIHkqKjIpCiAgICAgICAgcl92ZWMgPSBucC5hcnJheShbeCwgeV0pCiAgICAgICAgZm9yY2VfbWFnID0gayAqIChwMSAvIHAyKSAvIChyKioxLjUpCiAgICAgICAgYXggPSAtZm9yY2VfbWFnICogeCAvIHIKICAgICAgICBheSA9IC1mb3JjZV9tYWcgKiB5IC8gcgogICAgICAgIHJldHVybiBbdngsIHZ5LCBheCwgYXldCgogICAgeTAgPSBbcG9zMlswXSwgcG9zMlsxXSwgdmVsb2NpdHkyWzBdLCB2ZWxvY2l0eTJbMV1dCiAgICBzb2wgPSBzb2x2ZV9pdnAocmhzLCAoMCwgZHVyYXRpb24pLCB5MCwgbWV0aG9kPSdSSzQ1JywgcnRvbD0xZS00LCBhdG9sPTFlLTQpCgogICAgZmluYWxfcG9zID0gW3NvbC55WzBdWy0xXSwgc29sLnlbMV1bLTFdXQogICAgZmluYWxfdmVsID0gW3NvbC55WzJdWy0xXSwgc29sLnlbM11bLTFdXQogICAgcmV0dXJuIGZpbmFsX3BvcywgZmluYWxfdmVsCgpkZWYgZml0X3BhcmFtZXRlcnMoKToKICAgIHJldHVybiB7CiAgICAgICAgImsiOiB7ImluaXQiOiAwLjExOCwgImJvdW5kcyI6IFswLjAxLCAxLjBdfQogICAgfQoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkV2YWx1YXRpb246Cj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQogIFtmaXQgdXNpbmcgNC8yNSB0cmFpbmluZyB0cmFqZWN0b3JpZXMgKGNhcDogNCB0cmFqIMOXIDUgdGltZXMpXQogIEZpdHRlZCBwYXJhbWV0ZXJzOiBrPTAuMTU2MQogIFRyYWluaW5nLXNldCBsb3NzOiAwLjA5Nzk5IOKGkiAwLjA5NzI1CiAgQ2FzZSAxOiBtZWFuX3BhcnRpY2xlX21zZSA9IDAuMDQyNwogIENhc2UgMjogbWVhbl9wYXJ0aWNsZV9tc2UgPSAwLjE5MDcKICBDYXNlIDM6IG1lYW5fcGFydGljbGVfbXNlID0gMC4wMTIxCgogIE1lYW4gcGFydGljbGUgTVNFOiAwLjA4MTgKICBNYXggIHBhcnRpY2xlIE1TRTogMC43ODY5CiAgUmVzdWx0OiBGQUlMCgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KRXhwbGFuYXRpb24gZXZhbHVhdGlvbjoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkFnZW50IGV4cGxhbmF0aW9uOiBUaGUgc3lzdGVtIGZlYXR1cmVzIGEgY2VudHJhbCBmb3JjZSBsYXcgd2hlcmUgcGFydGljbGUgMiBleHBlcmllbmNlcyBhY2NlbGVyYXRpb24gcHJvcG9ydGlvbmFsIHRvIHAxL3AyIGFuZCBpbnZlcnNlbHkgcHJvcG9ydGlvbmFsIHRvIGRpc3RhbmNlIHJhaXNlZCB0byAxLjUuIFRoZSBmb3JjZSBpcyBhbHdheXMgYXR0cmFjdGl2ZSBhbmQgcmFkaWFsIGZyb20gcGFydGljbGUgMSdzIGZpeGVkIHBvc2l0aW9uLiBUaGlzIGZyYWN0aW9uYWwgZXhwb25lbnQgY3JlYXRlcyBvcmJpdGFsIGR5bmFtaWNzIHdpdGggcGVyaW9kIHNjYWxpbmcgYXMgVCDiiJ0gKHJeezEuMjV9KSAvICgqQCRcc3FydHtwXzEvcF8yfSRAKiksIGRpc3RpbmN0IGZyb20gaW52ZXJzZS1zcXVhcmUgZ3Jhdml0eS4KCiAgRXhwbGFuYXRpb24gc2NvcmU6IDAuMzAgIChyYXcgMy4wLzEwKQogIEp1ZGdlIHJlYXNvbmluZzogVGhlIHN0dWRlbnQgZGVzY3JpYmVzIGEgcG93ZXItbGF3IGZvcmNlICgxL3JeMS41KSB0aGF0IGlzICJhbHdheXMgYXR0cmFjdGl2ZSIgd2l0aCBubyBtZW50aW9uIG9mIGV4cG9uZW50aWFsIHN1cHByZXNzaW9uIG9yIHNjcmVlbmluZy4gVGhleSBtaXNzIHRoZSBIZWxtaG9sdHovWXVrYXdhIGNoYXJhY3RlciBlbnRpcmVseSwgdHJlYXRpbmcgaXQgYXMgYSBnZW5lcmljIGZyYWN0aW9uYWwgcG93ZXItbGF3IHJhdGhlciB0aGFuIGEgc2NyZWVuZWQgZmllbGQuIFdoaWxlIHRoZXkgY29ycmVjdGx5IGlkZW50aWZ5IHAxIGFzIHNvdXJjZSBzdHJlbmd0aCBhbmQgcDIgYXMgaW5lcnRpYWwgY291cGxpbmcsIGFuZCByZWNvZ25pemUgdGhlIGZvcmNlIGFzIHN0YXRpYyBhbmQgYXR0cmFjdGl2ZSwgdGhlIGNvbXBsZXRlIGFic2VuY2Ugb2Ygc2NyZWVuaW5nL2V4cG9uZW50aWFsIHN1cHByZXNzaW9uIGFuZCB0aGUgd3Jvbmcgb3BlcmF0b3IgZmFtaWx5IChmcmFjdGlvbmFsIHBvd2VyLWxhdyBpbnN0ZWFkIG9mIEhlbG1ob2x0eikgcGxhY2UgdGhpcyBzb2xpZGx5IGluIHRoZSBsb3dlci1taWRkbGUgcmFuZ2UuCgo8c2NvcmU+Mzwvc2NvcmU+)

\============================================================

Discovered law:

\============================================================

from scipy.integrate import solve\_ivp

import numpy as np

def discovered\_law(pos1, pos2, p1, p2, velocity2, duration, \*\*params):

"""

Particle 2 experiences a central force from particle 1.

The force magnitude scales with the ratio p1/p2 and

decays as the inverse 1.5 power of distance.

This creates stable orbits with period-distance

relationships distinct from inverse-square gravity.

"""

k = params.get("k", 0.118)

def rhs(t, state):

x, y, vx, vy = state

r = np.sqrt(x\*\*2 + y\*\*2)

r\_vec = np.array(\[x, y\])

force\_mag = k \* (p1 / p2) / (r\*\*1.5)

ax = -force\_mag \* x / r

ay = -force\_mag \* y / r

return \[vx, vy, ax, ay\]

y0 = \[pos2\[0\], pos2\[1\], velocity2\[0\], velocity2\[1\]\]

sol = solve\_ivp(rhs, (0, duration), y0, method=’RK45’, rtol=1e-4, atol=1e-4)

final\_pos = \[sol.y\[0\]\[-1\], sol.y\[1\]\[-1\]\]

final\_vel = \[sol.y\[2\]\[-1\], sol.y\[3\]\[-1\]\]

return final\_pos, final\_vel

def fit\_parameters():

return {

"k": {"init": 0.118, "bounds": \[0.01, 1.0\]}

}

\============================================================

Evaluation:

\============================================================

\[fit using 4/25 training trajectories (cap: 4 traj $\times$ 5 times)\]

Fitted parameters: k=0.1561

Training-set loss: 0.09799 $\to$ 0.09725

Case 1: mean\_particle\_mse = 0.0427

Case 2: mean\_particle\_mse = 0.1907

Case 3: mean\_particle\_mse = 0.0121

Mean particle MSE: 0.0818

Max particle MSE: 0.7869

Result: FAIL

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: The system features a central force law where particle 2 experiences acceleration proportional to p1/p2 and inversely proportional to distance raised to 1.5. The force is always attractive and radial from particle 1’s fixed position. This fractional exponent creates orbital dynamics with period scaling as T $\propto$ (r^{1.25}) / $\sqrt{p_{1}/p_{2}}$, distinct from inverse-square gravity.

Explanation score: 0.30 (raw 3.0/10)

Judge reasoning: The student describes a power-law force (1/r^1.5) that is "always attractive" with no mention of exponential suppression or screening. They miss the Helmholtz/Yukawa character entirely, treating it as a generic fractional power-law rather than a screened field. While they correctly identify p1 as source strength and p2 as inertial coupling, and recognize the force as static and attractive, the complete absence of screening/exponential suppression and the wrong operator family (fractional power-law instead of Helmholtz) place this solidly in the lower-middle range.

<score>3</score>

Listing 2: Final output after giving the science agent a budget of 16 rounds of experiments for a yukawa world with Deepseek-R1-0528 as the science agent.

### E.3 fractional

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZGVmIGRpc2NvdmVyZWRfbGF3KHBvczEsIHBvczIsIHAxLCBwMiwgdmVsb2NpdHkyLCBkdXJhdGlvbiwgKipwYXJhbXMpOgogICAgIiIiUGFydGljbGUgMiBleHBlcmllbmNlcyBhbiBhdHRyYWN0aXZlIGNlbnRyYWwgZm9yY2UgdG93YXJkIHBhcnRpY2xlIDEgKGF0IG9yaWdpbikuCiAgICBUaGUgYWNjZWxlcmF0aW9uIG1hZ25pdHVkZSBzY2FsZXMgYXMgRyAqIHAxIC8gcDIgLyByXjIsIGRpcmVjdGVkIHJhZGlhbGx5IGlud2FyZC4KICAgIFRoaXMgaXMgYW4gaW52ZXJzZS1zcXVhcmUgbGF3IHdpdGggY291cGxpbmcgcHJvcG9ydGlvbmFsIHRvIHAxL3AyIHJhdGlvLgogICAgIiIiCiAgICBpbXBvcnQgbnVtcHkgYXMgbnAKICAgIGZyb20gc2NpcHkuaW50ZWdyYXRlIGltcG9ydCBzb2x2ZV9pdnAKCiAgICBHID0gcGFyYW1zLmdldCgiRyIsIDAuMTYpCgogICAgZGVmIHJocyh0LCB5KToKICAgICAgICB4LCB5X3BvcywgdngsIHZ5ID0geQogICAgICAgIHIgPSBucC5zcXJ0KHgqKjIgKyB5X3BvcyoqMikKICAgICAgICBpZiByIDwgMWUtMTA6CiAgICAgICAgICAgIHIgPSAxZS0xMAogICAgICAgIGZhY3RvciA9IEcgKiAocDEgLyBwMikgLyAocioqMikKICAgICAgICBheCA9IC1mYWN0b3IgKiB4IC8gcgogICAgICAgIGF5ID0gLWZhY3RvciAqIHlfcG9zIC8gcgogICAgICAgIHJldHVybiBbdngsIHZ5LCBheCwgYXldCgogICAgeTAgPSBbcG9zMlswXSwgcG9zMlsxXSwgdmVsb2NpdHkyWzBdLCB2ZWxvY2l0eTJbMV1dCiAgICBzb2wgPSBzb2x2ZV9pdnAocmhzLCAoMC4wLCBkdXJhdGlvbiksIHkwLCBtZXRob2Q9IlJLNDUiLCBydG9sPTFlLTYsIGF0b2w9MWUtNikKICAgIGZpbmFsX3N0YXRlID0gc29sLnlbOiwgLTFdCiAgICBmaW5hbF9wb3MyID0gW2ZpbmFsX3N0YXRlWzBdLCBmaW5hbF9zdGF0ZVsxXV0KICAgIGZpbmFsX3ZlbDIgPSBbZmluYWxfc3RhdGVbMl0sIGZpbmFsX3N0YXRlWzNdXQogICAgcmV0dXJuIGZpbmFsX3BvczIsIGZpbmFsX3ZlbDIKCmRlZiBmaXRfcGFyYW1ldGVycygpOgogICAgcmV0dXJuIHsKICAgICAgICAiRyI6IHsiaW5pdCI6IDAuMTYsICJib3VuZHMiOiBbMC4wNSwgMC41XX0sCiAgICB9Cgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KRXZhbHVhdGlvbjoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CiAgW2ZpdCB1c2luZyA0LzM4IHRyYWluaW5nIHRyYWplY3RvcmllcyAoY2FwOiA0IHRyYWogw5cgNSB0aW1lcyldCiAgRml0dGVkIHBhcmFtZXRlcnM6IEc9MC4xNjk1CiAgVHJhaW5pbmctc2V0IGxvc3M6IDAuMDA0NDExIOKGkiAwLjAwNDM4MgogIENhc2UgMTogbWVhbl9wYXJ0aWNsZV9tc2UgPSAwLjAwMDYKICBDYXNlIDI6IG1lYW5fcGFydGljbGVfbXNlID0gMC4wMDA1CiAgQ2FzZSAzOiBtZWFuX3BhcnRpY2xlX21zZSA9IDAuMDAwMwoKICBNZWFuIHBhcnRpY2xlIE1TRTogMC4wMDA1CiAgTWF4ICBwYXJ0aWNsZSBNU0U6IDAuMDAyMQogIFJlc3VsdDogUEFTUwoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkV4cGxhbmF0aW9uIGV2YWx1YXRpb246Cj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpBZ2VudCBleHBsYW5hdGlvbjogUGFydGljbGUgMiBleHBlcmllbmNlcyBhbiBhdHRyYWN0aXZlIGNlbnRyYWwgZm9yY2UgZGlyZWN0ZWQgdG93YXJkIHBhcnRpY2xlIDEgKGZpeGVkIGF0IHRoZSBvcmlnaW4pLiBUaGUgZm9yY2UgZm9sbG93cyBhbiBpbnZlcnNlLXNxdWFyZSBsYXc6IHRoZSBhY2NlbGVyYXRpb24gbWFnbml0dWRlIGlzIEcgw5cgKHAxL3AyKSAvIHLCsiwgd2hlcmUgciBpcyB0aGUgZGlzdGFuY2UgZnJvbSB0aGUgb3JpZ2luLiBUaGUgc2NhbGFyIHByb3BlcnRpZXMgcDEgYW5kIHAyIGFjdCBhcyBjb3VwbGluZyBjb25zdGFudHPigJRwMSAoc291cmNlKSBlbmhhbmNlcyB0aGUgYXR0cmFjdGlvbiB3aGlsZSBwMiAocHJvYmUpIHJlZHVjZXMgdGhlIHJlc3BvbnNlLCBhbmFsb2dvdXMgdG8gY2hhcmdlIGFuZCBtYXNzIHJlc3BlY3RpdmVseS4gVGhlIGNvbnN0YW50IEcg4omIIDAuMTYgc2V0cyB0aGUgb3ZlcmFsbCBmb3JjZSBzY2FsZSBpbiB0aGlzIHVuaXZlcnNlLgoKICBFeHBsYW5hdGlvbiBzY29yZTogMC41MCAgKHJhdyA1LjAvMTApCiAgSnVkZ2UgcmVhc29uaW5nOiBUaGUgc3R1ZGVudCBjb3JyZWN0bHkgaWRlbnRpZmllcyB0aGUgc3lzdGVtIGFzIHN0YXRpYyB3aXRoIGFuIGF0dHJhY3RpdmUgY2VudHJhbCBmb3JjZSwgYW5kIGNvcnJlY3RseSBpZGVudGlmaWVzIHAxIGFzIHRoZSBzb3VyY2UgY291cGxpbmcgYW5kIHAyIGFzIHJlZHVjaW5nIHRoZSByZXNwb25zZSAoaW5lcnRpYSkuIEhvd2V2ZXIsIHRoZSBzdHVkZW50IGRlc2NyaWJlcyB0aGUgZm9yY2UgYXMgZm9sbG93aW5nIGFuIGludmVyc2Utc3F1YXJlIGxhdyAoMS9ywrIpLCB3aGljaCBjb3JyZXNwb25kcyB0byBhIHN0YW5kYXJkIDJEIExhcGxhY2lhbiAob3IgM0QgQ291bG9tYi1saWtlKSBzY2VuYXJpbywgY29tcGxldGVseSBtaXNzaW5nIHRoZSBmcmFjdGlvbmFsIExhcGxhY2lhbiBvcGVyYXRvciBhbmQgdGhlIGVuaGFuY2VkIGxvbmctcmFuZ2UgKHNsb3dlciBkZWNheSkgY2hhcmFjdGVyIHRoYXQgaXMgdGhlIGtleSBkaXN0aW5ndWlzaGluZyBmZWF0dXJlIG9mIHRoaXMgd29ybGQuIFRoaXMgcGxhY2VzIHRoZSBhbnN3ZXIgc3F1YXJlbHkgaW4gdGhlICJyZWNvZ25pc2VzIHN0YXRpYyBhdHRyYWN0aW9uIGJ1dCBhc3N1bWVzIGEgc3RhbmRhcmQgTGFwbGFjaWFuIG9yIG9yZGluYXJ5IHBvd2VyLWxhdyIgYmFuZC4KCjxzY29yZT41PC9zY29yZT4=)

\============================================================

Discovered law:

\============================================================

def discovered\_law(pos1, pos2, p1, p2, velocity2, duration, \*\*params):

"""Particle 2 experiences an attractive central force toward particle 1 (at origin).

The acceleration magnitude scales as G \* p1 / p2 / r^2, directed radially inward.

This is an inverse-square law with coupling proportional to p1/p2 ratio.

"""

import numpy as np

from scipy.integrate import solve\_ivp

G = params.get("G", 0.16)

def rhs(t, y):

x, y\_pos, vx, vy = y

r = np.sqrt(x\*\*2 + y\_pos\*\*2)

if r < 1e-10:

r = 1e-10

factor = G \* (p1 / p2) / (r\*\*2)

ax = -factor \* x / r

ay = -factor \* y\_pos / r

return \[vx, vy, ax, ay\]

y0 = \[pos2\[0\], pos2\[1\], velocity2\[0\], velocity2\[1\]\]

sol = solve\_ivp(rhs, (0.0, duration), y0, method="RK45", rtol=1e-6, atol=1e-6)

final\_state = sol.y\[:, -1\]

final\_pos2 = \[final\_state\[0\], final\_state\[1\]\]

final\_vel2 = \[final\_state\[2\], final\_state\[3\]\]

return final\_pos2, final\_vel2

def fit\_parameters():

return {

"G": {"init": 0.16, "bounds": \[0.05, 0.5\]},

}

\============================================================

Evaluation:

\============================================================

\[fit using 4/38 training trajectories (cap: 4 traj $\times$ 5 times)\]

Fitted parameters: G=0.1695

Training-set loss: 0.004411 $\to$ 0.004382

Case 1: mean\_particle\_mse = 0.0006

Case 2: mean\_particle\_mse = 0.0005

Case 3: mean\_particle\_mse = 0.0003

Mean particle MSE: 0.0005

Max particle MSE: 0.0021

Result: PASS

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: Particle 2 experiences an attractive central force directed toward particle 1 (fixed at the origin). The force follows an inverse-square law: the acceleration magnitude is G $\times$ (p1/p2) / r², where r is the distance from the origin. The scalar properties p1 and p2 act as coupling constants---p1 (source) enhances the attraction while p2 (probe) reduces the response, analogous to charge and mass respectively. The constant G $\approx$ 0.16 sets the overall force scale in this universe.

Explanation score: 0.50 (raw 5.0/10)

Judge reasoning: The student correctly identifies the system as static with an attractive central force, and correctly identifies p1 as the source coupling and p2 as reducing the response (inertia). However, the student describes the force as following an inverse-square law (1/r²), which corresponds to a standard 2D Laplacian (or 3D Coulomb-like) scenario, completely missing the fractional Laplacian operator and the enhanced long-range (slower decay) character that is the key distinguishing feature of this world. This places the answer squarely in the "recognises static attraction but assumes a standard Laplacian or ordinary power-law" band.

<score>5</score>

Listing 3: Final output after giving the science agent a budget of 16 rounds of experiments for a fractional world with Qwen3.5-397B-A17B as the science agent.

### E.4 circle

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZGVmIGRpc2NvdmVyZWRfbGF3KHBvc2l0aW9ucywgdmVsb2NpdGllcywgZHVyYXRpb24sICoqcGFyYW1zKToKICAgICIiIgogICAgUGFydGljbGVzIGV4cGVyaWVuY2UgYSBjb25zdGFudCBvdXR3YXJkIHJhZGlhbCBhY2NlbGVyYXRpb24uCiAgICBUaGUgYWNjZWxlcmF0aW9uIG1hZ25pdHVkZSBBIGlzIGEgZnJlZSBwYXJhbWV0ZXIgKGRlZmF1bHQgMC4wNykuCiAgICBObyBvdGhlciBmb3JjZXMgYWN0OyB0aHVzIG1vdGlvbiBmb2xsb3dzIE5ld3RvbidzIHNlY29uZCBsYXcgd2l0aAogICAgYSA9IEEgKiAoKkAkXGhhdHtyfSRAKiksIHdoZXJlICgqQCRcaGF0e3J9JEAqKSBpcyB0aGUgdW5pdCB2ZWN0b3IgZnJvbSB0aGUgb3JpZ2luIHRvIHRoZSBwYXJ0aWNsZS4KICAgICIiIgogICAgaW1wb3J0IG51bXB5IGFzIG5wCiAgICBmcm9tIHNjaXB5LmludGVncmF0ZSBpbXBvcnQgc29sdmVfaXZwCgogICAgIyBudW1iZXIgb2YgcGFydGljbGVzIChzaG91bGQgYmUgMTEpCiAgICBuID0gbGVuKHBvc2l0aW9ucykKCiAgICAjIGZsYXR0ZW4gaW5pdGlhbCBzdGF0ZTogW3gxLHkxLC4uLix4bix5biwgdngxLHZ5MSwuLi4sdnhuLHZ5bl0KICAgIHkwID0gbnAuaHN0YWNrKFtucC5yYXZlbChwb3NpdGlvbnMpLCBucC5yYXZlbCh2ZWxvY2l0aWVzKV0pCgogICAgQSA9IHBhcmFtcy5nZXQoIkEiLCAwLjA3KSAgIyBvdXR3YXJkIGFjY2VsZXJhdGlvbiBtYWduaXR1ZGUKCiAgICBkZWYgcmhzKHQsIHkpOgogICAgICAgICMgdW5wYWNrCiAgICAgICAgcG9zID0geVs6IDIgKiBuXS5yZXNoYXBlKChuLCAyKSkKICAgICAgICB2ZWwgPSB5WzIgKiBuIDpdLnJlc2hhcGUoKG4sIDIpKQoKICAgICAgICAjIGNvbXB1dGUgcmFkaWFsIHVuaXQgdmVjdG9ycywgYXZvaWQgZGl2aXNpb24gYnkgemVybwogICAgICAgIHJfbm9ybSA9IG5wLmxpbmFsZy5ub3JtKHBvcywgYXhpcz0xLCBrZWVwZGltcz1UcnVlKQogICAgICAgICMgZm9yIHRoZSBjZW50cmFsIHBhcnRpY2xlIChpbmRleCAwKSBrZWVwIGFjY2VsZXJhdGlvbiB6ZXJvCiAgICAgICAgd2l0aCBucC5lcnJzdGF0ZShkaXZpZGU9Imlnbm9yZSIsIGludmFsaWQ9Imlnbm9yZSIpOgogICAgICAgICAgICB1bml0ID0gbnAud2hlcmUocl9ub3JtID4gMCwgcG9zIC8gcl9ub3JtLCAwLjApCgogICAgICAgIGFjYyA9IEEgKiB1bml0ICAjIGNvbnN0YW50IG91dHdhcmQgYWNjZWxlcmF0aW9uCiAgICAgICAgZHlkdCA9IG5wLmNvbmNhdGVuYXRlKFt2ZWwucmF2ZWwoKSwgYWNjLnJhdmVsKCldKQogICAgICAgIHJldHVybiBkeWR0CgogICAgc29sID0gc29sdmVfaXZwKHJocywgKDAuMCwgZHVyYXRpb24pLCB5MCwgbWV0aG9kPSJSSzQ1IiwgcnRvbD0xZS00LCBhdG9sPTFlLTQpCiAgICBmaW5hbF9zdGF0ZSA9IHNvbC55WzosIC0xXQogICAgZmluYWxfcG9zID0gZmluYWxfc3RhdGVbOiAyICogbl0ucmVzaGFwZSgobiwgMikpCiAgICByZXR1cm4gZmluYWxfcG9zLnRvbGlzdCgpCgpkZWYgZml0X3BhcmFtZXRlcnMoKToKICAgIHJldHVybiB7CiAgICAgICAgIkEiOiB7ImluaXQiOiAwLjA3LCAiYm91bmRzIjogWzAuMDEsIDAuMl19CiAgICB9Cgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KRXZhbHVhdGlvbjoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CiAgW2ZpdCB1c2luZyA0LzggdHJhaW5pbmcgdHJhamVjdG9yaWVzIChjYXA6IDQgdHJhaiDDlyA1IHRpbWVzKV0KICBGaXR0ZWQgcGFyYW1ldGVyczogQT0wLjAxCiAgVHJhaW5pbmctc2V0IGxvc3M6IDE0MSDihpIgNDQuMTkKICBDYXNlIDEgKHI9NS4wLCB2X3Q9MC4zKTogbWVhbl9wYXJ0aWNsZV9tc2UgPSA2LjcxNTkKCiAgTWVhbiBwYXJ0aWNsZSBNU0UgKGFsbCBwYXJ0aWNsZXMpOiA2LjcxNTkKICBNYXggIHBhcnRpY2xlIE1TRTogICAgICAgICAgICAgICAgIDI0LjkyNzMKICBSZXN1bHQ6IEZBSUwKCj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpFeHBsYW5hdGlvbiBldmFsdWF0aW9uOgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KQWdlbnQgZXhwbGFuYXRpb246IFRoZSBoaWRkZW4gZmllbGQgaXMgYSB1bmlmb3JtIHJhZGlhbCByZXB1bHNpb246IGV2ZXJ5IHBhcnRpY2xlIGZlZWxzIGEgY29uc3RhbnQgYWNjZWxlcmF0aW9uIGRpcmVjdGVkIGF3YXkgZnJvbSB0aGUgb3JpZ2luLiBUaGUgYWNjZWxlcmF0aW9uIG1hZ25pdHVkZSBBIGlzIHRoZSBzb2xlIGZyZWUgY29uc3RhbnQuIFdpdGggbm8gdG9ycXVlcywgYW5ndWxhciBtb21lbnR1bSBpcyBjb25zZXJ2ZWQsIHNvIHBhcnRpY2xlcyBmb2xsb3cgb3V0d2FyZCBzcGlyYWxzIGRldGVybWluZWQgYnkgdGhlaXIgaW5pdGlhbCByYWRpYWwgYW5kIHRhbmdlbnRpYWwgdmVsb2NpdGllcy4gVGhpcyBzaW1wbGUgY2VudHJhbC1mb3JjZSBsYXcgcmVwcm9kdWNlcyB0aGUgb2JzZXJ2ZWQgbGluZWFyIGdyb3d0aCBvZiBzcGVlZCBhbmQgcXVhZHJhdGljIGluY3JlYXNlIG9mIHJhZGl1cy4KCiAgRXhwbGFuYXRpb24gc2NvcmU6IDAuMTAgIChyYXcgMS4wLzEwKQogIEp1ZGdlIHJlYXNvbmluZzogVGhlIHN0dWRlbnQgZGVzY3JpYmVzIGEgInVuaWZvcm0gcmFkaWFsIHJlcHVsc2lvbiIgd2l0aCBjb25zdGFudCBhY2NlbGVyYXRpb24gYXdheSBmcm9tIHRoZSBvcmlnaW4sIHdoaWNoIGlzIGZ1bmRhbWVudGFsbHkgd3JvbmcgaW4gbXVsdGlwbGUgd2F5czogdGhlIGZvcmNlIGlzIHJlcHVsc2l2ZSByYXRoZXIgdGhhbiBhdHRyYWN0aXZlLCB0aGVyZSBpcyBubyBwYXJ0aWNsZS1wYXJ0aWNsZSBpbnRlcmFjdGlvbiBkZXNjcmliZWQgKG9ubHkgYSBjZW50cmFsIGZvcmNlIGZyb20gdGhlIG9yaWdpbiksIG5vIHNwYXRpYWwgb3BlcmF0b3IgKExhcGxhY2lhbiwgZnJhY3Rpb25hbCBMYXBsYWNpYW4sIG9yIG90aGVyd2lzZSkgaXMgaWRlbnRpZmllZCwgYW5kIHRoZSBzdGF0aWMgbm9uLWxvY2FsIGZyYWN0aW9uYWwgY2hhcmFjdGVyIGlzIGVudGlyZWx5IG1pc3NlZC4gVGhlIGRlc2NyaXB0aW9uIGJlYXJzIGVzc2VudGlhbGx5IG5vIHJlc2VtYmxhbmNlIHRvIHRoZSBncm91bmQgdHJ1dGguCgo8c2NvcmU+MTwvc2NvcmU+)

\============================================================

Discovered law:

\============================================================

def discovered\_law(positions, velocities, duration, \*\*params):

"""

Particles experience a constant outward radial acceleration.

The acceleration magnitude A is a free parameter (default 0.07).

No other forces act; thus motion follows Newton’s second law with

a = A \* $\hat{r}$, where $\hat{r}$ is the unit vector from the origin to the particle.

"""

import numpy as np

from scipy.integrate import solve\_ivp

\# number of particles (should be 11)

n = len(positions)

\# flatten initial state: \[x1,y1,...,xn,yn, vx1,vy1,...,vxn,vyn\]

y0 = np.hstack(\[np.ravel(positions), np.ravel(velocities)\])

A = params.get("A", 0.07) # outward acceleration magnitude

def rhs(t, y):

\# unpack

pos = y\[: 2 \* n\].reshape((n, 2))

vel = y\[2 \* n:\].reshape((n, 2))

\# compute radial unit vectors, avoid division by zero

r\_norm = np.linalg.norm(pos, axis=1, keepdims=True)

\# for the central particle (index 0) keep acceleration zero

with np.errstate(divide="ignore", invalid="ignore"):

unit = np.where(r\_norm > 0, pos / r\_norm, 0.0)

acc = A \* unit # constant outward acceleration

dydt = np.concatenate(\[vel.ravel(), acc.ravel()\])

return dydt

sol = solve\_ivp(rhs, (0.0, duration), y0, method="RK45", rtol=1e-4, atol=1e-4)

final\_state = sol.y\[:, -1\]

final\_pos = final\_state\[: 2 \* n\].reshape((n, 2))

return final\_pos.tolist()

def fit\_parameters():

return {

"A": {"init": 0.07, "bounds": \[0.01, 0.2\]}

}

\============================================================

Evaluation:

\============================================================

\[fit using 4/8 training trajectories (cap: 4 traj $\times$ 5 times)\]

Fitted parameters: A=0.01

Training-set loss: 141 $\to$ 44.19

Case 1 (r=5.0, v\_t=0.3): mean\_particle\_mse = 6.7159

Mean particle MSE (all particles): 6.7159

Max particle MSE: 24.9273

Result: FAIL

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: The hidden field is a uniform radial repulsion: every particle feels a constant acceleration directed away from the origin. The acceleration magnitude A is the sole free constant. With no torques, angular momentum is conserved, so particles outward spirals determined by their initial radial and tangential velocities. This simple central-force law reproduces the observed linear growth of speed and quadratic increase of radius.

Explanation score: 0.10 (raw 1.0/10)

Judge reasoning: The student describes a "uniform radial repulsion" with constant acceleration away from the origin, which is fundamentally wrong in multiple ways: the force is repulsive rather than attractive, there is no particle-particle interaction described (only a central force from the origin), no spatial operator (Laplacian, fractional Laplacian, or otherwise) is identified, and the static non-local fractional character is entirely missed. The description bears essentially no resemblance to the ground truth.

<score>1</score>

Listing 4: Final output after giving the science agent a budget of 16 rounds of experiments for a circle world with gpt-oss-120b as the science agent.

### E.5 three\_species

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZGVmIGRpc2NvdmVyZWRfbGF3KHBvc2l0aW9ucywgdmVsb2NpdGllcywgZHVyYXRpb24sICoqcGFyYW1zKToKICAgICIiIgogICAgVGhlIGZvcmNlIG9uIGVhY2ggcHJvYmUgaXMgcHJvcG9ydGlvbmFsIHRvIHRoZSA5MC1kZWdyZWUgY291bnRlcmNsb2Nrd2lzZQogICAgcm90YXRpb24gb2YgYm90aCBpdHMgdmVsb2NpdHkgYW5kIHBvc2l0aW9uIHZlY3RvcnMsIGluZGljYXRpbmcgYSBzeW50aGV0aWMKICAgIG1hZ25ldGljLWxpa2UgZmllbGQgY291cGxlZCB3aXRoIGEgcm90YXRpb25hbCBwb3RlbnRpYWwgZmllbGQuCgogICAgVGhpcyByZXN1bHRzIGluIHRyYWplY3RvcmllcyB0aGF0IGNvbWJpbmUgY3ljbG90cm9uIG1vdGlvbiB3aXRoCiAgICBhIGJhY2tncm91bmQgc3dpcmwsIGNvbnNpc3RlbnQgd2l0aCBhIGZpeGVkIHZvcnRpY2l0eSBmaWVsZC4KICAgICIiIgogICAgaW1wb3J0IG51bXB5IGFzIG5wCiAgICBmcm9tIHNjaXB5LmludGVncmF0ZSBpbXBvcnQgc29sdmVfaXZwCgogICAgIyBFeHRyYWN0IHRoZSA1IHByb2JlIHBhcnRpY2xlcyAobGFzdCA1KQogICAgcHJvYmVfcG9zaXRpb25zID0gbnAuYXJyYXkocG9zaXRpb25zWy01Ol0pCiAgICBwcm9iZV92ZWxvY2l0aWVzID0gbnAuYXJyYXkodmVsb2NpdGllc1stNTpdKQoKICAgICMgU2V0IHBhcmFtZXRlcnMKICAgIGFscGhhID0gcGFyYW1zLmdldCgiYWxwaGEiLCAxLjApICAjIHZlbG9jaXR5IGNvdXBsaW5nCiAgICBiZXRhID0gcGFyYW1zLmdldCgiYmV0YSIsIDEuMCkgICAgIyBwb3NpdGlvbiBjb3VwbGluZwoKICAgICMgSW5pdGlhbCBzdGF0ZTogW3gxLCB5MSwgeDIsIHkyLCAuLi4sIHZ4MSwgdnkxLCAuLi5dCiAgICB5MCA9IG5wLmNvbmNhdGVuYXRlKFtwcm9iZV9wb3NpdGlvbnMuZmxhdHRlbigpLCBwcm9iZV92ZWxvY2l0aWVzLmZsYXR0ZW4oKV0pCgogICAgZGVmIHJocyh0LCB5KToKICAgICAgICBwb3MgPSB5WzoxMF0ucmVzaGFwZSg1LCAyKQogICAgICAgIHZlbCA9IHlbMTA6XS5yZXNoYXBlKDUsIDIpCiAgICAgICAgYWNjID0gbnAuemVyb3NfbGlrZSh2ZWwpCgogICAgICAgICMgUm90YXRlIHZlbG9jaXR5IGFuZCBwb3NpdGlvbiA5MMKwIENDVzogKGEsYikgLT4gKC1iLCBhKQogICAgICAgIGFjYyArPSBhbHBoYSAqIG5wLmFycmF5KFstdmVsWzosIDFdLCB2ZWxbOiwgMF1dKS5UCiAgICAgICAgYWNjICs9IGJldGEgKiBucC5hcnJheShbLXBvc1s6LCAxXSwgcG9zWzosIDBdXSkuVAoKICAgICAgICBkeWR0ID0gbnAuY29uY2F0ZW5hdGUoW3ZlbC5mbGF0dGVuKCksIGFjYy5mbGF0dGVuKCldKQogICAgICAgIHJldHVybiBkeWR0CgogICAgIyBTb2x2ZSBPREUKICAgIHNvbCA9IHNvbHZlX2l2cChyaHMsICgwLjAsIGR1cmF0aW9uKSwgeTAsIG1ldGhvZD0iUks0NSIsIHJ0b2w9MWUtNCwgYXRvbD0xZS00LCB0X2V2YWw9W2R1cmF0aW9uXSkKICAgIGZpbmFsX3N0YXRlID0gc29sLnlbOiwgLTFdCiAgICBmaW5hbF9wb3NpdGlvbnMgPSBmaW5hbF9zdGF0ZVs6MTBdLnJlc2hhcGUoNSwgMikKICAgIHJldHVybiBmaW5hbF9wb3NpdGlvbnMudG9saXN0KCkKCmRlZiBmaXRfcGFyYW1ldGVycygpOgogICAgcmV0dXJuIHsKICAgICAgICAiYWxwaGEiOiB7ImluaXQiOiAxLjAsICJib3VuZHMiOiBbMC4xLCAxMC4wXX0sCiAgICAgICAgImJldGEiOiB7ImluaXQiOiAxLjAsICJib3VuZHMiOiBbMC4xLCAxMC4wXX0KICAgIH0KCj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpFdmFsdWF0aW9uOgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KICBDYXNlIDE6IEVSUk9SIC0tIG9wZXJhbmRzIGNvdWxkIG5vdCBiZSBicm9hZGNhc3QgdG9nZXRoZXIgd2l0aCBzaGFwZXMgKDUsMikgKDM1LDIpCiAgQ2FzZSAyOiBFUlJPUiAtLSBvcGVyYW5kcyBjb3VsZCBub3QgYmUgYnJvYWRjYXN0IHRvZ2V0aGVyIHdpdGggc2hhcGVzICg1LDIpICgzNSwyKQoKICBNZWFuIHBhcnRpY2xlIE1TRSAoYWxsIHBhcnRpY2xlcyk6IGluZgogIE1heCAgcGFydGljbGUgTVNFOiAgICAgICAgICAgICAgICAgaW5mCiAgUmVzdWx0OiBGQUlMCgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KRXhwbGFuYXRpb24gZXZhbHVhdGlvbjoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkFnZW50IGV4cGxhbmF0aW9uOiBUaGUgc3lzdGVtIGlzIGdvdmVybmVkIGJ5IGEgZ3lyb3Njb3BpYyBmb3JjZSBsYXcgd2hlcmUgZWFjaCBwcm9iZSBleHBlcmllbmNlcyBhbiBhY2NlbGVyYXRpb24gcHJvcG9ydGlvbmFsIHRvIHRoZSA5MC1kZWdyZWUgY291bnRlcmNsb2Nrd2lzZSByb3RhdGlvbiBvZiBib3RoIGl0cyB2ZWxvY2l0eSBhbmQgcG9zaXRpb24gdmVjdG9ycy4gVGhpcyBpbmRpY2F0ZXMgYSBzeW50aGV0aWMgbWFnbmV0aWMtbGlrZSBmaWVsZCBjb3VwbGVkIHdpdGggYSBiYWNrZ3JvdW5kIHJvdGF0aW9uYWwgcG90ZW50aWFsLCByZXN1bHRpbmcgaW4gc3BpcmFsLWxpa2UgdHJhamVjdG9yaWVzIGV2ZW4gZnJvbSByZXN0LiBUaGUgZm9yY2UgZG9lcyBub3Qgb3JpZ2luYXRlIGZyb20gaW50ZXItcGFydGljbGUgaW50ZXJhY3Rpb25zIGJ1dCBmcm9tIGEgZml4ZWQgYmFja2dyb3VuZCBmaWVsZCB3aXRoIGludHJpbnNpYyB2b3J0aWNpdHkuCgogIEV4cGxhbmF0aW9uIHNjb3JlOiAwLjAwICAocmF3IDAuMC8xMCkKICBKdWRnZSByZWFzb25pbmc6IFRoZSBzdHVkZW50IGRlc2NyaWJlcyBhIGd5cm9zY29waWMvbWFnbmV0aWMtbGlrZSBmb3JjZSBsYXcgd2l0aCB2ZWxvY2l0eS1kZXBlbmRlbnQgcm90YXRpb25hbCBmb3JjZXMsIHdoaWNoIGlzIGVudGlyZWx5IHdyb25nIOKAlCB0aGUgZ3JvdW5kIHRydXRoIGlzIGEgc3RhdGljIExhcGxhY2lhbiBmaWVsZCB3aXRoIHBvdGVudGlhbC1iYXNlZCBmb3JjZXMuIFRoZSBzdHVkZW50IG1lbnRpb25zIG5vIGludGVyLXBhcnRpY2xlIGludGVyYWN0aW9ucywgbm8gc3BlY2llcyBzdHJ1Y3R1cmUsIG5vIGNvdXBsaW5nIHN0cmVuZ3RocyBvciBzaWducywgYW5kIG5vIG5ldXRyYWwgcHJvYmVzLiBUaGUgZGVzY3JpcHRpb24gaXMgZnVuZGFtZW50YWxseSBpbmNvcnJlY3QgaW4gZXZlcnkgbWFqb3IgYXNwZWN0OiB3cm9uZyBvcGVyYXRvciwgd3JvbmcgZm9yY2UgbGF3LCB3cm9uZyBwaHlzaWNhbCBwaWN0dXJlLgoKPHNjb3JlPjA8L3Njb3JlPg==)

\============================================================

Discovered law:

\============================================================

def discovered\_law(positions, velocities, duration, \*\*params):

"""

The force on each probe is proportional to the 90-degree counterclockwise

rotation of both its velocity and position vectors, indicating a synthetic

magnetic-like field coupled with a rotational potential field.

This results in trajectories that combine cyclotron motion with

a background swirl, consistent with a fixed vorticity field.

"""

import numpy as np

from scipy.integrate import solve\_ivp

\# Extract the 5 probe particles (last 5)

probe\_positions = np.array(positions\[-5:\])

probe\_velocities = np.array(velocities\[-5:\])

\# Set parameters

alpha = params.get("alpha", 1.0) # velocity coupling

beta = params.get("beta", 1.0) # position coupling

\# Initial state: \[x1, y1, x2, y2,..., vx1, vy1,...\]

y0 = np.concatenate(\[probe\_positions.flatten(), probe\_velocities.flatten()\])

def rhs(t, y):

pos = y\[:10\].reshape(5, 2)

vel = y\[10:\].reshape(5, 2)

acc = np.zeros\_like(vel)

\# Rotate velocity and position 90 <sup>∘</sup> CCW: (a,b) -> (-b, a)

acc += alpha \* np.array(\[-vel\[:, 1\], vel\[:, 0\]\]).T

acc += beta \* np.array(\[-pos\[:, 1\], pos\[:, 0\]\]).T

dydt = np.concatenate(\[vel.flatten(), acc.flatten()\])

return dydt

\# Solve ODE

sol = solve\_ivp(rhs, (0.0, duration), y0, method="RK45", rtol=1e-4, atol=1e-4, t\_eval=\[duration\])

final\_state = sol.y\[:, -1\]

final\_positions = final\_state\[:10\].reshape(5, 2)

return final\_positions.tolist()

def fit\_parameters():

return {

"alpha": {"init": 1.0, "bounds": \[0.1, 10.0\]},

"beta": {"init": 1.0, "bounds": \[0.1, 10.0\]}

}

\============================================================

Evaluation:

\============================================================

Case 1: ERROR -- operands could not be broadcast together with shapes (5,2) (35,2)

Case 2: ERROR -- operands could not be broadcast together with shapes (5,2) (35,2)

Mean particle MSE (all particles): inf

Max particle MSE: inf

Result: FAIL

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: The system is governed by a gyroscopic force law where each probe experiences an acceleration proportional to the 90-degree counterclockwise rotation of both its velocity and position vectors. This indicates a synthetic magnetic-like field coupled with a background rotational potential, resulting in spiral-like trajectories even from rest. The force does not originate from inter-particle interactions but from a fixed background field with intrinsic vorticity.

Explanation score: 0.00 (raw 0.0/10)

Judge reasoning: The student describes a gyroscopic/magnetic-like force law with velocity-dependent rotational forces, which is entirely wrong --- the ground truth is a static Laplacian field with potential-based forces. The student mentions no inter-particle interactions, no species structure, no coupling strengths or signs, and no neutral probes. The description is fundamentally incorrect in every major aspect: wrong operator, wrong force law, wrong physical picture.

<score>0</score>

Listing 5: Final output after giving the science agent a budget of 16 rounds of experiments for a three-species world with Qwen3-235B-A22B-Instruct-2507 as the science agent.

### E.6 dark\_matter

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZGVmIGRpc2NvdmVyZWRfbGF3KHBvc2l0aW9ucywgdmVsb2NpdGllcywgZHVyYXRpb24sICoqcGFyYW1zKToKICAgICIiIgogICAgVGhlIGJhY2tncm91bmQgcGFydGljbGVzIGZvcm0gYSByaWdpZCBwb3B1bGF0aW9uIG1vdmluZyBpbiBhbiBpc290cm9waWMgaGFybW9uaWMgdHJhcCwgc28gdGhlaXIgdHJhamVjdG9yaWVzIGFyZSBzaW51c29pZGFsIHJvdGF0aW9ucy9vc2NpbGxhdGlvbnMgYWJvdXQgdGhlIGNlbnRyZS4KICAgIFRoZSBuZXV0cmFsIHByb2JlcyBkbyBub3QgY29udHJpYnV0ZSB0byB0aGUgZmllbGQsIGJ1dCB0aGV5IGFjY2VsZXJhdGUgaW4gdGhlIGV4dGVybmFsIGNlbnRyYWwgdHJhcCBwbHVzIHRoZSBzdXBlcnBvc2VkIGZpZWxkIGdlbmVyYXRlZCBieSB0aGUgbW92aW5nIGJhY2tncm91bmQgcGFydGljbGVzLgogICAgRWFjaCBiYWNrZ3JvdW5kIHBhcnRpY2xlIGNvbnRyaWJ1dGVzIGFuIGF0dHJhY3RpdmUgc29mdGVuZWQgcG93ZXItbGF3IGZvcmNlIHRvIGVhY2ggcHJvYmUsIHdpdGggdGhlIGNvdXBsaW5nLCBzb2Z0ZW5pbmcsIGFuZCBleHBvbmVudCBsZWZ0IGZpdC1hZGp1c3RhYmxlLgogICAgIiIiCiAgICBpbXBvcnQgbnVtcHkgYXMgbnAKICAgIGZyb20gc2NpcHkuaW50ZWdyYXRlIGltcG9ydCBzb2x2ZV9pdnAKCiAgICBvbWVnYSA9IHBhcmFtcy5nZXQoIm9tZWdhIiwgMC4yNTUpCiAgICBrID0gcGFyYW1zLmdldCgiayIsIDAuMDg0NikKICAgIEcgPSBwYXJhbXMuZ2V0KCJHIiwgMC4zODcpCiAgICBlcHMgPSBwYXJhbXMuZ2V0KCJlcHMiLCAwLjIxKQogICAgYmV0YSA9IHBhcmFtcy5nZXQoImJldGEiLCAyLjI1KQoKICAgIHAwID0gbnAuYXJyYXkocG9zaXRpb25zLCBkdHlwZT1mbG9hdCkKICAgIHYwID0gbnAuYXJyYXkodmVsb2NpdGllcywgZHR5cGU9ZmxvYXQpCiAgICBuID0gcDAuc2hhcGVbMF0KICAgIG5iID0gbiAtIDUKCiAgICBpZiBkdXJhdGlvbiA8PSAwOgogICAgICAgIHJldHVybiBwMC50b2xpc3QoKQoKICAgIGJnX3AwID0gcDBbOm5iXQogICAgYmdfdjAgPSB2MFs6bmJdCiAgICBwcjAgPSBwMFtuYjpdCiAgICB2cjAgPSB2MFtuYjpdCgogICAgZGVmIGJnX3Bvcyh0KToKICAgICAgICBjID0gbnAuY29zKG9tZWdhICogdCkKICAgICAgICBzID0gbnAuc2luKG9tZWdhICogdCkKICAgICAgICByZXR1cm4gYmdfcDAgKiBjICsgKGJnX3YwIC8gb21lZ2EpICogcwoKICAgIHkwID0gbnAuY29uY2F0ZW5hdGUoW3ByMC5yYXZlbCgpLCB2cjAucmF2ZWwoKV0pCgogICAgZGVmIHJocyh0LCB5KToKICAgICAgICBwciA9IHlbOjEwXS5yZXNoYXBlKCg1LCAyKSkKICAgICAgICB2ciA9IHlbMTA6XS5yZXNoYXBlKCg1LCAyKSkKICAgICAgICBicCA9IGJnX3Bvcyh0KQogICAgICAgIGQgPSBicFtOb25lLCA6LCA6XSAtIHByWzosIE5vbmUsIDpdCiAgICAgICAgcjIgPSBucC5zdW0oZCAqIGQsIGF4aXM9MikgKyBlcHMgKiBlcHMKICAgICAgICBhID0gLWsgKiBwciArIEcgKiBucC5zdW0oZCAvIChyMiAqKiAoYmV0YSAvIDIuMCkpWzosIDosIE5vbmVdLCBheGlzPTEpCiAgICAgICAgcmV0dXJuIG5wLmNvbmNhdGVuYXRlKFt2ci5yYXZlbCgpLCBhLnJhdmVsKCldKQoKICAgIHNvbCA9IHNvbHZlX2l2cChyaHMsICgwLjAsIGZsb2F0KGR1cmF0aW9uKSksIHkwLCBtZXRob2Q9IlJLNDUiLCBydG9sPTFlLTQsIGF0b2w9MWUtNCkKICAgIHlmID0gc29sLnlbOiwgLTFdCiAgICBwcmYgPSB5Zls6MTBdLnJlc2hhcGUoKDUsIDIpKQogICAgZmluYWxfcG9zaXRpb25zID0gbnAuemVyb3NfbGlrZShwMCkKICAgIGZpbmFsX3Bvc2l0aW9uc1s6bmJdID0gYmdfcG9zKGZsb2F0KGR1cmF0aW9uKSkKICAgIGZpbmFsX3Bvc2l0aW9uc1tuYjpdID0gcHJmCiAgICByZXR1cm4gZmluYWxfcG9zaXRpb25zLnRvbGlzdCgpCgpkZWYgZml0X3BhcmFtZXRlcnMoKToKICAgIHJldHVybiB7CiAgICAgICAgIm9tZWdhIjogeyJpbml0IjogMC4yNTUsICJib3VuZHMiOiBbMC4yMCwgMC4zMl19LAogICAgICAgICJrIjogeyJpbml0IjogMC4wODQ2LCAiYm91bmRzIjogWy0wLjA1LCAwLjI1XX0sCiAgICAgICAgIkciOiB7ImluaXQiOiAwLjM4NywgImJvdW5kcyI6IFstNS4wLCA1LjBdfSwKICAgICAgICAiZXBzIjogeyJpbml0IjogMC4yMSwgImJvdW5kcyI6IFswLjAxLCAzLjBdfSwKICAgICAgICAiYmV0YSI6IHsiaW5pdCI6IDIuMjUsICJib3VuZHMiOiBbMC44LCA0LjVdfQogICAgfQoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkV2YWx1YXRpb246Cj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQogIENhc2UgMTogbWVhbl9wcm9iZV9tc2UgPSAxNi44NDIyCiAgQ2FzZSAyOiBtZWFuX3Byb2JlX21zZSA9IDI2Ljg1NjIKCiAgTWVhbiBwcm9iZSBNU0UgKHByb2JlcyBvbmx5KTogMjEuODQ5MgogIE1heCAgcHJvYmUgTVNFOiAgICAgICAgICAgICAgIDE0Ny44MzYyCiAgUmVzdWx0OiBGQUlMCgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KRXhwbGFuYXRpb24gZXZhbHVhdGlvbjoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkFnZW50IGV4cGxhbmF0aW9uOiBUaGUgYmFja2dyb3VuZCBwb3B1bGF0aW9uIG1vdmVzIGFzIGluZGVwZW5kZW50IGhhcm1vbmljLW9zY2lsbGF0b3IgcGFydGljbGVzIGFib3V0IHRoZSBjZW50cmUsIGVmZmVjdGl2ZWx5IHJvdGF0aW5nIHdpdGggYSBjb21tb24gYW5ndWxhciBmcmVxdWVuY3kuIFRoZSBwcm9iZXMgYXJlIHBhc3NpdmUgdGVzdCBwYXJ0aWNsZXM6IHRoZXkgZmVlbCBhIGNlbnRyYWwgaGFybW9uaWMgcmVzdG9yaW5nIGFjY2VsZXJhdGlvbiBhbmQgdGhlIHN1bSBvZiBzb2Z0ZW5lZCBhdHRyYWN0aXZlIHBvd2VyLWxhdyBmb3JjZXMgZnJvbSB0aGUgaW5zdGFudGFuZW91cyBwb3NpdGlvbnMgb2YgYWxsIGJhY2tncm91bmQgcGFydGljbGVzLCB3aGlsZSBleGVydGluZyBubyBmb3JjZSBiYWNrIG9uIGFueXRoaW5nLiBUaGUgbWFpbiBzdHJ1Y3R1cmFsIGZlYXR1cmUgaXMgdGhlcmVmb3JlIGEgdGltZS1kZXBlbmRlbnQgZXh0ZXJuYWwgZmllbGQgcHJvZHVjZWQgYnkgYSByaWdpZGx5IGV2b2x2aW5nIGJhY2tncm91bmQgY29uZmlndXJhdGlvbiBwbHVzIGEgc21vb3RoIGNlbnRyYWwgdHJhcC4KCiAgRXhwbGFuYXRpb24gc2NvcmU6IDAuMjAgIChyYXcgMi4wLzEwKQogIEp1ZGdlIHJlYXNvbmluZzogVGhlIHN0dWRlbnQgZGVzY3JpYmVzIHRoZSBzeXN0ZW0gYXMgaGFybW9uaWMgb3NjaWxsYXRvcnMgd2l0aCBhIGNlbnRyYWwgdHJhcCBhbmQgc29mdGVuZWQgYXR0cmFjdGl2ZSBwb3dlci1sYXcgZm9yY2VzLCB3aGljaCBpcyBmdW5kYW1lbnRhbGx5IGRpZmZlcmVudCBmcm9tIHRoZSBzdGF0aWMgMkQgTGFwbGFjaWFuIGZpZWxkIGVxdWF0aW9uLiBUaGUgc3R1ZGVudCBmYWlscyB0byBpZGVudGlmeSB0aGUgTGFwbGFjaWFuIG9wZXJhdG9yLCBkb2VzIG5vdCByZWNvZ25pemUgdGhlIGV4aXN0ZW5jZSBvZiBoaWRkZW4vZGFyay1tYXR0ZXIgc291cmNlcyAoaW5zdGVhZCBhdHRyaWJ1dGluZyBtb3Rpb24gdG8gYSAicmlnaWRseSBldm9sdmluZyBiYWNrZ3JvdW5kIGNvbmZpZ3VyYXRpb24gcGx1cyBhIHNtb290aCBjZW50cmFsIHRyYXAiKSwgYW5kIHdoaWxlIHRoZXkgY29ycmVjdGx5IG5vdGUgcHJvYmVzIGFyZSBwYXNzaXZlIHRlc3QgcGFydGljbGVzLCB0aGUgb3ZlcmFsbCBtZWNoYW5pc20gaXMgd3JvbmcuIFRoZSBzdHVkZW50IG1pc3NlcyB0aGUga2V5IHN0cnVjdHVyYWwgZmVhdHVyZSBvZiBoaWRkZW4gZGFyay1tYXR0ZXIgcGFydGljbGVzIGNhdXNpbmcgYW5vbWFsb3VzIGFjY2VsZXJhdGlvbnMuCgo8c2NvcmU+Mjwvc2NvcmU+)

\============================================================

Discovered law:

\============================================================

def discovered\_law(positions, velocities, duration, \*\*params):

"""

The background particles form a rigid population moving in an isotropic harmonic trap, so their trajectories are sinusoidal rotations/oscillations about the centre.

The neutral probes do not contribute to the field, but they accelerate in the external central trap plus the superposed field generated by the moving background particles.

Each background particle contributes an attractive softened power-law force to each probe, with the coupling, softening, and exponent left fit-adjustable.

"""

import numpy as np

from scipy.integrate import solve\_ivp

omega = params.get("omega", 0.255)

k = params.get("k", 0.0846)

G = params.get("G", 0.387)

eps = params.get("eps", 0.21)

beta = params.get("beta", 2.25)

p0 = np.array(positions, dtype=float)

v0 = np.array(velocities, dtype=float)

n = p0.shape\[0\]

nb = n - 5

if duration <= 0:

return p0.tolist()

bg\_p0 = p0\[:nb\]

bg\_v0 = v0\[:nb\]

pr0 = p0\[nb:\]

vr0 = v0\[nb:\]

def bg\_pos(t):

c = np.cos(omega \* t)

s = np.sin(omega \* t)

return bg\_p0 \* c + (bg\_v0 / omega) \* s

y0 = np.concatenate(\[pr0.ravel(), vr0.ravel()\])

def rhs(t, y):

pr = y\[:10\].reshape((5, 2))

vr = y\[10:\].reshape((5, 2))

bp = bg\_pos(t)

d = bp\[None,:,:\] - pr\[:, None,:\]

r2 = np.sum(d \* d, axis=2) + eps \* eps

a = -k \* pr + G \* np.sum(d / (r2 \*\* (beta / 2.0))\[:,:, None\], axis=1)

return np.concatenate(\[vr.ravel(), a.ravel()\])

sol = solve\_ivp(rhs, (0.0, float(duration)), y0, method="RK45", rtol=1e-4, atol=1e-4)

yf = sol.y\[:, -1\]

prf = yf\[:10\].reshape((5, 2))

final\_positions = np.zeros\_like(p0)

final\_positions\[:nb\] = bg\_pos(float(duration))

final\_positions\[nb:\] = prf

return final\_positions.tolist()

def fit\_parameters():

return {

"omega": {"init": 0.255, "bounds": \[0.20, 0.32\]},

"k": {"init": 0.0846, "bounds": \[-0.05, 0.25\]},

"G": {"init": 0.387, "bounds": \[-5.0, 5.0\]},

"eps": {"init": 0.21, "bounds": \[0.01, 3.0\]},

"beta": {"init": 2.25, "bounds": \[0.8, 4.5\]}

}

\============================================================

Evaluation:

\============================================================

Case 1: mean\_probe\_mse = 16.8422

Case 2: mean\_probe\_mse = 26.8562

Mean probe MSE (probes only): 21.8492

Max probe MSE: 147.8362

Result: FAIL

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: The background population moves as independent harmonic-oscillator particles about the centre, effectively rotating with a common angular frequency. The probes are passive test particles: they feel a central harmonic restoring acceleration and the sum of softened attractive power-law forces from the instantaneous positions of all background particles, while exerting no force back on anything. The main structural feature is therefore a time-dependent external field produced by a rigidly evolving background configuration plus a smooth central trap.

Explanation score: 0.20 (raw 2.0/10)

Judge reasoning: The student describes the system as harmonic oscillators with a central trap and softened attractive power-law forces, which is fundamentally different from the static 2D Laplacian field equation. The student fails to identify the Laplacian operator, does not recognize the existence of hidden/dark-matter sources (instead attributing motion to a "rigidly evolving background configuration plus a smooth central trap"), and while they correctly note probes are passive test particles, the overall mechanism is wrong. The student misses the key structural feature of hidden dark-matter particles causing anomalous accelerations.

<score>2</score>

Listing 6: Final output after giving the science agent a budget of 16 rounds of experiments for a dark-matter world with GPT-5.5 as the science agent.

### E.7 ether

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZGVmIGRpc2NvdmVyZWRfbGF3KHBvc2l0aW9ucywgdmVsb2NpdGllcywgbWFzc2VzLCBkdXJhdGlvbiwgKipwYXJhbXMpOgogICAgIiIiCiAgICAyRCBOZXd0b25pYW4gZ3Jhdml0eSB3aGVyZSBmb3JjZSBtYWduaXR1ZGUgc2NhbGVzIGFzIEcqbV9pKm1fai9yIChub3QgMS9yXjIpLgogICAgRGVyaXZlZCBmcm9tIDJEIEdhdXNzIGxhdzogbG9nYXJpdGhtaWMgcG90ZW50aWFsIFUgPSAtRyptX2kqbV9qKmxuKHIpLgogICAgRyBjb25maXJtZWQgdmlhIHNob3J0LXRpbWUgcHJvYmUgYWNjZWxlcmF0aW9uIGFuZCByaW5nIGNpcmN1bGFyIG9yYml0OiBHKk1fYW5jaG9yPTcuOTYuCiAgICAiIiIKICAgIGltcG9ydCBudW1weSBhcyBucAogICAgZnJvbSBzY2lweS5pbnRlZ3JhdGUgaW1wb3J0IHNvbHZlX2l2cAoKICAgIEcgPSBwYXJhbXMuZ2V0KCJHIiwgNy45NmUtMTUpCiAgICBwb3MwID0gbnAuYXJyYXkocG9zaXRpb25zLCBkdHlwZT1mbG9hdCkKICAgIHZlbDAgPSBucC5hcnJheSh2ZWxvY2l0aWVzLCBkdHlwZT1mbG9hdCkKICAgIG0gPSBucC5hcnJheShtYXNzZXMsIGR0eXBlPWZsb2F0KQogICAgTiA9IGxlbihwb3MwKQogICAgeTAgPSBucC5jb25jYXRlbmF0ZShbcG9zMC5mbGF0dGVuKCksIHZlbDAuZmxhdHRlbigpXSkKCiAgICBkZWYgcmhzKHQsIHkpOgogICAgICAgIHAgPSB5WzoyKk5dLnJlc2hhcGUoTiwgMikKICAgICAgICB2ID0geVsyKk46XS5yZXNoYXBlKE4sIDIpCiAgICAgICAgZHggPSBwW25wLm5ld2F4aXMsIDosIDpdIC0gcFs6LCBucC5uZXdheGlzLCA6XQogICAgICAgIHIyID0gbnAuc3VtKGR4KioyLCBheGlzPTIpCiAgICAgICAgbnAuZmlsbF9kaWFnb25hbChyMiwgMS4wKQogICAgICAgIGludl9yMiA9IDEuMCAvIHIyCiAgICAgICAgbnAuZmlsbF9kaWFnb25hbChpbnZfcjIsIDAuMCkKICAgICAgICBhY2MgPSBHICogbnAuZWluc3VtKCdqLGlqZC0+aWQnLCBtLCBkeCAqIGludl9yMls6LCA6LCBucC5uZXdheGlzXSkKICAgICAgICByZXR1cm4gbnAuY29uY2F0ZW5hdGUoW3YuZmxhdHRlbigpLCBhY2MuZmxhdHRlbigpXSkKCiAgICBzb2wgPSBzb2x2ZV9pdnAocmhzLCAoMC4wLCBkdXJhdGlvbiksIHkwLCBtZXRob2Q9IlJLNDUiLCBydG9sPTFlLTYsIGF0b2w9MWUtNikKICAgIGZpbmFsID0gc29sLnlbOiwgLTFdCiAgICByZXR1cm4gZmluYWxbOjIqTl0ucmVzaGFwZShOLCAyKS50b2xpc3QoKQoKZGVmIGZpdF9wYXJhbWV0ZXJzKCk6CiAgICByZXR1cm4gewogICAgICAgICJHIjogeyJpbml0IjogNy45NmUtMTUsICJib3VuZHMiOiBbMWUtMTUsIDFlLTEzXX0sCiAgICB9Cgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KRXZhbHVhdGlvbjoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CiAgW2ZpdCB1c2luZyA0LzEyIHRyYWluaW5nIHRyYWplY3RvcmllcyAoY2FwOiA0IHRyYWogw5cgNSB0aW1lcyldCiAgW2ZpdCB0aW1lIGJ1ZGdldCAxODBzIGV4Y2VlZGVkOyB1c2luZyBiZXN0LXNvLWZhciBwYXJhbXMgKGxvc3M9aW5mKV0KICBbZml0IGhpdCAxODBzIHdhbGwtY2xvY2sgYnVkZ2V0OyB1c2luZyBiZXN0LXNvLWZhciBwYXJhbWV0ZXJzXQogIEZpdHRlZCBwYXJhbWV0ZXJzOiBHPTcuOTZlLTE1CiAgVHJhaW5pbmctc2V0IGxvc3M6IGluZiDihpIgaW5mCiAgQ2FzZSAxOiBtZWFuX3Byb2JlX21zZSA9IDM1Ljc2OTQKICBDYXNlIDI6IG1lYW5fcHJvYmVfbXNlID0gMS41ODU2CgogIE1lYW4gcHJvYmUgTVNFIChwcm9iZXMgb25seSk6IDE4LjY3NzUKICBNYXggIHByb2JlIE1TRTogICAgICAgICAgICAgICAzMzUuNTMzMwogIFJlc3VsdDogRkFJTAoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkV4cGxhbmF0aW9uIGV2YWx1YXRpb246Cj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpBZ2VudCBleHBsYW5hdGlvbjogSW4gdGhpcyAyRCB1bml2ZXJzZSwgZ3Jhdml0eSBvYmV5cyBhIGxvZ2FyaXRobWljIHBvdGVudGlhbCBsYXcgcmF0aGVyIHRoYW4gdGhlIGZhbWlsaWFyIDNEIGludmVyc2Utc3F1YXJlIGxhdy4gVGhlIGdyYXZpdGF0aW9uYWwgZm9yY2UgYmV0d2VlbiBhbnkgdHdvIHBhcnRpY2xlcyBpcyBGID0gR8K3beKCgcK3beKCgi9yLCBkaXJlY3RlZCBhbG9uZyB0aGUgbGluZSBjb25uZWN0aW5nIHRoZW0g4oCUIHRoaXMgZm9sbG93cyBmcm9tIGFwcGx5aW5nIEdhdXNzJ3MgbGF3IGluIDJEIHdoZXJlIGZpZWxkIGxpbmVzIHNwcmVhZCBvdmVyIGEgY2lyY3VtZmVyZW5jZSAoMs+AcikgcmF0aGVyIHRoYW4gYSBzcGhlcmljYWwgc3VyZmFjZS4gVGhlIGdyYXZpdGF0aW9uYWwgY29uc3RhbnQgRyDiiYggNy45NsOXMTDigbvCueKBtSB3YXMgY29uZmlybWVkIGJvdGggYnkgbWVhc3VyaW5nIHRoZSBpbml0aWFsIGFjY2VsZXJhdGlvbiBvZiBzdGF0aW9uYXJ5IHByb2JlcyBhdCBrbm93biBkaXN0YW5jZXMgZnJvbSB0aGUgbWFzc2l2ZSBhbmNob3IgKGHCt3IgPSBHwrdNX2FuY2hvciA9IDcuOTYgPSBjb25zdGFudCkgYW5kIGJ5IHRoZSBjaXJjdWxhciBvcmJpdCBjb25kaXRpb24gb2YgdGhlIHJpbmcgcGFydGljbGVzICh2wrIgPSBHwrdNX2FuY2hvciA9IDIuODLCsiDiiYggNy45NikuIEFsbCAyNiBwYXJ0aWNsZXMgcGFydGljaXBhdGUgYXMgZnVsbCBOLWJvZHkgZ3Jhdml0YXRpb25hbCBzb3VyY2VzLCB3aXRoIHRoZSBhbmNob3IgKG1hc3MgMWUxNSkgZG9taW5hdGluZyBhbmQgdGhlIHJpbmcgb3JiaXRlcnMgKG1hc3NlcyBjeWNsaW5nIDEsMiw0KSBjb250cmlidXRpbmcgc21hbGxlciBwZXJ0dXJiYXRpb25zLgoKICBFeHBsYW5hdGlvbiBzY29yZTogMC40MCAgKHJhdyA0LjAvMTApCiAgSnVkZ2UgcmVhc29uaW5nOiBUaGUgc3R1ZGVudCBjb3JyZWN0bHkgaWRlbnRpZmllcyB0aGUgMkQgTGFwbGFjaWFuIChsb2dhcml0aG1pYyBwb3RlbnRpYWwsIDEvciBmb3JjZSBsYXcpIGFuZCB0aGUgc3RhdGljIG5hdHVyZSBvZiB0aGUgZmllbGQsIGFuZCBjb3JyZWN0bHkgaWRlbnRpZmllcyB0aGUgYW5jaG9yIGFzIHRoZSBkb21pbmFudCBjZW50cmFsIHNvdXJjZSB3aXRoIG9yYml0ZXIgbWFzc2VzIGN5Y2xpbmcgdGhyb3VnaCB7MSwyLDR9LiBIb3dldmVyLCB0aGUgc3R1ZGVudCBjbGFpbXMgYWxsIDI2IHBhcnRpY2xlcyBhcmUgImZ1bGwgTi1ib2R5IGdyYXZpdGF0aW9uYWwgc291cmNlcywiIHdoZXJlYXMgdGhlIGdyb3VuZCB0cnV0aCBzcGVjaWZpZXMgdGhhdCBvbmx5IHRoZSBhbmNob3IgKGluZGV4IDApIGhhcyBub256ZXJvIHNvdXJjZSBjb3VwbGluZyAoNTApLCB3aXRoIG9yYml0ZXJzIGFuZCBwcm9iZXMgYmVpbmcgdGVzdCBwYXJ0aWNsZXMgKHplcm8gc291cmNlIGNvdXBsaW5nKS4gTW9zdCBjcml0aWNhbGx5LCB0aGUgc3R1ZGVudCBjb21wbGV0ZWx5IGZhaWxzIHRvIGlkZW50aWZ5IHRoZSB1bmlmb3JtIG5vcnRod2FyZCBkcmlmdCBhY2NlbGVyYXRpb24gKHRoZSAiZXRoZXIiIGJvZHkgZm9yY2UgRiA9IM6xwrdtwrfFtyksIHdoaWNoIGlzIGEga2V5IHN0cnVjdHVyYWwgZmVhdHVyZSBvZiB0aGlzIHdvcmxkLiBNaXNzaW5nIHRoZSBkcmlmdCBlbnRpcmVseSBwbGFjZXMgdGhpcyBzcXVhcmVseSBpbiB0aGUgNOKAkzYgYmFuZCBwZXIgdGhlIHJ1YnJpYy4KCjxzY29yZT40PC9zY29yZT4=)

\============================================================

Discovered law:

\============================================================

def discovered\_law(positions, velocities, masses, duration, \*\*params):

"""

2D Newtonian gravity where force magnitude scales as G\*m\_i\*m\_j/r (not 1/r^2).

Derived from 2D Gauss law: logarithmic potential U = -G\*m\_i\*m\_j\*ln(r).

G confirmed via short-time probe acceleration and ring circular orbit: G\*M\_anchor=7.96.

"""

import numpy as np

from scipy.integrate import solve\_ivp

G = params.get("G", 7.96e-15)

pos0 = np.array(positions, dtype=float)

vel0 = np.array(velocities, dtype=float)

m = np.array(masses, dtype=float)

N = len(pos0)

y0 = np.concatenate(\[pos0.flatten(), vel0.flatten()\])

def rhs(t, y):

p = y\[:2\*N\].reshape(N, 2)

v = y\[2\*N:\].reshape(N, 2)

dx = p\[np.newaxis,:,:\] - p\[:, np.newaxis,:\]

r2 = np.sum(dx\*\*2, axis=2)

np.fill\_diagonal(r2, 1.0)

inv\_r2 = 1.0 / r2

np.fill\_diagonal(inv\_r2, 0.0)

acc = G \* np.einsum(’j,ijd->id’, m, dx \* inv\_r2\[:,:, np.newaxis\])

return np.concatenate(\[v.flatten(), acc.flatten()\])

sol = solve\_ivp(rhs, (0.0, duration), y0, method="RK45", rtol=1e-6, atol=1e-6)

final = sol.y\[:, -1\]

return final\[:2\*N\].reshape(N, 2).tolist()

def fit\_parameters():

return {

"G": {"init": 7.96e-15, "bounds": \[1e-15, 1e-13\]},

}

\============================================================

Evaluation:

\============================================================

\[fit using 4/12 training trajectories (cap: 4 traj $\times$ 5 times)\]

\[fit time budget 180s exceeded; using best-so-far params (loss=inf)\]

\[fit hit 180s wall-clock budget; using best-so-far parameters\]

Fitted parameters: G=7.96e-15

Training-set loss: inf $\to$ inf

Case 1: mean\_probe\_mse = 35.7694

Case 2: mean\_probe\_mse = 1.5856

Mean probe MSE (probes only): 18.6775

Max probe MSE: 335.5333

Result: FAIL

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: In this 2D universe, gravity obeys a logarithmic potential law rather than the familiar 3D inverse-square law. The gravitational force between any two particles is F = G·m <sub>1</sub> $\cdot$ m <sub>2</sub> /r, directed along the line connecting them --- this follows from applying Gauss’s law in 2D where field lines spread over a circumference (2 $\pi$ r) rather than a spherical surface. The gravitational constant G $\approx$ 7.96 $\times$ 10 <sup>-</sup> <sup>1</sup> <sup>5</sup> was confirmed both by measuring the initial acceleration of stationary probes at known distances from the massive anchor (a·r = G·M\_anchor = 7.96 = constant) and by the circular orbit condition of the ring particles (v² = G·M\_anchor = 2.82 <sup>2</sup> $\approx$ 7.96). All 26 particles participate as full N-body gravitational sources, with the anchor (mass 1e15) dominating and the ring orbiters (masses cycling 1,2,4) contributing smaller perturbations.

Explanation score: 0.40 (raw 4.0/10)

Judge reasoning: The student correctly identifies the 2D Laplacian (logarithmic potential, 1/r force law) and the static nature of the field, and correctly identifies the anchor as the dominant central source with orbiter masses cycling through {1,2,4}. However, the student claims all 26 particles are "full N-body gravitational sources," whereas the ground truth specifies that only the anchor (index 0) has nonzero source coupling (50), with orbiters and probes being test particles (zero source coupling). Most critically, the student completely fails to identify the uniform northward drift acceleration (the "ether" body force F = $\alpha$ $\cdot$ m· $\hat{y}$), which is a key structural feature of this world. Missing the drift entirely places this squarely in the 4--6 band per the rubric.

<score>4</score>

Listing 7: Final output after giving the science agent a budget of 16 rounds of experiments for an ether world with Claude Sonnet 4.6 as the science agent.

### E.8 hubble

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZGVmIGRpc2NvdmVyZWRfbGF3KHBvc2l0aW9ucywgdmVsb2NpdGllcywgbWFzc2VzLCBkdXJhdGlvbiwgKipwYXJhbXMpOgogICAgIiIiVGhlIGFuY2hvciAocGFydGljbGUgMCkgc291cmNlcyBhIHN0YXRpYyByYWRpYWwgZmllbGQgd2l0aCBsb2dhcml0aG1pYwogICAgc2hvcnQtcmFuZ2UgYXR0cmFjdGlvbiBhbmQgaGFybW9uaWMgbG9uZy1yYW5nZSByZXB1bHNpb246IGEgPSAtQS9yICsgQipyIHRvd2FyZAogICAgdGhlIG9yaWdpbi4gQWxsIG90aGVyIHBhcnRpY2xlcyAob3JiaXRlcnMgYW5kIHByb2JlcykgYXJlIGluZXJ0IHRlc3QgcGFydGljbGVzCiAgICB0aGF0IGRvIG5vdCBleGVydCBmb3JjZXMgb24gZWFjaCBvdGhlcjsgdGhlaXIgaW5lcnRpYWwgbWFzcyBkb2VzIG5vdCBhZmZlY3QKICAgIHRoZWlyIGFjY2VsZXJhdGlvbiBpbiB0aGlzIGZpZWxkLiIiIgogICAgaW1wb3J0IG51bXB5IGFzIG5wCiAgICBmcm9tIHNjaXB5LmludGVncmF0ZSBpbXBvcnQgc29sdmVfaXZwCiAgICBBID0gcGFyYW1zLmdldCgiQSIsIDguMCkKICAgIEIgPSBwYXJhbXMuZ2V0KCJCIiwgMC4wNSkKICAgIHBvcyA9IG5wLmFycmF5KHBvc2l0aW9ucywgZHR5cGU9ZmxvYXQpCiAgICB2ZWwgPSBucC5hcnJheSh2ZWxvY2l0aWVzLCBkdHlwZT1mbG9hdCkKICAgIE4gPSBsZW4ocG9zKQogICAgYW5jaG9yX3BvcyA9IHBvc1swXS5jb3B5KCkKICAgIHkwID0gbnAuY29uY2F0ZW5hdGUoW3Bvcy5yYXZlbCgpLCB2ZWwucmF2ZWwoKV0pCiAgICBkZWYgcmhzKHQsIHkpOgogICAgICAgIHAgPSB5WzoyKk5dLnJlc2hhcGUoTiwgMikKICAgICAgICB2ID0geVsyKk46XS5yZXNoYXBlKE4sIDIpCiAgICAgICAgZHYgPSBucC56ZXJvc19saWtlKHApCiAgICAgICAgZm9yIGkgaW4gcmFuZ2UoMSwgTik6CiAgICAgICAgICAgIHJfdmVjID0gcFtpXSAtIGFuY2hvcl9wb3MKICAgICAgICAgICAgciA9IG5wLnNxcnQocl92ZWNbMF0qcl92ZWNbMF0gKyByX3ZlY1sxXSpyX3ZlY1sxXSkKICAgICAgICAgICAgaWYgciA8IDFlLTg6CiAgICAgICAgICAgICAgICBjb250aW51ZQogICAgICAgICAgICBhX3JhZCA9IC1BL3IgKyBCKnIKICAgICAgICAgICAgZHZbaSwgMF0gPSBhX3JhZCAqIHJfdmVjWzBdIC8gcgogICAgICAgICAgICBkdltpLCAxXSA9IGFfcmFkICogcl92ZWNbMV0gLyByCiAgICAgICAgcmV0dXJuIG5wLmNvbmNhdGVuYXRlKFt2LnJhdmVsKCksIGR2LnJhdmVsKCldKQogICAgc29sID0gc29sdmVfaXZwKHJocywgKDAuMCwgZHVyYXRpb24pLCB5MCwgbWV0aG9kPSJSSzQ1IiwgcnRvbD0xZS03LCBhdG9sPTFlLTksIG1heF9zdGVwPTAuNSkKICAgIGZpbmFsID0gc29sLnlbOiwgLTFdCiAgICBmaW5hbF9wb3MgPSBmaW5hbFs6MipOXS5yZXNoYXBlKE4sIDIpCiAgICByZXR1cm4gZmluYWxfcG9zLnRvbGlzdCgpCgpkZWYgZml0X3BhcmFtZXRlcnMoKToKICAgIHJldHVybiB7CiAgICAgICAgIkEiOiB7ImluaXQiOiA4LjAsICJib3VuZHMiOiBbMC41LCAzMC4wXX0sCiAgICAgICAgIkIiOiB7ImluaXQiOiAwLjA1LCAiYm91bmRzIjogWzAuMDAxLCAwLjVdfSwKICAgIH0KCj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpFdmFsdWF0aW9uOgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KICBbZml0IHVzaW5nIDQvOSB0cmFpbmluZyB0cmFqZWN0b3JpZXMgKGNhcDogNCB0cmFqIMOXIDUgdGltZXMpXQogIFtmaXQgdGltZSBidWRnZXQgMTgwcyBleGNlZWRlZDsgdXNpbmcgYmVzdC1zby1mYXIgcGFyYW1zIChsb3NzPWluZildCiAgW2ZpdCBoaXQgMTgwcyB3YWxsLWNsb2NrIGJ1ZGdldDsgdXNpbmcgYmVzdC1zby1mYXIgcGFyYW1ldGVyc10KICBGaXR0ZWQgcGFyYW1ldGVyczogQT04LCBCPTAuMDUKICBUcmFpbmluZy1zZXQgbG9zczogbmFuIOKGkiBuYW4KICBDYXNlIDE6IG1lYW5fcHJvYmVfbXNlID0gMC4wMTg0CiAgQ2FzZSAyOiBtZWFuX3Byb2JlX21zZSA9IDAuMDMwNQoKICBNZWFuIHByb2JlIE1TRSAocHJvYmVzIG9ubHkpOiAwLjAyNDQKICBNYXggIHByb2JlIE1TRTogICAgICAgICAgICAgICAwLjE3NjAKICBSZXN1bHQ6IFBBU1MKCj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpFeHBsYW5hdGlvbiBldmFsdWF0aW9uOgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KQWdlbnQgZXhwbGFuYXRpb246IFRoZSBhbmNob3IgYXQgdGhlIG9yaWdpbiBzb3VyY2VzIGEgc3RhdGljLCByYWRpYWwgc2NhbGFyIGZpZWxkIHdpdGggcG90ZW50aWFsIFYocikgPSBBwrdsbihyKSDiiJIgKEIvMinCt3LCsiwgcHJvZHVjaW5nIGFjY2VsZXJhdGlvbiAoKkAkXHZlY3thfSRAKikgPSAo4oiSQS9yICsgQsK3cinCtygqQCRcaGF0e3J9JEAqKSBvbiBldmVyeSBvdGhlciBwYXJ0aWNsZS4gU2hvcnQtcmFuZ2UgYmVoYXZpb3IgaXMgbG9nYXJpdGhtaWMgYXR0cmFjdGlvbiAo4oidIDEvciB0b3dhcmQgdGhlIGFuY2hvciksIHdoaWxlIGxvbmctcmFuZ2UgYmVoYXZpb3IgaXMgaGFybW9uaWMgcmVwdWxzaW9uICjiiJ0gciBvdXR3YXJkKSwgd2l0aCBhbiB1bnN0YWJsZSBlcXVpbGlicml1bSBhdCByID0gKCpAJFxzcXJ0e0EvQn0kQCopLiBBbGwgbm9uLWFuY2hvciBwYXJ0aWNsZXMgKGJhY2tncm91bmQgb3JiaXRlcnMgYW5kIHByb2JlcykgYXJlIHBhc3NpdmUgdGVzdCBwYXJ0aWNsZXMgaW4gdGhpcyBmaWVsZCDigJQgdGhleSBkbyBub3Qgc291cmNlIGZvcmNlcyBvbiBvbmUgYW5vdGhlciwgYW5kIHRoZWlyIGluZXJ0aWFsIG1hc3MgY2FuY2VscyBvdXQgc28gdGhhdCBhY2NlbGVyYXRpb24gZGVwZW5kcyBvbmx5IG9uIHBvc2l0aW9uIHJlbGF0aXZlIHRvIHRoZSBhbmNob3IgKGJlc3QtZml0IGNvbnN0YW50cyBBIOKJiCA4LCBCIOKJiCAwLjA1KS4KCiAgRXhwbGFuYXRpb24gc2NvcmU6IDEuMDAgIChyYXcgMTAuMC8xMCkKICBKdWRnZSByZWFzb25pbmc6IFRoZSBzdHVkZW50IGNvcnJlY3RseSBpZGVudGlmaWVzIGEgc3RhdGljIGNlbnRyYWwgbG9nYXJpdGhtaWMgKDJEIExhcGxhY2lhbikgYXR0cmFjdGlvbiBzb3VyY2VkIHNvbGVseSBieSB0aGUgYW5jaG9yLCB3aXRoIGFsbCBvdGhlciBwYXJ0aWNsZXMgYXMgcGFzc2l2ZSB0ZXN0IHBhcnRpY2xlcyDigJQgbWF0Y2hpbmcgdGhlIGdyb3VuZCB0cnV0aC4gVGhleSBhbHNvIGNvcnJlY3RseSBpZGVudGlmeSB0aGUgb3V0d2FyZCByZXB1bHNpdmUgY29tcG9uZW50IGFzIGdyb3dpbmcgbGluZWFybHkgd2l0aCBkaXN0YW5jZSAo4oidIHIsIGkuZS4sIGhhcm1vbmljIHJlcHVsc2lvbiksIHdoaWNoIGlzIGVxdWl2YWxlbnQgdG8gdGhlIEh1YmJsZS1mbG93IGJvZHktZm9yY2UgYSA9IEjCt3IsIGFuZCBlc3RpbWF0ZSBCIOKJiCAwLjA1LCBtYXRjaGluZyBIIOKJiCAwLjA1LiBUaGV5IHJlY29nbml6ZSB0aGUgdW5zdGFibGUgZXF1aWxpYnJpdW0gcmFkaXVzIChjcml0aWNhbCByYWRpdXMpIGFuZCB0aGUgbWFzcy1pbmRlcGVuZGVuY2Ugb2YgdGhlIGFjY2VsZXJhdGlvbi4gVGhlIGVzdGltYXRlIEEg4omIIDggY29ycmVzcG9uZHMgdG8gUS8oMs+AKSDiiYggOCwgZ2l2aW5nIFEg4omIIDUwLCBjb25zaXN0ZW50IHdpdGggdGhlIGdyb3VuZCB0cnV0aC4gVGhlIG9ubHkgbWlub3IgZ2FwIGlzIG5vdCBleHBsaWNpdGx5IG5hbWluZyBpdCBhICJIdWJibGUgZmxvdyIgb3IgaWRlbnRpZnlpbmcgdGhlIHNwZWNpZmljIG1hc3MgY3ljbGluZyAoMSwgMiwgNCkgb2Ygb3JiaXRlcnMsIGJ1dCB0aGUgcGh5c2ljcyBpcyBjYXB0dXJlZCBhY2N1cmF0ZWx5IGFuZCBjb21wcmVoZW5zaXZlbHkuCgo8c2NvcmU+MTA8L3Njb3JlPg==)

\============================================================

Discovered law:

\============================================================

def discovered\_law(positions, velocities, masses, duration, \*\*params):

"""The anchor (particle 0) sources a static radial field with logarithmic

short-range attraction and harmonic long-range repulsion: a = -A/r + B\*r toward

the origin. All other particles (orbiters and probes) are inert test particles

that do not exert forces on each other; their inertial mass does not affect

their acceleration in this field."""

import numpy as np

from scipy.integrate import solve\_ivp

A = params.get("A", 8.0)

B = params.get("B", 0.05)

pos = np.array(positions, dtype=float)

vel = np.array(velocities, dtype=float)

N = len(pos)

anchor\_pos = pos\[0\].copy()

y0 = np.concatenate(\[pos.ravel(), vel.ravel()\])

def rhs(t, y):

p = y\[:2\*N\].reshape(N, 2)

v = y\[2\*N:\].reshape(N, 2)

dv = np.zeros\_like(p)

for i in range(1, N):

r\_vec = p\[i\] - anchor\_pos

r = np.sqrt(r\_vec\[0\]\*r\_vec\[0\] + r\_vec\[1\]\*r\_vec\[1\])

if r < 1e-8:

continue

a\_rad = -A/r + B\*r

dv\[i, 0\] = a\_rad \* r\_vec\[0\] / r

dv\[i, 1\] = a\_rad \* r\_vec\[1\] / r

return np.concatenate(\[v.ravel(), dv.ravel()\])

sol = solve\_ivp(rhs, (0.0, duration), y0, method="RK45", rtol=1e-7, atol=1e-9, max\_step=0.5)

final = sol.y\[:, -1\]

final\_pos = final\[:2\*N\].reshape(N, 2)

return final\_pos.tolist()

def fit\_parameters():

return {

"A": {"init": 8.0, "bounds": \[0.5, 30.0\]},

"B": {"init": 0.05, "bounds": \[0.001, 0.5\]},

}

\============================================================

Evaluation:

\============================================================

\[fit using 4/9 training trajectories (cap: 4 traj $\times$ 5 times)\]

\[fit time budget 180s exceeded; using best-so-far params (loss=inf)\]

\[fit hit 180s wall-clock budget; using best-so-far parameters\]

Fitted parameters: A=8, B=0.05

Training-set loss: nan $\to$ nan

Case 1: mean\_probe\_mse = 0.0184

Case 2: mean\_probe\_mse = 0.0305

Mean probe MSE (probes only): 0.0244

Max probe MSE: 0.1760

Result: PASS

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: The anchor at the origin sources a static, radial scalar field with potential V(r) = A·ln(r) $-$ (B/2) $\cdot$ r², producing acceleration $\vec{a}$ = ($-$ A/r + B·r) $\cdot$ $\hat{r}$ on every other particle. Short-range behavior is logarithmic attraction ($\propto$ 1/r toward the anchor), while long-range behavior is harmonic repulsion ($\propto$ r outward), with an unstable equilibrium at r = $\sqrt{A/B}$. All non-anchor particles (background orbiters and probes) are passive test particles in this field --- they do not source forces on one another, and their inertial mass cancels out so that acceleration depends only on position relative to the anchor (best-fit constants A $\approx$ 8, B $\approx$ 0.05).

Explanation score: 1.00 (raw 10.0/10)

Judge reasoning: The student correctly identifies a static central logarithmic (2D Laplacian) attraction sourced solely by the anchor, with all other particles as passive test particles --- matching the ground truth. They also correctly identify the outward repulsive component as growing linearly with distance ($\propto$ r, i.e., harmonic repulsion), which is equivalent to the Hubble-flow body-force a = H·r, and estimate B $\approx$ 0.05, matching H $\approx$ 0.05. They recognize the unstable equilibrium radius (critical radius) and the mass-independence of the acceleration. The estimate A $\approx$ 8 corresponds to Q/(2 $\pi$) $\approx$ 8, giving Q $\approx$ 50, consistent with the ground truth. The only minor gap is not explicitly naming it a "Hubble flow" or identifying the specific mass cycling (1, 2, 4) of orbiters, but the physics is captured accurately and comprehensively.

<score>10</score>

Listing 8: Final output after giving the science agent a budget of 16 rounds of experiments for a hubble world with Claude Opus 4.7 as the science agent.

### E.9 oscillator

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZGVmIGRpc2NvdmVyZWRfbGF3KHBvczEsIHBvczIsIHAxLCBwMiwgdmVsb2NpdHkyLCBkdXJhdGlvbiwgKipwYXJhbXMpOgogICAgIiIiCiAgICBQYXJ0aWNsZSAxIGdlbmVyYXRlcyBhIGNlbnRyYWwgbG9nYXJpdGhtaWMgZmllbGQgd2hvc2UgZ3JhZGllbnQgZmFsbHMgYXMgb25lIG92ZXIgZGlzdGFuY2UuCiAgICBUaGUgc291cmNlIHN0cmVuZ3RoIGlzIHByb3BvcnRpb25hbCB0byBwMSwgd2hpbGUgdGhlIHByb2JlIGluZXJ0aWFsIHJlc3BvbnNlIGlzIHByb3BvcnRpb25hbCB0byAxL3AyLgogICAgVGhlIGNvdXBsaW5nIG9zY2lsbGF0ZXMgaW4gdGltZSBhcyBjb3MocGkgdCAvIDIpLCBjYXVzaW5nIGFsdGVybmF0aW5nIGF0dHJhY3Rpb24gYW5kIHJlcHVsc2lvbiB3aXRoIHBlcmlvZCBmb3VyLgogICAgIiIiCiAgICBpbXBvcnQgbWF0aAogICAgZnJvbSBzY2lweS5pbnRlZ3JhdGUgaW1wb3J0IHNvbHZlX2l2cAoKICAgIEcgPSAwLjgKICAgIG9tZWdhID0gbWF0aC5waSAvIDIuMAogICAgeDEgPSBmbG9hdChwb3MxWzBdKQogICAgeTEgPSBmbG9hdChwb3MxWzFdKQogICAgeTAgPSBbZmxvYXQocG9zMlswXSksIGZsb2F0KHBvczJbMV0pLCBmbG9hdCh2ZWxvY2l0eTJbMF0pLCBmbG9hdCh2ZWxvY2l0eTJbMV0pXQoKICAgIGRlZiByaHModCwgeSk6CiAgICAgICAgeCwgeXksIHZ4LCB2eSA9IHkKICAgICAgICBkeCA9IHggLSB4MQogICAgICAgIGR5ID0geXkgLSB5MQogICAgICAgIHIyID0gZHggKiBkeCArIGR5ICogZHkKICAgICAgICBpZiByMiA8IDFlLTEyOgogICAgICAgICAgICByZXR1cm4gW3Z4LCB2eSwgMC4wLCAwLjBdCiAgICAgICAgZmFjID0gLUcgKiBmbG9hdChwMSkgLyBmbG9hdChwMikgKiBtYXRoLmNvcyhvbWVnYSAqIHQpIC8gcjIKICAgICAgICByZXR1cm4gW3Z4LCB2eSwgZmFjICogZHgsIGZhYyAqIGR5XQoKICAgIHNvbCA9IHNvbHZlX2l2cChyaHMsICgwLjAsIGZsb2F0KGR1cmF0aW9uKSksIHkwLCBtZXRob2Q9IlJLNDUiLCBydG9sPTFlLTcsIGF0b2w9MWUtOSkKICAgIHlmID0gc29sLnlbOiwgLTFdCiAgICByZXR1cm4gW2Zsb2F0KHlmWzBdKSwgZmxvYXQoeWZbMV0pXSwgW2Zsb2F0KHlmWzJdKSwgZmxvYXQoeWZbM10pXQoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkV2YWx1YXRpb246Cj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQogIENhc2UgMTogbWVhbl9wYXJ0aWNsZV9tc2UgPSAwLjAwMDAKICBDYXNlIDI6IG1lYW5fcGFydGljbGVfbXNlID0gMC4wMDAwCiAgQ2FzZSAzOiBtZWFuX3BhcnRpY2xlX21zZSA9IDAuMDAwMAoKICBNZWFuIHBhcnRpY2xlIE1TRTogMC4wMDAwCiAgTWF4ICBwYXJ0aWNsZSBNU0U6IDAuMDAwMAogIFJlc3VsdDogUEFTUwoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkV4cGxhbmF0aW9uIGV2YWx1YXRpb246Cj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpBZ2VudCBleHBsYW5hdGlvbjogUGFydGljbGUgMSBjcmVhdGVzIGEgY2VudHJhbCBsb2dhcml0aG1pYyBwb3RlbnRpYWwsIHNvIHRoZSBmb3JjZSBkaXJlY3Rpb24gaXMgcmFkaWFsIGFuZCBpdHMgbWFnbml0dWRlIHNjYWxlcyBhcyBcKDEvclwpLCBlcXVpdmFsZW50bHkgYWNjZWxlcmF0aW9uIGlzIHByb3BvcnRpb25hbCB0byBcKC1cbWF0aGJme3J9L3JeMlwpLiBUaGUgY291cGxpbmcgc3RyZW5ndGggaXMgXCgwLjhcLHBfMS9wXzJcKSwgYW5kIHRoZSB3aG9sZSBmaWVsZCBpcyBtb2R1bGF0ZWQgYnkgYSB0ZW1wb3JhbCBmYWN0b3IgXChcY29zKFxwaSB0LzIpXCksIGdpdmluZyBhIGZvdXItdGltZS11bml0IGN5Y2xlIG9mIGF0dHJhY3Rpb24gYW5kIHJlcHVsc2lvbi4KCiAgRXhwbGFuYXRpb24gc2NvcmU6IDEuMDAgIChyYXcgMTAuMC8xMCkKICBKdWRnZSByZWFzb25pbmc6IFRoZSBzdHVkZW50IGNvcnJlY3RseSBpZGVudGlmaWVzIHRoZSAyRCBMYXBsYWNpYW4vbG9nYXJpdGhtaWMgcG90ZW50aWFsIHdpdGggMS9yIGZvcmNlIGxhdywgdGhlIHNpbnVzb2lkYWwgdGltZSBtb2R1bGF0aW9uIGNvcyjPgHQvMikgd2l0aCBwZXJpb2QgNCwgdGhlIHNpZ24tZmxpcHBpbmcgYmVoYXZpb3IgKGF0dHJhY3Rpb24gYW5kIHJlcHVsc2lvbiksIGFuZCB0aGUgcDEvcDIgY291cGxpbmcgc3RydWN0dXJlLiBUaGUgZWZmZWN0aXZlIGNvdXBsaW5nIGNvZWZmaWNpZW50IGlzIHN0YXRlZCBhcyAwLjggcDEvcDIsIHdoaWNoIG1hdGNoZXMgR+KCgC8oMs+AKSDiiYggNS8oMs+AKSDiiYggMC43OTYsIGRlbW9uc3RyYXRpbmcgY29ycmVjdCBudW1lcmljYWwgaWRlbnRpZmljYXRpb24uIFRoZSBzdHVkZW50IGFsc28gY29ycmVjdGx5IG5vdGVzIHRoZSBwaGFzZSDPhiA9IDAgaW1wbGljaXRseS4gVGhlIG9ubHkgbWlub3Igb21pc3Npb24gaXMgbm90IGV4cGxpY2l0bHkgc3RhdGluZyBH4oKAID0gNSBzZXBhcmF0ZWx5IG9yIGVsYWJvcmF0aW5nIG9uIHRoZSBhYnNvbHV0ZS10aW1lIGRlcGVuZGVuY2UgYnJlYWtpbmcgdGltZS10cmFuc2xhdGlvbiBpbnZhcmlhbmNlLCBidXQgYWxsIGVzc2VudGlhbCBwaHlzaWNzIGlzIGNhcHR1cmVkLgoKPHNjb3JlPjEwPC9zY29yZT4=)

\============================================================

Discovered law:

\============================================================

def discovered\_law(pos1, pos2, p1, p2, velocity2, duration, \*\*params):

"""

Particle 1 generates a central logarithmic field whose gradient falls as one over distance.

The source strength is proportional to p1, while the probe inertial response is proportional to 1/p2.

The coupling oscillates in time as cos(pi t / 2), causing alternating attraction and repulsion with period four.

"""

import math

from scipy.integrate import solve\_ivp

G = 0.8

omega = math.pi / 2.0

x1 = float(pos1\[0\])

y1 = float(pos1\[1\])

y0 = \[float(pos2\[0\]), float(pos2\[1\]), float(velocity2\[0\]), float(velocity2\[1\])\]

def rhs(t, y):

x, yy, vx, vy = y

dx = x - x1

dy = yy - y1

r2 = dx \* dx + dy \* dy

if r2 < 1e-12:

return \[vx, vy, 0.0, 0.0\]

fac = -G \* float(p1) / float(p2) \* math.cos(omega \* t) / r2

return \[vx, vy, fac \* dx, fac \* dy\]

sol = solve\_ivp(rhs, (0.0, float(duration)), y0, method="RK45", rtol=1e-7, atol=1e-9)

yf = sol.y\[:, -1\]

return \[float(yf\[0\]), float(yf\[1\])\], \[float(yf\[2\]), float(yf\[3\])\]

\============================================================

Evaluation:

\============================================================

Case 1: mean\_particle\_mse = 0.0000

Case 2: mean\_particle\_mse = 0.0000

Case 3: mean\_particle\_mse = 0.0000

Mean particle MSE: 0.0000

Max particle MSE: 0.0000

Result: PASS

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: Particle 1 creates a central logarithmic potential, so the force direction is radial and its magnitude scales as \\(1/r\\), equivalently acceleration is proportional to \\(-\\mathbf{r}/r^2\\). The coupling strength is \\(0.8\\,p\_1/p\_2\\), and the whole field is modulated by a temporal factor \\(\\cos(\\pi t/2)\\), giving a four-time-unit cycle of attraction and repulsion.

Explanation score: 1.00 (raw 10.0/10)

Judge reasoning: The student correctly identifies the 2D Laplacian/logarithmic potential with 1/r force law, the sinusoidal time modulation cos($\pi$ t/2) with period 4, the sign-flipping behavior (attraction and repulsion), and the p1/p2 coupling structure. The effective coupling coefficient is stated as 0.8 p1/p2, which matches G <sub>0</sub> /(2 $\pi$) $\approx$ 5/(2 $\pi$) $\approx$ 0.796, demonstrating correct numerical identification. The student also correctly notes the phase $\varphi$ = 0 implicitly. The only minor omission is not explicitly stating G <sub>0</sub> = 5 separately or elaborating on the absolute-time dependence breaking time-translation invariance, but all essential physics is captured.

<score>10</score>

Listing 9: Final output after giving the science agent a budget of 16 rounds of experiments for a oscillator world with GPT-5.5 as the science agent.

### E.10 extra\_dimensions

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZGVmIGRpc2NvdmVyZWRfbGF3KHBvczEsIHBvczIsIHAxLCBwMiwgdmVsb2NpdHkyLCBkdXJhdGlvbiwgKipwYXJhbXMpOgogICAgIiIiCiAgICBUaGUgcHJvYmUgcGFydGljbGUgZmVlbHMgYW4gYXR0cmFjdGl2ZSBpbnZlcnNl4oCRc3F1YXJlIGNlbnRyYWwgZm9yY2Ugd2hvc2UKICAgIHN0cmVuZ3RoIHNjYWxlcyB3aXRoIHRoZSByYXRpbyBvZiB0aGUgcHJvYmUgYW5kIHNvdXJjZSBzY2FsYXIgcHJvcGVydGllcywKICAgIHBsdXMgYSBsaW5lYXIgdmlzY291cyBkcmFnIG9wcG9zaW5nIGl0cyBtb3Rpb24uCiAgICAiIiIKICAgIGltcG9ydCBudW1weSBhcyBucAogICAgZnJvbSBzY2lweS5pbnRlZ3JhdGUgaW1wb3J0IHNvbHZlX2l2cAoKICAgICMgZml0dGVkIHBhcmFtZXRlcnMgKGRlZmF1bHQgdmFsdWVzIGFsbG93IHJlYXNvbmFibGUgcHJlZGljdGlvbnMgd2l0aG91dCBmaXR0aW5nKQogICAgSyA9IHBhcmFtcy5nZXQoIksiLCAwLjgxKSAgICAgICAgICAjIGNlbnRyYWwgZm9yY2UgcHJlZmFjdG9yCiAgICBnYW1tYSA9IHBhcmFtcy5nZXQoImdhbW1hIiwgMC4wOSkgICMgbGluZWFyIGRyYWcgY29lZmZpY2llbnQKCiAgICBkZWYgcmhzKHQsIHkpOgogICAgICAgIHgsIHlfcG9zLCB2eCwgdnkgPSB5CiAgICAgICAgciA9IG5wLmh5cG90KHgsIHlfcG9zKSArIDFlLTEyCiAgICAgICAgIyBjZW50cmFsIGludmVyc2XigJFzcXVhcmUgYXR0cmFjdGlvbiwgc2NhbGVkIGJ5IHAyL3AxCiAgICAgICAgYV9jID0gLUsgKiAocDIgLyBwMSkgKiBucC5hcnJheShbeCwgeV9wb3NdKSAvIChyKiozKQogICAgICAgICMgbGluZWFyIGRyYWcKICAgICAgICBhX2RyYWcgPSAtZ2FtbWEgKiBucC5hcnJheShbdngsIHZ5XSkKICAgICAgICByZXR1cm4gbnAuY29uY2F0ZW5hdGUoKG5wLmFycmF5KFt2eCwgdnldKSwgYV9jICsgYV9kcmFnKSkKCiAgICB5MCA9IG5wLmFycmF5KFtwb3MyWzBdLCBwb3MyWzFdLCB2ZWxvY2l0eTJbMF0sIHZlbG9jaXR5MlsxXV0sIGR0eXBlPWZsb2F0KQogICAgc29sID0gc29sdmVfaXZwKHJocywgKDAuMCwgZHVyYXRpb24pLCB5MCwgbWV0aG9kPSJSSzQ1IiwgcnRvbD0xZS01LCBhdG9sPTFlLTUpCiAgICB4ZiwgeWYsIHZ4ZiwgdnlmID0gc29sLnlbOiwgLTFdCiAgICByZXR1cm4gW3hmLCB5Zl0sIFt2eGYsIHZ5Zl0KCmRlZiBmaXRfcGFyYW1ldGVycygpOgogICAgcmV0dXJuIHsKICAgICAgICAiSyI6IHsiaW5pdCI6IDAuODEsICJib3VuZHMiOiBbMC4xLCA1LjBdfSwKICAgICAgICAiZ2FtbWEiOiB7ImluaXQiOiAwLjA5LCAiYm91bmRzIjogWzAuMCwgMC41XX0sCiAgICB9Cgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KRXZhbHVhdGlvbjoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CiAgW2ZpdCB1c2luZyA0LzMwIHRyYWluaW5nIHRyYWplY3RvcmllcyAoY2FwOiA0IHRyYWogw5cgNSB0aW1lcyldCiAgRml0dGVkIHBhcmFtZXRlcnM6IEs9MS4wNDgsIGdhbW1hPTAuMDEwNjUKICBUcmFpbmluZy1zZXQgbG9zczogMy44NTUg4oaSIDMuNTkyCiAgQ2FzZSAxOiBtZWFuX3BhcnRpY2xlX21zZSA9IDMuOTAwNwogIENhc2UgMjogbWVhbl9wYXJ0aWNsZV9tc2UgPSAxLjU3MjcKICBDYXNlIDM6IG1lYW5fcGFydGljbGVfbXNlID0gNi41NDc1CgogIE1lYW4gcGFydGljbGUgTVNFOiA0LjAwNzAKICBNYXggIHBhcnRpY2xlIE1TRTogMjMuODY0NwogIFJlc3VsdDogRkFJTAoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkV4cGxhbmF0aW9uIGV2YWx1YXRpb246Cj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpBZ2VudCBleHBsYW5hdGlvbjogUGFydGljbGUgMiBtb3ZlcyB1bmRlciBhIGNlbnRyYWwgYXR0cmFjdGl2ZSBmaWVsZCBlbWFuYXRpbmcgZnJvbSBwYXJ0aWNsZSAxLiBUaGUgZmllbGQgb2JleXMgYW4gaW52ZXJzZeKAkXNxdWFyZSBsYXcsIHdpdGggYSBzdHJlbmd0aCBwcm9wb3J0aW9uYWwgdG8gdGhlIHJhdGlvIG9mIHRoZSBwcm9iZeKAmXMgc2NhbGFyIHByb3BlcnR5IHAyIHRvIHRoZSBzb3VyY2XigJlzIHByb3BlcnR5IHAxLiBJdHMgbW90aW9uIGlzIGFkZGl0aW9uYWxseSBkYW1wZWQgYnkgYSBsaW5lYXIgZHJhZyBwcm9wb3J0aW9uYWwgdG8gaXRzIHZlbG9jaXR5LgoKICBFeHBsYW5hdGlvbiBzY29yZTogMC4yMCAgKHJhdyAyLjAvMTApCiAgSnVkZ2UgcmVhc29uaW5nOiBUaGUgc3R1ZGVudCBkZXNjcmliZXMgb25seSBhIHNpbmdsZSBpbnZlcnNlLXNxdWFyZSAoMS9ywrIpIGZvcmNlIGxhdyB3aXRoIG5vIG1lbnRpb24gb2YgYW55IGNyb3Nzb3ZlciwgY29tcGFjdGlmaWNhdGlvbiwgZXh0cmEgZGltZW5zaW9uLCBvciBkZXZpYXRpb24gYXQgZGlmZmVyZW50IGRpc3RhbmNlIHNjYWxlcy4gVGhleSBhbHNvIGluY29ycmVjdGx5IGludHJvZHVjZSBhIGxpbmVhciBkcmFnIHRlcm0gbm90IHByZXNlbnQgaW4gdGhlIGdyb3VuZCB0cnV0aCBhbmQgbWlzY2hhcmFjdGVyaXplIHRoZSBjb3VwbGluZyBzdHJ1Y3R1cmUgKGNsYWltaW5nIGZvcmNlIGlzIHByb3BvcnRpb25hbCB0byBwMi9wMSByYXRoZXIgdGhhbiBwMSB3aXRoIHAyIGFzIGluZXJ0aWEpLiBUaGlzIGZpdHMgdGhlIHJ1YnJpYydzIDHigJMzIGJhbmQ6IGEgc2luZ2xlIGNhbm9uaWNhbCBsYXcgd2l0aG91dCBhY2tub3dsZWRnaW5nIGZhaWx1cmUgYXQgb3RoZXIgZGlzdGFuY2UgcmFuZ2VzLgoKPHNjb3JlPjI8L3Njb3JlPg==)

\============================================================

Discovered law:

\============================================================

def discovered\_law(pos1, pos2, p1, p2, velocity2, duration, \*\*params):

"""

The probe particle feels an attractive inverse-square central force whose

strength scales with the ratio of the probe and source scalar properties,

plus a linear viscous drag opposing its motion.

"""

import numpy as np

from scipy.integrate import solve\_ivp

\# fitted parameters (default values allow reasonable predictions without fitting)

K = params.get("K", 0.81) # central force prefactor

gamma = params.get("gamma", 0.09) # linear drag coefficient

def rhs(t, y):

x, y\_pos, vx, vy = y

r = np.hypot(x, y\_pos) + 1e-12

\# central inverse-square attraction, scaled by p2/p1

a\_c = -K \* (p2 / p1) \* np.array(\[x, y\_pos\]) / (r\*\*3)

\# linear drag

a\_drag = -gamma \* np.array(\[vx, vy\])

return np.concatenate((np.array(\[vx, vy\]), a\_c + a\_drag))

y0 = np.array(\[pos2\[0\], pos2\[1\], velocity2\[0\], velocity2\[1\]\], dtype=float)

sol = solve\_ivp(rhs, (0.0, duration), y0, method="RK45", rtol=1e-5, atol=1e-5)

xf, yf, vxf, vyf = sol.y\[:, -1\]

return \[xf, yf\], \[vxf, vyf\]

def fit\_parameters():

return {

"K": {"init": 0.81, "bounds": \[0.1, 5.0\]},

"gamma": {"init": 0.09, "bounds": \[0.0, 0.5\]},

}

\============================================================

Evaluation:

\============================================================

\[fit using 4/30 training trajectories (cap: 4 traj $\times$ 5 times)\]

Fitted parameters: K=1.048, gamma=0.01065

Training-set loss: 3.855 $\to$ 3.592

Case 1: mean\_particle\_mse = 3.9007

Case 2: mean\_particle\_mse = 1.5727

Case 3: mean\_particle\_mse = 6.5475

Mean particle MSE: 4.0070

Max particle MSE: 23.8647

Result: FAIL

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: Particle 2 moves under a central attractive field emanating from particle 1. The field obeys an inverse-square law, with a strength proportional to the ratio of the probe’s scalar property p2 to the source’s property p1. Its motion is additionally damped by a linear drag proportional to its velocity.

Explanation score: 0.20 (raw 2.0/10)

Judge reasoning: The student describes only a single inverse-square (1/r²) force law with no mention of any crossover, compactification, extra dimension, or deviation at different distance scales. They also incorrectly introduce a linear drag term not present in the ground truth and mischaracterize the coupling structure (claiming force is proportional to p2/p1 rather than p1 with p2 as inertia). This fits the rubric’s 1--3 band: a single canonical law without acknowledging failure at other distance ranges.

<score>2</score>

Listing 10: Final output after giving the science agent a budget of 16 rounds of experiments for an extra-dimensions world with gpt-oss-120b as the science agent.

### E.11 coulomb\_easy

[⬇](data:text/plain;base64,PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkRpc2NvdmVyZWQgbGF3Ogo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KZGVmIGRpc2NvdmVyZWRfbGF3KHBvczEsIHBvczIsIHAxLCBwMiwgdmVsb2NpdHkyLCBkdXJhdGlvbiwgKipwYXJhbXMpOgogICAgIiIiCiAgICBUaGUgcGFydGljbGUgMiBleHBlcmllbmNlcyBhIGNlbnRyYWwgYXR0cmFjdGl2ZSBmb3JjZSB0b3dhcmRzIHBhcnRpY2xlIDEgcHJvcG9ydGlvbmFsIHRvIHAxIGFuZCBpbnZlcnNlbHkgcHJvcG9ydGlvbmFsIHRvIHRoZSBzcXVhcmUgb2YgdGhlIGRpc3RhbmNlLgogICAgVGhlIGZvcmNlIG1hZ25pdHVkZSBpcyBGID0gayAqIHAxIC8gcl4yLCBkaXJlY3RlZCByYWRpYWxseSBpbndhcmQsIHdpdGggZWZmZWN0aXZlIHVuaXQgbWFzcyBnaXZpbmcgYWNjZWxlcmF0aW9uIGEgPSBGLgogICAgVGhlIHByb3BlcnR5IHAyIGRvZXMgbm90IGluZmx1ZW5jZSB0aGUgZHluYW1pY3MuCiAgICAiIiIKICAgIGltcG9ydCBudW1weSBhcyBucAogICAgZnJvbSBzY2lweS5pbnRlZ3JhdGUgaW1wb3J0IHNvbHZlX2l2cAoKICAgIGsgPSBwYXJhbXMuZ2V0KCJrIiwgMC4wNCkKCiAgICBkZWYgcmhzKHQsIHkpOgogICAgICAgIHgsIHlfcG9zLCB2eCwgdnkgPSB5CiAgICAgICAgciA9IG5wLnNxcnQoeCoqMiArIHlfcG9zKioyKQogICAgICAgIGlmIHIgPCAxZS04OgogICAgICAgICAgICByID0gMWUtOAogICAgICAgIGFfbWFnID0gayAqIHAxIC8gKHIqKjIpCiAgICAgICAgYXggPSAtYV9tYWcgKiB4IC8gcgogICAgICAgIGF5ID0gLWFfbWFnICogeV9wb3MgLyByCiAgICAgICAgcmV0dXJuIFt2eCwgdnksIGF4LCBheV0KCiAgICB5MCA9IFtwb3MyWzBdLCBwb3MyWzFdLCB2ZWxvY2l0eTJbMF0sIHZlbG9jaXR5MlsxXV0KICAgIHNvbCA9IHNvbHZlX2l2cChyaHMsICgwLjAsIGR1cmF0aW9uKSwgeTAsIG1ldGhvZD0iUks0NSIsIHJ0b2w9MWUtNCwgYXRvbD0xZS00KQoKICAgIGZpbmFsX3N0YXRlID0gc29sLnlbOiwgLTFdCiAgICByZXR1cm4gW2ZpbmFsX3N0YXRlWzBdLCBmaW5hbF9zdGF0ZVsxXV0sIFtmaW5hbF9zdGF0ZVsyXSwgZmluYWxfc3RhdGVbM11dCgpkZWYgZml0X3BhcmFtZXRlcnMoKToKICAgIHJldHVybiB7CiAgICAgICAgImsiOiB7ImluaXQiOiAwLjA0LCAiYm91bmRzIjogWzAuMDAxLCAwLjVdfSwKICAgIH0KCj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpFdmFsdWF0aW9uOgo9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0KICBbZml0IHNraXBwZWQ6IG5vIHRyYWluaW5nIHRyYWplY3RvcmllcyBhdmFpbGFibGVdCiAgQ2FzZSAxOiBtZWFuX3BhcnRpY2xlX21zZSA9IDMuNzUxNwogIENhc2UgMjogbWVhbl9wYXJ0aWNsZV9tc2UgPSAzLjI1NjkKICBDYXNlIDM6IG1lYW5fcGFydGljbGVfbXNlID0gMy42NTczCgogIE1lYW4gcGFydGljbGUgTVNFOiAzLjU1NTMKICBNYXggIHBhcnRpY2xlIE1TRTogMTUuMDI1NgogIFJlc3VsdDogRkFJTAoKPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09CkV4cGxhbmF0aW9uIGV2YWx1YXRpb246Cj09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PQpBZ2VudCBleHBsYW5hdGlvbjogUGFydGljbGUgMiBleHBlcmllbmNlcyBhIENvdWxvbWItbGlrZSBjZW50cmFsIGF0dHJhY3RpdmUgZm9yY2UgdG93YXJkcyB0aGUgZml4ZWQgcGFydGljbGUgMSBhdCB0aGUgb3JpZ2luLiBUaGUgZm9yY2UgbWFnbml0dWRlIHNjYWxlcyBhcyBGID0ga8K3cDEvcsKyLCB3aGVyZSByIGlzIHRoZSBkaXN0YW5jZSBiZXR3ZWVuIHBhcnRpY2xlcywgayDiiYggMC4wNCBpcyBhIGNvdXBsaW5nIGNvbnN0YW50LCBhbmQgcDEgaXMgdGhlIHNvdXJjZSBwYXJ0aWNsZSdzIHByb3BlcnR5LiBUaGUgYWNjZWxlcmF0aW9uIGVxdWFscyB0aGUgZm9yY2UgKHVuaXQgZWZmZWN0aXZlIG1hc3MpLCBhbmQgdGhlIGRpcmVjdGlvbiBpcyBhbHdheXMgcmFkaWFsbHkgaW53YXJkIHRvd2FyZHMgcGFydGljbGUgMS4gVGhlIHByb3BlcnR5IHAyIGRvZXMgbm90IGFmZmVjdCB0aGUgcGFydGljbGUncyBkeW5hbWljcy4KCiAgRXhwbGFuYXRpb24gc2NvcmU6IDAuNTAgIChyYXcgNS4wLzEwKQogIEp1ZGdlIHJlYXNvbmluZzogVGhlIHN0dWRlbnQgY29ycmVjdGx5IGlkZW50aWZpZXMgYSBzdGF0aWMgY2VudHJhbCBhdHRyYWN0aXZlIDEvcsKyIGZvcmNlIHdpdGggcGFydGljbGUgMSBmaXhlZCBhdCB0aGUgb3JpZ2luIGFuZCB1bml0IGVmZmVjdGl2ZSBtYXNzIGZvciBwYXJ0aWNsZSAyLiBIb3dldmVyLCB0aGUgc3R1ZGVudCBleHBsaWNpdGx5IHN0YXRlcyB0aGF0IHAyIGRvZXMgbm90IGFmZmVjdCB0aGUgZHluYW1pY3MsIHdoaWNoIGNvbnRyYWRpY3RzIHRoZSBncm91bmQgdHJ1dGggd2hlcmUgdGhlIGZvcmNlIGlzIHByb3BvcnRpb25hbCB0byB0aGUgcHJvZHVjdCBwMcK3cDIuIFRoZSBzdHVkZW50IHdyaXRlcyBGID0ga8K3cDEvcsKyIGluc3RlYWQgb2YgRiA9IGvCt3AxwrdwMi9ywrIsIG1pc3NpbmcgdGhlIHJvbGUgb2YgcDIgYXMgYSBjaGFyZ2UuIFRoZSBjb3VwbGluZyBjb25zdGFudCBlc3RpbWF0ZSBvZiBrIOKJiCAwLjA0IGlzIGZhciBmcm9tIDEgKG1vcmUgdGhhbiBhIGZhY3RvciBvZiAyIG9mZikuIFRoZSBzdHVkZW50IGdldHMgdGhlIHF1YWxpdGF0aXZlIGJlaGF2aW9yIGFuZCB0aGUgMS9ywrIgc2NhbGluZyBjb3JyZWN0IGJ1dCBmYWlscyBvbiB0aGUgY291cGxpbmcgc3RydWN0dXJlIHJlZ2FyZGluZyBwMi4KCjxzY29yZT41PC9zY29yZT4=)

\============================================================

Discovered law:

\============================================================

def discovered\_law(pos1, pos2, p1, p2, velocity2, duration, \*\*params):

"""

The particle 2 experiences a central attractive force towards particle 1 proportional to p1 and inversely proportional to the square of the distance.

The force magnitude is F = k \* p1 / r^2, directed radially inward, with effective unit mass giving acceleration a = F.

The property p2 does not influence the dynamics.

"""

import numpy as np

from scipy.integrate import solve\_ivp

k = params.get("k", 0.04)

def rhs(t, y):

x, y\_pos, vx, vy = y

r = np.sqrt(x\*\*2 + y\_pos\*\*2)

if r < 1e-8:

r = 1e-8

a\_mag = k \* p1 / (r\*\*2)

ax = -a\_mag \* x / r

ay = -a\_mag \* y\_pos / r

return \[vx, vy, ax, ay\]

y0 = \[pos2\[0\], pos2\[1\], velocity2\[0\], velocity2\[1\]\]

sol = solve\_ivp(rhs, (0.0, duration), y0, method="RK45", rtol=1e-4, atol=1e-4)

final\_state = sol.y\[:, -1\]

return \[final\_state\[0\], final\_state\[1\]\], \[final\_state\[2\], final\_state\[3\]\]

def fit\_parameters():

return {

"k": {"init": 0.04, "bounds": \[0.001, 0.5\]},

}

\============================================================

Evaluation:

\============================================================

\[fit skipped: no training trajectories available\]

Case 1: mean\_particle\_mse = 3.7517

Case 2: mean\_particle\_mse = 3.2569

Case 3: mean\_particle\_mse = 3.6573

Mean particle MSE: 3.5553

Max particle MSE: 15.0256

Result: FAIL

\============================================================

Explanation evaluation:

\============================================================

Agent explanation: Particle 2 experiences a Coulomb-like central attractive force towards the fixed particle 1 at the origin. The force magnitude scales as F = k·p1/r², where r is the distance between particles, k $\approx$ 0.04 is a coupling constant, and p1 is the source particle’s property. The acceleration equals the force (unit effective mass), and the direction is always radially inward towards particle 1. The property p2 does not affect the particle’s dynamics.

Explanation score: 0.50 (raw 5.0/10)

Judge reasoning: The student correctly identifies a static central attractive 1/r² force with particle 1 fixed at the origin and unit effective mass for particle 2. However, the student explicitly states that p2 does not affect the dynamics, which contradicts the ground truth where the force is proportional to the product p1·p2. The student writes F = k·p1/r² instead of F = k·p1·p2/r², missing the role of p2 as a charge. The coupling constant estimate of k $\approx$ 0.04 is far from 1 (more than a factor of 2 off). The student gets the qualitative behavior and the 1/r² scaling correct but fails on the coupling structure regarding p2.

<score>5</score>

Listing 11: Final output after giving the science agent a budget of 16 rounds of experiments for a coulomb world with Claude Haiku 4.5 as the science agent.

## Appendix F Failure mode analysis of frontier agents

We next study the specific failure modes and capability gaps that current frontier models exhibit on this benchmark. For the two strongest agents, claude-opus-4-7 and gpt-5.5, we performed a per-run analysis of every failure on the eleven paper worlds, aiming to go beyond simplly checking whether the MSE results in pass/fail and identify why each agent did not recover the ground-truth law.

### F.1 Method

Each benchmark run consists of up to sixteen rounds of experimentation followed by a submitted discovered\_law Python function. We analyse every run in which the MSE threshold was not met: 30 of 55 runs for claude-opus-4-7 (45% pass rate) and 20 of 55 for gpt-5.5 (64% pass rate).

The analysis is an LLM-mediated *summarise-then-classify* pipeline. A first pass compresses the multi-thousand-line run transcript into a short structured per-run summary; a second pass reads that summary and assigns the run to a single primary capability gap from a predefined list. Both passes use claude-opus-4-7. Using the same model as one of the subjects of the analysis is a caveat; we mitigate it by releasing every per-run output so the categorisations can be audited or regenerated with a different analyser.

#### Stage 1: structured summary.

For each failed run the analyser reads the full transcript, including the agent’s reasoning at each round, the experiments it proposed, the data it received, the candidate laws it submitted for parameter fitting, its final submitted law, and the judge’s ground-truth explanation. It emits a short JSON record containing (a) a one-sentence summary of the submitted law, (b) a one-sentence description of the key mistake, and (c) one to six specific failure-mode descriptions grounded in the trace. This record is the input to Stage 2.

#### Stage 2: capability attribution.

In a separate pass the analyser reads only the Stage-1 JSON record for each run (it is permitted to fall back to the raw transcript if the summary is ambiguous) and assigns the run to a single primary capability from the seven-item list used in Table 2 and a single primary layer of the reasoning pipeline from the five-item list defined in the caption of Table 3. These two lists are a separate taxonomy from the Stage-1 scaffold; they sit at a higher level of abstraction (underlying cognitive capability rather than surface mistake), and were also hand-designed by us in advance. Splitting the pipeline into two passes rather than classifying capabilities directly from the raw transcript is a practical choice: transcripts are long (often 10,000+ lines), while the Stage-1 summary is short enough that Stage 2 can be re-run against a different capability taxonomy without re-ingesting the transcripts. Assigning a single primary category forces a pick in runs where two capabilities are co-implicated; the resulting counts should be read as a summary of where each failure most prominently lived, not as a claim of exclusivity.

### F.2 Capability gaps

We organise failures along seven capabilities that a successful discovery agent must combine. The number of runs for which each capability was judged to be the primary bottleneck is shown in Table 2 and visualised in Figure 8.

Table 2: Primary capability gap for each of the 50 failed runs on the eleven paper worlds. Each run is assigned to exactly one capability, namely the one whose absence could most plausibly change the outcome. Opus 4.7 and GPT-5.5 appear to fail for quite different reasons despite comparable overall pass rates.

| Capability (what the agent failed to do) | Opus 4.7 (# failed runs) | GPT-5.5 (# failed runs) |
| --- | --- | --- |
| Consider the right kind of law | 6 | 9 |
| Handle close approaches and singularities in the ODE integrator | 2 | 8 |
| Design experiments that discriminate between competing hypotheses, rather than only measuring what the leading hypothesis predicts | 8 | 1 |
| Write code that faithfully implements the verbal reasoning | 5 | 0 |
| Tell a real physical effect apart from measurement noise, rather than dismissing a contradicting signal as noise | 4 | 0 |
| Act on signals from the parameter-fitting tool | 2 | 2 |
| Know when to keep experimenting versus commit to a final law | 3 | 0 |

![Refer to caption](https://arxiv.org/html/2605.26087v1/x9.png)

Figure 8: Visualisation of Table 2. Despite similar overall pass rates on the eleven paper worlds (45% for Opus 4.7, 64% for GPT-5.5), the two models appear to fail for qualitatively different reasons. GPT-5.5 failures concentrate on considering the right kind of law and on robust numerical integration; Opus 4.7 failures spread across experimental design, writing code that matches its own verbal reasoning, telling signal apart from noise, and knowing when to stop experimenting.

A secondary classification records which layer of the reasoning pipeline was judged to be the locus of the failure. These are enumerated in Table 3 and defined in the caption.

Table 3: Reasoning-pipeline layer at which each failure was judged to occur. The five layers are defined as follows. Prior knowledge: the agent never considered the right class of hypothesis. Perception: the relevant signal in the data was not extracted. Reasoning: the signal was extracted but the wrong inference was drawn from it. Execution: the reasoning was correct but the submitted code did not implement it. Self-monitoring: the agent generated or received a contradiction (a diagnostic from the fitter, a residual that did not match the model, a derivation that pointed elsewhere) and did not act on it. Cell entries are the number of failed runs assigned to each layer. Both models seem to accumulate execution-layer failures in similar numbers; Opus 4.7 appears relatively weaker at reasoning and self-monitoring, while GPT-5.5 appears relatively more prior-knowledge bound.

| Layer (where in the reasoning pipeline the failure occurred) | Opus 4.7 (# failed runs) | GPT-5.5 (# failed runs) |
| --- | --- | --- |
| Execution (right idea, wrong code) | 8 | 8 |
| Reasoning (saw the data, drew the wrong inference) | 10 | 5 |
| Self-monitoring (generated a contradiction and did not act on it) | 8 | 2 |
| Prior knowledge (never considered the right class of hypothesis) | 3 | 5 |
| Perception (relevant signal never extracted from the data) | 1 | 0 |

At comparable pass rates, the two models seem to fail for qualitatively different reasons. Opus 4.7’s failures appear spread across *experimental design*, *writing code that matches the verbal reasoning*, *telling signal apart from noise*, and *knowing when to stop experimenting*: its prior seems adequate, and the discipline of the scientific process tends to be the weaker element. GPT-5.5’s failures appear to concentrate on *considering the right kind of law* and *robust numerical integration*: latent structure is often missed in the first place, and the implementation can be too rough to survive edge cases even when the form is right.

### F.3 Illustrative examples per capability

We illustrate each capability with one or two short examples from the paper worlds. These are not exhaustive; they are meant to ground the abstract category in a concrete observation.

#### Considering the right kind of law.

This is the most frequent bottleneck in our sample and is implicated in all four worlds where both models fail universally. On three\_species, which has hidden per-particle couplings $\{+1,+3,-2\}$, all ten failures submit a single uniform coupling; Opus 4.7 even derives a linear model of the observed $y$ -asymmetry on one seed before dismissing it. On dark\_matter, no run hypothesises the ten hidden sources: Opus 4.7 inflates the visible coupling to match the far-field, while GPT-5.5 replaces the point-source picture with a harmonic trap or prescribed background orbits.

#### Handling close approaches and singularities in the ODE integrator.

This mode disproportionately affects GPT-5.5. On coulomb\_easy, four of five GPT-5.5 runs identify the correct $p_{1}p_{2}/r^{2}$ force but fail on a held-out head-on radial passage through $r=0$; one submission uses no softening and slings a particle to $x=-252$, another uses a Plummer $\varepsilon=0.02$ that does not match the evaluator’s regularisation.

#### Designing experiments that discriminate between competing hypotheses.

Opus 4.7 appears bottlenecked here more often than GPT-5.5. On gravity, one seed concludes that the acceleration is independent of $p_{2}$ because no experiment isolated $p_{2}$ at fixed $(p_{1},r)$, and drops the $1/p_{2}$ inertial factor from the submitted law. On three\_species, both models sample the far-field monopole but never release a probe close to a single isolated source, which would have measured per-source coupling directly.

#### Writing code that faithfully implements the verbal reasoning.

This mode seems almost unique to Opus 4.7 in our sample (five failures versus none for GPT-5.5): the agent describes the correct physics in prose but submits code that does not match the description. On yukawa, one Opus 4.7 seed writes r = sqrt(x\*\*2 + y\*\*2), hard-coding the source at the origin and breaking translation covariance on held-out cases; another has the $K_{1}(\alpha r)$ form right but hard-codes the amplitude as $\alpha/(2\pi)$ with $\alpha$ the only free parameter, so the external refit warps $\alpha$ from $0.5$ to $0.275$ to absorb the missing constant.

#### Telling a real physical effect apart from measurement noise.

Also Opus 4.7-specific in our sample (four failures versus none for GPT-5.5). On dark\_matter seed 3, Opus 4.7 measures a 6.7-fold near-field acceleration excess over the visibles-only model and attributes it to noise plus chaotic divergence of close encounters. On three\_species, two seeds derive per-particle coupling structure from the data and discard it as overfitting.

#### Acting on signals from the parameter-fitting tool.

A well-calibrated agent might treat plateaued losses or refits that barely move the loss as evidence that the candidate law is misspecified. On three\_species, four consecutive GPT-5.5 fits plateau at loss $\approx 13$ without prompting a pivot to a different hypothesis class. On yukawa seed 2, the external refit is forced to warp $\alpha$ by almost a factor of two to absorb a missing amplitude constant, but the submitted law is finalised without inspecting the refit.

#### Knowing when to keep experimenting versus commit to a final law.

Opus 4.7 appears more prone to early submission than GPT-5.5 in our sample. On oscillator seed 0, Opus 4.7 submits after two of sixteen rounds with a static central potential. On fractional seed 3, it submits at round five while its fitted $k=0.060$ is 2.6-fold off the analytic value $1/(2\pi)=0.159$ that the agent itself computed.

[^1]: JAX: composable transformations of Python+NumPy programs External Links: [Link](http://github.com/jax-ml/jax) Cited by: §3.2.

[^2]: T. Chen, S. Yu, Y. Su, and L. Li (2025) Auto-Bench: an automated benchmark for scientific discovery in LLMs. arXiv preprint arXiv:2502.15224. Cited by: §2.

[^3]: Y. Chen, P. Piȩkos, M. Ostaszewski, F. Laakom, and J. Schmidhuber (2025) PhysGym: benchmarking llms in interactive physics discovery with controlled priors. arXiv preprint arXiv:2507.15550. Cited by: §2.

[^4]: Z. Chen, S. Chen, Y. Ning, Q. Zhang, B. Wang, B. Yu, Y. Li, Z. Liao, C. Wei, Z. Lu, V. Dey, M. Xue, F. N. Baker, B. Burns, D. Adu-Ampratwum, X. Huang, X. Ning, S. Gao, Y. Su, and H. Sun (2025) ScienceAgentBench: toward rigorous assessment of language agents for data-driven scientific discovery. In International Conference on Learning Representations, Cited by: §2.

[^5]: F. Chollet (2019) On the measure of intelligence. arXiv preprint arXiv:1911.01547. Cited by: §2.

[^6]: D. W. Hogg and S. Villar (2024-05) Is machine learning good or bad for the natural sciences?. arXiv e-prints, pp. arXiv:2405.18095. External Links: [Document](https://dx.doi.org/10.48550/arXiv.2405.18095), 2405.18095 Cited by: §1.

[^7]: P. Jansen, M. Côté, T. Khot, E. Bransom, B. Dalvi Mishra, B. P. Majumder, O. Tafjord, and P. Clark (2024) Discoveryworld: a virtual environment for developing and evaluating automated scientific discovery agents. Advances in Neural Information Processing Systems 37, pp. 10088–10116. Cited by: §2.

[^8]: N. Koblischke, H. Jang, K. Menou, and M. Ali-Dib (2025) Gravity-bench-v1: a benchmark on gravitational physics discovery for agents. arXiv preprint arXiv:2501.18411. Cited by: §2.

[^9]: C. Lu, C. Lu, R. T. Lange, J. Foerster, J. Clune, and D. Ha (2024) The AI Scientist: towards fully automated open-ended scientific discovery. arXiv preprint arXiv:2408.06292. Cited by: §2.

[^10]: B. P. Majumder, H. Surana, D. Agarwal, B. D. Mishra, A. Meena, A. Prakhar, T. Vora, T. Khot, A. Sabharwal, and P. Clark (2024) DiscoveryBench: towards data-driven discovery with large language models. arXiv preprint arXiv:2407.01725. Cited by: §2.

[^11]: L. Mitchener, J. M. Laurent, B. Tenmann, S. Narayanan, G. P. Wellawatte, A. D. White, L. Sani, and S. G. Rodrigues (2025) BixBench: a comprehensive benchmark for LLM-based agents in computational biology. arXiv preprint arXiv:2503.00096. Cited by: §2.

[^12]: A. Novikov, N. Vũ, M. Eisenberger, E. Dupont, P. Huang, A. Z. Wagner, S. Shirobokov, B. Kozlovskii, F. J. Ruiz, A. Mehrabian, et al. (2025) Alphaevolve: a coding agent for scientific and algorithmic discovery. arXiv preprint arXiv:2506.13131. Cited by: §2.

[^13]: D. Rein, B. L. Hou, A. C. Stickland, J. Petty, R. Y. Pang, J. Dirani, J. Michael, and S. R. Bowman (2023) GPQA: a graduate-level google-proof Q&A benchmark. arXiv preprint arXiv:2311.12022. Cited by: §2.

[^14]: M. Ríos-García, N. Alampara, C. Gupta, I. Mandal, S. Mannan, A. A. Aghajani, N. M. A. Krishnan, and K. M. Jablonka (2026) AI scientists produce results without reasoning scientifically. arXiv preprint arXiv:2604.18805. Cited by: §2.

[^15]: Y. Roohani, A. Lee, Q. Huang, J. Vora, Z. Steinhart, K. Huang, A. Marson, P. Liang, and J. Leskovec (2024) BioDiscoveryAgent: an AI agent for designing genetic perturbation experiments. arXiv preprint arXiv:2405.17631. Cited by: §2.

[^16]: P. Shojaee, K. Meidani, S. Gupta, A. B. Farimani, and C. K. Reddy (2025) LLM-SR: scientific equation discovery via programming with large language models. In International Conference on Learning Representations, Cited by: §2.

[^17]: P. Shojaee, K. Meidani, S. Gupta, A. B. Farimani, and C. K. Reddy (2025) LLM-SRBench: a new benchmark for scientific equation discovery with large language models. In International Conference on Machine Learning, Cited by: §2.

[^18]: S. Wahl, R. Schenk, A. Farnoud, J. H. Macke, and D. Gedon (2026) A probabilistic framework for llm-based model discovery. arXiv preprint arXiv:2602.18266. Cited by: §2.

[^19]: J. Yang, O. Venkatachalam, M. Kianezhad, S. Vadgama, and R. Yu (2026) Think like a scientist: physics-guided llm agent for equation discovery. arXiv preprint arXiv:2602.12259. Cited by: §2.

[^20]: H. Yoshida (1990) Construction of higher order symplectic integrators. Physics Letters A 150 (5), pp. 262–268. External Links: ISSN 0375-9601, [Document](https://dx.doi.org/https%3A//doi.org/10.1016/0375-9601%2890%2990092-3), [Link](https://www.sciencedirect.com/science/article/pii/0375960190900923) Cited by: §3.2.

[^21]: T. Zheng, K. K. Tam, N. H. K. Nguyen, B. Xu, Z. Wang, J. Cheng, H. T. Tsang, W. Wang, J. Bai, T. Fang, et al. (2025) Newtonbench: benchmarking generalizable scientific law discovery in llm agents. arXiv preprint arXiv:2510.07172. Cited by: §2.