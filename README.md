# Assignment for Research and Development / AI  
### Estimation of Unknown Parameters (θ, M, X) for a Rotated Exponential–Sinusoid Curve

---

##  Overview
This project estimates the unknown variables **θ**, **M**, and **X** in the parametric equations of a rotated and translated exponential–sinusoidal curve using given point data (`xy_data.csv`).

The curve is defined as:

\[
\begin{aligned}
x(t) &= t\cos(\theta) - e^{M|t|}\sin(0.3t)\sin(\theta) + X, \\
y(t) &= 42 + t\sin(\theta) + e^{M|t|}\sin(0.3t)\cos(\theta)
\end{aligned}
\]

where the parameters are bounded as:
\[
0^\circ < \theta < 50^\circ,\quad -0.05 < M < 0.05,\quad 0 < X < 100,\quad 6 < t < 60
\]

---

##  Objective
Given a set of points `(x, y)` sampled from the curve, the goal is to estimate the **unknowns**:
- Rotation angle **θ**
- Exponential growth/decay rate **M**
- Horizontal shift **X**

and reproduce the curve that best fits the provided data.

---

##  Methodology

### 1. **Preprocessing**
- **Offset removal:** Subtract the known vertical offset `y₀ = y - 42`.
- **Outlier detection:**  
  - **Mahalanobis Distance** is used to remove far-away points in `(x, y₀)` space.  
  - **Residual Trimming:** Points with residuals beyond `3 × MAD` (Median Absolute Deviation) are removed after an initial fit.  
  - This ensures robustness against noise or misaligned data segments.

### 2. **Initial Parameter Estimation**
- **Rotation (θ):**  
  Estimated using **Principal Component Analysis (PCA)** / **Total Least Squares (TLS)** — the first principal component gives the dominant direction of data spread, corresponding to the tangent of the curve.
- **Shift (X):**  
  Computed by aligning the mean `u` (rotated coordinate) near the center of the theoretical `t` range (≈33).
- **Growth factor (M):**  
  Estimated from the slope of the logarithmic amplitude envelope of `|v(u)|`, where  
  \[
  v(u) = e^{Mu}\sin(0.3u)
  \]
  Peaks of `|v|` are used for envelope extraction.

### 3. **Phase Alignment**
A fine adjustment of `X` is performed by minimizing the L1 distance between the predicted and actual `v(u)` signals while keeping `(θ, M)` fixed.  
This step ensures that the sinusoidal phase matches the observed oscillations.

### 4. **Nonlinear Optimization (Refinement)**
A **Levenberg–Marquardt (LM)** optimization is applied jointly on `(θ, X, M)` with:
- Analytic Jacobian for high precision and stability.
- **Huber IRLS weighting** for robustness to remaining outliers.
- Box constraints ensuring parameters stay within valid ranges.

---

##  Geometric Insight

By rotating and translating the data, we find:
\[
u = (x - X)\cos\theta + (y - 42)\sin\theta = t, \\
v = -(x - X)\sin\theta + (y - 42)\cos\theta = e^{Mt}\sin(0.3t)
\]

Thus, the complex 2D curve becomes a simple 1D function in the transformed space.  
This decoupling allows easier parameter estimation and provides geometric intuition for the meaning of each parameter:
- **θ** controls the rotation of the curve in the plane.
- **M** controls how the oscillation envelope grows or decays.
- **X** shifts the curve horizontally.

---

##  Final Estimated Parameters

| Parameter | Symbol | Value |
|------------|:-------:|------:|
| Rotation Angle | θ | **30° (0.523598776 rad)** |
| Growth Rate | M | **0.03** |
| Horizontal Shift | X | **55.0** |

---

##  Submission Equation
\left(t*\cos(0.523598776)-e^{(0.03*\operatorname{abs}\left(t\right))}\sin(0.3t)\sin(0.523598776)+55,\ 42+t*\sin(0.523598776)+e^{(0.03*\operatorname{abs}\left(t\right))}\sin(0.3t)\cos(0.523598776)\right)

