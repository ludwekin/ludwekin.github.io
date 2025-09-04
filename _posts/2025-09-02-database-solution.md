---

title : "database design real solution summary"
published : false
---

1. 产线：
    - 干法线
         - 干法线1    
            - 第一次喷涂
                - 一涂烘箱
            - 第二次喷涂
            - 第三次喷涂
    -    - 干法线2 
            - 
         - 干法线3 
            - 收卷
            - 气表
            - 闭合
            - 电表




    - 除味
    - 干法测厚仪
    - 饰面
    - 

## 数据库设计

数据库采用 postgres,使用DBeaver管理。



## 解决问题

物联网搭建完成后，现在需要验证2025年8月的数据，具体背景如下：
    - 本来工厂是人工每天下午6点派人去抄表，现在我们搭建完这套物联网后，可以随时随地抄表。
    - 现需要 对比 8月每一天的 电子读表 和 人工抄表 差距。汇总到 数据库 里面 形成 直观对比。

两行命令，分别查询 到：

```nginx
meta_id | key_id | ts | bool_v | str_v | long_v | dbl_v
```
需要读取里面的 dbl_v 值来做减法，得到当天 电表/气表 插值。

我设计了以下脚本：
```sql
WITH first_row AS (
    SELECT ts, dbl_v
    FROM data_ts_kv
    WHERE meta_id = 1924398362079105025
      AND key_id = 50
      AND ts >= 1756396800000
      AND ts < 1756483199000
    ORDER BY ts ASC
    LIMIT 1
),
last_row AS (
    SELECT ts, dbl_v
    FROM data_ts_kv
    WHERE meta_id = 1924398362079105025
      AND key_id = 50
      AND ts >= 1756396800000
      AND ts < 1756483199000
    ORDER BY ts DESC
    LIMIT 1
)
SELECT 
    f.ts AS first_ts,
    f.dbl_v AS first_value,
    l.ts AS last_ts,
    l.dbl_v AS last_value,
    l.dbl_v - f.dbl_v AS diff
FROM first_row f, last_row l;
```
![image-center](/assets/images/database.png){: .align-center}

此举解决了公司后续此需求的大量人工投入。
现在可以直接返回每一天对应的 电力 和 蒸汽 消耗量。

![image-center](/assets/images/database_result.png){: .align-center}

## 业务拆解

执行：

```sql
SELECT tablename
FROM pg_tables
WHERE schemaname = 'public';
```

得出消息：

tablename            |
---------------------+
data_attribute_kv    |
data_duration        |
data_meta_info       |
data_meta_source     |
data_stat            |
sys_log_error        |
data_ts_kv_latest    |
sys_api_auth         |
sys_log_login        |
sys_log_operation    |
sys_log_mqtt         |
sys_user_token       |
data_ts_kv_dictionary|
sys_language         |
sys_menu             |
data_ts_kv           |
sys_user             |
sys_role             |
sys_role_menu        |
sys_role_user        |

这些表组合在一起，明显是一个 IoT 平台的数据存储模型。


接着查询表的描述：

![image-center](/assets/images/table_info.png){: .align-center}

查看schema图表：

![image-center](/assets/images/schema1.png){: .align-center}

![image-center](/assets/images/schema2.png){: .align-center}

![image-center](/assets/images/schema3.png){: .align-center}

## 搭建实时看板（产线监测）网站


![image-center](/assets/images/realtime_producing.png){: .align-center}