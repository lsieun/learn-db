# 20180202 #

## 1、题目 ##

使用case/when添加一个临时字段，然后在order by 中进行排序。

## 2、解决 ##

```sql
SELECT id,plan_id,plan_title,plan_start_time,plan_end_time,is_valid, 
	CASE 
	WHEN CURRENT_TIMESTAMP() >= plan_start_time AND CURRENT_TIMESTAMP() <= plan_end_time THEN 0 
	WHEN CURRENT_TIMESTAMP() > plan_end_time AND CURRENT_TIMESTAMP() <= DATE_ADD(plan_end_time,INTERVAL 30 MINUTE) THEN 1 
	WHEN CURRENT_TIMESTAMP() < plan_start_time THEN 2 WHEN CURRENT_TIMESTAMP() > DATE_ADD(plan_end_time,INTERVAL 30 MINUTE) THEN 3 
	ELSE 4 
	END AS `plan_live_state` 
FROM personal_live_plan 
WHERE 1=1 AND is_valid = 1 
ORDER BY `plan_live_state` ASC, plan_start_time ASC LIMIT 0,10

```



