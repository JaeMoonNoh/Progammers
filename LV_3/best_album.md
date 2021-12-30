# 프로그래머스 베스트앨범 LV.3

[베스트앨범](https://programmers.co.kr/learn/courses/30/lessons/42579)

## 문제 설명

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

  1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
  2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
  3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

## 제한사항

  * genres[i]는 고유번호가 i인 노래의 장르입니다.
  * plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
  * genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
  * 장르 종류는 100개 미만입니다.
  * 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
  * 모든 장르는 재생된 횟수가 다릅니다.

## 전체코드

```c++
#include<string>
#include<vector>
#include<map>
#include<algorithm>

using namespace std;

bool comp1(const pair<int,int>& a,const pair<int,int>& b)
{
	if(a.second == b.second) // pair<int,int> 비교 
		return a.first < b.first;
	return a.second > b.second;
}

bool comp(const pair<string,int>& a, const pair<string,int>& b)
{
	return a.second > b.second; // pair<string,int> 비교 
}

vector<int> solution(vector<string> genres, vector<int> plays)
{
	vector<int> answer;
	
	map<string,int> m;
	map<string,vector<pair<int,int> > > m1;
	
	for(int i = 0; i < genres.size(); i++)
	{
		m[genres[i]] += plays[i];
		m1[genres[i]].push_back({i,plays[i]});
	}
	
	for(auto &c : m1)
	{
		sort(c.second.begin(),c.second.end(),comp1); //  장르에 맞춰서 몇번이 더 많이 재생 됐는지 
	}
	
	vector<pair<string,int> > vec(m.begin(), m.end()); // map내부를 vec내부로 옮기는 방법 
	sort(vec.begin(), vec.end(), comp); // 음악 장르중 가장 많이 재생 된 것 
	
	for(int i = 0; i < vec.size(); i++)
	{
		string genre_name = vec[i].first; // 장르의 이름 
		for(int j = 0; (j < m1[genre_name].size()) && (j < 2); j++) // 장르가 재생된 수만큼 진행이 되지만 두개만 뽑힌다. 
		{
			answer.push_back(m1[genre_name][j].first);
		}
	}
	
	return answer;
	
}
```
