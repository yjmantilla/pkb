---
title: "There Will Be a Scientific Theory of Deep Learning"
source: "https://arxiv.org/html/2604.21691v1"
author:

created: 2026-04-28
description:
tags:
  - "clippings"
  - "citable"
  - "methods"
  - "reverse-engineering"
---

There Will Be a Scientific Theory of Deep Learning 
Jamie Simon, Daniel Kunin, Alexander Atanasov, Enric Boix-Adserà, Blake Bordelon, Jeremy Cohen, Nikhil Ghosh, Florentin Guth, Arthur Jacot, Mason Kamb, Dhruva Karkada, Eric J. Michaud, Berkan Ottlik, Joseph Turnbull 
UC Berkeley, Harvard University, University of Pennsylvanie, Flatiron Institute, New York Univerity, Stanford University, Asteria Institute, 
Imbue 2026 
https://arxiv.org/abs/2604.21691 
http://youtube.com/watch?v=gT07OoBOPNo 
https://learningmechanics.pub/
"Deep learning is going to have a scientific theory. We can see the pieces starting to come together, and it's looking a lot like physics!
We're releasing a paper pulling together these emerging threads and giving them a name: learning mechanics." 
https://x.com/learning.../status/2047723849874330047/photo/1
For years, deep learning has often felt like modern alchemy: researchers build larger neural networks, tune countless parameters, and discover that some combinations mysteriously work better than others. Models become more powerful, but the deeper question remains: :why do they work so well?"
This paper argues that we are finally moving toward something much more rigorous—a true scientific theory of deep learning.
The authors suggest that deep learning is beginning to develop its own kind of “physics,” a framework that can explain how neural networks learn, why certain training strategies succeed, and what hidden patterns emerge inside these systems. Rather than treating AI progress as trial and error, they believe we are approaching a stage where learning itself can be understood through laws, predictions, and testable principles.
They call this emerging framework “learning mechanics.”
The idea is not to explain every neuron individually, but to understand the larger forces shaping the system—much like physics explains gases without tracking every molecule. Instead of focusing only on microscopic details, learning mechanics studies broad patterns: how training evolves over time, how internal representations form, how final model behavior depends on scale, and why performance improves in predictable ways.
The paper highlights five major research directions driving this theory forward.
First, researchers study simplified toy models where the math can be solved exactly, helping reveal intuition about what happens in far larger real-world systems. 
Second, they examine special limits—such as infinitely wide neural networks—that make otherwise impossible problems mathematically tractable. 
Third, they identify surprisingly simple mathematical laws, like scaling laws, that describe how performance improves as data, compute, and model size grow. 
Fourth, they develop theories of hyperparameters—things like learning rates and batch sizes—to separate the role of tuning from the deeper learning process itself. 
Finally, they search for universal behaviors that appear across many different architectures and datasets, helping identify what is fundamental rather than accidental.
Together, these approaches point toward something powerful: deep learning may not be a collection of hacks after all, but a system governed by discoverable principles.
The authors also respond to skeptics who argue that neural networks are too messy, too complex, or too engineered for a true theory to exist. They disagree. Complexity, they argue, did not stop physics from building thermodynamics or statistical mechanics. In the same way, AI may not need a perfect microscopic explanation before useful scientific laws emerge.
They also see strong connections between this “learning mechanics” perspective and mechanistic interpretability—the effort to reverse-engineer what models are doing internally. One studies the large-scale laws of learning; the other studies the fine-grained circuits inside the machine. Together, they may form a much deeper understanding of intelligence itself.
The larger message is optimistic: deep learning is entering a new phase. Instead of merely building bigger models, researchers are beginning to uncover the rules that govern them.
If this succeeds, future AI development may look less like guesswork and more like engineering grounded in first principles.
Not just bigger models—but understandable ones. 
#artificialintelligence #DeepLearning #NeuralNetworks 
#learningmechanics #MechanisticInterpretability #FoundationModels #physics

Jamie Simon <sup>∗</sup>  
UC Berkeley and Imbue Daniel Kunin  
UC Berkeley Alexander Atanasov  
Harvard University Enric Boix-Adserà  
University of Pennsylvania Blake Bordelon  
Harvard University Jeremy Cohen  
Flatiron Institute Nikhil Ghosh  
Flatiron Institute Florentin Guth  
NYU & Flatiron Institute Arthur Jacot  
New York University Mason Kamb  
Stanford University Dhruva Karkada  
UC Berkeley Eric J. Michaud  
Astera Institute Berkan Ottlik  
University of Pennsylvania Joseph Turnbull  
UC Berkeley

###### Abstract

In this paper, we make the case that a scientific theory of deep learning is emerging. By this we mean a theory which characterizes important properties and statistics of the training process, hidden representations, final weights, and performance of neural networks. We pull together major strands of ongoing research in deep learning theory and identify five growing bodies of work that point toward such a theory:

1. solvable idealized settings that provide intuition for learning dynamics in realistic systems;
2. tractable limits that reveal insights into fundamental learning phenomena;
3. simple mathematical laws that capture important macroscopic observables;
4. theories of hyperparameters that disentangle them from the rest of the training process, leaving simpler systems behind; and
5. universal behaviors shared across systems and settings which clarify which phenomena call for explanation.

Taken together, these bodies of work share certain broad traits: they are concerned with the dynamics of the training process; they primarily seek to describe coarse aggregate statistics; and they emphasize falsifiable quantitative predictions. We argue that the emerging theory is best thought of as a mechanics of the learning process, and suggest the name learning mechanics. We assert that learning mechanics should be a mathematical theory, grounded in first-principles calculations that closely predict empirics, reliant on well-tested approximations and assumptions, aiming for broad impact across the machine learning stack once it reaches maturity.

We discuss the relationship between this mechanics perspective and other approaches for building a theory of deep learning, including the statistical and information-theoretic perspectives. In particular, we anticipate a symbiotic and mutually supportive relationship between learning mechanics and the developing discipline of mechanistic interpretability. Where mechanistic interpretability aims to be the biology of deep learning, learning mechanics should aspire to be its physics, mirroring the complementary relationship between biology and physics in the natural sciences.

We also review and address common arguments that fundamental theory will not be possible or is not important. We conclude with a portrait of important open directions in learning mechanics and advice for beginners. We host further introductory materials, perspectives, and open questions at [learningmechanics.pub](https://arxiv.org/html/2604.21691v1/learningmechanics.pub).

<sup>†</sup>

## 1 Introduction

Deep learning is famously a black-box learning method, the most powerful, most inscrutable, and now most technologically important member of the machine learning pantheon. Properly trained, neural networks learn to perform a wide array of tasks with superhuman performance, but we have no unified scientific framework that explains why or how. Motivated by both scientific curiosity and the promise of practical engineering benefit, the effort to put rigorous mathematical and scientific backing behind this applied discipline has spanned decades. Despite some progress, however, our understanding remains primitive: neural networks are still trained using methods discovered largely through trial and error rather than first principles, and theory plays little role in the day-to-day practice of deep learning. The challenge has only compounded as practice has advanced, and in the era of large language models and diffusion models, the mysteries are arguably deeper than they were one or two decades ago. Will we ever understand?

<svg id="S1.p2.pic1" height="3663.92" overflow="visible" version="1.1" viewBox="0 0 600 3663.92" width="600"><g style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,3663.92) matrix(1 0 0 -1 0 0)" fill="#000000" stroke="#000000" stroke-width="0.4pt"><g style="--ltx-fill-color:#000000;" fill="#000000" fill-opacity="1.0"><path style="stroke:none" d="M 0 0 L 0 3663.92 L 600 3663.92 L 600 0 Z"></path></g><g style="--ltx-fill-color:#FFFFFF;" fill="#FFFFFF" fill-opacity="1.0"><path style="stroke:none" d="M 1.97 1.97 L 1.97 3661.95 L 598.03 3661.95 L 598.03 1.97 Z"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 3650.14)"><foreignObject style="--ltx-fg-color:#000000;--ltx-fo-width:40.23em;--ltx-fo-height:0em;--ltx-fo-depth:262.8em;" width="556.69" height="3636.36" transform="matrix(1 0 0 -1 0 0)" overflow="visible" color="#000000"><span id="S1.p2.pic1.1.1.1.1.1" style="width:34.98em;"><span id="S1.p2.pic1.1.1.1.1.1.1"><span id="S1.p2.pic1.1.1.1.1.1.1.1">This paper makes the case that, yes, there will be a scientific theory of deep learning; that we can see pieces of this theory starting to emerge; and that this theory will take the form of a <span id="S1.p2.pic1.1.1.1.1.1.1.1.1">mechanics</span> of the learning process.</span></span></span></foreignObject></g></g></svg>

The questions driving deep learning theory have changed over time, and to understand where the field is going, it is useful to first look back at how we got here. Deep learning theory is as old as machine learning itself, with roots in the McCulloch–Pitts neuron and the perceptron in the middle of the last century. The earliest theoretical questions in machine learning were about expressivity: what functions can simple models represent, and how can they be learned from data? As learning came to be understood as a statistical problem, and simple learning systems found practical success, the theoretical focus shifted to ask: when does learning from finite samples generalize? This gave rise to classical learning theory, including statistical and computational/PAC learning theory. Paired with classical optimization theory, these frameworks gave clean end-to-end guarantees of the optimization and generalization of simple learning systems. In parallel, a classical tradition of the statistical physics of machine learning developed satisfying theories of the average-case behavior of simple models.

While these classical theories built a strong foundation for understanding learning, the rise of deep learning through multilayer networks, backpropagation, and increasing scale in both data and compute exposed limitations in their explanatory power. Neural networks are complex, nonconvex, and overparameterized (in contrast with the simple, convex, parsimonious models for which classical learning theory excels), and they optimize and generalize better than these classical approaches can guarantee or explain. Furthermore, it became clear that neural networks were not merely fitting data or achieving low training error, they were learning structured internal representations and displaying striking regularities across tasks and scales. The classical questions of performance and efficiency remained important, but answering them would first require understanding a new host of phenomena shaped both by the dynamics of neural networks through training and the structure of the data they are trained on.

This marked a transition in which deep learning theory changed in character from a largely mathematical study of what is possible to a truly scientific effort to describe, explain, and ultimately predict the behavior of complex empirical systems. New scientific endeavors often start with an empirical tension in which nature presents something interesting we cannot predict or explain with existing tools, and although neural networks are artificial computational systems, this same scientific tension is present here. We should thus approach this task as scientists, embracing empirics, seeking unifying principles, and identifying recurring motifs. We should also expect the path forward to look more like the development of a scientific field than the development of a mathematical one.

The purpose of this paper is to convince the reader that this scientific tension is gradually giving way to a scientific theory which resolves it. In Section 2, we pull together major strands of ongoing research and identify five lines of evidence that such a theory is emerging:

These lines of research broadly share several overarching characteristics: they are concerned with the dynamics of the training process; they primarily seek to describe coarse aggregate statistics of learning; and they emphasize accurate average-case predictions over rigorous worst-case bounds. In this sense, the emerging scientific theory of neural networks appears to have much in common with theories in physics such as classical mechanics, continuum mechanics, statistical mechanics, and quantum mechanics. We argue that this emerging theory is best understood as a mechanics of learning.

### 1.1 What’s in a mechanics?

Mechanics is the branch of physics studying how forces acting on objects determine their movement through space and time. Neural network learning can be thought of in this way: much as an object moves continuously through physical space, learning involves a model moving through parameter space via discrete updates. In the physical sciences, forces come from interactions between components of a system. Similarly, the process of deep learning is shaped by interactions between the parameters, dataset, task, and learning rule. In physics, these forces are mediated by fields; in deep learning, they are mediated by gradients. In physics, systems settle into equilibria at local minima of a potential determined by internal interactions and external constraints; analogously, neural networks converge to local minima of a loss landscape shaped by their architecture and training data. While the systems under study are very different, since the key problems of both are essentially about movement and interaction, we might expect some features of the resulting sciences to be shared.

These analogies are not just speculation: we can see these similarities reflected in the lines of research listed above. All branches of mechanics (and especially classical mechanics) develop a library of analytically solvable settings to gain intuition; so too does learning mechanics. All branches of mechanics use limits as simplifying tools; so too does learning mechanics. Continuum and statistical mechanics, the branches which most directly deal with large numbers of interacting components, describe zoomed-out summary statistics rather than the motion of every particle; this has also proven a useful approach in dealing with the complexity of deep learning. Every physical system has one or more system parameters (characteristic scales, coupling constants, etc.) affecting its behavior, and some techniques for treating these are essentially the same as those used to study hyperparameters in deep learning. Finally, physics is full of cases in which the same phenomena show up in very different settings, and similarly we see universal behavior emerging across deep learning systems.

All considered, the emerging science shares deep similarities with established branches of mechanics. By analogy to classical, continuum, statistical, and quantum mechanics, we suggest the intended theory be called learning mechanics.

#### Seven desiderata for learning mechanics.

We should be clear at the outset what we want from a mechanics of learning. Assessing how mature branches of mechanics were motivated, developed, and succeeded, we can see what sort of goals to aim for. Here are seven desiderata for this research program:

1. Learning mechanics should be fundamental, proceeding logically from a first-principles description of neural network training. Interim assumptions about network weights, dynamics, and performance will be useful tools, but they should ultimately be explained from first principles.
2. Learning mechanics should be mathematical, making unambiguous quantitative statements about important properties of neural networks. No mechanics is a qualitative science; neither will be learning mechanics.
3. Learning mechanics should be predictive, making claims supported by simple, repeatable empirical measurements. We have excellent experimental control of our system, and every major development should be unambiguously verified in experiment.
4. Learning mechanics should be comprehensive, describing aspects of neural networks’ training process, hidden representations, and final weights in a single picture. It is worth emphasizing that this theory will not — and should not — aim to describe everything. A map at the full resolution of the world would be the size of the world and thus of little use. What we seek instead is a theory that operates at the right level of resolution — one that sacrifices detail in favor of insight.
5. Learning mechanics should be intuitive, being simple, illuminating, and satisfying in its demystification of deep learning. Like physics, learning mechanics should strive for simple insight over technical complexity.
6. Learning mechanics should be *useful*, serving as the scientific foundation for applied deep learning as physics does for other forms of engineering. Concrete goals should include greatly reducing the need for hyperparameter tuning, giving predictive tools for dataset design, and providing rigorous foundations for AI safety work.
7. Finally, learning mechanics should be humble, being solid in what it describes and explicit about what it cannot. Every branch of physical science has a regime of applicability outside of which it breaks down, and these boundaries are taught together with the science so that it may be used reliably. We anticipate the mechanics of learning applicable to realistic deep learning will break down in many small-scale, handcrafted, or otherwise special cases, and this is the price we will pay for the right simple picture in the regimes we care about.

A mechanics of learning with these virtues — one that is fundamental, mathematical, predictive, comprehensive, intuitive, useful, and humble — would be transformative, paradigm-setting. We expect such a theory would resolve important open questions that have long remained out of reach, as we discuss in Section 5.

### 1.2 Why learning mechanics matters

Building learning mechanics will not be easy. It will require sustained effort, both intellectual and institutional. It is therefore worth being clear about why such a project matters. The reasons to seek a mechanics of learning fall into three broad categories: scientific, practical, and safety-related.

The scientific reasons concern what such a theory could teach us about intelligence and the natural world. The striking engineering success of large neural networks suggests that they exploit deep principles of learning and representation that we do not yet understand. This has historical precedent: technology has often preceded scientific theory, as was the case with steam engines’ role in motivating thermodynamics, which went on to explain much more than engine efficiency. A similar story played out in flight: the development of airplanes through trial and error and inspiration from the natural world helped motivate aerodynamic theory, which in turn enabled both better aircraft design and a deeper understanding of how birds themselves fly. In our case, the principles that govern learning in artificial neural networks may also shed light on our own biological intelligence, with potentially important implications for neuroscience and cognitive science.

The practical reasons concern the design and development of real-world AI systems. A mature theory of deep learning could guide model design, optimization, scaling, and deployment, replacing trial and error with more reliable principles. Theory has already begun to play this role in a limited but growing number of cases, including empirical scaling laws (Section 2.3), mathematical prescriptions for hyperparameter scaling (Section 2.4), and theoretically-motivated optimizers and methods for data attribution (Section 4). A deeper, more complete theory will give more such guidance and make it sharper and more predictive.

The safety reasons concern our ability to describe, characterize, and govern increasingly powerful AI systems. Some form of regulation will likely be necessary, but it is difficult to regulate a technology that we cannot clearly describe. A theory that identifies the relevant variables, mechanisms, and organizing principles of large models could help provide the clarity needed for reliability, oversight, and control. One avenue by which fundamental theory might aid in AI safety is by supporting mechanistic interpretability, a point to which we return in Section 3.

### 1.3 Plan for this paper

This paper is structured as follows. In Section 2, we present our five lines of evidence that a scientific theory of deep learning is beginning to emerge. We motivate each line of evidence with an intuitive explanation and highlight examples of research successes that illustrate the underlying principle. In Section 3, we discuss the relationship between learning mechanics and other perspectives on the science of deep learning, including a possible symbiotic relationship between learning mechanics and mechanistic interpretability. In Section 4, we review and address common arguments that fundamental theory will not be possible. In Section 5, we give a portrait of ten important open directions in learning mechanics, from predicting scaling laws to eliminating hyperparameters, where we expect to see major progress in the coming years. Finally, in Section 6, we offer some advice for young researchers looking to get involved in this scientific project and extend a hand with some introductory resources.

We write this paper for a broad audience. We hope the veteran scientist of deep learning will find something valuable in our synthesis of useful approaches and results, and feel galvanized by our depiction of an emerging science. We hope to convince the deep learning practitioner that theory is on a path to fulfilling its longstanding promise of practical utility and to encourage them to experiment with their systems with an eye for science. We hope to convince the AI safety or mechanistic interpretability researcher that white-box theory is difficult yet possible — that a first-principles study of dynamics can help put solid foundations beneath their important work, and that our communities should work together (see Section 3 for our vision of symbiosis). Lastly, we hope to make it easier for young students and newcomers to the field to get involved. This is an exciting and important area of work, and while it requires some mathematical maturity to get started, it is our belief that the barrier to entry could be much lower. Various deep intuitions about this science have been percolating inside the theory community for a while, and this paper is an attempt to state them clearly. We hope to make it easier for folks with the requisite background to quickly get up to speed and contribute.

## 2 Evidence of an emerging mechanics of learning

A great cause for optimism that a mechanics of learning is possible is the fact that the essential ingredients of deep learning are both explicit and measurable. A deep learning system is characterized by the following components:

$$
\displaystyle\text$a neural network $f($\bm$x$$;$\bm$\theta$$)\text$ specified as a composition of simple linear and nonlinear transformations.$
$$
 
$$
\displaystyle\text$a dataset $\mathcal$D$=\$($\bm$x$$_$i$,$\bm$y$$_$i$)\$_$i=1$^$n$\text$ consisting of samples from an unknown data-generating distribution $
$$
 
$$
\displaystyle($\bm$x$$,$\bm$y$$)\sim\mathcal$P$_$\mathrm$data$$.
$$
$$
\displaystyle\text$an objective $\mathcal$L$($\bm$\theta$$)\text$ measuring the performance of the network $f($\bm$x$$;$\bm$\theta$$)\text$ on the dataset $\mathcal$D$.
$$
$$
\displaystyle\text$a gradient-based update equation, e.g. $$\bm$\theta$$^$(t+1)$=$\bm$\theta$$^$(t)$-\eta\nabla\mathcal$L$($\bm$\theta$$^$(t)$)\text$, together with a parameter$
$$
 
$$
\displaystyle\text$initialization, e.g. $$\bm$\theta$$^$(0)$_$i$\sim\mathcal$N$(0,\alpha^$2$_$\mathrm$init$$)\text$ and optimization hyperparameters, e.g. learning rate $\eta\text$.$
$$

Nothing about the learning process is hidden. Unlike many complex systems where the equations governing dynamics must be inferred from observations, deep learning directly exposes its “equations of motion.” Moreover, these dynamics are extraordinarily measurable: every weight, activation, gradient, and loss value can be recorded, along with arbitrary statistics derived from them. As a result, deep learning experiments are unusually easy to design, replicate, and interrogate, making it more straightforward to discover empirical regularities and rigorously test theoretical predictions. Few fast-moving scientific domains offer comparable transparency in their governing equations or comparable freedom in what can be measured.

What, then, stands in the way of a scientific theory of deep learning? The central challenge is not *opacity*, but *complexity*. While we have direct access to the architecture, data, task, and learning rule, the interaction of these components gives rise to learning dynamics that are nonlinear, coupled, and high-dimensional. These dynamics depend in subtle ways on the choice of hyperparameters. And even though we can inspect every training sample, data distributions are complex and have defied simple characterization.

Nevertheless, we argue that this complexity conceals underlying regularities, and that deep learning will indeed admit a scientific theory. In what follows, we present five broad observations that serve as evidence for an emerging mechanics of learning. Each of these admits direct analogies to tools and ideas in other disciplines of mechanics. These are summarized in Table 1.

| Section | Approach | Examples in deep learning | Examples from physics |
| --- | --- | --- | --- |
| 2.1 | solvable settings | deep linear networks, kernel regression, multi-index models | harmonic oscillator, hydrogen atom, Ising model |
| 2.2 | simplifying limits | lazy vs. rich learning, width, depth $\rightarrow\infty$, small initialization | thermodynamic limit $(n,V\rightarrow\infty)$, classical limit $(\hbar\rightarrow 0)$, hydrodynamic limit $($\bm$k$$,\omega\rightarrow 0)$ |
| 2.3 | simple empirical laws | neural scaling laws edge of stability neural feature ansatz | the laws of Kepler, Snell, Boyle, Hooke, Newton, Faraday, Ohm, Poiseuille, Planck, Hubble, etc. |
| 2.4 | study of system parameters | step size as sharpness regularization, $\mu$ P and width-scaling | scaling analysis, nondimensionalization, chaotic vs. ordered regimes |
| 2.5 | universal phenomena | common inductive biases and representations across models | critical phenomena, renormalization group flow |

Table 1: Useful tools and ideas in the emerging science of deep learning closely resemble important tools and ideas from physics, particularly classical mechanics, continuum mechanics, statistical mechanics, and quantum mechanics. Extrapolating, this suggests that there will be a mechanics of learning which offers a unifying first-principles theory of the training process, hidden representations, final weights, and test-time performance of neural networks.

### 2.1 Analytically solvable settings exist

A reliable way to build scientific understanding in complex systems is to study pared-down yet representative settings in which quantitative calculations are possible. For example, physics uses representative solvable settings like the harmonic oscillator and the hydrogen atom as sources of intuition for much broader classes of system. Deep learning appears to be particularly amenable to this approach: scientists have identified a rich landscape of minimal models where the learning dynamics simplify and many quantities of interest become solvable. These analytically tractable cornerstones are useful because they reveal phenomena and mechanisms to look for when we turn to realistic deep learning.<sup>1</sup>

One particularly fruitful simplification is linearization. Here we discuss two distinct instantiations of this idea: linearization in the data, where $f($\bm$x$$;$\bm$\theta$$)$ becomes linear in $$\bm$x$$$, and linearization in the parameters, where $f($\bm$x$$;$\bm$\theta$$)$ becomes linear in $$\bm$\theta$$$.

![Refer to caption](https://arxiv.org/html/2604.21691v1/x1.png)

(a) Linearization in the data

#### Linearization in the data.

A *deep linear network* is obtained by removing all nonlinearities from a neural network’s architecture, yielding a model that is linear in its inputs $$\bm$x$$$ but remains highly *nonlinear* in its parameters $$\bm$\theta$$$:

$$
f($\bm$x$$;$\bm$\theta$$)=\mathbf$W$_$L$\mathbf$W$_$L-1$\cdots\mathbf$W$_$1$$\bm$x$$,\qquad\text$where $$\bm$\theta$$:=\$\mathbf$W$_$\ell$\$_$\ell=1$^$L$\text$, each $\mathbf$W$_$\ell$\text$ is a linear transformation, and $L\geq 2.
$$

Deep linear networks have a long history of study because, despite their simplicity, they retain many hallmark behaviors of deep learning [^199]. These include saddle-point-dominated loss landscapes [^16], dynamics with sharp phase transitions and separation of timescales [^97] [^9], edge-of-stability oscillations with gradient descent [^86], and strong initialization-dependent inductive biases [^275] [^146]. Analysis of these networks is typically carried out with the *gradient flow* learning rule — the continuous-time limit of gradient descent — under simplifying assumptions on the data distribution and with carefully chosen initializations [^88] [^240] [^262] [^78]. In these regimes, the learning dynamics can often be solved exactly or reduced to low-dimensional dynamical systems.

Across many such analyses, a consistent lesson emerges: learning exhibits a *greedy* low-rank bias, acquiring some components of the task before others. Canonical work by [^240] first showed how deep linear networks learn singular vectors of the input–output correlation sequentially during training, with learning prioritized toward modes associated with the largest singular values, as shown in Figure 1. This bias has been hypothesized to benefit generalization by separating the signal from the noise [^148], and closely mirrors behavior observed in nonlinear networks, where simpler functions are often learned before more complex ones [^134] [^254]. Moreover, a range of factors — including small initializations [^96] [^163] [^124] [^215], increased depth [^105] [^6] [^7], stronger mini-batch noise [^216] [^55], and explicit $\ell_$2$$ regularization [^295] [^268] — have all been shown to further strengthen this greedy learning bias.

#### Linearization in the parameters.

A linearized network is obtained by truncating the nonlinear terms in a network’s Taylor expansion around its initial parameters. This yields a model that is linear in its parameters $$\bm$\theta$$$ but remains highly *nonlinear* in the data $$\bm$x$$$:

$$
f_$\text$lin$$($\bm$x$$;$\bm$\theta$$)=f($\bm$x$$;$\bm$\theta$$_$0$)+\nabla_$$\bm$\theta$$$f($\bm$x$$;$\bm$\theta$$_$0$)^$\top$($\bm$\theta$$-$\bm$\theta$$_$0$),\qquad\text$where $\nabla_$$\bm$\theta$$$f(\cdot;$\bm$\theta$$_$0$)\text$ is the gradient at initialization.$
$$

This is not some contrived construction: in fact, there are settings in which a model is well-approximated throughout training by its linearization, i.e., $\forall t,\ f($\bm$x$$;$\bm$\theta$$_$t$)\approx f_$\text$lin$$($\bm$x$$;$\bm$\theta$$_$t$)$. For example, any neural network architecture can be driven into the linearized regime by taking suitable limits [^123] [^153] [^58] [^167], as discussed in Section 2.2. Additionally, recent evidence suggests that language model fine-tuning occurs in a near-linearized regime [^177] [^229].

Since a linearized network is linear in its parameters, its learning dynamics are identical to those of linear regression, with one key difference: while the dynamics of linear regression are driven by the Gram kernel, $K_$\text$Gram$$($\bm$x$$,$\bm$x$$^$\prime$)=$\bm$x$$^$\top$$\bm$x$$^$\prime$$, linearized networks are described by the neural tangent kernel (NTK), $K_$\text$NTK$$($\bm$x$$,$\bm$x$$^$\prime$)\coloneqq\nabla_$$\bm$\theta$$$f($\bm$x$$;$\bm$\theta$$_$0$)^$\top$\nabla_$$\bm$\theta$$$f($\bm$x$$^$\prime$;$\bm$\theta$$_$0$)$. When the task is least squares regression and training uses small-step gradient descent, the dynamics are analytically tractable and the final predictor is given by kernel ridge regression with the NTK [^123].

This setting yields insight into a variety of deep learning phenomena. For example, since the details of the network architecture influence the mathematical structure of the NTK through the fixed feature map $\nabla_$$\bm$\theta$$$f(\cdot;$\bm$\theta$$_$0$)$, one learns how the linearized model’s inductive bias follows from its architecture [^8] [^91]. Furthermore, one may accurately predict the model’s expected generalization error on arbitrary targets $f^$\star$$ by accounting for the structure of the input data [^125] [^49] [^170] [^112] [^270] [^253], as shown in Figure 1. Applying this framework to realistic data distributions uncovers the origin of the typical models’ tendency to learn simple and generalizing functions [^25] [^138]. Linearized models also capture relevant phenomena such as double descent [^27] [^2] and scaling laws [^50] [^218] [^68] [^11].

However, despite these theoretical merits, linearized networks are unrealistic in a few critical ways. Most notably, they do not capture the strong feature-learning capabilities that generic neural networks exhibit, often leading to overly pessimistic predictions for sample complexity [^94] [^266]. Moreover, by reducing training to a tractable linear problem, these models sidestep the intrinsically nonconvex optimization phenomena of deep learning. To describe these and other aspects of deep learning, one must look beyond linearization.

#### Beyond linearization.

An important frontier for theory lies in developing analytically tractable toy models that remain genuinely nonlinear in both the data and the parameters (see 1). In these settings, the influence of the data distribution becomes more complex, making it difficult to obtain a unified and general framework. However, a growing body of work is progressing in this direction by isolating specific nonlinear mechanisms and making them solvable under assumptions on the data.

One line of work studies Gaussian inputs and structured targets (e.g., single- and multi-index models). Fully nonlinear neural networks provably outperform kernel methods using fewer samples because they exploit the structure in the target function to learn relevant features [^1] [^73] [^32] [^13] [^74]. Complementarily, methods from statistical physics enable computing exact asymptotics for Bayes-optimal inference and learning dynamics in these models [^20] [^12] [^193]. A related setting is two-layer neural networks with quadratic activation functions, where recent results have characterized the exact asymptotics, training dynamics, and scaling laws [^85] [^28] [^76] [^230]. Several other lines of research isolate distinct nonlinear phenomena: the convergence of homogeneous networks trained on logistic losses to max-margin solutions [^259] [^171], the reduction of training dynamics to low-dimensional summary statistics in teacher-student models [^237] [^99] [^29] [^265] [^284], memorization in associative memory models [^204], learned algorithmic structure in modular arithmetic tasks [^197] [^104] [^145], nonlinear solvable models of attention [^290] [^36], and improved scaling laws from nonlinear feature learning [^38].

Taken together, these approaches illustrate both the promise and the limitations of current nonlinear toy models: each captures a slice of fully nonlinear learning dynamics, yet no unified framework has emerged. We view this space as an open and rapidly evolving area, and return to these challenges in our discussion of open problems in Section 5.

### 2.2 Insightful limits reveal fundamental behavior

Modern deep learning systems are enormous: they regularly involve hundreds of interacting architectural components comprised of hundreds of billions of parameters and trained on trillions of tokens. With so many interacting degrees of freedom, constructing detailed microscopic theories that track individual parameters in practical systems seems all but hopeless.

Fortunately, complex systems often simplify when approximated as effectively *infinite* in size, revealing simple mathematical structure that remains informative even for the original finite system. This strategy is well established in statistical and chemical physics: for example, the ideal gas law, $PV=nRT$, is derived in a limit of infinite number of particles (often termed the thermodynamic limit) yet accurately describes real parcels of gas of finite volume. Limits are a central mathematical tool for managing the complexity of deep learning, and their recurring success in doing so provides strong evidence for an emerging theory.

Here we discuss the limit of infinite width in detail. We conclude by mentioning other limits and offering some unifying ideas.

#### The infinite width limit and the lazy/rich dichotomy.

The dynamics of a deep neural network often simplify when one takes the number of neurons in each hidden layer to infinity. Such a limit generally leads to so-called mean-field behavior in which we only need to describe the evolution of the neuron population as a whole (as e.g. a probability distribution) and we can ignore what each individual neuron is doing. However, achieving this limit requires shrinking the initialization scale as width increases to prevent activations in deeper layers from diverging. The key subtlety in taking the infinite width limit is that the rate at which we suppress these initial weights strongly influences the resulting training dynamics, leading to one of two qualitatively distinct limiting behaviors.

The lazy, kernel, or linearized regime. The first forays into the land of infinite width studied only a network’s statistics at initialization, not its training dynamics [^202] [^219]. These works found that, in order for the inputs to hidden neurons to neither vanish nor explode as width increases, the parameter size at initialization has to decay as $\text$[width]$^$-1/2$$. This is not a surprise: it is just the well-known LeCun initialization rule [^151], which can be easily derived from the central limit theorem. Later works that tried naively training the parameters of these infinite-width networks found the surprising fact that the network’s weights and hidden representations change only negligibly, yet these small changes accumulate to produce substantial changes in the output function. As a result, the training dynamics are linear in the parameters in the sense discussed in Section 2.1, and the evolution of the target function may be expressed entirely in terms of the NTK [^123] [^153]. While a network in this limit is wonderfully analytically tractable, the fact that its hidden representations do evolve only negligibly means that it fails to exhibit feature learning. While the definition of feature learning is much debated (see 4), all agree that at minimum it requires the network’s hidden activations on a given data sample to change from their values at initialization, which does not happen in this limit. This suggests the NTK infinite-width limit is not the right one to study. Networks in this linearized regime were later termed “lazy” by [^58].

![Refer to caption](https://arxiv.org/html/2604.21691v1/figs/limits/lazy_rich_plots.png)

Figure 2: Large and small network output multipliers are sufficient to induce lazy and rich training dynamics. We train a shallow student network f ^ ( 𝒙 ) = α n ∑ i 1 a ReLU 𝒘 ⊤ \\hat$f$($\\bm$x$$)=\\frac$\\alpha$$n$\\sum\_$i=1$^$n$a\_$i$\\text$ReLU$($\\bm$w$$\_$i$^$\\top$$\\bm$x$$) with width 200 n=200 to match a teacher network ∗ 3 f^$\*$($\\bm$x$$)=\\sum\_$i=1$^$3$a^$\*$\_$i$\\text$ReLU$(($\\bm$w$$^$\*$\_$i$)^$\\top$$\\bm$x$$) on two-dimensional input data. We plot the training trajectories of the student weights w w\_$i$ (color denotes sgn \\mathrm$sgn$(a\_$i$) ) against the teacher feature directions. Left: with 0.1 \\alpha=0.1, the dynamics are rich: the student weights grow significantly and cluster in angle around the teacher feature directions. Right: 30 \\alpha=30 lazy: the student weights move negligibly during training, even though the loss drops. Experiment reproduced from 58.

The rich, active, or feature-learning regime. In answer to this, several authors identified an alternative infinite width limit in which training does induce feature learning. The key insight was essentially to downscale the final-layer weights by a factor of $\text$[width]$^$-1$$, rather than the earlier $\text$[width]$^$-1/2$$, thereby forcing the network weights to change more to compensate.<sup>2</sup> While this makes the function trivial at initialization (at infinite width it is uniformly zero), it can still grow nontrivially during training, changing by an order-one amount upon each gradient step.

This downscale-the-network-output idea first appeared in the shallow “mean-field networks” of [^188], [^233], and [^57]. [^93] and [^280] found that this idea also works for networks of arbitrary depth, bundling the resulting hyperparameter scaling factors together into the celebrated “Maximal Update Parameterization” discussed in Section 2.4. It is now widely accepted that infinite-width neural networks can learn features.

Wide networks in this “rich” regime display a huge range of interesting behaviors that their lazy counterparts do not. The most significant is certainly that the hidden features of these networks change over time, adapting to the structure in the input data, altering the internal geometry of hidden representations over the course of training [^41]. Subpopulations of neurons specialize, learning to attend to different features latent in the data [^12] [^99] [^230]. For instance, in tasks where the optimal predictions involve low-dimensional subspaces of high dimensional data, the distribution over first layer weights evolves to amplify weights in the subspace of interest [^189] [^1] [^195] [^69] [^76] [^85] [^196]. When the scale of the initialization is made even smaller, they often show the greedy low-rank bias discussed in Section 2.1, acquiring some components of the task before others [^241] [^9] [^10].<sup>3</sup>

The lazy–rich dichotomy, and its dependence on initialization scale, emerged as a central finding of infinite-width analyses. Subsequent work has shown that analogous behavior appears even at finite width: scaling down the network output promotes feature learning, pushing models toward the rich regime, whereas increasing the output scale tends to linearize training dynamics and induce lazy behavior [^58]. This sensitivity to initialization scale connects to a broader literature on inductive bias, where seemingly small changes to the learning setup can steer training toward fundamentally different solution classes [^173] [^275]. Figure 2 illustrates how the same finite network, trained with different output scalings, can exhibit either lazy or rich learning dynamics.

#### The infinite depth limit and other hyperparameter limits.

As with infinite width, one can arrive at a stable infinite depth limit of a deep residual network by downscaling the contribution of each layer so the residual stream does not blow up. Here, too, there are different limiting behaviors depending on the size of this downscaling factor: suppressing each layer by a factor of $\text$[depth]$^$-1$$ results in limiting dynamics in which the residual stream changes smoothly over depth [^39] [^59] [^52] (reminiscent of Neural ODEs [^56]) while suppressing each layer by a factor of $\text$[depth]$^$-1/2$$ results in limiting dynamics in which the residual stream diffuses as if driven by a stochastic differential equation [^40] [^282]. Networks in these two limits converge to qualitatively different solutions in realistic architectures such as transformers [^77]. It is not yet clear which is the more important limit to study.

Some deep learning architectures admit size limits other than those of large width or large depth. Instead of increasing size or total number of distinct feedforward layers, one can also analyze the infinite limits of recurrent architectures using similar mean-field ideas [^61] [^26]. State-of-the-art transformer models include more expressive constituent blocks such as multi-head self-attention layers and mixture-of-expert multi-layer perceptrons. These layers have multiple scaling directions including head count, head size, and context length for attention [^120] [^39] and expert count, expert size, and sparsity for mixture-of-expert models [^175] [^129]. Clarifying the interplay of different infinite limits in these models is important to making contact with modern practice and to disentangle various hyperparameters related to initialization and optimization (see Section 2.4).

Lastly, most optimization hyperparameters have an associated limit. As the batch size approaches infinity, we reach population gradient descent. As we take learning rate to zero, we recover gradient flow. If we add an infinitesimal weight decay and take training time to infinity, we first optimize the loss to convergence, then perform parameter norm minimization conditioned on the final value of the loss. We discuss how to understand the corrections induced by having finite values for some of these hyperparameters in Section 2.4.

#### Joint scaling limits.

Sometimes scaling limits in multiple variables ($\nu_$1$,\nu_$2$$) play nicely, in the sense that $\displaystyle\lim_$\nu_$1$\to\infty$\,\lim_$\nu_$2$\to\infty$$ gives the same result as $\displaystyle\lim_$\nu_$2$\to\infty$\,\lim_$\nu_$1$\to\infty$$. For example, the infinite width and depth limits in residual networks usually commute in this way, so long as one takes a sensible parameterization [^113]. However, in many theoretical machine learning settings, different scaling dimensions do not commute, and the limiting behavior could depend on a limiting ratio $\nu_$2$/\nu_$1$$. Such joint/proportional scaling limits are common in random matrix theory: for example, consider the SVD of a random matrix with $P$ rows and $N$ columns with $N,P\to\infty$ with $P/N$ held constant. In machine learning theory, neural networks trained with random data can often be described by a joint scaling limit where both the dataset size and parameter count are taken to infinity, but one or more of the ratios $\frac$\text$[data]$$$\text$[input dim]$$$, $\frac$\text$[data]$$$\text$[width]$$$, or $\frac$\text$[data]$$$\text$[parameters]$$$ is a finite value [^247] [^237] [^286] [^159] [^174] [^185] [^19]. This joint scaling is likely necessary in the study of compute-optimal neural scaling laws where the training horizon (i.e. dataset size) is scaled linearly with the total parameters [^116] and to theoretically characterize hyperparameter transfer phenomena [^43]. These joint (data & model size) limits are potentially important as infinite parameter limits at fixed dataset size are capable of perfect interpolation and do not capture scaling law behaviors across model sizes (see Section 2.3). Other well-studied joint scaling quantities include the ratio $\frac$\text$[width]$$$\text$[depth]$$$ in non-residual networks [^110] [^157] [^206] [^109], the ratio $\frac$\text$[learning rate]$$$\text$[output multiplier]$$$ in the rich regime [^10], and the “SGD noise temperature” $\frac$\text$[learning rate]$$$\text$[batch size]$$$ [^179] [^127].

#### The Discretization Hypothesis.

Overall, the widespread use of limits to manage the complexity of deep learning reflects a recurring theme across scientific disciplines: appropriate asymptotic perspectives often render otherwise intractable systems analytically tractable. Many theorists share a heuristic belief that most practical neural networks can be understood as noisy, finite approximations to models of infinite size.<sup>4</sup> By analogy, one numerically solves a partial differential equation by discretizing over space and time, and the finer the discretization, the smaller the numerical error from the desired continuum process. This is very possibly also true of deep neural networks, with width and depth taking the place of space and time. Other finite hyperparameters, such as the learning rate, batch size, and dataset size, might also be understood in this way.

We might call this belief the Discretization Hypothesis. While it has yet to be made precise or proven (see 5), this hypothesis has implicitly underpinned much important work, and little in the analytical study of large models makes sense without it.

The Discretization Hypothesis amounts to the statement that finite-size corrections from limits typically worsen performance while saving costs in data, time, memory, and compute. Showing that these finite-size effects deliver a general benefit that cannot be achieved any other way would falsify this hypothesis.

### 2.3 Simple empirical laws capture meaningful macroscopic statistics

Deep learning is highly measurable: it is easy to track a vast array of quantities before, during, and after training. While any quantity can be measured, the most lawful are typically aggregate, macroscopic statistics over many weights and samples. For instance, the train and test losses are aggregates over many samples. These quantities are occasionally described by simple empirical laws relating one to another. Such laws have already played an important role in shaping both our understanding and practice of deep learning.

This pattern has ample precedent in the quantitative sciences. Many important physical and chemical laws were first discovered as empirical regularities and only later understood in terms of deeper principles, including laws due to Kepler, Snell, Boyle, Hooke, Faraday, Ohm, Poiseuille, and Planck. Given how often scientific fields have developed in this way, it seems likely that deep learning will continue to yield empirical laws as its science matures. Here, we highlight a handful of examples and conclude with takeaways for theorists.

![Refer to caption](https://arxiv.org/html/2604.21691v1/x3.png)

Figure 3: The loss of large neural networks decays according to predictable neural scaling laws. These neural scaling laws take the form of power laws (linear on log-log plots) in compute, dataset size, and parameter count. Reproduced from 136

#### Neural scaling laws.

The single most important measurement of any machine learning system is its test loss. Given the complexity of large deep learning systems, one might expect the test loss to be a complex, unknowable function of the system’s hyperparameters. This is not so: studies of neural scaling laws [^136] [^115] demonstrate that, within an architectural family, the final loss follows a predictable power law function governed by only three scalar variables: compute, the amount of data, and the network’s size. These power laws are shown in Figure 3.

Why does test loss decay as a power law in these variables, and what determines the scaling law exponent? We still do not know! While scaling laws are often attributed to structure in the data, with candidate explanations in terms of the dimensionality of the data manifold [^251] [^14], feature superposition [^168], and power laws latent in task structure [^67] [^37] [^192] [^230] [^76], they may also depend on details of the architecture and optimizer [^21]. At present, no framework can robustly predict the observed exponents a priori from dataset and architectural properties across realistic settings (see 7), though recent progress has begun to move in this direction [^48]. The fact that test loss is so predictable strongly suggests that a simple underlying explanation remains to be found.

#### Weight dynamics at the edge of stability.

Because every model is the result of a training process, we would like to understand the dynamics and trajectory of a model’s weights during training. While there are simple cases where these dynamics are exactly solvable (see Section 2.1), this is usually well out of reach. The loss landscape dictates the network’s dynamics, but a direct visualization of the loss, as is done in [^156], suggests an immensely complicated landscape that is unlikely to have lawful regularities.

Nonetheless, some robust patterns in the coarse, aggregate properties of weight trajectories have been found. One of these is the sharpness of the network loss surface, defined as the largest eigenvalue of the Hessian with respect to the parameters. When a typical network is trained using full-batch gradient descent with learning rate $\eta$, the sharpness undergoes two distinct phases: a gradual increase (termed progressive sharpening) followed by a plateau near $2/\eta$ ([^62]; see Figure 4), called the edge of stability.

![Refer to caption](https://arxiv.org/html/2604.21691v1/x4.png)

Figure 4: Gradient descent occurs near the edge of stability. Three architectures are trained with full-batch gradient descent on CIFAR-10 with varying learning rate η \\eta. Plots show the train loss (top row) and Hessian sharpness (bottom row). For each step size, observe that the sharpness rises to 2 / 2/\\eta (dashed horizontal lines) and hovers at or just above this value. Reproduced from 62.

Having identified these regularities, we can begin to understand them. Progressive sharpening provably occurs in deep linear networks [^86] [^283], yet a quantitative explanation suitable to realistic nonlinear networks remains to be found (see 8). More is understood about why the sharpness stabilizes at $2/\eta$. Particularly, $2/\eta$ is the maximum stable sharpness achieved in convex optimization–any sharpness larger than $2/\eta$ would cause parameter oscillations of increasing magnitude. In more general cases, [^72] showed how coarse properties of the third-order loss curvature can cause the (second-order) sharpness to stabilize at $2/\eta$. Follow-up work reveals that loss dynamics at the edge of stability can be decomposed as smooth, time-averaged, gradient flow dynamics plus oscillations in unstable directions [^63]. These works make quantitative predictions about the parameter trajectory which closely match experiment.

#### Coarse properties of hidden representations and weights.

There are a handful of other cases in which coarse properties of neural networks’ hidden representations and weights are known to obey simple equations. We will briefly mention three of these.

Neural collapse. Consider a neural network classifier trained to choose among $C$ classes. [^211] found that, at the end of training, the final-hidden-layer representations of samples from each class tend to cluster tightly around their class mean. Furthermore, the $C$ class mean vectors form a regular simplex. Later theoretical work has explained this geometric arrangement as the natural energy-minimizing configuration when (a) the loss used is cross-entropy and (b) a small amount of weight decay is applied [^291].<sup>5</sup>

The neural feature ansatz. At the other end of the network, there are some robust regularities known about the first-layer weights. [^221] show that, after training, the Gram matrix of the the first-layer weights $$\bm$W$$_$1$^$\top$$\bm$W$$_$1$$ aligns with the average gradient outer product:

$$
$\bm$W$$_$1$^$\top$$\bm$W$$_$1$\propto\mathbb$E$_$$\bm$x$$\sim\mathcal$P$_$\text$data$$$\!\left[\nabla_$$\bm$x$$$f($\bm$x$$;$\bm$\theta$$)\nabla_$$\bm$x$$$f($\bm$x$$;$\bm$\theta$$)^$\top$\right],
$$

where $\nabla_$$\bm$x$$$f($\bm$x$$;$\bm$\theta$$)$ denotes the Jacobian of the network with respect to $$\bm$x$$$. While this rule is heuristic and inexact, it often makes strikingly accurate predictions for quantities like the top eigenvectors of $$\bm$W$$_$1$^$\top$$\bm$W$$_$1$$. Similar heuristics hold at deeper layers. At time of writing, there are only partial theoretical explanations for this phenomenon; see [^293] [^35].

Gradient flow conservation laws. A striking regularity identified in linear networks is that the difference between the covariance and Gram matrices of consecutive layers $$\bm$W$$_$\ell$$\bm$W$$_$\ell$^$\top$-$\bm$W$$_$\ell+1$^$\top$$\bm$W$$_$\ell+1$$ is conserved under gradient flow [^240] [^81] [^5]. What initially appeared to be a curiosity of linear networks was later shown to follow from continuous symmetries of the parameterization — an instance of the Noether principle — and thus could be used to identify similar conserved quantities in nonlinear networks [^147] [^261] [^181] [^182]. For instance, the rescaling symmetries in networks with homogeneous nonlinearities (e.g., ReLU), the scale symmetries preceding normalization layers (e.g., batch normalization), the translation symmetries in the logits preceding a softmax, and the rotation symmetries between key and query matrices in attention all lead to symmetry-specific statistics of the parameters that are conserved under gradient flow and weakly broken by SGD in predictable ways.

#### Takeaways for theorists.

Theory can be built “bottom-up,” starting from first-principles math as in Sections 2.1 and 2.2, or “top-down,” starting from empirical observations and attempting to explain them. In this section we have highlighted a few notable examples of top-down theories. We expect more to come. The measurability of deep learning makes observation and empiricism a particularly fruitful approach, since experimentation can be iterated on quickly, while revealing mathematically simple relations and structure in trained models. Of course, some caution is necessary: most macroscopic statistics don’t obey a simple and general mathematical law — or at least don’t seem to until plotted against the right quantity — and so the challenge is to find those that do. We encourage theorists of deep learning to proactively use experiments to look for lawful regularities in neural networks.

### 2.4 Hyperparameters can be disentangled and understood

Training a deep learning system involves many numerical knobs, termed “hyperparameters.” These include optimization hyperparameters such as the learning rate, batch size, momentum, and initialization variance, as well as architecture hyperparameters such as width, and depth. The large number of hyperparameters in deep learning presents a challenge not only for practitioners, who must tune them carefully in order to achieve optimal performance, but also for researchers, who must grapple with many confounding factors when trying to interpret the outcome of scientific experiments. It is only in the last few years that the theory community has come to realize that hyperparameters can be disentangled and understood, and that the resulting mathematics is often both useful for practitioners and clarifying for theorists.

This study of hyperparameters bears similarities to the study of the constant parameters governing the behavior of a physical dynamical system. For example, in a fluid flowing through a pipe, a dimensionless number called the Reynolds number computed from the pipe diameter and the fluid’s speed, density, and viscosity determines whether flow is laminar or turbulent. While solving for the trajectory of the turbulent fluid is extremely difficult, it is nonetheless very helpful to be able to quickly predict whether flow will be turbulent at all — and how things change if you scale up the pipe diameter or increase the fluid flow. Similarly, while solving the optimization dynamics of a neural network is very difficult, it is often very helpful to quickly obtain a coarse picture of how things change if you change one or more hyperparameters. In this section we highlight two lines of work in which hyperparameters have been found to admit explanatory theory.

#### Understanding optimization hyperparameters.

Stochastic gradient descent has two hyperparameters: learning rate and batch size. The algorithm’s dynamics are often invariant under a simultaneous rescaling of both. That is, if one doubles both the learning rate and batch size, and halves the number of optimizer steps (or equivalently, keeps fixed the number of training examples processed), then the trajectory stays nearly the same. This so-called *linear scaling rule* [^101] is useful for transferring a learning rate that was tuned for one batch size to a different one. A line of theoretical work has clarified this rule of thumb by interpreting SGD as a discretization of an underlying stochastic differential equation (SDE), a perspective that predicts the linear scaling rule [^179] [^127] [^53] [^158] [^164]. [^176] extended this line of work from SGD to adaptive optimizers, for which they argued that the learning rate should scale with the square root of the batch size.

This invariance perspective explains how to adjust hyperparameters across batch sizes, but not how to choose the batch size itself. That choice involves an inherent tradeoff between two resources: serial time (the number of sequential training steps) and overall compute (the total amount of computation, often closely tied to cost) [^172] [^126] [^186] [^249]. For a practitioner who cares only about serial time and not at all about cost, the optimal batch size is the full dataset. Conversely, for a practitioner who cares only about cost and not at all about serial time, the optimal batch size is 1. In reality, no practitioner falls exactly in either bucket; a practitioner might care more about one resource than the other, but is generally willing to accept some slack in return for a better deal on the second resource. A frequently discussed concept is that of the *critical batch size*, a batch size which trades off between these two concerns. [^186] proposed a simple model of this tradeoff under which the Pareto frontier between serial time and compute takes the form of a hyperbola.

Optimization hyperparameters in deep learning affect not just the speed and cost of training but also the trajectory that training follows. This in turn affects various properties of the learned network, including generalization performance [^139] [^243] and compressibility [^51] [^23]. A fruitful line of work has sought to explain these effects through the hypothesis that many implicit effects of optimizer hyperparameters can be understood as *implicit regularization of loss function curvature*.<sup>6</sup> Empirical studies initially observed that first-order optimizers regularize the curvature (i.e. Hessian) of the loss function, with larger learning rates and smaller batch sizes yielding stronger regularization strengths [^139] [^127] [^128] [^62]. Meanwhile, theoretical works in simplified settings showed that this effect can be explained by Taylor-expanding the objective to third order, as such a calculation reveals that oscillating or fluctuating dynamics automatically induce curvature regularization [^34] [^165] [^71] [^271] [^161]. Building on this body of work, [^63] recently showed that for several optimizers in the full-batch setting, the whole training trajectory on realistic neural nets is well-modeled by a curvature-penalized gradient flow, where the role of the hyperparameters is to modulate both the form and strength of the curvature penalty. As a result, we now have a mathematical understanding of the learning rate in full-batch gradient descent, and are mostly free to instead study the simpler dynamics of gradient flow plus a loss curvature penalty.<sup>7</sup> Other analyses have developed analogous characterizations for stochastic dynamics in more specialized settings [^216] [^55]. Fully extending this characterization to stochastic and adaptive optimizers would give us a common language for reasoning about the implicit effects of optimization hyperparameters on the training trajectory. It then remains to understand how these modifications to the training trajectory influence properties of the learned network (see 8).

![Refer to caption](https://arxiv.org/html/2604.21691v1/x5.png)

Figure 5: The theory of network parameterization permits learning rate transfer across widths. Transformers of varying widths trained on WikiText-2 under standard parameterization (left) and μ \\mu P (right). Under standard parameterization, the optimal learning rate decreases as model width increases. Under P, by contrast, the optimal learning rate remains nearly constant across widths, making it possible to predict the learning rate for wide networks from experiments on narrower, cheaper models. Reproduced from 279.

#### Disentangling architecture hyperparameters from optimization hyperparameters.

There has been a highly successful line of work aimed at disentangling architecture hyperparameters such as width, depth, and output multiplier (see the lazy/rich dichotomy in Section 2.2), from optimization hyperparameters such as the learning rate and initialization variance. The Tensor Programs framework [^280] [^281] makes this separation explicit, writing hyperparameters such as the learning rate in the form $\eta=\eta_$0$\cdot[\mathrm$width$]^$c$$, separating a scale-independent coefficient $\eta_$0$$ from a width dependent factor with exponent $c$. This line of work then asks: how can we set these exponents such that we retain interesting training behavior at infinite width? A remarkable insight from this analysis is that all non-trivial and non-explosive scalings give one of two limiting behaviors, analogous to the rich/lazy dichotomy in Section 2.2: in the Neural Tangent Parameterization (NTP), features are frozen during training, and in the Maximal Update Parameterization ($\mu$ P), features evolve. Since feature learning is essential for most tasks, this analysis tells us that $\mu$ P is the scaling to use, resolving how hyperparameters should scale with model width. This understanding enables hyperparameter transfer: we can tune hyperparameters on small proxy models and then transfer them to large, production-size models, where they remain near-optimal when both models are sufficiently wide ([^279]; Figure 5).

At the same time, the theory underpinning this result is asymptotic and does not fully account for its empirical effectiveness. In practice, models are trained at widths far smaller than the dataset size, and the usefulness of transfer depends on how quickly optimal hyperparameters stabilize with width. [^207] and [^95] and [^114] take steps toward closing this gap, providing evidence that a small set of spectral statistics stabilizes rapidly across widths under $\mu$ P and approximately governs the optimal hyperparameters. This scaling-centric approach to hyperparameters was later extended to depth scaling [^282] [^40] [^77], and leveraging this approach with other scaling dimensions remains an important future direction (see 6).

### 2.5 Universal phenomena appear across settings and tasks

![Refer to caption](https://arxiv.org/html/2604.21691v1/figs/universality/diffusion_arch_names.png)

(a) Universality across architectures

Deep learning is not a single recipe followed exactly every time: different systems use very different architectures, datasets, training algorithms, and objectives, with ingredients combined in creative ways. This versatility has enabled successes on many tasks and modalities including vision, language, speech, time series, protein sequences, and games, but the resulting model diversity makes it less clear how to approach the development of scientific theory. Do these diverse settings share deep commonalities we might hope to capture scientifically?

Here, we review a growing body of evidence that there are indeed universal phenomena at play in these diverse settings. This is good news for theory: when many different complex systems exhibit the same universal behavior, it suggests that a simple underlying explanation may exist. We highlight this universality through three different viewpoints: (1) different architectures reach comparably good performance on many tasks; (2) different datasets share similar statistical properties; and (3) the learned representations and weights across different architectures and datasets are surprisingly alike. This roughly echoes examples of universality in which disparate physical systems share deep commonalities or display similar behavior at large scales.<sup>8</sup> We end by highlighting a few theoretical successes in modeling universal phenomena.

#### Universal inductive biases.

Performance on a given task is often robust to variations in architectures, training algorithms, and objectives, in the sense that many alternate choices still lead to models that can solve the task. A well-known example is the choice between convolutional networks and transformers in computer vision tasks, which after much debate have been shown to obtain similar performance when matching compute, data size, and training recipes [^169] [^255]. In diffusion models, this similarity has been further shown to hold at the level of input-output mappings, with transformers and UNets generating near-identical images when fed with the same noise samples [^289], as shown in Figure 6. These results strongly indicate that different architectures share similar inductive biases despite their apparent differences. As a partial explanation, recent work has shown that assuming inductive biases towards locality and adaptivity to geometric structures leads to accurate quantitative predictions about the behavior of diffusion generative models [^133] [^135] [^205].

#### Universal structure in data.

The no-free-lunch theorem states that generalization on completely arbitrary data with a common learning strategy is not possible [^274]. Therefore, deep learning must rely on particular features of the data present across all datasets and modalities on which it succeeds. For instance, many classes of images and audio signals share power-law spectral properties, sparsity patterns, and multiscale structures, and can be analyzed with general-purpose wavelet bases [^209] [^178] A similar phenomenon in text data is the ubiquity of Zipf’s law (word frequencies obey a power-law distribution) that holds over many natural and artificial languages [^160] [^217]. Hierarchical, compositional structure is also routinely used to model both images and text, which can sometimes be related through a common model [^47] [^244] [^46]. These shared statistical properties are a partial explanation for the ability of a single learning algorithm (say, a transformer trained with SGD) to tackle seemingly unrelated datasets, leaving only the finer-grained differences between them to be learned.

#### Universality in representations.

Going deeper in the internals of the network, it has been observed that representations learned by different networks can be similar across random initializations, widths, and architecture [^222] [^143] [^17] [^121] [^198]. It has been shown that networks trained to solve different tasks learn similar representations across training datasets (ImageNet and Places-365, [^154]), objectives (supervised or self-supervised, [^17]), and modalities (vision or language, [^121]). Furthermore, this similarity grows as model size and performance increase, hinting that neural activations converge towards a universal (“Platonic”) representation [^17] [^121], as shown in Figure 6. In simplified settings such as random feature representations, this convergence is a consequence of the law of large numbers applied to the feature kernels [^223] [^108]; in deep linear networks, it can be proven to arise from the implicit regularization of SGD [^294]; in more diverse settings, recent evidence suggests that the universality of representations may ultimately trace its origins to universal structure in data [^121] [^137]. Recent advances in identifiability theory [^122] also have shown that representational convergence happens at the global optimum of unsupervised [^142], self-supervised [^292] and supervised [^228] objective functions under a suitable data generating process [^227]. Several works have also shown empirically that this similarity can extend to the level of individual neurons [^162] [^80] [^140]. In some cases, similar representations have been found in both artificial neural networks and biological neural networks [^209] [^277] [^187], though the extent of this correspondence remains controversial [^44]. While a global trend towards similarity is emerging, it should be noted that the range of settings in which this convergence is observed, and its extent, are not fully known (see 10). In particular, recent work has shown that this apparent convergence to universal representations depends crucially on the chosen comparison metric across similarities [^103]). A growing literature is devoted to understanding which representation similarity metrics one should choose in different circumstances [^260] [^141] and highlighting the cases where they can be unified [^111] [^272].

If the mechanisms learned by large models are indeed universal, this is very encouraging for theory: behavior shared across *many* systems should depend primarily on the features common to *all* such systems, and thus admit a description simpler than any particular model in isolation. Moreover, if the internal structure of trained neural networks primarily reflects the structure of data, then in studying neural networks we may ultimately be studying the structure of data and its generating processes (see 2). In particular, since language data comes directly from humans, understanding its structure may teach us something new and fundamental about ourselves.

## 3 Relation to other perspectives

There are several ongoing approaches to developing explanatory scientific theory of deep learning, each adopting a different perspective and using different sets of tools. We believe that these perspectives are essentially all complementary: all either directly seek a mechanics of learning or would symbiotically benefit from one.

#### The statistical perspective.

The rich tradition of classical learning theory remains influential today.<sup>9</sup> [^24] offer a lucid summary of its central framing: any statistical prediction method must balance expressivity (to represent the richness of real data), complexity control (to make the most of finite training data), and computational efficiency (to yield practical algorithms). It is apparent that deep learning is sufficiently expressive, but it is not clear how a good function is selected from this enormous function class, nor why simple gradient methods suffice to train such complex beasts. The modern statistical viewpoint suggests two answers: deep learning has an implicit inductive bias towards simple, well-generalizing functions [^273], and despite their nonconvexity, the very high dimensionality (overparameterization) of neural networks makes optimization easy.

These questions are good ones, and we believe these answers are basically correct. The challenge is now to make them precise in the case of neural networks. It is clear that doing so will require taking a close look at the nature of the training process. Only once we have done so will we be able to back out how this implicit bias arises and why gradient methods suffice for optimization. We do not believe these answers will be generic statements, but instead critically rely on important properties of deep learning and of natural data. The statistical perspective thus leads naturally to a serious scientific study of the mechanics of training.

#### The information-theoretic perspective.

A closely-related approach seeks to explain deep learning in terms of information-theoretic ideas. In this view, learning is a process of extracting information from datasets, and a learning system works when it extracts information useful for prediction while discarding irrelevant information. This perspective hopes to understand learning as compression of the dataset into either the model’s parameters or its hidden representations, with good generalization resulting when this compression is successful [^252] [^276].

We find this perspective insightful, and it seems likely to us that a picture of this nature will hold. As with the statistical perspective, a major remaining question is how to make this view concrete and actionable: how do the architecture and training process of deep learning interact to actually implement this compression, and what factors make it more or less successful? Doing this, too, will require taking a close look at the nature of the training process, the architecture, the data, and their interactions. The information-theoretic perspective thus also leads naturally to a serious scientific study of the mechanics of training.

#### Physics of deep learning.

This community descends from the older physics of machine learning lineage [^119] [^3] [^90] and essentially seeks satisfying average-case theories of neural network learning [^287] [^15] [^191] [^231].<sup>10</sup> The close relationship between physics and machine learning was recognized by the [2024 Nobel Prize in Physics](https://www.nobelprize.org/prizes/physics/2024/summary/). This approach is in line with (and has largely shaped) the perspective presented in this paper, and the project of this community is arguably the development of a mechanics of learning. The challenge then is to clarify important problems and coordinate effort for efficient progress.

#### Perspectives from neuroscience.

Several approaches to developing a science of the brain suggest approaches to developing a science of deep learning. One approach starts from hypotheses about neural systems — for example, that their computation amounts to some form of approximate probabilistic inference — and seeks to make deductions and predictions from this hypothesis [^75] [^87]. Some of these predictions seem to hold suspiciously well in deep learning: see, for example, the case of edge-selective cells in the visual cortex [^209] and edge-selective receptive fields in convolutional networks [^288]. Another approach termed systems neuroscience seeks to directly decompose subsets of the brain into interpretable circuits and reverse-engineer the structure of their learned representations [^60] [^30] [^144]. This approach resembles mechanistic interpretability, which has adopted some of its methods and intuitions.

We expect and encourage this dialogue to continue, and it seems plausible that some of these high-level hypotheses about the brain — e.g., that the brain admits at least a partial decomposition into interpretable circuits and that local circuits implicitly solve inference tasks — will turn out to be true of deep learning. The reasons these facts are true, if indeed they are, is surely bound up in the dynamical way learning actually happens. A study of the mechanics of learning is thus important to the continued exploration of these ideas.

#### Developmental interpretability/singular learning theory.

This approach, which grew out of the mechanistic interpretability community, seeks first-principles predictive theories of neural network learning based on the singular learning theory framework of [^269], emphasizing a Bayesian perspective and aiming to understand training as a process of sequential phase transitions mediated by the geometry of the loss landscape [^117]. We see this community as seeking the same goal we suggest here — a fundamental mechanics of learning, and a rigorous foundation for interpretability — but with a toolkit that differs from the other listed perspectives. There is potential for fruitful cross-pollination and tool-sharing between these different approaches.

#### Science of deep learning.

It has long been appreciated by practitioners that machine learning is largely a practice of trial and error and that it may be possible and beneficial to systematize it [^149] [^89] [^224] [^18]. Indeed, much of the rapid empirical progress of the last decade resulted from systematic organization around agreed-upon benchmark tasks [^79]. Nonetheless, the training and application of large models remains more alchemy than science. We believe that a fundamental mechanics of the learning process is the foundation on which this science will finally be built.

### 3.1 Learning mechanics ⇄\\rightleftarrows mechanistic interpretability

We discuss mechanistic interpretability specially because there is a unique opportunity for cooperation. Mechanistic interpretability aims to understand trained neural networks by identifying the internal mechanisms — features, circuits, and learned algorithms — that give rise to their behavior. At its core, this approach is guided by the belief that neural networks admit a human-understandable, mechanistic description that can be uncovered through careful empirical reverse engineering.<sup>11</sup> This approach has already borne fruit: many visually striking or interpretable mechanisms have been discovered in large models to date [^208] [^263] [^84] [^107] [^166].<sup>12</sup>

This is a complementary perspective to our own and presents a wonderful opportunity for symbiosis. At time of writing, mechanistic interpretability remains largely a qualitative science, more reliant on human-judged empirics than on compact mathematical principles or simple governing laws. This is quite natural: semantically-meaningful functions resist mathematical characterization.<sup>13</sup> On the other hand, a mechanics of learning would be quantitative by definition, but by the same token will be too low-level to answer important questions of semantic meaning on its own. These approaches study the same system — i.e., deep learning — at different levels of abstraction, and so of course they can (and should) work together for mutual gain. Calls for rigorous foundations for interpretability have been steadily growing [^250] [^132] [^102], and this is one thing learning mechanics can and should seek to help provide. In turn, mechanistic interpretability offers learning mechanics a rich and growing tableau of empirical phenomena ripe for the development of explanatory mathematical theory.

#### Learning mechanics →\\rightarrow mechanistic interpretability.

We emphasize two complementary avenues through which learning mechanics can support mechanistic interpretability: formalizing core assumptions and explaining how mechanisms develop through training.

*Formalizing core assumptions.* Learning mechanics can make explicit, formalize, and, where necessary, challenge the core and often implicit assumptions that guide interpretability research. These include:

- *linear representability* — that features correspond to meaningful directions in activation space [^194] [^213] [^201] [^183] [^130] [^66];
- *locality* — features and circuits are localizable to particular subsets of model components [^190] [^267] [^65] [^4];
- *sparsity* — that individual features and circuits are activated or functionally relevant on only a small fraction of inputs [^70] [^45]; and
- *compositionality* — that complex network representations and computations arise from the composition of simpler, modular sub-mechanisms [^264] [^257] [^155] [^242] [^225].

These core assumptions underpin the identification, isolation, and analysis of the internal mechanisms of trained neural networks in mechanistic interpretability research. A mathematical theory of learning offers a way to clarify the regimes in which these assumptions hold, the conditions under which they fail, and the sense in which they can be derived from training dynamics and data statistics (see 4).

*Explaining how mechanisms develop through training.* Mechanistic interpretability has generally prioritized describing *what* mechanisms trained neural networks have learned, and there remains a rich opportunity for work which aims to explain *how* and *why* such mechanisms form in the first place. There is already substantial interest within parts of the interpretability community in this dynamical/theoretical perspective, including work on the formation of induction heads [^83] [^210], grokking and progress measures [^200], sudden phase transitions in circuit formation [^82] [^54] [^100] [^212], and the research program of developmental interpretability [^117] [^118], discussed earlier. Our goal is not to replace these efforts but to encourage deeper engagement between mechanistic interpretability and the broader landscape of mathematically grounded ideas and tools in learning mechanics. Echoing [^239], we hope that learning mechanics can play a role analogous to evolution in biology: just as *“nothing in biology makes sense except in the light of evolution,”* the internal mechanisms of trained networks may be most naturally understood in the light of the processes that give rise to them.

#### Learning mechanics ←\\leftarrow mechanistic interpretability.

Conversely, learning mechanics has been deeply influenced by the empirical discoveries of mechanistic interpretability, which often identify concrete phenomena that invite first-principles explanation. Mechanistic interpretability places the structure of data at the center of its analyses, revealing settings in which the relationship between input structure and learned mechanisms is especially clear [^200] [^248]. By contrast, much of classical deep learning theory has relied on highly simplified data models, leaving a gap between theoretical predictions and behaviors observed in practice. In this way, mechanistic interpretability helps bridge this gap by providing learning mechanics with concrete, well-defined targets for theoretical modeling.

Several such observations have already proven influential in stimulating work in learning mechanics, including the emergence of induction heads for in-context learning [^33] [^226] [^203], the role of Fourier features in algebraic tasks [^197] [^145] [^180], and the geometry of features arising from the structure of correlations in the data [^84] [^220] [^137]. Just as the development of physics was often driven by empirical discoveries in adjacent fields, we expect progress in learning mechanics to be driven by theorists who take seriously empirical phenomena, including those uncovered by the mechanistic interpretability community, and seek to explain them.

## 4 Reasons for skepticism and responses

We have made a case that an ambitious mathematical theory of deep learning is possible and that developing this theory is a worthwhile endeavor. This is far from a universal view, and so we now address common counterarguments that a theory of deep learning is either not possible or not a goal worthy of our effort.

#### Competent researchers have been trying to develop a theory of deep learning for decades, and we don’t have one. Surely if there was a theory, we would have already found it.

It is true that machine learning theory is a field with a long history, and certain avenues for developing theory have been thoroughly explored. Why should now be different?

There are several reasons for optimism. First, the practical success of deep learning is comparatively recent, and we have a wealth of new empirical systems to study and mine for explainable phenomena. Some of these phenomena, like the apparent convergence to universal representations discussed in Section 2.5, were only revealed by the last few years of model scaling. These developments have turned the search for a theory of deep learning from a mathematics into an empirical science (and one with no lack of interesting things to measure). We now have much better means to ask questions and check our answers in a tight feedback loop.

Second, the field is much bigger: empirical successes have attracted researchers from physics, mathematics, neuroscience, and other adjacent fields, and so we have more and more diverse minds on the case. Third, it is worth noting that the development of major sciences has usually taken at least several decades, so we should not be too discouraged that we do not yet have all the answers.

#### The objects currently understood from theory are very primitive compared to e.g. LLMs. Surely first-principles understanding of large models is too heavy a lift.

Indeed, we expect that building up to LLMs will be a heavy lift and take considerable time. The near-term hope is instead that some understanding of the basic building blocks of deep learning will prove useful even without a constructive theory that explains the whole model. We can see this happening already in isolated pockets, including empirical scaling laws (Section 2.3), mathematical prescriptions for hyperparameter scaling (Section 2.4), neural-tangent-kernel-based methods for data attribution [^214], and theoretically motivated optimizers [^106] [^131]. These “local theories” of small pieces of the deep learning stack are useful for hyperparameter scaling in large models, even though they are in no way comprehensive theories of the model! One might hope for similarly useful “local theories” that treat subjects like training instabilities, dataset selection and attribution, or the effect of normalization layers.

It is also important to stress that the identification of the right basic objects in a field of science often makes it possible to ask applied questions in a more sensible way. Consider, for example, how the understanding that all matter is made of atoms underlies virtually all other basic science, and how knowledge of electromagnetism permits optical and radiological tools in countless applied disciplines. As discussed in Section 3.1, we hope that learning mechanics can offer tools that adjacent fields such as mechanistic interpretability can apply to better carry out their work. In this way, rigorous work on primitive objects can aid the applied science of large models even without a rigorous theory that builds all the way up.

#### What matters is a model’s high-level behavior. Microscopic theories are too zoomed in to see this.

Models’ high-level behavior is indeed important. How does this fit in with the lower-level sciences of deep learning? We argue that deep learning may be studied at the level of physics, biology, or psychology, with this last including the study of the model’s capabilities, personality [^31], and goals. It seems likely that study at all levels will be necessary. Learning mechanics (the physics of deep learning) is the farthest from model psychology, with mechanistic interpretability (the biology) lying in the middle and connecting the two.<sup>14</sup>

#### We don’t need a theory of deep learning, we need a theory of data.

We think we need both: we need a theory of the structure in data and a theory of how a parameterized model learns it. We touch on the necessity of developing a useful theory of data in Sections 2.5 and 2. These are both part of the project of developing a mechanics of learning.

#### AI will understand itself before we do. Why try to build theory?

This is a present concern for human intellectual endeavors across the board. Our response here has three parts. First, theory is already useful, and will continue to be more impactful as it develops, so this scientific work is likely to make a near-term impact. Second, it seems unlikely that AI working in isolation will suddenly and separately “solve deep learning theory.” It seems more likely that breakthrough progress in a transitory period will come from human scientists using or working with AI, and expert humans will remain in the loop. Third, if one’s goal is AI safety, some human oversight of AI systems will be necessary (unless one trusts the AIs to fully police themselves), and having a human-parseable theory of deep learning gives us a foot in the door.

## 5 Open directions in learning mechanics

It is important for any field, at any stage of development, to have a sense of its important open questions and goals. In this section, we present a curated list of open directions which we expect can be solved by a theory of the mechanics of learning in the next decade. These directions are loosely ordered by their connection to the lines of evidence introduced in Section 2. We hope this helps sharpen a shared research agenda. For a longer catalog and a forum for community discussion, see [learningmechanics.pub/openquestions](https://arxiv.org/html/2604.21691v1/learningmechanics.pub/openquestions).

###### Open Direction 1: What are simple, solvable models of genuinely deep, nonlinear learning?

As discussed in Section 2.1, deep linear networks and kernel methods are the two main workhorse solvable models of learning mechanics. The first captures nonlinear dynamics of the parameters, and the second learns nonlinear functions of the data. While a few special cases of solvable models with both forms of nonlinearity are known, no unified framework has emerged.

Can we get the best of both worlds while maintaining some level of generality? Is there a class of solvable model that captures both deep, nonlinear dynamics and nonlinear function learning? Can such models illuminate new things about feature learning, the role of depth, optimization phenomena (e.g. progressive sharpening), and architectural innovations (e.g. normalization layers, residual streams, self-attention, and gated nonlinearities)? Can it be usefully applied to modern learning paradigms like self-supervised learning, reinforcement learning, and denoising diffusion?

###### Open Direction 2: What would a theory capable of capturing natural data look like?

Deep neural networks find and exploit structure in natural data. This means that the structure of the data must somehow enter into our theories. What is this structure, and how do we find it?

Despite the complexity of data, in many cases models appear to derive their learning signal from a small set of sufficient statistics. What are these minimal data statistics, and how do they enter into a predictive theory of what the model learns? Are these statistics different for different models and at different stages of training? Can we describe the relevant structure in a dataset in terms of a model with free parameters found via an empirical fit?

###### Open Direction 3: Does deep learning implicitly minimize some notion of functional complexity?

Deep networks trained by conventional optimizers are widely believed to have some sort of bias towards learning simple functions. This idea has surfaced many times under different names (e.g. implicit regularization, maximum margin bias, simplicity bias, and spectral bias), but has only been characterized precisely in highly specific settings, and a general picture has not been found. Do deep neural networks broadly seek to minimize some precise notion of complexity among functions with low loss?

If so, what is the appropriate notion of complexity — Kolmogorov, circuit, weight norm, or something else? In what settings or limits is this minimization exact, and when is it only approximate? Do the sparse features and circuitry studied by mechanistic interpretability naturally emerge as the solution to this minimization problem?

###### Open Direction 4: How do we formally define the features learned by neural networks?

Mechanistic interpretability seeks to identify and disentangle the features, circuits, and mechanisms learned by neural networks. Can these concepts be given precise mathematical definitions grounded in first principles? What formal structures naturally emerge from such a definition? Can we use these notions to evaluate and formalize central assumptions of mechanistic interpretability, including linear representability, locality, sparsity, and compositionality, as discussed in Section 3? How do these ideas connect with the less semantically-meaningful — but more precise — rich vs. lazy picture of feature learning discussed in Section 2.2?

###### Open Direction 5: Are finite neural networks properly understood as approximations to infinite limits?

In Section 2.2, we articulated the Discretization Hypothesis, which states that finite neural networks are simply discretized approximations to infinite networks, analogous to how a spatiotemporal discretization is used to numerically approximate the solution to a differential equation. For network width, the limiting continuous object is the measure of neuron activity in hidden layers, while finite depth in a residual network can be viewed as a discretization of a neural SDE or ODE. Small step sizes can render stochastic optimization algorithms approximately equivalent to some kind of flow. In this view, increasing model size (and decreasing learning rate while commensurately increasing step count) serve essentially to improve model performance by decreasing discretization error, at the price of additional computation. Is this the right way to understand width, depth, learning rate, and other finite hyperparameters in deep learning? What does the limiting continuum system look like?

###### Open Direction 6: Can we understand and eliminate all hyperparameters?

In Sections 2.2 and 2.4, we outlined a research program in which hyperparameters are systematically analyzed, disentangled, and in some cases removed by taking appropriate limits. How far can this program go? Can we reach zero hyperparameters, or are some hyperparameters irreducible? If we eliminate all hyperparameters, what remains?

###### Open Direction 7: Can we predict scaling law exponents a priori?

As discussed in Section 2.3, large models exhibit robust power-law scaling of loss with respect to model size, data, and compute. The observed exponents are nontrivial: they do not appear to be simple fractions which might result from elementary dimensionality arguments. It is widely believed that these values are driven largely by structure latent in the dataset, but may also depend on details of the architecture and optimizer. While many explanations for scaling laws have been proposed, a decisive test of any such theory is its ability to predict these exponents quantitatively from first principles. At present, no framework can robustly do so across realistic settings. Can we develop a theory of scaling laws that both explains why power laws arise and predicts their exponents a priori? What measurements of the dataset, architecture, and optimization are required to do so?

###### Open Direction 8: How does loss curvature interplay with architecture, features, and generalization?

As discussed in Sections 2.3 and 2.4, a significant feature of deep learning optimization is that the optimizer implicitly regularizes the curvature (i.e. Hessian) along its trajectory, by steering towards regions of the loss landscape with lower curvature. While progress has been made on formalizing this effect using curvature-penalized gradient flows, it remains unclear how these curvature dynamics relate to other concerns in deep learning theory. Why does the curvature tend to rise in the absence of any such implicit regularization, and can this “progressive sharpening” be attributed to certain properties of the architecture or data distribution? How does the implicit curvature regularization affect the features that are learned? Why does it sometimes lead to improved generalization?

###### Open Direction 9: What makes for a good optimizer in deep learning?

It remains fundamentally unclear why some deep learning optimizers work better than others. Why do adaptive methods, such as Adam and Muon, consistently outperform simpler alternatives like SGD when training large language models? How does adaptive preconditioning in these optimizers interact with a network’s architecture and loss landscape to lead to faster, more stable training? Can we identify fundamental principles that explain the success of modern optimizers, predict when they will fail, and guide the design of new ones?

###### Open Direction 10: In what sense do large models trained differently learn similar representations?

In Section 2.5, we discussed evidence that large models trained from different random seeds — and sometimes even with different widths, architectures, data, or objectives — tend to learn similar internal representations. A precise version of this claim would be very powerful: understanding how representation learning is *universal* would give us confidence that theory developed for one model and setting transfers to many others.

The central difficulty here is methodological: how do we assess “similarity”? There is no single way to compare high-dimensional representations — metrics based on kernel alignment, nearest-neighbors, model stitching, and more compare different aspects of representation geometry. Which ones are stable across training regimes? What is the appropriate metric that quantifies this similarity? What is the largest range of experimental settings under which convergence is observed — what are the representation universality classes?

## 6 How to get involved in the development of learning mechanics

It is always difficult to start doing research in a new field. Consequently, we would like to make it as easy as possible for newcomers to get started. In this section, we extend a hand with some encouragement and advice.

There is no specific academic background required to do useful work in this field. Well-regarded researchers in deep learning theory come from backgrounds in physics, mathematics, computer science, neuroscience, statistics, and more. Moreover, knowing another field well is useful, and established ideas from other fields can be applied to deep learning in some form or another, as the diversity of perspectives on deep learning theory attests (Section 3). Good things grow from cross-pollination. A firm grasp of undergraduate mathematics, a familiarity with deep learning, and a desire to learn are the only definite prerequisites.

### 6.1 Tenets for getting started

If you want to join this field, you are more than welcome. While there is no single correct way to craft theory, there are plenty of pitfalls that many of us encountered when starting out. To help avoid some of them, we have compiled a shortlist of guiding principles for doing research in this field. These tenets are not intended to maximize your number of citations in the short term, and following them may involve some swimming against the current of academia. Instead, they are intended to maximize your impact in the long term and your ability to integrate and contribute to the community.

1. Do experiments frequently. As discussed in Section 2.3, deep learning is a field where the cost of doing experiments is relatively low, with a fast turnaround time. Use that to your advantage! Experiments serve to check assumptions, inform theoretical models, reveal the limitations of a theory, peer beyond the cases that a theory covers, and surface interesting phenomena to study in the first place. Try to include experiments in every paper, and make them as simple and revealing as possible.
2. Simplicity and insight matter more than technical complexity. If you want to do work that is useful for others, they need to understand it, not merely be impressed by it. Take the time to simplify your findings, identify the underlying intuition, and check with simple experiments. A useful idea for thinking about a type of problem is generally more valuable than a difficult theorem or a solution to any particular problem, so emphasize these useful ideas when you present your work. This will make your results more accessible and easier for others to extend and apply. Your conference reviewers may disagree with this philosophy — the conference system tends to reward technical complexity and undervalue simplicity — but it will make your work more impactful in the long run.
3. Value scientific understanding over state-of-the-art performance. Applied deep learning is a field of engineering whose progress is measured by benchmarks. For scientists of deep learning, the game is different: your contribution is gauged by your contribution to collective understanding. It is easy to feel pressure to tack on some engineering benefit in a scientific paper to make the paper seem more relevant and timely, but doing so usually dilutes the paper’s scientific contribution without really affecting practice. Of course, fundamental science should eventually improve state-of-the-art performance, and when this naturally falls out of the science, it is a powerful way to demonstrate what has been understood.<sup>15</sup> When it does not, though, there is no need to force it: set benchmarks aside for the time being and seek understanding on its own terms.
4. Don’t try to do it alone. Deep learning is a field with a lot of history and many known results, and guidance from a live human will help you. You can get pretty far from reading material online and talking to AI, but it doesn’t replace human collaboration and mentorship. You should seek out other people interested in this area, ask for their feedback, and ask to work with them. A weak corollary to this is: when one exists, watch the talk version of a paper in addition to reading it. A great deal more of the nuance of a project is conveyed through in a live presentation. The [Physics Meets ML](http://physicsmeetsml.org/) and [Physics of Learning](https://www.physicsoflearning.org/webinar-series) series are good places to look for talks on what we here call learning mechanics.
5. Try a few different problems before going deep into one. Deep learning theory has so many open questions that you probably won’t know where to start. That’s okay — just jump in, and feel free to change problems a few times early on.<sup>16</sup> Knowing multiple areas is essential to having high-level ideas anyways, and working in an area is the best way to learn it.
6. Invest in fundamental tools and techniques. Compared to more established fields of science, mathematics, and engineering, deep learning is very young. These other fields have identified deep ideas and developed powerful tools applicable to broad classes of problem. Learning these fundamental tools comes in handy when similar problems arise in deep learning. For example, statistical physics and random matrix theory have powerful tools for thinking about high-dimensional interacting systems, and these tools are of great use in learning mechanics when one takes infinite limits. Many of the central ideas of classical optimization theory make regular appearances in the study of neural network optimization. Other concepts from statistical signal processing, such as wavelet decompositions, graphical models, and information theory, have been comparatively less leveraged so far, but may prove to be useful and complementary tools of learning mechanics.

We will put as much useful introductory material as we can on [learningmechanics.pub](https://arxiv.org/html/2604.21691v1/learningmechanics.pub), and we encourage discussion in the comments there. We also encourage taking a crack at the open directions in Section 5. Work hard, have fun, and best of luck — we hope to see a great deal more fundamental science of deep learning in the next few years!

## Acknowledgements

We are grateful for feedback on this paper from many people from several overlapping groups: researchers working on the theory of machine learning, researchers working on AI safety and mechanistic interpretability, practitioners and applied deep learning scientists, neuroscientists, and physicists. This includes Alberto Bietti, Alex Infanger, Alex Williams, Amil Dravid, Anthony Thomas, Avrajit Ghosh, Bin Yu, Bruno Loureiro, Chandan Singh, Clémentine Dominé, David Berman, David Klindt, Denny Wu, Ev Gunter, Honam Wong, Itay Lavie, Jacob Yates, Jacob Zavatone-Veth, Jeff Gore, Jesse Hoogland, Jiechao Feng, Jingfeng Wu, Kaden Tro, Lauren Greenspan, Lenka Zdeborová, Lily Stelling, Lukas Bongartz, Nina Miolane, Noa Rubin, Peter Bartlett, Raymond Fan, Samyak Jain, Soufiane Hayou, Sultan Daniels, Wanyu Lei, Yasaman Bahri, and Zohar Ringel.

[^1]: The merged-staircase property: a necessary and nearly sufficient condition for sgd learning of sparse functions on two-layer neural networks. In Conference on Learning Theory, pp. 4782–4887. Cited by: §2.1, §2.2.

[^2]: High-dimensional dynamics of generalization error in neural networks. Neural Networks 132, pp. 428–446. Cited by: §2.1.

[^3]: Spin-glass models of neural networks. Physical Review A 32 (2), pp. 1007. Cited by: §3.

[^4]: Language model circuits are sparse in the neuron basis. Note: Blog post: [https://transluce.org/neuron-circuits](https://transluce.org/neuron-circuits) External Links: 2601.22594 Cited by: 2nd item.

[^5]: A convergence analysis of gradient descent for deep linear neural networks. External Links: 1810.02281 Cited by: §2.3.

[^6]: On the optimization of deep networks: implicit acceleration by overparameterization. In International conference on machine learning, pp. 244–253. Cited by: §2.1.

[^7]: Implicit regularization in deep matrix factorization. Advances in Neural Information Processing Systems 32. Cited by: §2.1.

[^8]: On exact computation with an infinitely wide neural net. Advances in neural information processing systems 32. Cited by: §2.1.

[^9]: Neural networks as kernel learners: the silent alignment effect. In International Conference on Learning Representations, Cited by: §2.1, §2.2.

[^10]: The optimization landscape of sgd across the feature learning strength. In The Thirteenth International Conference on Learning Representations, Cited by: §2.2, §2.2.

[^11]: Scaling and renormalization in high-dimensional regression. arXiv preprint arXiv:2405.00592. Cited by: §2.1.

[^12]: The committee machine: computational to statistical gaps in learning a two-layers neural network. Advances in Neural Information Processing Systems 31. Cited by: §2.1, §2.2.

[^13]: High-dimensional asymptotics of feature learning: how one gradient step improves the representation. Advances in Neural Information Processing Systems 35, pp. 37932–37946. Cited by: §2.1.

[^14]: Explaining neural scaling laws. Proceedings of the National Academy of Sciences 121 (27). External Links: ISSN 1091-6490, [Document](https://dx.doi.org/10.1073/pnas.2311878121) Cited by: §2.3.

[^15]: Statistical mechanics of deep learning. Annual review of condensed matter physics 11 (1), pp. 501–528. Cited by: §3.

[^16]: Neural networks and principal component analysis: learning from examples without local minima. Neural networks 2 (1), pp. 53–58. Cited by: §2.1.

[^17]: Revisiting model stitching to compare neural representations. Advances in neural information processing systems 34, pp. 225–236. Cited by: §2.5.

[^18]: The science of deep learning. Proceedings of the National Academy of Sciences 117 (48), pp. 30029–30032. Cited by: §3.

[^19]: Statistical physics of deep learning: optimal learning of a multi-layer perceptron near interpolation. arXiv preprint arXiv:2510.24616. Cited by: §2.2.

[^20]: Optimal errors and phase transitions in high-dimensional generalized linear models. Proceedings of the National Academy of Sciences 116 (12), pp. 5451–5460. Cited by: §2.1.

[^21]: On the origin of neural scaling laws: from random graphs to natural language. arXiv preprint arXiv:2601.10684. Cited by: §2.3.

[^22]: Implicit gradient regularization. arXiv preprint arXiv:2009.11162. Cited by: footnote 6.

[^23]: Large learning rates simultaneously achieve robustness to spurious correlations and compressibility. In Proceedings of the IEEE/CVF International Conference on Computer Vision, pp. 2055–2066. Cited by: §2.4.

[^24]: Deep learning: a statistical viewpoint. Acta numerica 30, pp. 87–201. Cited by: §3.

[^25]: Frequency bias in neural networks for input of non-uniform density. In International conference on machine learning, pp. 685–694. Cited by: §2.1.

[^26]: A unified theory of feature learning in rnns and dnns. arXiv preprint arXiv:2602.15593. Cited by: §2.2.

[^27]: Reconciling modern machine-learning practice and the classical bias–variance trade-off. Proceedings of the National Academy of Sciences 116 (32), pp. 15849–15854. Cited by: §2.1.

[^28]: Learning quadratic neural networks in high dimensions: sgd dynamics and scaling laws. arXiv preprint arXiv:2508.03688. Cited by: §2.1.

[^29]: High-dimensional limit theorems for sgd: effective dynamics and critical scaling. Advances in neural information processing systems 35, pp. 25349–25362. Cited by: §2.1.

[^30]: The geometry of abstraction in the hippocampus and prefrontal cortex. Cell 183 (4), pp. 954–967. Cited by: §3.

[^31]: Training large language models on narrow tasks can lead to broad misalignment. Nature 649 (8097), pp. 584–589. Cited by: §4.

[^32]: Learning single-index models with shallow neural networks. Advances in neural information processing systems 35, pp. 9768–9783. Cited by: §2.1.

[^33]: Birth of a transformer: a memory viewpoint. Advances in Neural Information Processing Systems 36, pp. 1560–1588. Cited by: §3.1.

[^34]: Implicit regularization for deep neural networks driven by an ornstein-uhlenbeck like process. In Conference on learning theory, pp. 483–513. Cited by: §2.4.

[^35]: The features at convergence theorem: a first-principles alternative to the neural feature ansatz for how networks learn representations. External Links: 2507.05644 Cited by: §2.3.

[^36]: Single-head attention in high dimensions: a theory of generalization, weights spectra, and scaling laws. In Workshop on Scientific Methods for Understanding Deep Learning, Cited by: §2.1.

[^37]: A dynamical model of neural scaling laws. arXiv preprint arXiv:2402.01092. Cited by: §2.3.

[^38]: How feature learning can improve neural scaling laws. In The Thirteenth International Conference on Learning Representations, Cited by: §2.1.

[^39]: Infinite limits of multi-head transformer dynamics. Advances in Neural Information Processing Systems 37, pp. 35824–35878. Cited by: §2.2, §2.2.

[^40]: Depthwise hyperparameter transfer in residual networks: dynamics and scaling limit. arXiv preprint arXiv:2309.16620. Cited by: §2.2, §2.4.

[^41]: Self-consistent dynamical field theory of kernel evolution in wide neural networks. Advances in Neural Information Processing Systems 35, pp. 32240–32256. Cited by: §2.2.

[^42]: Dynamics of finite width kernel and prediction fluctuations in mean field neural networks. Advances in Neural Information Processing Systems 36, pp. 9707–9750. Cited by: footnote 4.

[^43]: Deep linear network training dynamics from random initialization: data, width, depth, and hyperparameter transfer. arXiv preprint arXiv:2502.02531. Cited by: §2.2.

[^44]: Deep problems with neural network models of human vision. Behavioral and Brain Sciences 46, pp. e385. Cited by: §2.5.

[^45]: Towards monosemanticity: decomposing language models with dictionary learning. Transformer Circuits Thread. Note: https://transformer-circuits.pub/2023/monosemantic-features/index.html Cited by: 3rd item.

[^46]: Learning curves theory for hierarchically compositional data with power-law distributed features. arXiv preprint arXiv:2505.07067. Cited by: §2.5.

[^47]: How deep neural networks learn compositional data: the random hierarchy model. Physical Review X 14 (3), pp. 031001. Cited by: §2.5.

[^48]: Deriving neural scaling laws from the statistics of natural language. arXiv preprint arXiv:2602.07488. Cited by: §2.3.

[^49]: Spectral bias and task-model alignment explain generalization in kernel regression and infinitely wide neural networks. Nature communications 12 (1), pp. 2914. Cited by: §2.1.

[^50]: Optimal rates for the regularized least-squares algorithm. Foundations of Computational Mathematics 7 (3), pp. 331–368. Cited by: §2.1.

[^51]: Training dynamics impact post-training quantization robustness. arXiv preprint arXiv:2510.06213. Cited by: §2.4.

[^52]: Resnets of all shapes and sizes: convergence of training dynamics in the large-scale limit. arXiv preprint arXiv:2603.18168. Cited by: §2.2.

[^53]: Stochastic gradient descent performs variational inference, converges to limit cycles for deep networks. In 2018 Information Theory and Applications Workshop (ITA), pp. 1–10. Cited by: §2.4.

[^54]: Sudden drops in the loss: syntax acquisition, phase transitions, and simplicity bias in mlms. arXiv preprint arXiv:2309.07311. Cited by: §3.1.

[^55]: Stochastic collapse: how gradient noise attracts sgd dynamics towards simpler subnetworks. Advances in Neural Information Processing Systems 36. Cited by: §2.1, §2.4.

[^56]: Neural ordinary differential equations. Advances in neural information processing systems 31. Cited by: §2.2.

[^57]: On the global convergence of gradient descent for over-parameterized models using optimal transport. Advances in neural information processing systems 31. Cited by: §2.2.

[^58]: On lazy training in differentiable programming. Advances in neural information processing systems 32. Cited by: Figure 2, Figure 2, §2.1, §2.2, §2.2.

[^59]: The hidden width of deep resnets: tight error bounds and phase diagrams. arXiv preprint arXiv:2509.10167. Cited by: §2.2.

[^60]: Neural population geometry: an approach for understanding biological and artificial neural networks. Current opinion in neurobiology 70, pp. 137–144. Cited by: §3.

[^61]: Structure, disorder, and dynamics in task-trained recurrent neural circuits. bioRxiv, pp. 2026–03. Cited by: §2.2.

[^62]: Gradient descent on neural networks typically occurs at the edge of stability. arXiv preprint arXiv:2103.00065. Cited by: Figure 4, Figure 4, §2.3, §2.4.

[^63]: Understanding optimization in deep learning with central flows. External Links: 2410.24206 Cited by: §2.3, §2.4.

[^64]: Learning curves for overparametrized deep neural networks: a field theory perspective. Physical Review Research 3 (2), pp. 023034. Cited by: footnote 3.

[^65]: Towards automated circuit discovery for mechanistic interpretability. Advances in Neural Information Processing Systems 36, pp. 16318–16352. Cited by: 2nd item.

[^66]: Recurrent neural networks learn to store and generate sequences using non-linear representations. arXiv preprint arXiv:2408.10920. Cited by: 1st item.

[^67]: Generalization error rates in kernel regression: the crossover from the noiseless to noisy regime. Advances in Neural Information Processing Systems 34, pp. 10131–10143. Cited by: §2.3.

[^68]: Error scaling laws for kernel classification under source and capacity conditions. Machine Learning: Science and Technology 4 (3), pp. 035033. Cited by: §2.1.

[^69]: Asymptotics of feature learning in two-layer networks after one gradient-step. arXiv preprint arXiv:2402.04980. Cited by: §2.2.

[^70]: Sparse autoencoders find highly interpretable features in language models. arXiv preprint arXiv:2309.08600. Cited by: 3rd item.

[^71]: Label noise sgd provably prefers flat global minimizers. Advances in Neural Information Processing Systems 34, pp. 27449–27461. Cited by: §2.4.

[^72]: Self-stabilization: the implicit bias of gradient descent at the edge of stability. arXiv preprint arXiv:2209.15594. Cited by: §2.3.

[^73]: Neural networks can learn representations with gradient descent. In Conference on Learning Theory, pp. 5413–5452. Cited by: §2.1.

[^74]: How two-layer neural networks learn, one (giant) step at a time. arXiv preprint arXiv:2305.18270. Cited by: §2.1.

[^75]: The helmholtz machine. Neural computation 7 (5), pp. 889–904. Cited by: §3.

[^76]: Scaling laws and spectra of shallow neural networks in the feature learning regime. arXiv preprint arXiv:2509.24882. Cited by: §2.1, §2.2, §2.3.

[^77]: Don’t be lazy: completep enables compute-efficient deep transformers. arXiv preprint arXiv:2505.01618. Cited by: §2.2, §2.4.

[^78]: From lazy to rich: exact learning dynamics in deep linear networks. In The Thirteenth International Conference on Learning Representations, Cited by: §2.1.

[^79]: Data science at the singularity. Harvard Data Science Review 6 (1). Cited by: §3.

[^80]: Rosetta neurons: mining the common units in a model zoo. In Proceedings of the IEEE/CVF International Conference on Computer Vision, pp. 1934–1943. Cited by: §2.5.

[^81]: Algorithmic regularization in learning deep homogeneous models: layers are automatically balanced. Advances in neural information processing systems 31. Cited by: §2.3.

[^82]: Toy models of superposition. arXiv preprint arXiv:2209.10652. Cited by: §3.1.

[^83]: A mathematical framework for transformer circuits. Transformer Circuits Thread. Note: https://transformer-circuits.pub/2021/framework/index.html Cited by: §3.1.

[^84]: Not all language model features are one-dimensionally linear. arXiv preprint arXiv:2405.14860. Cited by: §3.1, §3.1.

[^85]: The nuclear route: sharp asymptotics of erm in overparameterized quadratic networks. arXiv preprint arXiv:2505.17958. Cited by: §2.1, §2.2.

[^86]: (S) gd over diagonal linear networks: implicit bias, large stepsizes and edge of stability. Advances in Neural Information Processing Systems 36, pp. 29406–29448. Cited by: §2.1, §2.3.

[^87]: The free-energy principle: a unified brain theory?. Nature reviews neuroscience 11 (2), pp. 127–138. Cited by: §3.

[^88]: Effect of batch learning in multilayer neural networks. Gen 1 (04), pp. 1E–03. Cited by: §2.1.

[^89]: The science of deep learning. Note: [https://www.cs.ox.ac.uk/people/yarin.gal/website/blog\_5058.html](https://www.cs.ox.ac.uk/people/yarin.gal/website/blog_5058.html) Cited by: §3.

[^90]: The space of interactions in neural network models. Journal of physics A: Mathematical and general 21 (1), pp. 257–270. Cited by: §3.

[^91]: On the similarity between the laplace and neural tangent kernels. Advances in Neural Information Processing Systems 33, pp. 1451–1461. Cited by: §2.1.

[^92]: Causal abstraction: a theoretical foundation for mechanistic interpretability. Journal of Machine Learning Research 26 (83), pp. 1–64. Cited by: footnote 11.

[^93]: Disentangling feature and lazy training in deep neural networks. Journal of Statistical Mechanics: Theory and Experiment 2020 (11), pp. 113301. Cited by: §2.2.

[^94]: When do neural networks outperform kernel methods?. Advances in Neural Information Processing Systems 33, pp. 14820–14830. Cited by: §2.1.

[^95]: Understanding the mechanisms of fast hyperparameter transfer. arXiv preprint arXiv:2512.22768. Cited by: §2.4.

[^96]: Implicit regularization of discrete gradient dynamics in linear neural networks. Advances in Neural Information Processing Systems 32. Cited by: §2.1.

[^97]: The implicit bias of depth: how incremental learning drives generalization. arXiv preprint arXiv:1909.12051. Cited by: §2.1.

[^98]: Propagation of chaos in one-hidden-layer neural networks beyond logarithmic time. arXiv preprint arXiv:2504.13110. Cited by: footnote 4.

[^99]: Dynamics of stochastic gradient descent for two-layer neural networks in the teacher-student setup. Advances in neural information processing systems 32. Cited by: §2.1, §2.2.

[^100]: Abrupt learning in transformers: a case study on matrix completion. Advances in Neural Information Processing Systems 37, pp. 55053–55085. Cited by: §3.1.

[^101]: Accurate, large minibatch sgd: training imagenet in 1 hour. arXiv preprint arXiv:1706.02677. Cited by: §2.4.

[^102]: Towards worst-case guarantees with scale-aware interpretability. arXiv preprint arXiv:2602.05184. Cited by: §3.1.

[^103]: Revisiting the platonic representation hypothesis: an aristotelian view. arXiv preprint arXiv:2602.14486. Cited by: §2.5.

[^104]: Grokking modular arithmetic. arXiv preprint arXiv:2301.02679. Cited by: §2.1.

[^105]: Implicit bias of gradient descent on linear convolutional networks. Advances in neural information processing systems 31. Cited by: §2.1.

[^106]: Shampoo: preconditioned stochastic tensor optimization. External Links: 1802.09568, [Document](https://dx.doi.org/10.48550/arXiv.1802.09568) Cited by: §4.

[^107]: When models manipulate manifolds: the geometry of a counting task. Transformer Circuits Thread. External Links: [Link](https://transformer-circuits.pub/2025/linebreaks/index.html) Cited by: §3.1.

[^108]: A rainbow in deep network black boxes. Journal of Machine Learning Research 25 (350), pp. 1–59. Cited by: §2.5.

[^109]: Global universality of singular values in products of many large random matrices. arXiv preprint arXiv:2503.07872. Cited by: §2.2.

[^110]: Finite depth and width corrections to the neural tangent kernel. arXiv preprint arXiv:1909.05989. Cited by: §2.2, footnote 4.

[^111]: Duality of bures and shape distances with implications for comparing neural representations. In Proceedings of UniReps: the First Workshop on Unifying Representations in Neural Models, pp. 11–26. Cited by: §2.5.

[^112]: Surprises in high-dimensional ridgeless least squares interpolation. The Annals of Statistics 50 (2), pp. 949–986. Cited by: §2.1.

[^113]: Width and depth limits commute in residual networks. In International Conference on Machine Learning, pp. 12700–12723. Cited by: §2.2.

[^114]: A proof of learning rate transfer under $mu$ p. arXiv preprint arXiv:2511.01734. Cited by: §2.4.

[^115]: Deep learning scaling is predictable, empirically. External Links: 1712.00409 Cited by: §2.3.

[^116]: Training compute-optimal large language models. arXiv preprint arXiv:2203.15556. Cited by: §2.2.

[^117]: Towards developmental interpretability. Note: LessWrongAccessed: 2026-02-03 External Links: [Link](https://www.lesswrong.com/posts/TjaeCWvLZtEDAS5Ex/towards-developmental-interpretability) Cited by: §3, §3.1.

[^118]: Loss landscape degeneracy and stagewise development in transformers. Transactions on Machine Learning Research. Cited by: §3.1.

[^119]: Neural networks and physical systems with emergent collective computational abilities.. Proceedings of the national academy of sciences 79 (8), pp. 2554–2558. Cited by: §3.

[^120]: Infinite attention: nngp and ntk for deep attention networks. In International Conference on Machine Learning, pp. 4376–4386. Cited by: §2.2.

[^121]: Position: the platonic representation hypothesis. In Forty-first International Conference on Machine Learning, Cited by: Figure 6, Figure 6, §2.5.

[^122]: Identifiability of latent-variable and structural-equation models: from linear to nonlinear. Annals of the Institute of Statistical Mathematics 76 (1), pp. 1–33. Cited by: §2.5.

[^123]: Neural tangent kernel: convergence and generalization in neural networks. Advances in neural information processing systems 31. Cited by: §2.1, §2.1, §2.2.

[^124]: Saddle-to-saddle dynamics in deep linear networks: small initialization training, symmetry, and sparsity. arXiv preprint arXiv:2106.15933. Cited by: §2.1.

[^125]: Kernel alignment risk estimator: risk prediction from training data. Advances in neural information processing systems 33, pp. 15568–15578. Cited by: §2.1.

[^126]: Parallelizing stochastic gradient descent for least squares regression: mini-batching, averaging, and model misspecification. Journal of machine learning research 18 (223), pp. 1–42. Cited by: §2.4.

[^127]: Three factors influencing minima in sgd. arXiv preprint arXiv:1711.04623. Cited by: §2.2, §2.4, §2.4.

[^128]: The break-even point on optimization trajectories of deep neural networks. arXiv preprint arXiv:2002.09572. Cited by: §2.4.

[^129]: Hyperparameter transfer with mixture-of-expert layers. arXiv preprint arXiv:2601.20205. Cited by: §2.2.

[^130]: On the origins of linear representations in large language models. arXiv preprint arXiv:2403.03867. Cited by: 1st item.

[^131]: Muon: an optimizer for hidden layers in neural networks. External Links: [Link](https://kellerjordan.github.io/posts/muon/) Cited by: §4.

[^132]: Causality is key for interpretability claims to generalise. arXiv preprint arXiv:2602.16698. Cited by: §3.1.

[^133]: Generalization in diffusion models arises from geometry-adaptive harmonic representations. In The Twelfth International Conference on Learning Representations, Cited by: §2.5.

[^134]: Sgd on neural networks learns functions of increasing complexity. Advances in neural information processing systems 32. Cited by: §2.1.

[^135]: An analytic theory of creativity in convolutional diffusion models. In Forty-second International Conference on Machine Learning, Cited by: §2.5.

[^136]: Scaling laws for neural language models. External Links: 2001.08361 Cited by: Figure 3, Figure 3, §2.3.

[^137]: Symmetry in language statistics shapes the geometry of model representations. arXiv preprint arXiv:2602.15029. Cited by: §2.5, §3.1.

[^138]: Predicting kernel regression learning curves from only raw data statistics. arXiv preprint arXiv:2510.14878. Cited by: §2.1.

[^139]: On large-batch training for deep learning: generalization gap and sharp minima. arXiv preprint arXiv:1609.04836. Cited by: §2.4.

[^140]: Privileged representational axes in biological and artificial neural networks. bioRxiv, pp. 2024–06. Cited by: §2.5.

[^141]: Similarity of neural network models: a survey of functional and representational measures. ACM Computing Surveys 57 (9), pp. 1–52. Cited by: §2.5.

[^142]: Towards nonlinear disentanglement in natural data with temporal sparse coding. arXiv preprint arXiv:2007.10930. Cited by: §2.5.

[^143]: Similarity of neural network representations revisited. In International conference on machine learning, pp. 3519–3529. Cited by: §2.5.

[^144]: Representational similarity analysis-connecting the branches of systems neuroscience. Frontiers in systems neuroscience 2, pp. 249. Cited by: §3.

[^145]: Alternating gradient flows: a theory of feature learning in two-layer neural networks. arXiv preprint arXiv:2506.06489. Cited by: §2.1, §3.1.

[^146]: Get rich quick: exact solutions reveal how unbalanced initializations promote rapid feature learning. Advances in Neural Information Processing Systems 37, pp. 81157–81203. Cited by: §2.1.

[^147]: Neural mechanics: symmetry and broken conservation laws in deep learning dynamics. In International Conference on Learning Representations, Cited by: §2.3.

[^148]: An analytic theory of generalization dynamics and transfer learning in deep linear networks. arXiv preprint arXiv:1809.10374. Cited by: §2.1.

[^149]: Machine learning as an experimental science. Machine Learning 3 (1), pp. 5–8. Cited by: §3.

[^150]: Towards understanding inductive bias in transformers: a view from infinity. arXiv preprint arXiv:2402.05173. Cited by: footnote 3.

[^151]: Gradient-based learning applied to document recognition. Proceedings of the IEEE 86 (11), pp. 2278–2324. External Links: [Document](https://dx.doi.org/10.1109/5.726791) Cited by: §2.2.

[^152]: Deep neural networks as gaussian processes. arXiv preprint arXiv:1711.00165. Cited by: footnote 3.

[^153]: Wide neural networks of any depth evolve as linear models under gradient descent. Advances in neural information processing systems 32. Cited by: §2.1, §2.2.

[^154]: Understanding image representations by measuring their equivariance and equivalence. In Proceedings of the IEEE conference on computer vision and pattern recognition, pp. 991–999. Cited by: §2.5.

[^155]: Break it down: evidence for structural compositionality in neural networks. Advances in Neural Information Processing Systems 36, pp. 42623–42660. Cited by: 4th item.

[^156]: Visualizing the loss landscape of neural nets. In Advances in Neural Information Processing Systems, S. Bengio, H. Wallach, H. Larochelle, K. Grauman, N. Cesa-Bianchi, and R. Garnett (Eds.), Vol. 31, pp.. Cited by: §2.3.

[^157]: The neural covariance sde: shaped infinite depth-and-width networks at initialization. Advances in Neural Information Processing Systems 35, pp. 10795–10808. Cited by: §2.2.

[^158]: Stochastic modified equations and dynamics of stochastic gradient algorithms i: mathematical foundations. Journal of Machine Learning Research 20 (40), pp. 1–47. Cited by: §2.4.

[^159]: Statistical mechanics of deep linear neural networks: the backpropagating kernel renormalization. Physical Review X 11 (3), pp. 031059. Cited by: §2.2.

[^160]: Zipf’s law everywhere.. Glottometrics 5 (2002), pp. 14–21. Cited by: §2.5.

[^161]: Adam reduces a unique form of sharpness: theoretical insights near the minimizer manifold. arXiv preprint arXiv:2511.02773. Cited by: §2.4.

[^162]: Convergent learning: do different neural networks learn the same representations?. In Feature Extraction: Modern Questions and Challenges, pp. 196–212. Cited by: §2.5.

[^163]: Towards resolving the implicit bias of gradient descent for matrix factorization: greedy low-rank learning. In International Conference on Learning Representations, Cited by: §2.1.

[^164]: On the validity of modeling sgd with stochastic differential equations (sdes). External Links: 2102.12470 Cited by: §2.4.

[^165]: What happens after sgd reaches zero loss?–a mathematical framework. arXiv preprint arXiv:2110.06914. Cited by: §2.4.

[^166]: On the biology of a large language model. Transformer Circuits Thread. External Links: [Link](https://transformer-circuits.pub/2025/attribution-graphs/biology.html) Cited by: §3.1.

[^167]: On the linearity of large non-linear models: when and why the tangent kernel is constant. Advances in Neural Information Processing Systems 33, pp. 15954–15964. Cited by: §2.1.

[^168]: Superposition yields robust neural scaling. External Links: 2505.10465 Cited by: §2.3.

[^169]: A convnet for the 2020s. In Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, pp. 11976–11986. Cited by: §2.5.

[^170]: Learning curves of generic features maps for realistic datasets with a teacher-student model. Advances in Neural Information Processing Systems 34, pp. 18137–18151. Cited by: §2.1.

[^171]: Gradient descent maximizes the margin of homogeneous neural networks. In International Conference on Learning Representations, Cited by: §2.1.

[^172]: The power of interpolation: understanding the effectiveness of sgd in modern over-parametrized learning. In International Conference on Machine Learning, pp. 3325–3334. Cited by: §2.4.

[^173]: Gradient descent quantizes relu network features. arXiv preprint arXiv:1803.08367. Cited by: §2.2.

[^174]: Bayes-optimal learning of an extensive-width neural network from quadratically many samples. Advances in Neural Information Processing Systems 37, pp. 82085–82132. Cited by: §2.2.

[^175]: $\mu$ -Parametrization for mixture of experts. arXiv preprint arXiv:2508.09752. External Links: 2508.09752 Cited by: §2.2.

[^176]: On the sdes and scaling rules for adaptive gradient algorithms. In Advances in Neural Information Processing Systems, S. Koyejo, S. Mohamed, A. Agarwal, D. Belgrave, K. Cho, and A. Oh (Eds.), Vol. 35, pp. 7697–7711. External Links: [Link](https://proceedings.neurips.cc/paper_files/paper/2022/file/32ac710102f0620d0f28d5d05a44fe08-Paper-Conference.pdf) Cited by: §2.4.

[^177]: A kernel-based view of language model fine-tuning. In International Conference on Machine Learning, pp. 23610–23641. Cited by: §2.1.

[^178]: A wavelet tour of signal processing. Elsevier. Cited by: §2.5.

[^179]: Stochastic gradient descent as approximate bayesian inference. Journal of Machine Learning Research 18 (134), pp. 1–35. Cited by: §2.2, §2.4.

[^180]: Sequential group composition: a window into the mechanics of deep learning. arXiv preprint arXiv:2602.03655. External Links: 2602.03655 Cited by: §3.1.

[^181]: Abide by the law and follow the flow: conservation laws for gradient flows. External Links: 2307.00144 Cited by: §2.3.

[^182]: Keep the momentum: conservation laws beyond euclidean gradient flows. External Links: 2405.12888 Cited by: §2.3.

[^183]: The geometry of truth: emergent linear structure in large language model representations of true/false datasets. arXiv preprint arXiv:2310.06824. Cited by: 1st item.

[^184]: Vision: a computational investigation into the human representation and processing of visual information. MIT press. Cited by: footnote 14.

[^185]: On the impact of overparameterization on the training of a shallow neural network in high dimensions. In International Conference on Artificial Intelligence and Statistics, pp. 3655–3663. Cited by: §2.2.

[^186]: An empirical model of large-batch training. arXiv preprint arXiv:1812.06162. Cited by: §2.4.

[^187]: Deep learning models of the retinal response to natural scenes. Advances in neural information processing systems 29. Cited by: §2.5.

[^188]: Mean-field theory of two-layers neural networks: dimension-free bounds and kernel limit. In Conference on learning theory, pp. 2388–2464. Cited by: §2.2.

[^189]: A mean field view of the landscape of two-layer neural networks. Proceedings of the National Academy of Sciences 115 (33), pp. E7665–E7671. Cited by: §2.2.

[^190]: Locating and editing factual associations in gpt. Advances in neural information processing systems 35, pp. 17359–17372. Cited by: 2nd item.

[^191]: A physics of systems that learn. External Links: [Link](https://ericjmichaud.com/physics-of-learning.pdf) Cited by: §3.

[^192]: The quantization model of neural scaling. Advances in Neural Information Processing Systems 36, pp. 28699–28722. Cited by: §2.3.

[^193]: Dynamical mean-field theory for stochastic gradient descent in gaussian mixture classification. Advances in Neural Information Processing Systems 33, pp. 9540–9550. Cited by: §2.1.

[^194]: Linguistic regularities in continuous space word representations. In Proceedings of the 2013 conference of the north american chapter of the association for computational linguistics: Human language technologies, pp. 746–751. Cited by: 1st item.

[^195]: A theory of non-linear feature learning with one gradient step in two-layer neural networks. arXiv preprint arXiv:2310.07891. Cited by: §2.2.

[^196]: Phase transitions for feature learning in neural networks. arXiv preprint arXiv:2602.01434. Cited by: §2.2.

[^197]: Feature emergence via margin maximization: case studies in algebraic tasks. arXiv preprint arXiv:2311.07568. Cited by: §2.1, §3.1.

[^198]: Relative representations enable zero-shot latent space communication. arXiv preprint arXiv:2209.15430. Cited by: §2.5.

[^199]: Position: solve layerwise linear models first to understand neural dynamical phenomena (neural collapse, emergence, lazy/rich regime, and grokking). arXiv preprint arXiv:2502.21009. Cited by: §2.1.

[^200]: Progress measures for grokking via mechanistic interpretability. arXiv preprint arXiv:2301.05217. Cited by: §3.1, §3.1.

[^201]: Emergent linear representations in world models of self-supervised sequence models. In Proceedings of the 6th BlackboxNLP Workshop: Analyzing and Interpreting Neural Networks for NLP, pp. 16–30. Cited by: 1st item.

[^202]: Priors for infinite networks. In Bayesian Learning for Neural Networks, pp. 29–53. Cited by: §2.2.

[^203]: How transformers learn causal structure with gradient descent. In Proceedings of the 41st International Conference on Machine Learning, pp. 38018–38070. Cited by: §3.1.

[^204]: Understanding factual recall in transformers via associative memories. In The Thirteenth International Conference on Learning Representations, Cited by: §2.1.

[^205]: Towards a mechanistic explanation of diffusion model generalization. In Forty-second International Conference on Machine Learning, Cited by: §2.5.

[^206]: The shaped transformer: attention models in the infinite depth-and-width limit. Advances in Neural Information Processing Systems 36, pp. 54250–54281. Cited by: §2.2.

[^207]: Super consistency of neural network landscapes and learning rate transfer. Advances in Neural Information Processing Systems 37, pp. 102696–102743. Cited by: §2.4.

[^208]: Zoom in: an introduction to circuits. Distill 5 (3), pp. e00024–001. Cited by: §3.1.

[^209]: Emergence of simple-cell receptive field properties by learning a sparse code for natural images. Nature 381 (6583), pp. 607–609. Cited by: §2.5, §2.5, §3.

[^210]: In-context learning and induction heads. Transformer Circuits Thread. Note: https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html Cited by: §3.1.

[^211]: Prevalence of neural collapse during the terminal phase of deep learning training. Proceedings of the National Academy of Sciences 117 (40), pp. 24652–24663. External Links: [Document](https://dx.doi.org/10.1073/pnas.2015509117), https://www.pnas.org/doi/pdf/10.1073/pnas.2015509117 Cited by: §2.3.

[^212]: Emergence of hidden capabilities: exploring learning dynamics in concept space. Advances in Neural Information Processing Systems 37, pp. 84698–84729. Cited by: §3.1.

[^213]: The linear representation hypothesis and the geometry of large language models. arXiv preprint arXiv:2311.03658. Cited by: 1st item.

[^214]: Trak: attributing model behavior at scale. arXiv preprint arXiv:2303.14186. Cited by: §4.

[^215]: Saddle-to-saddle dynamics in diagonal linear networks. Advances in Neural Information Processing Systems 36, pp. 7475–7505. Cited by: §2.1.

[^216]: Implicit bias of sgd for diagonal linear networks: a provable benefit of stochasticity. Advances in Neural Information Processing Systems 34, pp. 29218–29230. Cited by: §2.1, §2.4.

[^217]: Zipf’s word frequency law in natural language: a critical review and future directions. Psychonomic bulletin & review 21 (5), pp. 1112–1130. Cited by: §2.5.

[^218]: Statistical optimality of stochastic gradient descent on hard learning problems through multiple passes. Advances in Neural Information Processing Systems 31. Cited by: §2.1.

[^219]: Exponential expressivity in deep neural networks through transient chaos. In Advances in Neural Information Processing Systems (NeurIPS), pp. 3360–3368. Cited by: §2.2.

[^220]: Correlations in the data lead to semantically rich feature geometry under superposition. In Mechanistic Interpretability Workshop at NeurIPS 2025, Cited by: §3.1.

[^221]: Mechanism for feature learning in neural networks and backpropagation-free machine learning models. Science 383 (6690), pp. 1461–1467. External Links: [Document](https://dx.doi.org/10.1126/science.adi5639), https://www.science.org/doi/pdf/10.1126/science.adi5639 Cited by: §2.3.

[^222]: Svcca: singular vector canonical correlation analysis for deep learning dynamics and interpretability. Advances in neural information processing systems 30. Cited by: §2.5.

[^223]: Random features for large-scale kernel machines. Advances in neural information processing systems 20. Cited by: §2.5.

[^224]: Let’s take machine learning from alchemy to electricity: test‑of‑time award presentation. Note: Presentation at the 31st Conference on Neural Information Processing Systems (NIPS 2017), Long Beach, CA, USATest‑of‑Time Award talk Cited by: §3.

[^225]: Compositional capabilities of autoregressive transformers: a study on synthetic, interpretable tasks. arXiv preprint arXiv:2311.12997. Cited by: 4th item.

[^226]: The mechanistic basis of data dependence and abrupt learning in an in-context classification task. arXiv preprint arXiv:2312.03002. Cited by: §3.1.

[^227]: Position: an empirically grounded identifiability theory will accelerate self-supervised learning research. arXiv preprint arXiv:2504.13101. Cited by: §2.5.

[^228]: Cross-entropy is all you need to invert the data generating process. arXiv preprint arXiv:2410.21869. Cited by: §2.5.

[^229]: Learning dynamics of LLM finetuning. In The Thirteenth International Conference on Learning Representations, Cited by: §2.1.

[^230]: Emergence and scaling laws in sgd learning of shallow neural networks. External Links: 2504.19983 Cited by: §2.1, §2.2, §2.3.

[^231]: Applications of statistical field theory in deep learning. arXiv preprint arXiv:2502.18553. Cited by: §3.

[^232]: The principles of deep learning theory. Vol. 46, Cambridge University Press Cambridge, MA, USA. Cited by: footnote 4.

[^233]: Parameters as interacting particles: long time convergence and asymptotic error scaling of neural networks. Advances in neural information processing systems 31. Cited by: §2.2.

[^234]: Mitigating the curse of detail: scaling arguments for feature learning and sample complexity. arXiv preprint arXiv:2512.04165. Cited by: footnote 3.

[^235]: From kernels to features: a multi-scale adaptive theory of feature learning. arXiv preprint arXiv:2502.03210. Cited by: footnote 3.

[^236]: Grokking as a first order phase transition in two layer networks. arXiv preprint arXiv:2310.03789. Cited by: footnote 3.

[^237]: Exact solution for on-line learning in multilayer neural networks. Physical Review Letters 74 (21), pp. 4337–4340. Cited by: §2.1, §2.2.

[^238]: Mechanistic?. In Proceedings of the 7th BlackboxNLP Workshop: Analyzing and Interpreting Neural Networks for NLP, pp. 480–498. Cited by: footnote 12.

[^239]: Interpretability creationism. Note: Blog postAccessed: 2026-02-03 External Links: [Link](https://nsaphra.net/post/creationism/) Cited by: §3.1.

[^240]: Exact solutions to the nonlinear dynamics of learning in deep linear neural networks. In Proceedings of the International Conference on Learning Represenatations 2014, Cited by: Figure 1, Figure 1, §2.1, §2.1, §2.3.

[^241]: Deep linear neural networks: a theory of learning in the brain and mind. Stanford University. Cited by: §2.2.

[^242]: Discovering modular solutions that generalize compositionally. arXiv preprint arXiv:2312.15001. Cited by: 4th item.

[^243]: LoRA without regret. Thinking Machines Lab: Connectionism. Note: https://thinkingmachines.ai/blog/lora/ External Links: [Document](https://dx.doi.org/10.64434/tml.20250929) Cited by: §2.4.

[^244]: A phase transition in diffusion models reveals the hierarchical nature of data. Proceedings of the National Academy of Sciences 122 (1), pp. e2408799121. Cited by: §2.5.

[^245]: Unified field theoretical approach to deep and recurrent neuronal networks. Journal of Statistical Mechanics: Theory and Experiment 2022 (10), pp. 103401. Cited by: footnote 4.

[^246]: Separation of scales and a thermodynamic description of feature learning in some cnns. Nature Communications 14 (1), pp. 908. Cited by: footnote 3.

[^247]: Statistical mechanics of learning from examples. Physical review A 45 (8), pp. 6056. Cited by: §2.2.

[^248]: Transformers represent belief state geometry in their residual stream. Advances in Neural Information Processing Systems 37, pp. 75012–75034. Cited by: §3.1.

[^249]: Measuring the effects of data parallelism on neural network training. Journal of Machine Learning Research 20 (112), pp. 1–49. Cited by: §2.4.

[^250]: Open problems in mechanistic interpretability. arXiv preprint arXiv:2501.16496. Cited by: §3.1.

[^251]: Scaling laws from the data manifold dimension. Journal of Machine Learning Research 23 (9), pp. 1–34. Cited by: §2.3.

[^252]: Opening the black box of deep neural networks via information. arXiv preprint arXiv:1703.00810. Cited by: §3.

[^253]: The eigenlearning framework: a conservation law perspective on kernel ridge regression and wide neural networks. Transactions on Machine Learning Research. Cited by: Figure 1, Figure 1, §2.1.

[^254]: On the stepwise nature of self-supervised learning. In International Conference on Machine Learning, pp. 31852–31876. Cited by: §2.1.

[^255]: Convnets match vision transformers at scale. arXiv preprint arXiv:2310.16764. Cited by: §2.5.

[^256]: On the origin of implicit regularization in stochastic gradient descent. arXiv preprint arXiv:2101.12176. Cited by: footnote 6.

[^257]: Tensor product variable binding and the representation of symbolic structures in connectionist systems. Artificial intelligence 46 (1-2), pp. 159–216. Cited by: 4th item.

[^258]: The implicit bias of gradient descent on separable data. The Journal of Machine Learning Research 19 (1), pp. 2822–2878. Cited by: footnote 5.

[^259]: The implicit bias of gradient descent on separable data. Journal of Machine Learning Research 19 (70), pp. 1–57. Cited by: §2.1.

[^260]: Getting aligned on representational alignment. arXiv preprint arXiv:2310.13018. Cited by: §2.5.

[^261]: Noether’s learning dynamics: role of symmetry breaking in neural networks. Advances in Neural Information Processing Systems 34, pp. 25646–25660. Cited by: §2.3.

[^262]: Understanding the dynamics of gradient flow in overparameterized linear models. In International Conference on Machine Learning, pp. 10153–10161. Cited by: §2.1.

[^263]: Scaling monosemanticity: extracting interpretable features from claude 3 sonnet. Transformer Circuits Thread. External Links: [Link](https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html) Cited by: §3.1.

[^264]: Local vs. distributed coding. Intellectica 8 (2), pp. 3–40. Cited by: 4th item.

[^265]: Phase diagram of stochastic gradient descent in high-dimensional two-layer neural networks. Advances in Neural Information Processing Systems 35, pp. 23244–23255. Cited by: §2.1.

[^266]: Limitations of the ntk for understanding generalization in deep learning. arXiv preprint arXiv:2206.10012. Cited by: §2.1.

[^267]: Interpretability in the wild: a circuit for indirect object identification in gpt-2 small. arXiv preprint arXiv:2211.00593. Cited by: 2nd item.

[^268]: Implicit bias of sgd in $L\_2$ -regularized linear dnns: one-way jumps from high to low rank. In The Twelfth International Conference on Learning Representations, Cited by: §2.1.

[^269]: Algebraic geometry and statistical learning theory. Vol. 25, Cambridge university press. Cited by: §3.

[^270]: More than a toy: random matrix models predict how real-world neural representations generalize. In International conference on machine learning, pp. 23549–23588. Cited by: §2.1.

[^271]: How does sharpness-aware minimization minimize sharpness?. arXiv preprint arXiv:2211.05729. Cited by: §2.4.

[^272]: Equivalence between representational similarity analysis, centered kernel alignment, and canonical correlations analysis. In Proceedings of UniReps: the Second Edition of the Workshop on Unifying Representations in Neural Models, pp. 10–23. Cited by: §2.5.

[^273]: Position: deep learning is not so mysterious or different. In Forty-second International Conference on Machine Learning Position Paper Track, Cited by: §3.

[^274]: The lack of a priori distinctions between learning algorithms. Neural computation 8 (7), pp. 1341–1390. Cited by: §2.5.

[^275]: Kernel and rich regimes in overparametrized models. In Conference on Learning Theory, pp. 3635–3673. Cited by: §2.1, §2.2.

[^276]: Information-theoretic analysis of generalization capability of learning algorithms. Advances in neural information processing systems 30. Cited by: §3.

[^277]: Performance-optimized hierarchical models predict neural responses in higher visual cortex. Proceedings of the national academy of sciences 111 (23), pp. 8619–8624. Cited by: §2.5.

[^278]: A theory of representation learning gives a deep generalisation of kernel methods. In International Conference on Machine Learning, pp. 39380–39415. Cited by: footnote 3.

[^279]: Tensor programs v: tuning large neural networks via zero-shot hyperparameter transfer. arXiv preprint arXiv:2203.03466. Cited by: Figure 5, Figure 5, §2.4, footnote 15.

[^280]: Tensor programs iv: feature learning in infinite-width neural networks. In International Conference on Machine Learning, pp. 11727–11737. Cited by: §2.2, §2.4.

[^281]: Tensor programs ivb: adaptive optimization in the infinite-width limit. arXiv preprint arXiv:2308.01814. Cited by: §2.4.

[^282]: Tensor programs vi: feature learning in infinite-depth neural networks. arXiv preprint arXiv:2310.02244. Cited by: §2.2, §2.4.

[^283]: Understanding sharpness dynamics in nn training with a minimalist example: the effects of dataset difficulty, depth, stochasticity, and more. In International Conference on Machine Learning, pp. 72574–72617. Cited by: §2.3.

[^284]: Summary statistics of learning link changing neural representations to behavior. Frontiers in Neural Circuits 19, pp. 1618351. Cited by: §2.1.

[^285]: Asymptotics of representation learning in finite bayesian neural networks. Advances in neural information processing systems 34, pp. 24765–24777. Cited by: footnote 4.

[^286]: Statistical physics of inference: thresholds and algorithms. Advances in Physics 65 (5), pp. 453–552. Cited by: §2.2.

[^287]: Understanding deep learning is also a job for physicists. Nature Physics 16 (6), pp. 602–604. Cited by: §3.

[^288]: Visualizing and understanding convolutional networks. In European Conference on Computer Vision, pp. 818–833. Cited by: §3.

[^289]: The emergence of reproducibility and consistency in diffusion models. In Forty-first International Conference on Machine Learning, Cited by: Figure 6, Figure 6, §2.5.

[^290]: Training dynamics of in-context learning in linear attention. arXiv preprint arXiv:2501.16265. Cited by: §2.1.

[^291]: A geometric analysis of neural collapse with unconstrained features. Advances in Neural Information Processing Systems 34, pp. 29820–29834. Cited by: §2.3.

[^292]: Contrastive learning inverts the data generating process. In International conference on machine learning, pp. 12979–12990. Cited by: §2.5.

[^293]: Formation of representations in neural networks. arXiv preprint arXiv:2410.03006. Cited by: §2.3.

[^294]: Proof of a perfect platonic representation hypothesis. arXiv preprint arXiv:2507.01098. Cited by: §2.5.

[^295]: Exact solutions of a deep linear network. Advances in Neural Information Processing Systems 35, pp. 24446–24458. Cited by: §2.1.