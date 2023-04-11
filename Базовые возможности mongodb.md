# Базовые возможности mongodb

### Установить MongoDB одним из способов: ВМ, докер

Для выполнения задания был выбран способ развертывания MongoDB с помощью ВМ, используя Yandex Cloud

Для развертывания MongoDB пользуемся следующими командами:

`wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -` -- импортируем публичный GPG ключ 

`echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list` -- создаём файл /etc/apt/sources.list.d/mongodb-org-6.0.list

`apt update` -- обновляем список пакетов

`apt-get install -y mongodb-org` -- устанавливаем пакеты MongoDB

`systemctl status mongod` -- проверяем статус службы

`systemctl start mongod` -- в случае, если служба не запущена запускаем

`mongosh --port 27017` -- подключаемся к MongoDB используя дефолтный порт

![Успешное подключение к MongoDB](https://i.ibb.co/fGJSHd1/2023-04-11-213120.png)

Создадим пользователя:
```db.createUser( { user: "root", pwd: "asdqwe123", roles: [ "userAdminAnyDatabase", "dbAdminAnyDatabase", "readWriteAnyDatabase" ] } )```


![](https://i.ibb.co/VqZSvWy/2023-04-11-213120.png)

Далее настроем авторизацию, чтобы БД была доступна извне. Для этого правим конфиг /etc/mongod.conf, добавляя строки:
`bindIpAll: true`

`authorization: enabled`

![Настройки конфига](https://user-images.githubusercontent.com/56552417/231258124-609a468e-9d9e-436f-bc11-5729402e3cc7.png)

Перезапускаем службу ```systemctl restart mongod```

После чего логинимся под созданным пользователем ```mongosh --port 27017 -u "root" -p "******" --authenticationDatabase "admin"```

### Заполнить данными

Создадим коллекцию: ```db.createCollection("people")```

Добавим один документ: ```db.people.insertOne( {Тут должен бытьь JSON} )```
![Один документ](https://i.ibb.co/sjZmqj2/2023-04-11-213120.png)
Создадим множество документов: ```db.people.insertMany([ {},{},{},...])```

Просмотрим всё содержимое коллекции: ```db.people.find()```
![](https://i.ibb.co/7r7D0W9/2023-04-11-213120.png)
![](https://i.ibb.co/b5JDRsw/2023-04-11-213120.png)

### Написать несколько запросов на выборку и обновление данных
-- У кого есть питомец по имени Charlie

```db.people.find({"pets": "Charlie"})```
![](https://i.ibb.co/GtFZBjF/2023-04-11-213120.png)
