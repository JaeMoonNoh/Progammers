# 프로그래머스 단어 변환 LV.3

[단어 변환](https://programmers.co.kr/learn/courses/30/lessons/43163)

## 문제 설명

두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

```c++
1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.
```
예를 들어 begin이 "hit", target가 "cog", words가 ["hot","dot","dog","lot","log","cog"]라면 "hit" -> "hot" -> "dot" -> "dog" -> "cog"와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

## 제한사항

  * 각 단어는 알파벳 소문자로만 이루어져 있습니다.
  * 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
  * words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
  * begin과 target은 같지 않습니다.
  * 변환할 수 없는 경우에는 0를 return 합니다.

## 전체 코드


```c++
#include<iostream>
#include<string>
#include<vector>

using namespace std;

int result = 100;

void DFS(string begin, string target, vector<string> words, vector<bool> visited, int cnt)
{
	int count = 0;
	
	for(int i = 0; i < words.size(); i++)
	{
		count = 0;
		string tempBegin = begin; // 시작 string 
		
		for(int j = 0; j < words[i].size(); j++)
		{
			if(!visited[i] && begin[j] != words[i][j]) // 한글자씩 변환 
				count++;
		}
		
		if(count == 1) // 한글자만 차이가 나면, 
		{
			if(target == words[i] && result > cnt + 1) // 타겟이랑 같고, result > cnt + 1 보다 크면 더 빨리 바뀐것이니까. 
			{
				//가장 짧은 변환 과정을 의미 
				result = cnt + 1;
				return;
			}
			
			visited[i] = true; // 한번 거쳐갔고 
			DFS(words[i], target, words,visited,cnt+1);
			visited[i] = false; // 거쳐간거 풀어주고 
		}
		else
		{
			continue // 한글자 이상 차이가 나면 continue; 
		}
	}
}

int solution(string begin, string target, vector<string> words)
{
	int answer = 0;
	
	vector<bool> visited(words.size(), false);
	
	DFS(begin, target, words, visited, 0);
	
	if(result == 100)
		return 0;
		
	answer = result;
	
	return answer;
}
```
