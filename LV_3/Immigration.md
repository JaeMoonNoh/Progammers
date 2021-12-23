# 프로그래머스 입국심사 BinarySearch

[입국심사](https://programmers.co.kr/learn/courses/30/lessons/43238)

## 문제 설명

n명이 입국심사를 위해 줄을 서서 기다리고 있습니다. 각 입국심사대에 있는 심사관마다 심사하는데 걸리는 시간은 다릅니다.

처음에 모든 심사대는 비어있습니다. 한 심사대에서는 동시에 한 명만 심사를 할 수 있습니다. 가장 앞에 서 있는 사람은 비어 있는 심사대로 가서 심사를 받을 수 있습니다. 하지만 더 빨리 끝나는 심사대가 있으면 기다렸다가 그곳으로 가서 심사를 받을 수도 있습니다.

모든 사람이 심사를 받는데 걸리는 시간을 최소로 하고 싶습니다.

입국심사를 기다리는 사람 수 n, 각 심사관이 한 명을 심사하는데 걸리는 시간이 담긴 배열 times가 매개변수로 주어질 때, 모든 사람이 심사를 받는데 걸리는 시간의 최솟값을 return 하도록 solution 함수를 작성해주세요.

## 제한 사항

  * 입국심사를 기다리는 사람은 1명 이상 1,000,000,000명 이하입니다.
  * 각 심사관이 한 명을 심사하는데 걸리는 시간은 1분 이상 1,000,000,000분 이하입니다.
  * 심사관은 1명 이상 100,000명 이하입니다.

## 전체 코드


```c++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

long long answer = 0;
long long mid;

long long BinarySearch(int n, long long start, long long end, vector<int> times)
{
	
	/*
	최대로 시간이 오래 걸려 상담할 수 있는 것은
	times 정렬 후 times[times.size()-1] * n 인원 수 이기 때문에
	end = times[times.size()-1] * n
	start = 1 시작 시간으로 지정해서 문제를 해결 
	
	*/
	
	while(start <= end)
	{
		mid = (start + end) / 2;
		long long person = 0;
		
		for(int i = 0; i < times.size(); i++)
		{
			person += mid / times[i];
		}
		
		if(person < n) // 처리할 수 있는 사람의 수가 원래 사람 수보다 작으면, 숫자 범위를 높여야 한다. 
		{
			start = mid + 1;
		}
		else // person이 더 많다면, end = mid - 1 범위를 줄여주어야 한다. 
		{

			answer = mid; // mid가 end보다 작다. 계속해서 갱신 최솟값을 찾는것 

			end = mid - 1; // end 범위를 낮춰주는 것 	
		}
	}
	return answer;
}

long long solution(int n, vector<int> times)
{
	sort(times.begin(), times.end());
	
	long long start = 1;
	long long end = (long long)times[times.size()-1] * n;
	
	return BinarySearch(n,start,end,times);
}
```
