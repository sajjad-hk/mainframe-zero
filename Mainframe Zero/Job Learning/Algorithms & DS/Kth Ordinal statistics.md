### Problem 1: Find k-th ordinal statistic in a list

Using merge sort:

```Java
int findKthSmallestElement(int[] numbers, int k) {
	if (numbers == null || numbers.length < k) {
		Integer.MIN_VALUE;
	}
	return kthSmallest(numbers, start, end, k);
}

int kthSmallest(int[] arr, int start, int end, int k) {
	if (k > 0 && k < end - start + 1) {
		int pos = partition(arr, start, end);
		if (pos - start == k - 1) {
			return arr[pos];
		}
		
		if (pos - start > k - 1) {
			return kthSmallest(arr, start, pos - 1, k);
		}
		return kthSmallest(arr, pos + 1, end, k - pos + start + 1);
	}
	return Integer.MAX_VALUE;
}
```

```Java
public int findKthLargest(int[] numbers, int k) {
	if(numbers == null || numbers.length < k)
		return Integer.MIN_VALUE;
	return kthLargest(numbers, 0, numbers.length - 1, k);
}

static int kthLargest(int[] arr, int start, int end, int k) {
	if (k > 0 && k <= end - start + 1) {
		int pos = partition(arr, start, end);
		if (pos - start == k - 1) return arr[pos];
			if (pos - start > k - 1) {
				return kthLargest(arr, start, pos - 1, k);
			}
			return kthLargest(arr, pos + 1, end, k - (pos - start + 1));
	}
	return Integer.MAX_VALUE;
}

  
int partition(int[] arr, int start, int end) {
	int pivot = arr[start];
	int i = start;
	for(int j = start + 1; j <= end; j++) {
		if(arr[j] >= pivot) {
			i++;
			int temp = arr[i];
			arr[i] = arr[j];
			arr[j] = temp;
		}
	}
	int temp = arr[i];
	arr[i] = arr[start];
	arr[start] = temp;
	return i;
}
```
### Problem 2: Count the number of Inversions in a list

```Java
class Result {

    public int[] sorted;

    public long inversions;

  

    public Result(int[] sorted, long inversions) {

        this.sorted = sorted;

        this.inversions = inversions;

    }

}

  

class CountInversions {

    public Result countInversions(int[] arr) {

        if (arr.length <= 1) {

            return new Result(arr, 0);

        }

        int middle = arr.length / 2;

        Result left = countInversions(Arrays.copyOfRange(arr, 0, middle));

        Result right = countInversions(Arrays.copyOfRange(arr, middle, arr.length));

        Result result = mergeAndCountInversions(left.sorted, right.sorted);

        // implement this

        return new Result(result.sorted, left.inversions + right.inversions + result.inversions);

    }

  

    private Result mergeAndCountInversions(int[] left, int[] right) {

        int[] merged = new int[left.length + right.length];

        int i = 0, j = 0;

        long inversions = 0;

        for (int k = 0; k < merged.length; k++) {

            if (i < left.length && (j >= right.length || left[i] <= right[j])) {

                merged[k] = left[i++];

            } else {

                merged[k] = right[j++];

                inversions += left.length - i;

            }

        }

        return new Result(merged, inversions);

    }
```