---
layout: post
title: Programmers Lv.2 완전범죄
date: 2025-06-15 09:00:00 +0900
category: Training
---

## Programmers Lv.2 완전범죄

1. [접근](#1-접근)
2. [풀이](#2-풀이)
3. [코드](#3-코드)


---

<br><br>

[](https://school.programmers.co.kr/learn/courses/30/lessons/389480)

<br>

---

<br><br>

>### 1. 접근

- 동적 계획법을 응용, B의 흔적이 i 일 때 A 흔적의 최소치를 갱신해 나간다.

<br>

---

<br><br>

>### 2. 풀이

아래의 테스트 케이스를 예시로 들어 보자:

|info                       |n      |m      |result |
|---                        |---    |---    |---    |
|[[1, 2], [2, 3], [2, 1]]   |4      |4      |2      |

<br>

아무것도 훔치지 않은 경우, B 흔적이 0일 때 A 흔적이 0 인 경우만 존재한다.

|0  |1  |2  |3  |
|---|---|---|---|
|0  |Inf|Inf|Inf|

<br>

이후 [1, 2], [2, 3], [2, 1] 의 물건에 대해 차례대로 훔치기를 수행한다.
이 때, A가 훔칠 경우와 B가 훔칠 경우로 나누어 진행한다.

원래 B 흔적이 0인 경우에 대해서 [1, 2]를 훔칠 때, A가 훔쳤다면 A 흔적이 0 -> 1 이 되고 B가 훔쳤다면 A 흔적은 0, B 흔적은 0 -> 2 가 되므로 표가 아래와 같이 작성된다:

|0                                  |1  |2                                  |3  |
|---                                |---|---                                |---|
|0                                  |Inf|Inf                                |Inf|
|<span style="color:red"> 1 </span> |Inf|<span style="color:red"> 0 </span> |Inf|

다른 경우가 없으므로, 다음 줄의 갱신을 시작한다.

<br>

원래 B 흔적이 0인 경우에 대해서 [2, 3]를 훔칠 때, A가 훔쳤다면 A 흔적이 1 -> 3 이 되고 B가 훔쳤다면 A 흔적은 1, B 흔적은 0 -> 3 가 되므로 표가 아래와 같이 작성된다:

|0                                  |1  |2  |3                                  |
|---                                |---|---|---                                |
|0                                  |Inf|Inf|Inf                                |
|1                                  |Inf|0  |Inf                                |
|<span style="color:red"> 3 </span> |Inf|Inf|<span style="color:red"> 1 </span> |

원래 B 흔적이 2인 경우에 대해서 [2, 3]를 훔칠 때, A가 훔쳤다면 A 흔적이 0 -> 2 이 되고 B는 훔칠 경우 흔적이 4가 넘어가므로 불가능하다.

|0  |1  |2                                  |3  |
|---|---|---                                |---|
|0  |Inf|Inf                                |Inf|
|1  |Inf|0                                  |Inf|
|3  |Inf|<span style="color:red"> 2 </span> |1  |

<br>

같은 규칙으로 계속 표를 작성하면 아래 표와 같이 된다:

|0  |1  |2  |3  |
|---|---|---|---|
|0  |Inf|Inf|Inf|
|1  |Inf|0  |Inf|
|3  |Inf|2  |1  |
|Inf|3  |Inf|2  |

B의 흔적이 1일 때 A 흔적의 최소값은 3, B의 흔적이 3일 때 A 흔적의 최소값은 2이며, 모든 경우의 수에서 A 흔적 최소값은 2가 된다.

만약, 모든 최소값이 Inf 인 경우 n, m 의 제한조건에 부합하는 최소값이 없는 것이므로 -1 을 반환하면 된다.


<br>

---

<br><br>

>### 3. 코드

```cpp

#include <string>
#include <vector>
#include <climits>
#include <algorithm>

using namespace std;

int solution(vector<vector<int>> info, int n, int m) {
    int answer = 0;
    vector<int> dp(m, INT_MAX);
    // 아무것도 훔치지 않은 경우 (0,0)
    dp[0] = 0;
    // 앞에서부터 하나씩 훔치며 최소값으로 갱신
    for (const auto& pair : info)
    {
        // 현재 dp 초기값은 최대값으로 설정
        vector<int> dp_new(m, INT_MAX);
        for (int i = 0; i < m; ++i)
        {
            // 기존 dp값이 Inf 인 경우 패스
            if (dp[i] == INT_MAX)
                continue;
            
            // A가 훔친 경우: 기존 dp 값에 흔적값을 추가한 후 현재 dp값과 비교해서 갱신
            if (pair[0] + dp[i] < n)
            {
                dp_new[i] = min(pair[0] + dp[i], dp_new[i]);
            }
            
            // B가 훔친 경우: 기존 B 흔적에 현재 흔적을 더한 dp의 값을 기존 dp 값으로 비교 후 갱신
            if (pair[1] + i < m)
            {
                dp_new[pair[1] + i] = min(dp_new[pair[1] + i], dp[i]);
            }
        }
        // 기존 dp를 새로 작성된 dp로 갱신
        dp = move(dp_new);
    }
    
    // 최종 dp 에 저장된 각 B 흔적값에서의 A 흔적값 최소 중 최종 최소값을 반환
    int min = *min_element(dp.begin(), dp.end());
    if (min == INT_MAX)
        return -1;
    else
        return min;
}

```

<br>

---