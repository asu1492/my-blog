
###### What is an API
1. Interface that enables two software components to communicate with each other using set of definitions and protocols.
2. REST are built around Resources(Resource Modeling). Resources are just core entities. 
3. Resources as plural nouns, e.g. /events/{id}
4. ![[Pasted image 20260228205619.png]]
5. REST
	1. HTTP Methods 
		1. GET - retrieve data 
		2. POST - create data but non idempotent 
		3. PUT - idempotent update/replace
		4. PATCH - partial update
		5. DELETE 
	2. Inputs
		1. Path Params - required - specifies which resources you're working with e.g. /events/{id}
		2. Query Params -optional filters - ?city=LA&date=&time=
		3. Request Body 
	3. Response 
		1. Status Code [2xx, 4xx, 5xx]
		2. Response Body
			1. /events -> Events[]
6. GraphQL 
	1. Why ? Meta - showing mobile network, multiple calls & over/under fetching
	2. Lower latency than REST and can club multiple request entities into single one - in case of REST we may end up calling multiple APIs
	3. Also, enable authorization at attribute level - check permission field by field 
	4. ![[Pasted image 20260228211853.png]]
	5. Tradeoffs 
		1. N+1 total queries -> Solution: Data Loader to batch queries 
7. RPC
	1. for internal micro-services communication 
	2. very efficient 
	3. RPC with Protocol Buff uses compact binary formats, cutting JSON and HTTP overhead. 
8. Realtime Protocol - websocket, server sense events, web RTC; these creates open continuous connection where we can stream data b/w client and the server. E.g. chat app. 
9. Pagination
	1. When REST endpoint will returns million resources/records 
	2. This will lead to high latency 
	3. Two types 
		1. Offset/Page Based - e.g. ?page=1&size=25
		2. Cursor-based 
			1. /events/cursor=123&limit=25 [cursor is last item]
10. Security 
	1. via JWT which carries signed claims(role,expiry)
	2. Also, it is not best practice to send userId in request body, e.g. {"userId", "text"}, userId should be inferred from JWT Token 