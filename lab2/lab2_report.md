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
## I - ������ OVPN client �� ������ CHR
����� VirtualBox ������� ����� ������ `Mikrotik CHR`, ����� �� ��� �������� ������ `OVPN client`.
> �������: �� ������� ����������� Mikrotik CHR, ��� �������� � ����� ������ ������������� ������� ����������, ������� ���������� `Mikrotik CHR` ������ ������ 

> �������: OVPN ������ ��������� ����� **������ 2 �������� ����������**

� ����� ����� 2 �������� `CHR'a` ������������ � `OVPN`

�������1

## II - ������ � ansible

`Ansible` �������� ����� ssh �����������, ������ ��� ������ ����� ��� ���������. ��� ����� � ������� IP=>SSH ������� ����������� ��� ����������, � ����� �������� ������� ����� ������������ `OVPN �������`. ������ ������� �����������:
�������2

�����, � ���������� �� ���������� `playbook.yml`, ��� ���������:
```
--- //������ yaml �����
  - name: Scenario name //��������, �� �������� ������� ���������� playbook'�
    hosts: routers  //������ ������ ��� ������� ����� ����������� ��������
    tasks:
    - name: Name of task //�������� ��������
        community.routeros.command: //������ ��� ���������� ������
            commands: /system license print //�������, ��� ����� �� �� ������ �� 
                                            //������ � Mikrotik 
            register: output    //��������� �������� ������ � ���������� �� � 
                            //���������� � ������ ���������
    - name: Show output
        debug:
            var: output //�������������� ������� ��� �������� ������
```
� ��� ��� ��� ����� ��� ���������� `playbook'�`:
1) ������� ���� hosts � ��������� ip'������ �������� � ����������
> �������: ���� hosts.ini ������ ����� ��������� ���:
```
[routers]   //������������ ��� ������
first_Mikrotik ansible_host=1.1.1.1
second_Mikrotik ansible_host=1.1.1.1
//ansible_something - ���������� ���������� ���������� ansible, � ������
//��� ����������� ��� ssh-�����������

[routers:vars]
ansible_connection=ansible.netcommon.network_cli
ansible_network_os=community.routeros.routeros
ansible_user=admin
ansible_ssh_pass=admin
```
> ������ ���� � https://github.com/ansible-collections/community.routeros

2) ������� ��� playbook, ��� ���� ����� ����� ������� ��� ��������� Mikrotik:
    - ���������� �������������;
    - ��������� NTP Client;
    - ��������� OSPF;
    - ���� ������ ������� � �� ��������� OSPF.

��� ������, ���� ������� ����� �������, �������� ���� hosts.
�������3

����� ���� ��� ������� ��� ������ �������, ������ � ���� playbook.
�������4

> �������: ��� ������ ������� ������ �� �����������, ����� �������� �������, ��� ���� ��������� � ������������ ����������, � ������� ��� yml ����� **�� �������� ���������**, � �����, ��� **�������** ���� ������ **�� ��������� �������** � ������ **����� �����**

�������5

�������� playbook �� �������:
�������6

�� ��� ������� � ������� ��������� �����:
�������7
�������7.1

��������� ���������� ����������
�������8

�������� ��������� ������ OSPF:
������� 9

### III ����������
1) ������� ��� ���� `Mikrotik CHR`;
2) ������������ � `ansible`;
3) ������� � �������� �������� �� `ansible`;
4) �������� `OSPF`, `NTP-������` � ������ ������ � �������, **����� �� �����** `Mirtotik CHR`.
