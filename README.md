# Python-projects

# 1. Python with GUROBI to solve an integer linear programming problem  from the paper: 
Szádoczki, Z., Bozóki, S., & Tekile, H. A. (2022). Filling in pattern designs for incomplete pairwise comparison matrices:(quasi-) regular graphs with minimal diameter. Omega, 107, 102557.

Let $N=\{1,\ldots,22\}$ be the nodes, and let $P=\{i \in N,j \in N:i \text{ less than } j\}$ be the set of node pairs. For $(i,j) \in P$, let binary decision variable $X_{i,j}$ indicate whether $(i,j)$ is an edge. For $(i,j) \in P$ and $k \in N \setminus \{i,j\}$, let binary decision variable $Y_{i,j,k}$ indicate whether $k$ is a common neighbor of $i$ and $j$. For $(i,j) \in P$ let binary decision variable $SLACK_{i,j}$ be a slack variable. 

$\min{\sum_{(i,j) \in P}{SLACK_{i,j}}}$

 $\sum_{(i,j) \in P: k \in \{i,j\}}{X_{i,j} =5}$ for $k \in N$

 $X_{i,j}+\sum_{k \in N \setminus \{i,j\}}{Y_{i,j,k}} + SLACK_{i,j} \geq 1$ for $(i,j) \in P$
 
 $Y_{i,j,k} \leq X_{i,k}$ [for i<k] + $X_{k,i}$ [for k<i], $(i,j) \in P$ for $k \in N \setminus \{i,j\}$
 
$Y_{i,j,k} \leq X_{j,k}$ [for j<k]+ $X_{k,j}$ [for k<j], $(i,j) \in P$ for  $k \in N \setminus \{i,j\}$


- The integer program contains 5082 variables, 9493 constraints, and 1 objective function (when n=22)


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
\begin{equation*}
\begin{aligned}
\min \quad & \lambda_{max} \mathbf{(A(x))}\\
\textrm{s.t.} \quad &1/9\leq x_1 \leq 9 \\
  &1/9\leq x_2 \leq 9 . 
\end{aligned}
\end{equation*}

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


# 4.  Pymoo with solvers GLPK, IPOPT and GUROBI
- Pymoo is a Python-based open-source software package that supports a diverse set of optimization capabilities for formulating, solving, and analyzing optimization models (framework for linear and nonlinear programming with many potential libraries). Refer http://www.pyomo.org

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
