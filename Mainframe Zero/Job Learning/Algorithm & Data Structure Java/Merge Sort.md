```Java
void merge(int[] arr, int left, int mid, int right) {
	int n1 = (mid - start) + 1;
	int[] arr1 = new int[n1];
	
	int n2 = end - mid;
	int[] arr2 = new int[n2];
	
	for (int i = 0; i < n1; i++) {
		arr1[i] = arr[left + i];
	}
	for (int j = 0; j < n2; j++) {
		arr2[j] = arr[mid + 1 + j];
	}
	 
	 int i = 0, j = 0;
	 int k = left;
	 while (i < n1 && j < n2) {
		 if (arr1[i] < arr2[j]) {
			 arr[k] = arr1[i++];
		 } else {
			 arr[k] = arr2[j++];
		 }
		 k++;
	 }
	 
	 while (i < n1) {
		 arr[k] = arr1[i++];
		 k++;
	 }
	 
	 while (j < n2) {
		 arr[k] = arr2[j++];
		 k++;
	 }
}
```

```Java
void mergeSort(int[] arr, int left, int right) {
	if (left < right) {
		int mid = (left + right) / 2;
		mergeSort(arr, left, mid);
		mergeSort(arr, mid + 1, right);
		merge(arr, left, mid, right);
	}
}
```