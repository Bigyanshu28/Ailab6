AIM: To implement Block World Problem using LISP with Depth-Limited Depth First Search (DFS). 
Procedure 
1.	State Representation 
•	Each state is represented as a list of pairs, where each pair defines a block and its support. Example: 
'((a table) (b a)) 
•	Here: 
o	a is on the table. 
o	b is on a. 
 
2.	Goal Test 
•	The function same-state checks whether two states represent the same configuration. It compares sorted versions of both states to ensure order does not matter. 
 
3.	Move Generation 
•	The function make-move generates all valid successor states from the current state. 
Moves include: 
•	Moving a block to the table, if it’s currently on another block. 
•	Moving a block onto another clear block, if both are clear. 
Helper functions: 
•	is-clear – checks if no block is on top of the given block. 
4.	Depth-Limited DFS 
•	The function dfs performs depth-limited search to find a sequence of moves from start to goal. 
Steps: 
a.	If the current state equals the goal, return the solution path. 
b.	If the current depth is less than the maximum limit, generate all valid moves. 
c.	Recursively explore each new state. 
d.	Prevent revisiting the same state to avoid loops. 
e.	Accumulate valid moves into a list representing the final solution. 
 
5. Solution Printing 
	• 	The function print-solution displays the sequence of moves in a readable format: 
 
 
1.	Time Complexity: 
•	In the worst case, Depth-Limited DFS explores all nodes up to a given depth d. 
•	For a branching factor b (average number of successors per state): 
𝑇(𝑛)=𝑂(𝑏𝑑) 
 
•	Here: 
o 	b = number of possible moves from a state (depends on number of blocks) o 	d = depth limit (maximum move sequence length) 
Example: 
If there are 3 blocks and a max depth of 10 → possible states grow exponentially. 
 
2.	Space Complexity: 
•	DFS only stores the current path and recursion stack → linear space usage: 
𝑆(𝑛)=𝑂(𝑏×𝑑) 
 
or simply O(d) when only tracking the current branch. 
 

