# 프로그래머스 소수 찾기 LV.2
[소수 찾기](https://programmers.co.kr/learn/courses/30/lessons/42839)

## 문제 설명

한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

## 제한사항

  * numbers는 길이 1 이상 7 이하인 문자열입니다.
  * numbers는 0~9까지 숫자만으로 이루어져 있습니다.
  * "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

## 전체 코드

```c++
//string이 주어졌을 때, int형으로 넣을 수 있는 방법.

string numbers = "17" // 이런 형식으로 주어졌을 때

	for(int i = 0; i < numbers.size(); i++)
		v.push_back(numbers[i] - '0'); // string 값 넣어주는 방법
```

```c++

//중복되는 값 제거해주는 방식

	ans.erase(unique(ans.begin(), ans.end()), ans.end());
```

```c++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

bool Prime_chk(int n){
	if(n < 2)
		return false; // 1
		
	for(int i = 2; i*i <= n; i++){
		if(n % i == 0)
			return false;
	}
	return true;
}

int solution(string numbers){
	int answer = 0;
	
	vector<int> v;
	
	for(int i = 0; i < numbers.size(); i++)
		v.push_back(numbers[i] - '0'); // string 값 넣어주는 방법
	
	sort(v.begin(), v.end()); // 
	vector<int> ans;
	
	do{
		for(int i = 0; i <= v.size(); i++){
			int temp = 0;
			
			for(int j = 0; j < i; j++){
				temp = (temp * 10) + v[j];
				ans.push_back(temp);
			}
		}
		
	}while(next_permutation(v.begin(),v.end()));
	
	
	sort(ans.begin(), ans.end());
	ans.erase(unique(ans.begin(), ans.end()), ans.end());
	
	for(int i = 0; i < ans.size(); i++){
		if(Prime_chk(ans[i]))
			answer += 1;
	}
	
	return answer;
}
```
