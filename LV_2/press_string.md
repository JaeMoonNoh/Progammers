# 프로그래머스 문자열 압축 2020 kakao blind

[문자열 압축](https://programmers.co.kr/learn/courses/30/lessons/60057)

## 문제 설명

데이터 처리 전문가가 되고 싶은 "어피치"는 문자열을 압축하는 방법에 대해 공부를 하고 있습니다. 최근에 대량의 데이터 처리를 위한 간단한 비손실 압축 방법에 대해 공부를 하고 있는데, 문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 알고리즘을 공부하고 있습니다.
간단한 예로 "aabbaccc"의 경우 "2a2ba3c"(문자가 반복되지 않아 한번만 나타난 경우 1은 생략함)와 같이 표현할 수 있는데, 이러한 방식은 반복되는 문자가 적은 경우 압축률이 낮다는 단점이 있습니다. 예를 들면, "abcabcdede"와 같은 문자열은 전혀 압축되지 않습니다. "어피치"는 이러한 단점을 해결하기 위해 문자열을 1개 이상의 단위로 잘라서 압축하여 더 짧은 문자열로 표현할 수 있는지 방법을 찾아보려고 합니다.

예를 들어, "ababcdcdababcdcd"의 경우 문자를 1개 단위로 자르면 전혀 압축되지 않지만, 2개 단위로 잘라서 압축한다면 "2ab2cd2ab2cd"로 표현할 수 있습니다. 다른 방법으로 8개 단위로 잘라서 압축한다면 "2ababcdcd"로 표현할 수 있으며, 이때가 가장 짧게 압축하여 표현할 수 있는 방법입니다.

다른 예로, "abcabcdede"와 같은 경우, 문자를 2개 단위로 잘라서 압축하면 "abcabc2de"가 되지만, 3개 단위로 자른다면 "2abcdede"가 되어 3개 단위가 가장 짧은 압축 방법이 됩니다. 이때 3개 단위로 자르고 마지막에 남는 문자열은 그대로 붙여주면 됩니다.

압축할 문자열 s가 매개변수로 주어질 때, 위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라 압축하여 표현한 문자열 중 가장 짧은 것의 길이를 return 하도록 solution 함수를 완성해주세요.

## 제한 사항

  * s의 길이는 1 이상 1,000 이하입니다.
  * s는 알파벳 소문자로만 이루어져 있습니다.

## 전체 코드

```c++
#include<iostream>
#include<string>
#include<vector>

using namespace std;

int solution(string s)
{
	string result = "";
	int len = s.size();
	int answer = len;
	
	int n = len / 2;
	
	for(int i = 1; i <= n; i++)
	{
		result = "";
		string substring = s.substr(0,i);
		
		int cnt = 1;
		
		for(int j = i; j <= len; j+=i) // i 길이만큼 이동을 해야한다. 
		{
			if(substring == s.substr(j,i))
			{
				cnt += 1;
			}
			else
			{
				if(cnt == 1)
				{
					result += substring;
				}
				else
				{
					result += (to_string(cnt) + substring);
				}
				
				substring = s.substr(j,i);
				cnt = 1;
			}
		}
		if((len%i) != 0)
		{
			result += s.substr((len/i)*i); // 길이 / 나누는 몫 * 나누는 몫 하면 마지막 나머지 위치부터 자르면 된다. 
			// 가장 신경써야할 부분, 나머지 부분이 존재하는 것을 모르면 틀릴 수 밖에 없음 
		}
		
		if(answer > result.size())
			answer = result.size();  
			 
	}
	
	return answer;
}

int main()
{
	string str = "abcabcabcabcdededededede";
	
	solution(str);
	
	return 0;
}

```

## 알아두셔야 할점

이전에 풀었던 문제들을 다시 복습하는 중이기에, 누군가의 블로그에서 참조한
내용일 가능성이 농후합니다.... 제가 푼것도 있겠지만, 다른 누군가의 블로그의 내용과 유사할 수 있스빈다. 혹은 똑같거나
참조를 못달아서 죄송합니다..
