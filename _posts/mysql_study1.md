---
title: MySQL Study 1
date: 2019-01-30 00:00:00
categories: Database
tags:
  - MySQL
header:
  teaser: /assets/images/mysql.png
excerpt: MySQL 구조 및 기본적인 사용법을 알아봅니다.
---
> MySQL 기초

## 1. MySQL 구조
![mysql_structure](/assets/images/mysql_structure.png)
<cite>[출처: Opentutorials - DATABASE2 MySQL - 4.MySQL의 구조](https://opentutorials.org/course/3161/19533)</cite>
{: .text-right}
여기서 표(table)의 행에 해당하는 것을 row, record 라 부르고<br>
열에 해당하는 것을 column, field 라 부릅니다.<br>
<br>
## 2. MySQL 서버 접속하기
```bash
$ mysql -uroot -p
Enter password: *****
```
여기서 `mysql -uroot -p` 는 mysql 서버에 root 계정으로 password 를 다음 입력으로 입력하여 접속하겠다는 뜻입니다.<br>
<br>
## 3. Database(Schema) 생성, 삭제하기
```bash
mysql> CREATE DATABASE db_joyoon; // db 생성
mysql> DROP DATABASE db_joyoon; // db 삭제
```
<br>
## 4. Database(Schema) 사용하기
```bash
mysql> USE db_joyoon;
```
<br>
## 5. 테이블(Table) 생성하기
![table_example](/assets/images/spreadsheet.PNG)
<cite>[출처: Opentutorials - DATABASE2 MySQL - 8.1.테이블의 생성](https://opentutorials.org/course/3161/19537)</cite>
{: .text-right}
이러한 구조를 가진 테이블을 만들어봅시다.<br>

```bash
mysql> CREATE TABLE table_example(
    ->  id INT(11) NOT NULL AUTO_INCREMENT,
    ->  title VARCHAR(100) NOT NULL,
    ->  description TEXT NULL,
    ->  created DATETIME NOT NULL,
    ->  author VARCHAR(30) NULL,
    ->  profile VARCHAR(100) NULL,
    ->  PRIMARY KEY(id));
```
기본 문법은,<br>
`CREATE TABLE [테이블명](필드1 정보, 필드2 정보, ...);`<br>
이런 모양을 갖습니다.<br>
<br>
또 필드 정보를 나타낼때에는<br>
`[필드명] [타입] [그외 추가 정보...]`<br>
이런 모양을 갖는다고 보시면 되겠습니다.<br>
<br>
위에서 쓰인 키워드를 설명해보겠습니다.<br>
- INT(11) : 정수형 타입, 11자 까지만 받습니다.
- VARCHAR(100) : 문자열 타입, 100자 까지만 받습니다.
- TEXT : 문자열 타입, VARCHAR() 에 비해 더 많은 문자열을 받을 수 있습니다.
- DATETIME : 시간 타입, 'year-month-day-time' 형태의 시간 정보를 받습니다.
- NOT NULL : 이 필드에는 NULL 값을 허용하지 않습니다.
- NULL : 이 필드에는 NULL 값을 허용합니다.
- AUTO_INCREMENT : 레코드 삽입시, 따로 지정해주지 않으면 1씩 증가한 값으로 받습니다.
- PRIMARY KEY(id) : 이 테이블의 대표키 값을 **id** 필드로 정합니다.

<br>
테이블이 제대로 생성되었는지 확인해보려면,
```bash
mysql> DESC table_example;
+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| id          | int(11)      | NO   | PRI | NULL    | auto_increment |
| title       | varchar(100) | NO   |     | NULL    |                |
| description | text         | YES  |     | NULL    |                |
| created     | datetime     | NO   |     | NULL    |                |
| author      | varchar(30)  | YES  |     | NULL    |                |
| profile     | varchar(100) | YES  |     | NULL    |                |
+-------------+--------------+------+-----+---------+----------------+
6 rows in set (0.00 sec)
```
`DESC [테이블명]` 을 통해서 확인할 수 있습니다.<br>
여기서 DESC 는 DESCRIBE 의 약자입니다.
<br>
## 6. CRUD (Create, Read, Update, Delete)
이번엔 테이블에 레코드를 Create, Read, Update, Delete 하는 방법을 알아보겠습니다.<br>
### - Create
`INSERT INTO [테이블명] (필드 lists) VALUES(필드 값들)`
```bash
mysql> INSERT INTO table_example
    -> (title, description, created, author, profile)
    -> VALUES('MySQL', 'MySQL is ...', NOW(), 'joyoon', 'admin');
```
### - Read
`SELECT [필드명] FROM [테이블명] [조건절]`
```bash
mysql> SELECT id, title, description, author FROM table_example WHERE author='joyoon' ORDER BY id DESC LIMIT 2;
```
`결과`
```bash
+----+--------+---------------+--------+
| id | title  | description   | author |
+----+--------+---------------+--------+
|  2 | MySQL  | MySQL is ...  | joyoon |
|  1 | Oracle | Oracle is ... | joyoon |
+----+--------+---------------+--------+
```
### - Update
`UPDATE [테이블명] SET [필드명=변경할값] [조건절]`
```bash
mysql> UPDATE table_example SET title='MySQL!!' description='MySQL is awesome ...' WHERE id=2;
```
### - Delete
`DELETE FROM [테이블명] [조건절]`
```bash
mysql> DELETE FROM table_example WHERE id=2;
```
<br>
<br>
