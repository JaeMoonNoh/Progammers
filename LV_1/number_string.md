# 프로그래머스 숫자 문자열과 영단어 Lv 1

## 문제 설명

네오와 프로도가 숫자놀이를 하고 있습니다. 네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.

다음은 숫자의 일부 자릿수를 영단어로 바꾸는 예시입니다.

  * 1478 → "one4seveneight"
  * 234567 → "23four5six7"
  * 10203 → "1zerotwozero3"

이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 s가 매개변수로 주어집니다. s가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.

참고로 각 숫자에 대응되는 영단어는 다음 표와 같습니다.



```c++
숫자	영단어
0	zero
1	one
2	two
3	three
4	four
5	five
6	six
7	seven
8	eight
9	nine
```

## 제한 사항

  * 1 ≤ s의 길이 ≤ 50
  * s가 "zero" 또는 "0"으로 시작하는 경우는 주어지지 않습니다.
  * return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 s로 주어집니다.

## 전체코드

문제를 보았을때, 어느 부분에서 데이터를 출력해 0~9를 만드는 것이 아니라, 숫자를 직접 map에 저장을 해서 문제를 해결해야한다.
그리고 각 문자를 구성해봤을때, map에 등록되어있는 것들을 숫자 문자로 바꾸어주는 과정을 하고, 숫자는 그대로 유지한다.
map.find() 함수를 사용할줄 알아야 한다

```c++
#include<iostream>
#include<string>
#include<vector>
#include<unordered_map>

using namespace std;

unordered_map<string,string> m;

int solution(string s)
{
	int answer = 0;
	
	m["zero"] = "0";
	m["one"] = "1";
	m["two"] = "2";
	m["three"] = "3";
	m["four"] = "4";
	m["five"] = "5";
	m["six"] = "6";
	m["seven"] = "7";
	m["eight"] = "8";
	m["nine"] = "9";
	
	string str = "";
	string ans_str = "";
	
	for(int i = 0; i <= s.length(); i++)
	{
		if(m.find(str) != m.end())
		{
			ans_str += m[str];
			str = "";
		}
		
		if('0' <= s[i] && s[i] <= '9')
			ans_str += s[i];
		else
			str += s[i];
	}
	
	answer = stoi(ans_str);
	return answer;	
}

```


