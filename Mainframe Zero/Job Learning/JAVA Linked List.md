---
tags:
  - Learning
source: https://codesignal.com/learn/paths/mastering-algorithms-and-data-structures-in-java
---

## Reverse Traversal Linked List

```Java
Stack<Integer> stack = new Stack<>();

ListNode currentNode = head;

while (currentNode != null) {
	stack.push(currentNode.next);
	currentNode = currentNode.next;
}

while (!stack.isEmpty()) {
	System.out.println(stack.pop());
}

```

## Calculate Linked List length

```Java
int length = 0;

ListNode currentNode = head;
while (currentNode != null) {
	length++;
	currentNode = currentNode.next
}

return length;
```

## Eliminating duplicates in Linked List

```Java
ListNode removeDuplicates(ListNode head) {
	if (head == null || head.next == null) {
		return head;
	}
	HashSet<Integer> seenBooks = new HashSet<>();
	ListNode current = head;
	seenBooks.add(current.value);
	
	while (current.next != null) {
		
		if (seenBooks.contains(current.next.value)) {
			current.next = current.next.next;
		} else {
			seenBooks.add(current.next.value);
			current = current.next;
		}
	}
	return head;
}
```

## Find average of every third elements

```Java
double averageEveryThirdElements(ListNode head) {
	if (head == null || head.next ==  null || head.next.next == null) {
		return 0.0;
	}
	double sum = 0;
	int count = 0;
	ListNode current = head;
	for (int index = 1; current != null; current = current.next, index++) {
		if (index % 3 == 0) {
			sum += current.value;
			count++;
		}
	}
	return (double) sum / count;
}
```