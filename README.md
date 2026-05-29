# Solutionary

A Java desktop application for solving numerical methods problems, built with Swing and styled with FlatLaf. Solutionary provides a clean, dark-themed GUI that lets students and engineers apply common numerical analysis techniques without writing code.

![Java](https://img.shields.io/badge/Java-23-blue) ![Maven](https://img.shields.io/badge/build-Maven-orange) ![License](https://img.shields.io/badge/license-MIT-green)

---

## Features

Solutionary is organized into six functional modules, accessible from a persistent side-panel navigation:

### Equation Root Finding (ATES)
Finds roots of single-variable equations `f(x) = 0` using:
- **Bisection Method**
- **False Position (Regula Falsi)**
- **Newton-Raphson**
- Fixed Point Iteration *(in progress)*
- Secant Method *(in progress)*

Supports automatic interval `[a, b]` detection and displays a full iteration table with each step.

### Interpolation
Estimates values between known data points:
- **Lagrange Interpolation**

### Curve Fitting
Fits a mathematical model to a set of data points:
- **Linear Regression**
- **Exponential Regression**
- **Quadratic Regression**

### Numerical Differentiation
Solves ordinary differential equations (ODEs) numerically:
- **Runge-Kutta 1st Order (Euler's Method)**
- **Runge-Kutta 2nd Order**
- **Runge-Kutta 4th Order**

### Numerical Integration
Computes definite integrals:
- **Trapezoidal Rule**
- **Simpson's 1/3 Rule**
- **Simpson's 3/8 Rule**
- **Weddle's Rule**

### Extra Tools
- **Gauss Elimination** — solves systems of linear equations
- **Gauss-Jordan** — solves systems of linear equations with full row reduction
- Font utility for debugging available system fonts

---

## Tech Stack

| Dependency | Version | Purpose |
|---|---|---|
| Java | 23 | Language |
| Maven | — | Build & dependency management |
| [FlatLaf](https://www.formdev.com/flatlaf/) | 3.5.2 | Modern dark/light UI theme |
| [exp4j](https://www.objecthunter.net/exp4j/) | 0.4.8 | Runtime math expression parsing |
| [JLaTeXMath](https://github.com/opencollab/jlatexmath) | 1.0.7 | LaTeX formula rendering in labels |

---

## Getting Started

### Prerequisites
- Java 23 or later
- Maven 3.6+

### Build

```bash
git clone https://github.com/your-username/solutionary.git
cd solutionary
mvn package
```

### Run

```bash
java -jar target/Solutionary-1.0-SNAPSHOT.jar
```

Or open the project in IntelliJ IDEA and run `Main.java` directly. The project includes a pre-configured IntelliJ artifact (`Solutionary_jar.xml`).

---

## Project Structure

```
src/main/java/
├── Main.java                         # Entry point
├── UI/
│   ├── UI.java                       # Main application window (JFrame)
│   ├── SidePanel.java                # Navigation sidebar
│   ├── DashBoardPanel.java           # Home/dashboard screen
│   ├── PanelButton.java              # Custom sidebar button component
│   ├── CompUtil.java                 # Shared UI component utilities
│   ├── ErrorUtil.java                # Error dialog helpers
│   └── SolutionarySplashScreen.java  # Startup splash screen
├── ATES/                             # Equation root finding
│   ├── ATESPanel.java
│   ├── Bisection.java
│   ├── FalsePosition.java
│   ├── NewtonRaphson.java
│   ├── Interval.java
│   └── IntervalFinder.java
├── Interpolation/
│   ├── InterpolationPanel.java
│   └── Lagrange.java
├── CurveFitting/
│   ├── CurveFittingPanel.java
│   ├── Linear/LinearEquation.java
│   └── NonLinear/
│       ├── ExponentialEquation.java
│       └── QuadraticEquation.java
├── NumericalDifferentiation/
│   ├── NumericalDifferentiationPanel.java
│   └── RungeKutta.java
├── NumericalIntegration/
│   ├── NumericalIntegrationPanel.java
│   ├── Trapezoidal.java
│   ├── Simpsons_1_3.java
│   ├── Simpsons_3_8.java
│   └── Weddles.java
└── ExtraTools/
    ├── ExtraToolsPanel.java
    ├── GaussElimination.java
    ├── GaussJordan.java
    └── LegacyInterpolationPanel.java
```

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

## Improvement Plan

See [IMPROVEMENT_PLAN.md](IMPROVEMENT_PLAN.md) for a detailed roadmap of planned enhancements.
