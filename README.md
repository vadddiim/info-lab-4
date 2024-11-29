# Лабораторная работа №4

## Задание

Необходимо запустить в контейнере приложение “aafire”. Далее в рамках лабораторной работы необходимо самостоятельно настроить сеть между двумя контейнерами. Для проверки сети между контейнерами вам потребуется утилита ping.

## Ход работы

### Огонь

Для начала я создал docker image в файле `Dockerfile`. Он будет работать на основе образа Ubuntu с установленными пакетами libaa-bin (команда aafire) и iputils-ping (команда ping).

```Dockerfile
FROM ubuntu:latest
RUN apt update && apt install -y libaa-bin iputils-ping
```

Далее я сбилдил docker image с помощью команды `sudo docker build . -t fireplace`. Чтобы проверить, что камин с огнем работает, я запустил контейнер `sudo docker run -it fireplace` и прописал `aafire`. Появился огонь из символов.

![Screenshot 2024-11-29 at 16 01@2x](https://github.com/user-attachments/assets/d5728995-1fe8-4c69-b5b7-83ac1bc6e505)

### Подключение машин в сети

Чтобы настроить подключение между двумя контейнерами, заранее я создал docker сеть с помощью команды `sudo docker network create --subnet 192.168.0.0/16 mynetwork`. Далее я запустил в разных консолях два контейнера:

```bash
sudo docker run --net mynetwork --ip 192.168.0.101 -it --name container1 fireplace
sudo docker run --net mynetwork --ip 192.168.0.102 -it --name container2 fireplace
```

И далее нам остается только проверить подключение с помощью команды `ping`, которую мы установили в начале.

```
ping 192.168.0.102 # На первом контейнере
ping 192.168.0.101 # На втором контейнере
```

![Screenshot 2024-11-29 at 16 02@2x](https://github.com/user-attachments/assets/82891541-afed-40ad-9119-3c9cb167e5dd)
![Screenshot 2024-11-29 at 16 03@2x](https://github.com/user-attachments/assets/1210fe53-f0dd-4dc2-8ae3-acbae2bc1b3d)


## Вывод

Таким образом, у меня получилось установить Docker с виртуальным камином, а так же установить связь между двумя Docker-контейнерами.
