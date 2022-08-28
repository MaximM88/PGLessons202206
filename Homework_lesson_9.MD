## Домашняя работа
#### Занятие 9 . Блокировки
- ##### Настройте сервер так, чтобы в журнал сообщений сбрасывалась информация о блокировках, удерживаемых более 200 миллисекунд. Воспроизведите ситуацию, при которой в журнале появятся такие сообщения.
ALTER SYSTEM SET log_lock_waits TO on;
ALTER SYSTEM SET deadlock_timeout TO 200;
SELECT pg_reload_conf(); 
В 1 сессии: begin; create index on t(id); 
Во 2 сессии: update t set val = 'val4u' where id = 5;
В 1 сессии : commit; 
	
/var/log/postgresql/postgresql-14-main.log
```	
2022-08-21 11:00:33.768 UTC [5674] postgres@test LOG:  process 5674 still waiting for RowExclusiveLock on relation 16386 of database 16384 after 200.108 ms at character 8
2022-08-21 11:00:33.768 UTC [5674] postgres@test DETAIL:  Process holding the lock: 5502. Wait queue: 5674.
2022-08-21 11:00:33.768 UTC [5674] postgres@test STATEMENT:  update t set val = 'val4u' where id = 5;
2022-08-21 11:00:54.309 UTC [5674] postgres@test LOG:  process 5674 acquired RowExclusiveLock on relation 16386 of database 16384 after 20741.360 ms at character 8
2022-08-21 11:00:54.309 UTC [5674] postgres@test STATEMENT:  update t set val = 'val4u' where id = 5;
```
- ##### Смоделируйте ситуацию обновления одной и той же строки тремя командами UPDATE в разных сеансах. Изучите возникшие блокировки в представлении pg_locks и убедитесь, что все они понятны. Пришлите список блокировок и объясните, что значит каждая.
 locktype | relation |       mode       | granted | pid  | wait_for	|  |
----------|----------|------------------|---------|------|----------|--|
 relation | t        | RowExclusiveLock | t       | 8054 | {}		| - блокировка таблицы 1 транзакией |
 relation | t        | RowExclusiveLock | t       | 8062 | {8054}	| - блокировка таблицы 2 транзакией |
 tuple    | t        | ExclusiveLock    | t       | 8062 | {8054}	| - блокировка версии строки, блокировка получена |
 relation | t        | RowExclusiveLock | t       | 8068 | {8062}	| - блокировка таблицы 3 транзакией |
 tuple    | t        | ExclusiveLock    | f       | 8068 | {8062}	| - блокировка версии строки 3 транзакией, блокировка не получена |


- ##### Воспроизведите взаимоблокировку трех транзакций. Можно ли разобраться в ситуации постфактум, изучая журнал сообщений?
	Разобраться в причинах возникновения дэдлока возможно. В лог выводится информация о блокировках, в том числе запросы, которые вызвали блокировки.	

```2022-08-28 09:17:04.788 UTC [59937] postgres@test STATEMENT:  update t set val = '1upd6' where id = 6;
2022-08-28 09:17:04.788 UTC [59937] postgres@test ERROR:  deadlock detected
2022-08-28 09:17:04.788 UTC [59937] postgres@test DETAIL:  
		Process 59937 waits for ShareLock on transaction 771; blocked by process 60280.
		Process 60280 waits for ShareLock on transaction 770; blocked by process 60058.
		Process 60058 waits for ShareLock on transaction 769; blocked by process 59937.
		Process 59937: update t set val = '1upd6' where id = 6;
		Process 60280: update t set val = '3upd5' where id = 5;
		Process 60058: update t set val = '2upd4' where id = 4;
```

- ##### Могут ли две транзакции, выполняющие единственную команду UPDATE одной и той же таблицы (без where), заблокировать друг друга? Попробуйте воспроизвести такую ситуацию.
	На мой взгляд взаимоблокироваться они не могут, т.к. по сути устанавливается блокировка на всю таблицу
