# Flam_RnD_Assignment
R&amp;D assignment solution for FLAM — curve parameter estimation using Python

AIM:
To determine the unknown parameters theta (rotation angle), M (exponential factor), and X (horizontal translation) in the given parametric equation of a curve using the provided dataset of points (x, y).
The goal is to find the parameter values that produce the best-fitting curve to the data for 6 < t < 60.

EQUATIONS:

x(t) = \left(t*\cos(\theta)-e^{M\left|t\right|}\cdot\sin(0.3t)\sin(\theta)\ +X \right )

y(t) = \left (42 + t*\sin(\theta)+e^{M\left|t\right|}\cdot\sin(0.3t)\cos(\theta)\right)

FILES INCLUDED IN REPOSITORY

xy_data.csv — Dataset containing (x, y) coordinates of the given curve.

FLAM_Assignment.ipynb — Google Colab notebook containing Python implementation, optimization steps, and visualizations.

README.md — Documentation explaining aim, process, results, and final equations.

STEP-BY-STEP PROCESS:
Step 1: Uploading the Data
The dataset file named xy_data.csv is uploaded into Google Colab

Step 2: Loading and Exploring the Data
Read the uploaded file into the table to check the number of rows and columns (x and y).
Validate that the data is in the right format and ready for use.

Step 3: Visualizing the Raw Data
A scatter plot is generated to see the overall pattern of the curve.
This helps verify that the points form a continuous path suitable for analysis.

Step 3: Visualizing the Raw Data
We created a scatter plot to visualize the overall trend of the curve. This provides an opportunity to ensure the points are arranged continuously throughout for subsequent analysis.

Step 4: Defining the Parameter 't'
A uniform parameter t is assigned to all data points in the uniform range between 6 and 60. This parameter serves as the input variable of the curve.

Step 5: Estimating Theta (Rotation Angle) and X (Horizontal Shift)
Once we have a mathematical relationship on x, y, and t, we estimate theta and X by minimizing mean square errors between calculated and observed values. This step identifies the amount the curve is rotated and shifted horizontally into location.

Step 6: Estimating M (Exponential Factor)
After determining theta and X, the exponential factor M is estimated using the relationship v = e^(M*t)sin(0.3t). By taking the log of the amplitude, M is given by the slope of the straight line.

Step 7: Optimizing Parameters Together
All three parameters (theta, M, and X) are refined together in an optimization process that minimizes the total error between the predicted curve and real dataset for best fit.

Step 8: Plotting the Fitted Curve
The fitted curve is finally plotted with the optimized parameters, which allows one to visualize how close the fit is when compared to the original points.
The model is represented by the red line and the original data by blue dots.

Step 9: Computing the L1 Distance
The L1 distance (mean absolute error) is computed to evaluate the distance between the fitted curve and actual data.
A smaller L1 distance indicates a better fit. 

Step 10: Recording and Reporting the Outcomes
The final values of the parameters theta, M, and X with the L1 metric are noted.
The equations and comments are recorded for submission and future reference.

FINAL RESULTS
Rotation Angle (theta): 29.583 degrees (0.5163 radians)
Exponential Factor (M): -0.05000
Translation Offset (X): 55.014

Final Desmos Equation after substituting these values:

\left(t\cdot\cos\left(0.5163\right)\ -\ e\ ^{\left(-0.05\ \cdot\ \operatorname{abs}\left(t\ \right)\right)}\cdot\sin\left(0.3\cdot t\right)\cdot\sin\left(0.5163\right)+55.014,\ 42\ +\ t\ \cdot\ \sin\left(0.5163\right)\ +\ e^{\left(-0.05\cdot\operatorname{abs}\left(t\right)\right)}\cdot\sin\left(0.3\cdot t\right)\cdot\cos\left(0.5163\right)\right)\ \left\{6\ \le\ t\ \le\ 60\right\} 

EVALUATION METRIC

L1 Distance: 25.40
(Mean absolute deviation between predicted and actual (x, y) coordinates)

CONCLUSION

The project was able to estimate the parameters theta, M, and X associated with the provided curve fairly uniformly.

The optimized curve fits the pattern of the provided sample well.

The L1 distance value indicates that the model and data fit well.

The process illustrates a complete workflow from data upload, estimating parameters, optimizing the fit, to then evaluating the model.
