---
title: "Engineers as philosophers, Part 3"
source: "https://realizable.substack.com/p/engineers-as-philosophers-part-3?utm_source=profile&utm_medium=reader2"
author:
  - "[[Maxim Raginsky]]"
published: 2023-08-24
created: 2026-04-27
description: "The New Aristotelians."
tags:
  - "clippings"
---
The usual story goes that, until Newton came onto the stage, scientific explanation rested on [Aristotle’s](https://plato.stanford.edu/entries/aristotle-causality/) *[four causes](https://plato.stanford.edu/entries/aristotle-causality/)*:

- material cause — something is the way it is because of the material substance making it up;
- efficient cause — something is the way it is because of the effort and energy expended to make it the way it is;
- formal cause — something is the way it is because of its structure and the context in which it finds itself;
- final cause — something is the way it is because of someone’s (or even its own) will or desire or goal, *telos.*

Newton’s mechanistic universe, so the story goes, needs no final cause,[^1] since everything is explained in terms of particles (material causes), forces acting at a distance (efficient causes), and the structure of the physical law (formal causes). With just one incantation, *hypotheses non fingo*, the [Last Magician](https://www.neh.gov/humanities/2011/januaryfebruary/feature/newton-the-last-magician) has banished final causes, and thus teleology, from the realm of scientific explanation.

But that’s physics, and physics deals with the natural. Engineering, on the other hand, is a science of the *artificial*, and it would be downright strange to insist that engineering artifacts have no purpose or telos. Even (some) physicists are forced to admit this; here is what [John Bechhoefer](https://www.sfu.ca/physics/people/faculty/johnb.html) writes in his book *[Control Theory for Physicists](https://www.sfu.ca/chaos/book.html)*, which I highly recommend:

> What is different about control theory? In a word, *purpose.* Control always has a goal, and control theory shows how to achieve that goal. By contrast, physical theories describe nature, and historically physicists have gone to great lengths to remove the subjective from their theories. Thus, to study control theory as a physicist is a bit like licking the forbidden fruit. We know we are not supposed to do it, but it does seem interesting. I hope to assuage such feelings of guilt, as much in the natural world is designed, either explicitly by people, or implicitly through evolution and natural selection. And even if you do not share this way of looking at the world, learning about a great idea and its implications is worthy in itself, and the contrast can teach you much about “nonpurposeful” dynamics.

The crucial word here is *control*. The shift from the closed systems of Newton’s mechanics to open systems that interact with their environment is highly consequential: A closed dynamical system of the form

$\frac{d x_{1}}{d t} = f_{1} \left(\right. x_{1} , \ldots , x_{n} \left.\right) , \ldots , \frac{d x_{n}}{d t} = f_{n} \left(\right. x_{1} , \ldots , x_{n} \left.\right)$

chugs along subject to the iron law of [Laplacian determinism](https://en.wikipedia.org/wiki/Laplace%27s_demon), its future completely determined by its past. On the other hand, the future trajectory of an open system like

$\frac{d x_{1}}{d t} = f_{1} \left(\right. x_{1} , \ldots , x_{n} , u_{1} , \ldots , u_{m} \left.\right) , \ldots , \frac{d x_{n}}{d t} = f_{n} \left(\right. x_{1} , \ldots , x_{n} , u_{1} , \ldots , u_{m} \left.\right)$

can be steered (or controlled) by appropriately choosing the inputs (or controls) *u* (*t*). It is natural to think here in terms of [counterfactuals](https://academic.oup.com/book/4324): Situation A will happen if we choose one set of controls, but some other situation B will obtain if we go with another set of controls. And once we recognize this, we can start talking about *optimal control*, i.e., the possibility of choosing the controls (or *efficient causes*) so that some global objective (*final cause*) is minimized.

Interestingly, even though state-space models of the above form are now ubiquitous in the theory of control systems (thanks in no small part to the revolutionary work of Rudolf Kalman), Norbert Wiener’s 1948 *Cybernetics* made no use of them and phrased things instead in terms of externalist input-output descriptions given by convolution integrals (in the linear case) or Volterra series (in the nonlinear case). However, the importance of state-space models with control inputs was recognized as early as 1945 (three years before Wiener’s book!) by [W. Ross Ashby](https://archives.library.illinois.edu/thought-collective/cyberneticians/w-ross-ashby/), a pioneer of British cybernetics,[^2] who referred to the change of control inputs (or parameters, as he called them) as a change of system’s *organization*.[^3]

The main idea here is that final causes are located at a higher level of abstraction compared to efficient causes. The theory of [optimal control](http://liberzon.csl.illinois.edu/teaching/cvoc.pdf) deals with global optimization in an infinite-dimensional function space, but, once a suitable control is found, all the work is done locally through efficient causation. Final causes belong to functional theories, whereas efficient causes operate at a lower level described by physical theory. This is how we think of these things now even in the context of classical mechanics: Hamilton’s equations of motion provide a local mechanistic account in terms of efficient causes, while something like the principle of least action provides a global teleological account in terms of final causes. When one looks at such things as [Bellman’s dynamic programming](https://press.princeton.edu/books/paperback/9780691146683/dynamic-programming) or [Pontryagin’s maximum principle](https://en.wikipedia.org/wiki/Pontryagin%27s_maximum_principle), the interpretation in terms of global final causes and local efficient causes becomes not only apparent, but in fact quite indispensable.[^4] There is even a clean correspondence between the gradient of the cost being optimized (final cause) and the so-called system co-state evolving backward in time according to the adjoint equation (efficient cause). However, as pointed out by Mark Wilson [^5], this viewpoint was already present in some form in the work of Leibniz on the elasticity properties of loaded beams.[^6] According to Wilson, this is where one can already see the precursors of Leibniz’s monadology, the notion of preestablished harmony, and all that (and even his Panglossian optimism, caricatured by Voltaire in *Candide*), but, as Wilson points out,

> \[t\]his basic theme—calculus formulas represent simplified, downwardly projected generators of larger scale behaviors—is central to Leibniz’s elaborate musings upon the “metaphysics of the calculus”; differential equations represent shorthand, asymptotic formulas that capture a material’s characteristics only down to some indefinite choice of scale size. We moderns normally view physics’ most fundamental differential equations in a more favorable light, as directly registering nature’s workings at an infinitesimal level. However, when we consider the standard equations for the classical continua of everyday life (beams, strings, fluids, et al.), we tacitly shift to Leibniz’s point of view, for their validity can only be understood as resulting from a downward projection of homogeneous patterns witnessed at large-to-middling-scale lengths.

In other words, what Leibniz called “the kingdom of ends or final causes” pertains to global optimization over an infinite-dimensional space of functions, the subject of variational calculus, but these final causes are implemented locally at a lower scale, the “kingdom of efficient causation” instantiated by vector fields on finite-dimensional state space. Wilson quotes Leibniz saying

> I have shown that everything in bodies takes place through shape and motion, everything in souls through perception and appetite; that in the latter there is a kingdom of final causes, in the former a kingdom of efficient causes, which two kingdoms are virtually independent of one another, but nevertheless are harmonious.

To Leibniz, and to Wilson, this is the opportune backdrop for talking about such lofty matters as *free will*, but that’s not such a fanciful notion after all. For instance, we see the following passage in the Preface to *[Control Theory from the Geometric Viewpoint](https://link.springer.com/book/10.1007/978-3-662-06404-7)* by Andrei Agrachev and Yuri Sachkov:

> The classical deterministic physical world is described by smooth dynamical systems: the future in such a system is completely determined by the initial conditions. Moreover, the near future changes smoothly with the initial data. If we leave room for "free will" in this fatalistic world, then we come to control systems. We do so by allowing certain parameters of the dynamical system to change freely at every instant of time. That is what we routinely do in real life with our body, car, cooker, as well as with aircraft, technological processes etc. We try to *control* all these dynamical systems!

So, if it’s final causes you are after, then they are alive and well in the realm of engineering, although more modest, local, and scaled back to the level of human ambition and foresight.

[^1]: That said, given Newton’s deeply religious worldview, he was taking for granted that there is an *immanent cause*, i.e., God, that everything else depends on for its functioning and unfolding.

[^2]: W. Ross Ashby, “The physical origin of adaptation by trial and error,” *The Journal of General Psychology*, vol. 32, no. 1, pp. 13-25 (1945).

[^3]: Strictly speaking, it makes more sense to identify the system’s organization with the structure (or functional form) of its dynamical law *f* (*x,u*), which is better thought of as formal cause.

[^4]: And even in the case of classical mechanics the optimal control perspective is [often more illuminating](https://sites.math.rutgers.edu/~sussmann/papers/brachistochrone-mex.pdf).

[^5]: M. Wilson, “From the bending of beams to the problem of free will,” in *Physics Avoidance*, Oxford University Press, 2017.

[^6]: G.W. Leibniz, “New Proofs Concerning the Resistance of Solids,” *Acta Eruditorum*, July 1684.