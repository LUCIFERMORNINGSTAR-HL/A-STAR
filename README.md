<h1>ExpNo 4 : Implement A* search algorithm for a Graph</h1> 
<h3>Name: R K JAYA KRISNAA      </h3>
<h3>Register Number:  212223223002         </h3>
<H3>Aim:</H3>
<p>To ImplementA * Search algorithm for a Graph using Python 3.</p>
<H3>Algorithm:</H3>

``````
// A* Search Algorithm
1.  Initialize the open list
2.  Initialize the closed list
    put the starting node on the open 
    list (you can leave its f at zero)

3.  while the open list is not empty
    a) find the node with the least f on 
       the open list, call it "q"

    b) pop q off the open list
  
    c) generate q's 8 successors and set their 
       parents to q
   
    d) for each successor
        i) if successor is the goal, stop search
        
        ii) else, compute both g and h for successor
          successor.g = q.g + distance between 
                              successor and q
          successor.h = distance from goal to 
          successor (This can be done using many 
          ways, we will discuss three heuristics- 
          Manhattan, Diagonal and Euclidean 
          Heuristics)
          
          successor.f = successor.g + successor.h

        iii) if a node with the same position as 
            successor is in the OPEN list which has a 
           lower f than successor, skip this successor

        iV) if a node with the same position as 
            successor  is in the CLOSED list which has
            a lower f than successor, skip this successor
            otherwise, add  the node to the open list
     end (for loop)
  
    e) push q on the closed list
    end (while loop)

``````

<hr>
<h2>Sample Graph I</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/b1377c3f-011a-4c0f-a843-516842ae056a)

## Program : 

```PYTHON
# ExpNo 4 : Implement A* search algorithm for a Graph
# Name: R K JAYA KRISNAA
# Register Number: 212223223002

def a_star_algorithm(start_node, stop_node, graph, heuristics):
    # open_set is the set of nodes which have been visited, but whose neighbors haven't all been inspected
    open_set = {start_node}
    # closed_set is the set of nodes which have been visited and whose neighbors have been inspected
    closed_set = set()
    
    # g contains current distances from start_node to all other nodes
    g = {start_node: 0}
    
    # parents contains an adjacency map of all nodes
    parents = {start_node: start_node}
    
    while len(open_set) > 0:
        n = None
        
        # Find a node with the lowest value of f(n) = g(n) + h(n)
        for v in open_set:
            if n is None or g[v] + heuristics[v] < g[n] + heuristics[n]:
                n = v
                
        if n is None:
            print('Path does not exist!')
            return None
            
        # If the current node is the stop_node, we are done
        if n == stop_node:
            path = []
            while parents[n] != n:
                path.append(n)
                n = parents[n]
            path.append(start_node)
            path.reverse()
            print(f"Path found: {path}")
            return path
            
        # For all neighbors of the current node
        for (m, weight) in graph.get(n, []):
            # If the current node isn't in both open_set and closed_set, add it to open_set
            if m not in open_set and m not in closed_set:
                open_set.add(m)
                parents[m] = n
                g[m] = g[n] + weight
            else:
                # If it's already in the open or closed set, check if we found a better path
                if g.get(m, float('inf')) > g[n] + weight:
                    g[m] = g[n] + weight
                    parents[m] = n
                    if m in closed_set:
                        closed_set.remove(m)
                        open_set.add(m)
                        
        # Remove n from the open_set, and add it to closed_set
        open_set.remove(n)
        closed_set.add(n)
        
    print('Path does not exist!')
    return None

def main():
    # 1. Read Number of Vertices (V) and Edges (E)
    try:
        v, e = map(int, input().split())
    except ValueError:
        return
        
    graph = {}
    
    # 2. Read all the edges
    for _ in range(e):
        u, v_node, w = input().split()
        w = int(w)
        # Undirected graph implementation
        if u not in graph: graph[u] = []
        if v_node not in graph: graph[v_node] = []
        graph[u].append((v_node, w))
        graph[v_node].append((u, w))
        
    heuristics = {}
    start_node = None
    goal_node = None
    
    # 3. Read heuristics for all vertices
    for _ in range(v):
        node, h = input().split()
        h = int(h)
        heuristics[node] = h
        
        # The first node provided in heuristics is typically the start node
        if start_node is None:
            start_node = node
            
        # The node with a heuristic of 0 is the goal node
        if h == 0:
            goal_node = node
            
    # 4. Run the A* algorithm
    a_star_algorithm(start_node, goal_node, graph, heuristics)

if __name__ == "__main__":
    main()
```

<h2>Sample Input</h2>

10 14 <br>
A B 6 <br>
A F 3 <br>
B D 2 <br>
B C 3 <br>
C D 1 <br>
C E 5 <br>
D E 8 <br>
E I 5 <br>
E J 5 <br>
F G 1 <br>
G I 3 <br>
I J 3 <br>
F H 7 <br>
I H 2 <br>
A 10 <br>
B 8 <br>
C 5 <br>
D 7 <br>
E 3 <br>
F 6 <br>
G 5 <br>
H 3 <br>
I 1 <br>
J 0 <br>

<h2>Output</h2>

<img width="516" height="461" alt="image" src="https://github.com/user-attachments/assets/12e77e05-054c-4787-b779-81800977ab52" />



<h2>Sample Graph II</h2>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/acbb09cb-ed39-48e5-a59b-2f8d61b978a3)



<h2>Sample Input</h2>

6 6 <br>
A B 2 <br>
B C 1 <br>
A E 3 <br>
B G 9 <br>
E D 6 <br>
D G 1 <br>
A 11 <br>
B 6 <br>
C 99 <br>
E 7 <br>
D 1 <br>
G 0 <br>
<hr>
<h2>Output</h2>

<img width="360" height="357" alt="image" src="https://github.com/user-attachments/assets/b913305d-189c-4216-99dd-12cef0041834" />

## Result : 
Thus, the A* search algorithm for a graph was successfully implemented and the optimal path for the given sample graphs was executed and verified successfully.
