### EXP NO: 05

### DATE: 23/05/2022

# <p align = "center"> Hill Climbing Algorithm for Eight Queens Problem </p>
 
## AIM

To develop a code to solve eight queens problem using the hill-climbing algorithm.

## THEORY
Hill climbing algorithm is a local search algorithm which continuously moves in the direction of increasing elevation/value to find the peak of the mountain or best solution to the problem. It terminates when it reaches a peak value where no neighbor has a higher value.Itis a technique which is used for optimizing the mathematical problems. One of the widely discussed examples of Hill climbing algorithm is Traveling-salesman Problem in which we need to minimize the distance traveled by the salesman.It is also called greedy local search as it only looks to its good immediate neighbor state and not beyond that.Hill Climbing is mostly used when a good heuristic is available.

## DESIGN STEPS

A node of hill climbing algorithm has two components which are state and value.

 
### STEP 1:
Import the necessary libraries

### STEP 2:
Define the Intial State and calculate the objective function for that given state

### STEP 3:
Make a decision whether to change the state with a smaller objective function value, or stay in the current state.

### STEP 4:
Repeat the process until the total number of attacks, or the Objective function, is zero.

### STEP 5:
Display the necessary states and the time taken.


## PROGRAM
```
Developed By
Student name : Kumaran.B
Reg.no : 212220230026
```
```python
%matplotlib inline
import matplotlib.pyplot as plt
import random
import math
import sys
from collections import defaultdict, deque, Counter
from itertools import combinations
from IPython.display import display
from notebook import plot_NQueens

class Problem(object):
    def __init__(self, initial=None, goal=None, **kwds): 
        self.__dict__.update(initial=initial, goal=goal, **kwds) 
        
    def actions(self, state):        
        raise NotImplementedError
    def result(self, state, action): 
        raise NotImplementedError
    def is_goal(self, state):        
        return state == self.goal
    def action_cost(self, s, a, s1): 
        return 1
    
    def __str__(self):
        return '{0}({1}, {2})'.format(
            type(self).__name__, self.initial, self.goal)

class Node:
    def __init__(self, state, parent=None, action=None, path_cost=0):
        self.__dict__.update(state=state, parent=parent, action=action, path_cost=path_cost)
    def __str__(self): 
        return '<{0}>'.format(self.state)
    def __len__(self): 
        return 0 if self.parent is None else (1 + len(self.parent))
    def __lt__(self, other): 
        return self.path_cost < other.path_cost
failure = Node('failure', path_cost=math.inf) # Indicates an algorithm couldn't find a solution.
cutoff  = Node('cutoff',  path_cost=math.inf) # Indicates iterative deepening search was cut off.
def expand(problem, state):
    return problem.actions(state)

class NQueensProblem(Problem):

    def __init__(self, N):
        super().__init__(initial=tuple(random.randint(0,N-1) for _ in tuple(range(N))))
        self.N = N

    def actions(self, state):
        """ finds the nearest neighbors"""
        neighbors = []
        for i in range(self.N):
            for j in range(self.N):
                if j == state[i]:
                    continue
                s1 = list(state)
                s1[i]=j
                new_state = tuple(s1)
                yield Node(state=new_state)

    def result(self, state, row):
        """Place the next queen at the given row."""
        col = state.index(-1)
        new = list(state[:])
        new[col] = row
        return tuple(new)

    def conflicted(self, state, row, col):
        """Would placing a queen at (row, col) conflict with anything?"""
        return any(self.conflict(row, col, state[c], c)
                   for c in range(col))

    def conflict(self, row1, col1, row2, col2):
        """Would putting two queens in (row1, col1) and (row2, col2) conflict?"""
        return (row1 == row2 or  # same row
                col1 == col2 or  # same column
                row1 - col1 == row2 - col2 or  # same \ diagonal
                row1 + col1 == row2 + col2)  # same / diagonal

    def goal_test(self, state):
        return not any(self.conflicted(state, state[col], col)
                       for col in range(len(state)))

    def h(self, node):
        """Return number of conflicting queens for a given node"""
        num_conflicts = 0
        # Write your code here
        for (r1,c1) in enumerate(node.state):
            for (r2,c2) in enumerate(node.state):
                if (r1,c1)!=(r2,c2):
                    num_conflicts += self.conflict(r1,c1,r2,c2) 
        return num_conflicts

def shuffled(iterable):
    """Randomly shuffle a copy of iterable."""
    items = list(iterable)
    random.shuffle(items)
    return items

def argmin_random_tie(seq, key):
    """Return an element with highest fn(seq[i]) score; break ties at random."""
    return min(shuffled(seq), key=key)

def hill_climbing(problem,iterations = 10000):
    # as this is a stochastic algorithm, we will set a cap on the number of iterations        
    current = Node(problem.initial)
    i=1
    while i < iterations:
        neighbors = expand(problem,current.state)
        if not neighbors:
            break
        neighbour = argmin_random_tie(neighbors,key=lambda node:problem.h(node))
        if problem.h(neighbour)<=problem.h(current):
            current.state= neighbour.state
            if problem.goal_test(current.state)==True:
                print('The Goal state is reached at {0}'.format(i))
                return current              
        i += 1        
    return current 
nq1=NQueensProblem(8)    
plot_NQueens(nq1.initial)
n=[8,16,32,64]
time=[]
case=1
for i in n:
    nq1=NQueensProblem(i)
    n1 = Node(state=nq1.initial)
    num_conflicts = nq1.h(n1)
    print("Case {0}:\tN-value:{1}".format(case,i))
    print("Initial Conflicts = {0}".format(num_conflicts))
    import time
    start=time.time()
    sol1=hill_climbing(nq1,iterations=20000)
    end=time.time()
    print(sol1)
    sol1.state
    num_conflicts = nq1.h(sol1)
    print("Final Conflicts = {0}".format(num_conflicts))
    print("The total time required for 20000 iterations is {0:.4f} seconds\n\n".format(end-start))
    
    case+=1
    


```
## OUTPUT:

### Initial positions
![Screenshot (221)](https://user-images.githubusercontent.com/75243072/175769725-885d97f6-5755-4707-b3fb-1082d687bb70.png)

### <br><br><br><br>Final positions
![Screenshot (243)](https://user-images.githubusercontent.com/75243072/175769763-57aca1f7-7fb1-4050-96e0-a5b51a8de9bb.png)



<br><br><br><br>![Screenshot (239)](https://user-images.githubusercontent.com/75243072/175769751-a2e9e1cc-1ca4-4181-9b98-b0d3a7dcdfdf.png)




## <br><br><br><br><br><br><br><br><br><br>Solution Justification:
When the state is larger, the longer it take to complete the search.The iteration will undergo untill the objective function reaches zero.


## <br><br>Time Complexity Plot

#### Plotting a graph for various value of N and time(seconds)
![Screenshot (222)](https://user-images.githubusercontent.com/75243072/175769782-9da47ffe-460c-4e22-ae72-3e087236226b.png)
 
## <br><br><br><br><br><br><br><br><br><br><br><br>RESULT:
Hence, a code to solve eight queens problem using the hill-climbing algorithm has been implemented.



