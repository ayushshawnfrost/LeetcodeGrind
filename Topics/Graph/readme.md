# Graph Representation

<details id="Adjcency Matrix">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">Adjcency Matrix 
</span>
</summary>

```java
// We can use Map or ArrayList<> to make Adjcency List
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        Map<Integer, List<Integer>> adjList = new HashMap<Integer, List<Integer>>();

        // creating the adjcency list(this node is the prerequisite for what alll nodes)
        for (int i = 0; i < prerequisites.length; i++) {
            int post = prerequisites[i][0];
            int pre = prerequisites[i][1];
            List<Integer> lst = adjList.getOrDefault(pre, new ArrayList<Integer>());
            lst.add(post);
            adjList.put(pre, lst);
        }   
    }
}
```
</details>

<details id="Matrix">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">Matrix 
</span>
</summary>

```java
// We can use Map or ArrayList<> to make Adjcency List
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        Map<Integer, List<Integer>> adjList = new HashMap<Integer, List<Integer>>();
        int[] indegree = new int[numCourses];

        // creating the adjcency list(this node is the prerequisite for what alll nodes)
        for (int i = 0; i < prerequisites.length; i++) {
            int post = prerequisites[i][0];
            int pre = prerequisites[i][1];
            List<Integer> lst = adjList.getOrDefault(pre, new ArrayList<Integer>());
            lst.add(post);
            adjList.put(pre, lst);
            // How many cources has to be completed before taking this course
            indegree[post] += 1;
        }   
    }
}
```

</details>


# DFS & BFS


<details id="Depth First Search">
<summary> 
<span style="color:blue;font-size:16px;font-weight:bold">Depth First Search 
</span>
</summary>

https://www.geeksforgeeks.org/problems/depth-first-traversal-for-a-graph/1

```java
// call for DFS
dfs(adj, 0, visited );

// DFS logic
void dfs(List<List<Integer>> adj, int node, boolean visited){

    if(vivited[node])return;
    visited[node]=true;
    // Do something
    for(int i: adj.get(node)){
        if(!visited[i])dfs(adj, i, visited);
    }
}

```

```java

// Implementation
class Solution {
    // Function to return a list containing the DFS traversal of the graph.
    public ArrayList<Integer> dfsOfGraph(int V, ArrayList<ArrayList<Integer>> adj) {
        boolean[] visited=new boolean[V];
        ArrayList<Integer> ans=new ArrayList<>();
        dfs(adj, 0, visited, ans);
        return ans;
    }
    
    public void dfs(ArrayList<ArrayList<Integer>> adj, int node, boolean[]  visited, ArrayList<Integer> ans){
        if(visited[node])return;
        visited[node]=true;
        ans.add(node);
        for(int i:adj.get(node)){
            if(!visited[i])dfs(adj, i, visited, ans);
        }
    }
}
```
</details>


<details id="Breadh First Search">
<summary> 
<span style="color:blue;font-size:16px;font-weight:bold">Breadh First Search 
</span>
</summary>

Most of the time BFS is used to find the shortest path insted of DFS.

    Node enters the Queue --> node is marked "visited" 

```java
// call for BFS
bfs(adj, 0, visited);

// DFS logic
void bfs(List<List<Integer>> adj, int node, boolean visited){
Queue<Integer> queue=new LinkedList<>();
        queue.offer(node);
        visited[node]=true;
        ans.add(node);
        while(!queue.isEmpty()){
            int n=queue.poll();
            for(int i:adj.get(n)){
                if(!visited[i]){
                    queue.add(i);
                    visited[i]=true;
                    ans.add(i);
                }
            }
        }
}
```

```java
// Implementation
class Solution {
    public ArrayList<Integer> bfsOfGraph(int V, ArrayList<ArrayList<Integer>> adj) {
        boolean[] visited=new boolean[V];
        ArrayList<Integer> ans=new ArrayList<>();
        bfs(adj, 0, visited, ans);
        return ans;        
    }
    public void bfs(ArrayList<ArrayList<Integer>> adj, int node, boolean[]  visited, ArrayList<Integer> ans){
        Queue<Integer> queue=new LinkedList<>();
        queue.offer(node);
        visited[node]=true;
        ans.add(node);
        while(!queue.isEmpty()){
            int n=queue.poll();
            for(int i:adj.get(n)){
                if(!visited[i]){
                    queue.add(i);
                    visited[i]=true;
                    ans.add(i);
                }
            }
        }
    }
}
```
</details>


### Detect Cycle :


<details id="Detect cycle in a graph | Undirected Graph | DFS">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Detect cycle in a graph | Undirected Graph | DFS 
</span>
</summary>

https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1

### 1. Using DFS
```java
class Solution {
    public boolean isCycle(int V, ArrayList<ArrayList<Integer>> adj) {
        boolean[] visited=new boolean[V];
        // For Connected Components
        for(int i=0;i<V;i++){
            if(!visited[i] && isCycleUtility(adj,-1,visited,i)){
                return true;
            }
        }
        return false;
    }
    public boolean isCycleUtility(ArrayList<ArrayList<Integer>> adj,  int parent, boolean[] visited, int node){
        visited[node]=true;
        
        for(int i:adj.get(node)){
            if(i == parent)continue;
            if(visited[i])return true;
            if(isCycleUtility(adj,node,visited,i))return true;
        }
        return false;
    }
}
```
</details>


<details id="Detect cycle in a graph | Undirected Graph | BFS">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Detect cycle in a graph | Undirected Graph | BFS 
</span>
</summary>

### 2. Using BFS

```java
class Co{
    int node;
    int parent;
    Co(int n, int p){
        this.node=n;
        this.parent=p;
    }
}
class Solution {
    public boolean isCycle(int V, ArrayList<ArrayList<Integer>> adj) {
        boolean[] visited=new boolean[V];
        
        for(int i=0;i<V;i++){
            if(!visited[i] && isCycleUtility(adj,visited,i)){
                return true;
            }
        }
        return false;
    }
    public boolean isCycleUtility(ArrayList<ArrayList<Integer>> adj, boolean[] visited, int node){
        visited[node]=true;
        Queue<Co> queue=new LinkedList<>();
        queue.add(new Co(node, -1));
        
        while(!queue.isEmpty()){
            Co pair=queue.poll();
            
            for(int i:adj.get(pair.node)){
                if(i==pair.parent)continue;
                if(visited[i])return true;
                if(!visited[i]){
                    visited[i]=true;
                    queue.add(new Co(i,pair.node));
                }
            }
        }
        return false;
    }
}
```
</details>


<details id="Detect cycle in a graph | Undirected | DSU(Disjoint Set Union)">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Detect cycle in a graph | Undirected | DSU(Disjoint Set Union) 
</span>
</summary>

### 3. Using DSU(Disjoint Set Union)
```java
//Function to detect cycle using DSU in an undirected graph.
public int detectCycle(int V, ArrayList<ArrayList<Integer>> adj)
{
    int[] parent=new int[V];
    int[] rank=new int[V];
    for(int i=0;i<V;i++)parent[i]=i;
    
    // Iterate through each edge of the graph.
    for (int u = 0; u < V; u++) {
        for (int v : adj.get(u)) {
            if (u < v) { // To avoid considering the same edge twice.
                if (find(u,parent) == find(v,parent))
                    return 1;
                else
                    union(u, v, parent, rank);
            }
        }
    }
    return 0;
    
}
// Function to find the parent of a node x with path compression.
private int find(int x, int[] parent) {
    if (parent[x] == x)
        return x;
    return parent[x] = find(parent[x], parent);
}

// Function to perform the union of two sets x and y.
private void union(int x, int y, int[] parent, int[] rank) {
    int xRoot = find(x, parent);
    int yRoot = find(y, parent);

    if (xRoot != yRoot) {
        if (rank[xRoot] > rank[yRoot]) {
            parent[yRoot] = xRoot;
        } else if (rank[xRoot] < rank[yRoot]) {
            parent[xRoot] = yRoot;
        } else {
            parent[xRoot] = yRoot;
            rank[yRoot]++;
        }
    }
}
```


</details>

<details id="Detect cycle in a graph | Directed | DFS">
<summary> 
<span style="color:blue;font-size:16px;font-weight:bold">Detect cycle in a graph | Directed | DFS
</span>
</summary>

### 1. Using DFS

```java
class Solution {
    boolean isCycleDFS(List<List<Integer>> adj, int u, List<Integer> visited, List<Integer> inRecursion) {
        visited[u] = true;
        inRecursion[u] = true;
        
        
        for(int v : adj.get(u)) {
            //if not visited, then we check for cycle in DFS
            if(visited[v] == false && isCycleDFS(adj, v, visited, inRecursion))
                return true;
            else if(inRecursion[v] == true)
                return true;
        }
        
        inRecursion[u] = false;
        return false;
        
    }
    
    // Function to detect cycle in a directed graph.
    bool isCyclic(int V, List<Integer> adj) {
        vector<bool> visited(V, false);
        boolean[] inRecursion=new boolean[V];
        
        for(int i = 0; i<V; i++) {
            if(!visited[i] && isCycleDFS(adj, i, visited, inRecursion))
                return true;
        }
        return false;
    }
}

```



</details>


<details id="Detect cycle in a graph | Directed | BFS (Khan's Algorithm)">
<summary> 
<span style="color:blue;font-size:16px;font-weight:bold">Detect cycle in a graph | Directed | BFS (Khan's Algorithm) 
</span>
</summary>

### Using BFS (Khan's Algorithm)


```java

public class DirectedGraph {
    private int V; // Number of vertices
    private LinkedList<Integer>[] adj; // Adjacency list

    public DirectedGraph(int v) {
        V = v;
        adj = new LinkedList[v];
        for (int i = 0; i < v; ++i) {
            adj[i] = new LinkedList<>();
        }
    }

    // Function to add an edge into the graph
    public void addEdge(int v, int w) {
        adj[v].add(w);
    }

    // Function to check if the graph contains a cycle
    public boolean isCyclic() {
        int[] inDegree = new int[V];

        // Calculate in-degrees of all vertices
        for (int i = 0; i < V; i++) {
            for (int neighbor : adj[i]) {
                inDegree[neighbor]++;
            }
        }

        // Create a queue and enqueue all vertices with in-degree 0
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (inDegree[i] == 0) {
                queue.add(i);
            }
        }

        int visitedVertices = 0;

        // Perform BFS
        while (!queue.isEmpty()) {
            int vertex = queue.poll();
            visitedVertices++;

            // Decrease in-degree by 1 for all its neighboring nodes
            for (int neighbor : adj[vertex]) {
                inDegree[neighbor]--;
                // If in-degree becomes 0, add it to the queue
                if (inDegree[neighbor] == 0) {
                    queue.add(neighbor);
                }
            }
        }

        // If visitedVertices count is not equal to the number of vertices,
        // the graph contains a cycle
        return visitedVertices != V;
    }

    public static void main(String[] args) {
        DirectedGraph graph = new DirectedGraph(4);
        graph.addEdge(0, 1);
        graph.addEdge(1, 2);
        graph.addEdge(2, 3);
        graph.addEdge(3, 1); // This edge creates a cycle

        if (graph.isCyclic()) {
            System.out.println("Graph contains a cycle");
        } else {
            System.out.println("Graph does not contain a cycle");
        }
    }
}

```

</details>


### Topological Sort (DAG)

<details id="Topological Sort Definition">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">Topological Sort in DAG 
</span>
</summary>

    Topological sort is only possible for a Directed, Acyclic graph. Just use general code for cycle detection using BFS/DFS add a small logic for stack to store the order. Code is extended version of the cycle detection. Because if there is a cycle in the graph then it is not acyclic hence no topological order is present. 

### Topological Sort (DFS)
</details>

<details id="207. Course Schedule">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">207. Course Schedule 
</span>
</summary>

https://leetcode.com/problems/course-schedule/

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return true if you can finish all courses. Otherwise, return false.

 

Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
Example 2:

Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
 

Constraints:

1 <= numCourses <= 2000
0 <= prerequisites.length <= 5000
prerequisites[i].length == 2
0 <= ai, bi < numCourses
All the pairs prerequisites[i] are unique.

```java
// Used DFS to check if the cycle is present--> topological order cant be obtained or YES 
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adj=new ArrayList<>();
        boolean[] visited=new boolean[numCourses];
        boolean[] inRec=new boolean[numCourses];
        
        // Initializing adjcency list
        for(int i=0;i<numCourses;i++){
            adj.add(new ArrayList<>());
        }
        
        // Making adjcency list
        for(int[] pre:prerequisites){
            adj.get(pre[1]).add(pre[0]);
        } 

        // For Connected Components
        for(int i=0;i<numCourses;i++){
            if(!visited[i]  && checkCycleDFS(adj, visited, inRec, i)){
                return false;
            }
        }
        return true;
    }

    public boolean checkCycleDFS(List<List<Integer>> adj, boolean[] visited, boolean[] inRec, int node){
        // If visited --> check if it is in current Recursion 
        if(visited[node] && inRec[node])return true;
        if(visited[node])return false;
        
        visited[node]=true;
        inRec[node]=true;

        for(int neighbour: adj.get(node)){
            if(checkCycleDFS(adj, visited, inRec, neighbour)){
                return true;
            }
        }
        inRec[node]=false;
        return false;
    }
}

```
</details>

<details id="210. Course Schedule II">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">210. Course Schedule II 
</span>
</summary>

https://leetcode.com/problems/course-schedule-ii/description/

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

 

Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
Example 2:

Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
Example 3:

Input: numCourses = 1, prerequisites = []
Output: [0]
 

Constraints:

1 <= numCourses <= 2000
0 <= prerequisites.length <= numCourses * (numCourses - 1)
prerequisites[i].length == 2
0 <= ai, bi < numCourses
ai != bi
All the pairs [ai, bi] are distinct.


```java
// Very similar to CourceScheduel, here additionally we need to print the sequence

class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adj=new ArrayList<>();
        boolean[] visited=new boolean[numCourses];
        boolean[] inRec=new boolean[numCourses];
        Stack<Integer> topoOrder=new Stack<>();

        // Initializing adjcency list
        for(int i=0;i<numCourses;i++){
            adj.add(new ArrayList<>());
        }
        
        // Making adjcency list
        for(int[] pre:prerequisites){
            adj.get(pre[1]).add(pre[0]);
        } 

        // For Connected Components
        for(int i=0;i<numCourses;i++){
            if(!visited[i]  && checkCycleDFS(adj, visited, inRec, topoOrder, i)){
                return new int[0];
            }
        }

        // Convert the stack to an array
        int[] result = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            result[i] = topoOrder.pop();
        }

        return result;
    }

    private boolean checkCycleDFS(List<List<Integer>> graph, boolean[] visited, boolean[] onPath, Stack<Integer> stack, int course) {
        if (visited[course] && onPath[course]) {
            return true; // Cycle detected
        }
        if (visited[course]) {
            return false; // Already visited node
        }

        // Mark the node as visited and part of the current path
        visited[course] = true;
        onPath[course] = true;

        // Visit all the neighbors
        for (int neighbor : graph.get(course)) {
            if (checkCycleDFS(graph, visited, onPath, stack, neighbor)) {
                return true;
            }
        }
        onPath[course] = false;
        // Add the course to the stack
        stack.push(course);

        return false;
    }
}

```


    MIK method

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adj=new ArrayList<>();
        boolean[] visited=new boolean[numCourses];
        boolean[] inRec=new boolean[numCourses];
        
        // Initializing adjcency list
        for(int i=0;i<numCourses;i++){
            adj.add(new ArrayList<>());
        }
        
        // Making adjcency list
        for(int[] pre:prerequisites){
            adj.get(pre[1]).add(pre[0]);
        } 

        // For Connected Components
        for(int i=0;i<numCourses;i++){
            if(!visited[i]  && dfs(adj, visited, inRec, i)){
                return false;
            }
        }
        return true;
    }

    public boolean dfs(List<List<Integer>> adj, boolean[] visited, boolean[] inRec, int node){
        // Detect a cycle, hence cant find a topological order
        visited[node]=true;
        inRec[node]=true;

        for(int neighbour: adj.get(node)){
            if(visited[neighbour]==false && dfs(adj, visited, inRec, neighbour)){
                return true;
            }
            if(inRec[neighbour]==true) return true;
        }
        inRec[node]=false;
        return false;
    }
}
```
</details>


### Bipartite
<details id="Bipartite Graph | Definition">
<summary> 
<span style="color:purple;font-size:16px;font-weight:bold">Bipartite Graph | Definition 
</span>
</summary>

    Can you color a graph in 2 colors, such that no 2 adjcent nodes have same color
</details>

<details id="Bipartite Graph | DFS">
<summary> 
<span style="color:purple;font-size:16px;font-weight:bold">Bipartite Graph | DFS
</span>
</summary>
Given an adjacency list of a graph adj of V no. of vertices having 0 based index. Check whether the graph is bipartite or not.


```java


class Solution
{
    public boolean isBipartite(int V, ArrayList<ArrayList<Integer>>adj)
    {
        int[] colors=new int[V];
        Arrays.fill(colors,-1);
        
        for(int i=0;i<V;i++){
            if(colors[i]==-1 && !dfs(adj,0,colors,i)){
                return false;
            }
        }
        return true;
    }
    public boolean dfs(ArrayList<ArrayList<Integer>>adj, int color, int[] colors, int node){
        colors[node]=color;
        
        for(int i:adj.get(node)){
            if(colors[i]==color)return false;
            
            if(colors[i]==-1 && !dfs(adj,1-color,colors,i)){
                return false;
            }
        }
        return true;
    }
}
```
</details>




<details id="Bipartite Graph | BFS">
<summary> 
<span style="color:purple;font-size:16px;font-weight:bold">Bipartite Graph | BFS 
</span>
</summary>

```java
class Solution
{
    public boolean isBipartite(int V, ArrayList<ArrayList<Integer>>adj)
    {
        int[] colors=new int[V];
        Arrays.fill(colors,-1);
        
        for(int i=0;i<V;i++){
            if(colors[i]==-1 && !dfs(adj,0,colors,i)){
                return false;
            }
        }
        return true;
    }
    public boolean dfs(ArrayList<ArrayList<Integer>>adj, int color, int[] colors, int node){
        Queue<Integer> queue=new LinkedList<>();
        queue.add(node);
        colors[node]=color;
        
        while(!queue.isEmpty()){
            int n=queue.poll();
            
            for(int i:adj.get(n)){
                if(colors[i]==colors[n])return false;
                
                if(colors[i]==-1){
                    colors[i]=1-colors[n];
                    queue.offer(i);
                }
            }
            
        }
        return true;
    }
}
```
</details>

### Disjoint Set Union | Union-Find

<details id="Disjoint Set Union | Definition">
<summary> 
<span style="color:orange;font-size:16px;font-weight:bold">Disjoint Set Union | Definition 
</span>
</summary>

    Sets whose intersection is NULL are called Disjoint SETs
    S1^S2^S3 = NULL


```java
// Find (Tell if 2 members (a,b) belongs to the same set or not)
int find(int i, List<Integer> parent){

    if(i == parent[i])return i;

    return parent[i]=find(parent[i], parent);
}
```
```java
// Union (Combine 2 given sets)
void union(int x, int y, List<Integer> parent, List<Integer> rank){
    int x_parent = find(x, parent);
    int y_parent = find(y, parent);

    if(x_parent == y_parent){
        return;
    }else if(rank[x_parent]> rank[y_parent]){
        parent[y_parent]=x_parent;
    }else if(rank[x_parent]< rank[y_parent]){
        parent[x_parent]=y_parent;
    }else{
        parent[x_parent]=y_parent;
        rank[y_parent]++;
    }
}
```
</details>

<details id="1319. Number of Operations to Make Network Connected">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1319. Number of Operations to Make Network Connected 
</span>
</summary>

There are n computers numbered from 0 to n - 1 connected by ethernet cables connections forming a network where connections[i] = [ai, bi] represents a connection between computers ai and bi. Any computer can reach any other computer directly or indirectly through the network.

You are given an initial computer network connections. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected.

Return the minimum number of times you need to do this in order to make all the computers connected. If it is not possible, return -1.

 

Example 1:

![alt text](image.png)

Input: n = 4, connections = [[0,1],[0,2],[1,2]]
Output: 1
Explanation: Remove cable between computer 1 and 2 and place between computers 1 and 3.

Example 2:

![alt text](image-1.png)

Input: n = 6, connections = [[0,1],[0,2],[0,3],[1,2],[1,3]]
Output: 2
Example 3:

Input: n = 6, connections = [[0,1],[0,2],[0,3],[1,2]]
Output: -1
Explanation: There are not enough cables.
 

Constraints:

1 <= n <= 105
1 <= connections.length <= min(n * (n - 1) / 2, 105)
connections[i].length == 2
0 <= ai, bi < n
ai != bi
There are no repeated connections.
No two computers are connected by more than one cable.

```java
// Using simple DFS to count the connected components and return ans-1
class Solution {
    public int makeConnected(int n, int[][] connections) {
        if(connections.length<n-1)return -1;
        boolean[] visited=new boolean[n];
        List<List<Integer>> adj=new ArrayList<>();
        int ans=0;
        for(int i=0;i<n;i++){
            adj.add(new ArrayList<Integer>());
        }
        // Undirected graph
        for(int[] i:connections){
            adj.get(i[0]).add(i[1]);
            adj.get(i[1]).add(i[0]);
        }

        for(int i=0;i<n;i++){
            if(!visited[i]){
                ans++;
                dfs(adj, visited, i);
            }
        }
        return ans-1;
    }
    public void dfs(List<List<Integer>> adj, boolean[] visited, int node){
        visited[node]=true;

        for(int i:adj.get(node)){
            if(!visited[i]){
                dfs(adj, visited, i);
            }
        }
    }
}
```


```java
// Using DSU
class Solution {
    public int makeConnected(int n, int[][] connections) {
        if(connections.length<n-1)return -1;
        int[] parent=new int[n];
        int[] rank=new int[n];
        for(int i=0;i<n;i++){
            parent[i]=i;
        }
        int components=n;
        for(int[] i:connections){
            if(find(i[0], parent) != find(i[1],parent)){
                union(i[0],i[1],parent, rank);
                components--;
            }
        }
        return components - 1;
    }

    public int find(int node, int[] parent ){
        if(parent[node]==node){
            return node;
        }
        return parent[node]= find(parent[node],parent);
    }

    void union(int x, int y, int[] parent, int[] rank){
        int x_parent = find(x, parent);
        int y_parent = find(y, parent);

        if(x_parent == y_parent){
            return;
        }else if(rank[x_parent]> rank[y_parent]){
            parent[y_parent]=x_parent;
        }else if(rank[x_parent]< rank[y_parent]){
            parent[x_parent]=y_parent;
        }else{
            parent[x_parent]=y_parent;
            rank[y_parent]++;
        }
    }
}

```
</details>

<details id="2316. Count Unreachable Pairs of Nodes in an Undirected Graph">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2316. Count Unreachable Pairs of Nodes in an Undirected Graph 
</span>
</summary>

https://leetcode.com/problems/count-unreachable-pairs-of-nodes-in-an-undirected-graph/description/

You are given an integer n. There is an undirected graph with n nodes, numbered from 0 to n - 1. You are given a 2D integer array edges where edges[i] = [ai, bi] denotes that there exists an undirected edge connecting nodes ai and bi.

Return the number of pairs of different nodes that are unreachable from each other.

 

Example 1:

![alt text](image-2.png)

Input: n = 3, edges = [[0,1],[0,2],[1,2]]
Output: 0
Explanation: There are no pairs of nodes that are unreachable from each other. Therefore, we return 0.

Example 2:

![alt text](image-3.png)

Input: n = 7, edges = [[0,2],[0,5],[2,4],[1,6],[5,4]]
Output: 14
Explanation: There are 14 pairs of nodes that are unreachable from each other:
[[0,1],[0,3],[0,6],[1,2],[1,3],[1,4],[1,5],[2,3],[2,6],[3,4],[3,5],[3,6],[4,6],[5,6]].
Therefore, we return 14.
 

Constraints:

1 <= n <= 105
0 <= edges.length <= 2 * 105
edges[i].length == 2
0 <= ai, bi < n
ai != bi
There are no repeated edges.
```java
// Using DSU (Can also be done using DFS/BFS)
class Solution {
    public long countPairs(int n, int[][] edges) {
        int[] parent = new int[n];
        int[] rank = new int[n];
        
        // Initialize the parent array where each node is its own parent
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
        
        // Apply union for each edge
        for (int[] edge : edges) {
            union(edge[0], edge[1], parent, rank);
        }
        
        // Count the size of each component
        Map<Integer, Integer> componentSizeMap = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int root = find(i, parent);
            componentSizeMap.put(root, componentSizeMap.getOrDefault(root, 0) + 1);
        }
        
        // Calculate the number of valid pairs
        long result = 0;
        long remainingNodes = n;
        
        for (int size : componentSizeMap.values()) {
            result += size * (remainingNodes - size);
            remainingNodes -= size;
        }
        
        return result;
    }
    
    // Union-Find with union by rank
    private void union(int x, int y, int[] parent, int[] rank) {
        int rootX = find(x, parent);
        int rootY = find(y, parent);
        
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootX] = rootY;
                rank[rootY]++;
            }
        }
    }
    
    // Find with path compression
    private int find(int node, int[] parent) {
        if (node != parent[node]) {
            parent[node] = find(parent[node], parent);
        }
        return parent[node];
    }
}

```
</details>

### Single Source Shortest path

<details id="Dijkstra Algorithm">
<summary> 
<span style="color:blue;font-size:16px;font-weight:bold">Dijkstra Algorithm 
</span>
</summary>

https://www.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1?utm_source=youtube&utm_medium=collab_striver_ytdescription&utm_campaign=implementing-dijkstra-set-1-adjacency-matrix

Single source shortest Path algorithm

Time Complexity: O(E * logV), Where E is the number of edges and V is the number of vertices.
The while loop runs at most V times because each vertex is processed only once.
For each vertex, we perform relaxation on its neighbors. In the worst case, each edge is considered once.
Therefore, the overall time complexity is O(E*logV), where V is the number of vertices, and the logV factor comes from the priority queue operations.
Auxiliary Space: O(V),
The dist array of size V is used to store the minimum distances from the source vertex to all other vertices. So, the space complexity for dist is O(V)
The pq priority queue is used to keep track of vertices to visit next. In the worst case, it can contain all V vertices. So, the space complexity for the priority queue is O(V).
Overall, the space complexity of the algorithm is O(V).

```java
import java.util.*;

class DriverClass {
    class NodeDistPair {
        int dist;
        int node;
        
        NodeDistPair(int dist, int node) {
            this.dist = dist;
            this.node = node;
        }
    }
    
    class Solution {
        // Function to find the shortest distance of all the vertices from the source vertex S.
        static int[] dijkstra(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj, int S) {
            // Min Heap
            PriorityQueue<NodeDistPair> pq = new PriorityQueue<>((a, b) -> a.dist - b.dist);
            
            int[] dist = new int[V];
            Arrays.fill(dist, Integer.MAX_VALUE);
            dist[S] = 0;
            pq.offer(new NodeDistPair(0, S));
            
            while (!pq.isEmpty()) {
                NodeDistPair current = pq.poll(); // Fetch and remove the min element
                int dis = current.dist;
                int node = current.node;
                
                for (int i = 0; i < adj.get(node).size(); i++) {
                    int adjNode = adj.get(node).get(i).get(0);
                    int edgeWeight = adj.get(node).get(i).get(1);
                    
                    if (dis + edgeWeight < dist[adjNode]) {
                        dist[adjNode] = dis + edgeWeight;
                        pq.offer(new NodeDistPair(dist[adjNode], adjNode));
                    }
                }
            }
            
            return dist;
        }
    }
}

```
</details>


<details id="743. Network Delay Time">
<summary> 
<span style="color:blue;font-size:16px;font-weight:bold">743. Network Delay Time 
</span>
</summary>

https://leetcode.com/problems/network-delay-time/description/

## Learn this implementation

### Maintain a minimum distance array and update it on the go. Use min heap to put (weight, destination node). 
https://www.youtube.com/watch?v=hptQEIpvaxM&list=PLpIkg8OmuX-LZB9jYzbbZchk277H5CbdY&index=28


You are given a network of n nodes, labeled from 1 to n. You are also given times, a list of travel times as directed edges times[i] = (ui, vi, wi), where ui is the source node, vi is the target node, and wi is the time it takes for a signal to travel from source to target.

We will send a signal from a given node k. Return the minimum time it takes for all the n nodes to receive the signal. If it is impossible for all the n nodes to receive the signal, return -1.

 

Example 1:

![alt text](image-72.png)

Input: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
Output: 2
Example 2:

Input: times = [[1,2,1]], n = 2, k = 1
Output: 1
Example 3:

Input: times = [[1,2,1]], n = 2, k = 2
Output: -1
 

Constraints:

1 <= k <= n <= 100
1 <= times.length <= 6000
times[i].length == 3
1 <= ui, vi <= n
ui != vi
0 <= wi <= 100
All the pairs (ui, vi) are unique. (i.e., no multiple edges.)

### Time Complexity
![alt text](image-73.png)

```java
import java.util.*;

class NodeDist {
    int weight;
    int node;

    NodeDist(int weight, int node) {
        this.weight = weight;
        this.node = node;
    }
}

class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        int[] minTimes = new int[n + 1];
        Arrays.fill(minTimes, Integer.MAX_VALUE);
        List<List<NodeDist>> adj = new ArrayList<>();
        for (int i = 0; i < n + 1; i++) {
            adj.add(new ArrayList<>());
        }
        
        for (int[] time : times) {
            adj.get(time[0]).add(new NodeDist(time[2], time[1]));
        }
        
        PriorityQueue<NodeDist> queue = new PriorityQueue<>((a, b) -> a.weight - b.weight);
        queue.add(new NodeDist(0, k));
        minTimes[k] = 0;
        
        while (!queue.isEmpty()) {
            NodeDist current = queue.poll();
            int node = current.node;
            int weight = current.weight;
            
            if (weight > minTimes[node]) {
                continue;
            }
            
            for (NodeDist neighbor : adj.get(node)) {
                int newWeight = weight + neighbor.weight;
                if (newWeight < minTimes[neighbor.node]) {
                    minTimes[neighbor.node] = newWeight;
                    queue.add(new NodeDist(newWeight, neighbor.node));
                }
            }
        }
        
        int maxTime = Integer.MIN_VALUE;
        for (int i = 1; i <= n; i++) {
            if (minTimes[i] == Integer.MAX_VALUE) {
                return -1; // If any node is unreachable, return -1.
            }
            maxTime = Math.max(maxTime, minTimes[i]);
        }
        
        return maxTime;
    }
}

```


</details>

<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->

<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->

<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->

<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->

