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
 
CODE 
;; Block World Problem using Depth-Limited DFS in Lisp 
 
(defun same-state (state1 state2) 
  "Check if two states represent the same configuration." 
  (equal (sort (copy-list state1) #'string<) 
         (sort (copy-list state2) #'string<))) 
 
(defun is-clear (block state) 
  "A block is clear if no other block is on it." 
  (not (find-if (lambda (x) (equal (second x) block)) state))) 
 
(defun replace-position (state old new) 
  "Replace one relationship in the state list with a new one." 
  (mapcar (lambda (x) (if (equal x old) new x)) state)) 
 
(defun make-move (state) 
  "Generate all possible moves from the given state." 
  (let ((moves '())) 
    (dolist (pair state) 
      (let ((block (first pair)) 
            (support (second pair))) 
        (when (and (not (equal support 'table)) 
                   (is-clear block state)) 
          (let ((new-state (replace-position state pair (list block 'table)))) 
            (push (list new-state (list 'move block 'table)) moves)))) 
      ;; Try moving block onto another clear block 
      (dolist (target state) 
        (let ((block (first pair)) 
              (target-block (first target))) 
          (when (and (not (equal block target-block)) 
                     (is-clear block state) 
                     (is-clear target-block state)) 
            (let ((new-state (replace-position state pair (list block target-block))))               (push (list new-state (list 'move block target-block)) moves))))))     moves)) 
 
(defun dfs (state goal path depth max-depth) 
  "Perform depth-limited DFS." 
  (cond 
    ((same-state state goal) 
     (reverse path))     ((>= depth max-depth) 
     nil) 
    (t 
     (let ((next-states (make-move state)) 
           (solution nil)) 
       (dolist (ns next-states) 
         (let ((new-state (first ns)) 
               (move (second ns))) 
           (unless (equal new-state state) 
             (setq solution (dfs new-state goal (cons move path) (+ depth 1) max-depth)) 
             (when solution (return solution))))) 
       solution)))) 
 
(defun solve-blocks (start goal) 
  "Solve the block world problem between START and GOAL with depth limit."   (dfs start goal '() 0 10)) 
 
(defun print-solution (solution) 
  "Nicely print out the sequence of moves." 
  (if (null solution) 
      (format t "No solution found.~%") 
      (progn 
        (format t "SOLUTION FOUND:~%") 
        (dolist (move solution) 
          (let ((block (second move)) 
                (target (third move))) 
            (if (equal target 'table) 
                (format t "Move ~a to table~%" block) 
                (format t "Move ~a onto ~a~%" block target))))))) 
 
(defun example1 () 
  (format t "~%Example 1~%Initial: B on A, A on table~%Goal: A on B, B on table~%~%") 
  (let* ((initial '((a table) (b a))) 
         (goal '((b table) (a b))) 
         (moves (solve-blocks initial goal))) 
    (print-solution moves))) 
 
(defun example2 () 
  (format t "~%Example 2~%Initial: A, B, C all on table~%Goal: A on B, B on C, C on A (impossible circle)~%~%") 
  (let* ((initial '((a table) (b table) (c table))) 
         (goal '((a b) (b c) (c a))) 
         (moves (solve-blocks initial goal))) 
    (print-solution moves))) 
 
(defun run-examples () 
  (example1) 
  (example2) 
  (format t "~%Program completed.~%")) 
 
(run-examples) 
 
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
 

