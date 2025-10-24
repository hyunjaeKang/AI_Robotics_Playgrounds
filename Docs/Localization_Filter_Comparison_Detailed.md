# Comparison of Kalman Filter, Extended Kalman Filter, Particle Filter, and Histogram Filter

This document provides a comprehensive explanation and comparison of four major **state estimation techniques** used in robotics, control, and tracking: **Kalman Filter (KF)**, **Extended Kalman Filter (EKF)**, **Particle Filter (PF)**, and **Histogram Filter (HF)**.

---

## 🧠 1. Kalman Filter (KF)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FByEqu%2FbtsIfyzWneF%2FAAAAAAAAAAAAAAAAAAAAAOKLgum_tgeKL5-etxvsy7is1Q9Oasf5FzSSWFO3mE-g%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DdZ83slM434ppcPQDe9ty1geayaE%253D">


<p></p>


<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbEDmzp%2FbtsH5Qhz017%2FAAAAAAAAAAAAAAAAAAAAAFgCs8vxInT8hYkuD7g_WGiED9L54EG_IJXrFxzrkWBk%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DBVv6THdGTP6%252FfJQSWHW%252B9QqrAFE%253D">
<p></p>

**Purpose:**  
Estimates the state of a **linear** dynamic system with **Gaussian** noise.

**Model:**
$$
x_k = A x_{k-1} + B u_k + w_k
$$
$$
z_k = H x_k + v_k
$$

where  
- $x_k$: state vector  
- $u_k$: control input  
- $z_k$: observation  
- $w_k \sim \mathcal{N}(0,Q)$: process noise  
- $v_k \sim \mathcal{N}(0,R)$: observation noise  

**Method:**
1. **Prediction step:** Estimate next state and uncertainty.  
2. **Correction step:** Use new observation to update the estimate.

**Characteristics:**
- Tracks mean and covariance (both Gaussian).
- Exact and **optimal** for linear-Gaussian systems.

**Pros:**
- Fast and computationally efficient.  
- Optimal estimator under its assumptions.

**Cons:**
- Not valid for nonlinear or non-Gaussian systems.

---

## 🔁 2. Extended Kalman Filter (EKF)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FcgKka9%2FbtsH7zZrbyr%2FAAAAAAAAAAAAAAAAAAAAAGc39EuINn43jx6bNzmfEuA1PtXx37gUV_OfCdppC0Yg%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3D7keEe03GnSrmioy2WWVyWV0pg24%253D">

**Purpose:**  
Handles **nonlinear** systems by **linearizing** them locally.

**Model:**
$$
x_k = f(x_{k-1}, u_k) + w_k
$$
$$
z_k = h(x_k) + v_k
$$

where $f$ and $h$ are nonlinear functions.

**Method:**
- Approximate $f$ and $h$ using their **Jacobians**:
  $$
  F_k = \frac{\partial f}{\partial x}\bigg|_{\hat{x}_{k-1}}, \quad H_k = \frac{\partial h}{\partial x}\bigg|_{\hat{x}_{k}}
  $$
- Then apply the standard Kalman update equations.

**Characteristics:**
- Approximates posterior as a **single Gaussian**.  
- Works for *mildly nonlinear* systems.

**Pros:**
- Extends Kalman filtering to nonlinear cases.  
- Efficient and easy to implement.

**Cons:**
- Poor performance for strongly nonlinear systems.  
- May diverge if the linearization is inaccurate.

---

## 🧩 3. Particle Filter (PF)

**Purpose:**  
Approximates the **posterior distribution** by a set of random samples (particles).

**Idea:**  
Represent the belief distribution $p(x_k | z_{1:k})$ with **N weighted samples**:
$$
\{x_k^{(i)}, w_k^{(i)}\}_{i=1}^{N}
$$
Each particle represents a possible state hypothesis.

**Method:**
1. **Prediction:** sample new states from motion model.  
2. **Update:** compute weights using observation likelihood.  
3. **Resample:** replicate likely particles and discard unlikely ones.

**Characteristics:**
- Works with **nonlinear** and **non-Gaussian** models.  
- Can represent **multi-modal** distributions.

**Pros:**
- Very general and powerful.  
- Works even with ambiguous or discontinuous systems.

**Cons:**
- Computationally expensive.  
- Suffers from *particle degeneracy* (many particles have negligible weight).

---

## 📊 4. Histogram Filter (HF)

**Purpose:**  
Represents the probability distribution using discrete **grid cells** in the state space.

**Method:**
- Divide state space into bins (cells).  
- Maintain probability for each bin.  
- Update each bin using **Bayes’ rule**:
  $$
  bel(x) = \eta \; p(z | x) \sum_{x'} p(x | u, x') bel(x')
  $$

**Characteristics:**
- Fully discrete representation of belief.  
- Conceptually simple and intuitive.

**Pros:**
- Handles nonlinearities and arbitrary distributions.  
- Easy to visualize (e.g., for robot localization).

**Cons:**
- High computational cost in large spaces (curse of dimensionality).  
- Precision limited by grid resolution.

---

## ⚖️ Comparison Summary

| Feature / Filter | **Kalman Filter (KF)** | **Extended KF (EKF)** | **Particle Filter (PF)** | **Histogram Filter (HF)** |
|------------------|------------------------|------------------------|---------------------------|----------------------------|
| System Model | Linear | Nonlinear (linearized) | Nonlinear | Nonlinear (discretized) |
| Noise Assumption | Gaussian | Gaussian | Any | Any |
| Representation | Mean & covariance | Linearized Gaussian | Weighted particles | Grid probabilities |
| Distribution Shape | Unimodal (Gaussian) | Unimodal (approx.) | Multimodal allowed | Multimodal allowed |
| Optimality | Optimal (linear case) | Approximate | Approximate (Monte Carlo) | Approximate (discrete) |
| Computational Cost | Low | Moderate | High | Very High |
| Typical Application | GPS/IMU tracking | SLAM, mild nonlinearities | SLAM, robot localization | Grid-based localization |

---

## 🧭 Conceptual Summary

- **Kalman Filter:** Elegant and exact — but only for linear Gaussian systems.  
- **Extended Kalman Filter:** Adds local linearization — works for mild nonlinearities.  
- **Particle Filter:** Powerful and flexible — works for arbitrary nonlinearities.  
- **Histogram Filter:** Simple and visual — but scales poorly with dimensionality.

----

Powered by ChatGPT

