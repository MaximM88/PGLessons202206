## Домашняя работа
#### Занятие 3. Установка PostgreSQL
### `1. Cделать в GCE инстанс с Ubuntu 20.04 или развернуть докер любым удобным способом. Поставить на нем Docker Engine`


Создан инстанс на Yandex Cloud. Установлен Docker.
![01](https://github.com/MaximM88/PGLessons202206/blob/main/3-1.png?raw=true)

### `2. Cделать каталог /var/lib/postgres`

![2](https://github.com/MaximM88/PGLessons202206/blob/main/3-2.png?raw=true)

### `3. Развернуть контейнер с PostgreSQL 14 смонтировав в него /var/lib/postgres`
![3](https://github.com/MaximM88/PGLessons202206/blob/main/3-3.png?raw=true)

### `4. Развернуть контейнер с клиентом postgres. Подключится из контейнера с клиентом к контейнеру с сервером и сделать таблицу с парой строк`

![4](https://github.com/MaximM88/PGLessons202206/blob/main/3-4.png?raw=true)
![4](https://github.com/MaximM88/PGLessons202206/blob/main/3-5.png?raw=true)
### `5. Подключится к контейнеру с сервером с ноутбука/компьютера извне инстансов GCP/места установки докера`
- Подключение с локалхоста ВМ
![6](https://github.com/MaximM88/PGLessons202206/blob/main/3-6.png?raw=true)
###
- Подключение с ноутбука из-под Windows
![7](https://github.com/MaximM88/PGLessons202206/blob/main/3-7.png?raw=true)
### `6. Удалить контейнер с сервером`
![8](https://github.com/MaximM88/PGLessons202206/blob/main/3-8.png?raw=true)

### `7. Создать контейнер заново. Подключится снова из контейнера с клиентом к контейнеру с сервером. Проверить, что данные остались на месте`

![q3](https://github.com/MaximM88/PGLessons202206/blob/main/3-9.png?raw=true)
Все данные сохранились. При создании нового серверного контейнера задал новый пароль, но при подключении по новому паролю в БД зайти не удалось - только по старому.
