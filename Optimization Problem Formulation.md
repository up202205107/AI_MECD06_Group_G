# COP Model Formulation
To solve this mathematically, we define the following components based on the problem description:

## Variables and Domains

$M$: Total available pizzas $\{P_0, P_1, ..., P_{M-1}\}$.

$T_N$: Number of teams of size $N \in \{2, 3, 4\}$.

Decision Variable ($x_{i,j, N}$): A binary variable where $x_{i,j, N} = 1$ if Pizza $i$ is assigned to a Team $j$ with $N$ members, and $0$ otherwise.

$D$: Total number of deliveries

## Constraints

Pizza Uniqueness: Each pizza can be delivered to at most one team. 

$$\sum_{j} x_{i,j} \le 1 \quad \forall i \in \{0, \dots, M-1\}$$

Team Capacity: If a team $j$ of size $N$ receives a delivery, the team must receive exactly $N$ pizzas.

$$\sum_{i} x_{i,j, N} \in \{0, N\} \quad \forall \text{ Team } j \text{ of size } N$$

Team Count: 

The Total number of deliveries to teams cannot exceed the Total number of Teams

$$D \le \sum_N T_N$$

The number of deliveries to teams of size $N$ cannot exceed $T_N$.

$$D_N \le T_N$$

## Objective Function
The goal is to maximize the sum of the squares of the number of unique ingredients per team. Let $I_j$ be the set of unique ingredients in all pizzas assigned to team $j$.


$$\text{Maximize } \sum_{j} (|I_j|)^2$$

