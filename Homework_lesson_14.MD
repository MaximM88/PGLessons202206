## Домашняя работа
#### Занятие 14 . Репликация
> На 1 ВМ создаем таблицы test для записи, test2 для запросов на чтение. Создаем публикацию таблицы test и подписываемся на публикацию таблицы test2 с ВМ №2. На 2 ВМ создаем таблицы test2 для записи, test для запросов на чтение. Создаем публикацию таблицы test2 и подписываемся на публикацию таблицы test1 с ВМ №1. 3 ВМ использовать как реплику для чтения и бэкапов (подписаться на таблицы из ВМ №1 и №2 ). Небольшое описание, того, что получилось. 

Созданы таблицы cars и customers на двух ВМ. Использована логическая репликация, на обеих wal_level = logical. В postgresql.conf и pg_hba.conf внесены необходимые настройки для возможности подключения между ВМ.  На 3 ВМ то же самое. Использованы публикации первой таблицы на 1 ВМ и 2 таблицы на 2 ВМ.

|ВМ1						|ВМ2	| ВМ3|
|---------------------------|-------------|--|
|![01](https://github.com/MaximM88/PGLessons202206/blob/main/14-1.png?raw=true)          |	![01](https://github.com/MaximM88/PGLessons202206/blob/main/14-2.png?raw=true)	        | ![01](https://github.com/MaximM88/PGLessons202206/blob/main/14-3.png?raw=true)
|


> \* реализовать горячее реплицирование для высокой доступности на 4ВМ. Источником должна выступать ВМ №3. Написать с какими проблемами столкнулись. 

Использована физическая репликация. Настройки выставлены в соответствии с рекомендациями: на 3 ВМ synchronous_commit=on, на 4 ВМ hot_standby_feedback=off. Каких-то проблем в процессе не возникло, все работает. 
![01](https://github.com/MaximM88/PGLessons202206/blob/main/14-4.png?raw=true)