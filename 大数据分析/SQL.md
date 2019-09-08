# SQL基础

>>调用Python脚本
```sql
ADD FILE $curdir/user_doc_generator_filter_session.py;
FROM(
    SELECT
        eventid,
        uid,
        fid,
        weight_info
    FROM
        engine_with_pingback
    WHERE
        dt = '$common_cur_dt'
        AND hour='$common_cur_hour'
        AND method='baseline'
        cluster by eventid
        ) b
    INSERT OVERWRITE DIRECTORY '$cur_hdata_path'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    REDUCE
        eventid, uid, fid,weight_info
    USING
        'python user_doc_generator_filter_session.py user_doc_analyzer_reducer'
    AS
        uid, eventid, fid, weight_info
```
---

## SQL调优
  1. 创建索引；避免索引上引用计算；预编译查询；临时表暂存（减少阻塞提高并发性能）；避免使用having（分组后过滤，开销很大）
  2. 分区dt,strategy统计，用样本估计整体；
  3. 常用列建立索引，但不是越多越好。

## SQL关键字
- +：多个列或者不同表的多个列的值相加；后跟as
- between 66 and 68：选择范围
- like：'_9','%9%'正则匹配单个和多个任意字符，比如姓王的人'王%'
- top……order by……desc：降序后取前5；默认升序
- distinct：去重
- group by：查询的内容必须存在groupby内；
- not null：查询不为空的信息，值判断不等<>
- concat_ws(',',c1,c2)：格式化连接多个字段
- ltrim(rtrim(name))as name：左右去掉多余空格
- round(c1,-1)：c1字段四舍五入，-1处为小数位数
- substring(c1,1,3)：位置处取子串
- stuff(c1,2,2,20170901)as new_number：在c1列插入新的数字
- (len(s1)-len(replace(s2,p,'')))/len(p)：用字符串p换为空，得到缺失长度/p的长度，计算p字符串出现的次数
- upper，lower大小写转换
- replace(s,p,q)：对字符串s的p字串替换为q
- row_number() over (partition by id order by salary desc) rank
- ```sql
  case c1
      when condition1 then result1
      when condition2 then result2
      when condition3 then result3
  else 
      result0
  end new_column_name;

- sum,avg,min,max
- count(distinct(c1))as type：去重后计算种类
- ```sql
  select id,name,sum(a,b,c,d)as grade from student group by id name order by grade desc;
- avg,sum(coalesce(c1,0))：无参数时则非空后的记录进行操作；有参数则空行换数后取平均或者和
- Group by ROLLUP(A, B, C)：首先会对(A、B、C)进行GROUP BY，然后对(A、B)进行GROUP BY，然后是(A)进行GROUP BY，最后对全表进行GROUP BY操作。递进式分组聚合
- GROUP BY CUBE(A, B, C)，首先会对(A、B、C)进行GROUP BY，然后依次是(A、B)，(A、C)，(A)，(B、C)，(B)，(C)，最后对全表进行GROUP BY操作。交叉式分组聚合
- having：分组聚合后进行选择
- asc：递增
- where c1 in (v1,v2,v3)：c1列中是v1,v2,v3的一种即可
- select * into newTable：插入新表
- unique：创建表时保证某个字段不可重
- primary key：值唯一，非NULL

## 子查询
- where型：内层查询结果当作外层查询的比较条件
  ```sql
  SELECT goods_id,goods_name,shop_price FROM goods WHERE goods_id = (SELECT MAX(goods_id) FROM goods);
- from型：内层查询结果作为临时表，供外层sql再此查询，临时表名
- ```sql
  SELECT goods_id,goods_name,cat_id,shop_price FROM(SELECT goods_id,goods_name,cat_id,shop_price FROM goods ORDER BY cat_id ASC,goods_id DESC) AS tmp GROUP BY cat_id;
- exists型：把外层取出来拿到内层sql测试，成立则该行取出
- ```sql
  SELECT c.cat_id,c.cat_name FROM category c WHERE EXISTS (SELECT 1 FROM goods g WHERE g.cat_id = c.cat_id);

## join
```sql
SELECT dname,sum(salary)as s from (
    SELECT a.*, b.salary from (
        SELECT n.id,n.name,d.dname from 
            ntable n ,dtable d 
            where n.dpid = d.did
        )a,stable b 
        where a.id = b.id
    )c GROUP BY dname ORDER BY s desc;
```

## 补充
```sql
##插入
INSERT INTO Websites (name, url, country) VALUES ('stackoverflow', 'http://stackoverflow.com/', 'IND');
##更新（对全表）
UPDATE Websites SET alexa='5000', country='USA' WHERE name='菜鸟教程';
##添加删除修改列
ALTER TABLE table_name ADD column_name datatype
ALTER TABLE table_name DROP COLUMN column_name
##删除（不删除表）
DELETE * FROM table_name;
DELETE FROM Websites WHERE name='百度' AND country='CN';
```
