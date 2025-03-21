# 자료구조와 알고리즘 핵심 개념

## 1. 기본 자료구조 특징

### Array
- **특징**: 연속된 메모리 공간에 순차적 저장
- **장점**: 
  - 인덱스를 통한 빠른 접근 (O(1))
  - 메모리 효율적 사용
- **단점**: 
  - 크기 변경 불가
  - 삽입/삭제 시 O(n) 시간 소요
- **용도**: 데이터 크기가 고정적이고 빠른 접근이 필요한 경우

#### Reverse
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

#### Split
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

### List
- **특징**:
    - 노드 기반 순차적 데이터 구조
	- 연속적인 메모리 위치에 저장되지 않는 선형 데이터 구조로 포인터를 사용해서 연결된다
	- 각 노드는 데이터 필드와 다음 노드에 대한 참조를 포함하는 노드로 구성된다
- **장점**: 
  - 삽입/삭제가 용이
  - 동적 크기 조절 가능
- **단점**: 
  - 임의 접근 불가
  - 추가 메모리 공간 필요
- Sample Code:
	```cpp
	/**
	 * Single Linked List.
	 */
	struct Node
	{
		int data;         // 노드의 데이터
		Node* next;       // 다음 노드를 가리키는 포인터
	};

	class LinkedList
	{
	private:
		Node* head;       // 연결 리스트의 첫 번째 노드

	public:
		LinkedList() : head(nullptr) {}  // 기본 생성자

		// 연결 리스트에 노드를 삽입하는 함수
		void insert(int data);

		// 연결 리스트에서 노드를 삭제하는 함수
		void remove(int data);

		// 연결 리스트에서 특정 값을 검색하는 함수
		bool search(int data);

		// 연결 리스트를 출력하는 함수
		void display();
	};

	void LinkedList::insert(int data)
	{
		Node* newNode = new Node();  // 새로운 노드 생성
		newNode->data = data;        // 데이터 할당
		newNode->next = head;        // 새로운 노드의 next 포인터를 현재 head로 설정
		head = newNode;              // head를 새로운 노드로 업데이트함
	}

	void LinkedList::remove(int data)
	{
		if (head == nullptr) return;  // 리스트가 비어 있을 경우

		if (head->data == data)
		{     // 삭제할 노드가 head일 경우
			Node* temp = head;
			head = head->next;
			delete temp;
			return;
		}

		Node* curr = head;
		while (curr->next != nullptr)
		{
			if (curr->next->data == data)
			{
				Node* temp = curr->next;
				curr->next = curr->next->next;
				delete temp;
				return;
			}
			curr = curr->next;
		}
	}

	bool LinkedList::search(int data)
	{
		Node* temp = head;
		while (temp != nullptr)
		{
			if (temp->data == data) return true;
			temp = temp->next;
		}
		return false;
	}

	void LinkedList::display()
	{
		Node* temp = head;
		while (temp != nullptr)
		{
			std::cout << temp->data << " ";
			temp = temp->next;
		}
		std::cout << std::endl;
	}

	int main() {
		LinkedList list;

		// 노드 삽입
		list.insert(6);
		list.insert(9);
		list.insert(1);
		list.insert(3);
		list.insert(7);

		std::cout << "Current list: ";
		list.display();

		// 노드 삭제
		std::cout << "Remove 1: ";
		list.remove(1);
		list.display();

		// 값 검색
		std::cout << "Search 7: " << (list.search(7) ? "true" : "false") << std::endl;
		std::cout << "Search 13: " << (list.search(13) ? "true" : "false") << std::endl;

		return 0;
	}
	```

### Stack
- **특징**: LIFO(Last In First Out) 구조
- **장점**: 
  - 구현이 간단
  - 메모리 효율적 사용
- **단점**: 데이터 접근의 제한성

### Queue
- **특징**: FIFO(First In First Out) 구조
- **장점**: 
  - 데이터 순서 보장
  - 동시성 처리에 유용
- **단점**: 중간 데이터 접근 불가