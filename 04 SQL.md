# SQL

Under the grammar of MySQL. 



## SELECT ... FROM ... WHERE ...

````sql
SELECT column1, column2, ...
FROM table_name
WHERE conditions; 
````



## SELECT DISTINCT ...

````sql
SELECT DISTINCT column1, column2, ...
...
````



## AND/OR/NOT

````sql
... WHERE condition1 AND condition2; 
````



## ORDER BY ... ASC/DESC

(Can be ordered by multiple columns. )

````sql
SELECT column1, column2, ...
FROM table_name
WHERE conditions
ORDER BY column1, column2, ... ASC; 
````



## INSERT INTO ... VALUES ...

It is possible to write the `INSERT INTO` statement in two ways:

1. Specify both the column names and the values to be inserted:

````sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...); 
````

2. Adding values for all the columns of the table:

````sql
INSERT INTO table_name
VALUES (value1, value2, ...); 
````

To insert multiple records, 

````sql
INSERT INTO table_name
VALUES (value_set1), (value_set2), ...;
````





## UPDATE ... SET ...

````sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE conditions; 
````



## DELETE FROM ...

````sql
DELETE FROM table_name
WHERE conditions; 
````



## SELECT TOP ...

The `SELECT TOP` clause is used to specify the number of records to return.

(First several lines of data on the original table but not ranked by any column. )

(Can also use ``UPDATE TOP`` etc.)

````sql
SELECT column1, column2, ...
FROM table_name
WHERE conditions
LIMIT number; 
````



## MIN/MAX/COUNT/AVG/SUM()

````sql
SELECT MIN(column1) ...
````



## GROUP BY ...

The `GROUP BY` statement is often used with aggregate functions (`COUNT()`, `MAX()`, `MIN()`, `SUM()`, `AVG()`) to group the result-set by one or more columns.

(``columns`` in ``SELECT`` statement and ``GROUP BY`` statement must be the same. )

(``GROUP BY`` can take multiple columns. )

````sql
SELECT column1, column2, ... , MIN(column0)
FROM table_name
GROUP BY column1, column2, ...; 
````



## HAVING ...

The `HAVING` clause was added to SQL because the `WHERE` keyword cannot be used with aggregate functions.

(``WHERE`` decides whether each record is to be taken by the aggregate function or not. )

(``HAVING`` decides whether each result after the aggregate function is to be shown or not. )

````sql
SELECT column1, column2, ... , MIN(column0)
FROM table_name
WHERE conditions
GROUP BY column1, column2, ...
HAVING conditions; 
````



## LIKE ... & Wildcards

````sql
... WHERE column1 LIKE pattern; 
````

| Symbol | Description                                                | Example                                    |
| ------ | ---------------------------------------------------------- | ------------------------------------------ |
| %      | Represents zero or more characters                         | ``bl%`` finds bl, black, blue, and blob    |
| _      | Represents a single character                              | ``h_t`` finds hot, hat, and hit            |
| []     | Represents any single character within the brackets        | ``h[oa]t`` finds hot and hat, but not hit  |
| ^      | Represents any character not in the brackets               | ``h[^oa]t`` finds hit, but not hot and hat |
| -      | Represents any single character within the specified range | ``c[a-b]t`` finds cat and cbt              |



## IN ...

````sql
... WHERE column1 IN (value1, value2, ...); 
````

````sql
... WHERE column1 IN (SELECT ...); 
````



## BETWEEN ... AND ...

````sql
... WHERE column1 BETWEEN value1 AND value2; 
````



## AS ... & Aliases

1. Alias table

````sql
... FROM table_name AS alias_name ...
````

2. Alias column

````sql
SELECT column1 AS alias_name ...
````



## JOIN ... ON ... & Joins

````sql
... JOIN table2 ON table1.column1 = table2.column2; 
````

A `JOIN` clause is used to combine rows from two or more tables, based on a related column between them.

- `(INNER) JOIN`: Returns records that have matching values in both tables
- `LEFT (OUTER) JOIN`: Returns all records from the left table, and the matched records from the right table
- `RIGHT (OUTER) JOIN`: Returns all records from the right table, and the matched records from the left table
- `FULL (OUTER) JOIN`: Returns all records when there is a match in either left or right table

(``JOIN`` is by default ``INNER JOIN``. )

(Only ``JOIN``, `INNER JOIN` and `FULL JOIN` can take more than two columns at the same time. )

![image-20211004192808172](C:\Users\29828\AppData\Roaming\Typora\typora-user-images\image-20211004192808172.png)



## Self Join

A self join is a regular join, but the table is joined with itself.

````sql
SELECT columns
FROM table1 T1, table1 T2
WHERE condition; 
````



## UNION

The `UNION` operator is used to combine the result-set of two or more `SELECT` statements.

- Every `SELECT` statement within `UNION` must have the same number of columns
- The columns must also have similar data types
- The columns in every `SELECT` statement must also be in the same order

````sql
SELECT column1, column2, ... FROM table1
UNION
SELECT column3, column4, ... FROM table2; 
````



## EXISTS()

The `EXISTS` operator returns TRUE if the subquery returns one or more records.

````sql
... WHERE EXISTS(SELECT column1, column2, ... FROM table_name WHERE conditions); 
````



## ANY()

The `ANY` operator returns TRUE if ANY of the subquery values meet the condition. 

````sql
... WHERE column1 operator ANY(SELECT column2 FROM table2 WHERE conditions); 
````

(The *operator* must be a standard comparison operator (=, <>, !=, >, >=, <, or <=). )



## ALL()

The `ALL` operator returns TRUE if ALL of the subquery values meet the condition. 

````sql
... WHERE column1 operator ALL(SELECT column2 FROM table2 WHERE conditions); 
````



## SELECT ... INTO ...

The `SELECT INTO` statement copies data from one table into a **new** table.

````sql
SELECT column1, column2, ...
INTO newtable [IN externaldb]
FROM oldtable
WHERE conditions; 
````



## INSERT INTO ... SELECT ...

The `INSERT INTO SELECT` statement copies data from one table and inserts it into another table in the database.

````sql
INSERT INTO table2
SELECT column1, column2, ...
FROM table1
WHERE conditions; 
````

````sql
INSERT INTO table2 (column1, column2, ...)
SELECT column3, column4, ...
FROM table1
WHERE conditions; 
````



## CASE ... WHEN ... THEN ... ELSE ... END

````sql
CASE
	WHEN condition1 THEN result1
	...
	ELSE result0
END; 
````



## IFNULL/ISNULL/COALESCE/NVL()

`IFNULL(expression, alt_value)` returns the expression if the expression is not NULL, otherwise, it returns the alt_value. 

`ISNULL(value)` returns 1 or 0 depending on whether an expression is NULL

`COALESCE(value1, ...)` returns the first NON NULL value in the list. 



## Stored Procedure

````sql
CREATE PROCEDURE precedure_name
AS
sql_statement
GO; 
````

````sql
EXEC procedure_name; 
````



## Comments

````sql
-- comments until the end of the line
/* multi-line comments */
````



## Operators

| Operator | Description              |
| -------- | ------------------------ |
| &        | Bitwise AND              |
| \|       | Bitwise OR               |
| ^        | Bitwise exclusive OR     |
| =        | Equal to                 |
| >        | Greater than             |
| <        | Less than                |
| >=       | Greater than or equal to |
| <=       | Less than or equal to    |
| +=   | Add equals               |
| -=   | Subtract equals          |
| *=   | Multiply equals          |
| /=   | Divide equals            |
| %=   | Modulo equals            |
| &=   | Bitwise AND equals       |
| ^-=  | Bitwise exclusive equals |
| \|*= | Bitwise OR equals        |




# SQL Database



## Create DB

````sql
CREATE DATABASE dbname; 
````



## Delete DB

````sql
DROP DATABASE dbname; 
````



## Backup DB

````sql
BACKUP DATABASE dbname
TO DISK = 'C:\filepath'; 
````

A differential back up only backs up the parts of the database that have changed since the last full database backup.

````sql
BACKUP DATABASE dbname
TO DISK = 'C:\filepath'
WITH DIFFERENTIAL; 
````



## Create Table

````sql
CREATE TABLE table_name(
	column1 datatype, 
	...
); 
````

(Common data types in SQL Server: char, text, image, bit, int, bigint, decimal, float, date, time, datetime, xml, table ...)



## Delete Table

````sql
DROP TABLE table_name; 
````



## Add/Delete Column

````sql
ALTER TABLE table_name
ADD column_name datatype; 
````

````sql
ALTER TABLE table_name
DROP column_name; 
````



## Modify Column Datatype

````sql
ALTER TABLE table_name
MODIFY COLUMN column_name datatype; 
````



## Constraints

Constraints can be specified when the table is created with the `CREATE TABLE` statement, or after the table is created with the `ALTER TABLE` statement.

````sql
... column datatype constraint, ...
````

- `NOT NULL` - Ensures that a column cannot have a NULL value

- `UNIQUE` - Ensures that all values in a column are different

- `PRIMARY KEY` - A combination of a `NOT NULL` and `UNIQUE`. Uniquely identifies each row in a table

  (You can have many `UNIQUE` constraints per table, but only one `PRIMARY KEY` constraint per table.)

- `FOREIGN KEY` - Prevents actions that would destroy links between tables

-  `CHECK` - Ensures that the values in a column satisfies a specific condition

-  `DEFAULT` - Sets a default value for a column if no value is specified

-  `CREATE INDEX` - Used to create and retrieve data from the database very quickly



## NOT NULL

````sql
... column datatype NOT NULL, ...
````

````sql
... ALTER COLUMN column_name datatype NOT NULL;
````



## UNIQUE

1. To create a new column with `UNIQUE` constraint:

````sql
...
column datatype, 
UNIQUE (column), 
...
````

2. To define a `UNIQUE` constraint on multiple new columns: 

````sql
... CONSTRAINT constraint UNIQUE (columns), ...
````

3. To add a `UNIQUE` constraint on a column already exist:

````sql
... ADD UNIQUE (columns), ...
````

4. To define a `UNIQUE` constraint on multiple columns already exist:

````sql
... ADD CONSTRAINT constraint UNIQUE (columns), ...
````

5. To drop a `UNIQUE` constraint:

````sql
ALTER TABLE table
DROP INDEX constraint; 
````



## PRIMARY KET

Primary keys must contain UNIQUE values, and cannot contain NULL values.

A table can have only ONE primary key; and in the table, this primary key can consist of single or multiple columns (fields). 

Codes are the same as `UNIQUE`. 



## FOREIGN KEY

The `FOREIGN KEY` constraint is used to prevent actions that would destroy links between tables.

A `FOREIGN KEY` is a field (or collection of fields) in one table, that refers to the `PRIMARY KEY` in another table.

The table with the foreign key is called the child table, and the table with the primary key is called the referenced or parent table. 

**SQL Server**

````sql
... column1 datatype1 FOREIGN KEY REFERENCES table2(column2), ...
````

**MySQL**

```sql
...
column1 datatype1, 
FOREIGN KEY (column1) REFERENCES table2(column2), 
...
```



## CHECK

````sql
...
column datatype, 
CHECK (condition),
...
````



## DEFAULT

1. To set a `DEFAULT` value while creating a new column: 

````sql
... column datatype DEFAULT default_value, ...
````

2. To add a `DEFAULT` constraint on a column already exist: 

````sql
ALTER column SET DEFAULT default_value; 
````

3. To drop a `DEFAULT` constraint: 

````sql
ALTER column DROP DEFAULT; 
````



## CREATE INDEX

The `CREATE INDEX` statement is used to create indexes in tables.

Updating a table with indexes takes more time than updating a table without (because the indexes also need an update). So, only create indexes on columns that will be frequently searched against. 

````sql
CREATE INDEX index_name
ON table (columns); 
````

Duplicate values of `columns` are not allowed:

````sql
CREATE UNIQUE INDEX index_name
ON table (columns); 
````

Drop index: 

````sql
ALTER TABLE table
DROP INDEX index; 
````



## AUTO_INCREMENT

Auto-increment allows a unique number to be generated automatically when a new record is inserted into a table.

Often this is the primary key field that we would like to be created automatically every time a new record is inserted.

To insert a new record into the table, we will NOT have to specify the value for this column. 

By default, the starting value for `AUTO_INCREMENT` is 1, and it will increment by 1 for each new record.

````sql
...
column datatype NOT NULL AUTO_INCREMENT, 
PRIMARY KEY(column), 
...
````

To change the start value: 

````sql
ALTER TABLE table AUTO8INCREMENT = start_value; 
````



## Dates

- `DATE` - format YYYY-MM-DD
- `DATETIME` - format: YYYY-MM-DD HH:MI:SS
- `SMALLDATETIME` - format: YYYY-MM-DD HH:MI:SS
- `TIMESTAMP` - format: a unique number

The value comes between `''`:

````sql
-- ex: ... WHERE Date = '2020-10-10'; 
````



## CREATE VIEW ... AS ...

A view always shows up-to-date data! The database engine recreates the  view, every time a user queries it.

````sql
CREATE VIEW view_name AS
SELECT ...
````

We can query the view:

````sql
SELECT ... FROM [view_name]; 
````

A view can be updated with the `CREATE OR REPLACE VIEW` statement.

````sql
CREATE OR REPLACE VIEW view_name AS
SELECT ...
````

To drop a view: 

````sql
DROP VIEW view_name; 
````



## Injection (Hacking)

1. SQL Injection Based on 1=1 is Always True

````sql
SELECT * FROM Users WHERE UserId = 105 OR 1=1; 
````

2. SQL Injection Based on ""="" is Always True

````sql
 SELECT * FROM Users WHERE Name ="John Doe" AND Pass ="myPass" 
 -- will be turned into as follows by replaceing both Name and Pass by '" or ""="': 
 SELECT * FROM Users WHERE Name ="" or ""="" AND Pass ="" or ""="" 
````

3. SQL Injection Based on Batched SQL Statements

````sql
txtSQL = "SELECT * FROM Users WHERE UserId = " + txtUserId;
-- if we replace txtUserId by '105; DROP TABLE Suppliers': 
SELECT * FROM Users WHERE UserId = 105; DROP TABLE Suppliers; 
````



# MySQL in Python

## Introduction

Il nous faut un driver (une API, interface d'application) pour accéder à un SGBD (Système de Gestion de Bases de Données). 

`pip install mysql-connector-python` dans un environement SQL. ````

Code SQL pour créer une base de données : 

````sql
CREATE DATABASE courspython; 

USE courspython; 

CREATE TABLE personne(
	num INT PRIMARY KEY AUTO_INCREMENT, 
	nom VARCHAR(30), 
	prenom VARCHAR(30)
); 

SHOW TABLES; 

INSERT INTO personne (nom, prenom)
VALUES ('Wick', 'John'), ("Dalton", "Jack"); 

SELECT * FROM personne; 
````



## Utilisation

**étape 1 : établir la connexion avec la base de données**

````python
import mysql.connextor

conenxion = mysql.connextor.connect(
	host = "localhost", 
    user = "root", 
    password = "", 
    dtabase = "courspython", 
    port = 3308
)
````

**étape 2 : créer et exécuter des requêtes SQL**

````python
request = "SELECT * FROM personne"

curseur = connexion.cursor()
curseur.execute(request)
````

**étape 3 : récupérer le résultat**

````python
personnes = curseur.fetchall()

for personnne in personnes:
    print(personne)
````

**étape 4 : fermer la connexion**

````python
connexion.close()
curseur.close()
````



## Amélioration

Les instruction d'accès peuvent lever une exception. Ajoutons donc `try ... except ... finally`

````python
import mysql.connector
from mysql.connector import Error

try:
    connexion = mysql.connector.connect(
        host = "localhost", 
    	user = "root", 
    	password = "", 
    	dtabase = "courspython", 
    	port = 3308
    )
    
    request = "SELECT * FROM Personne"
    
    curseur = connexion.cursor()
    curseur.execute(request)
    
    personnes = curseur.fetchall()
    for personne in personnes:
    	print(personne)

except Error as e:
	print("Exception : ", e)

finally:
	if (connexion.is_connected()):
        connexion.close()
        curseur.close()
        print("La connexion à MySQL est désormais fermée")
````

Pour afficher le résultat avec plus de détails : 

````python
....
    curseur = connexion.cursor(dictionary = True) # If dictionary is True, the cursor returns rows as dictionaries
    curseur.execute(request)
    
    personnes = curseur.fetchall()
    for personne in personnes:
    	print(personne['prenom'], personne['nom'])
....
````

Pour indiquer le nombre de tuples à récupérer : 

````python
...
	personnes = curseur.fetchmany(2) # seulement deux premières lignes
...
````

Pour récupérer seulement le premier tuple : 

````python
...
	personne = curseur.fetchone()
...
````

On peut aussi utiliser les requêtes paramétrées : 

````python
...
	curseur = connexion.cursor()
	tuple = (2,) # seulemet la deuxième ligne, virgule obligatoire
	curseur.execute(request, tuple)
	
	personne = curseur.fetchone() # une seule ligne dans le résultat
...
````



## Insertion

````python
request = """INSERT INTO personne (nom, prenom) VALUES (%s, %s) """ # prendre des valuers de tuple

tuple = ('muller', 'thomas')
curseur = connexion.cursor()
curseur.execute(request, tuple)

connexion.commit() # 'execute' et 'commit' dont indispensable pour valider la requête de modifier le database
````

Pour annuler la requête : 

````python
connexion.rollback() # après 'execute', avant 'commit'
````

Pour récupérer la valeur de la clé primaire auto-générée : 

````python
print("Tuple inséré avec succès dans la table personne avec id = ", curseur.lastrowid)
````

Pour une insertion multiple : 

````python
tuples = [('muller', 'thomas'), ('ribery', 'franc')]
````



## Restruction du Code

* cours-python_mysql
  * src
    * config
      * config.json
      * config.py
      * myconnection.py
    * dao
      * dao.py
      * personnedao.py
    * model
      * personne.py
  * main.py

`config.json`

````json
{
	"mysql": {
		"host": "localhost",
        "user": "root",
        "password": "",
        "db": "courspython",
        "port": 3308
	}
}
````

`config.py` charge des données dans `config.json`

````python
import json

config = {}

with open('src/config/config.json') as f:
	config = json.load(f)
````

`myconnection.py`

````python
import mysql.connector as pymysql
from mysql.connector import Error
from .config import config

class MyConnection:
    __connection = None
    __cursor = None
    
    def __init__(self):
        __db_config = config[’mysql’]
        self.__connection = pymysql.connect(host=__db_config[’host’],
       								user = __db_config[’user’],
       								password = __db_config[’password’],
        							db = __db_config[’db’],
        							port=__db_config[’port’])
        self.__cursor = self.__connection.cursor()
    
    def query(self, query, params):
        self.__cursor.execute(query, params)
        return self.__cursor
    
    def close(self):
    	self.__connection.close()
````

`personne.py`

````python
class Personne:
    def __init__(self, num: int = 0, nom: str = ’’, prenom: str = ’’):
        self._num = num
        self._nom = nom
        self._prenom = prenom
        
    @property
    def num(self) -> int:
    	return self._num
    
    @num.setter
    def num(self, num) -> None:
        if num > 0:
        self._num = num
        else:
        self._num = 0
       
    @property
    def nom(self) -> str:
        return self._nom
        
    @nom.setter
    def nom(self, nom) -> None:
    	self._nom = nom
    
    @property
    def prenom(self) -> str:
    	return self._prenom
    
    @prenom.setter
    def prenom(self, prenom) -> None:
    	self._prenom = prenom
    
    @prenom.deleter
    def prenom(self) -> str:
    	del self._prenom
    def __str__(self) -> str:
    	return str(self._num) + " " + self._prenom + " " + self._nom
````

`Dao.py` classe abstraite

````python
from typing import TypeVar, Generic, Iterable
from abc import ABC, abstractmethod

T = TypeVar(’T’)

class Dao (Generic[T], ABC):
    @abstractmethod
    def save(self, t: T) -> T:
    	pass
    @abstractmethod
    def findAll(self) -> Iterable:
    	pass
    @abstractmethod
    def findById(self, t: int) -> T:
    	pass
    @abstractmethod
    def update(self, t: T) -> T:
    	pass
    @abstractmethod
    def remove(self, t: T) -> None:
    	pass
````

`personneDao.py` implémente `Dao[Personne]`

````python
from ..config.myconnection import MyConnection
from ..model.personne import Personne
from .dao import Dao

class PersonneDao (Dao[Personne]):
    __db = None
    
    def __init__(self):
    	self.__db = MyConnection()
        
    # méthodes à compléter
    def findAll(self):
    	return self.__db.query("SELECT * FROM personne", None).fetchall()
    def save(self, personne: Personne) -> Personne:
    	...
    def findById(self, t: int) -> Personne:
    	...
    def update(self, t: Personne) -> Personne:
    	...
    def remove(self, t: Personne) -> None:
    	...
````

`main.py` pour tester toutes les classes

````python
from mysql.connector import Error
from src.dao.personnedao import PersonneDao
from src.model.personne import Personne

try:
    personneDao = PersonneDao() # implémenté jusqu'à 'curseur.execute(request)'
    for personne in personneDao.findAll():
    	print(personne)
except Error as e:
	print("Exception : ", e)
````



# MySQL in DOS

在cmd模式下输入``mysql --version``以查看mysql版本
完整的命令可以通过`mysql --help`来获取

1. 连接数据库

不借助数据库管理软件(如Navicat等软件)，通过dos连接mysql软件库服务器，然后操作数据库

连接数据库通用格式：`mysql -P 端口号 -h mysql主机名或ip地址 -u 用户名 -p`
解释：`-P`代表端口，`-p`代表密码，`h`代表主机名或ip，`u`代表user用户
EX：`mysql -P 3306 -h 192.168.1.104 -u root -p`

* 本地连接
  如果是命令行是mysql所在的本机，而且用默认的端口 3306 时，可以简化语句为：`mysql -u root -p`

* 远程连接
  注意：使用远程连接时，使用的连接用户和该用户现在的ip地址应该是远程数据库中允许的用户和允许的ip：`mysql -P 3306 -h 192.168.1.104 -u root -p`

2. 操作数据库

在使用用户名和密码成功登录mysql数据库后，在改用户的权限范围内可以操作该用户对数据库的操作，在操作数据时每条语句是用`;`或`\g`来标志结束的。

    1.查看所有数据库
    show databases;
    2.创建数据库
    create database db_yves;
    3.使用数据库
    use db_yves;
    4.显示数据库中所有表
    show tables;
    5.查看表结构
    show columns from customers; 或者使用快捷方式:DESCRIBE customers;
    这里写图片描述
    6.删除数据库
    drop database db_yves;

3. 关于命令行模式数据库文件的导入和导出：

    导出数据库文件
    包括导出数据库到指定表.
    1.导出数据库db_yves的结构和数据
    mysqldump -h localhost -u root -p db_yves > D:\db_yves.sql
    2.导出数据库db_yves的结构(加-d参数):
    mysqldump -h localhost -u root -p db_yves -d > D:\db_yves_stru.sql
    3.导出数据库db_yves中的customers表的结构和数据:
    mysqldump -h localhost -u root -p db_yves customers > D:\customers.sql
    4.导出数据库db_yves中的customers表的结构(加-d参数):
    mysqldump -h localhost -u root -p db_yves -d > D:\customers_stru.sql
    
    导入数据库文件
    向数据库db_yves导入数据库文件db_yves.sql.
    mysql -h localhost -u root -p db_yves < D:\db_yves.sql

4. 其他常用语句

    SHOW STATUS，用于显示广泛的服务器状态信息；
    SHOW CREATE DATABASE和SHOW CREATE TABLE，分别用来显示创 建特定数据库或表的MySQL语句；
    SHOW GRANTS，用来显示授予用户（所有用户或特定用户）的安 全权限；
    SHOW ERRORS和SHOW WARNINGS， 用来显示服务器错误或警告消息。



# NOTES

1. NEVER FORGET ' ' for string. 
2. To delete a record from the table, use `DELETE`; to delete a database, a table or a column, use `DROP`
3. To add a record into a table, use `INSERT`; to add a database or a table, use `CREATE`; to add a new column, use `ADD`
4. To modify a record in a table, use `UPDATE`; to modify a column, use `ALTER` / `MODIFY`
5. SQL语句不区分大小写。（除部分配置环境下table名称）

