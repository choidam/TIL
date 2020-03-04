# DFS 와 BFS

![screenshot](./screenshots/dfs-bfs.gif)



### 깊이 우선 탐색 (DFS; Depth First Search)
- 현재 정점에서 갈 수 있는 점들까지 들어가면서 탐색함
- 스택 또는 재귀함수로 구현함
- 빠르게 모든 경우의 수를 탐색하고자 할 때 사용함
<br/>



```c++
#include <iostream>
#include <stdio.h>
#include <vector>
#include <algorithm>

using namespace std;

// dfs에 들어오면 '방문'한거로 판단
// 해당 위치에 check true로 해준다.
void dfs(int start, vector<int> graph[], bool check[]){
	check[start]= true;
	printf("%d ", start);

	for(int i=0; i < graph[start].size(); i++){
		int next = graph[start][i];
		// 방문하지 않았다면
		if(check[next]==false){
			// 재귀함수를 호출한다.
			dfs(next, graph, check);
		}
	}
}

int main (){

	int n, m, start;
	cin >> n >> m >> start;

	vector<int> graph[n+1];
	bool check [n+1];
	fill(check, check+n+1, false);

	for(int i=0; i<m; i++){
		int u,v;
		cin >> u >> v;

		graph[u].push_back(v);
		graph[v].push_back(u);
	}

	for(int i=1; i<=n; i++){
		sort(graph[i].begin(), graph[i].end());
	}

	//dfs
	dfs(start, graph, check);
	printf("\n");

	return 0;
}
```



<br/>


### 너비 우선 탐색 (BFS; Breadth First Search)
- 큐를 이용해서 구현 (FIFO)
- 루트 노드에서 시작해서 인접한 노드를 먼저 탐색하는 방법
- 깊게(deep) 탐색하기 전에 넓게(wide) 탐색함
- 두 노드 사이에 최단 경로 혹은 임의의 경로를 찾고 싶을 때 사용함
<br/>


```c++
#include <iostream>
#include <stdio.h>
#include <vector>
#include <algorithm>
#include <queue>

using namespace std;

void bfs(int start, vector<int> graph[], bool check[]){
	queue<int> q;

	q.push(start);
	check[start] = true;

	while(!q.empty()){
		int tmp = q.front();
		q.pop();
		printf("%d ",tmp);
		for(int i=0; i<graph[tmp].size(); i++){

			// 방문하지 않았다면
			if(check[graph[tmp][i]] == false){
				// 큐에 넣어주고 방문했음을 표시한다.
				q.push(graph[tmp][i]);
				check[graph[tmp][i]] = true;
			}
		}
	}

}

int main (){

	int n, m, start;
	cin >> n >> m >> start;

	vector<int> graph[n+1];
	bool check [n+1];
	fill(check, check+n+1, false);

	for(int i=0; i<m; i++){
		int u,v;
		cin >> u >> v;

		graph[u].push_back(v);
		graph[v].push_back(u);
	}

	for(int i=1; i<=n; i++){
		sort(graph[i].begin(), graph[i].end());
	}

	//bfs
	bfs(start, graph, check);
	printf("\n");

	return 0;
}
```


