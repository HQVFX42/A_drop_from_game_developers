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
std::vector<int> v{0, 11, 22, 33, 44, 55, 66};

int BinarySearch(int N)
{
	int left = 0;
	int right = v.size() - 1;

	while (left <= right)
	{
		int mid = (left + right) / 2;

		if (n < v[mid]) { right = mid - 1; }
		else if (n > v[mid] { left = mid + 1; }
		else { return mid; }
	}

	return -1;
}
```
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