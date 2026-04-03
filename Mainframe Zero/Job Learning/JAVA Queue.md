---
tags:
  - Learning
source: https://codesignal.com/learn/paths/mastering-algorithms-and-data-structures-in-java
---

### Problem 1: Queue interleaving
We use 2 auxiliary queue

```Java
Queue<Integer> firstHalf = new LinkedList<>();
Queue<Integer> secondHalf = new LinkedList<>();

while (queue.size() < n / 2) {
	firstHalf.add(queue.remove());
}

while (!queue.isEmpty()) {
	secondHalf.add(queue.remove());
}

while (!firstHalf.isEmpty() || !secondHalf.isEmpty()) {
	if (!firstHalf.isEmpty()) {
		queue.add(firstHalf.remove());
	}
	if (!secondHalf.isEmpty()) {
		queue.add(secondHalf.remove());
	}
}
```

### Problem 2: Calculating moving average from Data stream

```Java
class MovingAvrage {
	int size;
	Queue<Integer> window;
	double sum;
	
	public MovingAvrage(int size) {
		this.size = size();
		this.window = new LinkedList<>();
		this.sum = 0.0;
	}
	
	public double next(int value) {
		if (window.size() == size) {
			sum -= window.remove();
		}
		window.add(value);
		sum += value;
		return sum / window.size();
	}

}
```