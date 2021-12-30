# 프로그래머스 보석 쇼핑 LV.3
[보석 쇼핑](https://programmers.co.kr/learn/courses/30/lessons/67258)

## 문제 설명

개발자 출신으로 세계 최고의 갑부가 된 어피치는 스트레스를 받을 때면 이를 풀기 위해 오프라인 매장에 쇼핑을 하러 가곤 합니다.
어피치는 쇼핑을 할 때면 매장 진열대의 특정 범위의 물건들을 모두 싹쓸이 구매하는 습관이 있습니다.
어느 날 스트레스를 풀기 위해 보석 매장에 쇼핑을 하러 간 어피치는 이전처럼 진열대의 특정 범위의 보석을 모두 구매하되 특별히 아래 목적을 달성하고 싶었습니다.
진열된 모든 종류의 보석을 적어도 1개 이상 포함하는 가장 짧은 구간을 찾아서 구매

예를 들어 아래 진열대는 4종류의 보석(RUBY, DIA, EMERALD, SAPPHIRE) 8개가 진열된 예시입니다.
```c++
진열대 번호	1	2	3	4	5	6	7	8
보석 이름	DIA	RUBY	RUBY	DIA	DIA	EMERALD	SAPPHIRE	DIA
```
진열대의 3번부터 7번까지 5개의 보석을 구매하면 모든 종류의 보석을 적어도 하나 이상씩 포함하게 됩니다.

진열대의 3, 4, 6, 7번의 보석만 구매하는 것은 중간에 특정 구간(5번)이 빠지게 되므로 어피치의 쇼핑 습관에 맞지 않습니다.

진열대 번호 순서대로 보석들의 이름이 저장된 배열 gems가 매개변수로 주어집니다. 이때 모든 보석을 하나 이상 포함하는 가장 짧은 구간을 찾아서 return 하도록 solution 함수를 완성해주세요.
가장 짧은 구간의 시작 진열대 번호와 끝 진열대 번호를 차례대로 배열에 담아서 return 하도록 하며, 만약 가장 짧은 구간이 여러 개라면 시작 진열대 번호가 가장 작은 구간을 return 합니다.

## 제한 사항

  * gems 배열의 크기는 1 이상 100,000 이하입니다.
    * gems 배열의 각 원소는 진열대에 나열된 보석을 나타냅니다.
    * gems 배열에는 1번 진열대부터 진열대 번호 순서대로 보석이름이 차례대로 저장되어 있습니다.
    * gems 배열의 각 원소는 길이가 1 이상 10 이하인 알파벳 대문자로만 구성된 문자열입니다.

## 전체 코드

```c++
#include<iostream>
#include<string>
#include<map>
#include<vector>
#include<queue>

using namespace std;

vector<int> solution(vector<string> gems)
{
	vector<int> answer = {0,0};
	queue<string> q;
	map<string,int> m;
	int mSize = 0;
	
	for(int i = 0; i < gems.size(); i++)
		m[gems[i]] = 1; // 총보석의 개수를 구해야한다.
		// 진열장에 있는 보석을 하나 이상씩은 다 챙겨야하기 때문에 
		
	mSize = m.size();
	m.clear();
	
	int from = 0;
	int first = 0;
	int last = 10000001; // 100000이하의 수 
	
	for(int i = 0; i < gems.size(); i++)
	{
		if(m[gems[i]] == 0) // 보석이 등록되어있지 않으면 
			m[gems[i]] = 1;
		else
			m[gems[i]]++; // 보석이 등록되어있으면 
			
		q.push(gems[i]);
		
		while(1)
		{
			if(m[q.front()] > 1) // 현재 큐에 들어가있는 값이 1을 초과하면 
			{
				m[q.front()]--; // q.front() 빼주기 
				q.pop();
				from++; // from++
			}
			else
				break;
		}
		
		if(m.size() == mSize && last > q.size())
		{
			first = from;
			last = q.size();
		}
	}
	
	answer[0] = first + 1;
	answer[1] = first + last;
	
	cout << answer[0] << " " << answer[1] << endl;
	
	return answer;
}

int main()
{
	
	vector<string> v = {"DIA", "RUBY", "RUBY", "DIA", "DIA", "EMERALD", "SAPPHIRE", "DIA"};
	
	solution(v);
}
```
