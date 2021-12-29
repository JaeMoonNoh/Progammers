# 프로그래머스 순위 LV.3
[순위](https://programmers.co.kr/learn/courses/30/lessons/49191)

## 문제 설명

n명의 권투선수가 권투 대회에 참여했고 각각 1번부터 n번까지 번호를 받았습니다. 권투 경기는 1대1 방식으로 진행이 되고, 만약 A 선수가 B 선수보다 실력이 좋다면 A 선수는 B 선수를 항상 이깁니다. 심판은 주어진 경기 결과를 가지고 선수들의 순위를 매기려 합니다. 하지만 몇몇 경기 결과를 분실하여 정확하게 순위를 매길 수 없습니다.

선수의 수 n, 경기 결과를 담은 2차원 배열 results가 매개변수로 주어질 때 정확하게 순위를 매길 수 있는 선수의 수를 return 하도록 solution 함수를 작성해주세요.

## 제한 사항

  * 선수의 수는 1명 이상 100명 이하입니다.
  * 경기 결과는 1개 이상 4,500개 이하입니다.
  * results 배열 각 행 [A, B]는 A 선수가 B 선수를 이겼다는 의미입니다.
  * 모든 경기 결과에는 모순이 없습니다.

## 전체 코드

```c++
#include<iostream>
#include<string>
#include<vector>

#define MAX 101
#define INF 987654321
using namespace std;

int graph[MAX][MAX];

int solution(int n, vector<vector<int>> results)
{
	int answer = 0;
	
	for(int i = 1; i <= n; i++)
		for(int j = 1; j <= n; j++)
			graph[i][j] = INF;
			
	for(auto &c : results)
	{
		graph[c[0]][c[1]] = 1;
		graph[c[1]][c[0]] = -1;
	}
	
	for(int via = 1; via <= n; via++)
	{
		for(int from = 1; from <= n; from++)
		{
			for(int to = 1; to <= n; to++)
			{
				if(graph[from][to] == INF)
				{
					if(graph[from][via] == 1 && graph[via][to] == 1)
						graph[from][to] = 1;
					if(graph[from][via] == -1 && graph[via][to] == -1)
						graph[from][to] = -1;
				}
			}
		}
	}
	
	for(int i = 1; i <= n; i++)
	{
		bool chk = false;
		
		for(int j = 1; j <= n; j++)
		{
			if(i == j)
				continue;
			if(graph[i][j] == INF) // INF 가 동일 i,j가 아닌 상태에서 나오게 되었다면 정확한 순위를 모르는 것이다.
			{
				chk = true;
				break;
			}
		}
		answer += chk ? 0 : 1;
	}
	return answer;
}
```
