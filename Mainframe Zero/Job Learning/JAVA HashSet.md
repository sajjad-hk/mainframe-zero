---
tags:
  - Learning
source: https://codesignal.com/learn/paths/mastering-algorithms-and-data-structures-in-java
---

1. HashSet is part of Java's Collections framework
2. It stores unique elements in an unordered manner 
3. It provides efficient membership checks and remove duplicate elements
## Complexity analysis
- Time complexity O(1): `add`, `contains`, `remove`
- Space complexity: Linier, O(n)
### Problem 1: Check if two Sets are disjoint

```Java
HashSet<Integer> set1 = new HashSet<>();
for (int num: arr1) {
	set1.add(num);
}

for (int num: arr2) {
	if (set1.contains(num)) {
		return false;
	}
}
return true;
```

### Problem 2: Remove duplicates from an array
```Java
HashSet<Integer> set = new HashSet<Integer>();
for (int num: arr) {
	set.add(num);
}

int[] result = new int[](set.size());
int i = 0;
for (int num: set) {
	result[i++] = num;
}
return result;
```

## Unique elements and Anagram detection
### Problem 1: Unique Echo
```Java
HashSet<String> wordsSet = new HashSet<>();
HashSet<String> duplicatesSet = new HashSet<>();

for (String word: words) {
	if (wordsSet.contains(word)) {
		duplicatesSet.add(word);
	} else {
		wordsSet.add(word);
	}
}
wordsSet.removeAll(duplicatesSet);
String lastUniqueWord;
for (int i = words.length - 1; i >= 0; i--) {
	if (wordsSet.contains(words[i])) {
		lastUniqueWord = words[i];
		break;
	}
}
return lastUniqueWord;
```

### Problem 2: Anagram Matcher
i.e. "Listen" and "Silent" are anagrams.

```Java
String sortCharacters(String input) {
	char[] chars = input.toCharArray();
	Arrays.sort(chars);
	return new String(chars);
}

HashSet<String> sortedWordsInArray2 = new HashSet<>();

for (String word: array2) {
	sortedWordsInArray2.add(sortCharacters(word));
}

ArrayList<String> result = new ArrayList<>();
HashSet<String> anagramMatched = new HashSet<>();

for (String word: array1) {
	if (sortedWordsInArray2.contains(sortCharacters(word))) {
		if (!anagramMatched.cotains(word)) {
			result.add(word);
			anagramMatched.add(word);
		}
	}
}

return result;
```
