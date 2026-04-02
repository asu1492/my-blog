[[Study Right now]]


---
![[Pasted image 20260314184756.png]]

Collections API (List → Set → Map) → Iterator/Iterable → Comparator → Stream API (Intermediate → Terminal → Collectors) → Optional → Concurrent Collections → Parallel Streams → Generics deep dive

---

###### Map 

HashMap
1. compute() - use when new value depends on oldValue 
2. merge() -> merging old and new value ; combining two maps 
3. computeIfAbsent()
4. computeIfPresent() 



##### Sets

TreeSet 
1. Keeps the elements sorted and prevents duplicate 


##### Sets

HashMap
1. Store/Access element as name/value pair 


LinkedHasMap 
1. Along with HasMap Attibutes, can remember the order in which elements(name/value pair) were inserted 
###### Intermediate Operation
1. Stateless 
	1. map
	2. flatmap 
	3. filter
	4. peek 
	5. mapMulti() (Java 16+)
2. Stateful 
	1. sorted 
	2. distinct 
	3. limit
	4. skip 
	5. takeWhile (Java 9+)
	6. dropWhile (Java 9+)
3. Primitive Stream Mapping Operation 
	1. mapToInt()  
	2. mapToLong()  
	3. mapToDouble()  
	4. flatMapToInt()  
	5. flatMapToLong()  
	6. flatMapToDouble()
	7. `mapToObject()` / `boxed()` _(primitive → object)_

###### Terminal Operation
1. Aggregation 
	1. Count
	2. Collect 
	3. Reduce
	4. min()
	5. max()
	6. sum() (primitive streams only)
	7. average() (primitive streams only)
	8. summaryStatistics() (primitive streams only)
2. Search 
	1. FindFirst
	2. findAny
	3. allMatch()  
	4. noneMatch()
	5. AnyMatch()
3. Iterator 
	1.  ForEach 
	2. forEachOrdered
4. Conversion Operations
	1. toArray
	2. `toList()` — added in Java 16



Conversation Model : ChatSession.java
Map<String, Object> sessionParams, DataResponse 
Map<String, ChatSession> childSession
private final Map<String, Agent> botMap = new HashMap<>();

List<ChatResponseDTO> chatResponse
List<String> fullChat = new ArrayList<>();

Set<String> tags 

Collections.emptyMap() as safe default value 
Collections.singletonList(...)
Collections.emptyList()
Streams: agents.stream.map(..).collect(Collectors.toList())

If your goal is to “learn Collections using this codebase”, I’d suggest this order:
1. Start simple:
    - Read `Agent` and `ChatSession` to see how `List`, `Map`, and `Set` are used as fields and how getters/setters work.
2. Move to usage patterns:
    - Read the body of `ChatController.chat()` and `singleTurnChat()` for simple `Map`/`List` usage with `Collections.emptyMap()`.
3. Study transformation code:
    - Read `FunctionCaller.createTools()` and related private methods to see:
        - Replacing `null` with `Collections.emptyList()`.
        - Building new `List`s via `new ArrayList<>()` and streams.
4. Look at caches:
    - Read `BotCache` to see a `Map` used as an in-memory cache.
5. Practice:
    - Pick one class (e.g. `FunctionCaller`) and re-implement a method in a scratch file:
        - First using plain loops.
        - Then rewrite using `stream()`/`Collectors.toList()`

