# 프로그래머스 거리두기 확인하기 LV.2
[거리두기 확인하기](https://programmers.co.kr/learn/courses/30/lessons/81302)

## 문제 설명

개발자를 희망하는 죠르디가 카카오에 면접을 보러 왔습니다.

코로나 바이러스 감염 예방을 위해 응시자들은 거리를 둬서 대기를 해야하는데 개발 직군 면접인 만큼
아래와 같은 규칙으로 대기실에 거리를 두고 앉도록 안내하고 있습니다.

  1. 대기실은 5개이며, 각 대기실은 5x5 크기입니다.
  2. 거리두기를 위하여 응시자들 끼리는 맨해튼 거리1가 2 이하로 앉지 말아 주세요.
  3. 단 응시자가 앉아있는 자리 사이가 파티션으로 막혀 있을 경우에는 허용합니다.

예를 들어,
위 그림처럼 자리 사이에 파티션이 존재한다면 맨해튼 거리가 2여도 거리두기를 지킨 것입니다.	위 그림처럼 파티션을 사이에 두고 앉은 경우도 거리두기를 지킨 것입니다.	위 그림처럼 자리 사이가 맨해튼 거리 2이고 사이에 빈 테이블이 있는 경우는 거리두기를 지키지 않은 것입니다.

응시자가 앉아있는 자리(P)를 의미합니다.	빈 테이블(O)을 의미합니다.	파티션(X)을 의미합니다.
5개의 대기실을 본 죠르디는 각 대기실에서 응시자들이 거리두기를 잘 기키고 있는지 알고 싶어졌습니다. 자리에 앉아있는 응시자들의 정보와 대기실 구조를 대기실별로 담은 2차원 문자열 배열 places가 매개변수로 주어집니다. 각 대기실별로 거리두기를 지키고 있으면 1을, 한 명이라도 지키지 않고 있으면 0을 배열에 담아 return 하도록 solution 함수를 완성해 주세요.

## 제한 사항

  * places의 행 길이(대기실 개수) = 5
    * places의 각 행은 하나의 대기실 구조를 나타냅니다.
  * places의 열 길이(대기실 세로 길이) = 5
  * places의 원소는 P,O,X로 이루어진 문자열입니다.
    * places 원소의 길이(대기실 가로 길이) = 5
    * P는 응시자가 앉아있는 자리를 의미합니다.
    * O는 빈 테이블을 의미합니다.
    * X는 파티션을 의미합니다.
  * 입력으로 주어지는 5개 대기실의 크기는 모두 5x5 입니다.
  * return 값 형식
    * 1차원 정수 배열에 5개의 원소를 담아서 return 합니다.
    * places에 담겨 있는 5개 대기실의 순서대로, 거리두기 준수 여부를 차례대로 배열에 담습니다.
    * 각 대기실 별로 모든 응시자가 거리두기를 지키고 있으면 1을, 한 명이라도 지키지 않고 있으면 0을 담습니다.

## 전체 코드

```c++
#include<iostream>
#include<cstring>
#include<vector>
#include<queue>

using namespace std;

int map[6][6];
int map2[6][6];
bool visited[6][6];

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0};

vector<int> solution(vector<vector<string> > places)
{
	
	vector<int> answer;
	
	for(int i = 0; i < places.size(); i++)
	{
		memset(map, 0, sizeof(map));
		memset(visited,0,sizeof(visited));
		vector<pair<int,int> > location;
		for(int j = 0; j < places[i].size(); j++)
		{
			for(int k = 0; k < 5; k++)
			{
				if(places[i][j][k] == 'P')
				{
					map[j][k] = 0;
					location.push_back({j,k});
				}
				else if(places[i][j][k] == 'O')
				{
					map[j][k] = 0;
				}
				else
				{
					map[j][k] = -1;
				}	
			}
		}
		
		bool chk = true;
		
		for(int i = 0; i < location.size(); i++)
		{
			memset(visited,false,sizeof(visited));
			memset(map2,0,sizeof(map2));
			
			int y = location[i].first;
			int x = location[i].second;
			queue<pair<int,int> > q;
			q.push({y,x});
			visited[y][x] = true;
			
			while(!q.empty())
			{
				int qy = q.front().first;
				int qx = q.front().second;
				q.pop();
				
				for(int k = 0; k < 4; k++)
				{
					int ny = qy + dy[k];
					int nx = qx + dx[k];
					
					if(0 <= nx && nx < 5 && 0 <= ny && ny < 5)
					{
						if(!visited[ny][nx] && map[ny][nx] != -1)
						{
							visited[ny][nx] = true;
							map2[ny][nx] = map2[qy][qx] + 1;
							q.push({ny,nx});
						}	
					}
				}
			}
			for(int j = 0; j < location.size(); j++)
			{
				if(x == location[j].second && y == location[j].first)
					continue;
				
				if(visited[location[j].first][location[j].second] && map2[location[j].first][location[j].second] <= 2)
				{
					chk = false;
					break;
				}
			}
			if(!chk)
			{
				answer.push_back(0);
				break;
			}
		}
		
		if(chk)
		{
			answer.push_back(1);
		}
		
	}
	return answer;
}

int main()
{
	vector<vector<string> > p = {{"POOOP", "OXXOX", "OPXPX", "OOXOX", "POXXP"},{"POOPX", "OXPXP", "PXXXO", "OXXXO", "OOOPP"},{"PXOPX", "OXOXP", "OXPXX", "OXXXP", "POOXX"},{"OOOXX", "XOOOX", "OOOXX", "OXOOX", "OOOOO"},{"PXPXP", "XPXPX", "PXPXP", "XPXPX", "PXPXP"}};
	solution(p);
	return 0;
}
```
