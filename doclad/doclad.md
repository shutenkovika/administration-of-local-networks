---
# Front matter
title: "Доклад"
subtitle: "Конфигурирование VLAN. Статическая маршрутизация VLAN."
author: "Виктория Михайловна Шутенко"

# Generic otions
lang: ru-RU
toc-title: "Содержание"

# Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

# Pdf output format
toc: true # Table of contents
toc_depth: 2
lof: true # List of figures
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
  name: el
### Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Misc options
indent: true
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.
  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text

---

# Конфигурирование VLAN

## Понятие VLAN

VLANs (Virtual Local Area Network) – это виртуальные сети, существующие на втором уровне модели OSI. Это значит, что VLAN можно настроить на коммутаторе второго уровня. 

В случае, если отделить VLAN  от понятия «виртуальные сети», то можно сказать, что VLAN – является просто меткой в кадре, который, в свою очередь, может передаваться по сети. Метка содержит номер VLAN, который называется е VLAN ID или VID. На номер отводится 12 бит, следовательно, VLAN может нумероваться от 0 до 4095. Важным следует заметить, что его первый и последний номера зарезервированы и их использовать не нужно. В основном, рабочие станции с VLAN не взаимодействуют, если не конфигурировать VLAN на карточках специально. С ними работают коммутаторы. Порты коммутаторов содержат информацию в каком VLAN они расположены. В связи с этим весь трафик, который выходит через порт помечается меткой, иначе говоря, VLAN. Следовательно, каждый порт имеет PVID (port vlan identifier). Также этот трафик может проходить через другие порты коммутатора(ов), которые тоже находятся в этом VLAN и не пройдут через все остальные порты. Таким образом, создается изолированная среда, которая называется подсеть. Она не может взаимодействовать с другими подсетями без маршрутизатора.

## Назначение VLAN

- Возможность построения сети, логическая структура которой не зависит от физической. Поскольку топология сети на канальном уровне строится независимо от географического расположения составляющих компонентов сети.
- Возможность разбиения одного широковещательного домена на несколько широковещательных доменов. То есть, широковещательный трафик одного домена не проходит в другой домен и наоборот. При этом уменьшается нагрузка на сетевые устройства.
- Возможность обезопасить сеть от несанкционированного доступа. То есть, на канальном уровне кадры с других VLAN будут отсекаться портом коммутатора независимо от того, с каким исходным IP-адресом инкапсулирован пакет в данный кадр.
- Возможность применять политики на группу устройств, которые находятся в одном VLAN.
- Возможность использовать виртуальные интерфейсы для маршрутизации.

## Пример настройки VLAN

### Создание сети

Я создала небольшую сеть. Сеть состоит из: 
Коммутаторы: SW1, SW2.
Компьютеры: PC0.

![Сеть 1.](image/image1.png){ #fig:001 width=60% }

### Настройка VLAN 100 и порта

Я выполнила следующие команды для создания VLAN 100:

```
sw1#configure terminal
sw1(config)#vlan 100
sw1(config-vlan)#name my1
sw1(config-vlan)#
```

Затем я настроила порт, используя команды:

```
sw1#configure terminal
sw1(config)#interface f0/2
sw1(config-if)#switchport mode access
sw1(config-if)#switchport access vlan 100
```
C помощью команды ```show vlan``` проверила настройку.


![Просмотр vlan.](image/image2.png){ #fig:001 width=60% }

# Статическая маршрутизация

## Понятие статической маршрутизации

Маршруты к удаленным сетям могут быть сконфигурированы для каждого маршрутизатора вручную (статическая маршрутизация) или созданы с помощью маршрутизирующих протоколов (динамическая маршрутизация).

Статические маршруты полностью определены администратором, поэтому они более безопасны, требуют меньше вычислительных ресурсов и более узкую полосу пропускания по сравнению с динамическими маршрутами. Однако сети, использующие статическую маршрутизацию, плохо масштабируемы, при изменении топологии требуется внесение изменений в конфигурацию, что может приводить к ошибкам. Поэтому статическая маршрутизация используется либо в малых сетях, либо в комбинации с протоколами динамической маршрутизации на отдельных участках сети. 

## Назначение статической маршрутизации

- использование в тупиковых сетях, где обмен данными происходит через маршрутизатор, подключенный к одному соседнему маршрутизатору. При рассмотрении статической маршрутизации используется составная сеть. 

- использование при создании маршрута по умолчанию, указывающего путь к сетям, не имеющим соответствующих входов в таблице маршрутизации. 

## Пример настройки статической маршрутизации

### Создание сети

Я создала небольшую сеть. Сеть состоит из: 
Коммутаторы: Switch1.
Маршрутизаторы: Router0, Router1.
Компьютеры: PC0, PC1, PC2, PC3.

![Сеть 2.](image/image3.png){ #fig:001 width=60% }

### Настройка статической маршрутизации

Для конфигурирования статической маршрутизации используется команда ip route, которая содержит три параметра: адрес сети назначения, сетевую маску, адрес входного интерфейса следующего маршрутизатора на пути к адресату или идентификатор выходного интерфейса.

Для Switch0 (создание vlan, trunk):


```
Switch(config)#vlan 10
Switch(config-vlan)#ex
Switch(config)#vlan 20
Switch(config-vlan)#ex
Switch(config)#vlan 30
Switch(config-vlan)#ex
Switch(config)#vlan 99
Switch(config-vlan)#ex
Switch(config)#int fa0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit
Switch(config)#int fa0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-if)#ex
Switch(config)#int fa0/3
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 30
Switch(config-if)#exit
Switch(config)#int vlan 30
Switch(config-if)#ip address 172.16.12.5 255.255.255.0
Switch(config-if)#no shutdown
Switch(config-if)#exit
Switch(config)#line vty 0 5
Switch(config-line)#password cisco
Switch(config-line)#enable secret cisco
Switch(config-line)#exit
Switch(config)#ip default-gate 172.16.12.1
Switch(config)#exit
Switch(config)#int fa0/4
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk native vlan 99
Switch(config-if)#exit
Switch(config)#
```

Для Router0:

```
Router>en
Router#configure terminal
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
Router>en
Router#configure terminal
Router(config)#int s0/0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.252
Router(config-if)#clock rate 64000
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#ip route 172.22.30.0 255.255.255.0 192.168.10.2
```

Для Router1:

```
Router(config)#int g0/0
Router(config-if)#ip address 172.22.30.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#int s0/1/0
Router(config-if)#ip address 192.168.10.2 255.255.255.252
Router(config-if)#no shutdown

```
 Используя команду show ip route получила следующие результаты:

![Маршрутизатор 1.](image/image4.png){ #fig:001 width=60% }

![Маршрутизатор 2.](image/image5.png){ #fig:001 width=60% }

# Библиография
- Кулябов Д.С., Королькова А.В. Архитектура и принципы построения современных сетей и систем телекоммуникаций. - М. РУДН, 2008 http://lib.rudn.ru/polnotekstovye-knigi/61-Kulyabov.pdf
- https://mynetworkinglearningabc.wordpress.com/2014/10/25/general-or-standard-static-routing-protocol-configuration/
- https://intuit.ru/studies/courses/3646/888/lecture/31153
