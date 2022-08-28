## Домашняя работа
#### Занятие 10 . Журналы
- #### Настройте выполнение контрольной точки раз в 30 секунд. 10 минут c помощью утилиты pgbench подавайте нагрузку. Измерьте, какой объем журнальных файлов был сгенерирован за это время. Оцените, какой объем приходится в среднем на одну контрольную точку.
	 Объем журнальных файлов составил 445,75 Мб. В среднем на одну контрольную точку - 22.2 Мб
- #### Проверьте данные статистики: все ли контрольные точки выполнялись точно по расписанию. Почему так произошло?
	Все контрольные точки выполнялись по расписанию
```
2022-08-28 16:44:55.134 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:45:22.114 UTC [5472] LOG:  checkpoint complete: wrote 3072 buffers (18.8%); 0 WAL file(s) added, 0 removed, 2 recycled; write=26.898 s, sync=0.043 s, total=26.981 s; sync files=49, longest=0.022 s, average=0.001 s; distance=22629 kB, estimate=22711 kB
2022-08-28 16:45:25.117 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:45:52.017 UTC [5472] LOG:  checkpoint complete: wrote 1882 buffers (11.5%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.884 s, sync=0.004 s, total=26.900 s; sync files=23, longest=0.004 s, average=0.001 s; distance=16372 kB, estimate=22077 kB
2022-08-28 16:46:25.050 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:46:52.106 UTC [5472] LOG:  checkpoint complete: wrote 1955 buffers (11.9%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.983 s, sync=0.017 s, total=27.056 s; sync files=20, longest=0.008 s, average=0.001 s; distance=17839 kB, estimate=21653 kB
2022-08-28 16:46:55.109 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:47:22.054 UTC [5472] LOG:  checkpoint complete: wrote 1737 buffers (10.6%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.880 s, sync=0.018 s, total=26.946 s; sync files=6, longest=0.014 s, average=0.003 s; distance=19144 kB, estimate=21402 kB
2022-08-28 16:47:25.057 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:47:52.097 UTC [5472] LOG:  checkpoint complete: wrote 1939 buffers (11.8%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.987 s, sync=0.021 s, total=27.041 s; sync files=11, longest=0.012 s, average=0.002 s; distance=19336 kB, estimate=21196 kB
2022-08-28 16:47:55.101 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:48:22.103 UTC [5472] LOG:  checkpoint complete: wrote 1932 buffers (11.8%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.897 s, sync=0.032 s, total=27.003 s; sync files=8, longest=0.013 s, average=0.004 s; distance=21053 kB, estimate=21181 kB
2022-08-28 16:48:25.106 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:48:52.060 UTC [5472] LOG:  checkpoint complete: wrote 2055 buffers (12.5%); 0 WAL file(s) added, 0 removed, 2 recycled; write=26.890 s, sync=0.020 s, total=26.954 s; sync files=13, longest=0.011 s, average=0.002 s; distance=21465 kB, estimate=21465 kB
2022-08-28 16:48:55.063 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:49:22.160 UTC [5472] LOG:  checkpoint complete: wrote 1877 buffers (11.5%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.989 s, sync=0.033 s, total=27.097 s; sync files=7, longest=0.017 s, average=0.005 s; distance=18626 kB, estimate=21181 kB
2022-08-28 16:49:25.163 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:49:52.079 UTC [5472] LOG:  checkpoint complete: wrote 1949 buffers (11.9%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.804 s, sync=0.021 s, total=26.916 s; sync files=10, longest=0.015 s, average=0.003 s; distance=19690 kB, estimate=21032 kB
2022-08-28 16:49:55.082 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:50:22.061 UTC [5472] LOG:  checkpoint complete: wrote 1845 buffers (11.3%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.889 s, sync=0.015 s, total=26.979 s; sync files=7, longest=0.007 s, average=0.003 s; distance=18782 kB, estimate=20807 kB
2022-08-28 16:50:25.064 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:50:52.121 UTC [5472] LOG:  checkpoint complete: wrote 2012 buffers (12.3%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.992 s, sync=0.018 s, total=27.058 s; sync files=10, longest=0.011 s, average=0.002 s; distance=20031 kB, estimate=20729 kB
2022-08-28 16:50:55.125 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:51:22.064 UTC [5472] LOG:  checkpoint complete: wrote 1772 buffers (10.8%); 0 WAL file(s) added, 0 removed, 2 recycled; write=26.815 s, sync=0.022 s, total=26.940 s; sync files=6, longest=0.011 s, average=0.004 s; distance=18253 kB, estimate=20482 kB
2022-08-28 16:51:25.067 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:51:52.109 UTC [5472] LOG:  checkpoint complete: wrote 1948 buffers (11.9%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.981 s, sync=0.017 s, total=27.042 s; sync files=8, longest=0.017 s, average=0.003 s; distance=18223 kB, estimate=20256 kB
2022-08-28 16:51:55.112 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:52:22.068 UTC [5472] LOG:  checkpoint complete: wrote 1890 buffers (11.5%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.884 s, sync=0.023 s, total=26.957 s; sync files=6, longest=0.013 s, average=0.004 s; distance=20046 kB, estimate=20235 kB
2022-08-28 16:52:25.072 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:52:52.125 UTC [5472] LOG:  checkpoint complete: wrote 2146 buffers (13.1%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.981 s, sync=0.024 s, total=27.054 s; sync files=10, longest=0.013 s, average=0.003 s; distance=19759 kB, estimate=20187 kB
2022-08-28 16:52:55.128 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:53:22.076 UTC [5472] LOG:  checkpoint complete: wrote 1819 buffers (11.1%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.880 s, sync=0.022 s, total=26.949 s; sync files=6, longest=0.014 s, average=0.004 s; distance=18361 kB, estimate=20005 kB
2022-08-28 16:53:25.080 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:53:52.057 UTC [5472] LOG:  checkpoint complete: wrote 1984 buffers (12.1%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.883 s, sync=0.017 s, total=26.978 s; sync files=8, longest=0.017 s, average=0.003 s; distance=20473 kB, estimate=20473 kB
2022-08-28 16:53:55.058 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:54:22.112 UTC [5472] LOG:  checkpoint complete: wrote 1806 buffers (11.0%); 0 WAL file(s) added, 0 removed, 2 recycled; write=26.992 s, sync=0.020 s, total=27.055 s; sync files=6, longest=0.012 s, average=0.004 s; distance=19878 kB, estimate=20413 kB
2022-08-28 16:54:25.115 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:54:52.085 UTC [5472] LOG:  checkpoint complete: wrote 2163 buffers (13.2%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.908 s, sync=0.020 s, total=26.970 s; sync files=12, longest=0.012 s, average=0.002 s; distance=20215 kB, estimate=20394 kB
2022-08-28 16:54:55.088 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:55:22.128 UTC [5472] LOG:  checkpoint complete: wrote 1845 buffers (11.3%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.981 s, sync=0.019 s, total=27.041 s; sync files=6, longest=0.016 s, average=0.004 s; distance=20238 kB, estimate=20378 kB
2022-08-28 16:55:25.131 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:55:52.082 UTC [5472] LOG:  checkpoint complete: wrote 1967 buffers (12.0%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.888 s, sync=0.012 s, total=26.951 s; sync files=9, longest=0.012 s, average=0.002 s; distance=19219 kB, estimate=20262 kB
2022-08-28 16:55:55.085 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:56:22.018 UTC [5472] LOG:  checkpoint complete: wrote 1773 buffers (10.8%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.887 s, sync=0.009 s, total=26.933 s; sync files=6, longest=0.005 s, average=0.002 s; distance=18697 kB, estimate=20106 kB
2022-08-28 16:56:25.021 UTC [5472] LOG:  checkpoint starting: time
2022-08-28 16:56:52.019 UTC [5472] LOG:  checkpoint complete: wrote 1757 buffers (10.7%); 0 WAL file(s) added, 0 removed, 1 recycled; write=26.983 s, sync=0.003 s, total=26.998 s; sync files=7, longest=0.003 s, average=0.001 s; distance=16032 kB, estimate=19698 kB
```
- #### Сравните tps в синхронном/асинхронном режиме утилитой pgbench. Объясните полученный результат.
	tps в синхронном/асинхронном режиме 493.736818 / 3012.856434 
	В синхронном режиме все журнальные записи после завершения каждой транзакции должны записываться на диск, что влечет дополнительные наклыдные расходы ввода-вывода.

- #### Создайте новый кластер с включенной контрольной суммой страниц. Создайте таблицу. Вставьте несколько значений. Выключите кластер. Измените пару байт в таблице. Включите кластер и сделайте выборку из таблицы. Что и почему произошло? как проигнорировать ошибку и продолжить работу?
	Из-за ручного изменения файла страница данных, в которой содержится таблица, перестала проходить проверку контрольной суммы. Можно попробовать временно отключить проверку контрольной суммы, выставив параметр ignore_checksum_failure = on.