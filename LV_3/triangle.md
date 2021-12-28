# 프로그래머스 정수 삼각형 LV.3
[정수 삼각형](https://programmers.co.kr/learn/courses/30/lessons/43105)

## 문제 설명

```c++
    7
  3   8
8   1   0
.
.
.
```

위와 같은 삼각형의 꼭대기에서 바닥까지 이어지는 경로 중, 거쳐간 숫자의 합이 가장 큰 경우를 찾아보려고 합니다. 아래 칸으로 이동할 때는 대각선 방향으로 한 칸 오른쪽 또는 왼쪽으로만 이동 가능합니다. 예를 들어 3에서는 그 아래칸의 8 또는 1로만 이동이 가능합니다.

삼각형의 정보가 담긴 배열 triangle이 매개변수로 주어질 때, 거쳐간 숫자의 최댓값을 return 하도록 solution 함수를 완성하세요.

## 제한 사항

  * 삼각형의 높이는 1 이상 500 이하입니다.
  * 삼각형을 이루고 있는 숫자는 0 이상 9,999 이하의 정수입니다.

## 전체 코드

이것은 뭐냐면, 삼각형으로 생각을 하면, 1,2,3,4,5,6,7...식으로 내려갈수록 커지는 삼각형으로 생각을 하면 된다.
그것에 대한 규칙성을 찾아내는 것이 방법이다.
1열은 항상 윗칸과 자기 자신을 더하고
그 이후는 사이 값중 큰 값으로 자기자신과 더하면서 내려가고
마지막은 자기자신과 이전 1,1 윗칸으로 결정하면 된다.

```c++
#include<string>
#include<vector>

using namespace std;

int dp[500][500];

int solution(vector<vector<int>> triangle)
{
	int answer = 0;
	int n = triangle.size();
	dp[0][0] = triangle[0][0];
	
	for(int i = 1; i < n; i++)
	{
		for(int j = 0; j <= i; j++)
		{
			if(j == 0)
				dp[i][j] = dp[i-1][0] + triangle[i][j];
			else if(j == i)
				dp[i][j] = dp[i-1][j-1] + triangle[i][j];
			else
				dp[i][j] = max(dp[i-1][j-1], dp[i-1][j]) + triangle[i][j];
		}
	}
	
	for(int i = 0; i < n; i++)
	{
		if(answer < dp[n-1][i])
			answer = dp[n-1][i];
	}
	
	return answer;
}
```

