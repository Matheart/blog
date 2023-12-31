--- 
comment: True
---

# Foundations of Machine Learning
!!! Info
    This book, ***Foundations of Machine Learning***, serves as a general introduction to machine learning theory, it covers some basic tools of the field and also shows how to use the tools to justify and analyze the behevaior of machine learning algorithms.

# Ch 2 The PAC Learning Framework
!!! Introduction
    PAC's full name is **Probably Approximately Correct**, this framework is mainly used to mathematically analyze the lower bound of the sample size, in order to achieve a certain high possibility while the error is small to some extent.

To present in a more formal way, we define **concept** as the mapping we are going to learn from $\mathcal{X} \to \mathcal{Y}$. For example, concept can be the set of points inside a rectangle (which is a classical example we are going to show later), and the concept class $C$ is the set of concepts we wish to learn. We denote $O(n)$ as an u.b. on the cost of the computational representation of any element $x \in \mathcal{X}$, and size($c$) as the maximal cost of the computational representation of $c \in C$.

The learning problem is formulated as we have a **hypothesis set** $H$ and we pick **hypothesis** $h$ from it in order to approximate the ground truth concept (more formally, we say, **target concept**). Note that we always assume there is an underlying distribution $D$, and the examples are **independent and identically distributed (iid)**, with this assumption, it is much easier for us to derive the theorems.

The hypothesis does not always exactly match the target concept, so there must be error, there are two types of error respectively, namely **empirical error** ($\hat{R}(h)$), and **generalization error** ($R(h)$). Empirical error can be accessed while the generalization errror is not directly accessible to the learner.

Given a hypothesis $h \in H$, a target concept $c \in C$, and an underlying distribution $D$, then:

\begin{align}
R(h) &= \ \ \ \mathbb{E}_{x \sim D}{\  \mathbf{1}_{h(x) \neq c(x)}} = \text{Pr}_{x \sim D}{[h(x) \neq c(x)]}\\
\hat{R}(h) &= \frac{1}{m}\sum_{i = 1}^{m}{\ \mathbf{1}_{h(x) \neq c(x)}}
\end{align}

With iid assumption we can prove that the expectation of empirical error is the generalization error.

## PAC-Learnable
!!! Definition
    A concept class $C$ is said to be **PAC-learnable** if there exists an algorithm $\mathcal{A}$ and a polynomial function poly $(\cdot, \cdot, \cdot, \cdot)$ s.t. for any $\varepsilon, \delta > 0$, for all distributions $D$ on $X$ and for any target concept $c \in C$, the following holds for any sample size $m \geq$ poly $(1/\varepsilon, 1/\delta, n, \text{size}(c))$:

    $$\text{Pr}_{S \sim D^m} [R(h_S) \leq \varepsilon] \geq 1 - \delta$$

    It is called **efficiently** PAC-learnable if it further runs in poly $(1/\varepsilon, 1/\delta, n, \text{size}(c))$.

The intuition is that if the required sample size is not polynomial of those four "parameters" (for example it grows exponentially), in most practical case it is unrealistic to collect such amount of data.

Also note that this framework does not have any additional assumptions about underlying distribution, but both training and test samples should be drawn from the same distribution.

## Classical Example: Learning axis-aligned Rectangles
We would not elaborate the proof here but would highlight some of the key points. First is the algorithm $\mathcal{A}$ is to return the tightest rectangle containing all points with label 1 (denote as $R_S$), s.t. there is no False-positives and there's only False-negatives (and they are contained in $R$). 

With such construction, we can safely assume Pr$[R] > \varepsilon$, and construct four rectangles at the edge of $R$ respectively with each area exactly equal to $\varepsilon/4$. We could show that if $R_S$ meets all four small rectangles, the error area $\leq \varepsilon$. Its contraposition shows it must miss one of the small rectangle, the remaining part is just to derive probability inequality regarding $\text{Pr}_{S \sim D^m}[\text{R}(R_S) > \varepsilon]$, set $\delta$ equal to the upper bound of it, then we can finally derive the lower bound of $m$ i.e. $\frac{4}{\varepsilon}\log \frac{4}{\delta}$. We know that $n, \text{size}(c)$ is constant, so it is PAC-learnable.

Such way of proof can be extended to more interesting problems (exercises in book), such as extending this result from $\mathbb{R}^2$ to $\mathbb{R}^n$, learning concentric circles / triangles instead of rectangles, learn with noise etc.

## Guarantees for finite hypothesis sets
We extend the example above to more general settings, it is actually **consistent** i.e. the empirical error is 0 on training sample. In order to derive the bound more easily we need to assume that the hypothesis set is of finite size.

### Consistent case
!!! Theorem 
    Let $H$ be a finite set of functions mapping from $\mathcal{X}$ to $\mathcal{Y}$. Let $\mathcal{A}$ be an algorithm that for any target concept **$c \in H$** and i.i.d. sample $S$ returns a consistent hypothesis $h_S: \hat{R}(h_S) = 0$. Then for any $\varepsilon, \delta > 0$, the inequality $\text{Pr}_{S \sim D^m}[R(h_S) \leq \varepsilon] \geq 1 - \delta$ holds if:
    
    $$m \geq \frac{1}{\varepsilon}(\log |H| + \log \frac{1}{\delta})$$

    Another equivalent statement is that for any $\varepsilon, \delta > 0$, with probability at least $1 - \delta$, we have

    $$R(h_S) \leq \frac{1}{m}(\log |H| + \log \frac{1}{\delta})$$

Want: We don't know what will be the $h_S$, so we need to give an uniform convergence bound, bound Pr[$\exists h \in H : \hat{R}(h) = 0 \text{ and } R(h) > \varepsilon$] by $\delta$.

Trivially we know the set is equal to the union of all hypothesis $h$ that has $\hat{R}(h) = 0 \text{ and } R(h) > \varepsilon$, so by union bound, it is bounded by $\sum_{h \in H}{\text{Pr}[\hat{R}(h) = 0 \text{ and } R(h) > \varepsilon]}$, this is further bounded by the conditional probability $\sum_{h \in H}{\text{Pr}[\hat{R}(h) = 0 | R(h) > \varepsilon]}$.

For each $h$ we are able to bound that by $(1 - \varepsilon)^m$ by making use of definition of $R(h)$ and also i.i.d. property, this gives us the overall value is bounded by $|H|(1 - \varepsilon)^m \leq |H| e^{-\varepsilon m}$.

When $m \geq \frac{1}{\varepsilon}(\log |H| + \log \frac{1}{\varepsilon})$, with some simple algebraic manipulation we can obtain $|H| e^{-\varepsilon m} \leq \delta$, this concludes the proof.    $\square$

The second statement shows that when the training sample size $m$ increases, the upper bound of generalization error will be tighter, this theoretically justifies learning algorithms benefit from larger labeled training samples. However, the price to pay for coming up with a consistent algorithm is the use of a larger hypothesis set $H$ containing target concepts, the upper bound invreases with $|H|$ but the dependency is only logarithmic.

#### Some examples
**Conjunction of Boolean literals**: $|H|$ is $3^n$ which is finite, we can also carry out a consistent algorithm, which means this is suitable for the theorem above. We can show $m \geq ((\log 3)n + \log \frac{1}{\delta})$

**Universal Concept Class**: To guarantee a consistent hypothesis, $|H| \geq |U_n| = 2^{(2^n)}$, this gives us the sample complexity bound $m \geq \frac{1}{\varepsilon}((\log 2) 2^n + \log \frac{1}{\delta})$, this is actually not a polynomial, so it is not PAC-learnable.

**k-term DNF formulae**: $m \geq \frac{1}{\varepsilon}((\log 3)nk + \log \frac{1}{\delta})$. However, it is shown that this problem is **RP**, it is PAC-learnable but not efficient unless **RP = NP**.

**k-CNF formulae**: We can reduce the formula into conjection of boolean literals by a bijection, so this implies the PAC-learanability of **K-CNF formula**. However, although DNF formula can be rewritten into K-CNF formula, itself is still not PAC-learnable. This is beacause the number of new variables needed for this transformation is in $O(n^k)$. **This apparent paradox deals with key aspects of PAC-learning, which include the cost of the representation of the concept and the choice of the hypothesis set**.

### Inconsistent Case
In practice, $H$ may not contain the hypothesis consistent with the training sample, but inconsistent hypothesis with small number of errors on number of training samples is still useful. **Hoeffding's inequality** will be used to relate the generalization error and empirical error of a single hypothesis.

# Ch 3 Rademacher Complexity and VC-dimensions
!!! Introduction
    We deal with the infinite hypothesis size in this chapter, we can use various techniques to reduce it into finite sets of hypotheses can proceed as in the previous chapter. Using **Rademacher Complexity** based on **McDiarmid's inequality** can derive high-quality bounds. However, the computation of empirical Rademacher complexity is NP-hard for some hypothesis sets. So we will introduce the **growth function** and **VC-dimension** which is easier to compute.

Formally any loss function $L$ is $\mathcal{Y} \times \mathcal{Y} \to \mathbb{R}$, we use $G$ to denote family of loss functions associated to $H$, 