a very simple code in mysql to Data Partitioning in a Database , show right from creation of db, table, and trasaction step by step, a table with just 2 to 3 fields

EX.NO:1 :Write a Program to Perform Transaction Processing for a Database

mysql> CREATE DATABASE IF NOT EXISTS example_db;
Query OK, 1 row affected (0.01 sec)

mysql> USE example_db;
Database changed
mysql>  CREATE TABLE IF NOT EXISTS transactions (transaction_id INT AUTO_INCREMENT PRIMARY KEY,description VARCHAR(255),amount DECIMAL(10, 2));
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> INSERT INTO transactions (description, amount) VALUES('Initial deposit', 1000.00),('Purchase of goods', -500.50),('Salary credit', 1500.00);
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM transactions;
+----------------+-------------------+---------+
| transaction_id | description       | amount  |
+----------------+-------------------+---------+
|              1 | Initial deposit   | 1000.00 |
|              2 | Purchase of goods | -500.50 |
|              3 | Salary credit     | 1500.00 |
+----------------+-------------------+---------+
3 rows in set (0.00 sec)

mysql> START TRANSACTION;
Query OK, 0 rows affected (0.00 sec)

mysql> UPDATE transactions SET amount = amount - 50.00 WHERE transaction_id = 2;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> UPDATE transactions SET amount = amount - 50.00 WHERE transaction_id = 2;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> SELECT * FROM transactions;
+----------------+-------------------+---------+
| transaction_id | description       | amount  |
+----------------+-------------------+---------+
|              1 | Initial deposit   | 1000.00 |
|              2 | Purchase of goods | -600.50 |
|              3 | Salary credit     | 1500.00 |
+----------------+-------------------+---------+
3 rows in set (0.00 sec)

mysql> COMMIT;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from transactions
    -> ;
+----------------+-------------------+---------+
| transaction_id | description       | amount  |
+----------------+-------------------+---------+
|              1 | Initial deposit   | 1000.00 |
|              2 | Purchase of goods | -600.50 |
|              3 | Salary credit     | 1500.00 |
+----------------+-------------------+---------+
3 rows in set (0.00 sec)

mysql>

Ex No. 2Write a Program to do Data Partitioning in a Database

CREATE DATABASE IF NOT EXISTS partitioning_example;
Query OK, 1 row affected, 1 warning (0.00 sec)
mysql> USE partitioning_example;
Database changed
mysql>
mysql> CREATE TABLE IF NOT EXISTS partitioned_data (
       id INT AUTO_INCREMENT PRIMARY KEY,
         description VARCHAR(255),
         amount DECIMAL(10, 2),
         transaction_date DATE)
    -> PARTITION BY RANGE (YEAR(transaction_date)) (
    ->     PARTITION p0 VALUES LESS THAN (2020),
    ->     PARTITION p1 VALUES LESS THAN (2021),
    ->     PARTITION p2 VALUES LESS THAN (2022),
    ->     PARTITION p3 VALUES LESS THAN MAXVALUE
    -> );
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> INSERT INTO partitioned_data (description, amount, transaction_date) VALUES
    -> ('Initial deposit', 1000.00, '2020-01-01'),
    -> ('Purchase of goods', -500.50, '2021-03-15'),
    -> ('Salary credit', 1500.00, '2022-05-20');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM partitioned_data;
+----+-------------------+---------+------------------+
| id | description       | amount  | transaction_date |
+----+-------------------+---------+------------------+
|  1 | Initial deposit   | 1000.00 | 2020-01-01       |
|  2 | Purchase of goods | -500.50 | 2021-03-15       |
|  3 | Salary credit     | 1500.00 | 2022-05-20       |
+----+-------------------+---------+------------------+
3 rows in set (0.00 sec)
SELECT * FROM partitioned_data WHERE YEAR(transaction_date) < 2020;
Empty set (0.00 sec)

mysql> SELECT * FROM partitioned_data WHERE YEAR(transaction_date) > 2020;
+----+-------------------+---------+------------------+
| id | description       | amount  | transaction_date |
+----+-------------------+---------+------------------+
|  2 | Purchase of goods | -500.50 | 2021-03-15       |
|  3 | Salary credit     | 1500.00 | 2022-05-20       |
+----+-------------------+---------+------------------+
2	rows in set (0.00 sec)

EX.NO:3 Write a Program to Perform Parallel Indexing for a Database

CREATE DATABASE IF NOT EXISTS parallel_indexing_example;
Query OK, 1 row affected (0.01 sec)

mysql> USE parallel_indexing_example;
Database changed
mysql> CREATE TABLE IF NOT EXISTS my_table (
         id INT AUTO_INCREMENT PRIMARY KEY,
         name VARCHAR(255),
         value INT);
Query OK, 0 rows affected (0.03 sec)

mysql> INSERT INTO my_table (name, value) VALUES
    -> ('John', 100),
    -> ('Jane', 150),
    -> ('Bob', 200),
    -> ('Alice', 120),
    -> ('Charlie', 180);
Query OK, 5 rows affected (0.01 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> set session transaction isolation level serializable;
Query OK, 0 rows affected (0.00 sec)

mysql> START TRANSACTION;
Query OK, 0 rows affected (0.00 sec)

mysql> CREATE INDEX idx_value_parallel ON my_table (value)             
    ->ALGORITHM=INPLACE, LOCK=NONE;
mysql> COMMIT;
Query OK, 0 rows affected (0.00 sec)
mysql> SHOW INDEX FROM my_table;
+----------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table    | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+----------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| my_table |          0 | PRIMARY  |            1 | id          | A         |           5 |     NULL | NULL   |      | BTREE      |         |               |
+----------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
1	row in set (0.00 sec)

EX.NO:4 Write a Program to do Parallel Sort in a Database
CREATE DATABASE IF NOT EXISTS mydatabase;
Query OK, 1 row affected (0.00 sec)

mysql> USE mydatabase;
Database changed
mysql> CREATE TABLE mytable1 (
    ->   id INT AUTO_INCREMENT PRIMARY KEY,
    ->   name VARCHAR(50),
    ->   age INT
    -> );
Query OK, 0 rows affected (0.03 sec)

mysql> INSERT INTO mytable (name, age) VALUES
    -> ('Alice', 25),
    -> ('Bob', 30),
    -> ('Charlie', 22),
    -> ('David', 28);
Query OK, 4 rows affected (0.01 sec)
Records: 4  Duplicates: 0  Warnings: 0

select * from mytable;
+----+---------+------+
| id | name    | age  |
+----+---------+------+
|  1 | Alice   |   25 |
|  2 | Bob     |   30 |
|  3 | Charlie |   22 |
|  4 | David   |   28 |
+----+---------+------+
4 rows in set (0.00 sec)

mysql> SELECT * FROM mytable ORDER BY age;
+----+---------+------+
| id | name    | age  |
+----+---------+------+
|  3 | Charlie |   22 |
|  1 | Alice   |   25 |
|  4 | David   |   28 |
|  2 | Bob     |   30 |
+----+---------+------+
3	rows in set (0.00 sec)


EX.NO: 5 Write a Program to do Parallel Join in a Database


USE mydatabase;
Database changed
mysql> CREATE TABLE users (
    ->   user_id INT AUTO_INCREMENT PRIMARY KEY,
    ->   username VARCHAR(50)
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> CREATE TABLE orders (
    ->   order_id INT AUTO_INCREMENT PRIMARY KEY,
    ->   user_id INT,
    ->   product_name VARCHAR(50),
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO users (username) VALUES
    -> ('Alice'),
    -> ('Bob'),
    -> ('Charlie');
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> INSERT INTO orders (user_id, product_name, amount) VALUES
    -> (1, 'Laptop', 1200.50),
    -> (2, 'Phone', 599.99),
    -> (1, 'Headphones', 89.99),
    -> (3, 'Tablet', 299.99);
Query OK, 4 rows affected (0.01 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> select * from orders;
+----------+---------+--------------+---------+
| order_id | user_id | product_name | amount  |
+----------+---------+--------------+---------+
|        1 |       1 | Laptop       | 1200.50 |
|        2 |       2 | Phone        |  599.99 |
|        3 |       1 | Headphones   |   89.99 |
|        4 |       3 | Tablet       |  299.99 |
+----------+---------+--------------+---------+
4 rows in set (0.00 sec)

mysql> select * from users;
+---------+----------+
| user_id | username |
+---------+----------+
|       1 | Alice    |
|       2 | Bob      |
|       3 | Charlie  |
+---------+----------+
3 rows in set (0.00 sec)

mysql> SELECT users.user_id, users.username, orders.order_id, orders.product_name, orders.amount FROM users  JOIN orders ON users.user_id = orders.user_id;
+---------+----------+----------+--------------+---------+
| user_id | username | order_id | product_name | amount  |
+---------+----------+----------+--------------+---------+
|       1 | Alice    |        1 | Laptop       | 1200.50 |
|       2 | Bob      |        2 | Phone        |  599.99 |
|       1 | Alice    |        3 | Headphones   |   89.99 |
|       3 | Charlie  |        4 | Tablet       |  299.99 |
+---------+----------+----------+--------------+---------+
4	rows in set (0.00 sec)

EX.NO:6 Write a Program to Perform Distributed Transactions for a Database

CREATE DATABASE IF NOT EXISTS distributed_transactions;
Query OK, 1 row affected (0.01 sec)

mysql> USE distributed_transactions;
Database changed
mysql> CREATE TABLE IF NOT EXISTS orders (
    ->     order_id INT PRIMARY KEY,
    ->     product_name VARCHAR(255),
    ->     quantity INT
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO orders (order_id, product_name, quantity) VALUES
    -> (1, 'Product A', 10),
    -> (4, 'Product B', 5);
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> START TRANSACTION;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from orders;
+----------+--------------+----------+
| order_id | product_name | quantity |
+----------+--------------+----------+
|        1 | Product A    |       10 |
|        2 | Product B    |        5 |
+----------+--------------+----------+
2 rows in set (0.00 sec)

mysql> UPDATE orders SET quantity = 8 WHERE order_id = 1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> UPDATE orders SET quantity = 3 WHERE order_id = 2;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from orders;
+----------+--------------+----------+
| order_id | product_name | quantity |
+----------+--------------+----------+
|        1 | Product A    |        8 |
|        2 | Product B    |        3 |
+----------+--------------+----------+
2 rows in set (0.00 sec)

mysql> commit;
Query OK, 0 rows affected (0.00 sec)

EX.NO:7 Write a Program to Perform Concurrency Control for a Database

CREATE DATABASE IF NOT EXISTS concurrency_example;
Query OK, 1 row affected (0.01 sec)

mysql> USE concurrency_example;
Database changed
mysql> CREATE TABLE IF NOT EXISTS inventory (
    ->     product_id INT PRIMARY KEY,
    ->     product_name VARCHAR(255),
    ->     quantity INT
    -> );
Query OK, 0 rows affected (0.03 sec)

mysql> INSERT INTO inventory (product_id, product_name, quantity) VALUES
    -> (3, 'Product A', 10),
    -> (4, 'Product B', 5);
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> START TRANSACTION;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from inventory;
+------------+--------------+----------+
| product_id | product_name | quantity |
+------------+--------------+----------+
|          1 | Product A    |       10 |
|          2 | Product B    |        5 |
+------------+--------------+----------+
2 rows in set (0.00 sec)

mysql> DO SLEEP(5);
Query OK, 0 rows affected (5.01 sec)

mysql> UPDATE inventory SET quantity = quantity - 2 WHERE product_id = 1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> UPDATE inventory SET quantity = quantity + 2 WHERE product_id = 2;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
mysql> COMMIT;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from inventory;
+------------+--------------+----------+
| product_id | product_name | quantity |
+------------+--------------+----------+
|          1 | Product A    |        8 |
|          2 | Product B    |        7 |
+------------+--------------+----------+
2 rows in set (0.00 sec)


8.Write a Program to do Replication in a Database

CREATE DATABASE IF NOT EXISTS my_database;
mysql>  USE my_database;
Database changed
mysql> CREATE TABLE IF NOT EXISTS my_table (
    ->     id INT AUTO_INCREMENT PRIMARY KEY,
    ->     name VARCHAR(50),
    ->     age INT
    -> );

mysql> INSERT INTO my_table (name, age) VALUES ('Alice', 25);

mysql> INSERT INTO my_table (name, age) VALUES ('Bob', 30);

mysql> select * from my_table;
+----+-------+------+
| id | name  | age  |
+----+-------+------+
|  2 | Alice |   25 |
|  3 | Bob   |   30 |
+----+-------+------+

mysql> UPDATE my_table SET age = 35 WHERE name = 'Bob';

Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from my_table;
+----+-------+------+
| id | name  | age  |
+----+-------+------+
|  2 | Alice |   25 |
|  3 | Bob   |   35 |
+----+-------+------+

mysql> DELETE FROM my_table WHERE name = 'Alice';

mysql> select * from my_table;
+----+------+------+
| id | name | age  |
+----+------+------+
|  3 | Bob  |   35 |
+----+------+------+

mysql> SHOW MASTER STATUS;
Empty set (0.00 sec)

mysql> select * from my_table;
+----+------+------+
| id | name | age  |
+----+------+------+
|  3 | Bob  |   35 |
+----+------+------+
1 row in set (0.00 sec)

mysql> SHOW SLAVE STATUS;
+----------------------+-----------------------+------------------+-------------+---------------+------------------+---------------------+----------------------------------+---------------+-----------------------+------------------+-------------------+-----------------+---------------------+--------------------+------------------------+-------------------------+-----------------------------+------------+------------+--------------+---------------------+-----------------+-----------------+----------------+---------------+--------------------+--------------------+--------------------+-----------------+-------------------+----------------+-----------------------+-------------------------------+---------------+--------------------------------------------------------------------------------------------------------+----------------+----------------+-----------------------------+------------------+-------------+--------------------------------------------------------+-----------+---------------------+--------------------------------------------------------+--------------------+-------------+-------------------------+--------------------------+----------------+--------------------+--------------------+-------------------+---------------+----------------------+--------------+--------------------+
| Slave_IO_State       | Master_Host           | Master_User      | Master_Port | Connect_Retry | Master_Log_File  | Read_Master_Log_Pos | Relay_Log_File                   | Relay_Log_Pos | Relay_Master_Log_File | Slave_IO_Running | Slave_SQL_Running | Replicate_Do_DB | Replicate_Ignore_DB | Replicate_Do_Table | Replicate_Ignore_Table | Replicate_Wild_Do_Table | Replicate_Wild_Ignore_Table | Last_Errno | Last_Error | Skip_Counter | Exec_Master_Log_Pos | Relay_Log_Space | Until_Condition | Until_Log_File | Until_Log_Pos | Master_SSL_Allowed | Master_SSL_CA_File | Master_SSL_CA_Path | Master_SSL_Cert | Master_SSL_Cipher | Master_SSL_Key | Seconds_Behind_Master | Master_SSL_Verify_Server_Cert | Last_IO_Errno | Last_IO_Error                                                                                          | Last_SQL_Errno | Last_SQL_Error | Replicate_Ignore_Server_Ids | Master_Server_Id | Master_UUID | Master_Info_File                                       | SQL_Delay | SQL_Remaining_Delay | Slave_SQL_Running_State                                | Master_Retry_Count | Master_Bind | Last_IO_Error_Timestamp | Last_SQL_Error_Timestamp | Master_SSL_Crl | Master_SSL_Crlpath | Retrieved_Gtid_Set | Executed_Gtid_Set | Auto_Position | Replicate_Rewrite_DB | Channel_Name | Master_TLS_Version |
+----------------------+-----------------------+------------------+-------------+---------------+------------------+---------------------+----------------------------------+---------------+-----------------------+------------------+-------------------+-----------------+---------------------+--------------------+------------------------+-------------------------+-----------------------------+------------+------------+--------------+---------------------+-----------------+-----------------+----------------+---------------+--------------------+--------------------+--------------------------+----------------------+--------------+--------------------+
| Connecting to master | your_master_server_ip | replication_user |        3306 |            60 | mysql-bin.000001 |              123456 | DESKTOP-TJ8B6AI-relay-bin.000062 |             4 | mysql-bin.000001      | Connecting       | Yes               |                 |                     |                    |                        |                         |                             |          0 |            |            0 |              123456 |             154 | None            |                |             0 | No                 |                    |                    |                 |                   |                |                  NULL | No                            |          2005 | error connecting to master 'replication_user@your_master_server_ip:3306' - retry-time: 60  retries: 40 |              0 |                |                             |                0 |             | C:\ProgramData\MySQL\MySQL Server 5.7\Data\master.info |         0 |                NULL | Slave has read all relay log; waiting for more updates |              86400 |             | 240325 11:42:15         |                          |                |                    |                    |                   |             0 |                      |              |                    |
+----------------------+-----------------------+------------------+-------------+---------------+------------------+---------------------+----------------------------------+---------------+-----------------------+------------------+-----
1 row in set (0.00 sec)

mysql>
