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
- **특징**: 노드 기반 순차적 데이터 구조
- **장점**: 
  - 삽입/삭제가 용이
  - 동적 크기 조절 가능
- **단점**: 
  - 임의 접근 불가
  - 추가 메모리 공간 필요

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