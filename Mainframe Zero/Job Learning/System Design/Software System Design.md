---
tags:
  - Learning
  - NOT-DONE
source: https://codesignal.com/learn/paths/mastering-algorithms-and-data-structures-in-java
---

# Software System Design
## Caching 

### Requirements

#### Functional
- PUT (key, value)
- GET (key): value
- Algorithm to use for evicting data (cached data is expensive and storage is not unlimited) 
	- LRU: Last Recent Used
	- FIFO
	- LIFO
#### Non-Functional
* Performance
* Scalability
* Availability

### Data structure selection 
* HashMap for storing the cache data
* LinkedList for implementation of LRU 
### Hashing 
* Consistent Hashing
* Jump Hashing

