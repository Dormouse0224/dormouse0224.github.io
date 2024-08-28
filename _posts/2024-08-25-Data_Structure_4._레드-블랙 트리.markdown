---
layout: post
title: Data Structure 4. 레드-블랙 트리
date: 2024-08-25 18:00:00 +0900
category: Data_Structure
---

## 4. 레드-블랙 트리

1. [정의](#1-정의)
2. [구현]

---

<br><br>

>### 1. 정의

레드-블랙 트리는 자가 균형 이진 탐색 트리(Self-balancing binary search tree)의 종류 중 하나로, C++ 에서 제공되는 STL의 map이 이 알고리즘을 이용해 구현되어 있다.

일반적인 트리와 달리 트리 전체적인 균형을 삽입 및 삭제시마다 검사하여 맞춰주기 때문에 최악의 경우일 때에도 소요 시간이 연결형 리스트처럼 되지 않는다.

<br>

레드-블랙 트리는 다음과 같은 조건을 따른다:

1. 노드는 레드 또는 블랙의 색상을 갖는다.
2. 루트 노드는 블랙이다.
3. 모든 리프 노드(NIL)는 블랙이다.
4. 레드 노드의 자식은 모두 블랙 노드이다. (즉, 레드 노드는 연속적으로 나타날 수 없고, 블랙 노드만이 레드 노드의 부모가 될 수 있다.)
5. 루트 노드로부터 시작하는 리프노드 까지의 모든 경로에는 리프 노드를 제외하면 모두 같은 블랙 노드가 존재한다.


<br>

3번 조건의 경우, NIL 노드는 실제 데이터를 들고 있는 노드가 아닌 개념상으로만 존재하는 **"더미 노드"**이다. 따라서 실제 레드-블랙 트리를 구현할 때에도 반드시 구현할 필요는 없다.

삽입 및 삭제 자체는 기존의 트리와 완전히 같다.

다만, 각각 케이스에 따라 트리의 회전 및 재배열이 이루어지기 때문에 이를 관리할 알고리즘이 새로 필요하게 된다.

<br>

삽입 정렬 알고리즘:

0. 타겟을 삽입 노드로 설정

1. 루트 노드인 경우 - black 노드로 만든 후 알고리즘 종료
2. 루트 노드의 자식인 경우 - 처리할 게 없음. 알고리즘 종료
3. 조부모 노드가 있는 경우 - P(부모), G(조부모), U(삼촌) 설정 - 삼촌 노드는 없는 경우 black 더미 노드로 설정
    1. 부모 노드와 삼촌 노드가 모두 red 인 경우
        - 조부모를 red, 부모와 삼촌을 black 으로 만들고 타겟을 조부모로 변경, 알고리즘 재호출
    2. 부모 노드는 red, 삼촌 노드는 black 이고 자신이 안쪽 노드(비교값이 부모, 조부모 사이에 존재)인 경우
        - 부모와 타겟을 조부모로부터의 방향과 동일한 방향으로 회전.
    3. 부모 노드는 red, 삼촌 노드는 black 이고 자신이 바깥쪽 노드인 경우
        - 부모와 조부모를 삼촌 방향으로 회전.

<br>

삭제 정렬 알고리즘:

1. 리프 레드 삭제 - 변화 없음
2. 리프 블랙 삭제 - 더블 블랙 더미노드 남김 - 더블 블랙 알고리즘으로
3. 중간 레드 삭제 - 자식 블랙이 대체함
4. 중간 블랙 삭제 - 자식 레드가 블랙이 되며 대체함

- 더블 블랙:
    1. 형제가 레드인 경우 - 부모-형제 회전, 이후 서로 색 교환 - 알고리즘 다시 시작
    2. 형제가 블랙이고 조카가 모두 블랙인 경우 - 블랙을 부모로 옮김 - 알고리즘 다시 시작
    3. 안쪽 조카가 레드인 경우 - 레드 조카와 블랙 형제를 회전, 색 교환 - 알고리즘 다시 시작
    4. 바깥 조카가 레드인 경우 - 형제-부모 회전, 색교환, 바깥 조카를 블랙으로

---

<br><br>

>### 2. 구현

<br>

<details>
<summary>코드 접기/펼치기</summary>
<div markdown="1">

```cpp

#pragma once
#include "red_black.h"
#include <string>

template<typename T1, typename T2>
class red_black;

template<typename T1, typename T2>
struct pair {
	friend red_black<T1, T2>;
private:
	T1 first;
	T2 second;


public:
	pair()
		: first()
		, second()
	{}

	pair(T1&& _first, T2&& _second)
		: first(_first)
		, second(_second)
	{}
	pair(pair* _other)
		: first(_other->first)
		, second(_other->second)
	{}
	~pair()
	{
	}
};

enum class connection {
	parent,
	lchild,
	rchild,
	null,
};

enum class color {
	red,
	black,
	doubleblack,
	null,
};

template<typename T1, typename T2>
struct node {
	friend red_black<T1, T2>;
private:
	pair<T1, T2>* pData;
	color nodeColor;
	node* pNodeConnection[3];

public:
	node()
		: pData()
		, nodeColor(color::red)
		, pNodeConnection{ nullptr, nullptr, nullptr }
	{}
	
	node(pair<T1, T2>* _pData, node* _pParent = nullptr, node* _lchild = nullptr, node* rchild = nullptr)
		: pData(_pData)
		, nodeColor(color::red)
		, pNodeConnection{ _pParent , _lchild , rchild }
	{}

	~node()
	{
		delete pData;
	}
};

template<typename T1, typename T2>
class red_black {
private:
	node<T1, T2>* pRoot;
	int dataCount;

public:
	class iterator;
	void insert(pair<T1, T2>* _pair);
	iterator erase(iterator _target);
	iterator find(T1 _firstData);


private:
	connection compareFirst(T1 _a, T1 _b);
	void insert_sort(node<T1, T2>* pNew);
	bool isInnerNode(node<T1, T2>* _target);
	void delete_sort(node<T1, T2>* pDelete);




private:
	class iterator {
		friend red_black;
	private:
		node<T1, T2>* pTarget;
		red_black* pOwner;

	public:
		iterator& operator ++();
		iterator& operator --();
		bool operator ==(const iterator _other) const;
		bool operator !=(const iterator _other) const;
		pair<T1, T2>* operator *();
		pair<T1, T2> operator ->();

		bool isLchild();
		bool isRchild();

	public:
		iterator()
			: pTarget(nullptr)
			, pOwner(nullptr)
		{}

		iterator(node<T1, T2>* _pTarget, red_black* _pOwner)
			: pTarget(_pTarget)
			, pOwner(_pOwner)
		{}
	};



public:
	red_black()
		: pRoot()
		, dataCount(0)
	{}
};


template<typename T1, typename T2>
typename red_black<T1, T2>::iterator& red_black<T1, T2>::iterator::operator ++()
{
	//중위 순회
	//1. 오른쪽 자식이 있으면 이동 후 왼쪽 자식 끝까지 이동, 종료.
	if (pTarget->pNodeConnection[(int)connection::rchild] != nullptr)
	{
		pTarget = pTarget->pNodeConnection[(int)connection::rchild];
		while (pTarget->pNodeConnection[(int)connection::lchild] != nullptr)
		{
			pTarget = pTarget->pNodeConnection[(int)connection::lchild];
		}
		return *this;
	}
	//2. 없으면 자신이 왼쪽 자식일 때까지 부모로 이동 후 그 부모로 이동, 종료.
	else
	{
		while (pTarget->pNodeConnection[(int)connection::parent] != nullptr && pTarget->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::lchild] != pTarget)
		{
			pTarget = pTarget->pNodeConnection[(int)connection::parent];
		}
		//3. 중간에 루트 노드에 도달하면 end iterator 로 이동
		if (pTarget->pNodeConnection[(int)connection::parent] != nullptr)
		{
			pTarget = pTarget->pNodeConnection[(int)connection::parent];
		}
		else
		{
			pTarget = nullptr;
		}
		return *this;
	}
}

template<typename T1, typename T2>
typename red_black<T1, T2>::iterator& red_black<T1, T2>::iterator::operator --()
{
	//중위 순회
	//1. 왼쪽 자식이 있으면 이동 후 오른쪽 자식 끝까지 이동, 종료.
	if (pTarget->pNodeConnection[(int)connection::lchild] != nullptr)
	{
		pTarget = pTarget->pNodeConnection[(int)connection::lchild];
		while (pTarget->pNodeConnection[(int)connection::rchild] != nullptr)
		{
			pTarget = pTarget->pNodeConnection[(int)connection::rchild];
		}
		return *this;
	}
	//2. 없으면 자신이 오른쪽 자식일 때까지 부모로 이동 후 그 부모로 이동, 종료.
	else
	{
		while (pTarget->pNodeConnection[(int)connection::parent] != nullptr && pTarget->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::rchild] != pTarget)
		{
			pTarget = pTarget->pNodeConnection[(int)connection::parent];
		}
		//3. 중간에 루트 노드에 도달하면 end iterator 로 이동
		if (pTarget->pNodeConnection[(int)connection::parent] != nullptr)
		{
			pTarget = pTarget->pNodeConnection[(int)connection::parent];
		}
		else
		{
			pTarget = nullptr;
		}
		return *this;
	}
}

template<typename T1, typename T2>
bool red_black<T1, T2>::iterator::operator ==(const iterator _other) const
{
	if (_other->pOwner == pOwner && _other->pTarget == pTarget)
		return true;
	else
		return false;
}

template<typename T1, typename T2>
bool red_black<T1, T2>::iterator::operator !=(const iterator _other) const
{
	return !(*this == _other);
}

template<typename T1, typename T2>
pair<T1, T2>* red_black<T1, T2>::iterator::operator *()
{
	return pTarget->pData;
}

template<typename T1, typename T2>
pair<T1, T2> red_black<T1, T2>::iterator::operator ->()
{
	return pTarget->pData;
}

template<typename T1, typename T2>
bool red_black<T1, T2>::iterator::isLchild()
{
	return (pTarget->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::lchild] == pTarget);
}

template<typename T1, typename T2>
bool red_black<T1, T2>::iterator::isRchild()
{
	return (pTarget->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::rchild] == pTarget);
}

template<typename T1, typename T2>
void red_black<T1, T2>::insert(pair<T1, T2>* _pair)
{
	// 첫 데이터인 경우
	if (pRoot == nullptr)
	{
		node<T1, T2>* newNode = new node<T1, T2>(_pair);
		pRoot = newNode;
		newNode->nodeColor = color::black;
		++dataCount;
		return;
	}

	node<T1, T2>* current = pRoot;
	//비교 후 이동 방향 결정
	connection direction = compareFirst(current->pData->first, _pair->first);
	//같은 값을 만날 때까지 반복
	while (direction != connection::null)
	{
		//이동 방향이 비어있지 않다면 이동 후 방향 재설정
		if (current->pNodeConnection[(int)direction] != nullptr)
		{
			current = current->pNodeConnection[(int)direction];
			direction = compareFirst(current->pData->first, _pair->first);
		}
		//비어 있었다면 노드 삽입 후 종료
		else
		{
			node<T1, T2>* newNode = new node<T1, T2>(_pair, current);
			current->pNodeConnection[(int)direction] = newNode;


			++dataCount;
			insert_sort(newNode);
			return;
		}
	}
	//같은 값을 만난 경우 그냥 종료
	return;
}

template<typename T1, typename T2>
typename red_black<T1, T2>::iterator red_black<T1, T2>::erase(iterator _target)
{
	if (!_target.pTarget)
		return iterator(nullptr, _target.pOwner);

	// 1. 리프 노드 삭제 : 노드 삭제 후 연결 관리 정리
	// 2. 중간 노드 삭제 - 자식이 2개인 경우 : 중위 후속자 또는 선행자가 자리를 대체
	// 3. 중간 노드 삭제 - 자식이 1개인 경우 : 자식이 자리를 대체함
	// 4. 루트 노드 삭제 :  위 규칙을 따르되 객체 루트노드 포인터 변수의 갱신 필요
	//
	node<T1, T2>* pTargetNode = _target.pTarget;
	bool isRoot = (pTargetNode == pRoot);
	iterator nextIter = _target;
	++nextIter;
	node<T1, T2>* pNext = nextIter.pTarget;
	// 1. 리프 노드 삭제 : 노드 삭제, 연결 관계 정리
	if (pTargetNode->pNodeConnection[(int)connection::lchild] == nullptr && pTargetNode->pNodeConnection[(int)connection::rchild] == nullptr)
	{
		delete_sort(pTargetNode);
		// 연결 관계 정리
		if (!isRoot)
		{
			if (_target.isLchild())
			{
				pTargetNode->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::lchild] = nullptr;
			}
			else if (_target.isRchild())
			{
				pTargetNode->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::rchild] = nullptr;
			}
		}
		else
		{
			pRoot = nullptr;
		}
		// 노드 삭제
		delete pTargetNode;
		pTargetNode = nullptr;
		--dataCount;
		return nextIter;
	}
	// 2. 중간 노드 삭제 - 자식이 2개인 경우 : 중위 후속자 또는 선행자가 자리를 대체
	else if (pTargetNode->pNodeConnection[(int)connection::lchild] != nullptr && pTargetNode->pNodeConnection[(int)connection::rchild] != nullptr)
	{
		delete_sort(pNext);
		// 후속자 데이터로 덮어쓰기
		delete pTargetNode->pData;
		pair<T1, T2>* temp = new pair<T1, T2>(pNext->pData);
		pTargetNode->pData = temp;
		// 후속자 삭제
		// 리프노드가 삭제되는경우
		if (pNext->pNodeConnection[(int)connection::lchild] == nullptr && pNext->pNodeConnection[(int)connection::rchild] == nullptr)
		{
			if (nextIter.isLchild())
			{
				pNext->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::lchild] = nullptr;
			}
			else if (nextIter.isRchild())
			{
				pNext->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::rchild] = nullptr;
			}
			delete nextIter.pTarget;
			nextIter.pTarget = nullptr;
		}
		// 자식노드 1개를 보유한 노드가 삭제되는 경우
		else
		{
			if (nextIter.isLchild())
			{
				if (pNext->pNodeConnection[(int)connection::lchild] != nullptr)
				{
					pNext->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::lchild] = pNext->pNodeConnection[(int)connection::lchild];
					pNext->pNodeConnection[(int)connection::lchild]->pNodeConnection[(int)connection::parent] = pNext->pNodeConnection[(int)connection::parent];
				}
				else
				{
					pNext->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::lchild] = pNext->pNodeConnection[(int)connection::rchild];
					pNext->pNodeConnection[(int)connection::rchild]->pNodeConnection[(int)connection::parent] = pNext->pNodeConnection[(int)connection::parent];
				}
			}
			else if (nextIter.isRchild())
			{
				if (pNext->pNodeConnection[(int)connection::lchild] != nullptr)
				{
					pNext->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::rchild] = pNext->pNodeConnection[(int)connection::lchild];
					pNext->pNodeConnection[(int)connection::lchild]->pNodeConnection[(int)connection::parent] = pNext->pNodeConnection[(int)connection::parent];
				}
				else
				{
					pNext->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::rchild] = pNext->pNodeConnection[(int)connection::rchild];
					pNext->pNodeConnection[(int)connection::rchild]->pNodeConnection[(int)connection::parent] = pNext->pNodeConnection[(int)connection::parent];
				}
			}
		}
		--dataCount;
		return _target;
	}
	// 3. 중간 노드 삭제 - 자식이 1개인 경우 : 자식이 자리를 대체함
	else
	{
		if (isRoot)
		{
			delete_sort(pTargetNode);
			if (pTargetNode->pNodeConnection[(int)connection::lchild] != nullptr)
			{
				pTargetNode->pNodeConnection[(int)connection::lchild]->pNodeConnection[(int)connection::parent] = nullptr;
				pRoot = pTargetNode->pNodeConnection[(int)connection::lchild];
			}
			else
			{
				pTargetNode->pNodeConnection[(int)connection::rchild]->pNodeConnection[(int)connection::parent] = nullptr;
				pRoot = pTargetNode->pNodeConnection[(int)connection::rchild];
			}
		}
		else
		{
			delete_sort(pTargetNode);
			if (_target.isLchild())
			{
				if (pTargetNode->pNodeConnection[(int)connection::lchild] != nullptr)
				{
					pTargetNode->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::lchild] = pTargetNode->pNodeConnection[(int)connection::lchild];
					pTargetNode->pNodeConnection[(int)connection::lchild]->pNodeConnection[(int)connection::parent] = pTargetNode->pNodeConnection[(int)connection::parent];
				}
				else
				{
					pTargetNode->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::lchild] = pTargetNode->pNodeConnection[(int)connection::rchild];
					pTargetNode->pNodeConnection[(int)connection::rchild]->pNodeConnection[(int)connection::parent] = pTargetNode->pNodeConnection[(int)connection::parent];
				}
			}
			else if (_target.isRchild())
			{
				if (pTargetNode->pNodeConnection[(int)connection::lchild] != nullptr)
				{
					pTargetNode->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::rchild] = pTargetNode->pNodeConnection[(int)connection::lchild];
					pTargetNode->pNodeConnection[(int)connection::lchild]->pNodeConnection[(int)connection::parent] = pTargetNode->pNodeConnection[(int)connection::parent];
				}
				else
				{
					pTargetNode->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::rchild] = pTargetNode->pNodeConnection[(int)connection::rchild];
					pTargetNode->pNodeConnection[(int)connection::rchild]->pNodeConnection[(int)connection::parent] = pTargetNode->pNodeConnection[(int)connection::parent];
				}
			}
		}
		--dataCount;
		return nextIter;
	}
}

template<typename T1, typename T2>
typename red_black<T1, T2>::iterator red_black<T1, T2>::find(T1 _firstData)
{
	node<T1, T2>* current = pRoot;
	//비교 후 이동 방향 결정
	connection direction = compareFirst(current->pData->first, _firstData);
	//같은 값을 만날 때까지 반복
	while (direction != connection::null)
	{
		//이동 방향이 비어있지 않다면 이동 후 방향 재설정
		if (current->pNodeConnection[(int)direction] != nullptr)
		{
			current = current->pNodeConnection[(int)direction];
			direction = compareFirst(current->pData->first, _firstData);
		}
		//비어 있었다면 end iterator 반환
		else
		{
			return iterator(nullptr, this);
		}
	}
	//같은 값을 만난 경우 해당 iterator 반환
	return iterator(current, this);
}

template<typename T1, typename T2>
connection red_black<T1, T2>::compareFirst(T1 _a, T1 _b)
{
	connection direction = connection::null;

	if (_a < _b)
	{
		direction = connection::rchild;
	}
	else if (_a > _b)
	{
		direction = connection::lchild;
	}
	else
	{
		direction = connection::null;
	}
	return direction;
}

template<typename T1, typename T2>
inline void red_black<T1, T2>::insert_sort(node<T1, T2>* _target)
{
	// 1. 루트 노드는 블랙
	// 2. 레드 노드의 자식은 항상 블랙
	// 3. 블랙 노드 깊이는 항상 같다
	//
	node<T1, T2>* pNow = _target;
	node<T1, T2>* pParent = nullptr;
	node<T1, T2>* pGrand = nullptr;
	node<T1, T2>* pUncle = nullptr;

	node<T1, T2>* dummy = new node<T1, T2>;

	while (true)
	{
		// 루트 노드인 경우 - black 노드로 만든 후 함수 종료
		if (pNow->pNodeConnection[(int)connection::parent] == nullptr)
		{
			pNow->nodeColor = color::black;
			return;
		}
		// 조부모 노드가 있는 경우 - P, G, U 세팅
		else if (pNow->pNodeConnection[(int)connection::parent]->pNodeConnection[(int)connection::parent] != nullptr)
		{
			pParent = pNow->pNodeConnection[(int)connection::parent];
			pGrand = pParent->pNodeConnection[(int)connection::parent];
			// 삼촌 노드가 있으면 입력함
			if (pGrand->pNodeConnection[(int)connection::lchild] != nullptr && pGrand->pNodeConnection[(int)connection::rchild] != nullptr)
			{
				if (pParent == pGrand->pNodeConnection[(int)connection::lchild])
				{
					pUncle = pGrand->pNodeConnection[(int)connection::rchild];
				}
				else
				{
					pUncle = pGrand->pNodeConnection[(int)connection::lchild];
				}
			}
			// 없으면 black 더미 노드로 설정
			else
			{
				dummy->nodeColor = color::black;
				pUncle = dummy;
			}
		}
		// 루트 노드의 자식인 경우
		else
		{
			return;
		}


		// 부모 노드와 삼촌 노드가 모두 red 인 경우
		if (pParent->nodeColor == color::red && pUncle->nodeColor == color::red)
		{
			pGrand->nodeColor = color::red;
			pParent->nodeColor = color::black;
			pUncle->nodeColor = color::black;
			pNow = pGrand;
			continue;
		}

		
		// 부모 노드는 red, 삼촌 노드는 black 이고 자신이 안쪽 노드(비교값이 부모, 조부모 사이에 존재)인 경우
		if (pParent->nodeColor == color::red && pUncle->nodeColor == color::black && isInnerNode(pNow))
		{
			if (pNow == pParent->pNodeConnection[(int)connection::rchild])
			{
				node<T1, T2>* nowLC = pNow->pNodeConnection[(int)connection::lchild];
				pNow->pNodeConnection[(int)connection::parent] = pGrand;
				pGrand->pNodeConnection[(int)connection::lchild] = pNow;
				pNow->pNodeConnection[(int)connection::lchild] = pParent;
				pParent->pNodeConnection[(int)connection::parent] = pNow;
				if (nowLC != nullptr)
					nowLC->pNodeConnection[(int)connection::parent] = pParent;
				pParent->pNodeConnection[(int)connection::rchild] = nowLC;
			}
			else if (pNow == pParent->pNodeConnection[(int)connection::rchild])
			{
				node<T1, T2>* nowRC = pNow->pNodeConnection[(int)connection::rchild];
				pNow->pNodeConnection[(int)connection::parent] = pGrand;
				pGrand->pNodeConnection[(int)connection::rchild] = pNow;
				pNow->pNodeConnection[(int)connection::rchild] = pParent;
				pParent->pNodeConnection[(int)connection::parent] = pNow;
				if (nowRC != nullptr)
					nowRC->pNodeConnection[(int)connection::parent] = pParent;
				pParent->pNodeConnection[(int)connection::lchild] = nowRC;
			}

			pNow = pParent;
			continue;
		}


		// 부모 노드는 red, 삼촌 노드는 black 이고 자신이 바깥쪽 노드인 경우
		if (pParent->nodeColor == color::red && pUncle->nodeColor == color::black && !isInnerNode(pNow))
		{
			node<T1, T2>* grand_grand = nullptr;
			if (pRoot != pGrand)
			{
				grand_grand = pGrand->pNodeConnection[(int)connection::parent];
			}

			if (pParent == pGrand->pNodeConnection[(int)connection::lchild])
			{
				node<T1, T2>* parentRC = pParent->pNodeConnection[(int)connection::rchild];
				pParent->pNodeConnection[(int)connection::rchild] = pGrand;
				pGrand->pNodeConnection[(int)connection::parent] = pParent;
				if (parentRC != nullptr)
					parentRC->pNodeConnection[(int)connection::parent] = pGrand;
				pGrand->pNodeConnection[(int)connection::lchild] = parentRC;
			}
			else if (pParent == pGrand->pNodeConnection[(int)connection::rchild])
			{
				node<T1, T2>* parentLC = pParent->pNodeConnection[(int)connection::lchild];
				pParent->pNodeConnection[(int)connection::lchild] = pGrand;
				pGrand->pNodeConnection[(int)connection::parent] = pParent;
				if (parentLC != nullptr)
					parentLC->pNodeConnection[(int)connection::parent] = pGrand;
				pGrand->pNodeConnection[(int)connection::rchild] = parentLC;
			}

			pParent->pNodeConnection[(int)connection::parent] = grand_grand;
			if (grand_grand != nullptr)
			{
				if (grand_grand->pNodeConnection[(int)connection::lchild] == pGrand)
				{
					grand_grand->pNodeConnection[(int)connection::lchild] = pParent;
				}
				else
				{
					grand_grand->pNodeConnection[(int)connection::rchild] = pParent;
				}
			}

			pParent->nodeColor = color::black;
			pGrand->nodeColor = color::red;
			if (pRoot == pGrand)
				pRoot = pParent;
			continue;
		}

		if (pNow->nodeColor != pParent->nodeColor)
		{
			break;
		}
	}

	delete dummy;
	return;
}

template<typename T1, typename T2>
bool red_black<T1, T2>::isInnerNode(node<T1, T2>* _target)
{
	node<T1, T2>* pParent = _target->pNodeConnection[(int)connection::parent];
	node<T1, T2>* pGrand = _target->pNodeConnection[(int)connection::parent];
	if ((pParent->pNodeConnection[(int)connection::lchild] == _target && pGrand->pNodeConnection[(int)connection::rchild] == pParent)
		|| (pParent->pNodeConnection[(int)connection::rchild] == _target && pGrand->pNodeConnection[(int)connection::lchild] == pParent))
	{
		return true;
	}
	else
	{
		return false;
	}
}

template<typename T1, typename T2>
void red_black<T1, T2>::delete_sort(node<T1, T2>* pDelete)
{
	// 1. 리프 레드 삭제 - 변화 없음
	// 2. 리프 블랙 삭제 - 더블 블랙 더미노드 남김 - 더블 블랙 알고리즘으로
	// 3. 중간 레드 삭제 - 자식 블랙이 대체함
	// 4. 중간 블랙 삭제 - 자식 레드가 블랙이 되며 대체함
	// 더블 블랙:
	// 형제가 레드인 경우 - 부모-형제 회전, 이후 서로 색 교환 - 알고리즘 다시 시작
	// 형제가 블랙이고 조카가 모두 블랙인 경우 - 블랙을 부모로 옮김 - 알고리즘 다시 시작
	// 안쪽 조카가 레드인 경우 - 레드 조카와 블랙 형제를 회전, 색 교환 - 알고리즘 다시 시작
	// 바깥 조카가 레드인 경우 - 형제-부모 회전, 색교환, 바깥 조카를 블랙으로
	//
	node<T1, T2>* pNow = pDelete;

	while (true)
	{
		node<T1, T2>* pParent = nullptr;
		node<T1, T2>* pSibling = nullptr;
		node<T1, T2>* pNephewL = nullptr;
		node<T1, T2>* pNephewR = nullptr;
		node<T1, T2>* pGrand = nullptr;
		// 노드 세팅
		if (pNow->pNodeConnection[(int)connection::parent] != nullptr)
		{
			pParent = pNow->pNodeConnection[(int)connection::parent];
			if (pParent->pNodeConnection[(int)connection::lchild] != nullptr && pParent->pNodeConnection[(int)connection::rchild] != nullptr)
			{
				if (pParent->pNodeConnection[(int)connection::lchild] == pNow)
					pSibling = pParent->pNodeConnection[(int)connection::rchild];
				else
					pSibling = pParent->pNodeConnection[(int)connection::lchild];
				if (pSibling->pNodeConnection[(int)connection::lchild] != nullptr)
					pNephewL = pSibling->pNodeConnection[(int)connection::lchild];
				if (pSibling->pNodeConnection[(int)connection::rchild] != nullptr)
					pNephewR = pSibling->pNodeConnection[(int)connection::rchild];
			}
			if (pParent->pNodeConnection[(int)connection::parent] != nullptr)
				pGrand = pParent->pNodeConnection[(int)connection::parent];
		}

		// 2. 리프 블랙 삭제 - 더블 블랙 더미노드 남김 - 더블 블랙 알고리즘으로
		if (pNow->nodeColor == color::black
			&& pNow->pNodeConnection[(int)connection::lchild] == nullptr
			&& pNow->pNodeConnection[(int)connection::rchild] == nullptr)
		{
			pNow->nodeColor = color::doubleblack;
			continue;
		}

		// 더블 블랙 알고리즘
		else if (pNow->nodeColor == color::doubleblack)
		{
			// 더블 블랙이 루트인 경우 - 일반 블랙으로 변경 후 알고리즘 종료
			if (pParent == nullptr)
			{
				pNow->nodeColor = color::black;
				break;
			}

			// 형제가 레드인 경우 - 부모-형제 회전, 이후 서로 색 교환 - 알고리즘 다시 시작
			if (pSibling->nodeColor == color::red)
			{
				pSibling->nodeColor = pParent->nodeColor;
				pParent->nodeColor = color::red;
				if (pGrand != nullptr)
				{
					if (pGrand->pNodeConnection[(int)connection::lchild] == pParent)
					{
						pGrand->pNodeConnection[(int)connection::lchild] = pSibling;
					}
					else
					{
						pGrand->pNodeConnection[(int)connection::rchild] = pSibling;
					}
					pSibling->pNodeConnection[(int)connection::parent] = pGrand;
				}
				else
				{
					pRoot = pSibling;
				}

				if (pNow == pParent->pNodeConnection[(int)connection::rchild])
				{
					pSibling->pNodeConnection[(int)connection::rchild] = pParent;
					pParent->pNodeConnection[(int)connection::lchild] = pNephewR;
					pNephewR->pNodeConnection[(int)connection::parent] = pParent;
				}
				else
				{
					pSibling->pNodeConnection[(int)connection::lchild] = pParent;
					pParent->pNodeConnection[(int)connection::rchild] = pNephewL;
					pNephewL->pNodeConnection[(int)connection::parent] = pParent;
				}
				pParent->pNodeConnection[(int)connection::parent] = pSibling;

				continue;
			}
			// 형제가 블랙이고 
			else if (pSibling->nodeColor == color::black)
			{
				// 조카가 모두 블랙인 경우 - 블랙을 부모로 옮김 - 부모가 더블 블랙이 된 경우 알고리즘 다시 시작
				if ((pNephewL == nullptr || pNephewL->nodeColor == color::black) && (pNephewR == nullptr || pNephewR->nodeColor == color::black))
				{
					if (pParent->nodeColor == color::red)
						pParent->nodeColor = color::black;
					else if (pParent->nodeColor == color::black)
						pParent->nodeColor = color::doubleblack;

					pSibling->nodeColor = color::red;
					pNow->nodeColor = color::black;
					
					if (pParent->nodeColor == color::doubleblack)
					{
						pNow = pParent;
						continue;
					}
					else
					{
						break;
					}
				}
				// 안쪽 조카가 레드인 경우 - 레드 조카와 블랙 형제를 회전, 색 교환 - 알고리즘 다시 시작
				else if (pNephewR != nullptr && (pNow == pParent->pNodeConnection[(int)connection::rchild] && pNephewR->nodeColor == color::red))
				{
					pNephewR->nodeColor = color::black;
					pSibling->nodeColor = color::red;
					pParent->pNodeConnection[(int)connection::lchild] = pNephewR;
					pNephewR->pNodeConnection[(int)connection::parent] = pParent;
					pSibling->pNodeConnection[(int)connection::parent] = pNephewR;
					pSibling->pNodeConnection[(int)connection::rchild] = pNephewR->pNodeConnection[(int)connection::lchild];
					if (pNephewR->pNodeConnection[(int)connection::lchild] != nullptr)
						pNephewR->pNodeConnection[(int)connection::lchild]->pNodeConnection[(int)connection::parent] = pSibling;
					pNephewR->pNodeConnection[(int)connection::lchild] = pSibling;
				}
				else if (pNephewL != nullptr && (pNow == pParent->pNodeConnection[(int)connection::lchild] && pNephewL->nodeColor == color::red))
				{
					pNephewL->nodeColor = color::black;
					pSibling->nodeColor = color::red;
					pParent->pNodeConnection[(int)connection::rchild] = pNephewL;
					pNephewL->pNodeConnection[(int)connection::parent] = pParent;
					pSibling->pNodeConnection[(int)connection::parent] = pNephewL;
					pSibling->pNodeConnection[(int)connection::lchild] = pNephewL->pNodeConnection[(int)connection::rchild];
					if (pNephewL->pNodeConnection[(int)connection::rchild] != nullptr)
						pNephewL->pNodeConnection[(int)connection::rchild]->pNodeConnection[(int)connection::parent] = pSibling;
					pNephewL->pNodeConnection[(int)connection::rchild] = pSibling;
					continue;
				}
				// 바깥 조카가 레드인 경우 - 형제-부모 회전, 색교환, 바깥 조카를 블랙으로
				else
				{
					bool rootUpdate = false;
					if (pNow == pParent->pNodeConnection[(int)connection::rchild])
					{
						if (pGrand != nullptr)
						{
							if (pGrand->pNodeConnection[(int)connection::lchild] == pParent)
								pGrand->pNodeConnection[(int)connection::lchild] = pParent;
							else
								pGrand->pNodeConnection[(int)connection::rchild] = pParent;
						}
						else
						{
							rootUpdate = true;
						}
						pSibling->pNodeConnection[(int)connection::parent] = pGrand;
						pParent->pNodeConnection[(int)connection::parent] = pSibling;
						pSibling->pNodeConnection[(int)connection::rchild] = pParent;
						if (pNephewR != nullptr)
							pNephewR->pNodeConnection[(int)connection::parent] = pParent;
						pParent->pNodeConnection[(int)connection::lchild] = pNephewR;
						color temp = pParent->nodeColor;
						pParent->nodeColor = pSibling->nodeColor;
						pSibling->nodeColor = temp;
						if (pNephewL != nullptr)
							pNephewL->nodeColor = color::black;
						if (rootUpdate)
							pRoot = pSibling;
					}
					else
					{
						bool rootUpdate = false;
						if (pGrand != nullptr)
						{
							if (pGrand->pNodeConnection[(int)connection::rchild] == pParent)
								pGrand->pNodeConnection[(int)connection::rchild] = pSibling;
							else
								pGrand->pNodeConnection[(int)connection::lchild] = pSibling;
						}
						else
						{
							rootUpdate = true;
						}
						pSibling->pNodeConnection[(int)connection::parent] = pGrand;
						pParent->pNodeConnection[(int)connection::parent] = pSibling;
						pSibling->pNodeConnection[(int)connection::lchild] = pParent;
						if (pNephewL != nullptr)
							pNephewL->pNodeConnection[(int)connection::parent] = pParent;
						pParent->pNodeConnection[(int)connection::rchild] = pNephewL;
						color temp = pParent->nodeColor;
						pParent->nodeColor = pSibling->nodeColor;
						pSibling->nodeColor = temp;
						if (pNephewR != nullptr)
							pNephewR->nodeColor = color::black;
						if (rootUpdate)
							pRoot = pSibling;
					}
					pNow->nodeColor = color::black;
					break;
				}
			}
		}
		// 3. 중간 레드 삭제 - 자식 블랙이 대체함

		// 4. 중간 블랙 삭제 - 자식 레드가 블랙이 되며 대체함
		else if (pNow->nodeColor == color::black && (pNow->pNodeConnection[(int)connection::lchild] != nullptr || pNow->pNodeConnection[(int)connection::rchild] != nullptr))
		{
			if (pNow->pNodeConnection[(int)connection::lchild] != nullptr)
			{
				pNow->pNodeConnection[(int)connection::lchild]->nodeColor = color::black;
			}
			else
			{
				pNow->pNodeConnection[(int)connection::rchild]->nodeColor = color::black;
			}
			break;
		}

		else
		{
			break;
		}
	}
}


template <class _Ty>
struct _Unrefwrap_helper {
	using type = _Ty;
};

template <class _Ty>
using _Unrefwrap_t = typename _Unrefwrap_helper<std::decay_t<_Ty>>::type;



template<typename T1, typename T2>
pair<_Unrefwrap_t<T1>, _Unrefwrap_t<T2>>* make_pair(T1&& _first, T2&& _second)
{
	
	pair<_Unrefwrap_t<T1>, _Unrefwrap_t<T2>>* Pair = new pair<_Unrefwrap_t<T1>, _Unrefwrap_t<T2>>(std::forward<T1>(_first), std::forward<T2>(_second));

	//Pair.first = std::forward<T1>(_first);
	//Pair.second = std::forward<T2>(_second);

	return Pair;
}


```