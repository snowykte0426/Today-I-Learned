# 트리(Tree)

비선형 자료구조 중 하나인 트리(Tree)에 대해 알아보겠다

![Tree Image](https://t1.daumcdn.net/cfile/tistory/251FE74B5100D04F2F)

### 트리 관련 용어
+ **노드(Node)**: 각각의 요소를 노드라 칭함
+ **간선(Edge)**: 노드 사이의 연관 관계를 칭함
+ **루트(Root)**: 트리의 최상위 노드를 칭함
+ **서브트리(Subtree)**: 루트를 제외한 나머지 노드를 칭함
+ **부모(Parent)**,**자식(Child)**,**형제(Sibling)**,**조상(Ancestor)**,**자손(Descendent)**: 인간의 관계와 같이 노드 간의 관계를 정의한것
+ **단말노드(Terminal, Leaf)**: 자식이 없는 노드를 단말노드라 칭한다
+ **차수(Degree)**: 노드가 가지고 있는 자식의 수를 칭한다
+ **레벨(Level)**: 트리의 각 계층에 번호를 매긴 것을 칭한다
+ **높이(Height)**: 트리가 가지고 있는 최대 레벨을 칭한다
+ ### 이진 트리(Binary Tree)
+ 자식의 수를 **최대 2개로 제한**한  트리
+ #### 포화 이진 트리(Full Binary Tree)
    + 모든 레벨에 노드가 **꽉 차 있는** 이진 트리를 칭한다.
    + 다음과 같은 간단한 식으로 포화 이진 트리의 노드 갯수를 구할 수 있다
![LaTeX Equation](https://latex.codecogs.com/svg.latex?N%20%3D%20%5Csum_%7Bi%3D0%7D%5E%7Bh%7D%202%5Ei%20%3D%202%5E0%20%2B%202%5E1%20%2B%202%5E2%20%2B%20%5Cdots%20%2B%202%5Eh%20%3D%20%5Cfrac%7B2%5E%7Bh%2B1%7D%20-%201%7D%7B2%20-%201%7D%20%3D%202%5E%7Bh%2B1%7D%20-%201)
+ #### 완전 이진 트리(Complete Binary Tree)
    + 높이가 ``k``인 트리에서 레벨 1부터 레벨 ``k-1``까지는 노드가 모두 채워져 있고 마지막 레벨 ``k``는 **왼쪽**부터 노드가 채워져 있는 트리를 칭한다
    + 마지막 레벨 ``k``는 노드가 꽉 차 있지 않아도 되지만 중간에 **빈 곳**이 있으면 안된다
    + <b>힙(Heap)</b>은 **완전 이진 트리**의 대표적인 예이다
    + **포화 이진 트리** 역시 완전 이진 트리의 **하나**로 볼 수 있다

<div align="center">

  ![LaTeX Equation 2](https://latex.codecogs.com/svg.latex?T%20%3D%20%5Cbigcup_%7Bi%3D0%7D%5E%7Bh%7D%20L_i)

</div>

+ #### 균형 이진 트리(Balanced Binary Tree)
    + 모든 노드에서 **좌우 서브 트리**의 **높이 차**가 **1 이하**인 트리를 칭한다

<br>
    <details>
        <summary>경사 트리</summary>
        <ul>
           <li>좌우 서브 트리의 높이 차가 <b>2 이상</b>인 트리를 칭한다</li>
        </ul>
    </details>

+ ``n``개의 노드를 가진 이진 트리는 반드시 **``n-1``개의 간선**을 갖는다
+ 높이가 <b>``h``</b>인 이진 트리는 최소 <b>``h``</b>개의 노드를 가져야 한다
+ 높이가 <b>``n``</b>인 이진 트리는 최대 <b>``2ⁿ-1``</b>개의 노드를 가지는데 이는 **포화 이진 트리**이기 때문이다
+ <b>``n``</b>개의 노드를 가지는 이진 트리의 최대 높이는 <b>``n``</b>이고 최소 높이는 ![LaTeX Equation](https://latex.codecogs.com/svg.latex?%5Clog_2%28n%2B1%29) 이다
### 이진 트리의 순회(Traversal)
+ 트리의 모든 노드를 한 번씩 방문 하는것을 칭한다
+ 선형 자료구조의 순회는 **매우 간단하지만** 비선형 자료구조의 순회는 여러 <b>'순서'</b>로 순회될 수 있다
+ #### 표준 순회
    + **전위 순회(Preorder Traversal) - VLR**
        + **루트(V)**->**왼쪽 서브 트리(L)**->**오른쪽 서브 트리(R)**
    + **중위 순회(Inorder Traversal) - LVR**
        + **왼쪽 서브 트리(L)**->**루트(V)**->**오른쪽 서브 트리(R)**
    + **후위 순회(Postorder Traversal) - LRV**
        + **왼쪽 서브 트리(L)**->**오른쪽 서브 트리(R)**->**루트(V)**
+ #### 레벨 순회
    + 레벨 순으로 노드를 방문하는 순회법이다
    + 루트의 레벨이 1이고 아래로 갈수록 **왼쪽에서 오른쪽으로 방문**한다
 ### 이진 탐색 트리(BST; Binary Search Tree)
+ **효율적인 탐색**을 위한 이진 트리 기반의 자료구조를 칭한다
+ 이진 탐색 트리는 아래와 같이 **순환적으로 정의**된다
    + 모든 노드는 **유일한 키**를 갖는다
    + 왼쪽 서브 트리 키는 루트의 키보다 **작다**
    + 오른쪽 서브 트리의 키는 루트의 키보다 **크다**
    + 왼쪽과 오른쪽 서브 트리도 **이진 탐색 트리**이다
+ 이진 탐색 트리는 일반 이진 트리와는 달리 **위치를 지정하지 않더라도** 노드를 **삽입(Insert)**, **삭제(Delete)**,**탐색(Search)** 연산을 할 수 있다
+ #### 탐색(Search) 연산
    + 탐색 연산은 다음과 같은 과정을 반복한다
        + ``key``가 루트의 키와 같으면 **루트가 찾는 노드**이다
        + ``key``가 루트의 키보다 작으면 찾는 노드는 **왼쪽 서브 트리**에 있으며 탐색을 왼쪽 자식 노드를 기준으로 다시 시작한다
        + ``key``가 루트의 키보다 크면 찾는 노드는 **오른쪽 서브 트리**에 있으며 탐색을 루트의 오른쪽 자식 노드를 기준으로 다시 시작한다
        + 탐색 과정에서 **NULL**를 만난다면 탐색은 실패한다
+ #### 삽입(Insert) 연산
    + 먼저 삽입할 노드의 <b>``key``</b>를 이용한 탐색 과정이 수행되어야 하며 탐색이 **실패**한 위치가 노드를 삽입할 위치이다
    + 만약 탐색에 성공한다면 이미 해당 트리에 같은 ``key``값을 가진 노드가 있다는 것을 말하며 이 상황에선 <b>``key``의 중복을 허용하지 않으므로 삽입하지 않게</b>된다
+ #### 삭제(Delete) 연산
    + 삭제 연산의 경우에는 단말 노드인 경우와 비단말 노드인 경우로 나뉘어 진다
    + **단말 노드의 경우**
        + 단말 노드의 경우 삭제할 노드의 **부모 노드의 링크 필드(Link Field)의 값을 NULL**로 변경하고 메모리를 할당 해제하면 연산이 완료된다
    + **비단말 노드의 경우**
        + 비단말 노드의 경우 다시 2가지 경우의 수로 나뉘는데 자식이 하나 뿐이라면 **자신을 삭제하고** 유일한 자식을 **부모 노드에 연결**하면 되며 자식이 2개라면 자신과 값이 가장 비슷한 일명 **후계자 노드**를 찾아 그 노드와 연결해준다
### 힙 트리(Heap Tree)
+ #### 최대 힙(Max Heap)
    + 부모의 ``key``값이 자식의 ``key``값보다 크거나 같은 **완전 이진 트리**를 칭한다
+ #### 최소 힙(Min Heap)
    + 부모의 ``key``값이 자식의 ``key``값보다 작거나 같은 **완전 이진 트리**를 칭한다
+ #### 삽입 연산
    + 힙에 새로운 노드가 삽입되면 일단 **마지막 노드의 다음 위치**에 삽입한다
    + 힙의 조건을 충족 시키기 위하여 **새로운 노드를 부모와 교환**해주는 과정이 필요할수도 있다
+ #### 삭제 연산
    + 힙에서의 삭제 연산은 **루트 노드를 삭제하는 것**을 말한다
    + 힙에서 삭제가 일어나면 가장 말단 노드를 루트로 올리고 **순차적으로 강등**한다
### AVL 트리
+ #### 균형 인수(Balance Factor; BF)
    + **왼쪽 서브트리의 높이**와 **오른쪽 서브트리의 높이**의 차를 칭한다
<div align="center">

![Balance Factor Formula](https://latex.codecogs.com/png.latex?\text{Balance%20Factor}%20=%20h_{\text{left}}%20-%20h_{\text{right}})

</div>

+ #### 정의
    + **A**delson-**V**elskii와 **L**andis에 의해 제안된 트리로 항 상 **균형 트리**를 **보장**한다
    + **모든 노드의 균형 인수가 1 이하**인 트리를 AVL 트리라 부를 수 있다
+ #### 삽입 연산
    + 노드가 삽입되면 루트까지의 **모든 조상 노드의 균형 인수**가 영향을 받는다
    + 균형 인수를 바로잡기 위해서 **가장 가까운 조상 노드의 균형**부터 고쳐야 하는데 이를 위해서 **회전**이라는 아이디어를 적용한다
+ #### LL 회전
![LL Turn](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbiiR2r%2FbtrJUY5baM4%2FyUDa0wduJaJ4pmL0glLMl1%2Fimg.png)
+ #### RR 회전
![RR Turn](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FMgydF%2Fbtq2ZpcT9dF%2FWNzhK8Ka9KmiuX6iqj5Ws0%2Fimg.png)
+ #### RL 회전
![RL Turn](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbrTQV1%2Fbtq2TcMbXA3%2FmhrY8bPspDrRT90kkGDIR1%2Fimg.png)
+ #### LR 회전
![LR Turn](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbqqYJK%2FbtrTfOfWWBd%2FKVynhLISaSx98MoUoNORY1%2Fimg.png)