# Couchbase

### Развернуть кластер Couchbase

Для выполнения задания был выбран способ развертывания кластера Couchbase с помощью ВМ (Ubuntu 20.04), используя Yandex Cloud.
![](https://i.ibb.co/ZT9ZqsT/2023-05-30-232758.png)

Установка производилась в помощью следующих команд на каждой ноде:
```
sudo apt-get update 
sudo apt-get upgrade -y 
curl -O https://packages.couchbase.com/releases/couchbase-release/couchbase-release-1.0-amd64.deb 
sudo dpkg -i ./couchbase-release-1.0-amd64.deb 
sudo apt-get update 
sudo apt-get install couchbase-server -y
```
Убедимся, что сервис couchbase запущен на всех нодах
![](https://i.ibb.co/LgVfjFS/2023-05-30-233108.png)

Создадим кластер, с помощью "Add server" через веб-интерфейс Couchbase (https://51.250.25.6:18091): 
![](https://i.ibb.co/xfzpkPz/2023-05-30-233440.png)

### Создать БД, наполнить небольшими тестовыми данными

Для наполнения нашей БД воспользуемся готовым набором данных (beer-sample).
![](https://i.ibb.co/k8bbdHY/2023-05-30-233725.png)

Выполним тестовые запросы:
![](https://i.ibb.co/mc6PsZK/2023-05-30-234038.png)

![](https://i.ibb.co/Bcw2D3v/2023-05-30-234126.png)

### Проверить отказоустойчивость

* В настройках стоит по умолчанию  Auto-failover after 120 seconds for up to 1 node

Остановим первую ноду cb1. После чего видим потерю связи с хостом.
![](https://i.ibb.co/ZJhrYhz/2023-05-30-234541.png)
Воспользуемся для подключения к веб-интерфейсу ip другой ноды. Наблюдаем, что первая нода недоступна, через 120 секунд происходит автоматический фейловер, после чего запросы продолжат выполняться.
![](https://i.ibb.co/rMwdZd9/2023-05-30-234626.png)

Произведём ребаланс, после чего нода исключается из кластера.
![](https://i.ibb.co/hMD3BMv/2023-05-30-235414.png)

Вернём ноду в строй. Для этого поднимем обратно cb1 после чего добавим её через "Add server"
![](https://i.ibb.co/CM8nnH0/2023-05-30-235709.png)

Финальным этапом будет запуск ребаланса.
![](https://i.ibb.co/BNvQTJn/2023-05-30-235908.png)

Запросы успешно выполняются.
