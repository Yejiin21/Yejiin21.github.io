---
layout: post
title:  "Minimum Spanning Tree(Book).md"
date:   2022-03-26 15:20:28 +0900
categories: jekyll update
---

# 최소 신장 트리(Minimum Spanning Tree)
### **주어진 가중치 그래프에서 사이클이 없이 모든 점들을 연결시킨 트리들 중 간선들의 가중치 합이 최소인 트리이다.**

*신장 트리를 찾으려면 사이클이 없도록 모든 점을 연결시키면 된다. 그래프의 점의 수가 n이면, 신장 트리에는 정확히 (n-1)개의 간선이 있다. 만일 트리에 간선을 하나 추가시키면, 반드시 사이클이 만들어진다.*

최소 신장 트리를 찾는 대표적인 그리디 알고리즘으로는 **크러스컬(Kruskal)** , **프림(Prim)** 알고리즘이 있다. 단, 알고리즘의 입력은 1개의 연결성분(connected component)
으로 된 가중치 그래프이다.

******************************

## **크리스컬(Kruckal) 알고리즘**

크러스컬 알고리즘은 가중치가 가장 작은 간선이 사이클을 만들지 않을 때에만 '욕심내어' 그 간선을 추가시킨다.

**KruckalMST(G)**
    
        입력 : 가중치 그래프 G=(V, E), |V|=n(점의 수), |E|=m(간선의 수)
        출력 : 최소 신장 트리 T

    (1) 가중치의 오름차순으로 간선들을 정렬한다. 정렬된 간선 리스트를 L이라고 하자.

    (2) T = 0
    (3) while(T의 간선 수 < m-1) {
    (4)     L에서 가장 작은 가중치를 가진 간선 e를 가져오고, e를 L에서 제거한다.
    (5)     if(간선 e가 T에 추가되어 사이클을 만들지 않으면)
    (6)         e를 T에 추가시킨다.
    (7)     else
    (8)         e를 버린다.
        }
    (9) return 트리 T

* Line 1에서는 모든 간선들을 가중치의 오름차순으로 정렬한다. 정렬된 간선들의 리스트를 L이라고 하자.
* Line 2에서는 T를 초기화시킨다. 즉, T에는 아무 간선도 없는 상태에서 다음 단계가 시작된다.
* Line 3-8의 while-루프는 T의 간선 수가 (n-1)이 될 때까지 수행되는데 1번 수행될 때마다 L에서 가중치가 가장 작은 간선 e를 가져온다. 단, 가져온 간선 e는 L에서 삭제되어 다시는 고려되지 않는다.
* Line 5-8에서는 가져온 간선 e를 T에 추가하여 사이클을 만들지 않으면 e를 T에 추가시키고, 사이클을 만들면 간선 e를 버린다. 왜냐하면 모든 노드들이 연결되어 있으면서 사이클이 없는 그래프가 신장 트리이기 때문이다.

****************

### 다음 그래프에 대해서 KruckalMST 알고리즘이 최소 신장 트리를 찾는 과정을 살펴보자

![](https://postfiles.pstatic.net/MjAyMDA2MTRfMjE2/MDAxNTkyMDk3NzIxOTIz.vws21CMnWSCUMA23Hvmc6Bcr5yRdgZjFKBuIJ8z65VAg.jZGUBTQDhximrqmYIw4haALPTXv4I68tI11LitHwUtAg.PNG.diddnjs02/image.png?type=w773)

1. 가중치가 가장 낮은 간선 (E,G) 삽입 => 삽입한 간선 수 1개

![](https://postfiles.pstatic.net/MjAyMDA2MTRfMTgw/MDAxNTkyMDk4NjExNjE0.sNOMt-EcEMwbYnwph3AO6DeYEB3nUtljZUq2cKSjmngg.qG84Q4G1mjBoWZyxDRK0TYKKwxriPnd-bigUvYylVS0g.PNG.diddnjs02/image.png?type=w773)

1-2. 나머지 간선 중 가중치가 가장 낮은 간선(A,B)삽입 => 삽입한 간선 수 2개

![](https://postfiles.pstatic.net/MjAyMDA2MTRfMTA3/MDAxNTkyMDk4NjU2OTUy.tTXk2-9a15wmoDBwH_ZJLJhxDYu3OAwPhVS0TmwAolAg.WIDOaX8jbSo1qZqH1AJZEB2xmcLfgvWtZPDQc4FA3BEg.PNG.diddnjs02/image.png?type=w773)

1-3. 나머지 간선 중 가중치가 가장 낮은 간선(E,F)삽입 => 삽입한 간선 수 3개

![](https://postfiles.pstatic.net/MjAyMDA2MTRfMTA2/MDAxNTkyMDk4NjgzNDM2.ATSlOJ-LcKCpookpBH0CnPCOjjbf8n5jk7YrgYsYULQg.IafDw7Rq7TLsZ59oX_FYF7Bj-T1p_DlQh46Wmj5pUnog.PNG.diddnjs02/image.png?type=w773)

1-4. 나머지 간선 중 가중치가 가장 낮은 간선(B,D))삽입 => 삽입한 간선 수 4개

![](https://postfiles.pstatic.net/MjAyMDA2MTRfMjIg/MDAxNTkyMDk4NzExMzQy.tox0lBVgmrP-2ZyWewup5s8Q-6jNoXZDTOFyemuOSBUg.f9ZM2KTBxhHZtHVPMhV1RRccN6x3mIk9h3wVW__tlrUg.PNG.diddnjs02/image.png?type=w773)

1-5. 나머지 간선 중 가중치가 가장 낮은 간선(A),D))삽입 => **A-B-D-A 사이클 생김!! 삽입 불가** => 그 다음으로 가중치가 낮은 (C,F)삽입 삽입한 간선 수 5개

![](https://postfiles.pstatic.net/MjAyMDA2MTRfMTc5/MDAxNTkyMDk4NzY0OTcx.Ub2uD2Y2Oo0gNIe70PoktBD_VXcvB-w5eRVfqgsmpsgg.aLA6xjyO9Y0I1lwfLKXZwkm-1jVn3_hVdp-Pz-c_JR0g.PNG.diddnjs02/image.png?type=w773)

1-6. 나머지 간선 중 가중치가 가장 낮은 간선(D,E))삽입 => 삽입한 간선 수 6개

![](https://postfiles.pstatic.net/MjAyMDA2MTRfODkg/MDAxNTkyMDk4Nzk1MDcw.CzFk80bu2P-GuspZCP_P04_qcMgMO6ZStooEZlImyRUg.P6GUZPFgN1zLMAKqTqMlby_Kr0BiZr4CT22XtDxmM2cg.PNG.diddnjs02/image.png?type=w773)

2. 현재 삽입한 간선 수는 7-1 = 6 
    => 알고리즘 종료

![](https://postfiles.pstatic.net/MjAyMDA2MTRfMjkz/MDAxNTkyMDk4ODM4NTMx.w2MyI5NXW10pCDc8W2eGMA-7ROkoc9MVDr-r-QA3-H4g.IDEoJRQ9VAsa78NGsCGqejzFMpfFvsmNq0YIW7qKZggg.PNG.diddnjs02/image.png?type=w773)

### **시간복잡도**

Line 1에서는 간선들을 가중치로 정렬하는 데 O(mlogm) 시간이 걸린다. 단, m은 입력 그래프에 있는 간선의 수이다.   
Line 2에서는 T를 단순히 초기화하는 것이므로 O(1) 시간이 걸린다.   
Line 3-8의 while-루프는 최악의 경우 m번 수행된다. 즉, 그래프의 모든 간선이 while-루프 내에서 처리되는 경우이다. 그리고 while-루프 내에서는 L로부터 가져온 간선 e가 사이클을 만드는지를 검사하는데, 이는 O(log*m) 시간이 걸린다.   
따라서 크러스컬 알고리즘의 시간복잡도는 **O(mlogm)+O(mlogm) = O(mlogm)** 이다.

*****************
## **프림(Prim) 알고리즘**
최소 신장 트리 알고리즘은 주어진 가중치 그래프에서 임의의 점 하나를 선택한 후, (n-1)개의 간선을 하나씩 추가시켜 트리를 만든다.

**PrimMST(G)**

    입력 : 가중치 그래프 G=(V,E), |V|=n(점의 수), |E|=m(간선의 수)
    출력 : 최소 신장 트리 T

    (1) 그래프 G에서 임의의 점 p를 시작점으로 선택하고, D[p]=0dmfh 놓는다.
        // D[v]는 T에 있는 점 u와 v를 연결하는 간선의 최소 가중치를 저장한다.

    (2) for(점 p가 아닌 각 점 v에 대하여) { // 배열 D의 최소화
    (3)     if(간선 (p,v)가 그래프에 있으면)
    (4)         D[v] = 간선 (p,v)의 가중치
    (5) else
    (6)     D[v]=무한
       }
    (7) T={p} // 초기에 트리 T는 점 p만을 가진다.
    (8) while (T에 있는 점의 수 < n) {
    (9) T에 속하지 않은 각 점 v에 대하여, D[v]가 최소인 점 V(min)과 연결된 간선(u,v (min))을 T에 추가한다. 단, u는 T에 속한 점이고, 이때 점 v(min)도 T에 추가된다.
    (10) for(T에 속하지 않은 각 점 w에 대하여) {
    (11)    if(간선(v(min),w)의 가중치 
    (12)        D[w] = 간선(v(min),w)의 가중치 //D[w]를 갱신한다.
        }
            }
    (13) return T //T는 최소 신장 트리이다.

*******************************

### 다음 그래프에 대해서 Prim 알고리즘이 최소 신장 트리를 찾는 과정을 살펴보자

간선을 정렬하지 않고 **하나의 정점에서 시작** 하여 트리를 확장해 나가는 방법

    1. 그래프에서 시작 정점을 선택한다. 
    2. 선택한 정점에 인접한 모든 간선 중 가중치가 가장 낮은 간선을 연결하여 트리를 확장한다.
    3. 이전에 선택한 정점과 새로 확장된 정점 모두에 인접한 간선 중 가중치가 가장 낮은 간선을 삽입한다. 단, 사이클을 형성하는 간선은 삽입할 수 없으므로 이런 경우에는 그 다음으로 가중치가 낮은 간선​을 선택한다.
    4. 그래프의 간선이 n-1개 삽입될 때까지 과정 3을 반복한다.
    5. 그래프의 간선이 n-1개가 되면 최소 비용 신장 트리가 완성된다. 

1. 시작 정점 A를 선택한다.

![](https://postfiles.pstatic.net/MjAyMDA2MTRfMTAg/MDAxNTkyMDk5MDM1NDQw.0H2RGK8OH4H13KgwJyhWiWR23VZyPqPyvdoNc28sNvIg.7ZB3G38EK7WB8x5V1lET0x_mYtJc3sQvfieegZRFpIAg.PNG.diddnjs02/image.png?type=w773)

2. A와 인접한 간선들 중 가중치가 가장 낮은 애는? 3 => A-B 연결

    2-1. A,B 두 정점에 인접한 간선들 중 가중치가 가장 낮은 애는? 5 => A-B-D 연결

    2-2. A,B,D 모두에 인접한 간선들 중 가중치가 가장 낮은 애는? 6   
    BUT 6을 연결하면 A-B-D-A 사이클 발생하므로 연결 불가 => SKIP하기

    2-3. 그 다음 가중치가 낮은 애는? 9

3. 모든 정점들을 연결하면 종료

![](https://postfiles.pstatic.net/MjAyMDA2MTRfMTU3/MDAxNTkyMDk5MjAyMTUz.tLY0kPHxrV7el36cTMk4vfYDZe0vyIQ6GHoFcoEg3HMg.wcg2nYKpfKj6mNPVWiDyDbPqgEMmpE13FPjWcN4lJjkg.PNG.diddnjs02/image.png?type=w773)

### **시간복잡도**

while-루프가 (n-1)번 반복되고, 1회 반복될 때 line 9에서 T에 속하지 않은 각 점 v에 대하여, D[v]가 최소인 점 v(min)을 찾는 데 O(n) 시간이 걸린다.   
왜냐하면 1차원 배열 D에서 (현재 T에 속하지 않은 점들에 대하여) 최솟값을 찾는 것이고, 배열의 크기는 그래프의 점의 수인 n이기 때문이다.  
따라서 프림 알고리즘의 시간복잡도는 **(n-1) X O(n) = O(n^2)** 이다.

*******************

## *크러스컬 알고리즘과 프림 알고리즘의 수행과정 비교*

* 크러스컬 알고리즘에서는 간선이 하나씩 T에 추가되는데, 이는 마치 n개의 점들이 각각의 트리인 상태에서 간선이 추가되면 2개의 트리가 1개의 트리로 합쳐지는 것과 같다.   
크러스컬 알고리즘은 이를 반복하여 1개의 트리인 T를 만든다.   
**즉, n개의 트리들이 점차 합쳐져서 1개의 신장 트리가 만들어진다.**

* 프림 알고리즘에서는 T가 점 1개인 트리에서 시작되어 간선을 하나씩 추가시킨다.   
**즉, 1개의 트리가 자라나서 신장 트리가 된다.**