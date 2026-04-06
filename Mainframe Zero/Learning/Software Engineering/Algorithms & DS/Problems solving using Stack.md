### 1.

```Java
Stack<Character> stack = new Stack<>();

Map<Character, Character> bracketsMatches = new HashMap<>();
bracketsMatches.put('(', ')');
bracketsMatches.put('[', ']');
bracketsMatches.put('{', '}');

Set<Character> opens = new HashSet<>();
opens.add('(');
opens.add('[');
opens.add('{');

for (int i = 0; i < input.length; i++) {
	Character a = input.chatAt(i);
	if (opens.contains(a)) {
		stack.push(a);
	} else if (!stack.isEmpty() && a == bracketMatches.get(stack.peek())) {
		stack.pop();
	} else {
		return false;
	}
	return stack.isEmpty();
}

```

### 2.
```Java
Stack<Character> stack = new Stack<>();

for (Character c: input.toCharArray()) {
	stack.push(c);
}
StringBuilder str = new StringBuilder();
while(!stack.isEmpty()) {
	str.append(stack.pop());
}

return str.toString();

```


### 3. proceeding smaller element

```Java
int[] findProceedingSmallerElement(int[] arr) {
	Stack<Integer> stack<>();
	int[] result = new int[arr.length];
	for (int i = 0; i < arr.length; i++) {
		while (!stack.isEmpty() && arr[i] < stack.peek()) {
			stack.pop();
		}
		result[i] = stack.isEmpty() ? -1 : stack.peek();
		stack.push(arr[i]);
	}
	return result;
}

```

### 4. Stack with minimum tracing

```Java
class MinStack {
	Stack<Integer> stack = new Stack<>();
	Stack<Integer> minValues = new Stack<>();
	
	void push(int value) {
		if (minValues.isEmpty() || value <= minValues.peek()) {
			minValues.push(value);
		}
		stack.push(value);
	}
	
	void pop() {
		if (!stack.isEmpty() && stack.peek().equals(minValues.peek())) {
			minValue.pop();
		}
		if (!stack.isEmpty()) {
			stack.pop();
		}
	}
	
	
}
```