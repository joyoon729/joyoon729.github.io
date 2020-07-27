---
title: MySQL Study 2
date: 2019-01-31 00:00:00
categories: Database
tags:
  - MySQL
header:
  teaser: /assets/images/mysql.png
excerpt: 관계형 데이터베이스(Relational Database)의 특징과 JOIN 문 사용하기.
---
> 관계형 데이터베이스 (Relational Database)

MySQL 은 관계형 데이터베이스 중의 하나라고 할 수 있습니다.<br>
관계형 데이터베이스의 특징은 정보를 하나의 테이블에 담는 것이 아니라<br>
목적에 따라 여러개의 테이블에 **나누어** 담는 것인데요.<br>
예를 들어 보겠습니다.<br>
<br>

![nonerelational_db](/assets/images/relational_db1.png)
<cite>[출처: Opentutorials - DATABASE2 MySQL - 15.관계형 데이터베이스의 필요성](https://opentutorials.org/course/3161/19544)</cite>
{: .text-right}
정보를 이처럼 한 테이블에 담는다면, 일단 사용자가 보기에는 편할 것입니다.<br>
모든 정보를 이 한 테이블에서 얻어낼 수 있을테니까요.<br>
하지만 데이터를 관리하는 관리자 입장에서는 이런 테이블은 유지/보수 하기가 어려울 뿐더러,<br>
중복되는 정보들에 의미 부여를 하기가 어렵습니다.<br>
<br>
만약 위의 테이블에서 'egoing' 이라는 글쓴이 정보를 'EGOING' 이라고 바꿔야한다면<br>
id가 1,2,5인 테이블을 하나하나 바꿔야 합니다.<br>
만약 이 테이블에 더 많은 정보가 들어있었다면 더 많은 수정 작업이 필요했을 것입니다.<br>
또한 'egoing' 이라는 또다른 동명이인 글쓴이가 존재한다면, 이 테이블을 통해서는 동일한 글쓴이인지 판별하기가 어렵습니다.<br>
<br>
<br>

![relational_db](/assets/images/relational_db2.png)
<cite>[출처: Opentutorials - DATABASE2 MySQL - 15.관계형 데이터베이스의 필요성](https://opentutorials.org/course/3161/19544)</cite>
{: .text-right}
하지만 위와같이 테이블을 **author** 테이블과 **topic** 테이블로 나눈다면 문제를 해결할 수 있습니다.<br>
**topic** 테이블의 author 필드는 author_id 필드로 대체되었고, 본래 **topic** 테이블에 담겨있던 author 필드와 profile 필드의 정보들은 **author** 테이블로 옮겨졌습니다.<br>
그리고 **author** 테이블에 id 필드를 만들어 **topic** 의 author_id 와 대응되게 하였습니다.<br>
<br>
이제 관리자는 'egoing' 이라는 글쓴이 정보를 'EGOING' 으로 수정하려 할때<br>
여러번의 수정없이 **author** 테이블의 id=1 에 해당하는 'egoing' 만 수정하면 **topic** 테이블 전체에 수정하는 효과를 가져올 수 있습니다.<br>
또한 만약 'egoing' 이라는 이름을 가진 동명이인이 글을 쓴다고 하면, **author** 테이블에 id=4이고 'egoing' 이름을 가진 새로운 레코드를 추가함으로써 의미적으로 'id=1 egoing' 과 'id=4 egoing' 을 구분할 수 있을 것입니다.<br>
<br>
<br>
이러한 관계형 데이터베이스의 특징을 살펴보면서 이전에 JavaScript 공부를 하며 느꼈던 '함수의 필요성'과 많이 유사함을 느낄 수 있었습니다. 유지/보수와 의미부여 측면에 있어서요.<br>
전부 똑같다고 볼 순 없겠지만, 지금은 이런식으로 이해하면 도움이 될 것 같습니다.<br>
<br>
<br>
<hr>
> JOIN 사용해보기

이제 테이블을 나누었으니, 데이터 관리자 입장에서는 한결 수월해졌습니다.<br>
하지만 사용자 입장에서는 오히려 불편해졌지요. 하나였던 테이블을 두개에 걸쳐 번갈아가며 정보를 확인해야 하니까요.<br>
이를 해소할 방법이 있습니다. ***'JOIN'*** 입니다.<br>
JOIN 은 두 개의 테이블 사이에 공통되는 정보를 바탕으로 합성하는것입니다.<br>
일단 사용법은 'SELECT' 문에 JOIN문을 추가하는 형식인데,<br>
`SELECT * FROM [테이블명] JOIN [조인할 테이블명] ON [조건절]`<br>
이렇습니다. 예를 들어보겠습니다.<br>
```bash
mysql> SELECT * FROM topic JOIN author ON topic.author_id = author.id;
+----+------------+-------------------+---------------------+-----------+------+--------+---------------------------+
| id | title      | description       | created             | author_id | id   | name   | profile                   |
+----+------------+-------------------+---------------------+-----------+------+--------+---------------------------+
|  1 | MySQL      | MySQL is ...      | 2019-01-30 08:00:00 |         1 |    1 | egoing | developer                 |
|  2 | Oracle     | Oracle is ...     | 2019-01-30 08:01:00 |         1 |    1 | egoing | developer                 |
|  5 | MongoDB    | MongoDB is ...    | 2019-01-30 08:02:00 |         1 |    1 | egoing | developer                 |
|  3 | SQL Server | SQL Server is ... | 2019-01-30 08:03:00 |         2 |    2 | duru   | data administrator        |
|  4 | PostgreSQL | PostgreSQL is ... | 2019-01-30 08:04:00 |         3 |    3 | taeho  | data scientist, developer |
+----+------------+-------------------+---------------------+-----------+------+--------+---------------------------+
```
위와같이 topic.author_id 와 author.id 가 같은 레코드끼리 합성해 보여주고 있음을 알 수 있습니다.<br>
<br>
SELECT 문의 조건을 이용해서 더 깔끔하게 표현할 수 있습니다.
```bash
mysql> SELECT topic.id, title, description, created, name, profile FROM topic JOIN author ON topic.author_id = author.id ORDER BY id ASC;
+----+------------+-------------------+---------------------+--------+---------------------------+
| id | title      | description       | created             | name   | profile                   |
+----+------------+-------------------+---------------------+--------+---------------------------+
|  1 | MySQL      | MySQL is ...      | 2019-01-30 08:00:00 | egoing | developer                 |
|  2 | Oracle     | Oracle is ...     | 2019-01-30 08:01:00 | egoing | developer                 |
|  3 | SQL Server | SQL Server is ... | 2019-01-30 08:02:00 | duru   | data administrator        |
|  4 | PostgreSQL | PostgreSQL is ... | 2019-01-30 08:03:00 | taeho  | data scientist, developer |
|  5 | MongoDB    | MongoDB is ...    | 2019-01-30 08:04:00 | egoing | developer                 |
+----+------------+-------------------+---------------------+--------+---------------------------+
```
<br>
또한 필드명을 원하는 이름으로 바꿔서 보여주고 싶을때는 'AS' 키워드를 사용하면 됩니다.
```bash
mysql> SELECT topic.id, title, description, created, name AS author, profile FROM topic JOIN author ON topic.author_id = author.id ORDER BY id ASC;
+----+------------+-------------------+---------------------+--------+---------------------------+
| id | title      | description       | created             | author | profile                   |
+----+------------+-------------------+---------------------+--------+---------------------------+
|  1 | MySQL      | MySQL is ...      | 2019-01-30 08:00:00 | egoing | developer                 |
|  2 | Oracle     | Oracle is ...     | 2019-01-30 08:01:00 | egoing | developer                 |
|  3 | SQL Server | SQL Server is ... | 2019-01-30 08:02:00 | duru   | data administrator        |
|  4 | PostgreSQL | PostgreSQL is ... | 2019-01-30 08:03:00 | taeho  | data scientist, developer |
|  5 | MongoDB    | MongoDB is ...    | 2019-01-30 08:04:00 | egoing | developer                 |
+----+------------+-------------------+---------------------+--------+---------------------------+
```
<br>
<br>
이제 관계형 데이터베이스의 특징을 이용해서<br>
데이터 관리자에게 편리한 테이블 구조를 만듦과 동시에<br>
사용자에게도 동일한 정보에 동일한 가독성을 주는 데이터베이스를 만들어보았습니다.<br>
