# Лабораторная работа 4

В данной лабораторной работе будут решена поставленная задача по запуску и соединению двух контейнеров на основе Dockerfile.

## Задание:

1. Запустить в контейнере приложение "aafire".
2. Запустить два контейнера и оставить их в работающем состоянии.
3. Создать сеть и подключить контейнеры к ней.
4. Протестировать соединение при помощи утилиты ping.

## Решение:

1. Запустим в контейнере приложение "aafire". Для этого изменим файл, который был создан по примеру, и внутрь вставим следующее:  
   ```
   FROM ubuntu:latest  

   RUN apt-get update && apt-get install -y libaa-bin
   RUN apt-get install -y iputils-ping
   CMD ["aafire"]
   ```
   Далее введём в терминал команду ```docker build -t aafire .``` для построения образа. Вводим ```docker run --rm -it aafire``` и наслаждаемся результатом.

   ![image](https://github.com/user-attachments/assets/62f641b2-1123-459a-88aa-e329e5c9b46e)

2. По аналогии запустим ещё один контейнер с "aafire". (Теперь наш рабочий стол горит, о нет!)

   ![image](https://github.com/user-attachments/assets/8f4f6581-63c4-475e-aa38-1e2cd8e1912f)

3. Создадим новую сеть между контейнерами и подключим их к ней. Для этого пропишем в новом терминале ```docker network create myNetwork_aafire```. Узнаём названия контейнеров через ```docker ps```. Их названия: elastic_curran и blissful_mcclintock. Подключаем контейнеры к сети:
   ```
   docker network connect myNetwork_aafire elastic_curran
   docker network connect myNetwork_aafire blissful_mcclintock
   ```

   ![image](https://github.com/user-attachments/assets/9dfe8a64-220f-4582-b3d2-16058b769dd2)

4. Для того, чтобы протестировать соединение, воспользуемся утилитой ping, которую мы установили заранее. Прописываем в командную строку ```docker exec -it elastic_curran ping 172.19.0.3``` и получаем, что контейнеры соединены.

   ![image](https://github.com/user-attachments/assets/2a808466-ebdb-40cc-8f14-a137e05aa3c7)

   Проверим то же самое для blissful_mcclintock:

   ![image](https://github.com/user-attachments/assets/381381e5-8759-40cb-b763-92b278242981)

   *Пояснение: IP-адреса были взяты из тех, которые предоставила команда ```docker ps```.*

   ![image](https://github.com/user-attachments/assets/59bccb86-73ac-46c6-8bae-798b174f58db)

Задача выполнена!
