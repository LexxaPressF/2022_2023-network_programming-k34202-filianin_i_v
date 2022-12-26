University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2022/2023
Group: K34202
Author: Malyshev Alexey Andreevich
Lab: Lab4
Date of create: 20.12.2022
Date of finished: 26.12.2022
---
### I Подготовка
Склонирован репозиторий [p4lang/tutorials](https://github.com/p4lang/tutorials), установлен `Vagrant`, через него развернута виртуальная машина.

![1](https://user-images.githubusercontent.com/57321062/209579005-e1ade413-ab4e-4ef4-94f7-a27e61ff253a.png)
![2](https://user-images.githubusercontent.com/57321062/209579009-91f4890c-4b2c-4692-8d83-3cc9c115220e.png)

> Заметка: На поднятой машине есть два аккаунта - p4 и vagrant, каждый из которых отвечает за работу соответствующего инструмента.

Перейдем к практической части лабораторной работы.

### II Implementing Basic Forwarding
Командами make build && make run (под учеткой p4) запущен `mininet`, а после проверена доступность узлов в нем.
![3](https://user-images.githubusercontent.com/57321062/209579016-0e2d3d7b-4862-4508-a9c2-a328ad31b8f1.png)

Изменен код в файле basic.p4:
1. В парсер добавлены заголовки ethernet_t, ipv4_t;
2. Изменена функция action ip4_forward: 
2.1 Определен выходной порт,
2.2 Обновлен адреса назначения и источника пакетов,
2.3 Изменено значение TTL;
3. Добавлена валидация заголовка ipv4.

![4](https://user-images.githubusercontent.com/57321062/209579020-4fe87da1-0213-467a-a25a-3caf7d4663c1.png)
![5](https://user-images.githubusercontent.com/57321062/209579023-d07ba2f1-1a7d-422b-b180-443c6499e7df.png)
![6](https://user-images.githubusercontent.com/57321062/209579026-1ebb2685-c094-45db-92f5-afdc0da02cc5.png)


После проверена работа измененного скрипта.
![7](https://user-images.githubusercontent.com/57321062/209579027-e4895b18-f102-45c3-8680-996f7030c634.png)

Все работает, настройка пересылок пакетов окончена, переходим к туннелированию.
### III - Implementing Basic Tunneling
После нужно было изменит код в basic_tunnel.p4:
1. В парсер добавлена функция реализующая заголовок туннеля; 
2. Добавлен action myTunnel_forward;
3. Добавлена таблица myTunnel_exact;
4. Добавлена функция определяющая необходимость применения myTunnel_exact, в зависимости от заголовка;
5. В Депарсер добавлена поддержка внесенных изменений.

![8](https://user-images.githubusercontent.com/57321062/209579030-0c4d70a7-d2a8-4223-8bb1-296a78eb6caf.png)
![9](https://user-images.githubusercontent.com/57321062/209579035-331f2d96-93d1-4afa-a025-bf6ec02c7ff7.png)
![10](https://user-images.githubusercontent.com/57321062/209579038-bf77372b-0ee6-4da3-bbe9-165d33e91d53.png)
![11](https://user-images.githubusercontent.com/57321062/209579041-28eb86bb-991d-4f90-bdae-d4923ccb4c25.png)
![12](https://user-images.githubusercontent.com/57321062/209579044-ac5cece2-fe9f-4d3b-80f0-f7c88c5db1a8.png)

Снова проведена проверка доступности сети.
![13](https://user-images.githubusercontent.com/57321062/209579047-f7030d16-d30a-4e5c-ac53-dbac58cfeb92.png)

И проверка работы скрипта о туннелировании.
![14](https://user-images.githubusercontent.com/57321062/209579051-5f12d26d-1696-40da-a7af-607670ba92e4.png)

### IV Итоги
1. Схема сети mininet:
![15](https://user-images.githubusercontent.com/57321062/209579059-38a2284a-2f41-4bf9-94e0-2df470d15ffb.png)

2. Получен опыт работы с P4;
3. Выполнены несколько заданий из обуча.щего репозитория.
