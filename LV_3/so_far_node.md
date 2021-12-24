# 프로그래머스 가장 먼 노드 그래프

[가장 먼 노드](https://programmers.co.kr/learn/courses/30/lessons/49189)

## 문제 설명

n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.

## 제한 사항

  * 노드의 개수 n은 2 이상 20,000 이하입니다.
  * 간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
  * vertex 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.

## 전체 코드

다익스트라를 사용해서 문제를 해결했지만, 다른 방법이 있는지는 떠오르는 아이디어가 없다.

```c++
#include<iostream>
#include<string>
#include<vector>
#include<queue>
#include<algorithm>

#define MAX 20000 + 1
#define INF 987654321
using namespace std;

vector<pair<int,int> > graph[MAX];

vector<int> dijkstra(int start, int vertex)
{
	vector<int> distance(vertex,INF);
	distance[start] = 0;
	
	priority_queue<pair<int,int> > pq;
	pq.push({0,start});
	
	while(!pq.empty())
	{
		int curVertex = pq.top().second;
		int cost = -pq.top().first;
		pq.pop();
		
		if(distance[curVertex] < cost)
			continue;
			
		for(int i = 0; i < graph[curVertex].size(); i++)
		{
			int neighbor = graph[curVertex][i].second;
			int neighborDist = graph[curVertex][i].first + cost;
			
			if(distance[neighbor] > neighborDist)
			{
				distance[neighbor] = neighborDist;
				pq.push({-neighborDist, neighbor});
			}
		}
	}
	return distance;
} 

int solution(int n, vector<vector<int> > edge)
{
	int answer = 0;
	
	for(int i = 0; i < edge.size(); i++)
	{
		graph[edge[i][0]].push_back({1,edge[i][1]});
		graph[edge[i][1]].push_back({1,edge[i][0]});
	}
	
	vector<int> result = dijkstra(1,n+1);
	
	result.erase(result.begin());
	sort(result.begin(), result.end());
	
	int maxValue = result.back();
	
	for(int i = 1; i < result.size(); i++)
	{
		if(maxValue == result[i])
			answer++;
	}
	return  answer;
}
```
