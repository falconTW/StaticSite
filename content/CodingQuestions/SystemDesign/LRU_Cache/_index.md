---
title: "LRU Cache"
date: 2018-09-24T23:38:43+08:00
---

[Questions Link](https://leetcode.com/problems/lru-cache/description/) 

Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.

put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, 

it should invalidate the least recently used item before inserting a new item.

Follow up:
Could you do both operations in O(1) time complexity?

Example:

```
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

## Solution

#### Using hashtable -> TLE

Using input key as the key of the map and the value is composed of the value and a time stamp.

The least being used is the value, the smaller is the time stamp. 

```
get(int key) :  Set time of target entry to 0, and the time of the rest entries are deducted by one.
```
```
put(int key) : If capacity is not yet full,insert the key and value, set time to zero, 

               or delete the entry with the smallest time and insert the new one. 
```

Time complexity is O(1) when getting value but deducting time is time wasting that leads to TLE.

#### Using Linkedlist ->TLE

Search for target entry is time wasting, not O(1).

#### Using DoublyLinkedlist + Hashtable

Double-linked list along with Hashtable can ensure all the operations are O(1) complexity;

When we are trying to get the value, we visit the map, if there is need to modify the doubly linkedlist,

we don't need to search from the very beginning.

```C++
struct LRUElement
{
	int key;
	int value;
	LRUElement* prev;
	LRUElement* next;
	LRUElement() :key(0), value(0), prev(NULL), next(NULL) {};
};

class LRUCache 
{
	private:
		int _capacity;
		int _count;
		LRUElement* head;
		LRUElement* tail;
		unordered_map<int, LRUElement*> map;

		void _DetachNode(LRUElement* node)
		{
			node->prev->next = node->next;
			node->next->prev = node->prev;
		}

		void _MoveToFront(LRUElement* node)
		{
			node->next = head->next;
			node->prev = head;
			head->next = node;
			node->next->prev = node;
		}

		void _RemoveTailNode()
		{
			LRUElement* node = tail->prev;
			_DetachNode(node);
			map.erase(node->key);
			--_count;
		}

	public:
		LRUCache(int capacity)
		{
			_capacity = capacity;
			_count = 0;
			head = new LRUElement();
			tail = new LRUElement();
			head->prev = NULL;
			head->next = tail;
			tail->prev = head;
			tail->next = NULL;
		}

		int get(int key)
		{
			if (map.find(key) == map.end())
			{
				return -1;
			}
			else
			{
				LRUElement* node = map[key];
				_DetachNode(node);
				_MoveToFront(node);
				return node->value;
			}
		}

		void put(int key, int value)
		{
			if (map.find(key)==map.end())
			{
				LRUElement* node = new LRUElement();
				if (_count == _capacity)
				{
					_RemoveTailNode();
				}

				node -> key = key;
				node->value = value;
				map[key] = node;
				_MoveToFront(node);
				++_count;
			}
			else
			{
				LRUElement* node = map[key];
				_DetachNode(node);
				node->value = value;
				_MoveToFront(node);
			}
		}
};
```


