# Linuxtecher7-SystemD

Работа с загрузчиком, ДЗ №7

Создал аккаунт на GitHub - https://github.com/

Предварительно установленное и настроенное следующее ПО:

ПК на Linux c 16 ГБ ОЗУ или виртуальная машина с системой Ubuntu.

Oracle VirtualBox (https://www.virtualbox.org/wiki/Linux_Downloads).

Все дальнейшие действия были проверены при использовании VirtualBox 7.2.6 r172322, хостовая ОС: Ubuntu 24.04 Desktop.

Гостевая система — Ubuntu 24.04.4 LTS. (Сервер и клиент)

Оформить отчет в README-файле в GitHub-репозитории.

Цель:

Systemd — создание unit-файла


1. Написать service, который будет раз в 30 секунд мониторить лог на предмет наличия ключевого слова
   
   (файл лога и ключевое слово должны задаваться в /etc/default).

Для начала создам файл с конфигурацией для сервиса в директории 

/etc/default - из неё сервис будет брать необходимые переменные.

Убедимся, что файла нет

<img width="390" height="81" alt="image" src="https://github.com/user-attachments/assets/7e874a37-98d0-44a7-9b55-f524a1921509" />

И создам его, прописав в нем параметры, и укажем ключевое слово:

<img width="688" height="99" alt="image" src="https://github.com/user-attachments/assets/024fe1e7-1e91-4afd-a547-91c0aec66992" />

Затем создам /var/log/watchlog.log и пишу туда строки на своё усмотрение, плюс ключевое слово **ALERT**

<img width="615" height="178" alt="image" src="https://github.com/user-attachments/assets/7a817a03-2365-4fed-a750-826ba78960f5" />

Создам скрипт:

<img width="518" height="240" alt="image" src="https://github.com/user-attachments/assets/08eb8bc8-c6e7-41a8-959b-ede3c8979bdf" />

Дам права на запуск:

<img width="694" height="194" alt="image" src="https://github.com/user-attachments/assets/b67b9b5f-1274-4daa-9928-a7c5542dd0a9" />

Создам юнит для сервиса:

<img width="691" height="175" alt="image" src="https://github.com/user-attachments/assets/7d2d5395-a684-4c7a-8070-871969315245" />

Создам юнит для таймера:

<img width="686" height="195" alt="image" src="https://github.com/user-attachments/assets/7991646a-730a-4f41-87a3-9106ad494c73" />

Перечитаю все конфигурационные файлы (юниты) и обновим его внутреннюю базу данных о службах:

<img width="402" height="57" alt="image" src="https://github.com/user-attachments/assets/e3e6d37d-be8b-42c3-9949-c6f0d4355be5" />

Затем запущу timer:

<img width="461" height="65" alt="image" src="https://github.com/user-attachments/assets/b08b1759-7291-4de5-a6a8-d1db6306bcae" />

Ошибок нет при запуске.



