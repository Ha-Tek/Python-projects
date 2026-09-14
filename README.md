
# Python Projects

A collection of Python projects covering **mathematical optimization, integer programming, nonlinear optimization, machine learning, and multi-objective optimization**.

---

## 1. Integer Linear Programming with Python and Gurobi

This project uses **Python and Gurobi** to solve an integer linear programming (ILP) problem based on the following paper:

> Szádoczki, Z., Bozóki, S., & Tekile, H. (2022). *Filling in pattern designs for incomplete pairwise comparison matrices: (quasi-) regular graphs with minimal diameter*. **Omega, 107**, 102557.

### Problem Formulation

Let

$$
N = \{1,\ldots,22\}
$$

be the set of nodes, and let

$$
P = \{(i,j) : i,j \in N,\ i < j\}
$$

be the set of unordered node pairs.

For each $(i,j) \in P$, define the following binary decision variables:

* $X_{i,j}$: equals 1 if $(i,j)$ is an edge and 0 otherwise.
* $Y_{i,j,k}$: equals 1 if $k$ is a common neighbor of nodes $i$ and $j$ and 0 otherwise.
* $SLACK_{i,j}$: nonnegative slack variable associated with the pair $(i,j)$.

### Objective Function

The goal is to minimize the total slack:

$$
\min \sum_{(i,j)\in P} SLACK_{i,j}
$$

### Constraints

Each node must have degree 5:

$$
\sum_{\substack{(i,j)\in P\\k\in\{i,j\}}} X_{i,j} = 5,
\qquad \forall k\in N
$$

Each pair of nodes must either be directly connected, have a common neighbor, or use a slack variable:

$$
X_{i,j}
+
\sum_{k\in N\setminus\{i,j\}} Y_{i,j,k}
+
SLACK_{i,j}
\geq 1,
\qquad \forall (i,j)\in P
$$

A common neighbor $k$ of $i$ and $j$ requires an edge between $i$ and $k$:

$$
Y_{i,j,k} \leq
\begin{cases}
X_{i,k}, & i<k,\\
X_{k,i}, & k<i,
\end{cases}
$$

for all $(i,j)\in P$ and $k\in N\setminus{i,j}$.

Similarly, $k$ must be connected to $j$:

$$
Y_{i,j,k} \leq
\begin{cases}
X_{j,k}, & j<k,\\
X_{k,j}, & k<j,
\end{cases}
$$

for all $(i,j)\in P$ and $k\in N\setminus{i,j}$.


### Model Size

For $n=22$, the integer programming model contains approximately:

* **5,082 variables**
* **9,493 constraints**
* **1 objective function**

### Tools

* Python
* Gurobi
* Integer Linear Programming

---

## 2. Nelder-Mead Algorithm for Incomplete Pairwise Comparison Matrices

This project implements the **Nelder-Mead simplex algorithm** for the optimal completion of incomplete Pairwise Comparison Matrices (PCMs).

The implementation uses MATLAB's `fminsearch`, which is based on the standard Nelder-Mead algorithm, together with a coordinate transformation approach.

### Incomplete Pairwise Comparison Matrix

Consider the incomplete PCM $\mathbf{A}(\mathbf{x})$ with two unknown values:

$$
\mathbf{x} = (x_1,x_2)
$$

The matrix is:

$$
\mathbf{A}(\mathbf{x}) =
\begin{pmatrix}
1 & x_1 & \frac{1}{3} & x_2 \\
\frac{1}{x_1} & 1 & \frac{1}{9} & \frac{1}{3} \\
3 & 9 & 1 & 3 \\
\frac{1}{x_2} & 3 & \frac{1}{3} & 1
\end{pmatrix}
$$

### Constrained Eigenvalue Minimization

The optimal completion can be formulated as:

$$
\begin{aligned}
\min \quad & \lambda_{\max}\left(\mathbf{A}(\mathbf{x})\right) \\
\text{subject to} \quad
& \frac{1}{9} \leq x_1 \leq 9, \\
& \frac{1}{9} \leq x_2 \leq 9.
\end{aligned}
$$

### Result

Applying the **Nelder-Mead algorithm** produces the optimal solution:

$$
x_1 = 3,
\qquad
x_2 = 1
$$

with

$$
\lambda_{\max} = 4.
$$

The simplex iterations leading to the optimal solution are visualized using animation graphics.

### Topics

* Nelder-Mead optimization
* Simplex methods
* Pairwise Comparison Matrices
* Eigenvalue optimization
* Incomplete PCMs
* Coordinate transformations

---

## 3. Machine Learning Classification Algorithms

This project develops machine learning classifiers to predict whether a **loan application will be paid off or not**.

The historical loan dataset (`loan_train.csv`) is cleaned and prepared before applying several classification algorithms.

### Classification Algorithms

The following models are implemented:

1. **K-Nearest Neighbors (KNN)**
2. **Decision Tree**
3. **Support Vector Machine (SVM)**
4. **Logistic Regression**

### Model Evaluation

The classifiers are evaluated using appropriate performance metrics, including:

* **Jaccard Index**
* **F1-Score**
* **Log Loss**

The objective is to compare the performance of the different classification algorithms and identify the most suitable model for the loan classification problem.

### Topics

* Data preprocessing
* Supervised learning
* Classification
* Model evaluation
* Scikit-learn

---

## 4. Pyomo with GLPK, IPOPT, and Gurobi

This project explores mathematical optimization in Python using **Pyomo** and several optimization solvers.

### Pyomo

[Pyomo](https://www.pyomo.org/) is an open-source Python-based optimization modeling package that supports a wide range of optimization problems, including:

* Linear programming (LP)
* Mixed-integer programming (MIP)
* Nonlinear programming (NLP)
* Mixed-integer nonlinear programming (MINLP)

### GLPK

[GLPK (GNU Linear Programming Kit)](https://www.gnu.org/software/glpk/) is an open-source solver for:

* Linear programming
* Mixed-integer programming
* Related optimization problems

For users without an academic Gurobi license, GLPK is a useful open-source alternative for many linear and mixed-integer optimization problems.

Installation information:

[GLPK Installation — MacPorts](https://ports.macports.org/port/glpk/)

### IPOPT

[IPOPT (Interior Point Optimizer)](https://coin-or.github.io/Ipopt/) is an open-source solver designed for large-scale nonlinear optimization problems.

Installation resources:

[IPOPT Downloads](https://www.coin-or.org/download/binary/Ipopt/)

---

### Example: Optimization with Gurobi

Consider the following maximization problem.

Let:

$$
M = 10,
\qquad
T = 4
$$

The objective is:

$$
\max \sum_{m=1}^{M}\sum_{t=1}^{T} x_{m,t}
$$

subject to:

$$
2x_{2,t} - 8x_{3,t} \leq 0,
\qquad \forall t
$$

$$
x_{2,t} - 2x_{3,t-2} + x_{4,t} \geq 1,
\qquad \forall t>2
$$

$$
\sum_{m=1}^{M}x_{m,t} \leq 50,
\qquad \forall t
$$

$$
x_{1,t} - x_{2,t-1} + x_{3,t} + x_{4,t} \leq 10,
\qquad \forall t>1
$$

with variable bounds:

$$
0 \leq x_{m,t} \leq 10,
\qquad
\forall m,\forall t.
$$

### Tools

* Python
* Pyomo
* Gurobi
* GLPK
* IPOPT

### Source

Udemy:

[Optimization with Python — Linear, Nonlinear, and CPLEX/Gurobi](https://www.udemy.com/course/optimization-with-python-linear-nonlinear-and-cplex-gurobi/)

---

## 5. Multi-Objective Optimization with Python

This project explores **multi-objective optimization and decision-making** using Python and the [`pymoo`](https://pymoo.org/) framework.

The main goal is to balance multiple competing objectives and identify appropriate compromise solutions.

### Optimization Framework

The project follows these steps:

#### 1. Install and Import Libraries

Install `pymoo` and import the required Python libraries.

#### 2. Define the Optimization Problem

Create a custom problem class and define:

* Decision variables
* Objective functions
* Constraints
* Variable bounds

#### 3. Configure NSGA-II

The **NSGA-II** algorithm is initialized using:

```python
pop_size = 50
n_offsprings = 10
crossover = SBX(prob=0.9, eta=20)
mutation = PM(eta=25)
```

#### 4. Define the Termination Criterion

The optimization is terminated after:

```python
n_eval = 100
```

function evaluations.

#### 5. Visualize the Objective Space

Examine and visualize the resulting objective vectors.

#### 6. Normalize the Objectives

Normalize the objective vectors using the:

* **Ideal point**
* **Nadir point**

#### 7. Identify a Compromise Solution

Apply the following multi-criteria decision-making approaches:

* **Compromise Programming**
* **Pseudo-Weights**

> **Assumption:** The first objective is considered less important than the other objectives.

#### 8. Compare the Results

Visualize and compare the solutions obtained using the different decision-making methods.

### Topics

* Multi-objective optimization
* NSGA-II
* Pareto-optimal solutions
* Ideal and nadir points
* Compromise programming
* Pseudo-weights
* Decision-making

### Resources

* [pymoo Documentation](https://pymoo.org/)
* [Udemy — Multi-Objective Optimization with Python](https://www.udemy.com/course/multi-objective-optimization-with-python-bootcamp-a-z/?couponCode=KEEPLEARNING)

---

## Technologies and Tools

| Area                         | Tools / Libraries          |
| ---------------------------- | -------------------------- |
| Programming                  | Python, MATLAB             |
| Mathematical Optimization    | Pyomo, Gurobi, GLPK, IPOPT |
| Multi-Objective Optimization | pymoo, NSGA-II             |
| Machine Learning             | scikit-learn               |
| Optimization Algorithms      | Nelder-Mead                |
| Data Analysis                | NumPy, Pandas              |
| Visualization                | Matplotlib                 |

## References

1. Szádoczki, Z., Bozóki, S., & Tekile, H. (2022). *Filling in pattern designs for incomplete pairwise comparison matrices: (quasi-) regular graphs with minimal diameter*. **Omega, 107**, 102557.
2. [Pyomo](https://www.pyomo.org/)
3. [GLPK](https://www.gnu.org/software/glpk/)
4. [IPOPT](https://coin-or.github.io/Ipopt/)
5. [pymoo](https://pymoo.org/)
6. [Udemy — Optimization with Python](https://www.udemy.com/course/optimization-with-python-linear-nonlinear-and-cplex-gurobi/)
7. [Udemy — Multi-Objective Optimization with Python](https://www.udemy.com/course/multi-objective-optimization-with-python-bootcamp-a-z/?couponCode=KEEPLEARNING)




# Python-projects

# 1. Python with GUROBI to solve an integer linear programming problem  from the paper: 

Let $N=\{1,\ldots,22\}$ be the nodes, and let $P=\{i \in N,j \in N:i \text{ less than } j\}$ be the set of node pairs. For $(i,j) \in P$, let binary decision variable $X_{i,j}$ indicate whether $(i,j)$ is an edge. For $(i,j) \in P$ and $k \in N \setminus \{i,j\}$, let binary decision variable $Y_{i,j,k}$ indicate whether $k$ is a common neighbor of $i$ and $j$. For $(i,j) \in P$ let binary decision variable $SLACK_{i,j}$ be a slack variable. 

The goal is to solve the following  integer linear programming problem:


$\min{\sum_{(i,j) \in P}{SLACK_{i,j}}}$

 $\sum_{(i,j) \in P: k \in \{i,j\}}{X_{i,j} =5}$ for $k \in N$

 $X_{i,j}+\sum_{k \in N \setminus \{i,j\}}{Y_{i,j,k}} + SLACK_{i,j} \geq 1$ for $(i,j) \in P$
 
 $Y_{i,j,k} \leq X_{i,k}$ [for i<k] + $X_{k,i}$ [for k<i], $(i,j) \in P$ for $k \in N \setminus \{i,j\}$
 
$Y_{i,j,k} \leq X_{j,k}$ [for j<k]+ $X_{k,j}$ [for k<j], $(i,j) \in P$ for  $k \in N \setminus \{i,j\}$



- The integer program contains 5082 variables, 9493 constraints, and 1 objective function (when n=22)
  

Szádoczki, Z., Bozóki, S., & Tekile, H. A. (2022). Filling in pattern designs for incomplete pairwise comparison matrices:(quasi-) regular graphs with minimal diameter. Omega, 107, 102557.


# 2. The Nelder-Mead Algorithm simplex steps for the optimal completion of incomplete pairwise comparison matrices

Consider the incomplete PCM $\mathbf{A}$ with two unknowns $(x_1,x_2)=\mathbf{x}$: 


$\mathbf{A(x)} = 
\begin{pmatrix}
1 &x_1 &1/3 &x_2\\
1/x_1  &1  &1/9 &1/3\\
3 &9 &1 &3\\
{1/x_2 &3 &1/3 &1
\end{pmatrix}.$


- The constrained eigenvalue minimization problem can be constructed as follows:
  
$\begin{equation*}
\begin{aligned}
\min \quad & \lambda_{max} \mathbf{(A(x))}\\
\textrm{  s.t.} \quad &1/9\leq x_1 \leq 9 \\
  &1/9\leq x_2 \leq 9 . 
\end{aligned}
\end{equation*}$

That is:

$$ \begin{aligned} \min\quad & \lambda_{\max}\big(A(x)\big)\
\text{s.t.}\quad & \dfrac{1}{9}\le x_1 \le 9,\  & \dfrac{1}{9}\le x_2 \le 9. \end{aligned} $$

- Applying the Nelder-Mead algorithm, the algorithm arrives at the solution $x_1=3$ and $x_2=1$
with $\lambda_{max}=4$. Consequently, the simplex steps of the algorithm that leads to the optimal solution are provided in the form of animation graphics.

# 3.  Machine Learning Classification Algorithms

In this project, I will complete a notebook where I will build a classifier to predict whether a loan case will be paid off or not.
I load a historical dataset from  loan applications (loan_train.csv), clean the data, and apply different classification algorithm on the data. I am expected to use the following algorithms to build your models:

- k-Nearest Neighbour
- Decision Tree
- Support Vector Machine
- Logistic Regression

The results is reported as the accuracy of each classifier, using the following metrics when these are applicable:

- Jaccard index
- F1-score
- LogLoass


# 4.  Pyomo with solvers GLPK, IPOPT and GUROBI
- Pyomo is a Python-based open-source software package that supports a diverse set of optimization capabilities for formulating, solving, and analyzing optimization models (framework for linear and nonlinear programming with many potential libraries). Refer http://www.pyomo.org

- The GLPK (GNU Linear Programming Kit) package is intended for solving large-scale linear programming (LP), mixed integer programming (MIP), and other related problems. It is a set of routines written in ANSI C and organized in the form of a callable library. Refer https://www.gnu.org/software/glpk/
- If you don’t have acadeic license, it is better to use this solver inside Pyomo. See the installation page: https://ports.macports.org/port/glpk/

- Ipopt (Interior Point Optimizer, pronounced "Eye-Pea-Opt") is an open source software package for large-scale nonlinear optimization. Refer the documentation in https://coin-or.github.io/Ipopt/
For installation: see https://www.coin-or.org/download/binary/Ipopt/
# Example:  

Solve the following maximization problem using GUROBI (set upper limit of m and t as M=10, T=4, respectively):

$\max \sum_m \sum_t x_{m,t}$


$2x_{2,t} - 8x_{3,t} \leq 0 \, \forall t$


 $x_{2,t} - 2x_{3,t-2} + x_{4,t} \geq 1\, \forall t>2$ 

 
 $\sum_m x_{m,t}  \leq 50 \, \forall t$

 
 $x_{1,t} - x_{2,t-1} + x_{3,t} + x_{4,t}\leq 10\, \forall t>1$

 
 $0 \leq x_{m,t} \leq 10 \, \forall m, \forall t$



Source: Udemy 
https://www.udemy.com/course/optimization-with-python-linear-nonlinear-and-cplex-gurobi/

# 5.  Multiobjective optimization with Python: 

Multi-Objective Optimization and Decision-Making with pymoo: Balancing Objectives, Finding Solutions. 

https://pymoo.org

Steps to solve the given problem/Exercise:
1.    Install pymoo and import all the required libraries accordingly. 
2.   Develop a class and define a problem.
3. Initialize NSGA-II algorithm using below parameters:
  pop_size = 50,
  n_offsprings = 10,
  cross_over = SBX(prob=0.9, eta=20),
  mutation = PM(eta=25).
4. Use n_eval = 100 termination criteria.
5. Check out your objectives vector and visualize it.
6. Normalize the objective vector using ideal point and nadir point.
7. Use Compromise Programming and Pseudo-weights methods to find the Optimum Point.
 Note! Imagine that the first objective is less important than the other for us (assumption).
8. Visualize the results of each method and compare.


Source: https://www.udemy.com/course/multi-objective-optimization-with-python-bootcamp-a-z/?couponCode=KEEPLEARNING
