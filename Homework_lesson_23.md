## Домашняя работа
#### Занятие 23 . Процедуры и функции

```
CREATE OR REPLACE FUNCTION tf_test()
RETURNS trigger
AS
$$
BEGIN
	delete from good_sum_mart;
	insert into good_sum_mart SELECT G.good_name, sum(G.good_price * S.sales_qty)
		FROM goods G
		INNER JOIN sales S ON S.good_id = G.goods_id
	GROUP BY G.good_name;
RETURN NULL;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER tr_for_test
AFTER INSERT OR UPDATE OR DELETE
ON sales
FOR EACH ROW
EXECUTE PROCEDURE tf_test();
```