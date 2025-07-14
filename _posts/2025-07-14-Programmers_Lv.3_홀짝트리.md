---
layout: post
title: Programmers Lv.3 홀짝트리
date: 2025-07-14 09:00:00 +0900
category: Training
---

## Programmers Lv.3 홀짝트리

1. [접근](#1-접근)
2. [풀이](#2-풀이)
3. [코드](#3-코드)


---

<br><br>

[](https://school.programmers.co.kr/learn/courses/30/lessons/388354)

<br>

---

<br><br>

>### 1. 접근

- 노드별로 연결된 노드 정보를 수집
    - 연결된 노드가 없는 독립적인 노드의 경우, 스스로 홀짝/역홀짝 트리를 구성한다.

- 노드 연결 정보를 통해 각각의 트리에 대해 처리

- 트리의 노드가 홀짝/역홀짝인지를 판별하여, 트리가 홀짝/역홀짝 트리인지를 판단
    - 트리에 속하는 노드 중 홀짝/역홀짝 노드가 1개 뿐인 경우, 그 트리는 홀짝/역홀짝 트리가 된다.
    - 하나의 트리는 루트 노드가 누구냐에 따라 홀짝 트리, 역홀짝 트리가 동시에 될 수 있다.

<br>

---

<br><br>

>### 2. 풀이

> 1. 노드 연결 정보 수집

해시맵을 통해 각 노드별로 어떤 노드와 연결되었는지를 기록한다.

```cpp

// 노드별 연결된 노드 정보
unordered_map<int, vector<int>> NodeInfo;
for (int i = 0; i < edges.size(); ++i)
{
    NodeInfo[edges[i][0]].push_back(edges[i][1]);
    NodeInfo[edges[i][1]].push_back(edges[i][0]);
}

```

<br>

> 2. 트리별로 노드를 분석, 트리의 홀짝/역홀짝 판별

노드의 중복 방문 방지를 위해, 해시셋으로 방문했던 노드를 기록해 둔다.

임의의 첫 노드부터 시작하여, 큐에 연결된 노드를 push 하여 트리를 순회한다.

트리를 순회하며, 각 노드의 숫자와 연결된 간선 수를 통해 홀짝/역홀짝 노드를 구분한다. 이때, 부모-자식 여부를 무시한다.

순회가 끝나면, 해당 트리에 속한 노드에 몇개의 홀짝/역홀짝 노드가 있는지 알 수 있다.

```cpp

// 홀짝/역홀짝 노드 개수를 기록
vector<int> Type = { 0, 0 };

// 방문했던 노드는 패스
if (Visited.find(nodes[i]) != Visited.end())
    continue;

queue<int> q;
q.push(nodes[i]);

while (!q.empty())
{
    int curNode = q.front();
    q.pop();
    Visited.insert(curNode);

    auto iter = NodeInfo.find(curNode);
    if (iter == NodeInfo.end())
    {
        // 간선이 없는 노드의 경우
        ++Type[curNode % 2];
    }
    else
    {
        for (int elem : iter->second)
        {
            if (Visited.find(elem) == Visited.end())
                q.push(elem);
        }
        ++Type[(curNode % 2 != iter->second.size() % 2)];
    }
}

```

<br>

만약 홀짝/역홀짝 노드가 1개라면, 해당 노드를 트리의 루트 노드로 지정할 경우 나머지 노드들은 (자식 수) = (연결된 간선 수) - 1 이 되므로, 홀짝 노드가 역홀짝, 역홀짝 노드가 홀짝 노드가 된다.

따라서, 해당 트리는 홀짝/역홀짝 노드가 1개라면, 홀짝/역홀짝 트리가 된다.

만약, 홀짝 노드 1개와 역홀짝 노드 1개로 구성된 트리라면, 홀짝 트리가 될 수도, 역홀짝 트리가 될 수도 있다.

```cpp

// 트리에 홀짝/역홀짝 노드가 1개인 경우, 홀짝/역홀짝 트리가 될 수 있음
if (Type[0] == 1)
    ++answer[0];
if (Type[1] == 1)
    ++answer[1];

```

<br>

---

<br><br>

>### 3. 코드

```cpp

#include <string>
#include <vector>
#include <unordered_map>
#include <unordered_set>
#include <queue>

using namespace std;

vector<int> solution(vector<int> nodes, vector<vector<int>> edges) {
    // 노드별 연결된 노드 정보
    unordered_map<int, vector<int>> NodeInfo;
    for (int i = 0; i < edges.size(); ++i)
    {
        NodeInfo[edges[i][0]].push_back(edges[i][1]);
        NodeInfo[edges[i][1]].push_back(edges[i][0]);
    }

    // 간선 수가 0이면 홀짝/역홀짝 트리가 될 수 있음
    // 트리에 속하는 노드 중 하나만 홀짝/역홀짝 노드인 경우, 홀짝/역홀짝 트리가 될 수 있음
    unordered_set<int> Visited;
    vector<int> answer = { 0, 0 };
    for (int i = 0; i < nodes.size(); ++i)
    {
        // 홀짝/역홀짝 노드 개수를 기록
        vector<int> Type = { 0, 0 };

        // 방문했던 노드는 패스
        if (Visited.find(nodes[i]) != Visited.end())
            continue;

        queue<int> q;
        q.push(nodes[i]);

        while (!q.empty())
        {
            int curNode = q.front();
            q.pop();
            Visited.insert(curNode);
            auto iter = NodeInfo.find(curNode);

            if (iter == NodeInfo.end())
            {
                // 간선이 없는 노드의 경우
                ++Type[curNode % 2];
            }
            else
            {
                for (int elem : iter->second)
                {
                    if (Visited.find(elem) == Visited.end())
                        q.push(elem);
                }

                ++Type[(curNode % 2 != iter->second.size() % 2)];
            }
        }

        // 트리에 홀짝/역홀짝 노드가 1개인 경우, 홀짝/역홀짝 트리가 될 수 있음
        if (Type[0] == 1)
            ++answer[0];
        if (Type[1] == 1)
            ++answer[1];
    }

    return answer;
}

```

<br>

---