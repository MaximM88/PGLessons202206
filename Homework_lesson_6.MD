## Домашняя работа
#### Занятие 6. Физический уровень PostgreSQL
### `1. Создайте виртуальную машину c Ubuntu 20.04 LTS (bionic) в GCE типа e2-medium в default VPC в любом регионе и зоне, например us-central1-a. Поставьте на нее PostgreSQL 14 через sudo apt. Проверьте что кластер запущен через sudo -u postgres pg_lsclusters. Зайдите из под пользователя postgres в psql и сделайте произвольную таблицу с произвольным содержимым postgres=# create table test(c1 text); postgres=# insert into test values('1'); \q. Остановите postgres например через sudo -u postgres pg_ctlcluster 14 main stop`


Создана ВМ на Yandex Cloud. Создан кластер и таблица
![01](https://github.com/MaximM88/PGLessons202206/blob/main/4-1.png?raw=true)

### `2. Создайте новый standard persistent диск GKE через Compute Engine -> Disks в том же регионе и зоне что GCE инстанс размером например 10GB. Добавьте свеже-созданный диск к виртуальной машине - надо зайти в режим ее редактирования и дальше выбрать пункт attach existing disk. Проинициализируйте диск согласно инструкции и подмонтировать файловую систему`

![2](https://github.com/MaximM88/PGLessons202206/blob/main/4-2.png?raw=true)

### `3. Сделайте пользователя postgres владельцем /mnt/data - chown -R postgres:postgres /mnt/data/. Перенесите содержимое /var/lib/postgres/14 в /mnt/data - mv /var/lib/postgresql/14 /mnt/data. Попытайтесь запустить кластер - sudo -u postgres pg_ctlcluster 14 main start. Напишите получилось или нет и почему`
Запустить не получилось, т.к. postgres не смог найти каталог с данными
![3](https://github.com/MaximM88/PGLessons202206/blob/main/4-3.png?raw=true)

### `4. Задание: найти конфигурационный параметр в файлах раположенных в /etc/postgresql/10/main который надо поменять и поменяйте его. Напишите что и почему поменяли`
Изменил в файле postgresql.conf параметр data_directory с /var/lib/postgresql/14 на /mnt/data/14/main


### `5.  Попытайтесь запустить кластер - sudo -u postgres pg_ctlcluster 14 main start.  Напишите получилось или нет и почему. Зайдите через через psql и проверьте содержимое ранее созданной таблицы`
Запустить кластер получилось, т.к. кластер работает с новым каталогом data_directory. Содержимое таблицы сохранилось.
![6](https://github.com/MaximM88/PGLessons202206/blob/main/4-4.png?raw=true)

### `6. Задание со звездочкой *: не удаляя существующий GCE инстанс сделайте новый, поставьте на его PostgreSQL, удалите файлы с данными из /var/lib/postgres, перемонтируйте внешний диск который сделали ранее от первой виртуальной машины ко второй и запустите PostgreSQL на второй машине так чтобы он работал с данными на внешнем диске, расскажите как вы это сделали и что в итоге получилось.`
Установил PG на новой ВМ. Остановил кластер. Подключил внешний диск со старой ВМ на новую. Далее, чтобы с диском можно было работать, смонтировал файловую систему в каталог /mnt/data. Назначил его владельцем пользователя postgres. Изменил в файле /etc/postgresql/14/main/postgresql.conf параметр data_directory с /var/lib/postgresql/14 на /mnt/data/14/main. Попытался запустить кластер. Кластер успешно запустился. Сделал select к ранее созданной таблице. Данные на месте.
![8](https://github.com/MaximM88/PGLessons202206/blob/main/4-5.png?raw=true)
![8](https://github.com/MaximM88/PGLessons202206/blob/main/4-6.png?raw=true)