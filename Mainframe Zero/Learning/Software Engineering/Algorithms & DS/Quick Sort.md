```Java
int partition(int[] arr, int start, int end) {
	int pivot = arr[end];
	int i = start - 1;
	for (int j = start; j <= end; j++) {
		if (arr[j] <= pivot) {
			i++;
			
			int temp = arr[i];
			arr[i] = arr[j];
			arr[j] = temp;
		}
	}
	int temp = arr[i + 1];
	arr[i + 1] = arr[end];
	arr[end] = temp;
	
	return i + 1;
}
```

```Java
void quickSort(int[] arr, int start, int end) {
	if (start < end) {
		int partitionIndex = partition(arr, start, end);
		quickSort(arr, start, partitionIndex - 1);
		quickSort(arr, qartitionIndex + 1, end);
	}
}
```

```Java
static int partition(int[] arr, int start, int end) {
	int pivot = arr[start];
	int i = start;
	for (int j = start + 1; j <= end; j++) {
		if (arr[j] < pivot) {
			i++;
			int temp = arr[i];
			arr[i] = arr[j];
			arr[j] = temp;
		}
	}
	arr[start] = arr[i];
	arr[i] = pivot;
	return i;
}
```