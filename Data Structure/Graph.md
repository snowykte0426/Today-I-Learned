# 그래프(Graph)
비선형 자료구조 중 하나인 그래프(Graph)에 대해 알아보겠다

![Graph Image](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F74539cbb-5cca-4e9f-b61d-32d5fd6c4edd%2Fgraph.drawio_(1).png&blockId=097d7f6e-c43e-4dc4-97ca-0b9b79bb22a3)

### 그래프 관련 용어
+ **무방향 그래프(Undirected Graph)**: 간선에 방향이 표지되지 않은 그래프.즉, 양방향으로 갈 수 있는 길을 가진 그래프를 칭한다
+ **방향 그래프(Directed Graph)**: 간선에 방향성이 존재하는 그래프.즉, 일반통행 도로와 같이 일방향으로만 갈 수 있는 길을 가진 그래프를 칭한다
    <details>
    <summary>방향 그래프의 표기법</summary>
    <ul>
        <li><b><code>&lt;A, B&gt;</code></b> 와 같이 표기하는데 이는 정점 <code>A</code>에서 정점 <code>B</code>로만 갈 수 있는 간선을 의미한다</li>
    </ul>
    </details>

+ **가중치 그래프(Weighted Graph)**: 간선에 비용이나 가중치가 할당된 그래프를 말하는데, <b>네트워크(Network)</b>라고 칭한다
+ **부분 그래프(Subgraph)**: 그래프를 구성하는 정점과 간선의 **부분 집합**으로 이뤄진 그래프를 칭한다
+ **인접 정점(Adjacent Vertex)**: 간선에 의해 **직접 연결된** 정점을 칭한다
+ **차수(Degree)**: **정점에 연결된 간선의 수**를 칭한다.무방향 그래프에서는 **인접한 정점의 수**가 차수가 되지만 방향 그래프에서는 <b>진입 차수(In-Degree)</b>와 <b>진출 차수(Out-Degree)</b>로 나뉜다
+ **경로(Path)**: 간선을 따라서 갈 수 있는 길을 칭한다
+ **단순 경로(Simple Path)**: 경로 중 **반복되는 간선이 없는** 경로를 칭한다
+ **사이클(Cycle)**: 단순 경로의 **시작 정점과 종료 정점**이 같은 경로를 칭한다
+ **연결 그래프(Connected Graph)**: **모든 정점 사이에 경로가 존재**하는 그래프를 칭한다
+ **[트리(Tree)](https://github.com/snowykte0426/Today-I-Learned/blob/main/Data%20Structure/Tree.md)**: **사이클을 가지지 않는 연결 그래프**를 칭한다
+ **완전 그래프(Complete Graph)**: **모든 정점 사이에 간선이 존재**하는 그래프를 칭한다
### 그래프의 표현
+ #### 인접 행렬(Adjacency Matrix)
    ![Adjacency Matrix](https://latex.codecogs.com/png.latex?\text{Adjacency%20Matrix:%20}%20A%20=%20\begin{bmatrix}%20a_{11}%20&%20a_{12}%20&%20\cdots%20&%20a_{1n}%20\\%20a_{21}%20&%20a_{22}%20&%20\cdots%20&%20a_{2n}%20\\%20\vdots%20&%20\vdots%20&%20\ddots%20&%20\vdots%20\\%20a_{n1}%20&%20a_{n2}%20&%20\cdots%20&%20a_{nn}%20\end{bmatrix})
    + 간선의 집합을 2차원 배열로 표현하는 방법이다
    + 행렬의 각 성분이 두 정점의 연결 관계를 나타낸다
    + 행렬의 크기는 ![n x n](https://latex.codecogs.com/png.latex?n%20\times%20n)로 ``n``은 정점의 수를 의미한다
    + 값이 ``1``이라면 간선이 존재하는 것이고 ``0``이라면 간선이 존재하지 않는다
    + **무방향 그래프**에서는 인접 행렬이 **항상 대칭**이다
+ #### 인접 리스트(Adjacency List)
    ![Adjacency List](https://latex.codecogs.com/png.latex?\text{Adjacency%20List:}%20\begin{aligned}%201:%20\{a_{11},%20a_{12},%20\dots,%20a_{1k}\}%20\\%202:%20\{a_{21},%20a_{22},%20\dots,%20a_{2m}\}%20\\%20\vdots%20\\%20n:%20\{a_{n1},%20a_{n2},%20\dots,%20a_{np}\}%20\end{aligned})
    + 각 정점이 **인접한 정점의 리스트를 갖도록 하여** 간선들을 표현할 수도 있다
### 그래프의 탐색
+ #### 깊이 우선 탐색(Depth First Search; DFS)
    + 시작 정점에서부터 임의의 방향으로 갈 수 있는 곳까지 깊이 탐색하다가 **막장**일 때 **가장 마지막에 만났던 갈림길로 되돌아와 다른 방향을 재탐색**하는 탐색법이다
    + DFS는 가장 최근에 만난던 갈림길로 되돌아가기 위하여 **스택**을 이용한다
+ #### 너비 우선 탐색(Breath First Search; BFS)
    + 시작 정점에서부터 **가장 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문**하는 탐색법이다
    +  [트리(Tree)](https://github.com/snowykte0426/Today-I-Learned/blob/main/Data%20Structure/Tree.md)의 **레벨 순회**와 비슷하며 **큐**를 사용한다
### 신장트리(Spanning Tree)
+ 그래프의 **모든 정점을 포함하는 트리**로 **사이클이 존재하지 않으며 ``n``개의 정점을 정확하게 ``n-1``개의 간선으로 연결**한다
+ **DFS**와 **BFS**에서 사용된 간선을 모으면 신장트리가 된다
+ #### 최소비용 신장트리(Minimum Cost Spanning Tree; MST)
    + **최소 신장트리**나 **MST**라고도 부르며 **트리를 구성하는 간선들의 가중치 합이 최소**인 트리를 칭한다
    + **Prim Algorithm**
        + **하나의 정점에서 시작하여 MST를 단계적으로 확장해나가는 방법을 이용**하는 최소 신장트리 구축 알고리즘이다
        + 현재까지의 MST에 가장 인접한 정점 중 **간선의 가중치가 가장 작은 정점을 선택하여 트리를 확장**한다
    + **Kruskal Algorithm**
        + 그래프의 모든 간선을 **오름차순**으로 정렬한 다음 가중치가 가장 적은 간선 ``n-1``개를 순서대로 선택하는 최소 신장트리 알고리즘이다
        + 사이클이 생성되면 안되기에 **사이클을 만들지 않는 가장 가중치가 작은 간선 ``n-1``개를 순서대로 선택**한다
        + 사이클이 생성된다는 것을 알 수 있는 방법은 연결된 정점들이 같은 집합에 포함된다고 할 때 추가할 간선의 두 정점이 **같은 집합**에 속하면 사이클이 발생했다는 것을 알 수 있다
        + 사이클을 감시하는 알고리즘을 **``union-find`` 알고리즘**이라고 한다
    ![Union-Find Algorithm](https://latex.codecogs.com/png.latex?\text{Union-Find%20Algorithm:}%20\text{if%20Find}(u)%20=%20Find(v),%20\text{then%20a%20cycle%20exists;}%20\text{else%20}%20\begin{aligned}%20&\text{rootX}%20=%20Find(u),%20rootY%20=%20Find(v)\\%20&\text{if%20}rootX%20\neq%20rootY:%20parent[rootX]%20=%20rootY.%20\end{aligned})

### 최단 경로(Shortest Path)
+ 가중치 그래프에서 정점 u와 정점 v를 연결하는 경로 중에서 **가중치 합이 최소인 경로**를 칭한다
+ 최단 경로를 구하는 알고리즘에서는 간선의 가중치는 **양수**라고 가정한다
+ #### Dijkstra Algorithm
    + **하나의 정점에서 다른 모든 정점까지의 최단 경로**를 구하는 알고리즘이다
    + 매 단계에서 집합에 포함되지 않은 정점 중 가중치가 최소인 정점을 찾아 거리를 확정하고 집합에 추가한다
    + 새로운 정점이 추가되면 **기존에 집합에 있던 정점들의 값도 영향을 받는다**
    + **탐욕적인(Greedy)** 알고리즘을 분류된다
    ![Dijkstra's Algorithm](https://latex.codecogs.com/png.latex?\text{Dijkstra's%20Algorithm:}%20\text{dist}[v]%20=%20\begin{cases}%200,%20\text{if%20}v%20=%20s\\\infty,%20\text{otherwise}\end{cases},%20u%20=%20\arg\min_{v%20\in%20Q}%20\text{dist}[v],%20\text{dist}[v]%20=%20\min(\text{dist}[v],%20\text{dist}[u]%20+%20w(u,%20v))
+ ### Floyd Algorithm
    + **모든 정점 사이의 최단 경로를 한번에 탐색**하는 알고리즘이다
    + 최단 경로 거리행렬을 이용하는데 이것의 초기값은 그래프의 **인접 행렬**이다
    ![Floyd-Warshall Algorithm](https://latex.codecogs.com/png.latex?\text{Floyd-Warshall%20Algorithm:}%20D[i][j]%20=%20\begin{cases}%200,%20\text{if%20}i%20=%20j\\%20w(i,%20j),%20\text{if%20edge%20}(i,%20j)%20\text{exists}\\%20\infty,%20\text{otherwise}%20\end{cases},%20D[i][j]%20=%20\min(D[i][j],%20D[i][k]%20+%20D[k][j])%20\forall%20k%20\in%20\{1,%202,%20\dots,%20n\})