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

##  Final Estimated Parameters

| Parameter | Symbol | Value |
|------------|:-------:|------:|
| Rotation Angle | θ | **30° (0.523598776 rad)** |
| Growth Rate | M | **0.03** |
| Horizontal Shift | X | **55.0** |

---

##  Submission Equation
\left(t*\cos(0.523598776)-e^{(0.03*\operatorname{abs}\left(t\right))}\sin(0.3t)\sin(0.523598776)+55,\ 42+t*\sin(0.523598776)+e^{(0.03*\operatorname{abs}\left(t\right))}\sin(0.3t)\cos(0.523598776)\right)

