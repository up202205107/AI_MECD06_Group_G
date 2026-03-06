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

$$D \le \sum_n T_n$$

The number of deliveries to teams of size $N$ cannot exceed $T_N$.


## Objective Function
The goal is to maximize the sum of the squares of the number of unique ingredients per team. Let $I_j$ be the set of unique ingredients in all pizzas assigned to team $j$.


$$\text{Maximize } \sum_{j} (|I_j|)^2$$

# Solving with Neighborhood Search & Greedy Heuristics
Because this problem is a variation of the Set Covering Problem (trying to cover as many unique ingredients as possible with limited subsets/pizzas), it is NP-hard. For the Hash Code competition, a hybrid approach is most effective:

Step 1: Initial Greedy Construction
A "Greedy" approach provides a fast, feasible starting solution:
Prioritize large teams: Since the score is the square of unique ingredients, larger teams (4-person) generally offer a higher scoring potential ($10^2=100$ vs $5^2=25$).


Ingredient Diversity: For the first team, pick the pizza with the most ingredients. For the second pizza for that same team, pick the one that adds the most new unique ingredients.


Repeat until you run out of pizzas or teams.

Step 2: Neighborhood Search (Refinement)
Once you have a greedy solution, you can improve it using the Move types we discussed:
Swap: Swap Pizza A (in Team 1) with Pizza B (in Team 2). If the total sum of $(|I_1|)^2 + (|I_2|)^2$ increases, keep the change.
Relocate: Move an unassigned pizza into a team if it provides more unique ingredients than one currently assigned.

## Applying Simulated Annealing
To reach a near-optimal score for large datasets (like $M=100,000$), you can apply Simulated Annealing:
Start with your greedy solution.
Perturb: Randomly swap two pizzas between different teams.
Calculate $\Delta E$: Find the change in the total score.
Acceptance: * If the score improves, always accept.
If the score decreases, accept it with probability $P = e^{-\frac{\Delta E}{T}}$ to escape local optima.
Cooling: Slowly lower the "temperature" until the routes (pizza assignments) stabilize.
Summary of the Example Solution
Using the provided example:

Delivery 1 (2-person team): Given Pizzas 1 and 4.
Ingredients: {mushroom, tomato, basil} + {chicken, basil} = 4 unique.


Score: $4^2 = 16$.


Delivery 2 (3-person team): Given Pizzas 0, 2, and 3.
Ingredients: {onion, pepper, olive, chicken, mushroom, tomato, basil} = 7 unique.


Score: $7^2 = 49$.


Total: $16 + 49 = 65$.


Would you like me to write a Python pseudo-code implementation of the Greedy Diversity algorithm for this specific pizza problem?
