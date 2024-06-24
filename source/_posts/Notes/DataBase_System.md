---
title: 《数据库原理》笔记
tags:
  - 笔记
categories: 学习笔记
mathjax: true
abbrlink: Note_Database_System
date: 2024-01-16 4:55:00
description: 《数据库原理》笔记
---
# 一、概述（理解、记忆）
## 1.2. 基本知识
### 1. 数据、数据库
- 数据：表示客观事物的符号。
- 数据类型：文本、图形、图像、声音等。
- 数据库：**长期存储**在计算机内、**有组织**的、**可共享**的大量数据的集合。
### 2. 数据的特征
- 静态特征：数据结构、数据联系、数据约束
- 动态特征：数据操作
### 2. 数据库的特点
1.长期存储 2.有组织 3.可共享
### 3. 数据库管理系统（DBMS）
- 定义：提供给用户，并帮助用户建立、使用和管理数据库的软件系统。
- **是数据库系统（DBS）的核心**。
- 建立在操作系统之上，位于操作系统与用户之间。
### 4. 数据库的主要功能（和SQL语言相对应）
- 数据定义：数据定义语言（DDL），定义数据库的数据对象及其关系
- 数据操纵：数据操纵语言（DML），增删改查
- 事务和运行管理：数据控制语言（DCL），对数据库的安全性、完整性、并发性和恢复性进行保护控制
- 组织、存储和管理数据
- 数据库的建立和维护

<div style="page-break-after: always;"></div>

### 5. 数据库系统（DBS）的结构
- 硬件：计算机硬件和数据库专用硬件
- 软件：操作系统（OS）、数据库管理系统（DBMS）（核心）、开发工具（数据库专用工具、程序设计语言）和应用系统
- 人员：数据库管理员（DBA）、数据库设计人员、应用程序员和最终用户
### 6. 数据库管理员（DBA）的主要职责
- 决定数据库的信息内容和结构
- 决定数据库的存储结构和存取策略
- 定义数据库的安全性要求和完整性约束
### 7. 数据管理技术的3个阶段
- 人工管理阶段
数据不保存、没有结构化、不独立、不共享、冗余度高
- 文件管理阶段
文件系统：数据**可长期保存**但没有结构化、独立性低、共享性低、冗余度高
- **数据库系统阶段**
数据库系统：**数据结构化、独立性高、共享性高、冗余度低**
## 1.3. 数据模型
### 1. 数据模型的组成
1.数据结构 2.数据操作（增删改查） 3.数据的完整性约束
### 2. 数据模型
（实际问题） $\Rightarrow$ 概念模型（高层） $\Rightarrow$ 逻辑模型 $\Rightarrow$ 物理模型（底层）
### 3. 概念模型
1. 概念模型：数据及其关系的图形表示。如：实体-联系图（**E-R图**）
2. 要求：简单、清晰、易于理解，**独立于DBMS**
3. 名词解释
- 表（实体集、关系）：同一类实体的集合
- 元组（行、记录、实体）：客观存在且相互区别的事物
- 属性（列、字段、数据项）：实体所具有的特性
- 域（值域）：属性的取值范围
- 码/键：唯一标识实体的属性或属性组
- 候选键：唯一标识实体的最小属性组
- 主属性：包含在任一候选键中的属性
- 主键：用户选定的候选键
- 外键：一个实体集的属性集，它是另一个实体集的主键
- 超键：在键中删除一个或多个属性后，仍然能唯一标识实体的属性或属性组
- 全键：包含所有属性的候选键
- 联系：实体集之间或内部的关联。类型：一对一、一对多、多对多
- 元/度：表中属性的个数

<div style="page-break-after: always;"></div>

### 4. 逻辑模型
- 定义：概念模型的DBMS表示
- 分类：层次模型、网状模型、**关系模型**、面向对象模型等
- 关系模型
1. 关系（表）
2. 关系模式（表头）：关系的结构描述
3. 关系模型的特点：满足1NF、属性和元组不重复且无序。
4. 关系数据库：支持关系模型的数据库
5. 关系模式的数学表达：
$R(U,F),U=\{A_1,A_2,\cdots,A_n\},F=\{A_i\rightarrow A_j,\cdots\}$


## 1.4. 数据库系统结构
### 1. 数据库系统的模式结构
```
数据库系统
|---应用（一个外模式可以有多个应用）
|---DBMS
|   |---外模式（一个DBMS可以有多个外模式）
|   |---模式（一个）
|   |---内模式（一个）
|---数据库
```

### 2. 数据库系统的三级模式结构
1. 外模式（子模式、用户模式）：面向用户的局部数据的逻辑结构和特征的描述，数据库用户的数据视图
- 一个数据库可以有多个外模式
- 一个外模式可以对应多个应用，一个应用只能对应一个外模式
2. 模式（结构模式、逻辑模式）：数据库中全体数据的逻辑结构和特征的描述
- 一个数据库只有一个模式
- DBMS的核心
3. 内模式（存储模式、物理模式）：数据库在计算机内部的表示方式和数据存储方式
### 3. 数据库系统的二级映像
1. 外模式/模式映像：保证了数据库的逻辑独立性
2. 模式/内模式映像：保证了数据库的物理独立性
3. 逻辑独立性和物理独立性合称数据独立性

## 1.5. 数据库系统的体系结构
1. C/S结构：客户/服务器结构
2. B/S结构：浏览器/服务器结构

外部网络（表示层）$\Leftrightarrow$ 应用服务器（处理层）$\Leftrightarrow$ 内部网络（数据层）

<div style="page-break-after: always;"></div>

# 二、关系运算
## 2.1. 关系数据语言
```
关系数据语言
|---关系演算语言
|   |---关系代数语言（如ISBL）
|   |---元组关系演算语言（如ALPHA）
|   |---域关系演算语言（如QBE）
|---关系代数/演算双重语言：结构化查询语言（SQL）
```
## 2.2. 关系代数语言
### 基本运算
并$\cup$、差$-$、积$\times$、选择$\sigma$、投影$\pi$（注意：投影需要去重，即运算后记录数量可能变少）
### 导出运算
交$\cap$、除$\div$、连接$\Join$
### 核心运算
选择$\sigma$、投影$\pi$、连接$\Join$

## 2.3. 基于关系代数语言的更新运算
### 1. 插入运算
建立一个与原表$R$具有相同属性的临时关系$T$，计算$R\cup T$
### 2. 删除运算
建立一个与原表$R$具有相同属性的临时关系$T$，计算$R-T$
### 3. 修改运算
先删除，再插入

## 2.4. 查询优化
在做笛卡尔积前先尽可能地进行选择和投影，以减少计算量。

## *寄巧
查询不满足某条件的元组（如不能提供A的供应商）：$\pi_{SNO}(S)-\pi_{SNO}(S\Join\sigma_{PNAME=A} \(P\))$

查询满足多重条件的元组（如同时提供A和B的供应商）：
$\pi_{SNO}(S\Join\sigma_{PNAME=A} \(P\))\cap\pi_{SNO}(S\Join\sigma_{PNAME=B} \(P\))$

<div style="page-break-after: always;"></div>

# 三、数据完整性
数据完整性是指：数据的正确性、相容性和一致性
### 1. 实体完整性
- 定义：主属性A的值不能为空值（Not Null）
- 关键词：主键（Primary Key）
### 2. 参照完整性
- 定义：外键列的值必须是其参照表的主键列的值或空值
- 关键词：外键（Foreign Key）
### 3. 用户自定义完整性
- 定义：用户根据应用的需要定义的完整性约束
- 关键词：默认值（Default）、检查（Check）、非空（Not Null）、唯一（Unique）

## 3.4. 完整性控制机制
### 1. 完整性控制3个层面
- 属性级（列级）：对特定属性（列）的条件约束
- 元组级（行级）：元组各个属性之间的约束（如售价必须大于进价）
- 关系级（表级）：不同元组、不同表之间的约束（如外键）
### 2. 完整性控制机制具备的功能
定义功能、检查功能、违约处理
### 3. 定义功能
提供完善的定义完整性约束的功能
### 4. 检查功能
对于增删改的更新操作，提供检查完整性约束违约的功能

两种检查方式：立即检查（单语句）、延迟检查（多语句）
### 5. 违约处理
对于破坏完整性约束的更新操作，提供相应的违约处理功能
1. 拒绝更新：主键、非空、外键
2. 置空：非主属性、外键
3. 级联更新：把违约的更新转化为非违约更新，然后接受更新

# 四、SQL Server
## 结构化查询语言（SQL, Structured Query Language）
## 1. 数据定义语言（DDL, Data Definition Language）
- 相关功能：定义数据的关系及完整性约束
- 关键词：Create、Alter、Drop
## 2. 数据操纵语言（DML, Data Manipulation Language）
- 相关功能：操纵数据（增删改查）
- 关键词：Insert、Delete、Update、Select（对应增删改查）
## 3. 数据控制语言（DCL, Data Control Language）
- 相关功能：控制数据库的安全性、完整性、并发性和恢复性
- 关键词：Grant、Revoke、Deny、Check

<div style="page-break-after: always;"></div>

# 四（一）、数据定义语言（DDL）与完整性约束实现
## 3.1. 数据定义语言（DDL）
- 操作：创建Create、修改ALTER、删除DROP
- 对象：数据库Database、表Table、视图View、索引Index、登录Login、数据库用户User、数据库角色Role、触发器Trigger、断言Assertion、过程Procedure
- 数据库（存储文件）属于内模式，表属于模式，视图属于外模式，索引隶属于表。
- 数据库和断言没有修改操作

## 3.2. 数据库和表的定义
### 1. 创建数据库
```sql
Create Database EBook<数据库名>
[On(Name = 'EBook'<数据库逻辑名称>,
    FileName = 'D:\DataBases\EBook.mdf'<数据库物理名称>,
    -- 创建前需要先创建文件夹
    Size = 5MB<初始大小>,
    FileGrowth = 1MB<增长大小>,
    MaxSize = 100MB<最大大小>)]
[Log On(Name = 'EBook_log'<日志逻辑名称>,
    FileName = 'D:\DataBases\EBook_log.ldf'<日志物理名称>,
    Size = 1MB<初始大小>,
    FileGrowth = 5%<增长大小(可以是百分比)>,
    MaxSize = Unlimited<最大大小(可以是无限)>)]
```
### 2. 删除数据库
```sql
Drop Database EBook
```
### 3. 创建表
```sql
Use EBook
Create Table SyT<表名>
(
    -- 格式：<列名> <数据类型> [属性级完整性约束条件],
    No int,
    Name char(8) Unique,
    Code char(6) Check(Code like 'Sy[0-9][0-9][0-9][0-9]'),
    -- [表级完整性约束条件]
    Constraint PK_SyT Primary Key(No),
)
```

<div style="page-break-after: always;"></div>

### 4. 修改表（修改表的结构，属于DDL）
```sql
Use EBook
-- 添加属性：Add <列名> <数据类型> [属性级完整性约束条件]
Alter Table SyT
    Add Age int Not Null
-- 修改属性：Alter Column <列名> <新数据类型>
Alter Table SyT
    Alter Column Name char(10)
-- 删除属性：Drop Column <列名>
Alter Table SyT
    Drop Column Age
```

### 5. 删除表
```sql
Use EBook
Drop Table SyT
```

### 6. 数据类型
- 数值型：int、float
- 字符型：char(n)
- 日期型：date
- 图片型：image

<div style="page-break-after: always;"></div>

## 3.3. 完整性约束实现
### 1. 实体完整性（主键）
```sql
--建表时，属性级
No int [Constraint PK_No] Primary Key
--建表时，关系级,可以是属性组
[Constraint PK_No] Primary Key(No,Name)
--修改表时，关系级（Primary Key要求属性已为NOT NULL）
Alter Table SyT
    Add [Constraint PK_No] Primary Key(No,Name)
```

### 2. 参照完整性（外键）
```sql
--建表时，关系级
No int [Constraint FK_No] References CCT(No)
--建表时，关系级
[Constraint FK_No] Foreign Key(No) References CCT(No)
--修改表时，关系级，级联更新
Alter Table SyT
    Add [Constraint FK_No] Foreign Key(No) References CCT(No)
    On Update Cascade On Delete Cascade
```

### 3. 用户自定义完整性（默认值、检查、非空、唯一）
```sql
--建表时，属性级（NOT NULL只能在建表时定义）
No int [Constraint NN_No] Not Null
Name char(8) [Constraint UQ_Name] Unique
--建表时，元组级
[Constraint SP] Check(SPrice>PPrice)
--修改表时，属性级
Alter Table SyT
    Add [Constraint CK_Code] Check(Code like 'Sy[0-9][0-9][0-9][0-9]')
--修改表时，属性级，默认值
Alter Table SyT
    Add [Constraint DF_No] Default 0 For Age
```
like：模糊查询，%表示任意多个字符，_表示一个字符，[]表示一个字符集合，[a-z]表示a到z的任意一个字符，[^a-z]表示非a到z的任意一个字符

### 4. 删除完整性约束
```sql
Alter Table SyT
    Drop Constraint DF_No
```

<div style="page-break-after: always;"></div>

# 四（二）、数据操纵语言（DML）与数据查询
## 4.1. 数据操纵语言（DML）
- 操作：增Insert、删Delete、改Update、查Select
- 对象：表Table、视图View

## 4.2. 添加记录
```sql
Use EBook
Insert Into SyT[(No,Name,Code)] Values(1,'张三','Sy0001')
-- 一句只能添加一条记录
Insert Into SyT Values(2,'李四','Sy0002')
-- 不指定列名，列名和值一一对应
```

## 4.3. 删除记录
```sql
Use EBook
Delete From SyT [Where No=1<条件>]
-- 不加条件，删除所有记录
```

## 4.4. 修改记录
```sql
Use EBook
Update SyT Set Name='王五' [Where No=2<条件>]
-- 不加条件，修改所有记录
```

<div style="page-break-after: always;"></div>

## 4.5. 查询记录
```sql
Use EBook
Select [<行：All/Distinct/Top n>] <列：*/<列名1> <显示名1>,<列名2> <显示名2>,...> --行：选择/列：投影
From SyT [Join CCT On(SyT.A=CCT.B)] --连接
[Where <条件>] --选择
[Group By <列名> [Having <条件>]] --分组、筛选分组
[Order By <列名> [Asc/Desc]] --排序
```
1. 笛卡尔积：`Select * From SyT,CCT`
2. 并集：`Select * From SyT Union Select * From CCT`
3. 交集：`Select * From SyT Intersect Select * From CCT`
4. 差集：`Select * From SyT Except Select * From CCT`
5. 条件表达式：
    - 区间：`Where No Between 1 And 3`
    - 列表：`Where No In(1,3)`
    - 匹配：`Where Name Like '张%'`
    - 空值：`Where Name Is Null`
    - 取反：`Where Not No=1`
6. 统计（需要分组，除被统计量外，其他列必须在Group By中）：
    - Count：`Select Count(*) From SyT`
    - Sum：`Select Sum(No) From SyT`
    - Avg：`Select Avg(No) From SyT`
    - Max：`Select Max(No) From SyT`
    - Min：`Select Min(No) From SyT`
7. 嵌套查询：`Select … From SyT Where No In(Select No From CCT Where …)`
8. Having子句：`Select … From SyT Group By HNo Having Count(*)>1`，筛选满足特定条件的分组

<div style="page-break-after: always;"></div>

# 专：SQL例题
## 1. 创建数据库
创建一个名为`EBook`的数据库，数据库文件存放在`D:\DataBases`文件夹下，数据库文件名为`EBook.mdf`，日志文件名为`EBook_log.ldf`，数据库文件初始大小为`5MB`，增长大小为`1MB`，最大大小为`100MB`，日志文件初始大小为`1MB`，增长大小为`5%`，最大大小为`无限`。
## 2. 创建表
创建一个名为`SyT`的表，表中包含`No`、`Name`、`Code`三个属性，其中`No`为整型，`Name`为长度为`8`的字符型，`Code`为长度为`6`的字符型，`No`和`Name`不能为空，`Code`以`Sy`开头，后面跟着`4`位数字。
## 3. 添加记录
向`SyT`表中添加一条记录，`No`为`1`，`Name`为`张三`，`Code`为`Sy0001`。
## 4. 删除记录
删除`SyT`表中`No`为`1`的记录。
## 5. 添加属性
向`SyT`表中添加一个名为`Age`的属性，属性类型为整型，属性值不能为空。
## 6. 修改属性
将`SyT`表中`Name`属性的长度改为`10`。
## 7\*. 准备数据
利用以下语句创建名为`CCT`的表，并向其中添加记录。
```sql
Create Table CCT
(
    No int Not Null,
    Name char(8) Unique,
    Code char(6) Primary Key
)
Insert Into CCT Values(1,'张三','Sy0001')
Insert Into CCT Values(2,'李四','Sy0002')
Insert Into CCT Values(3,'王五','Sy0003')
Insert Into CCT Values(4,'赵六','Sy0004')
Insert Into CCT Values(5,'孙七','Sy0005')
Insert Into CCT Values(6,'周八','Sy0006')
Insert Into CCT Values(7,'吴九','Sy0007')
Insert Into CCT Values(8,'郑十','Sy0008')
Insert Into SyT Values(1,'张三','Sy0001',20)
```
## 7. 实体完整性
将`SyT`表中`No`、`Name`属性组设为主键。
## 8. 参照完整性
将`SyT`表中`Code`属性修改为外键，参照`CCT`表中`Code`属性，级联更新。
## 9. 用户自定义完整性
将`SyT`表中`Age`属性的默认值设为`0`。
## 10\*.准备数据
利用以下语句在`SyT`表中添加记录。
```sql
Insert Into SyT Values(1,'李四','Sy0002',21)
Insert Into SyT Values(1,'王五','Sy0003',22)
Insert Into SyT Values(1,'赵六','Sy0004',23)
Insert Into SyT Values(1,'孙七','Sy0005',24)
Insert Into SyT Values(1,'周八','Sy0006',25)
Insert Into SyT Values(1,'吴九','Sy0007',26)
Insert Into SyT Values(1,'郑十','Sy0008',27)
Insert Into SyT Values(2,'张三','Sy0001',21)
Insert Into SyT Values(2,'李四','Sy0002',22)
Insert Into SyT Values(2,'王五','Sy0003',23)
Insert Into SyT Values(2,'赵六','Sy0004',24)
Insert Into SyT Values(2,'孙七','Sy0005',25)
```
## 10. 查询记录
1. 查询`SyT`表中年龄前5名的学生的序号、姓名、年龄。
2. 统计`SyT`表中名为`张三`的学生的人数。
3. 查询名字在`CCT`表中出现且在`SyT`表中未出现的学生的序号、姓名、编码。

<div style="page-break-after: always;"></div>

# 五、概念模型和逻辑模型
## 5.1. 概念模型（E-R图）
1.实体：方框
2.属性：椭圆（主键用下划线标注）
3.关系：菱形

4.联系：线（实体与属性、实体与关系（标注1对1/1对n））
![](ER.png)

## 5.2. 逻辑模型（关系模型）
主键用下划线标注，外键用波浪线标注

上图的逻辑模型：

实体：
```
供应商（商号（主键）、商名、城市、电话）
项目（项目号（主键）、项目名、单位、负责人）
零件（零件号（主键）、名称、规格、单价）
仓库（仓库号（主键）、面积、电话）
职工（工号（主键）、姓名、性别、职称）
```
关系：
```
供应（商号（主键、外键）、项目号（主键、外键）、供应量）
库存（零件号（主键、外键）、仓库号（主键、外键）、库存量）
工作（工号（主键、外键）、仓库号（主键、外键）、合同）
领导（工号（主键、外键）、工号（主键、外键）、职补）
```

<div style="page-break-after: always;"></div>

# 六、关系规范化
## 6.1. 函数依赖
- 函数依赖：$X\rightarrow Y$，$X$决定$Y$，$Y$依赖于$X$
- 非平凡函数依赖：$X\rightarrow Y$，Y不是X的子集
- 完全函数依赖：$X\rightarrow Y$，$Y$不依赖于$X$的任何真子集，只依赖于$X$，记作$X -F\rightarrow Y$ 
- 部分函数依赖：$X\rightarrow Y$，$Y$依赖于$X$的某个真子集，记作$X -P\rightarrow Y$
- 传递函数依赖：$X\rightarrow Y$，$Y\rightarrow Z$，$Z$依赖于$X$，且$Y$不依赖于$X$，记作$X -T\rightarrow Z$
## 6.2. 范式与关系规范化
### 1. 第一范式（1NF）
- 定义：所有属性都是不可分的基本数据项

### 2. 第二范式（2NF）
- 定义：满足1NF，且每个非主属性完全函数依赖于候选键（消除部分函数依赖）

### 3. 第三范式（3NF）
- 定义：满足2NF，且每个非主属性不传递函数依赖于候选键

### 4. BC范式（BCNF）
- 定义：满足3NF，且任意函数依赖，$X\rightarrow Y$，$X$必包含候选键

### 5. \*第四范式（4NF）：不存在多值依赖
### 6. \*第五范式（5NF）：消除连接依赖

## 6.3. 关系规范化
### 1. Amstrong公理
- 自反律：$Y\subseteq X\Rightarrow X\rightarrow Y$
- 增广律：$X\rightarrow Y\Rightarrow XZ\rightarrow YZ$
- 传递律：$X\rightarrow Y,Y\rightarrow Z\Rightarrow X\rightarrow Z$
- 合成律：$X\rightarrow Y,Y\rightarrow Z\Rightarrow X\rightarrow YZ$
- 分解律：$X\rightarrow YZ\Rightarrow X\rightarrow Y,X\rightarrow Z$
- 伪传递律：$X\rightarrow Y,YW\rightarrow Z\Rightarrow XW\rightarrow YZ$

### 2. \*函数依赖集闭包$F^+$
### 3. 属性集闭包$X^+_F$
- 已知$X$，利用函数依赖集可以计算出的最大属性集
### 4. 最小函数依赖集$F_{min}$
- 从函数依赖集中删除冗余的函数依赖
### 5. 求所有候选键
- 只在左边出现：一定属于每个候选键
- 只在右边出现：一定不属于任何候选键
- 左右都出现：可能属于候选键，闭包计算
- 左右都不出现：一定属于每个候选键（否则无法得到它本身）

# 七、数据安全
## 7.1. 数据保护
主要包括：数据安全、数据完整、数据并发和数据恢复
## 7.2. 安全标准
4组7个等级：$D,C_1,C_2,B_1,B_2,B_3,A_1$
## 7.3. 数据安全
包括：用户鉴别、数据存取控制、视图、审计与数据加密
### 1. 用户鉴别
鉴别方式：静态鉴别（用户名和密码）和动态鉴别（验证码）等
### 2. 数据存取控制（SQL的DCL）
- Grant：授权
- Revoke：撤销
- Deny：拒绝

### 3. 视图
- 定义：从一个或多个基本表中导出的虚表，局部数据的临时体现
- 作用
（1）简化用户操作
（2）清晰表达查询
（3）同一数据可以以不同形式提供给不同用户
（4）在一定程度上确保了数据安全
（5）在一定程度上提供了逻辑独立性
- 数据操纵与表一致，关键词由`Table`改为`View`
- 更新视图：（表内数据同步更新）

### 4. 审计
### 5. 数据加密
常用的加密方法：替换方法、置换方法和混合方法等。

<div style="page-break-after: always;"></div>

# 八、并发控制
## 8.1. 事务
- 用户定义的一个数据操作序列，这些操作可作为一个完整的工作单元，要么全部执行，要么全部不执行，是一个不可分割的工作单位。
- 事务中的操作一般是对数据的更新操作，包括增、删、改。
- 以`BEGIN TRANSACTION`语句开始，以`COMMIT`语句（正常）或`ROLLBACK`语句（异常）结束
- 特征：原子性、一致性、隔离性、持久性
## 8.2. 并发控制错误
### 1. 丢失修改
事务T1,T2同时读入同一数据并加以修改，T2的提交结果会破坏T1提交的结果
### 2. 读脏数据
事务T1修改数据后撤销，使得T2读取的数据与数据库中不一致
### 3. 不可重复读
事务T1读取数据后，事务T2执行更新操作，使T1无法再现前一次读取结果
## 8.3. 封锁：最常用的并发控制方法
1. 排他锁（X锁），用于写操作，其他事务不允许读写，也不允许加锁
2. 共享锁（S锁），用于读操作，其他事务允许读，不允许写，允许加S锁，不允许加X锁

## 8.4. 封锁协议
### 1. 一级封锁协议
T1,T2都加X锁（解决丢失修改）
### 2. 二级封锁协议
T1加X锁，T2加S锁（解决丢失修改和不可重复读）
### 3. 三级封锁协议
T1加S锁，T2加X锁（解决丢失修改、不可重复读和读脏数据）

## 8.5. 两段锁协议和可串行化
满足两段锁协议一定可串行化，不满足两段锁协议也可能可串行化。


## 8.6. 封锁粒度
粒度越细，软件复杂性和系统开销也就越小

## 8.7. 活锁
- 由于授权封锁的随机性，导致事务T的封锁请求一直处于等待状态的封锁。即：T封锁一个数据对象时，T始终处于等待状态。
- 解决方法：采用先来先服务的授权策略。并发事务请求封锁数据对象时，按照请求封锁的先后次序依次进行授权。

## 8.8. 死锁
- 两个或两个以上事务在执行过程中，因争夺资源而造成的一种互相等待的现象。
- 预防：一次封锁、顺序封锁
- 诊断：超时法、等待图法

# 九、数据库恢复
## 9.1.  故障管理
- 事务故障：运行的事务T遭到强行终止，使T非正常结束。破坏了原子性。
- 系统故障：导致系统停止运转的任何事件。系统故障需要重新启动系统。
- 介质故障：导致外存储设备故障的任何事件。
- 病毒故障
## 9.2. 建立冗余数据
- 数据恢复的核心技术：建立冗余数据和利用冗余数据实施数据库恢复。
- 建立冗余数据的技术：数据转储和登记日志文件等。
- 数据转储分类：静态海量转储、静态增量转储、动态海量转储和动态增量转储，各自的含义。

# 十、数据库设计
数据库设计方法：手工经验法，新奥尔良法，ER图法，3NF法等

