# 프로그래머스 표편집 LV.3
[표편집](https://programmers.co.kr/learn/courses/30/lessons/81303)

## 문제 설명

위 그림에서 파란색으로 칠해진 칸은 현재 선택된 행을 나타냅니다. 단, 한 번에 한 행만 선택할 수 있으며, 표의 범위(0행 ~ 마지막 행)를 벗어날 수 없습니다. 이때, 다음과 같은 명령어를 이용하여 표를 편집합니다.

  * `"U X"`: 현재 선택된 행에서 X칸 위에 있는 행을 선택합니다.
  * `"D X"`: 현재 선택된 행에서 X칸 아래에 있는 행을 선택합니다.
  * `"C"` : 현재 선택된 행을 삭제한 후, 바로 아래 행을 선택합니다. 단, 삭제된 행이 가장 마지막 행인 경우 바로 윗 행을 선택합니다.
  * `"Z"` : 가장 최근에 삭제된 행을 원래대로 복구합니다. 단, 현재 선택된 행은 바뀌지 않습니다.

## 제한 사항

  * 5 ≤ n ≤ 1,000,000
  * 0 ≤ k < n
  * 1 ≤ cmd의 원소 개수 ≤ 200,000
    * cmd의 각 원소는 "U X", "D X", "C", "Z" 중 하나입니다.
    * X는 1 이상 300,000 이하인 자연수이며 0으로 시작하지 않습니다.
    * X가 나타내는 자연수에 ',' 는 주어지지 않습니다. 예를 들어 123,456의 경우 123456으로 주어집니다.
    * cmd에 등장하는 모든 X들의 값을 합친 결과가 1,000,000 이하인 경우만 입력으로 주어집니다.
    * 표의 모든 행을 제거하여, 행이 하나도 남지 않는 경우는 입력으로 주어지지 않습니다.
    * 본문에서 각 행이 제거되고 복구되는 과정을 보다 자연스럽게 보이기 위해 "이름" 열을 사용하였으나, "이름"열의 내용이 실제 문제를 푸는 과정에 필요하지는 않습니다. "이름"열에는 서로 다른 이름들이 중복없이 채워져 있다고 가정하고 문제를 해결해 주세요.
  * 표의 범위를 벗어나는 이동은 입력으로 주어지지 않습니다.
  * 원래대로 복구할 행이 없을 때(즉, 삭제된 행이 없을 때) "Z"가 명령어로 주어지는 경우는 없습니다.
  * 정답은 표의 0행부터 n - 1행까지에 해당되는 O, X를 순서대로 이어붙인 문자열 형태로 return 해주세요.

## 전체 코드

```c++
#include<string>
#include<vector>
#include<iostream>
#include<set>
#include<stack>

using namespace std;

set<int> s;

string solution(int n, int k, vector<string> cmd)
{
	string answer = "";
	
	for(int i = 0; i < n; i++)
		s.insert(i);
		
	set<int> :: iterator iter, temp;
	
	iter = s.begin();
	
	while(k--)
		iter++;
		
	stack<int> st;
	
	for(int i = 0; i < cmd.size(); i++)
	{
		string str = cmd[i];
		
		char c = str[0];
		
		if(c == 'C' || c == 'Z')
		{
			if(c == 'C')
			{
				st.push(*iter);
				temp = iter;
				temp++;
				
				if(temp == s.end())
				{
					temp = iter;
					--temp;
				}
				s.erase(iter);
				iter = temp;
			}
			else
			{
				s.insert(st.top());
				st.pop();
			}
		}
		else
		{
			string ss = str.substr(2);
			int num = stoi(ss);
			
			while(num--)
			{
				if(c == 'D')
				{
					iter++;
				}
				else
				{
					iter--;
				}
			}
		}
	}
	
	int idx=0;
	
  for(iter = s.begin();iter!=s.end();iter++)
	{
     int num = *iter;
        
		if(num==idx)
		{
       answer+='O';
    } 
    else
		{
      while(idx<num)
			{
                answer+='X';
                idx++;
            }
            answer+='O';
        }
        idx++;
    }
     
    while(idx < n)
    {
    	answer += 'X';
    	idx++;
	  }
       
    return answer;
}
```
