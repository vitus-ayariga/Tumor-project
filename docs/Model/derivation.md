# Model Derivations

---

Tumor growth is a complex process governed by the interaction between proliferating cancer cells, the surrounding environment, and the immune system. In this section, we derive a biologically informed mathematical model to describe and simulate this growth. The goal is to construct a system of differential equations that reflects key dynamics such as natural tumor proliferation, immune response interference, and carrying capacity limitations.

We start from biological intuition, translate into word equations, assign variables, and then systematically build up to the final governing equation used in our simulations.

---

## Governing Equation of tumor growth

### **1. Modeling Tumor Growth**

---

In the absence of any external control (e.g., immune system, drugs), tumors generally follow a **logistic growth pattern**:

- <p>Growth starts rapidly.</p>
- <p>slows as space, nutrients, and oxygen become limiting.</p>
- <p>...and eventually plateaus at a **carrying capacity**, denoted \( K_C \).</p>

**Is this biologically reasonable?**

> Initially, cancer cells divide unchecked. But resource constraints slow down their expansion overtime. Thus, tumor growth eventually halts unless external interventions are applied.

**Modeling:**

> **Rate of change of tumor volume** =  
> (intrinsic growth rate) × (current tumor size) × (resource limitation factor)

Translating this:

$$
\frac{dC}{dt} = \mu \cdot C \cdot \left(1 - \frac{C}{K_C} \right)
$$

Where:

- \( C(t) \): Tumor size at time \( t \)
- \( \mu \): Intrinsic growth rate of tumor cells
- \( K_C \): Tumor carrying capacity

This is the classical **logistic model**.

---

### **2. ...the Logistic Term, ...there's more**

---

Biological systems often deviate from simple logistic behavior, especially when tumor dynamics are influenced by nonlinear feedback mechanisms. To allow more flexibility in growth curve behavior, we introduce a **shape parameter** \( \alpha \), leading to the **generalized logistic (Richards) model**:

$$
\left(1 - \left(\frac{C}{K_C} \right)^\alpha \right)
$$

So the growth equation becomes:

$$
\frac{dC}{dt} = \mu \cdot C \cdot \left(1 - \left(\frac{C}{K_C} \right)^\alpha \right)
$$

Now:

- \( \alpha = 1 \) → Standard logistic growth
- \( \alpha > 1 \) → More abrupt slowdown near capacity
- \( \alpha < 1 \) → More gradual saturation
  <div><img src="../img/diff_alphas.png" alt="change in steepness due to alpha" style="width:400px; border-radius: 10px;">
  <a href="../code.html#effect-of-logistic-shape-parameter" class="btn btn-primary" role="button">See code</a>
</div>

---

### **3. Lets not forget the immune system**

---

The immune system doesn’t just “kill” cancer cells. It also interferes with their growth indirectly through inflammation, signaling interference, and disruption of angiogenesis. So instead of subtracting a kill term like \( -pCI \), we **modulate** the growth rate itself.

We introduce a new term:

$$
1 + \psi(I, C)
$$

Where:

- \( \psi(I, C) \) (derived later) is a function describing how the **immune cells \( I \)** affect the **growth behavior** of the tumor cells.
- The function can be negative (inhibitory) or positive (stimulatory), allowing for nuanced modeling.

This gives us:

$$
\frac{dC}{dt} = \mu \cdot (1 + \psi(I, C)) \cdot C \cdot \left(1 - \left(\frac{C}{K_C} \right)^\alpha \right)
$$

---

### **4. Final Normalization**

---

For mathematical elegance and numerical stability, we normalize the overall equation by \( \alpha \):

$$
\frac{dC}{dt} = \frac{\mu}{\alpha} (1 + \psi(I, C)) C \left(1 - \left(\frac{C}{K_C} \right)^\alpha \right)
$$

This is our **final tumor growth ODE**, and it matches **exactly** with the governing equation in the original project documentation.

---

**Final Tumor ODE:**

$$
\boxed{ \frac{dC}{dt} = \frac{\mu}{\alpha} (1 + \psi(I, C)) C \left(1 - \left(\frac{C}{K_C} \right)^\alpha \right) }
$$

---

## Immune Interference Function: ψ(I, C)

---

We define the immune interference function as:

\[
\psi(I, C) = -\theta \left( \frac{I^\beta}{\phi C^\beta + I^\beta} + \varepsilon \log\_{10}(1 + I) \right)
\]

This function models how the immune system inhibits the growth of tumor cells. Each term in the equation is derived from biological reasoning and mathematical modeling principles.

---

**Term-by-Term Breakdown**

### **1. The Scaling Factor: \( -\theta \)**

- **Negative sign**: Indicates inhibition of tumor growth.
- **θ (theta)**: Represents the maximum strength of immune-mediated suppression.
- It scales the entire function, setting a limit on how strong the immune system's effect can be.

---

### **2. Suppression Term:**

\[
\frac{I^\beta}{\phi C^\beta + I^\beta}
\]

This component reflects the competitive dynamics between immune cells \( I \) and cancer cells \( C \).

- **Numerator \( I^\beta \)**: Represents immune “attack power.” The exponent \( \beta > 1 \) introduces nonlinearity, modeling cooperative immune responses.
- **Denominator \( \phi C^\beta + I^\beta \)**: Models tumor resistance and competition.
  - \( \phi C^\beta \): Tumor's ability to resist immune action, scaled by a resistance constant \( \phi \).

This term behaves as saturation function:

- When \( I \gg C \), suppression is high.
- When \( C \gg I \), suppression drops toward zero.

---

### **3. Background Immune Activity Term:**

\[
\varepsilon \log\_{10}(1 + I)
\]

This term captures low-level immune suppression, even when \( C \) is large.

- The **logarithmic** form reflects diminishing returns: increasing immune cells beyond a point has less impact.
- \( \varepsilon \): A parameter that scales this background immune effect.

This term ensures:

- Some suppression occurs even at low \( I \),
- But suppression plateaus as \( I \) increases — modeling immune exhaustion or bottlenecks.

---

## Von Bertalanffy and Optimal Control Tumor Growth Models

### **Von Bertalanffy Tumor Growth Model**

---

> **Starting Assumption:** Tumor growth is driven by the surface area of the tumor, while cell death is proportional to its volume.

This is biologically reasonable because nutrients and oxygen (which drive proliferation) diffuse in through the surface area, while metabolic waste accumulation and lack of resources cause death throughout the volume.

---

### **Tumor Volume \( V(t) \) Derivation:**

Let:

- \( V(t) \): tumor volume at time \( t \)
- \( \frac{dV}{dt} \): rate of change of tumor volume

Now, using the assumptions:

> Growth is **proportional to surface area**:

1. <p>Surface area of a sphere \( \propto V^{2/3} \)</p>
2. <p>So growth term \( = a V^{2/3} \), where \( a \) is a growth coefficient.</p>

> **Death is proportional to volume**:

- <p>Death term \( = b V \), where \( b \) is a death (or decay) coefficient</p>

> Differential Equation

\[
\frac{dV}{dt} = aV^{\frac{2}{3}} - bV
\]

This is the **Von Bertalanffy equation**.

---

### **Solution to Von Bertalanffy equation**

---

The analytical solution to (1) is derived using integrating factor techniques

\[
V(t) = \left[\frac{a}{b} + \left( \frac{1}{V_0^{1/3}} - \frac{a}{b} \right)e^{-\frac{1}{3}b(t - t_0)}\right]^3
\]

Where:

- \( V_0 \): Initial volume at \( t_0 \)
- \( \frac{a}{b} \): Determines the carrying capacity \( K \)
- \( K = \left( \frac{a}{b} \right)^3 \): The saturation volume

This solution shows:

- If \( V_0 < K \), tumor grows to equilibrium.
- If \( V_0 > K \), tumor shrinks to equilibrium.
- If \( V_0 = K \), tumor remains constant.
<div><img src="../img/von_b.png" alt="change in steepness due to alpha" style="width:400px; border-radius: 10px;">
  <a href="../code.html#behavior-of-the-von-bertalanffy-model" class="btn btn-primary" role="button">See code</a>
</div>

---

## Control Optimization with Cost Functions

---

So far, we’ve modeled the tumor volume \( V(t) \) over time using a differential equation. Now, we want to **control** it. But not blindly. In the real world, controlling something like a tumor through drug treatment comes at a **cost**: side effects, resource limits, and drug resistance. So, instead of just minimizing tumor volume blindly, we need a **trade-off** between effectiveness and consequences.

We introduce a **control function** \( u(t) \), which models the intensity of the treatment (like drug dosage) at time \( t \). Our goal now is to **choose \( u(t) \) over time** in a way that balances:

> <p>keeping the tumor volume low, and</p>

> <p>not overusing the drug.</p>

This naturally leads to the idea of an **optimization problem**: we want to find the _best_ \( u(t) \) that makes this trade-off optimal.

---

### **...let's Start**

---

We model for how the tumor grows and how the drug affects it:

\[
V'(t) = rV(t)(1 - V(t)) - u(t)\delta(t)
\]

**Breakdown of each term:**

- \( rV(t)(1 - V(t)) \): basic tumor growth using logistic growth (tumor can’t grow forever),
- \( u(t)\delta(t) \): reduction in tumor due to drug; higher dosage \( u(t) \), stronger the effect,
- \( \delta(t) \): captures how effective the drug is at time \( t \); might decay over time.

**Initial condition:**

\[
V(0) = V_0
\]

**Control constraint:**

\[
u(t) \geq 0 \quad \text{for all } t \in [0, T]
\]

---

### **Introducing the Objective: What Are We Minimizing?**

---

We now ask: **What makes a “good” treatment strategy?**

Intuitively, we want to:

- keep tumor volume low throughout the treatment period,
- avoid unnecessarily high drug dosages.

To express this mathematically, we define a **cost function** (also called a performance index). This is a single quantity that reflects both goals, so we can minimize it.

\[
J(u) = \int_0^T \left[ aV(t)^2 + u(t)^2 \right] dt
\]

**Explanation:**

- \( V(t)^2 \): penalizes tumor size at each moment—bigger tumor, higher penalty,
- \( u(t)^2 \): penalizes how much drug we’re using—more drug, more cost,
- \( a > 0 \): a **weighting factor**—tells us how much more (or less) we care about tumor size compared to drug usage.

This integral adds up the total penalty from time 0 to time \( T \). So, the goal is to find a function \( u(t) \) that makes this total cost as small as possible—_without violating the tumor dynamics or the control constraint._

---

## The Optimization Problem

Putting everything together:

**Minimize:**

\[
J(u) = \int_0^T \left[ aV(t)^2 + u(t)^2 \right] dt
\]

**Subject to:**

\[
V'(t) = rV(t)(1 - V(t)) - u(t)\delta(t), \quad V(0) = V_0
\]

\[
u(t) \geq 0
\]

---

## Why This Matters

This formulation sets the stage for powerful mathematical techniques. We're no longer just watching the tumor evolve; we're actively searching for the _best way_ to intervene, balancing outcomes and side effects. The form of the cost function and the constraints reflect real-world priorities: effectiveness vs. safety.

Once the problem is written in this way, the next step—if we were to continue—would be to develop necessary conditions for optimality using calculus of variations or tools like Pontryagin's principle. But we stop here, having built a crystal-clear formulation of the problem from scratch.
