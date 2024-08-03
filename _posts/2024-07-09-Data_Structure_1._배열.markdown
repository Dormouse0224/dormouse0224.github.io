---
layout: post
title: Data Structure 1. 배열
date: 2024-07-09 18:00:00 +0900
category: Data_Structure
---

## 1. 배열

1. [정의](#1-정의)
2. [동적 배열](#2-동적-배열)
3. [동적 배열 구현](#3-동적-배열-구현)

---

<br><br>

>### 1. 정의

배열(Array)은 인덱스와 인덱스에 대응하는 데이터로 이루어진 자료구조이다.

특징으로, 배열은 데이터가 연속적인 공간에 저장되며, 이에 따라 주소값도 연속적이다.

그래서 인덱스 번호만 있으면 첫 주소에 합연산으로 해당 인덱스에 해당하는 데이터로의 접근이 매우 빠른 장점이 있다.

단, 배열은 데이터를 연속적인 공간에서 관리해야 한다는 특징으로 인해 중간의 데이터를 삭제하거나 중간에 삽입하려고 하면 그 뒤에 존재하는 모든 데이터를 밀거나 당겨서 자리를 옮겨 연속성을 유지시켜야 하므로, 중간의 데이터에 대한 삽입과 삭제는 힘들다.

그리고 저장되어 있는 데이터의 탐색도 비효율적이다. 특정 데이터를 배열 내에서 찾는다고 할 때, 배열은 모든 데이터를 확인해야 하므로 느린 탐색 속도를 보인다.

<br> 

||배열||
|데이터 작업||시간복잡도|
|:---:||:---:|
|접근||$O(1)$|
|탐색||$O(n)$|
|삽입||$O(n)$|
|삭제||$O(n)$|

---

<br><br>

>### 2. 동적 배열

동적 배열은 C++ STL에서 `vector` 라는 타입으로 지원되며, 코드 내에서 `arr[n]` 으로 스택 영역에 선언되는 크기가 고정된 정적 배열과는 달리 힙 영역에서 동적 할당을 통해 선언되는 유동적인 크기의 배열을 의미한다.

동적 배열 또한 연속적인 주소에 데이터를 저장하며, 기본적인 배열 자료구조의 특징을 모두 갖고 있다.

초기에 일정한 공간을 미리 힙 메모리에 할당받고, 이후 해당 메모리가 가득 찬 상태에서 추가적인 데이터를 저장하려 하면 그 때 더 넓은 메모리 공간을 새로 할당받은 뒤 기존 데이터를 복사하고 새로 받은 데이터를 추가로 넣은 후 기존 메모리 공간을 해제하는 식으로 확장이 이루어진다.


---

<br><br>

>### 3. 동적 배열 구현


|구현 멤버|설명|
|---|---|
|T* headptr|동적할당된 배열의 시작 주소를 저장하는 포인터|
|int count|배열이 현재 가지고 있는 데이터 개수|
|int capacity|배열이 최대 저장 가능한 데이터 공간의 개수|
|void resize(int _resizeCap)|_resizeCap 크기의 공간으로 기존 동적할당된 공간의 데이터를 재할당하는 함수|
|void push_back(const T& _data)|저장된 데이터의 가장 뒤에 새로운 데이터를 넣는 함수|
|void insert(const T& _data, unsigned int _index)|특정 인덱스에 새로운 데이터를 넣는 함수|
|int size()|현재 저장된 데이터의 개수를 반환하는 함수|
|bool empty()|배열이 비어있는지 확인하는 함수|
|void clear()|배열의 데이터를 전부 삭제하는 함수, 데이터 공간의 크기는 유지|
|void erase(unsigned int _index)|특정 인덱스의 데이터를 삭제하는 함수|
|T operator [](unsigned int _index)|특정 인덱스에 존재하는 데이터 값을 반환하는 함수|
|Vector()|기본 생성자, 크기 2의 빈 배열이 생성됨|
|Vector(int _count)|생성자 오버로딩, _count 만큼의 공간을 0으로 초기화한 배열이 생성됨|
|Vector(int _count, int _initNum)|생성자 오버로딩, _count 만큼의 공간을 _initNum으로 초기화한 배열이 생성됨|
|Vector(const Vector<T>& _vec)|복사 생성자, 같은 타입의 배열을 복사하여 생성함|
|~Vector()|소멸자, 메모리 할당 해제|


<br>

<details>
<summary>코드 접기/펼치기</summary>
<div markdown="1">

``` cpp

#pragma once

#include <assert.h>

template<typename T>
class Vector {
private:
	T* headptr;
	int count;
	int capacity;

	void resize(int _resizeCap) {
		if (capacity >= _resizeCap) {
			assert(nullptr);
		}

		T* newhead = new T[_resizeCap];
		for (int i = 0; i < count; ++i) {
			newhead[i] = headptr[i];
		}
		capacity = _resizeCap;
		T* delptr = headptr;
		headptr = newhead;
		delete[] delptr;
	}


public:

	// push_back
	void push_back(const T& _data) {
		// 만약 기존 배열이 꽉 찼다면 공간 확장
		if (count == capacity) {
			resize(capacity * 2);
		}

		// 마지막 요소의 뒤에 data 삽입
		headptr[count] = _data;
		count++;
	}


	// insert
	void insert(const T& _data, unsigned int _index) {
		// 입력된 인덱스 값이 수용량을 넘어선 경우 확장하고 중간 빈 공간을 0으로 채운다
		if (capacity <= _index) {
			resize(_index + 1);
			for (int i = count; i < _index; i++) {
				push_back(0);
			}
			push_back(_data);
			count = _index + 1;
		}
		// 수용량은 안넘지만 현재 자료 수보다 넘어선 공간에 넣는 경우 빈공간을 0으로 채운다
		else if (count <= _index) {
			for (int i = count; i < _index; i++) {
				push_back(0);
			}
			push_back(_data);
			count = _index + 1;
		}
		// 두 경우 모두 아니면 값을 자리에 넣고 이후 값을 뒤로 민다
		else {
			// 만약 기존 배열이 꽉 찼다면 공간 확장
			if (count == capacity) {
				resize(capacity * 2);
			}
			for (int i = count - 1; i >= _index; i--) {
				headptr[i + 1] = headptr[i];
			}
			headptr[_index] = _data;
			count++;
		}
	}

	// size
	int size() {
		return count;
	}

	// empty
	bool empty() {
		return (size() == 0);
	}

	// clear
	void clear() {
		count = 0;
	}

	// erase
	void erase(unsigned int _index) {
		if (_index >= count) {
			assert(nullptr);
		}
		for (int i = _index; i < count; i++) {
			headptr[i] = headptr[i + 1];
		}
		count--;
	}

	// []
	T operator [](unsigned int _index) {
		if (_index >= count) {
			assert(nullptr);
		}
		return headptr[_index];
	}


	// 생성자
	Vector() {
		headptr = new T[2];
		capacity = 2;
	}

	Vector(int _count) {
		headptr = new T[_count];
		for (int i = 0; i < _count; ++i) {
			headptr[i] = 0;
		}
		count = _count;
		capacity = _count;
	}

	Vector(int _count, int _initNum) {
		headptr = new T[_count];
		for (int i = 0; i < _count; ++i) {
			headptr[i] = _initNum;
		}
		count = _count;
		capacity = _count;
	}

	Vector(const Vector<T>& _vec) {
		headptr = new T[_vec.capacity];
		for (int i = 0; i < _vec.count; ++i) {
			headptr[i] = _vec.headptr[i];
		}
		count = _vec.count;
		capacity = _vec.capacity;
	}

	// 소멸자
	~Vector() {
		delete[] headptr;
	}

};

```

</div>
</details>

---