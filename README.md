# Flam_RnD_Assignment
R&amp;D assignment solution for FLAM — curve parameter estimation using Python

AIM:
To determine the unknown parameters theta (rotation angle), M (exponential factor), and X (horizontal translation) in the given parametric equation of a curve using the provided dataset of points (x, y).
The goal is to find the parameter values that produce the best-fitting curve to the data for 6 < t < 60.

EQUATIONS:
x=\left(t*\cos(\theta)-e^{M\left|t\right|}\cdot\sin(0.3t)\sin(\theta)\ +X \right )
y = \left (42 + t*\sin(\theta)+e^{M\left|t\right|}\cdot\sin(0.3t)\cos(\theta)\right)
