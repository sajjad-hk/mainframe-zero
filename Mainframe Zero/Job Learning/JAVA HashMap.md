1. Stores Key / Value pairs
2. Allows `null` value and `null` key
3. Doesn't guarantee any specific map order 

## Complexity Analysis
- Time complexity: O(1) for `get`  and `put`
- Space complexity: O(n)

### Problem 1: Word Counter

```Java
HashMap<String, Integer> wordCount = new HashMap<>();
String[] words = text.split(" ");
for (String word: words) {
	wordCount.put(word, wordCount.getOrDefault(word, 0) + 1)
}
```

### Problem 2: Sum of the mapped values
```Java
HashMap<String, Integer> map = new HashMap<>();

int sum = 0;
for (int value: map.values()) {
	sum += value;
}
return sum;
```

## Efficient problem solving

### Problem 1: Majority element Finder
```Java
HashMap<String, Integer> countMap = new HashMap<>();
int majorityThreshold = arr.length / 2;

for (int num: arr) {
	countMap.put(num, countMap.getOrDefault(num, 0) + 1);
	if (countMap.get(num) > majorityThreshold) {
		return num;
	}
}
return -1;
```

### Problem 2: Keyword Index Creator 

```Java
HashMap<String, List<Integer>> index = new HashMap<>();

for (int i = 0; i < docs.length; i++) {
	String[] words = docs[i].split(" ");
	for (String word: words) {
		List<String> items = index.getOrDefault(word, new ArrayList<>());
		items.add(word);
		index.put(word, items);
	}
}

return index;
```