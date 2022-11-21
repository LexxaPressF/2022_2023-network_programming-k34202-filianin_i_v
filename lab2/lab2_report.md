University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2022/2023
Group: K34202
Author: Malyshev Alexey Andreevich
Lab: Lab2
Date of create: 08.11.2022
Date of finished: 21.11.2022
---
## I - Второй OVPN client на втором CHR
Через VirtualBox создана новая машина `Mikrotik CHR`, затем на ней настроен второй `OVPN client`.
> Заметка: не удалось клонировать Mikrotik CHR, при создании в новой машине отсутствовали сетевые интерфейсы, поэтому установлен `Mikrotik CHR` другой версии 

> Заметка: OVPN сервер разрешает иметь **только 2 активных соединения**

В итоге имеем 2 активных `CHR'a` подключенных к `OVPN`

![рисунок1](https://user-images.githubusercontent.com/57321062/203110013-8189acfb-9519-4a35-bd22-d53d8dcd9cfe.png)

## II - Работа с ansible

`Ansible` работает через ssh подключение, потому для начала стоит его настроить. Для этого в разделе IP=>SSH указать подключение без шифрования, а затем получить адресса машин относительно `OVPN сервера`. Сделал пробное подключение:

![рисунок2](https://user-images.githubusercontent.com/57321062/203110049-cf3b834d-9ce3-4d2c-bb3a-6c2ae01d7864.png)

Затем, я разобрался со структурой `playbook.yml`, она следующая:
```
--- //Начало yaml файла
  - name: Scenario name //Название, по которому понятно назначение playbook'а
    hosts: routers  //Группа хостов для которых будет выполняться сценарий
    tasks:
    - name: Name of task //Название действия
        community.routeros.command: //Модуль для выполнения команд
            commands: /system license print //Команды, как будто мы их вводим на 
                                            //прямую в Mikrotik 
            register: output    //Получение выходных данных и добавление их в 
                            //переменную с данным названием
    - name: Show output
        debug:
            var: output //Соответственно выводит все выходные данные
```
И так что нам нужно для реализации `playbook'а`:
1) Создать файл hosts с указанием ip'шников роутеров и переменных
> Заметка: файл hosts.ini должен иметь следующий вид:
```
[routers]   //Определенное имя группы
first_Mikrotik ansible_host=1.1.1.1
second_Mikrotik ansible_host=1.1.1.1
//ansible_something - объявление глобальных переменных ansible, в данном
//они понадобятся для ssh-подключения

[routers:vars]
ansible_connection=ansible.netcommon.network_cli
ansible_network_os=community.routeros.routeros
ansible_user=admin
ansible_ssh_pass=admin
```
> Пример взят с https://github.com/ansible-collections/community.routeros

2) Создать сам playbook, для чего нужно найти команды для настройки Mikrotik:
    - Добавление пользователей;
    - Настройки NTP Client;
    - Настройки OSPF;
    - Сбор данных конфига и по топологии OSPF.

Для начала, было сделано самое простое, добавлен файл hosts.

![рисунок3](https://user-images.githubusercontent.com/57321062/203110101-5a192467-099b-436e-854d-5053f009ea43.png)

После того как найдены все нужные команды, создан и файл playbook.

![рисунок4](https://user-images.githubusercontent.com/57321062/203110130-0dd26609-eef4-4592-974a-08fc14f23aef.png)

> Заметка: при первом запуске ничего не выполнялось, вывод оказался красным, все дело оказалось в неправильном синтаксисе, я выяснил что yml файлы **не приемлят табуляции**, а также, что **команды** надо писать **на следующей строчке** и каждую **через дефис**

![рисунок5](https://user-images.githubusercontent.com/57321062/203110439-4bd07f22-178e-4fd9-97c8-a158fc93fb82.png)

Изменный playbook на рисунке:

![рисунок6](https://user-images.githubusercontent.com/57321062/203110480-e017963c-2e0a-430a-a772-b9026c0f06bd.png)

Он был запущен и получен следующий вывод:

![рисунок7](https://user-images.githubusercontent.com/57321062/203110513-36148318-14d3-4cdf-a1e2-8a02c65d34fb.png)

![рисунок7 1](https://user-images.githubusercontent.com/57321062/203110532-a4b7ba38-c649-49de-a3b6-64441d957d99.png)

Состояние микротиков изменилось

![рисунок8](https://user-images.githubusercontent.com/57321062/203110572-4982eb6a-5ae2-47cf-a132-6479d14f9705.png)

Отдельно проверена работа OSPF:

![рисунок9](https://user-images.githubusercontent.com/57321062/203110592-a5c97495-e2c3-4798-8a19-f73db3976ce0.png)

### III Результаты
1) Создали еще один `Mikrotik CHR`;
2) Познакомился с `ansible`;
3) Написал и исполнил сценарий из `ansible`;
4) Настроил `OSPF`, `NTP-клиент` и изучил работу с юзерами, **сразу на обоих** `Mirtotik CHR`.
