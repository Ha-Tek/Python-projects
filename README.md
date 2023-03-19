# Python-projects

# 1. Python with GUROBI to solve an integer linear programming problem  from the paper: 
Szádoczki, Z., Bozóki, S., & Tekile, H. A. (2022). Filling in pattern designs for incomplete pairwise comparison matrices:(quasi-) regular graphs with minimal diameter. Omega, 107, 102557.

Let $N=\{1,\ldots,22\}$ be the nodes, and let $P=\{i \in N,j \in N:i \text{ less than } j\}$ be the set of node pairs. For $(i,j) \in P$, let binary decision variable $X_{i,j}$ indicate whether $(i,j)$ is an edge. For $(i,j) \in P$ and $k \in N \setminus \{i,j\}$, let binary decision variable $Y_{i,j,k}$ indicate whether $k$ is a common neighbor of $i$ and $j$. For $(i,j) \in P$ let binary decision variable $SLACK_{i,j}$ be a slack variable. 




- The integer program contains 5082 variables, 9493 constraints, and 1 objective function (when n=22)

To see this, let's count the number of variables and constraints in each part of the problem.

Part 1: Variables

There are $|P| = \binom{22}{2} = 231$ variables $X_{i,j}$ and $|P||N\setminus{i,j}| = 231 \cdot 20 = 4620$ variables $Y_{i,j,k}$, as well as $|P| = 231$ slack variables $SLACK_{i,j}$. So the total number of variables is $231 + 4620 + 231 = 5082$.

Part 2: Constraints

There are $3 \cdot 22 = 66$ constraints of the form $\sum_{(i,j) \in P: k \in {i,j}} X_{i,j} = 5$ for $k \in N$, which gives a total of $66 \cdot 5 = 330$ constraints.

There are $|P| = 231$ constraints of the form $X_{i,j} + \sum_{k \in N \setminus {i,j}} Y_{i,j,k} + SLACK_{i,j} \geq 1$ for $(i,j) \in P$, which gives a total of 231 constraints.

Finally, there are $2|P||N\setminus{i,j}| = 2 \cdot 231 \cdot 20 = 9240$ constraints of the form $Y_{i,j,k} \leq X_{i,k} (\text{if i less than k}) + X_{k,i}(\text{if k less than i})$ and $Y_{i,j,k} \leq (\text{if j less than k})X_{j,k} + X_{k,j}(\text{if k less than j})$ for $(i,j) \in P$ and $k \in N \setminus {i,j}$, which gives a total of 9240 constraints.

So the total number of constraints is $330 + 231 + 9240 = 9493$.

Therefore, the integer program contains 5082 variables, 9493 constraints, and 1 objective function, as claimed.

# An example of $Y_{i,j,k}$ and $SLACK_{i,j}$:

Let's consider a simple undirected graph with five nodes: $N = {1, 2, 3, 4, 5}$, and let $X_{i,j}$ indicate whether there is an edge between nodes $i$ and $j$. Suppose we have $X_{1,2} = 1$, $X_{2,3} = 1$, $X_{3,4} = 1$, and $X_{4,5} = 1$. Then, we can define $Y_{i,j,k}$ as follows:


- $Y_{1,2,3} = 1$ since nodes $1$ and $2$ share a common neighbor with node $3$ (namely, node $2$).
- $Y_{1,2,4} = 0$ since nodes $1$ and $2$ do not share a common neighbor with node $4$.
- $Y_{2,3,1} = 1$ since nodes $2$ and $3$ share a common neighbor with node $1$ (namely, node $2$).
- $Y_{2,3,4} = 1$ since nodes $2$ and $3$ share a common neighbor with node $4$ (namely, node $3$).
- $Y_{3,4,5} = 1$ since nodes $3$ and $4$ share a common neighbor with node $5$ (namely, node $4$).
- $Y_{1,3,4} = 0$ since nodes $1$ and $3$ do not share a common neighbor with node $4$.

Note that a node $k$ is a neighbor of a node $i$ if there exists an edge between nodes $i$ and $k$. Similarly, we say that a node $k$ is a neighbor of a node $j$ if there exists an edge between nodes $j$ and $k$. Therefore, when we say that $k$ is a common neighbor of nodes $i$ and $j$, we mean that $k$ is connected to both nodes $i$ and $j$ by an edge in the given graph.
Note that in this example, we only defined $Y_{i,j,k}$ for a subset of all possible combinations of nodes $(i,j,k)$. In the actual formulation, $Y_{i,j,k}$ is defined for all $(i,j)$ pairs and all nodes $k$ that are not $i$ or $j$.

- Let also be $N = {1, 2, 3, 4, 5}$, and let $X_{i,j}$ indicate whether there is an edge between nodes $i$ and $j$. Suppose we have $X_{1,2} = 1$, $X_{2,3} = 1$, $X_{3,4} = 1$, and $X_{4,5} = 1$. In this case, $N = {1, 2, 3, 4, 5}$, and $P = {(i,j): i,j \in N, i \text{less than} j} = {(1,2), (1,3), (1,4), (1,5), (2,3), (2,4), (2,5), (3,4), (3,5), (4,5)}$.

The slack variable $SLACK_{i,j}$ measures the amount by which the sum of the degrees of nodes $i$ and $j$ exceeds $2$ (i.e., the number of edges that must be incident to $i$ and $j$ in a perfect matching). In this case, since we have $X_{1,2} = X_{2,3} = X_{3,4} = X_{4,5} = 1$, the sum of the degrees of any two nodes is exactly $2$, so $SLACK_{i,j}$ is always zero. In other words, it measures the "slack" in the degree constraints, i.e., how much the degree of nodes $i$ and $j$ can be increased without violating the degree constraint, or the number of additional edges that can be added without violating the constraints of the problem. 

Explicitly, the definition of $SLACK_{i,j}$ is:
$$SLACK_{i,j} = \max [0, X_{i,j}+  \sum_{k \in N \setminus {i,j}} X_{i,k}+\sum_{k \in N \setminus {i,j}} X_{j,k}-2]$$
Since $X_{1,2} = X_{2,3} = X_{3,4} = X_{4,5} = 1$, we have
$$SLACK_{1,2} = \max [0, X_{1,2}+  \sum_{k \in  {3,4,5}} X_{1,k}+\sum_{k \in  {3,4,5}} X_{2,k}-2]$$
$$ = \max {0, 1+  0+0-2}=0,$$

and similarly for all other pairs $(i,j) \in P$.

# A simple example to understand the first cinstraint $\sum_{(i,j) \in P: k \in \{i,j\}}{X_{i,j} = degree[k]}$:

Let $N=\{1,2,3,4\}$ be the nodes, and assume we want to find a graph with degree 2 (for the sake of simplicity). 

The constraint $\sum_{(i,j) \in P: k \in \{i,j\}}{X_{i,j} =2}$ enforces that each node $k$ must be incident to exactly two edges.

Let's consider an example to illustrate this constraint. Suppose we have a graph with nodes $N={1,2,3,4}$, and edges $(1,2)$, $(1,3)$, and $(2,4)$.
Then, we have $X_{1,2}=X_{1,3}=X_{2,4}=1$ and all other $X_{i,j}=0$. Now, let's check the constraint for each node $k$:

For $k=1$, we have $(i,j)\in{(1,2),(1,3)}$, so $\sum_{(i,j)\in P:k\in{i,j}}X_{i,j}=X_{1,2}+X_{1,3}=2$.
For $k=2$, we have $(i,j)=(1,2)$ and $(i,j)=(2,4)$, so $\sum_{(i,j)\in P:k\in{i,j}}X_{i,j}=X_{1,2}+X_{2,4}=2$.
For $k=3$, we have $(i,j)=(1,3)$ and $(i,j)=(3,4)$, so $\sum_{(i,j)\in P:k\in{i,j}}X_{i,j}=X_{1,3}+0=1$.
For $k=4$, we have $(i,j)=(2,4)$ and $(i,j)=(3,4)$, so $\sum_{(i,j)\in P:k\in{i,j}}X_{i,j}=0+X_{2,4}=1$.
Therefore, the constraint is satisfied for all nodes $k\in N$. Intuitively, this constraint ensures that each node is connected to exactly two other nodes, which is a property of a planar graph.


# 2.  Machine Learning Classification Algorithms

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

