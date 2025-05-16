# 자료구조와 알고리즘 핵심 개념
- 선형
	- [Array](#array), [Dynamic Array](#dynamic-array)
	- [Linked List](#linked-list)
	- [Stack](#stack), [Queue](#queue)
- 비선형
	- [Tree](#tree)
	- [Graph](#graph)

## Array
- **특징**: 연속된 메모리 공간에 순차적 저장
- **장점**: 
  - 인덱스를 통한 빠른 접근 (O(1))
  - 메모리 효율적 사용
- **단점**: 
  - 크기 변경 불가
  - 삽입/삭제 시 O(n) 시간 소요
- **용도**: 데이터 크기가 고정적이고 빠른 접근이 필요한 경우

### Reverse
```cpp
void ReverseArray(int arr[], int start, int end)
{
	while (start < end)
	{
		int temp = arr[start];
		arr[strat] = arr[end];
		arr[end] = temp;

		start++;
		end--;
	}
}
```

### Split
```cpp
/**
 * CPP split() method.
 */
vector<string> Split(string Input, string Delimiter)
{
	vector<string> Result;
	long long pos = 0;
	string token = "";
	while ((pos = Input.find(Delimiter)) != string::npos)
	{
		token = Input.substr(0, pos);
		Result.push_back(token);
		Input.erase(0, pos + Delimiter.length());
	}
	Result.push_back(Input);
	
	return Result;
}

/**
 * Cpp split() method - faster version.
 */
vector<string> Split(const string& Input, string Delimiter)
{
	vector<string> Result;
	auto Start = 0;
	auto End = Input.find(Delimiter);
	while (End != string::npos)
	{
		Result.push_back(Input.substr(Start, End - Start));
		Start = End + Delimiter.size();
		End = Input.find(Delimiter, Start);
	}
	Result.push_back(Input.substr(Start));

	return Result;
}
```

## Dynamic Array
- cpp : vector	/ csharp : list
- 저장공간을 늘려 저장할 수 있는 유동적인 자료구조로 자주 사용된다
- vector의 삽입/삭제
	- 시작 : O(N)
	- 중간 : O(N)
	- 끝 : O(1)
	- 임의의 index 접근 : O(1)
- sample code
```cpp
template<typename T>
class CaVector
{
public:
	explicit CaVector()
	{
	}
	~CaVector()
	{
		if (_buffer)
		{
			delete[] _buffer;
		}
	}

	void clear()
	{
		if (_buffer)
		{
			delete[] _buffer;
			_buffer = new T[_capacity];
		}
		_size = 0;
	}

	void push_back(const T& data)
	{
		if (_size == _capacity)
		{
			int newCapacity = static_cast<int>(_capacity * 1.5);
			if (newCapacity == _capacity)	// capacity가 0이나 1인경우
			{
				newCapacity++;
			}

			reserve(newCapacity);
		}

		_buffer[_size] = data;
		_size++;
	}

	void pop_back()
	{
		 //TODO : delete?
		_size--;
	}

	T& back()
	{
		assert(_size > 0);
		return _buffer[_size - 1];
	}

	// forcefully change size
	void resize(int size)
	{
		// TODO :
		reserve(size);
		_size = size;
	}

	// allocate
	void reserve(int capacity)
	{
		if (_capacity >= capacity)
		{
			return;
		}

		_capacity = capacity;

		T* newData = new T[_capacity];
		for (int i = 0; i < _size; i++)
		{
			newData[i] = _buffer[i];
		}

		if (_buffer)
		{
			delete[] _buffer;
		}

		_buffer = newData;
	}

	T& operator[](int index)
	{
		assert(index >= 0 && index < _size);
		return _buffer[index];
	}

	bool empty()
	{
		return _size == 0;
	}
	int size()
	{
		return _size;
	}
	int capacity()
	{
		return _capacity;
	}

private:
	T* _buffer = nullptr;
	int _size = 0;		// 데이터 개수
	int _capacity = 0;	// 할당한 전체 크기
};
```

## Linked List
- cpp : list	/ csharp : linked list
- list의 삽입/삭제
	- 시작 : O(1)
	- 중간 : O(1) `삭제할 노드를 알고 있을 때`
	- 끝 : O(1)
	- 임의의 index 접근 : O(N)
- **특징**:
    - 노드 기반 순차적 데이터 구조
	- 연속적인 메모리 위치에 저장되지 않는 선형 데이터 구조로 포인터를 사용해서 연결된다
	- 각 노드는 데이터 필드와 다음 노드에 대한 참조를 포함하는 노드로 구성된다
- **장점**: 
  - 삽입/삭제가 용이 (**항상 빠르다는 것이 아니라 삽입 삭제할 노드를 알고 있을 경우**)
  - 위 경우가 자주 일어나는 상황을 제외하고는 동적배열이 일반적으로 성능이 더 좋아 자주 사용되는 편은 아니다
  - 삭제도 굳이 중간에 있는 것을 빼는 것이 아니라 동적 배열에 추가적으로 boolean 변수를 이용하여 false면 폐기를 하는 경우가 더 많다
  ```cpp
  	void Insert(Node* posNode, int data)
	{
		/**					[node]					*/
		/** [dummy]	[prevN]		 [posNode]	[dummy]	*/
		/** [head]							[tail] 	*/
		Node* node = new Node(data);
		Node* prevNode = posNode->prev;

		prevNode->next = node;
		node->prev = prevNode;
		node->next = posNode;
		posNode->prev = node;
	}

	Node* Remove(Node* node)
	{
		/**					[node]					*/
		/** [dummy]	[prevN]		 [nextN]	[dummy]	*/
		/** [head]							[tail] 	*/
		Node* prevNode = node->prev;
		Node* nextNode = node->next;

		prevNode->next = nextNode;
		nextNode->prev = prevNode;

		delete node;

		return nextNode;
	}
  ```
  - 동적 크기 조절 가능
- **단점**: 
  - 임의 접근 불가
  - 추가 메모리 공간 필요
- Single Linked List:
    ```cpp
	// 노드 구조
	struct Node
	{
		int Data;         // 노드의 데이터
		Node* Next;       // 다음 노드를 가리키는 포인터

		// 기본 생성자
		Node(int InData = 0) : Data(InData), Next(nullptr) {}
	};
	```
	```cpp
	// 연결 리스트 클래스
	class LinkedList
	{
	private:
		Node* Head;      // 연결 리스트의 첫 번째 노드

	public:
		LinkedList() : Head(nullptr) {}

		// 노드를 삽입하는 함수
		Node* Insert(int Data);

		// 노드를 삭제하는 함수
		Node* Remove(int Data);

		// 두 리스트를 연결하는 함수
		Node* Concatenate(LinkedList& OtherList);

		// 순회 포인터를 이용한 역순 함수
		Node* Reverse();

		// 리스트의 크기를 반환하는 함수
		int Size();

		// 리스트를 출력하는 함수
		void Display();
	};
	```
	```cpp
	Node* LinkedList::Insert(int Data)
	{
		Node* NewNode = new Node(Data);  // 새로운 노드 생성

		if (Head == nullptr)
		{
			Head = NewNode;
		}
		else
		{
			Node* Current = Head;
			while (Current->Next != nullptr)
			{
				Current = Current->Next;
			}
			Current->Next = NewNode;
		}

		return NewNode;
	}    
	```
	```cpp
	Node* LinkedList::Remove(int Data)
	{
		if (Head == nullptr)
		{
			return nullptr;  // 리스트가 비어 있을 경우
		}

		if (Head->Data == Data)
		{
			Node* Temp = Head;
			Head = Head->Next;
			delete Temp;
			return nullptr;
		}

		Node* Current = Head;
		while (Current->Next != nullptr)
		{
			if (Current->Next->Data == Data)
			{
				Node* Temp = Current->Next;
				Current->Next = Current->Next->Next;
				delete Temp;
				return Current;
			}
			Current = Current->Next;
		}

		return nullptr;
	}
	```
	```cpp
	Node* LinkedList::Concatenate(LinkedList& OtherList)
	{
		if (Head == nullptr)
		{
			Head = OtherList.Head;
			return Head;
		}

		if (OtherList.Head == nullptr)
		{
			return nullptr;  // 다른 리스트가 비어 있을 경우
		}

		Node* Current = Head;
		while (Current->Next != nullptr)
		{
			Current = Current->Next;
		}
		Current->Next = OtherList.Head;
		OtherList.Head = nullptr;            // 다른 리스트 초기화

		return Current;
	}
	```
	```cpp
	 Node* LinkedList::Reverse()
	 {
		 // 역순으로 변환할 때 사용할 이전 노드 포인터를 초기화
		 Node* Prev = nullptr;

		 // 현재 노드 포인터를 리스트의 첫 번째 노드로 설정
		 Node* Current = Head;

		 // 다음 노드 포인터를 초기화
		 Node* Next = nullptr;

		 // 리스트의 모든 노드를 순회
		 while (Current != nullptr)
		 {
			 // 현재 노드의 다음 노드를 임시로 저장
			 Next = Current->Next;

			 // 현재 노드의 다음 포인터를 이전 노드로 설정하여 역순으로 연결
			 Current->Next = Prev;

			 // 이전 노드를 현재 노드로 이동
			 Prev = Current;

			 // 현재 노드를 다음 노드로 이동
			 Current = Next;
		 }

		 // 리스트의 첫 번째 노드를 역순으로 변환된 리스트의 첫 번째 노드로 설정
		 Head = Prev;

		 // 역순으로 변환된 리스트의 첫 번째 노드를 반환
		 return Head;
	 }
	```
	```cpp
	int LinkedList::Size()
	{
		int Count = 0;
		Node* Temp = Head;
		while (Temp != nullptr)
		{
			Count++;
			Temp = Temp->Next;
		}
		return Count;
	}
	```
	```cpp
	void LinkedList::Display()
	{
		Node* Temp = Head;
		while (Temp != nullptr)
		{
			std::cout << Temp->Data << " ";
			Temp = Temp->Next;
		}
		std::cout << std::endl;
	}
	```
	```cpp
	int main()
	{
		LinkedList List1;
		LinkedList List2;

		List1.Insert(1);
		List1.Insert(2);
		List1.Insert(3);

		List2.Insert(4);
		List2.Insert(5);
		List2.Insert(6);

		std::cout << "List1: ";
		List1.Display();

		std::cout << "List2: ";
		List2.Display();

		std::cout << "Concatenated List: ";
		List1.Concatenate(List2);
		List1.Display();

		std::cout << "Reversed List: ";
		List1.Reverse();
		List1.Display();

		return 0;
	}
	```
- Double Linked List:
```cpp
template<typename T>
class Node
{
	//using T = int;
public:
	Node(T data)
		: data(data), prev(nullptr), next(nullptr) {}

public:
	T data;
	Node* prev;
	Node* next;
};

template<typename T>
class CaList
{
public:
	CaList()
	{
		_head = new Node<T>(0);
		_tail = new Node<T>(0);
		_head->next = _tail;
		_tail->prev = _head;
	}
	~CaList()
	{
		Node<T>* node = _head;
		while (node)
		{
			Node* temp = node;
			node = node->next;
			delete temp;
		}
	}

	Node<T>* GetNode(int index)
	{
		Node<T>* node = _head->next;
		if (node == _tail)
		{
			return nullptr;
		}

		for (int i = 0; i < index; i++)
		{
			if (node == _tail->prev)
			{
				return nullptr;
			}

			node = node->next;
		}

		return node;
	}

	void Print()
	{
		Node<T>* node = _head->next;
		while (node != _tail)
		{
			std::cout << node->data << " ";
			node = node->next;
		}
		std::cout << std::endl;
	}

	Node<T>* AddAtHead(int data)
	{
		/** Without dummy pointer */
		/** [head]	[node]	[temp]	[2]	[3]	[tail] */
		//Node* node = new Node(data);
		//if (!_head)
		//{
		//	_head = node;
		//	_tail = node;
		//}
		//else
		//{
		//	Node* temp = _head;
		//	node->next = temp;
		//	temp->prev = node;
		//	_head = node;
		//}

		/** With dummy pointer */
		/** [dummy]	[node]	[temp]	[2]	[3]	[dummy] */
		/** [head]							[tail]  */
		Node<T>* node = new Node<T>(data);
		Node<T>* temp = _head->next;
		 
		node->next = temp;
		temp->prev = node;
		_head->next = node;
		node->prev = _head;

		return node;
	}

	Node<T>* AddAtTail(int data)
	{
		/** [dummy]	[1]	[2]	[temp]	[node]	[dummy] */
		/** [head]							[tail]  */
		Node<T>* node = new Node<T>(data);
		Node<T>* temp = _tail->prev;

		temp->next = node;
		node->prev = temp;
		node->next = _tail;
		_tail->prev = node;

		return node;
	}

	void Insert(Node<T>* posNode, int data)
	{
		/**					[node]					*/
		/** [dummy]	[prevN]		 [posNode]	[dummy]	*/
		/** [head]							[tail] 	*/
		Node<T>* node = new Node<T>(data);
		Node<T>* prevNode = posNode->prev;

		prevNode->next = node;
		node->prev = prevNode;
		node->next = posNode;
		posNode->prev = node;
	}

	Node<T>* Remove(Node<T>* node)
	{
		/**					[node]					*/
		/** [dummy]	[prevN]		 [nextN]	[dummy]	*/
		/** [head]							[tail] 	*/
		Node* prevNode = node->prev;
		Node* nextNode = node->next;

		prevNode->next = nextNode;
		nextNode->prev = prevNode;

		delete node;

		return nextNode;
	}

private:
	Node<T>* _head = nullptr;
	Node<T>* _tail = nullptr;
};
```

## Stack
- **특징**: LIFO(Last In First Out) 구조
- **장점**: 
  - 구현이 간단
  - 메모리 효율적 사용
- **단점**: 데이터 접근의 제한성
- 연결 리스트를 이용한 구현
```cpp
#include <iostream>

struct Node
{
    int data;
    Node* next;
};


Node* top = nullptr;
Node* bottom = nullptr;

//Stack
void push(int value)
{
    Node* buff = new Node();
    buff->data = value;
    buff->next = top;
    top = buff;
}

void pop()
{
    Node* backup = top;
    top = top->next;
    delete backup;
}

int Top()
{
    return top->data;
}
```
- 동적 배열을 이용한 구현 : CaVector는 위 [dynamic array](#dynamic-array)를 이용
```cpp
#include "CaVector.h"

template<typename T>
class CaStack
{
public:
	void push(const T& value)
	{
		_container.push_back(value);
	}

	void pop()
	{
		_container.pop_back();
	}

	T& top()
	{
		return _container.back();
	}

	bool empty()
	{
		return _container.empty();
	}
	int size()
	{
		return _container.size();
	}

private:
	CaVector<T> _container;
};
```

## Queue
- **특징**: FIFO(First In First Out) 구조
- **장점**: 
  - 데이터 순서 보장
  - 동시성 처리에 유용
- **단점**: 중간 데이터 접근 불가
- 연결 리스트를 이용한 구현
```cpp
#include <iostream>

struct Node
{
    int data;
    Node* next;
};


Node* head = nullptr;
Node* tail = nullptr;


//Queue
void push(int value)
{

    if (head == nullptr)
    {
        Node* buff = new Node();
        buff->data = value;
        head = buff;
        tail = buff;
    }
    else
    {
        tail->next = new Node();
        tail = tail->next;
        tail->data = value;
    }
}

void pop()
{
    Node* backup = head;
    head = head->next;
    delete backup;
}

int top()
{
    return head->data;
}
```
- 동적 배열을 이용한 구현 : [dynamic array](#dynamic-array) 이용
```cpp
/**
 * empty : [front/back][][][][][].
 * push : [front][back][][][][][].
 */
template<typename T>
class CaQueue
{
public:
	CaQueue()
	{
		_container.resize(100);
	}

	void push(const T& value)
	{
		// TODO : check if full
		if (_size == _container.size())
		{

		}

		_container[_back] = value;
		_back = (_back + 1) % _container.size();	// circular
		_size++;
	}

	void pop()
	{
		_front = (_front + 1) % _container.size();	// circular
		_size--;
	}

	T& front()
	{
		return _container[_front];
	}

	bool empty()
	{
		return _size == 0;;
	}

private:
	CaVector<T> _container;

	int _front = 0;
	int _back = 0;
	int _size = 0;
};
```

## Recursive

### **1. 재귀 함수의 본질**
- 재귀 함수란 단순히 "자기 자신을 호출하는 함수"가 아님
- **정확한 설명**: 재귀 함수는 동일한 코드가 재활용되어, 같은 이름이지만 서로 다른 함수로 동작하는 구조
  - 즉, **코드만 동일하고, 호출된 각각의 함수는 서로 독립적인 공간**을 가짐
  - 이를 이해하면 스택(Stack) 구조와 함수 호출 과정을 명확히 파악할 수 있다
- sample code
```cpp
// 10! = 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1
// 10! = 10 * 9!
// 9! = 9 * 8!
// n! = n * (n - 1)!

int Factorial(int n)
{
	if (n <= 1)
	{
		return 1;
	}
	return n * Factorial(n - 1);
}
```
```cpp
// Euclidean algorithm
// a > b
// GCD(4212, 2484)
// = GCD(2484, (4212 % 2484) = 1728)
// GCD(1728, (2484 % 1728) = 756)
// GCD(756, (1728 % 756) = 216)
// GCD(216, (756 % 216) = 108)
// GCD(108, (216 % 108) = 0)
// = 108
int GCD(int a, int b)
{
	if (b == 0)
	{
		return a;
	}

	GCD(b, a % b);
}
```

### **2. 함수 호출과 스택 구조**
- 모든 함수 호출은 __스택(Stack)__ 에 기록
  - 새로운 함수가 호출될 때마다 스택에 쌓이고, 반환(return) 시 해당 함수가 스택에서 제거
  - 가장 마지막에 호출된 함수가 먼저 종료되는 __LIFO(Last In, First Out)__ 구조를 따름
  - 스택에서 함수가 제거되기 이전에 계속 호출되어 스택에 할당되면 메모리가 고갈된다
  - 즉, 할당된 스택프레임을 초과해서 사용하게되면 __스택 오버플로우(Stack overflow)__ 발생

## Tree
- 재귀를 이용하여 쉽게 접근할 수 있다
```cpp
class Node
{
public:
	Node(const char* data)
		: data(data)
	{

	}

public:
	const char* data;
	std::vector<Node*> children;
};

class CaTree
{
public:
	Node* CreateTree()
	{
		Node* root = new Node("root");
		{
			Node* node = new Node("Team1");
			root->children.push_back(node);
			{
				Node* leaf = new Node("leaf1");
				node->children.push_back(leaf);
			}
			{
				Node* leaf = new Node("leaf2");
				node->children.push_back(leaf);
			}
			{
				Node* leaf = new Node("leaf3");
				node->children.push_back(leaf);
			}
		}
		{
			Node* node = new Node("Team2");
			root->children.push_back(node);
			{
				Node* leaf = new Node("leaf1");
				node->children.push_back(leaf);
				{
					Node* ll = new Node("leaf1-1");
					leaf->children.push_back(ll);
					{
						Node* lll = new Node("leaf1-1-1");
						ll->children.push_back(lll);
					}
					{
						Node* lll = new Node("leaf1-1-2");
						ll->children.push_back(lll);
					}
				}
			}
			{
				Node* leaf = new Node("leaf2");
				node->children.push_back(leaf);
			}
		}
		{
			Node* node = new Node("Team3");
			root->children.push_back(node);
			{
				Node* leaf = new Node("leaf1");
				node->children.push_back(leaf);
			}
			{
				Node* leaf = new Node("leaf2");
				node->children.push_back(leaf);
				{
					Node* ll = new Node("leaf2-1");
					leaf->children.push_back(ll);
				}
			}
			{
				Node* leaf = new Node("leaf3");
				node->children.push_back(leaf);
			}
			{
				Node* leaf = new Node("leaf4");
				node->children.push_back(leaf);
			}
		}

		return root;
	}

	// depth : root에서 node까지 가는 edge의 개수
	void PrintTree(Node* root, int depth = 0)
	{
		for (int i = 0; i < depth; i++)
		{
			std::cout << "--";
		}

		std::cout << root->data << std::endl;

		for (Node* child : root->children)
		{
			PrintTree(child, depth + 1);
		}
	}

	// 재귀를 이용하여 최하단 자식의 depth를 return
	int GetDepth(Node* root)
	{
		if (root == nullptr)
		{
			return 0;
		}

		int maxDepth = 0;
		for (Node* child : root->children)
		{
			int childDepth = GetDepth(child);
			if (maxDepth < childDepth)
			{
				maxDepth = childDepth;
			}
		}

		return maxDepth + 1;
	}
};
```
- Heap(complete binary tree)
```cpp
template<typename T, typename Predicate = std::less<T>>
class CaPriorityQueue
{
public:
	// O(log n) time complexity for push
	void push(const T& value)
	{
		// heap structure is a complete binary tree
		_heap.push_back(value);

		// Move up the last element until it is in the correct position
		int now = static_cast<int>(_heap.size()) - 1;
		while (now > 0)
		{
			// compare with the parent node
			int next = (now - 1) / 2;
			//if (_heap[now] < _heap[next])
			//{
			//	break;
			//}
			if (_predicate(_heap[now], _heap[next]))
			{
				break;
			}

			// if the current node is greater than the parent node,
			// swap with the parent node
			std::swap(_heap[now], _heap[next]);
			now = next;
		}
	}

	// O(log n) time complexity for pop
	void pop()
	{
		_heap[0] = _heap.back();
		_heap.pop_back();

		int now = 0;
		while (true)
		{
			int left = 2 * now + 1;
			int right = 2 * now + 2;

			// Check if the left child exists
			if (left >= static_cast<int>(_heap.size()))
			{
				break;
			}

			int next = now;
			// Check if the left child is greater than the current node
			if (_heap[next] < _heap[left])
			{
				next = left;
			}

			// Check if the right child exists and is greater than the current node
			//if (right < static_cast<int>(_heap.size()) && _heap[next] < _heap[right])
			//{
			//	next = right;
			//}
			if (right < static_cast<int>(_heap.size()) && _predicate(_heap[next], _heap[right]))
			{
				next = right;
			}

			// If the current node is greater than both children, break
			if (next == now)
			{
				break;
			}

			std::swap(_heap[now], _heap[next]);
		}
	}

	// O(1) time complexity for top
	T& top()
	{
		return _heap[0];
	}

	// O(1) time complexity for empty
	bool empty()
	{
		return _heap.empty();
	}

private:
	std::vector<T> _heap;
	Predicate _predicate;
};
```

## Graph