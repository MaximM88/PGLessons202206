## Домашняя работа
#### Занятие 23 . Процедуры и функции

```
CREATE OR REPLACE FUNCTION tf_test()
RETURNS trigger
AS
$$
DECLARE
c_good_name text;
BEGIN
CASE TG_OP
WHEN 'INSERT'
THEN 
SELECT g.good_name INTO c_good_name FROM goods G WHERE g.goods_id = NEW.good_id;
IF NOT EXISTS (SELECT gsm.good_name FROM good_sum_mart gsm WHERE gsm.good_name = c_good_name) THEN
INSERT INTO good_sum_mart (good_name, sum_sale) VALUES (c_good_name, 0);
END IF;

UPDATE good_sum_mart gsm
SET sum_sale = sum_sale + (
SELECT NEW.sales_qty*g.good_price
FROM goods g WHERE NEW.good_id = g.goods_id
)
WHERE gsm.good_name = c_good_name;

WHEN 'UPDATE' 
THEN
SELECT g.good_name INTO c_good_name FROM goods g WHERE g.goods_id = NEW.good_id;

UPDATE good_sum_mart gsm
SET sum_sale = sum_sale - (
SELECT OLD.sales_qty*g.good_price
FROM goods g WHERE OLD.good_id = g.goods_id
)
WHERE gsm.good_name = c_good_name;

UPDATE good_sum_mart gsm
SET sum_sale = sum_sale + (
SELECT NEW.sales_qty*g.good_price
FROM goods g WHERE NEW.good_id = g.goods_id
)
WHERE gsm.good_name = c_good_name;


WHEN 'DELETE' 
THEN
SELECT g.good_name INTO c_good_name FROM goods g WHERE g.goods_id = OLD.good_id;

UPDATE good_sum_mart gsm
SET sum_sale = sum_sale - (
SELECT OLD.sales_qty*g.good_price
FROM goods g WHERE OLD.good_id = g.goods_id
)
WHERE gsm.good_name = c_good_name;

END CASE;

DELETE FROM good_sum_mart WHERE sum_sale = 0;
RETURN NULL;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER tr_for_test
AFTER INSERT OR UPDATE OR DELETE
ON sales
FOR EACH ROW
EXECUTE PROCEDURE tf_test();
```