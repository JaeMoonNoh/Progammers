# 프로그래머스 카카오 프렌즈 컬러링 북 2017 kakaoCode

[카카오 프렌즈 컬러링 북](https://programmers.co.kr/learn/courses/30/lessons/1829)

## 문제 설명

출판사의 편집자인 어피치는 네오에게 컬러링북에 들어갈 원화를 그려달라고 부탁하여 여러 장의 그림을 받았다. 여러 장의 그림을 난이도 순으로 컬러링북에 넣고 싶었던 어피치는 영역이 많으면 색칠하기가 까다로워 어려워진다는 사실을 발견하고 그림의 난이도를 영역의 수로 정의하였다. (영역이란 상하좌우로 연결된 같은 색상의 공간을 의미한다.)

그림에 몇 개의 영역이 있는지와 가장 큰 영역의 넓이는 얼마인지 계산하는 프로그램을 작성해보자.


위의 그림은 총 12개 영역으로 이루어져 있으며, 가장 넓은 영역은 어피치의 얼굴면으로 넓이는 120이다.

## 입력 형식

입력은 그림의 크기를 나타내는 `m`과 `n`, 그리고 그림을 나타내는 `m × n` 크기의 2차원 배열 `picture`로 주어진다. 제한조건은 아래와 같다.

  * `1 <= m, n <= 100`
  * `picture`의 원소는 `0` 이상 `2^31 - 1` 이하의 임의의 값이다.
  * `picture`의 원소 중 값이 `0`인 경우는 색칠하지 않는 영역을 뜻한다.

## 출력 형식

리턴 타입은 원소가 두 개인 정수 배열이다. 그림에 몇 개의 영역이 있는지와 가장 큰 영역은 몇 칸으로 이루어져 있는지를 리턴한다.

## 전체 코드


```c++
#include<iostream>
#include<cstring>
#include<vector>
#include<queue>

#define MAX 101
using namespace std;

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0};

bool visited[MAX][MAX];

int BFS(int y, int x, int m, int n, int num, int area, vector<vector<int> > picture)
{
	queue<pair<int,int> > q;
	q.push({y,x});
	
	visited[y][x] = true;
	int cnt = 0;
	int num = num;
	
	while(!q.empty())
	{
		int y = q.front().first;
		int x = q.front().second;
		cnt++;
		q.pop();
		
		for(int i = 0; i < 4; i++)
		{
			int nx = x + dx[i];
			int ny = y + dy[i];
			
			if(0 <= ny && ny < m && 0 <= nx && nx < n)
			{
				if(!visited[ny][nx] && picture[ny][nx] == num)
				{
					visited[ny][nx] = true;
					q.push({ny,nx});
				}
			}
		}
	}
	return cnt;
}

vector<int> solution(int m, int n, vector<vector<int> > picture)
{
	int area = 0;
	int size_area = 0;
	
	memset(visited,0,sizeof(visited));
	
	for(int i = 0; i < m; i++)
	{
		for(int j = 0; j < n; j++)
		{
			if(!visited[i][j] && picture[i][j] != 0)
			{
				int size = BFS(i,j,m,n,picture[i][j],area,picture);
				if(size > size_area)
				{
					size_area = size;
				}
				area++;
			}
		}
	}
	
	vector<int> answer(2,0);
	answer[0] = area;
	answer[1] = size_area;
	
	return answer;
}
```
