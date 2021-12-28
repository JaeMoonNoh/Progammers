# 프로그래머스 네트워크 LV.3
[네트워크](https://programmers.co.kr/learn/courses/30/lessons/43162)

## 문제 설명

네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

## 제한사항

  * 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
  * 각 컴퓨터는 0부터 n-1인 정수로 표현합니다.
  * i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
  * computer[i][i]는 항상 1입니다.

## 전체코드

전형적인 DFS문제, 3정도의 문제는 아닌거 같다.

```c++
#include<vector>
#include<string>

using namespace std;


bool check[201] = {false,};

void DFS(int c, int n, vector<vector<int> >& computers)
{
	check[c] = true;
	for(int i = 0; i < n; i++)
	{
		if(computers[c][i] == true && !check[i])
		{
			DFS(i,n,computers);
		}
	}
}

int solution(int n, vector<vector<int> > computers)
{
	int answer = 0;
	int length = computers.size();
	for(int i = 0 ; i < length; i++)
	{
		if(check[i] == false)
		{
			answer++;
			DFS(i, length,computers);
		}
	}
	
	
	return answer;
}
```

