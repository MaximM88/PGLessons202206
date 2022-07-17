## Домашняя работа
#### Занятие 2.   SQL и реляционные СУБД. Введение в PostgreSQL
___
>создать новый проект в Google Cloud Platform, Яндекс облако или на любых ВМ, докере , например postgres2022-, где yyyymmdd год, месяц и день вашего рождения (имя проекта должно быть уникально на уровне GCP)

Создан проект на Yandex Cloud с именем cloud-study1
![Creating cloud](https://github.com/MaximM88/PGLessons202206/blob/main/CreateCloud0.png?raw=true)

###
>	далее создать инстанс виртуальной машины Compute Engine с дефолтными параметрами

Первоначально с помощью ssh-keygen сгенерировал ssh-ключ.
Создал ВМ через графический интерфейс. ВМ создана успешно.
![Creating cloud](https://github.com/MaximM88/PGLessons202206/blob/main/CreateVm.png?raw=true)
Также попытался сооздать ВМ через CLI Yandex Cloud:
	
	yc compute instance create --name test03 --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 --zone ru-central1-a --create-boot-disk type=network-hdd,size=15,image-id=fd8qs44945ddtla09hnr,auto-delete --memory 2 --cores 2 --core-fraction 20 --platform standard-v3 --service-account-name maximm --metadata user-data="#cloud-config\nusers:\n  - name: maxim\n    groups: sudo\n    shell: /bin/bash\n    sudo: ['ALL=(ALL) NOPASSWD:ALL']\n    ssh-authorized-keys:\n      - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDs8O121Wj//BYuHjbpyilWwU2YkPiW7yyL+lekCGPl3V9k19TDg4R3Av0ZJ8O+GKPWQmYWmZkSz7hwgIG/OElMs1xwSpBmZFWj4qo0icKyrGuOqyK9/jJazl3CGnbmSRCks4vuOgsYWxWbMW01Sub3ri32H67dNH856rP+GYrd9eLKUzaoMDEmaZFSSZVKXqg/k9TcZQwJnR6FVGSgfY+z6O7vt9yD0R8hbGUYTpPY8ceEFmC2T9/JyFrUxliesh86KqyC+JZIU/dv740J9kAjSSo2y5OIutXJbWnZvMdLYZCz0H/SY76vBiKwwRAHn/FKSNLxKBi9E5iZnovCOeed user@WIN-IS6GQOKQJTD"
ВМ создана успешно, но не получается подключиться по SSH под созданным юзером - запрашивает пароль.
###
>зайти удаленным ssh (первая сессия), не забывайте про ssh-add
поставить PostgreSQL
зайти вторым ssh (вторая сессия)
запустить везде psql из под пользователя postgres
выключить auto commit

![CreatingPGC](https://github.com/MaximM88/PGLessons202206/blob/main/PGConnect.png?raw=true)
###
>сделать в первой сессии новую таблицу и наполнить ее данными create table persons(id serial, first_name text, second_name text); insert into persons(first_name, second_name) values('ivan', 'ivanov'); insert into persons(first_name, second_name) values('petr', 'petrov'); commit;

![CreatingTable](https://github.com/MaximM88/PGLessons202206/blob/main/CreateTable.png?raw=true)
###
>посмотреть текущий уровень изоляции: show transaction isolation level

![IsolationLvl](https://github.com/MaximM88/PGLessons202206/blob/main/IsoLevel.png?raw=true)
###
>начать новую транзакцию в обоих сессиях с дефолтным (не меняя) уровнем изоляции
в первой сессии добавить новую запись insert into persons(first_name, second_name) values('sergey', 'sergeev');
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему?

![q1](https://github.com/MaximM88/PGLessons202206/blob/main/1Q.png?raw=true)
Новая запись не видна. У нас установлен уровень изоляции read committed, при котором не допускается "грязное чтение". Если бы допускалось, то изменения были бы видны. Фантомное чтение не проявляется, так как в первой сессии транзакция не зафиксировала изменения, т.е. не произведен commit.
###
>завершить первую транзакцию - commit;
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему?

![q2](https://github.com/MaximM88/PGLessons202206/blob/main/2Q.png?raw=true)
Новая запись видна, т.к. на уровне изоляции read commited допускается фантомное чтение. Поскольку первая транзакция закоммитилась, то в выборку во второй транзакции попадает добавленная первой транзакцией запись.
###
>начать новые но уже repeatable read транзации - set transaction isolation level repeatable read;
в первой сессии добавить новую запись insert into persons(first_name, second_name) values('sveta', 'svetova');
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему?

![q3](https://github.com/MaximM88/PGLessons202206/blob/main/3Q.png?raw=true)
Новая запись не видна, т.к. на уровне изоляции repeatable read запрещено грязное чтение. На данном этапе могло бы проявиться грязное чтение, т.к. первая танзакция не завершена, если бы оно было разрешено.
###
>завершить первую транзакцию - commit;
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему?

![q4](https://github.com/MaximM88/PGLessons202206/blob/main/4Q.png?raw=true)
Новая запись не видна. Добавляется новая запись в первой транзакции, а во второй эта запись подходит под условия повторяющейся выборки. Могло бы проявиться фантомное чтение, но на уровне repeatable read в PostgreSQL не оно допускается.
###
>завершить вторую транзакцию
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему? 

![q5](https://github.com/MaximM88/PGLessons202206/blob/main/5Q.png?raw=true)

Новая запись видна. Поскольку на данном уровне изоляции транзакция работает как бы со своим снэпшотом данных на момент ее начала, то после завершения транзакции во второй сессии новая транзакция с выборкой из persons начинает работать с обновленным транзакцией в 1 сессии снэпшотом.