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

# 포드 폴커슨 알고리즘 (Ford-Fulkerson Algorithm)

소스(source) 노드에서 시작해서 싱크(sink) 노드로 도착하는 유량   
- 유량 네트워크의 모든 간선의 유량을 0으로 초기화   
- 소스에서 싱크로 유량을 더 보낼 수 있는 경로를 찾아 유량 보내기를 반복   

증강 경로로 보낼 수 있는 최대 유량은 포함된 가선의 잔여 용량 중에서 가장 작은 값

증강 경로 찾기 ? `DFS` or `BFS`

유량 상쇄의 필요성(최대값을 찾지 못할 경우를 배제 시키는 것)

*유량 상쇄란 ?*   
모든 경로에, 기존에 존재하는 간선들과 반대되는 방향의 간선을 추가한 뒤,
각 간선으로 유량을 흘려보냈을 때, 반대 방향의 간선으로도 음의 유량을 흘려보냄으로써, 유량을 상쇄시키는 것을 의미합니다.

EX) a->b의 간선이 존재하고 유량 f(a,b)은 1, 용량 c(a,b)은 1이라면 연간선 b->a의 유량 f(b->a)의 유량 f(b,a)은 기존 간선의 방향과 반대이므로 –1이 된다.   
용량 c(b,a)은 실제 존재하는 간선이 아니므로 0이 된다.
따라서, 역간선 b->a의 잔여 용량 r(b,a)은 c(b,a) - f(b,a) = 0 - (-1) = 1이 된다.  
즉, 역간선 b->a로 1의 유량을 추가적으로 흘려보낼 수 있다.
		
흘러오는 유량을 줄이는 것은 상대에게 유량을 보내주는 것과 같은 효과  
즉, a와 b는 서로 상대에게 유량을 보내 주는 것이 의미가 없기에 **소스에서 싱크로 가는 총 유량도 변하지 않는다.**

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

