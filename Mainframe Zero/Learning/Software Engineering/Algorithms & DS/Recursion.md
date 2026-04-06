## Calculate Factorial of a number

```Java
int factorial(int n) {
	if (n == 1) {
		return 1;
	}
	return n * factorial(n - 1);
}
```

## Calculate sum of digits

```Java
int sumOfDigits(int n) {
	if (n < 10) {
		return n;
	}
	return n % 10 + sumOfDigits(n / 10);
}
```

## Binary Search recursive
```Java
int binarySearch(int[] sortedArray, int start, int end, int target) {
	if (start > end) return -1;
	int mid = start + (end - start) / 2;
	if (sortedArray[mid] == target) {
		return mid;
	}
	if (sortedArray[mid] > target) {
		return binarySearch(sortedArray, start, mid - 1, target);
	}
	return binarySearch(sortedArray, mid + 1, end, target);
}
```

### Time Complexity
O(log(n))

## Binary Search iteratively

```java
int binarySearch(int[] array, int target) {
	int start = 0;
	int end = array.length - 1;
	while (start <= end) {
		int mid = start + (end - start) / 2;
		if (array[mid] == target) {
			return mid;
		}
		if (array[mid] > target) {
			end = mid - 1;
		} else {
			start = mid + 1;
		}
	}
	return -1;
}
```

## Using Binary Search for advanced data structure

### Problem 1: Searching in Bitonic array 
1. Find the peak in the bitonic array.
```Java
int findPeak(int[] tempratures) {
	int low = 0, high = tempratures.length - 1;
	while (low < high) {
		int mid = low + (high - low) / 2;
		if (tempratures[mid] > temratures[mid + 1]) {
			high = mid;
		} else {
			low = mid + 1;
		}
	}
	return low;
}
```
2. Modify binary search to be applicable for ascending or descending array. 
```Java
int binarySearch(
	int[] tempratures, 
	int low, 
	int high, 
	int target, 
	boolean ascending
) {
	while (low <= high) {
		int mid = low + (high - low) / 2;
		if (tempratures[mid] == target) {
			return mid;
		}
		if (ascending ? tempratures[mid] < target : tempratures[mid] > target) {
			low = mid + 1;
		} else {
			high = mid - 1;
		}
	}
	return -1;
}
```
3. Implementation of Bitonic search
```Java
int bitonicSearch(int[] tempratures, int target) {
	int peakIndex = findPeak(tempratures);
	int result = binarySearch(tempratures, 0, peakIndex, target, true);
	if (result != -1) {
		return result;
	} else {
		return binarySearcdh(
			temprature,
			peakIndex + 1,
			tempratures.length - 1,
			target,
			false
		);
	}
}
```

### Problem 2: Minimum element in a Rotated Sorted Array

```Java
int findMin(int[] arr) {
	int left = 0, right = arr.length - 1;
	while (left < right) {
		int mid = left + (right - left) / 2;
		if (arr[mid] > arr[right]) { 
			left = mid + 1;
		} else { 
			right = mid;
		}
	}
	return arr[left];
}
```
