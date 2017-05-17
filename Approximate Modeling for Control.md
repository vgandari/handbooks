# Approximate Modeling for Control


<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Approximate Modeling for Control](#approximate-modeling-for-control)
  * [Definitions](#definitions)
  * [Model Structures](#model-structures)
  * [List of Properties](#list-of-properties)
  * [List of Methods](#list-of-methods)
  * [List of User Actions/Goals/Workflows](#list-of-user-actionsgoalsworkflows)
  * [Assumptions](#assumptions)
  * [Remarks](#remarks)
  * [Methods for affecting the model fit](#methods-for-affecting-the-model-fit)
  * [One Step Ahead Prediction](#one-step-ahead-prediction)
  * [Filtered Prediction Error](#filtered-prediction-error)
  * [Parseval's Theorem](#parsevals-theorem)
  * [Prediction Error Method](#prediction-error-method)
    * [Concept](#concept)
    * [Remarks](#remarks-1)
    * [Example: PEM (ARX)](#example-pem-arx)
  * [Troubleshooting](#troubleshooting)
  * [Design Considerations](#design-considerations)
    * [Best Practices](#best-practices)
    * [Remarks](#remarks-2)
  * [Questions About Theory](#questions-about-theory)

<!-- tocstop -->


<!-- ## Concept

- given a system response, design a model to capture system dynamics
- given a model of the system, design a controller and apply it to the actual system
- Controller only ‘sees’ I/O data of your system
- Controller in general much simpler than system dynamics

## Use Case

- controller and thus the cross-over frequency is not known and based on the model

## Keywords

- identification -->

## Definitions

**Sensitivity function** - Transfer function of the signal entering plant after feedback; also describes the transfer function from external disturbance to process output $$S(q) = \frac{1}{1+G(q)C(q)}, S(s) = \frac{1}{1+G(s)C(s)}$$ useful for choosing controller parameters such that the closed loop system is not sensitive to variations in process dynamics [https://en.wikipedia.org/wiki/Sensitivity_(control_systems)](https://en.wikipedia.org/wiki/Sensitivity_(control_systems))

**Sensitivity Circle** - the inverse of the shortest distance from the Nyquist curve of the loop transfer function to the critical point $-1$; guarantees that the distance $M_s$ from the critical point to the Nyquist curve is always greater than $1/M_s$

**Approximate Identification** - typically lower order models that capture the essential dynamics; estimation of models that only approximate the “real” behavior between input/output data. In contrast to “consistent” estimation.

**Essential dynamics** - dictated by the intended usage of the model.

**observations** - time domain measurements of input/output or frequency domain data


**Cramér-Rao bound** - expresses a lower bound on the variance of estimators of a deterministic (fixed, though unknown) parameter; bound states that the variance of any unbiased estimator is at least as high as the inverse of the Fisher information;  [https://en.wikipedia.org/wiki/Cram%C3%A9r%E2%80%93Rao_bound](https://en.wikipedia.org/wiki/Cram%C3%A9r%E2%80%93Rao_bound)

**Parseval’s formula** - replace 2-norm in time domain with an integral expression in the frequency domain; smallest possible minimum is determined by e(t); not only input spectrum, but also (to be estimated) noise
model influences the approximation of parameters.

**implicit weighting** - denominator of noise model determines which frequencies are

**Fractional Model Based Approach** -
**(Dual) Youla-Kucera Parametrization**
**Normalized Coprime Factors**

**IID** - $E{e(t)e(t − \tau)} = 0 "\tau > 0$

**monic** - $H_0(q) = 1 + \bar{H}_0(q)$, $\bar{H}_0(q)$ has one step time delay and $H_0(q)$ is stably invertible


<!-- ## Notation, Mathematical Representations -->

**Data Generating System** - $$\mathcal{S} : y(t) = G_0(q)u(t)+v(t), v(t) = H_0(q)e(t)$$

**Prediction Error** - difference between model prediction and system output, given by $$\varepsilon(t, \theta) = H^{-1}\theta (q)[y(t) − G\theta(q)u(t)]$$

## Model Structures

**ARX/FIR** - model and noise filter share the same denominator/poles; prediction error is linear in parameters; noise model unknown, use implicit weighting; amplitude Bode response of noise model is amplitude Bode response of the system model with only poles (no zeros); noise filter most likely low pass filter (inverse most likely high pass filter); Fit of model determined by input spectrum and implicit high frequency weighting (magnitude squared of denominator); $$G_\theta(q) = \frac{B(q, \theta)}{A(q, \theta)}, H_\theta(q) = \frac{1}{A(q, \theta)}$$

**ARMAX** - model and noise filter share the same denominator/poles; prediction error is nonlinear in parameters; $$G_\theta(q) = \frac{B(q, \theta)}{A(q, \theta)}, H_\theta(q) = \frac{C(q, \theta)}{A(q, \theta)}$$

**BJ** - parameters in plant and noise models are independent; $$G_\theta(q) = \frac{B(q, \theta)}{F(q, \theta)}, H_\theta(q) = \frac{C(q, \theta)}{D(q, \theta)}$$

**OE** - noise filter/model is unity; parameters in plant and noise models are independent; prediction error to be minimized is a simulation error; only input spectrum (not noise model) influences approximation; $$G_\theta(q) = \frac{B(q, \theta)}{F(q, \theta)}, H_\theta(q) = 1$$

**general model** - $$G_\theta(q) = \frac{B(q, \theta)}{A(q, \theta)F(q, \theta)}, H_\theta(q) = \frac{C(q, \theta)}{D(q, \theta)A(q, \theta)}$$

## List of Properties

- Bias and Variance (2)
- affecting the model fit
  - identification methods
    - Excitation spectrum — experiment design Chap 13
    - Signal Filtering — data weighting

- Approximate models for control design typically exhibit the need to model the essential dynamics around the cross-over frequency of the closed-loop system
- Direct identification of an OE model where the plant model has 1 step time delay leads to a bias term
- If a plant model has a bias term, then increasing the number of samples decreases the bias and the plant model dynamics approaches the true plant dynamics
- Without time delay, there is no bias term
- Fourier Transform is unitary

## List of Methods

- Prediction Error Methods (2, 3)
  - direct identification (4)
  - indirect identification (5, 6)
  - tailor-made identification (5, 6)
  - IV identification
  - two-stage identification
- Parseval's Formula (2, 3)
  - input spectrum acts like a frequency dependent weighting to influence fit of model onto system
  - could also minimize a filtered prediction error to emphasize certain frequency domains (L3.16)
- Approximate Models for Control via Filtered PE (3)
- Linear regression and IV method (6)
- Two-stage closed-loop identification (6)
- extended two-stage method (8)
- Ziegler & Nichols method
  - Implement a proportional controller and increase the gain Kp until system because marginally unstable
  - On the basis of the observed oscillation (one frequency domain measurement) and the value of Kp (previous controller)
  - adjust the values to obtain a proper PID controller (controller design)
  - for estimating both plant and noise dynamics

## List of User Actions/Goals/Workflows

- Rewrite asymptotic estimate in frequency domain
  - Apply Parseval's theorem (L3.S9)
  - input and noise model influences approximation
- Get rid of bias
  - Two options:
    - Simply try to estimate the ”best nominal model”
    - Estimate a nominal model and try to characterize the error
- Analyze stability of feedback when controller is designed with model
- Approximate Identification via PE framework
  - Useful: Parseval’s formula and the frequency domain
  - Open-loop and closed-loop approximation formulas
- Methods for affecting the model fit
- Approximate Identification from Closed-Loop Experiments
- Recursive and non-linear system identification
- Identification for Control — nominal model + uncertainty estimate
- Iterative Modeling and Control Design
- Prediction Error Methods (3)
- Analysis of CL Experiments (4)
- Closed-loop identification (7, 8)
  - direct identification (4)
  - indirect identification (5, 6)
  - tailor-made identification (5, 6)
  - IV identification
  - two-stage identification
- estimating unstable models (9)
- Fractional Model Based Identification (9, 10)
- Identification of Normalized Coprime Factors (10)

## Assumptions

- assume linear time invariant $G_0(q)$ of the format $$G_0(q)=q^{-n_k}\frac{b_0 + b_1q^{-1} + \dots + b_{n,b}q^{-n_b}}{1 + a_1q^{-1} + \dots + a_{n,a}q^{-n_a}}$$
- $e(t)$ is IID stationary process with zero mean and variance $\lambda$
- $H_0(q)$ is monic, WLOG

## Remarks

- Excessive peaking of sensitivity function leads to ringing of control loop.
- Approximate Identification for Control can be viewed as an enhanced Ziegler & Nichols method
  - Implement a proportional controller and increase the gain $K_p$ until system becomes marginally unstable $$L(q)=K_pG_0(q)$$
  - On the basis of the observed oscillation (one frequency domain measurement) and the value of $K_p$ (previous controller) adjust the values to obtain a proper PID controller (controller design)
- A good “open-loop” model approximation is not necessarily a good model for control design and vise versa
- Approximation of model should be “good around bandwidth”
- We must concentrate the approximation to important frequency ranges, and determine which these are. Iterative and adaptive approaches can be fit into this framework, as well as model validation.
- We need to learn how to deal with closed-loop experiments.
- The approximation/quality of the model for control design can be improved via closed-loop experiments.
- Variety of model structures lead to different parameters
- The notion of "bias/approximation" is clear if there DOES NOT exist a perfect parameter estimate for the model
- To understand the resulting model being estimated when parameter estimate is imperfect, rely on Parseval's theorem
- How prediction error is minimized determines the quality of your model for its intended purpose (control, simulation, prediction).
- quality of model is determined by experiment design, chosen model structure, possible filtering of prediction error.
- Quality of model can be expressed in terms of
  - bias : error between $G_0(q) \text{ and }  G_\theta(q) : \mid\mid G_0(e^{j\omega}) − G_\theta(e^{j\omega})\mid\mid$
  - variance : variations in $\theta \text{ or } G_\theta(q) : E{[\theta − \theta_0][[\theta − \theta_0]^T }$

## Methods for affecting the model fit

- Approximate Identification from Closed-Loop Experiments
  - Direct Identification and closed-loop bias formulæ
  - Indirect Identification
  - Tailor made parametrization methods
  - IV methods
  - Two-stage methods
- Recursive and non-linear system identification
  - Recursive estimation
  - Hammerstein systems
  - Parametrization, — Chap 5 + papers
- Identification for Control — nominal model + uncertainty estimate
  - Robust Control — control with approximate models
  - Stability robustness
  - Sensitivity (performance) robustness
  - Model Error Modeling
  - Controller Validation
- Iterative Modeling and Control Design
  - The need for an iterative solution — Schrama
  - The Windsurfer Approach
  - Examples of Iterative ID and Control

## One Step Ahead Prediction

$$y(t) = G_0(q)u(t)+v(t), v(t) = H_0(q)e(t)\\
H_0 = 1+ \bar{H}_0(q) \text{, (monic)}\\v(t|t − 1) = \bar{H}_0(q)e(t)\\
y(t|t − 1) = G_0(q)u(t)+v(t|t − 1)\\
y(t|t − 1) = G_0(q)u(t) + [1 − H^{-1}_0 (q)]v(t)\\
y(t|t − 1) = H^{-1}_0(q)G_0(q)u(t) + [1 − H^{-1}_0(q)]y(t)$\\
\mathcal{M}: y(t|t − 1, \theta) = H^{-1}\theta (q)G_\theta(q)u(t) + [1 − H^{-1}\theta (q)]y(t)$$



## Filtered Prediction Error

$$L_\theta(q) = H^{-1}_\theta (q)[G_\theta(q) H_\theta(q) − 1],$$
where
$$y(t|t − 1, \theta) = L_\theta(q)[u(t) \text{ , } y(t)]$$

- Use PEM with filter.
- Implication for OE model approximation result: integral inside arg min expression now contains filter $L(q)$.
- OE model approximation: create input spectrum u and filter L
    - Consider a noise free closed-loop experiment
    - To complete desired filtering, L3.S23, 24
    - Perform CL experiment then find a model by curve-fitting the frequency response, iteratively updating the weighting
    - closed-loop relevancy is obtained automatically by means of a closed-loop experiment with a reference signal
    - reference spectrum could also be used for additional weight-ing on the sensitivity difference minimization
    - Additional (iteratively updating) filter $L(q)$ is needed
    - combination of closed-loop experiments and filtering can be done in both the time-domain (as analyzed) or in the frequency domain
    - Frequency domain approach requires the use of a closed-loop experiment with knowledge and implementation of the right stabilizing controller C (requires iteration)

## Parseval's Theorem

- Link between 2-norm in time-domain and 2-norm in frequency-domain

## Prediction Error Method

### Concept

- Based on time-domain observations
- Formulate a (one-step ahead) prediction $y(t|t − 1)$ based on past input/output data
- Minimize (the 2-norm of) the prediction error as a function of a parameter $\theta$ that appears in the model to be estimated
- Assumptions: noise is white, noise filter is stable and monic, system is stable (OL, can be unstable for CL)
- In order to predict the output one step ahead, we only need to predict the noise one step ahead; can use past filtered noise observations to predict filtered noise
- requires noise filter to be stably invertible
- Ideally, prediction error should equal white noise entering the system

### Remarks

- How prediction error is minimized determines the quality of your model for its intended purpose (control, simulation, prediction).
- quality of model is determined by experiment design, chosen model structure, possible filtering of prediction error.
- Quality of model can be expressed in terms of bias and variance
- If there does not exist a parameter for the system that exactly matches the real system, you can't get rid of bias
- Using the full freedom in $G(q, \theta)$ and $H(q, \theta)$ aims at finding the best prediction model.
Choosing $H(q, \theta) = 1$ makes $\varepsilon(t, \theta)$ an output error to find the best simulation model.


### Example: PEM (ARX)

```
Go=0.001*tf([0 0 10 7.4 0.924 0.1764],...
[1 -2.14 1.553 -0.4387 0.042 0],1);
u=randn(2048,1);
y=lsim(Go,u);
THoe=oe([y u],[2 2 1]);[a,b,c,d,f]=th2poly(THoe);
Goe=tf(b,f,1);
THarx=arx([y u],[2 2 1]);[a,b,c,d,f]=th2poly(THarx);
Garx=tf(b,a,1);
bode(Go,Goe,Garx)
```

## Troubleshooting

- Excessive peaking of sensitivity leading to ringing of control loop!

## Design Considerations

- How good should the model be?
- What system dynamics is important for stability & performance?
- Do we need to find a perfect model of the actual system?
- Where do we make the approximation?
- Do we need to model possible non-linearities?
- Can we update models and/or controllers directly (adaptive control and/or regulation)?
- Is it possible to estimate model error/unceratinty for robust control design?
- Which filter and/or input spectrum to choose so that model becomes relevant for control design?
- How does the integral expression help you influence the quality of the model at a desired frequency range (make your model control relevant)?
- L3.S13: Why do all the models converge to the same phase at the highest frquency?

### Best Practices

- Best check for suitability of plant model for control design is either:
  - Design controller on the basis of plant model and implement on real plant of the system (and cross your fingers)
  - Less rigorous:
    - In case you already have a controller that seems to be working for your real plant of the system, see if it also works for the plant model
    - Cautiously modify/improve the controller C using the “new” knowledge obtained from your model $\hat{G}$ Necessity of an iterative scheme.

### Remarks

- Variety of model structures lead to different parameters
- The notion of "bias/approximation" is clear if there DOES NOT exist a perfect parameter estimate for the model
- To understand the resulting model being estimated when parameter estimate is imperfect, rely on Parseval's theorem
- quality of model is determined by experiment design, chosen model structure, possible filtering of prediction error.

## Questions About Theory

- What is causing “peaks” in the sensitivity function?
- difference between prediction and simulation?
- What does Parseval's integral expression mean?

<!-- ## Future Format


- Concept
    - Motivation
    - Objective
- Use Case
    - Basic Info
    - Applicability
- Keywords
- Definitions
    - Properties
- Tools/Methods
    - Usage
    - Arguments
    - Outputs
    - Capability
    - Performance
    - Errors/Exceptions
    - Alternate/similar functions
- List of User Actions/Workflows
- User Action/Workflow
    - Using multiple tools
    - Design Choices
    - Keep in Mind
    - Best Practices
    - Workflow Verification Methods
- Keep in Mind (General)
- Best Practices (General)
- Troubleshooting
- Analysis
- Theory
- Questions
- List of Related Concepts

Put notes and examples wherever you want. -->
