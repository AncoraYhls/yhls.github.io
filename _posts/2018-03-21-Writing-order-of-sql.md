---
title: sql书写顺序和执行顺序
description: 
categories: notes
tags: SQL
---

> 做项目时碰到这个问题，查了一些资料，做个记录。

### sql书写顺序
```sql
(8)SELECT (9)DISTINCT  (11)<Top Num> <select list>
(1)FROM [left_table]
(3)<join_type> JOIN <right_table>
(2)ON <join_condition>
(4)WHERE <where_condition>
(5)GROUP BY <group_by_list>
(6)WITH <CUBE | RollUP>
(7)HAVING <having_condition>
(10)ORDER BY <order_by_list>
```

### 逻辑查询处理阶段简介

`FROM`：对FROM子句中的前两个表执行笛卡尔积（Cartesian product)(交叉联接），生成虚拟表VT1

`ON`：对VT1应用ON筛选器。只有那些使<join_condition>为真的行才被插入VT2。

`OUTER(JOIN)`：如 果指定了OUTER JOIN（相对于CROSS JOIN 或(INNER JOIN),保留表（preserved table：左外部联接把左表标记为保留表，右外部联接把右表标记为保留表，完全外部联接把两个表都标记为保留表）中未找到匹配的行将作为外部行添加到 VT2,生成VT3.如果FROM子句包含两个以上的表，则对上一个联接生成的结果表和下一个表重复执行步骤1到步骤3，直到处理完所有的表为止。

`WHERE`：对VT3应用WHERE筛选器。只有使<where_condition>为true的行才被插入VT4.

`GROUP BY`：按GROUP BY子句中的列列表对VT4中的行分组，生成VT5.
`CUBE|ROLLUP`：把超组(Suppergroups)插入VT5,生成VT6.

`HAVING`：对VT6应用HAVING筛选器。只有使<having_condition>为true的组才会被插入VT7.

`SELECT`：处理SELECT列表，产生VT8.

`DISTINCT`：将重复的行从VT8中移除，产生VT9.

`ORDER BY`：将VT9中的行按ORDER BY 子句中的列列表排序，生成游标（VC10).

`TOP`：从VC10的开始处选择指定数量或比例的行，生成表VT11,并返回调用者。