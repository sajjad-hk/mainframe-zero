---
tags:
  - Learning
source: https://codesignal.com/learn/courses/advanced-asynchronous-programming-in-java
---

## Executors and Thread Pools

```Java
Executor executor = Executors.newSingleThreadExecutor();
executor.execute(() -> System.out.println("Task executed"))
```

```Java
ExecutorService executor = Executors.newFixedThreadPool(2);

executor.execute(() -> {
	System.out.println("Task 1 executed by ", 
		Thread.currentThread().getName());
});

executor.execute(() -> {
	System.out.println("Task 2 executed by ", 
		Thread.currentThread().getName());
});

executor.shutdown();

try {
	if (!executor.awaitTermination(5, java.util.concurrent.TimeUnit.SECONDS)) {
		executor.shutdownNow();
	}
} catch (InteruptedException ex) {
	executor.shutdownNow();
}
```

## `Future` and `Callable` interface
