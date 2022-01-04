# 프로그래머스 가장큰수 LV.2
[가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746)

## 문제 설명

0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

## 제한 사항

  * numbers의 길이는 1 이상 100,000 이하입니다.
  * numbers의 원소는 0 이상 1,000 이하입니다.
  * 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

## 전체 코드

문자열 비교를 통해서 sort하는 과정을 나타낸 것인데 좀 더 응용해서 문제를 해결할 필요가 있다고 생각이 듦

```c++
#include<iostream>
#include<string>
#include<vector>
#include<algorithm>

using namespace std;

vector<int> string_number;

bool comp(string s, string t)
{
	if(s + t > t + s)
		return true;
	return false;
}

string solution(vector<int> numbers)
{
	string answer = "";
	
	vector<string> num;
	
	for(int i = 0; i < numbers.size(); i++)
		num.push_back(to_string(numbers[i]));
	
	
	sort(num.begin(), num.end(), comp);
	
	for(int i = 0; i < num.size(); i++)
		answer = answer + num[i];
		
	if(answer[0] == '0')
		answer = '0';
	
	return answer;
}

```
