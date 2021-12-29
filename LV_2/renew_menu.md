# 프로그래머스 메뉴 리뉴얼 LV.2
[메뉴 리뉴얼](https://programmers.co.kr/learn/courses/30/lessons/72411)

## 문제 설명

레스토랑을 운영하던 스카피는 코로나19로 인한 불경기를 극복하고자 메뉴를 새로 구성하려고 고민하고 있습니다.
기존에는 단품으로만 제공하던 메뉴를 조합해서 코스요리 형태로 재구성해서 새로운 메뉴를 제공하기로 결정했습니다. 어떤 단품메뉴들을 조합해서 코스요리 메뉴로 구성하면 좋을 지 고민하던 "스카피"는 이전에 각 손님들이 주문할 때 가장 많이 함께 주문한 단품메뉴들을 코스요리 메뉴로 구성하기로 했습니다.
단, 코스요리 메뉴는 최소 2가지 이상의 단품메뉴로 구성하려고 합니다. 또한, 최소 2명 이상의 손님으로부터 주문된 단품메뉴 조합에 대해서만 코스요리 메뉴 후보에 포함하기로 했습니다.

예를 들어, 손님 6명이 주문한 단품메뉴들의 조합이 다음과 같다면,
(각 손님은 단품메뉴를 2개 이상 주문해야 하며, 각 단품메뉴는 A ~ Z의 알파벳 대문자로 표기합니다.)

```c++
손님 번호	주문한 단품메뉴 조합
1번 손님	A, B, C, F, G
2번 손님	A, C
3번 손님	C, D, E
4번 손님	A, C, D, E
5번 손님	B, C, F, G
6번 손님	A, C, D, E, H
```
가장 많이 함께 주문된 단품메뉴 조합에 따라 "스카피"가 만들게 될 코스요리 메뉴 구성 후보는 다음과 같습니다.

```c++
코스 종류	메뉴 구성	설명
요리 2개 코스	A, C	1번, 2번, 4번, 6번 손님으로부터 총 4번 주문됐습니다.
요리 3개 코스	C, D, E	3번, 4번, 6번 손님으로부터 총 3번 주문됐습니다.
요리 4개 코스	B, C, F, G	1번, 5번 손님으로부터 총 2번 주문됐습니다.
요리 4개 코스	A, C, D, E	4번, 6번 손님으로부터 총 2번 주문됐습니다.
```

## 제한 사항

  * orders 배열의 크기는 2 이상 20 이하입니다.
  * orders 배열의 각 원소는 크기가 2 이상 10 이하인 문자열입니다.
    * 각 문자열은 알파벳 대문자로만 이루어져 있습니다.
    * 각 문자열에는 같은 알파벳이 중복해서 들어있지 않습니다.
  * course 배열의 크기는 1 이상 10 이하입니다.
    * course 배열의 각 원소는 2 이상 10 이하인 자연수가 오름차순으로 정렬되어 있습니다.
    * course 배열에는 같은 값이 중복해서 들어있지 않습니다.
  * 정답은 각 코스요리 메뉴의 구성을 문자열 형식으로 배열에 담아 사전 순으로 오름차순 정렬해서 return 해주세요.
    * 배열의 각 원소에 저장된 문자열 또한 알파벳 오름차순으로 정렬되어야 합니다.
    * 만약 가장 많이 함께 주문된 메뉴 구성이 여러 개라면, 모두 배열에 담아 return 하면 됩니다.
    * orders와 course 매개변수는 return 하는 배열의 길이가 1 이상이 되도록 주어집니다.

## 전체 코드

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<unordered_map>

#define MAX 27
using namespace std;

int cnt[21];
unordered_map<string,int> m;
vector<string> menu[27][21];

void backTracking(string s, string re, int idx)
{
	if(re.length() >= 2)
	{
		m[re]++; // 중복되지 않은 값들의 크기 
		cnt[re.length()] = max(cnt[re.length()], m[re]); 
		// re.length() 길이의 최대 크기, m[re]의 값을 크기 비교 
		menu[re.length()][m[re]].push_back(re);  
	}
	for(int i = idx + 1; i < s.length(); i++)
	{
		re.push_back(s[i]);
		backTracking(s,re,i);
		re.pop_back();
	}
}

vector<string> solution(vector<string> orders, vector<int> course)
{
	vector<string> answer;
	
	for(int i = 0; i < orders.size(); i++)
	{
		string s = orders[i];
		sort(s.begin(), s.end()); // 순서를 맞추는 거 꼭 필수 
		backTracking(s,"",-1);
	}
	
	for(int i = 0; i < course.size(); i++)
	{
		if(cnt[course[i]] > 1)
		{
			for(string s: menu[course[i]][cnt[course[i]]]) // auto 쓰는법 course[i] : 길이, cnt[course[i]] 는 최댓값 길이의 최댓값 
			{
				answer.push_back(s);
			}
		}
	}
	
	sort(answer.begin(), answer.end());
	
	for(int i = 0; i < answer.size(); i++)
		cout << answer[i] << " ";
	
	return answer;
}

int main()
{
	
	vector<string> o = {"ABCFG", "AC", "CDE", "ACDE", "BCFG", "ACDEH"};
	vector<int> c = {2,3,4};
	
	solution(o,c);
}
```
