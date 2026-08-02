# LinuxTecher7-SystemD

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


**1. Написать service, который будет раз в 30 секунд мониторить лог на предмет наличия ключевого слова
   
   (файл лога и ключевое слово должны задаваться в /etc/default).**

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

Запускаем сервисы:

<img width="589" height="69" alt="image" src="https://github.com/user-attachments/assets/98bcf530-a272-4951-802d-fef8bf104348" />

Получаем результат:

<img width="689" height="203" alt="image" src="https://github.com/user-attachments/assets/19b54768-abf8-4872-bf5e-68dce51d30d1" />


**2. Установить spawn-fcgi и создать unit-файл (spawn-fcgi.sevice) с помощью переделки init-скрипта**

Устанавливаю spawn-fcgi и необходимые для него пакеты:

Процесс установки:

<img width="686" height="466" alt="image" src="https://github.com/user-attachments/assets/fce59fc6-7632-4c9b-a5c2-95951395bae7" />

Установил

<img width="620" height="285" alt="image" src="https://github.com/user-attachments/assets/94d5e43d-502a-442a-9756-e598db9deab7" />

Сам Init скрипт, который будe переписывать, найду здесь:
https://gist.github.com/cea2k/1318020 

Но перед этим необходимо создать файл с настройками для будущего сервиса в файле /etc/spawn-fcgi/fcgi.conf.

Он должен получится следующего вида:

<img width="635" height="156" alt="image" src="https://github.com/user-attachments/assets/62cbaec3-fba6-4994-bb9f-f0206575ff2a" />

А сам юнит-файл будет примерно следующего вида:

<img width="683" height="259" alt="image" src="https://github.com/user-attachments/assets/09b6913d-dc2d-4ac2-8db4-4f9dba02d7cf" />

Перечитаю все конфигурационные файлы (юниты) и обновим его внутреннюю базу данных о службах:

<img width="688" height="53" alt="image" src="https://github.com/user-attachments/assets/01e02410-6207-4a3d-9463-bd4c547d71dc" />

Убеждаюсь, что все успешно работает:

<img width="689" height="733" alt="image" src="https://github.com/user-attachments/assets/ffa62a92-5298-4d48-8b87-7d5c2f1be585" />

**3. Доработать unit-файл Nginx (nginx.service) для запуска нескольких инстансов сервера с разными конфигурационными файлами одновременно:**

Установим Nginx из стандартного репозитория:

У меня уже установлен пакет:

<img width="688" height="113" alt="image" src="https://github.com/user-attachments/assets/a3879c92-b648-454a-9082-e918f2899974" />

Для запуска нескольких экземпляров сервиса модифицируем исходный service для использования различной конфигурации, 

а также PID-файлов. Для этого создадим новый Unit для работы с шаблонами (/etc/systemd/system/nginx@.service):

<img width="688" height="370" alt="image" src="https://github.com/user-attachments/assets/069a3fb5-5887-4f4a-bc42-e3fd5206aa6f" />

Далее необходимо создать два файла конфигурации (/etc/nginx/nginx-first.conf, /etc/nginx/nginx-second.conf). 

Их можно сформировать из стандартного конфига /etc/nginx/nginx.conf, 

с модификацией путей до PID-файлов и разделением по портам:

<img width="504" height="91" alt="image" src="https://github.com/user-attachments/assets/e2c87681-21e7-4c59-b6e8-2e894ae639d6" />

<img width="559" height="86" alt="image" src="https://github.com/user-attachments/assets/08b99ccd-b279-471f-ad2a-9ee5e6e6fee2" />

<img width="1136" height="414" alt="image" src="https://github.com/user-attachments/assets/a700f3ba-44dc-4fec-8e48-e9ddfa8e31d0" />

<img width="1151" height="375" alt="image" src="https://github.com/user-attachments/assets/d0579497-1358-44e2-96f6-a9f84a69bdba" />

<img width="1161" height="211" alt="image" src="https://github.com/user-attachments/assets/6695b4d4-be72-4899-85ff-b4713be61632" />

Домашнее задание выполнено.
