---
layout: post
title: Data Structure 2. 리스트
date: 2024-07-14 18:00:00 +0900
category: Data Structure
---

## 1. 배열

1. [정의](#1-배열)
2. [연결 리스트 구현](#2-연결-리스트-구현)

---

<br><br>

리스트 자료구조는 연결 리스트(Linked List) 라고도 불리며, 데이터를 하나의 노드에 담아서 이 노드에 다음 데이터가 담긴 노드 주소를 같이 저장해 자료를 저장하는 방식이다.

리스트는 배열과 비교하여 연속적인 공간에 배정될 필요가 없고 각 노드는 개별적인 주소를 갖고 있기 때문에 데이터의 추가 및 제거가 용이하다. 다만 배열에 비해 자료가 노드 별로 주소가 별도로 존재하고 다음 자료에 대한 주소를 이전 노드가 가지고 있기에 특정 자료로의 접근이 순차적인 탐색이 이루어져야 가능하다.

또한 매번 공간을 미리 할당받아야 하는 배열과는 달리 각 노드 별로 공간을 차지하기에 데이터가 추가될 때마다 노드 공간을 할당해 주면 되므로 낭비되는 공간이 없고 필요한 만큼만 공간을 할당받아 사용한다.

만약 노드가 다음 노드의 주소 뿐만 아니라 이전 노드의 주소도 갖고 있다면 이는 이중 연결 리스트라고 하며, 마지막 노드의 포인터가 null 이 아닌 처음 노드를 다시 가리킨다면 순환 연결 리스트라고 한다.

C++의 STL에서 `list` 라는 템플릿을 제공한다.

<br> 

||연결 리스트||
|데이터 작업||시간복잡도|
|:---:||:---:|
|접근||$O(n)$|
|탐색||$O(n)$|
|삽입||$O(1)$|
|삭제||$O(1)$|

---

<br><br>

>### 2. 연결 리스트 구현

- 노드 구조체

|구현 멤버|설명|
|---|---|
|T data|자료형 T 데이터|
|Node<T>* pPrevNode|이전 노드를 가리키는 포인터|
|Node<T>* pNextNode|다음 노드를 가리키는 포인터|

- 리스트 클래스

|구현 멤버|설명|
|---|---|
|Node<T>* pHeadNode|첫 노드의 주소를 저장하는 포인터|
|int dataCount|리스트가 관리중인 데이터 개수|
|List()|기본 생성자|
|List(const List& _copy)|복사 생성자|
|~List()|소멸자, 호출 시 저장된 데이터를 순차적으로 메모리 해제|
|void push_back(T _input)|입력받은 데이터를 리스트의 가장 뒤쪽에 노드를 추가하여 삽입|
|void push_front(T _input)|입력받은 데이터를 리스트의 가장 앞쪽에 노드를 추가하여 삽입, 헤드 노드 갱신|
|iterator begin()|시작 노드를 가리키는 이터레이터를 반환하는 함수|
|iterator end()|마지막 데이터 그 다음 노드를 가리키는 이터레이터를 반환하는 함수.
|iterator insert(iterator _where, T _data)|입력받은 이터레이터가 가리키는 데이터 앞에 입력받은 데이터 노드를 삽입|
|iterator erase(iterator _where)|입력받은 이터레이터가 가리키는 데이터 노드를 삭제|


- 이터레이터 이너클래스

|구현 멤버|설명|
|---|---|
|Node<T>* pPointingNode|이터레이터가 가리키고 있는 데이터 노드의 주소|
|bool isValid|이터레이터가 유효한지 저장하는 bool 변수|
|List<T>* pHandlerList|이터레이터가 가리키는 데이터가 속한 리스트의 주소|





<br>

<details>
<summary>코드 접기/펼치기</summary>
<div markdown="1">


```cpp

#pragma once

#include <assert.h>


// 리스트의 노드 스트럭트 템플릿

template <typename T>
struct Node
{
	T data;
	Node<T>* pPrevNode;
	Node<T>* pNextNode;
};


// 리스트 본체 클래스 템플릿

template <typename T>
class List
{
private:
	Node<T>* pHeadNode;
	int dataCount;

public:
	// 생성자 및 소멸자
	List()
		: pHeadNode(nullptr), dataCount(0) {}

	List(const List& _copy)
		: pHeadNode(_copy.pHeadNode), dataCount(_copy.dataCount)
	{
		if (pHeadNode == nullptr) {
			dataCount = 0;
		}
	}

	~List() {
		Node<T>* pNextNode = pHeadNode;
		while (pNextNode == nullptr)
		{
			Node<T>* pDeleteNode = pNextNode;
			pNextNode = pNextNode->pNextNode;
			delete pDeleteNode;
		}
	}


	// 기본 함수

	void push_back(T _input) {
		Node<T>* newNode = new Node<T>;
		newNode->data = _input;
		newNode->pNextNode = nullptr;
		if (dataCount == 0) {
			newNode->pPrevNode = nullptr;
			pHeadNode = newNode;
		}
		else {
			Node<T>* pLastNode = pHeadNode;
			while (pLastNode->pNextNode != nullptr) {
				pLastNode = pLastNode->pNextNode;
			}
			newNode->pPrevNode = pLastNode;
			pLastNode->pNextNode = newNode;
		}
		dataCount++;
	}

	void push_front(T _input) {
		Node<T>* newNode = new Node<T>;
		newNode->data = _input;
		newNode->pPrevNode = nullptr;
		if (dataCount == 0) {
			newNode->pNextNode = nullptr;
		}
		else {
			newNode->pNextNode = pHeadNode;
			pHeadNode->pPrevNode = newNode;
		}
		pHeadNode = newNode;
		dataCount++;
	}


	// 이터레이터 활용 함수

	class iterator;

	iterator begin() {
		return iterator(this->pHeadNode, this);
	}

	iterator end() {
		return iterator(nullptr, this);
	}

	iterator insert(iterator _where, T _data) {
		Node<T>* newNode = new Node<T>;
		newNode->data = _data;
		newNode->pNextNode = _where.pPointingNode;
		newNode->pPrevNode = _where.pPointingNode->pPrevNode;
		if (newNode->pNextNode != nullptr) {
			newNode->pNextNode->pPrevNode = newNode;
		}
		if (newNode->pPrevNode != nullptr) {
			newNode->pPrevNode->pNextNode = newNode;
		}
		else {
			this->pHeadNode = newNode;
		}
		iterator newiter(_where);
		_where.isValid = false;
		this->dataCount++;
		return newiter;
	}


	iterator erase(iterator _where) {
		assert(_where.isValid || this->dataCount != 0);

		Node<T>* prevNode = _where.pPointingNode->pPrevNode;
		Node<T>* nextNode = _where.pPointingNode->pNextNode;
		if (prevNode != nullptr){
			prevNode->pNextNode = nextNode;
		}
		else {
			this->pHeadNode = nextNode;
		}
		if (nextNode != nullptr){
			nextNode->pPrevNode = prevNode;
		}
		iterator newiter(_where);
		newiter.pPointingNode = nextNode;
		delete _where.pPointingNode;
		_where.isValid = false;

		this->dataCount--;
		return  newiter;
	}

	// 이너클래스 이터레이터 구현

	class iterator
	{
		friend List;
	private:
		Node<T>* pPointingNode;
		bool isValid;
		List<T>* pHandlerList;


		// 이터레이터의 생성자 및 소멸자

	public:
		iterator()
			: pPointingNode(nullptr), isValid(true), pHandlerList(nullptr){}

		iterator(const iterator& _copy)
			: pPointingNode(_copy.pPointingNode), isValid(_copy.isValid), pHandlerList(_copy.pHandlerList){}

		iterator(Node<T>* _node, List<T>* _list)
			: pPointingNode(_node), isValid(true), pHandlerList(_list) {}


		// 이터레이터 연산자 오버로딩

		iterator& operator ++() {
			assert(pPointingNode != nullptr || isValid);

			pPointingNode = pPointingNode->pNextNode;

			return *this;
		}

		iterator operator ++(int) {
			assert(pPointingNode != nullptr || isValid);

			iterator iter(*this);
			pPointingNode = pPointingNode->pNextNode;

			return iter;
		}

		iterator& operator --() {
			assert(pPointingNode != nullptr || isValid || pPointingNode->pPrevNode != nullptr);

			pPointingNode = pPointingNode->pPrevNode;

			return *this;
		}

		iterator operator --(int) {
			assert(pPointingNode != nullptr || isValid || pPointingNode->pPrevNode != nullptr);

			iterator iter(*this);
			pPointingNode = pPointingNode->pPrevNode;

			return iter;
		}

		bool operator ==(const iterator _other) {
			return (pPointingNode == _other.pPointingNode);
		}

		bool operator !=(const iterator _other) {
			return !(pPointingNode == _other.pPointingNode);
		}

		T& operator*() {
			return pPointingNode->data;
		}
	};
};

```

---