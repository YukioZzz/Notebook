
<a id="database"></a>

## 💾 数据库

> 本节部分知识点来自《数据库系统概论（第 5 版）》

#### 为何使用数据库?
参考[链接](https://www.liaoxuefeng.com/wiki/1177760294764384/1179613436834240)

随着应用程序的功能越来越复杂，数据量越来越大，如何管理这些数据就成了大问题：

- 读写文件并解析出数据需要大量重复代码；
- 从成千上万的数据中快速查询出指定数据需要复杂的逻辑。

如果每个应用程序都各自写自己的读写数据的代码，一方面效率低，容易出错，另一方面，每个应用程序访问数据的接口都不相同，数据难以复用。

### 基本概念

* 数据（data）：描述事物的符号记录称为数据。
* 数据库（DataBase，DB）：是长期存储在计算机内、有组织的、可共享的大量数据的集合，具有永久存储、有组织、可共享三个基本特点。
* 数据库管理系统（DataBase Management System，DBMS）：是位于用户与操作系统之间的一层数据管理软件。
* 数据库系统（DataBase System，DBS）：是有数据库、数据库管理系统（及其应用开发工具）、应用程序和数据库管理员（DataBase Administrator DBA）组成的存储、管理、处理和维护数据的系统。
* 实体（entity）：客观存在并可相互区别的事物称为实体。
* 属性（attribute）：实体所具有的某一特性称为属性。
* 码（key）：唯一标识实体的属性集称为码。
* 实体型（entity type）：用实体名及其属性名集合来抽象和刻画同类实体，称为实体型。
* 实体集（entity set）：同一实体型的集合称为实体集。
* 联系（relationship）：实体之间的联系通常是指不同实体集之间的联系。
* 模式（schema）：模式也称逻辑模式，是数据库全体数据的逻辑结构和特征的描述，是所有用户的公共数据视图。
* 外模式（external schema）：外模式也称子模式（subschema）或用户模式，它是数据库用户（包括应用程序员和最终用户）能够看见和使用的局部数据的逻辑结构和特征的描述，是数据库用户的数据视图，是与某一应用有关的数据的逻辑表示。
* 内模式（internal schema）：内模式也称为存储模式（storage schema），一个数据库只有一个内模式。他是数据物理结构和存储方式的描述，是数据库在数据库内部的组织方式。

### 常用数据模型

* 层次模型（hierarchical model）
* 网状模型（network model）
* 关系模型（relational model）
    * 关系（relation）：一个关系对应通常说的一张表
    * 元组（tuple）：表中的一行即为一个元组
    * 属性（attribute）：表中的一列即为一个属性
    * 码（key）：表中可以唯一确定一个元组的某个属性组
    * 域（domain）：一组具有相同数据类型的值的集合
    * 分量：元组中的一个属性值
    * 关系模式：对关系的描述，一般表示为 `关系名(属性1, 属性2, ..., 属性n)`
* 面向对象数据模型（object oriented data model）
* 对象关系数据模型（object relational data model）
* 半结构化数据模型（semistructure data model）

### 常用 SQL 操作
`SQL`:  Structured Query Language

* 类别
    * `DDL：Data Definition Language`:允许用户定义数据，也就是创建表、删除表、修改表结构这些操作
    * `DML：Data Manipulation Language`:为用户提供添加、删除、更新数据的能力
    * `DQL：Data Query Language`:允许用户查询数据

SQL语言关键字不区分大小写！！！但是，不同的数据库表名和列名大小写要求可能不相同。

<table>
  <tr>
    <th>对象类型</th>
    <th>对象</th>
    <th>操作类型</th>
  </tr>
  <tr>
    <td rowspan="4">数据库模式</td>
    <td>模式</td>
    <td><code>CREATE SCHEMA</code></td>
  </tr>
  <tr>
    <td>基本表</td>
    <td><code>CREATE SCHEMA</code>，<code>ALTER TABLE</code></td>
  </tr>
    <tr>
    <td>视图</td>
    <td><code>CREATE VIEW</code></td>
  </tr>
    <tr>
    <td>索引</td>
    <td><code>CREATE INDEX</code></td>
  </tr>
    <tr>
    <td rowspan="2">数据</td>
    <td>基本表和视图</td>
    <td><code>SELECT</code>，<code>INSERT</code>，<code>UPDATE</code>，<code>DELETE</code>，<code>REFERENCES</code>，<code>ALL PRIVILEGES</code></td>
  </tr>
    <tr>
    <td>属性列</td>
    <td><code>SELECT</code>，<code>INSERT</code>，<code>UPDATE</code>，<code>REFERENCES</code>，<code>ALL PRIVILEGES</code></td>
  </tr>
</table>

> SQL 语法教程：[runoob . SQL 教程](http://www.runoob.com/sql/sql-tutorial.html)

### 关系型数据库

* 基本关系操作：查询（选择、投影、连接（等值连接、自然连接、外连接（左外连接、右外连接））、除、并、差、交、笛卡尔积等）、插入、删除、修改
* 关系模型中的三类完整性约束：实体完整性、参照完整性、用户定义的完整性

#### 查询
- 简单查询：`SELECT * FROM <表名>`; SELECT语句其实并不要求一定要有FROM子句，可以用来判断当前到数据库的连接是否有效。e.g. 许多检测工具会执行一条SELECT 1;来测试数据库连接。
- 条件查询：`SELECT`语句可以通过`WHERE`条件来设定查询条件，查询结果是满足查询条件的记录。优先级`NOT`>`AND`>`OR`。语句为`SELECT * FROM <表名> WHERE <条件表达式>`
- 投影查询：如果我们只希望返回某些列的数据，而不是所有列的数据，我们可以用`SELECT 列1, 列2, 列3 FROM ...`，让结果集仅包含指定列。这种操作称为`投影查询`。重命名语法： `SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM ...`
- 排序：查询结果集通常是按照`主键`排序，需要按照其他条件排序时，可以加上`ORDER BY`子句，可以对多列进行升序、倒序排序，其默认为`ASC`，倒叙需要显式表明为`DESC`。 如果有`WHERE`子句，那么`ORDER BY`子句要放到`WHERE`子句后面
- 分页查询：使用`LIMIT <M> OFFSET <N>`可以对结果集进行分页，每次查询返回结果集的一部分，OFFSET可省略，默认为0；`MySQL`则可以简写成`LIMIT <M>, <N>`;（即每页最多`LIMIT`条记录）
- 聚合查询：对于统计总数、平均数这类计算，SQL提供了专门的聚合函数(`COUNT`,`SUM`,`AVG`,`MAX`,`MIN`, etc.)，使用聚合函数进行查询，就是聚合查询。通常，使用聚合查询时，我们应该给列名设置一个别名，便于处理结果。此外，如果聚合查询的`WHERE`条件没有匹配到任何行，`COUNT()`会返回`0`，而`SUM()`、`AVG()`、`MAX()`和`MIN()`会返回`NULL`
    - 分组聚合：`SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;`此时返回的COUNT的结果不再是一个，而是与组数一致。且也可以使用多个列进行分组。
- 多表查询
    - `笛卡尔查询`，e.g.`SELECT * FROM <表1> <表2>`，其中一般还会使用`表名.列名`来引用列和设置别名以避免重复, 且表名也可以通过`FROM <表名1> <别名1>, <表名2> <别名2>`设置别名
    - `连接查询`，对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上。查询语句一般格式为`SELECT ... FROM tableA JOIN tableB ON tableA.column1 = tableB.column2;`，`JOIN`可选`INNER JOIN`,`LEFT OUTER JOIN`,`RIGHT OUTER JOIN`, `FULL OUTER JOIN`等。JOIN查询仍然可以使用`WHERE`条件和`ORDER BY`排序。

#### 修改数据
增查改删，即`CRUD：Create、Retrieve、Update、Delete`

- `INSERT`:`INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);`；可以一次性添加多条记录，只需要在`VALUES`子句中指定多个记录值，每个记录是由`(...)`包含的一组值
- `UPDATE`:`UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;`；在`UPDATE`语句中，更新字段时可以使用表达式
- `DELETE`: `DELETE FROM <表名> WHERE ...;`

#### 关系模型
主键、外键、索引

外键约束：

通过定义外键约束，关系数据库可以保证无法插入无效的数据。由于外键约束会降低数据库的性能，大部分互联网应用程序为了追求速度，并不设置外键约束。

添加
```SQL
ALTER TABLE students
ADD CONSTRAINT fk_class_id
FOREIGN KEY (class_id)
REFERENCES classes (id);
```
删除
```SQL
ALTER TABLE students
DROP FOREIGN KEY fk_class_id;
```

外键约束可以在一对多、多对多、一对一(e.g.空值和性能原因)关系中构建联系。

#### 索引

* 数据库索引：顺序索引、B+ 树索引、hash 索引
* [MySQL 索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)

创建索引(索引可以有多列)
```SQL
ALTER TABLE students
ADD INDEX idx_name_score (name, score);
```
添加唯一性索引或唯一性约束
```SQL
ALTER TABLE students
ADD UNIQUE INDEX uni_name (name);

ALTER TABLE students
ADD CONSTRAINT uni_name UNIQUE (name);
```


### 数据库完整性

* 数据库的完整性是指数据的正确性和相容性。
    * 完整性：为了防止数据库中存在不符合语义（不正确）的数据。
    * 安全性：为了保护数据库防止恶意破坏和非法存取。
* 触发器：是用户定义在关系表中的一类由事件驱动的特殊过程。

### 关系数据理论

* 数据依赖是一个关系内部属性与属性之间的一种约束关系，是通过属性间值的相等与否体现出来的数据间相关联系。
* 最重要的数据依赖：函数依赖、多值依赖。

#### 范式

* 第一范式（1NF）：属性（字段）是最小单位不可再分。
* 第二范式（2NF）：满足 1NF，每个非主属性完全依赖于主键（消除 1NF 非主属性对码的部分函数依赖）。
* 第三范式（3NF）：满足 2NF，任何非主属性不依赖于其他非主属性（消除 2NF 非主属性对码的传递函数依赖）。
* 鲍依斯-科得范式（BCNF）：满足 3NF，任何非主属性不能对主键子集依赖（消除 3NF 主属性对码的部分和传递函数依赖）。
* 第四范式（4NF）：满足 3NF，属性之间不能有非平凡且非函数依赖的多值依赖（消除 3NF 非平凡且非函数依赖的多值依赖）。

### 数据库恢复

* 事务：是用户定义的一个数据库操作序列，这些操作要么全做，要么全不做，是一个不可分割的工作单位。
* 事物的 `ACID` 特性：原子性、一致性、隔离性、持久性。
    * Atomic，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行； 
    * Consistent，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；
    * Isolation，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；
    * Duration，持久性，即事务完成后，对数据库数据的修改被持久化存储。
* 恢复的实现技术：建立冗余数据 -> 利用冗余数据实施数据库恢复。
* 建立冗余数据常用技术：数据转储（动态海量转储、动态增量转储、静态海量转储、静态增量转储）、登记日志文件。

| Isolation Level | 脏读（Dirty Read） |	不可重复读（Non Repeatable Read） |	幻读（Phantom Read） |
| --------------- | ----------------- | --------------------------------- | ------------------- |
| Read Uncommitted|	Yes	| Yes	| Yes |
| Read Committed	| -	  | Yes	| Yes |
| Repeatable Read	| -	  | -	  | Yes |
| Serializable    |	-	  | -	  |  -  |

### 并发控制

* 事务是并发控制的基本单位。
* 并发操作带来的数据不一致性包括：丢失修改、不可重复读、读 “脏” 数据。
* 并发控制主要技术：封锁、时间戳、乐观控制法、多版本并发控制等。
* 基本封锁类型：排他锁（X 锁 / 写锁）、共享锁（S 锁 / 读锁）。
* 活锁死锁：
    * 活锁：事务永远处于等待状态，可通过先来先服务的策略避免。
    * 死锁：事务永远不能结束
        * 预防：一次封锁法、顺序封锁法；
        * 诊断：超时法、等待图法；
        * 解除：撤销处理死锁代价最小的事务，并释放此事务的所有的锁，使其他事务得以继续运行下去。
* 可串行化调度：多个事务的并发执行是正确的，当且仅当其结果与按某一次序串行地执行这些事务时的结果相同。可串行性时并发事务正确调度的准则。
