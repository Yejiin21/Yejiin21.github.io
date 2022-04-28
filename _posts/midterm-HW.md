# 네트워크 플로우 (Network Flow)

## 유량 네트워크의 속성
    [용어]   
    
    * 용량 (Capacity)
        c(u, v) : 정점 u 에서 v 로 전송할 수 있는 최대 용량

    * 유량 (Flow)
        f(u, v) : 정점 u 에서 v 로 실제 흐르고 있는 유량

    * 소스 (Source)
        s : 모든 유량이 시작되는 정점

    * 싱크 (Sink)
        t : 모든 유량이 도착하는 정점
    
    * 잔여 용량 (Residual Capacity)
        r(u, v) = c(u, v) – f(u, v)
        간선의 용량과 살제로 흐르는 유량의 차이

    * 증가 경로(Augmenting Path)
        소스에서 싱크로 유량을 보낼 수 있는 경로


    [네트워크 유량은 항상 3가지 조건을 만족해야 한다.]

    1. 용량 제한 속성(capacity constraint) : f(u, v) <= c(u, v) -> 유량은 용량을 초과할 수 없다.

    2. 유량의 대칭성(skew symmetry) : f(u, v) = -f(v, u) -> u에서 v로 유량이 흐르면 v에서 u로 음수의 유량이 흐르는 것과 동일

    3. 유량의 보존(flow conservation) : 각 정점에 들어오는 유량과 나가는 유량은 같음

# Ford-Fulkerson Algorithm

소스(source) 노드에서 시작해서 싱크(sink) 노드로 도착하는 유량   
- 유량 네트워크의 모든 간선의 유량을 0으로 초기화   
- 소스에서 싱크로 유량을 더 보낼 수 있는 경로를 찾아 유량 보내기를 반복 **(이때 해당 경로는 반드시 여유 용량이 남아있어야 한다.)** 

증강 경로로 보낼 수 있는 최대 유량은 포함된 **가선의 잔여 용량 중에서 가장 작은 값**

증강 경로 찾기 ? `DFS` or `BFS`   
[Ford-Fulkerson에서는 BFS를 이용하여 증강경로를 찾는다.]

### **경로를 찾았다는 조건**
* source -> sink 도달
* 경로상에 반드시 여유용량이 있어야함

<그림으로 이해하기>

![1](https://user-images.githubusercontent.com/101397075/165730315-0f8eef53-5a75-4524-8bcf-d6b7db36a7cc.PNG)

경로에서 보낼 수 있는 최애 flow는 각각 구간에 남은 capacity의 최솟값(min)   
경로(A->B->E->H)에서 각각의 남은 capacity(c(a,b) - f(a,b))를 구하자면...

    A->B = 4 - 0 = 4
    B->E = 2 - 0 = 2
    E->H = 5 - 0 = 5
    위 3가지 경우에게 min은 2이다.

![2](https://user-images.githubusercontent.com/101397075/165731993-62f90ec5-c445-4b7f-ab1a-1bf478790918.PNG)
경로(A->C->F->H)에서 각각의 남은 capacity의 min은 1이다.

![3](https://user-images.githubusercontent.com/101397075/165733440-2cfc8de0-ad15-437c-9680-d3531f9d1d84.PNG)
경로(A->D->F->H)에서 각각의 남은 capacity의 min은 3이다.

![4](https://user-images.githubusercontent.com/101397075/165734672-659ee5d9-3473-46c5-8dd7-0277ba21cb76.PNG)
경로(A->D->G->H)에서 각각의 남은 capacity의 min은 2이다.

현재 T까지 도달한 Total 유량은 8 이다.

**종료조건은 증가경로를 더이상 찾지 못하는 경우가 된다.**

--------------------

### **?? 정말로 Total 유량이 8일까 ??**

결과부터 말하자면..

![역간선3](https://user-images.githubusercontent.com/101397075/165789701-cad57537-8a45-4e53-84ed-cb9beb4e24a4.PNG)

* A -> B -> E -> H 로 min 2
* A -> C -> F -> H 로 min 1
* A -> D -> F -> H 로 min 3
* A -> D -> F -> E -> H 로 min 3
* A -> D -> G-> H 로 min 2   
-> T까지 도달한 Total 유량은 10이다.

**Q : 내가 직접 구한 답과 해설의 답이 다른 걸까 ?**   
A : 가상의 간선 즉, 역간선이 있다고 가정하고 유량의 흐름을 조정해야한다.   
포드-풀커슨 알고리즘의 핵심은 **역간선**의 개념을 이용하는 것이다.

**Q : 역간선이란 무엇인가?**   
A : 간선(u,v)의 역간선(v,u)이 존재할 수 있다. 이는 역방향으로 음의 유량이 흐르고 있는 것으로 본다. (간선으로 이미 흐른 유량을 역방향으로 흐르는 양만큼 취소시키는 동작)

![역간선1](https://user-images.githubusercontent.com/101397075/165789690-29b41db0-0559-4cfc-bbdd-8636aab348f0.PNG)
위에서 문제풀이의 종료조건에 따라 더이상 증가경로를 찾지 못하고 프로그램은 종료되었다.   

![역간선2](https://user-images.githubusercontent.com/101397075/165789699-54aae381-bdf8-4524-9711-fbce05898d19.PNG)  
하지만 역간선을 통해 초록색 줄의 증가경로를 또 찾아볼 수 있다.

    증가경로 : A -> D -> F -> E -> H

증가경로 capacity를 구한다음 min의 값을 구해본다.   
* A -> D = 7 - 5 = 2
* D -> F = 0 - (-3) = 3
* F -> E = 3 - 0 = 3   
* E -> H = 5 - 2 = 3   
capacity의 min 값은 2임을 알 수 있다.

![역간선3](https://user-images.githubusercontent.com/101397075/165789701-cad57537-8a45-4e53-84ed-cb9beb4e24a4.PNG)

최종적인 증가경로와 흘릴 수 있는 최대 flow값을 구했으니 해당 경로에 flow을 흘려 보낸다.   

### 경로에 flow을 흘릴 때, 정방향은 flow을 + 하고, 역방향은 -한다.
* f(a, b) += flow
* f(b, a) -= flow

----------------

# Ford-Fulkerson 시간복잡도

최대의 유량이 f이라면 각 증가 경로 당 유량은 최소 1(f > 0)이기에 증가 경로의 최소 개수는 f개이다.   
하나의 증가 경로를 찾는데 방문하는 정점과 간선은 최대로 계산할 시, 각각 V, E개 입니다.  
따라서, 최대 유량을 얻어내는데 필요한 시간 복잡도는 O((|E|+|V|)f)이다.   
장점의 개수가 무시할 만큼 작다면 O(|E|f)이 된다.   
보통, E가 V보다 훨씬 더 크기 때문에 O(|E|f)가 된다.

-----------------

[코드 형태 작성]

1. Source에서 Sink로 가는 경로를 하나 찾는다.

        public static Path findPath() {
        // dfs or bfs
        // c(a, b) - f(a, b) > 0 조건에 따라서 path을 찾을 수 있게 함.
        }     

2. 찾아낸 경로에 보낼 수 있는 최대 flow를 찾는다.

        Path path = findPath();
        FlowVal f;
        for (p in path) {
          FlowVal pathFlow = p.capacity - p.flow;
         f = Math.min(f, pathFlow)
        }

3. 찾아낸 경로에 실제 flow을 흘려보낸다.

        for (p in path) {
            path(a,b).flow += f; // 순방향에는 + 로 흘려줍니다.
            path(b,a).flow -= f; // 역방향에는 - 로 흘려줍니다.
        }


[전체 코드]

    public static int capacity[][];
    public static int flow[][];
    public static int path[]; // 
    public static boolean visited[];
    public static LinkedList<Integer> graph[];

    public static boolean dfs(int start) {
        if (start == Sink) {
            return true;
        }

        visited[start] = true;
        LinkedList<Integer> nexts = graph[start];
        for (int next: nexts) {
            if ( !visited[next] && capacity[start][next] - flow[start][next] > 0) {
                path[next] = start;

                if (dfs(next)) { // 경로를 끝까지 찾으면 탈출, 아니면, 끝까지 찾기 재시도
                    return true;
                }
            }
        }
        return false;
    }

    // O(VE^2)
    public static int EdmondsKarp() {
        int total = 0;
        while (bfs()) {
            // flow값 찾기
            int flowNum = Integer.MAX_VALUE;
            for(int i = Sink; i != Source; i = path[i]) {
                int from = path[i];
                int to = i;
                flowNum = Math.min(flowNum, (capacity[from][to]) - flow[from][to]);
            }

            // flow흘려 보내기, 역방향도 반드시!!!!
            for(int i = Sink; i != Source; i = path[i]) {
                int from = path[i];
                int to = i;

                flow[from][to] += flowNum;
                flow[to][from] -= flowNum;
            }

            total += flowNum;
        }
        return total;
    }

    // O((V+E)F)
    public static int FordFulkerson() {
        int total = 0;
        while (dfs(Source)) { // dfs로 경로 찾기(증가경로), 경로가 더이상 없으면 종료임.
            // 찾은 경로에서 차단 간선 찾기 min (capacity[u][v] - flow[u][v])
            // 결국 의미는 경로에서 흘릴수 있는 최대의 유량(flow)을 찾기
            int flowNum = Integer.MAX_VALUE;
            for(int i = Sink; i != Source; i = path[i]) {
                int from = path[i];
                int to = i;
                flowNum = Math.min(flowNum, (capacity[from][to]) - flow[from][to]);
            }
            // 찾은 경로에 유량을 흘려보내기, 역방향도 반드시!!!!
            for(int i = Sink; i != Source; i = path[i]) {
                int from = path[i];
                int to = i;

                flow[from][to] += flowNum;
                flow[to][from] -= flowNum;
            }

            total += flowNum;

            // 찾은 경로를 초기화해서 dfs로 경로 찾기를 Source > Sink 까지 다시 할 수 있게 함.
            Arrays.fill(path, -1);
            Arrays.fill(visited, false);
        }
        return total;
    }

