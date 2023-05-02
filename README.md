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


# 2. The Nelder-Mead Algorithm simplex steps for the optimal completion of incomplete PCMs
Consider the incomplete PCM $\mathbf{\hat{A}}$ with {\color{red}two unknowns} $(x_1,x_2)=\mathbf{x}$: 
$$\mathbf{\hat{A}(x)} = 
\begin{pmatrix}
1 &{\color{red}x_1} &1/3 &{\color{red}x_2}\\
{\color{red}1/x_1}  &1  &1/9 &1/3\\
3 &9 &1 &3\\
{\color{red}1/x_2} &3 &1/3 &1
\end{pmatrix}.$$

 The constrained eigenvalue minimization problem can be constructed as follows: 
\begin{equation*}\label{Problem:2}
\begin{aligned}
\min \quad & \lambda_{max} \mathbf{(\hat{A}(x))}\\
\textrm{s.t.} \quad &1/9\leq x_1 \leq 9 \\
  &1/9\leq x_2 \leq 9 . 
\end{aligned}
\end{equation*}

Applying the Nelder-Mead algorithm, the algorithm arrives at the solution {\color{red}$x_1=3$} and {\color{red}$x_2=1$} 
with $\lambda_{max}=4$. Consequently, the simplex steps of the algorithm that leads to the optimal solution are provided.

# 3.  Machine Learning Classification Algorithms

In this project, you will complete a notebook where you will build a classifier to predict whether a loan case will be paid off or not.
You load a historical dataset from  loan applications, clean the data, and apply different classification algorithm on the data. You are expected to use the following algorithms to build your models:

- k-Nearest Neighbour
- Decision Tree
- Support Vector Machine
- Logistic Regression

The results is reported as the accuracy of each classifier, using the following metrics when these are applicable:

- Jaccard index
- F1-score
- LogLoass

# Setup Instructions:
A-Create an account in Watson Studio if you dont have (If you already have it, jump to step B).
- Browse into https://www.ibm.com/cloud/watson-studio
- Click on 'Start your free trial'
- Enter your email, and click 'Next'
- Enter your Name, and choose a Password. Then click on 'Create Account'
- Go to your email, and confirm your account.
- Click on 'Proceed'
In "Select Organization and Space" form, leave everything as default, and click on 'Continue'
It is done. Click on 'Get started!'

# B-Sign in into Watson Studio and import your notebook
- Sign in into https://www.ibm.com/cloud/watson-studio
- Click on 'New Project'
- Select 'Data Science' as type of project.
- Give a name to your project, and a description for your reference, then setup your project as following and click "Create".

Notice 1: because you are going to share this project with your peer for evaluation, please make sure you have unchecked Restrict who can be a collaborator

Notice 2: You have to create an IBM Object Storage, if you dont have any IBM Object Storage (you can use the free Lite plan)

- From the top-right, Click on 'Add to project', and then select 'Notebook'. C

- In the 'New notebook' form, click on 'From URL', and enter the Notebook URL: https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/ML0101ENv3/labs/ML0101EN-Proj-Loan-py-v1.ipynb

- Give the notebook a proper name and description and click on Create Notebook to initialize the notebook

# C. Complete the notebook

- Start running the notebook
- Complete the notebook based on the description in the notebook.

