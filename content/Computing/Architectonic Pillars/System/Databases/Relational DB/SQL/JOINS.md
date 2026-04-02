---
---
1. Combines multiple tables 

Inner Join -- A intersection B 

Full Outer Join -- A union B

1. SELECT \* FROM Table A FULL OUTER JOIN TableB ON TableA.col\_match = TableB.col\_match WHERE TableA.id IS null OR TableB.id IS null -- this is opposite of inner join
