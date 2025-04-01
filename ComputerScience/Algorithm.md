# Algorithm

## Sorting

### Bubble Sort
- Time Complexity: O(n^2)
- Sample Code:
```cpp
void BubbleSort(int arr[], int n)
{
	for (int i = 0; i < n - 1; i++)
	{
		for (int j = 0; j < n - i - 1; j++)
		{
			if (arr[j] > arr[j + 1])
			{
				int temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
		}
	}
}

void OptimizedBubbleSort(int arr[], int n)
{
	bool swapped;
	for (int i = 0; i < n - 1; i++)
	{
		swapped = false;
		for (int j = 0; j < n - i - 1; j++)
		{
			if (arr[j] > arr[j + 1])
			{
				int temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
				swapped = true;
			}
		}
		if (swapped == false) break;
	}
}
```

## Binary Search
- Time Complexity: O(log n)
- Sample Code:
```cpp
int BinarySearch(int arr[], int n, int target)
{
	int left = 0;
	int right = n - 1;
	while (left <= right)
	{
		int mid = left + (right - left) / 2;

		if (arr[mid] == target) return mid;
		if (arr[mid] < target) left = mid + 1;
		else right = mid - 1;
	}

	return -1;
}
```

## Recursive

### **1. 재귀 함수의 본질**
- 재귀 함수란 단순히 "자기 자신을 호출하는 함수"가 아님.
- **정확한 설명**: 재귀 함수는 동일한 코드가 재활용되어, 같은 이름이지만 서로 다른 함수로 동작하는 구조.
  - 즉, **코드만 동일하고, 호출된 각각의 함수는 서로 독립적인 공간**을 가짐.
  - 이를 이해하면 스택(Stack) 구조와 함수 호출 과정을 명확히 파악할 수 있다.

### **2. 함수 호출과 스택 구조**
- 모든 함수 호출은 **스택(Stack)**에 기록.
  - 새로운 함수가 호출될 때마다 스택에 쌓이고, 반환(return) 시 해당 함수가 스택에서 제거.
  - 가장 마지막에 호출된 함수가 먼저 종료되는 **LIFO(Last In, First Out)** 구조를 따름.