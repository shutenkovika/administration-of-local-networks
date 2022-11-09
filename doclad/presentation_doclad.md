---
## Front matter
lang: ru-RU
title: Доклад на тему
subtitle: Конфигурирование VLAN. Статическая маршрутизация VLAN.
author: Victoria M. Shutenko
institute: RUDN University, Moscow, Russian Federation
date: 22 March, 2022, Moscow, Russian Federation

## Formatting
toc: false
slide_level: 2
theme: metropolis
header-includes: 
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
aspectratio: 43
section-titles: true
---
# Содержание

- Понятие VLAN
- Назначение VLAN
- Пример настройки VLAN
- Понятие статической маршрутизации
- Назначение статической маршрутизации
- Пример настройки статической маршрутизации

# Понятие VLAN

VLANs (Virtual Local Area Network) – это виртуальные сети, существующие на втором уровне модели OSI. Это значит, что VLAN можно настроить на коммутаторе второго уровня. 

# Назначение VLAN

- Возможность построения сети, логическая структура которой не зависит от физической. Поскольку топология сети на канальном уровне строится независимо от географического расположения составляющих компонентов сети.
- Возможность разбиения одного широковещательного домена на несколько широковещательных доменов. То есть, широковещательный трафик одного домена не проходит в другой домен и наоборот. При этом уменьшается нагрузка на сетевые устройства.
- Возможность обезопасить сеть от несанкционированного доступа. То есть, на канальном уровне кадры с других VLAN будут отсекаться портом коммутатора независимо от того, с каким исходным IP-адресом инкапсулирован пакет в данный кадр.
- Возможность применять политики на группу устройств, которые находятся в одном VLAN.
- Возможность использовать виртуальные интерфейсы для маршрутизации.

# Пример настройки VLAN

![Сеть 1.](image/image1.png){ #fig:001 width=110% }

# Пример настройки VLAN

Cоздание VLAN 100:

```
sw1#configure terminal
sw1(config)#vlan 100
sw1(config-vlan)#name my1
sw1(config-vlan)#
```

Настройка порта:

```
sw1#configure terminal
sw1(config)#interface f0/2
sw1(config-if)#switchport mode access
sw1(config-if)#switchport access vlan 100
```

# Понятие статической маршрутизации

Статические маршруты полностью определены администратором, поэтому они более безопасны, требуют меньше вычислительных ресурсов и более узкую полосу пропускания по сравнению с динамическими маршрутами. Однако сети, использующие статическую маршрутизацию, плохо масштабируемы, при изменении топологии требуется внесение изменений в конфигурацию, что может приводить к ошибкам. Поэтому статическая маршрутизация используется либо в малых сетях, либо в комбинации с протоколами динамической маршрутизации на отдельных участках сети. 

# Назначение статической маршрутизации

- использование в тупиковых сетях, где обмен данными происходит через маршрутизатор, подключенный к одному соседнему маршрутизатору. При рассмотрении статической маршрутизации используется составная сеть. 

- использование при создании маршрута по умолчанию, указывающего путь к сетям, не имеющим соответствующих входов в таблице маршрутизации. 

# Пример настройки статической маршрутизации

![Сеть 2.](image/image3.png){ #fig:001 width=110% }

# Пример настройки статической маршрутизации


Для Router0:

```
Router>en
Router#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#int fa0/0
Router(config-if)#no shutdown
Router(config)#int fa0/0.10
Router(config-subif)#encapsulation dot1q 10
Router(config-subif)#ip address 172.16.10.1 255.255.255.0
Router(config-subif)#int fa0/0.20
Router(config-subif)#encapsulation dot1q 20
Router(config-subif)#ip address 172.16.11.1 255.255.255.0
Router(config-subif)#exit
Router(config)#int fa0/0.30
Router(config-subif)#encapsulation dot1q 30
Router(config-subif)#ip address 172.16.12.1 255.255.255.0
Router(config-subif)#exit
Router(config)#
Router>en
Router#configure terminal
Router(config)#int s0/0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.252
Router(config-if)#clock rate 64000
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#ip route 172.22.30.0 255.255.255.0 192.168.10.2
```

# Пример настройки статической маршрутизации

Для Router1:

```
Router(config-if)#ex
Router(config)#int g0/0
Router(config-if)#ip address 172.22.30.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#int s0/1/0
Router(config-if)#ip address 192.168.10.2 255.255.255.252
Router(config-if)#no shutdown
```

# Библиография
- Кулябов Д.С., Королькова А.В. Архитектура и принципы построения современных сетей и систем телекоммуникаций. - М. РУДН, 2008 http://lib.rudn.ru/polnotekstovye-knigi/61-Kulyabov.pdf
- https://mynetworkinglearningabc.wordpress.com/2014/10/25/general-or-standard-static-routing-protocol-configuration/
- https://intuit.ru/studies/courses/3646/888/lecture/31153

