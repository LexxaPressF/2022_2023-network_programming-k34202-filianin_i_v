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

рисунок1

## II - Работа с ansible

`Ansible` работает через ssh подключение, потому для начала стоит его настроить. Для этого в разделе IP=>SSH указать подключение без шифрования, а затем получить адресса машин относительно `OVPN сервера`. Сделал пробное подключение:
рисунок2

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
Рисунок3

После того как найдены все нужные команды, создан и файл playbook.
Рисунок4

> Заметка: при первом запуске ничего не выполнялось, вывод оказался красным, все дело оказалось в неправильном синтаксисе, я выяснил что yml файлы **не приемлят табуляции**, а также, что **команды** надо писать **на следующей строчке** и каждую **через дефис**

Рисунок5

Изменный playbook на рисунке:
Рисунок6

Он был запущен и получен следующий вывод:
Рисунок7
Рисунок7.1

Состояние микротиков изменилось
Рисунок8

Отдельно проверена работа OSPF:
Рисунок 9

### III Результаты
1) Создали еще один `Mikrotik CHR`;
2) Познакомился с `ansible`;
3) Написал и исполнил сценарий из `ansible`;
4) Настроил `OSPF`, `NTP-клиент` и изучил работу с юзерами, **сразу на обоих** `Mirtotik CHR`.
