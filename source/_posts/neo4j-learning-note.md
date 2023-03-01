---
title: neo4j介绍
date: 2018-08-17 18:28:18
categories: 
  - [工程]
tags:
   - [neo4j]
   - [图]
---


# neo4j 图数据库介绍

1. neo4j 图由点和边组成。点和边都可以与有各自的属性和label。
2. 一个简单的例子：人居住在城市中、城市是国家的一部分。
3. 节点：用()来表示
4. neo4j 分成社区版本和专业版。
   1. [社区版](https://neo4j.com/docs/operations-manual/current/tools/neo4j-admin/neo4j-admin-store-info/#neo4j-admin-store-standard)节点和关系支持数量均是 $2^{35}$  
   2. [专业版](https://neo4j.com/docs/operations-manual/current/tools/neo4j-admin/neo4j-admin-store-info/#neo4j-admin-store-high-limit)节点和关系支持数量均是 $2^{50}$

## node表示
```
() 表示节点
(p:Person)  p表示 节点变量； Person表示 标签
(p:Person {name: 'tom'})  {}表示属性； name 是tom


1. (matrix)：表示 一个名字叫matrix 的节点
2. (matrix:Movie)一个名字叫matrix 的节点，label 叫 Movie；
3. (matrix:Movie{title:"The Matrix"})一个名字叫matrix 的节点,label叫Movie,属性值title为The Matrix
```
## 关系
```sql
两个节点之间的关系使用箭头表示： arrow --> or <--
无向图关系用 -- 表示
 
(p1:Person)-[rel:IS_FRIENDS_WITH {since: 2018} ]->(p2:Persion)  rel表示关系变量； IS_FRIENDS_WIT 表示标签; 属性：since：2018

1. -- 表示无向
2. -->、<-- 表示有向图
3. -[:ACTED_IN]->: 有向、label
4. -[role:ACTED_IN]-> 有向、label、名字
5. -[role:ACTED_IN {roles: ["Neo"]}]-> 有向、label、名字、属性
```


## 常用的语句

```
1. CREATE (:Movie { title:"The Matrix",released:1997 })
2. CREATE (p:Person { name:"Keanu Reeves", born:1964 }) RETURN p
3. MATCH (m:Movie) RETURN m
4. MATCH (p:Person { name:"Keanu Reeves" }) RETURN p
```

# Cypher 简介

1. Cypher 读音[ˈsaifə] ,
2. 几个常用的 key，用来读取图中数据
3. - MATCH：模式匹配，在图中查询数据最常用的方式
   - WHERE：增加限制对一个模式，使用方式同SQL中的WHERE
   - RETURN：返回结果
4. 几个常用的key ，用来更新图：
   - CREATE(或者DELETE)：创建节点，或者删除关系
   - SET(或者 REMOVE)：给 属性设定值，或者给node 增加label。当移除他们的时候，使用 REMOVE
   - MERGE：匹配已经存在的或者创建新的节点和模式。

```sql
1. create (n:Person {name:'xiaohong',age:16})-[a:friend]->(m:Person {name:'xiaobai',age:18})
2. MATCH (john:Person {name: 'xiaohong'})-[:friend]->(fof:Person) RETURN john.name, fof.name
3. MATCH (user)-[:friend]->(follower) WHERE user.name IN ['Joe', 'xiaohong', 'Sara',      'Maria', 'Steve'] RETURN user.name, follower.name
4. match (n:Person)-[:friend]->(m:Person)  set n.count_m = 10 return n,count_m
5. merge (n:Person {name:'xiaohong',age:16})
```



## Querying and updating the graph

1. 如果你读图中的数据，然后更新图的信息。这个查询实际上被分成两部分：首先是读数据，然后是写入图
2. 如果你的查询仅仅执行读操作，Cypher 不会去真正得查询数据，直到你要求返回结果的时候，才会执行。这是Cypher将会是lazy model，类似spark
3. WITH：类似一条水平线分开了整个执行计划。基本可以理解成，先执行上面，有了上面的前提，再执行下面

```sql
match (n:Person)-[:friend]->(m:Person) 
with n,count(m) as count_m 
set n.count_m = 10
return n,count_m
```

## Returning data

1. RETURN：有3个关键字：SKIP/LIMIT/ORDER BY

## Transactions

1. 任何查询（更新）都会运行在一个事物中，一个更新操作，要么全部成功，要么全部不成功

## Uniqueness

1. A 是B 的朋友，B是C的朋友 。那么查询 A的朋友 的朋友 。返回值是C 不会是A的。
2. 但是上面的查询，如果使用两个MATCH 来查询，将会返回 A 和C。
3. 如果用一个MATCH ，但是用，来分割查询逻辑，效果同1

## 语法

### 数据类型

Cypher 分成3中数据类型 Property types、Structural types、Composite types.

### Property types（属性类型）：

- Number：包含两个子类型：Interger 和 Float
- String、
- Boolean、
- 空间数据类型：Point  、
- 时间类型：Date, Time, LocalTime, DateTime, LocalDateTime and Duration    

### Structural types：

- Nodes （点）:  Id、Labels、Map(属性集合)
- Relationships （关系）：id、Type、Map(属性集合)、ids(开始节点的id 和 结束节点的id)
- Paths：节点和关系的序列

### Composite types

- Lists：各种、有顺序的值。每一个都可以包含以上三种类型
- Maps：各种、没有顺序的值。key 是string，value 是任意数据类型。



## 命名规则与推荐

## 语法

1. 选择：case 表达式

   ​	`CASE test WHEN value THEN result [WHEN ...] [ELSE default] END`

   ​        `CASE WHEN predicate THEN result [WHEN ...][ELSE default] END`

2. --> 关系指向

3. 关键字：

   - CALL
   - CREATE
   - DELETE
   - DETACH
   - EXISTS
   - FOREACH
   - LOAD
   - MATCH
   - MERGE
   - OPTIONAL
   - REMOVE
   - RETURN
   - SET
   - START
   - UNION
   - UNWIND
   - WITH

4. 子关键字

   - LIMIT
   - ORDER
   - SKIP
   - WHERE
   - YIELD

5. 修饰语

   - ASC
   - ASCENDING
   - ASSERT
   - BY
   - CSV
   - DESC
   - DESCENDING
   - ON

6. 表达式：

   - ALL
   - CASE
   - ELSE
   - END
   - THEN
   - WHEN

7. 运算

   - AND
   - AS
   - CONTAINS
   - DISTINCT
   - ENDS       字符串以什么结束
   - IN
   - IS
   - NOT
   - OR
   - STARTS       字符串以什么开头
   - XOR

8. Schema

   - CONSTRAINT
   - CREATE
   - DROP
   - EXISTS
   - INDEX
   - NODE
   - KEY
   - UNIQUE

9. Hints

   - INDEX
   - JOIN
   - PERIODIC
   - COMMIT
   - SCAN
   - USING

10. Literals    

    - false
    - null
    - true

11. 保留字

    - ADD
    - DO
    - FOR
    - MANDATORY
    - OF
    - REQUIRE
    - SCALAR

## 增删改查[简单示例]
```
# 增 CREATE/MERGE
CREATE (p:Person {name: '123'}) 创建一个节点
MERGE (p:Person {name: '123'}) 如果存在该节点则不创建，否则创建这个节点。
 
# 查 MATCH
类似于SQL 中的 SELECT
MATCH(p: Person) return p limit 20 查找所有节点，返回20个
MATCH(p: Person {name:123}) return p limit 20 查找name是123的节点，返回20个
 
 
# 删 DELETE
MATCH(p: Person) delete p 删除所有节点，但是该节点必须不能存在关系
MATCH(p: Person) detach delete p 删除所有节点，以及该节点的所有关系。
 
# 改 SET
MATCH (n:Person {name: 'Jennifer'}) SET n.name= '123'
```

# 数据插入
```
# 少量数据 【百条所有数据】
使用Cypher 语句插入
 
# 小文件插入 【10m左右】
文件需放在 import 文件夹中。
LOAD CSV WITH HEADERS FROM 'file:///companies.csv' AS row
WITH row WHERE row.Id IS NOT NULL
MERGE (c:Company {companyId: row.Id});
 
 
 
# 大文件插入
文件需放在import 文件夹中
./bin/neo4j-admin database import full --nodes=import/$targetNode1 --nodes=import/$targetNode2 neo4j  --relationships=import/$targetRel --overwrite-destination
```

