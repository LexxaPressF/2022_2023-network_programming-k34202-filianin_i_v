University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2022/2023
Group: K34202
Author: Malyshev Alexey Andreevich
Lab: Lab2
Date of create: 15.12.2022
Date of finished: 21.12.2022
---
## I - Создание Netbox
Создана новая виртуальная машина на `Ubuntu 20.04`.
![1](https://user-images.githubusercontent.com/57321062/209478060-c4172b88-0da1-42ed-a4f2-53ae07f032b3.png)

Установлен `PostgreSQL`, затем настроена база данных и пользователь для работы. `NetBox'а`.
![2](https://user-images.githubusercontent.com/57321062/209478066-f8c7a59e-1695-4355-9572-8d33ae6316c0.png)

Также установлен `Redis`.
> Заметка: Redis - это хранилище ключей и значений в памяти, которое NetBox использует для кэширования и работе с очерелностью.

![3](https://user-images.githubusercontent.com/57321062/209478067-574c678c-4820-406e-924b-78004f4cc408.png)

Наконец установлен сам `Netbox`, далее будут скриншоты с установкой и настройкой.
![4](https://user-images.githubusercontent.com/57321062/209478071-4c35a71f-2ec1-470f-8bfe-4d603a192f33.png)
![5](https://user-images.githubusercontent.com/57321062/209478075-9fce1594-46ab-4a40-8b1d-6a5fbdef50c4.png)
![6](https://user-images.githubusercontent.com/57321062/209478079-a1317429-aaf2-4495-9829-152585fdb637.png)

Также отображаю главные строчки файла конфигурации:
![7](https://user-images.githubusercontent.com/57321062/209478080-8574006f-eb3e-4fa9-b59a-b0492129053e.png)
![8](https://user-images.githubusercontent.com/57321062/209478082-f1376b36-1524-4b29-83b8-30941e117e4c.png)

Последнее действие - запущен upgrade.sh которые выполняет слудующие функции: создает виртуальную среду (venv), запускает миграции и загржает статику
![9](https://user-images.githubusercontent.com/57321062/209478087-a23e1b9b-d78a-4f8a-be51-d9ce15953a4c.png)

## II Работа с Netbox
После создания суперюзера, открыт `Netbox`.
![10](https://user-images.githubusercontent.com/57321062/209478091-a0cf9a78-cc80-4ad3-9e2b-c4cac4bd9f26.png)

Внутри `Netbox'а` описаны роутеры:
![11](https://user-images.githubusercontent.com/57321062/209478930-45817109-d7be-46db-9bda-1b2b4977ba9a.png)

С виртуальной машины с запущенным `Netbox'ом` получен csv файл с содержанием данных о роутерах. В начале был написан `playbook` для чтения данных и вывода данных (на машине с ansible).
![12](https://user-images.githubusercontent.com/57321062/209478098-de29673a-b8a0-4dee-888f-a043df6e63af.png)
![13](https://user-images.githubusercontent.com/57321062/209478099-413a66f3-6a49-4af2-9899-9465d6e91ba5.png)

После скрипт был изменен, так чтобы он изменял названия роутеров, c R1 и R2 на Router 1 и Rоuter 2:
![14](https://user-images.githubusercontent.com/57321062/209478101-93f9c8b4-5820-47ae-8af8-3820361f2644.png)
![15](https://user-images.githubusercontent.com/57321062/209478103-02680bad-38eb-444d-afc1-8a13a9aa3e06.png)

Названия роутеров проверены через `winbox`:
![16](https://user-images.githubusercontent.com/57321062/209478106-e5992126-9c57-4d11-a09b-ae6257b1c69a.png)

После скрипт изменен, для переноса данных еще и в `netbox`:
![18](https://user-images.githubusercontent.com/57321062/209478901-b50e7705-b9e7-4c7f-9c51-076a6e6dce9b.png)

![17](https://user-images.githubusercontent.com/57321062/209478111-c050e3a6-abd2-4ebe-8b83-dc0d063bc313.png)

## III Итоги
1. Получен опыт работы с `Netbox'ом`
2. Изменена структура сети, теперь она выглядит следующим образом:


3. Получен опыт работы с `ansible` со сбором информации
![19](https://user-images.githubusercontent.com/57321062/209478892-b1a9c2e4-5006-4d0a-903f-29b6c83c2de7.png)


Пинги:
![20](https://user-images.githubusercontent.com/57321062/209478122-8afa8190-eccb-4117-b6dc-aa713ff91004.png)
![21](https://user-images.githubusercontent.com/57321062/209478124-7f44bfda-2f17-4fa8-b14b-220723338c87.png)
![22](https://user-images.githubusercontent.com/57321062/209478951-62d3d24d-1792-4446-a1d0-af09d2c36da3.png)


