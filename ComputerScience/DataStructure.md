# 자료구조와 알고리즘 핵심 개념
- 선형
	- [Array](#array)
	- [Linked List](#linked-list)
	- [Stack](#stack), [Queue](#queue)
- 비선형
	- [Tree](#tree)
	- [Graph](#graph)

---

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

## Linked List
- **특징**:
    - 노드 기반 순차적 데이터 구조
	- 연속적인 메모리 위치에 저장되지 않는 선형 데이터 구조로 포인터를 사용해서 연결된다
	- 각 노드는 데이터 필드와 다음 노드에 대한 참조를 포함하는 노드로 구성된다
- **장점**: 
  - 삽입/삭제가 용이 (**항상 빠르다는 것이 아니라 삽입 삭제할 노드를 알고 있을 경우**)
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
class Node
{
	using T = int;
public:
	Node(T data)
		: data(data), prev(nullptr), next(nullptr) {}

public:
	T data;
	Node* prev;
	Node* next;
};

class CaList
{
public:
	CaList()
	{
		_head = new Node(0);
		_tail = new Node(0);
		_head->next = _tail;
		_tail->prev = _head;
	}
	~CaList()
	{
		Node* node = _head;
		while (node)
		{
			Node* temp = node;
			node = node->next;
			delete temp;
		}
	}

	Node* GetNode(int index)
	{
		Node* node = _head->next;
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
		Node* node = _head->next;
		while (node != _tail)
		{
			std::cout << node->data << " ";
			node = node->next;
		}
		std::cout << std::endl;
	}

	Node* AddAtHead(int data)
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
		Node* node = new Node(data);
		Node* temp = _head->next;
		 
		node->next = temp;
		temp->prev = node;
		_head->next = node;
		node->prev = _head;

		return node;
	}

	Node* AddAtTail(int data)
	{
		/** [dummy]	[1]	[2]	[temp]	[node]	[dummy] */
		/** [head]							[tail]  */
		Node* node = new Node(data);
		Node* temp = _tail->prev;

		temp->next = node;
		node->prev = temp;
		node->next = _tail;
		_tail->prev = node;

		return node;
	}

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

private:
	Node* _head = nullptr;
	Node* _tail = nullptr;
};

int main()
{
	CaList list;
	list.AddAtHead(1);
	Node* temp1 = list.AddAtHead(2);
	list.AddAtHead(3);

	list.AddAtTail(4);
	Node* temp2 = list.AddAtTail(5);
	list.AddAtTail(6);

	list.Insert(temp1, 7);
	list.Remove(temp2);

	list.Print();
}
```

## Stack
- **특징**: LIFO(Last In First Out) 구조
- **장점**: 
  - 구현이 간단
  - 메모리 효율적 사용
- **단점**: 데이터 접근의 제한성

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

## Queue
- **특징**: FIFO(First In First Out) 구조
- **장점**: 
  - 데이터 순서 보장
  - 동시성 처리에 유용
- **단점**: 중간 데이터 접근 불가

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

## Tree

## Graph