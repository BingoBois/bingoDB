# ImportData

# 1
Mysql
```
SELECT distinct Author.name, Book.title FROM LocationInBook
INNER JOIN Book ON Book.id=fk_Book
INNER JOIN BookWrittenBy ON LocationInBook.fk_Book=BookWrittenBy.fk_Book
INNER JOIN Author ON Author.id=BookWrittenBy.fk_Author
WHERE fk_Location=?;'
```
Neo4j
```
match(b: Book)-[m:Mentions]->(l:Location {name: $1})
match(a:Author)-[w: Wrote]->(b)
return b, m, l, a, w;
```

# 2
Mysql
```
SELECT Book.title, Location.`name`, latitude, longitude FROM Book
INNER JOIN LocationInBook ON LocationInBook.fk_Book = Book.id
INNER JOIN Location on Location.`name`=LocationInBook.fk_Location
WHERE title = ?;
```
Neo4j
```
match(b:Book {title: $1})-[m:Mentions]->(l:Location)
return b, m, l
```

# 3
Mysql
```
SELECT Book.title, Location.`name`, latitude, longitude FROM Book
INNER JOIN LocationInBook ON LocationInBook.fk_Book = Book.id
INNER JOIN Location on Location.`name`=LocationInBook.fk_Location
INNER JOIN BookWrittenBy on BookWrittenBy.fk_Book = Book.id
INNER JOIN Author on Author.id = BookWrittenBy.fk_Author
WHERE Author.`name`=  "Boccaccio, Giovanni";
```
Neo4j
```
match(a:Author {name: "Longfellow, Henry Wadsworth"})-[m:Wrote]->(b: Book)
match(b1:Book {title: b.title})-[m1:Mentions]->(l:Location)
return b1, m1, l;
```

# 4
Mysql
```
SELECT Book.title FROM Location 
INNER JOIN LocationInBook ON LocationInBook.fk_Location = Location.`name`
INNER JOIN Book ON Book.id = LocationInBook.fk_Book
WHERE longitude <= 20+10 AND longitude >= 20-10 AND latitude <= 20+10 AND latitude >= 20-10;
```
Neo4j
```
match(b:Book)-[:Mentions]->(l:Location) 
WHERE toFloat(l.latitude) > 30 AND toFloat(l.latitude) < 50 AND toFloat(l.longitude) > 30 AND toFloat(l.longitude) < 50
return b.title LIMIT 500;
```
