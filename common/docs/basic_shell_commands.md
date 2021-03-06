# Основные команды в Ubuntu


### Содержание

- [Образ Ubuntu](#Образ-Ubuntu)
- [Файловая система](#Файловая-система)
    - [Отображение содержимого каталогов](#Отображение-содержимого-каталога)
    - [Поиск](#Поиск)
    - [Управление файлами и каталогами](#Управление-файлами-и-каталогами)
    - [Управление пользователями и группами](#Управление-пользователями-и-группами)
    - [Права доступа](#Права-доступа)
    - [Монтирование устройства хранения данных](#Монтирование-устройства-хранения-данных)
- [Процессы](#Процессы)
- [Менеджер пакетов `apt`](#Менеджер-пакетов-apt)
- [Загрузка файлов посредством `wget`](#Загрузка-файлов-посредством-wget)
- [Архиватор](#Архиватор)
    - [`tar`](#tar)
    - [`zip`/`unzip`](#zip/unzip)
- [Управление сетью](#Управление-сетью)
    - [Настройка сети посредством `netplan`](#Настройка-сети-посредством-netplan)
    - [Отображение состояния и параметров сети](#Отображение-состояния-и-параметров-сети)
        - [`ip addr`](#ip-addr)
        - [`ip link`](#ip-link)
        - [`ip neighbor`](#ip-neighbor)
        - [`ip route`](#ip-route)
    - [Беспроводные устройства](#Беспроводные-устройства)
    - [Контроль трафика](#Контроль-трафика)
        - [`iptables`](#iptables)
        - [`ufw`](#ufw)
    - [Мониторинг соединений](#Мониторинг-соединений)
        - [`ss`](#ss)
        - [`lsof`](#lsof)
        - [`nc`](#nc)
    - [Удаленный доступ](#Удаленный-доступ)
        - [`ssh`](#ssh)
        - [`scp`](#scp)


## Образ Ubuntu

[Ссылка на образ](https://disk.yandex.ru/d/0Hd92rzNB0_IHg)

Установленное ПО:
- Ubuntu 18
- Java (JDK) 8
- Hadoop 3.1.2
- Spark 2.4.7
- Zookeeper 3.6.1
- Kafka 2.5.1
- Anaconda3-2020.02 (`python 3.7`). Note: `python 3.8` is not supported by Spark 2.4.7

Пароль: `ubuntu`

Для доступа к общим каталогам на виртуалке необходимо добавить пользователя `ubuntu` в группу `vboxsf`:

```bash
sudo adduser $USER vboxsf
```

## Полезно знать

Открытие терминала

`Ctrl+Alt+t`

Автозаполнение в терминале

`Tab`

Поиск по истории команд в терминале

`Ctrl+r`

Копирование в буфер обмена

`Ctrl+Shift+c`

Вставка из буфера обмена

`Ctrl+Shift+v`

Справка по командам

```bash
man ls
```

Очистка отображаемых данных в терминале

```bash
clear
```

## Файловая система

### Отображение содержимого каталога

Путь к файлу/каталогу:


- `/home/ubuntu` - абсолютный путь
- `BigData/Spark` - относительный путь
- `~` - домашняя директория пользователя (например, `/home/ubuntu` для пользователя c именем `ubuntu`)
- `..` - родительская директория текущего каталога

Выбор текущей рабочей директории

```bash
cd /home/ubuntu
```

```bash
cd ~
```

```bash
cd ~/BigData
```

```bash
cd BigData
```

```bash
cd ..
```

Просмотр содержимого каталога

```bash
ls /
```

```bash
ls
```

```bash
ls -lha
```

```bash
ls -Rlha
```

Установка утилиты отображения дерева каталогов

```bash
sudo apt install tree
```

Просмотр содержимого каталога

```bash
tree -L 2 /home/ubuntu
```


## Поиск

```bash
sudo find $PATH -name $SUBSTRING_OR_PATTERN
```

```
sudo find / -name *.md
```

## Управление файлами и каталогами


Создание каталога

```bash
mkdir class
```

```bash
mkdir -p class
```
- `p` - создает родительские каталоги, если не существуют, и, если каталог уже существует, не выдает ошибку


```bash
cd class
```

Создание файла

```bash
touch welcome.md
ls -l
```

Добавление содержания

```bash
echo "Hello World!" > welcome.md
```

Отображение содержимого файла

```bash
cat welcome.md
```

```bash
echo "second line" >> welcome.md
cat welcome.md
```

Редактор

```bash
nano welcome.md
```

Копирование

```
cd ..
cp -R class class_copy
tree -L 2 ~
```

Перемещение

```
mkdir class_move
mv class/welcome.md class_move/
tree -L 2 ~
```

Переименование каталога и файла

```
mv class_move class_rename
mv class_rename/welcome.md class_rename/hello.md
tree -L 2 ~
```

Удаление

```
rm -R class class_rename
tree -L 2 ~
```
```
mv class_copy class
```

Ссылки

*Hard link* (только на файлы)

```bash
cd class
ln welcome.md hello.md
ls -l welcome.md hello.md

# изменение содержания welcome.md
echo "content" >> welcome.md
cat hello.md 

# изменение содержания hello.md
echo "more" >> hello.md 
cat welcome.md

rm welcome.md
ls -l
rm hello.md
```

*Soft link*

```bash
# создание soft link
echo "Hello World 1.0" >> welcome_1.0.md
ln -s welcome_1.0.md welcome.md
cat welcome.md
ls -l

# замена ссылки на файл
echo "Hello World 1.1" >> welcome_1.1.md
ln -sfn welcome_1.1.md welcome.md
cat welcome.md
ls -l

echo "updated" >> welcome.md
cat welcome_1.1.md

rm welcome_1.1.md
ls -l

rm welcome.md welcome_1.0.md
```


## Управление пользователями и группами

Вывод пользователей

```bash
cat /etc/passwd
```

```bash
less /etc/passwd
```

1. Имя пользователя
2. Пароль 
3. UID
4. GID (первичная группа)
5. Полное имя
6. Домашний каталог
7. Вход в shell

Текущие пользователи в системе

```bash
users
```

Вывод групп
```bash
cat /etc/group
```

1. Имя группы
2. Пароль
3. GID
4. Пользователи группы


Группы текущего пользователя

```bash
groups $USER
```

Идентификаторы пользователя

```bash
id $USER
```

Создание пользователя

```bash
sudo adduser anotheruser
```

```bash
getent passwd anotheruser
```

```bash
id anotheruser
```

Вход под новым пользователем

```
su -l anotheruser
```

Выход

```bash
exit
```

Создание группы

```bash
sudo addgroup anothergroup
```

```bash
getent group anothergroup
```

Добавление пользователя в группу

```bash
sudo adduser ubuntu anothergroup
```

или

```bash
usermod -aG anothergroup ubuntu
```

```bash
groups ubuntu
```

Удаление группы

```bash
sudo delgroup anothergroup
```

Удаление пользователя

```bash
sudo deluser anotheruser
```

```
ls /home
```

Для удаления пользовательских данных можно использовать следующие параметры: 

`--remove-home` или `--remove-all-files`

```
sudo rm -R /home/anotheruser/
```

## Права доступа

Создание файла

```bash
echo "Hello World" >> welcome.md
ls -l
```

Создание пользователей

```
sudo adduser alex
sudo adduser kate
```


```bash
# Откройте новую терминальную вкладку для пользователя alex

su -l alex
cat /home/ubuntu/class/welcome.md
echo "alex" >> /home/ubuntu/class/welcome.md
# exit
```

#### Права на запись всем пользователям

```bash
chmod o+w welcome.md
```

```bash
chmod 646 welcome.md
```

- `1` - выполнение (`x`)
- `2` - запись (`w`)
- `4` - чтение (`r`)

- Например: `x` + `w` + `r` = `7`

Параметр `-R` для рекурсивного применения команды

```bash
# Вкладка пользователя alex

echo "alex" >> /home/ubuntu/class/welcome.md
cat /home/ubuntu/class/welcome.md
```

```bash
chmod o-w welcome.md
```

или

```bash
chmod 644 welcome.md
```

#### Создание общей группы пользователей

```bash
sudo addgroup student
sudo adduser ubuntu student
sudo adduser alex student
```

```bash
# Пользователь ubuntu
chmod g+w welcome.md
sudo chown ubuntu:student welcome.md
ls -l welcome.md
```


```bash
# Вкладка пользователя alex
echo "alex" >> /home/ubuntu/class/welcome.md
```


```bash
# Вкладка пользователя kate
echo "kate" >> /home/ubuntu/class/welcome.md
```


```bash
chgrp ubuntu welcome.md 
```

```bash
rm welcome.md
delgroup student
deluser alex
deluser kate
```

Исполняемые файлы

```bash
echo 'echo "User name: $USER"' > script.sh
chmod u+x script.sh
ls -l script.sh 
./script.sh
```

## Монтирование устройства хранения данных

Список подключенных блочных устройств

```
lsblk
```

Отображение списка устройств (для определения типа файловой системы внешнего USB носителя)

```
sudo lshw | less
```

Создаем каталог, к которому будет подключено содержания носителя

```
sudo mkdir /media/pendrive
```

Монтируем к каталогу

```
sudo mount -t ntfs -o umask=0022,gid=1000,uid=1000 /dev/sdb1 /media/pendrive
```

Чтобы отменить монтирования, используется следующая команда

```
sudo umount /media/pendrive
```


## Процессы

Отслеживание выполнения процессов и потребляемых ими ресурсов

```
top
```


Список процессов

```
ps aux
```

Дерево процессов

```
pstree -p
```

Дерево процессов для текущего пользователя

```
pstree -p $USER
```

Java процессы

```bash
jps
```


```
sudo ls /proc
```

```
sudo kill $PID
```

## Менеджер пакетов `apt`

Получение обновленной актуальной информации о пакетах в репозиториях

```
sudo apt update
```

Установка пакетов

```bash
sudo apt install $PACKAGE_NAME
```


```bash
sudo apt install ${FILE.deb}
```

Обновление пакетов (не надо сейчас запускать)

```
sudo apt upgrade
```

```
sudo apt install --only-upgrade PACKAGE_NAME
```

Список установленных пакетов

```
dpkg --list
```

Удаление пакета

```bash
sudo apt remove $PACKAGE_NAME 
```

Удаление пакета со всеми конфигурационными файлами

```bash
sudo apt purge $PACKAGE_NAME
```

Очистка кэша локального репозитория от ранее полученных файлов (`/var/cache/apt/archives/`)

```
ls /var/cache/apt/archives/
```

```
sudo du -h /var/cache/apt/archives/
```

```
sudo apt clean
```

```
sudo du -h /var/cache/apt/archives/
```

Удаление невостребованных системой пакетов

```
sudo apt autoremove
```


## Загрузка файлов посредством `wget`

```
wget -P ~/Downloads https://mail.ru 
```

```
cat ~/Downloads/index.html | less
```


## Архиватор

### `tar`

Создание архива

```bash
tar -cvJ -f ${ARCHIVE.tar.xz} $SOURCE
```

```bash
tar -cvJ -C $SOURCE_PATH -f ${ARCHIVE.tar.xz} $SOURCE
```

Сжатие данных

- `-j` - bzip2 (`tar.bz2`)
- `-J` - xz (`tar.xz`)
- `-z` - gzip (`tar.gz`)
- `-a` - автоматическое определение по расширению

Отображение содержания архива

```bash
tar -tf ${ARCHIVE.tar.xz}
```

Распаковка архива

```bash
tar -xv -f ${ARCHIVE.tar.xz} --directory $DESTINATION --strip-components 1
```

### `zip`/`unzip`

Создание архива

```bash
zip ${ARCHIVE.zip} -r $SOURCE
```

```bash
cd $SOURCE_PATH; zip $DESTINATION_PATH/${ARCHIVE.zip} -r $SOURCE
```

Отображение содержания архива

```bash
unzip -l ${ARCHIVE.zip} 
```

Распаковка архива

```bash
unzip ${PATH_TO_ZIP.zip} -d $DESTINATION
```


## Управление сетью

### Настройка сети посредством `netplan`

```bash
cat /etc/network/interfaces

# ifupdown has been replaced by netplan(5) on this system.  See
# /etc/netplan for current configuration.
# To re-enable ifupdown on this system, you can run:
#    sudo apt install ifupdown
```

Конфигурация сети

```bash
ls -l /etc/netplan/
cat /etc/netplan/01-netcfg.yaml 
```


Параметры сети, полученные от DHCP сервера:

```
netplan ip leases eth0
```

или


```bash
ls -l /var/lib/NetworkManager

cat /var/lib/NetworkManager/${DHCP_CLIENT.lease}
```

```bash
# TODO: change dns server
```

### Отображение состояния и параметров сети

#### `ip addr`

IP адреса по всем интерфейсам

```bash
ip addr
```

IP адрес на интерфейсе `eth0`

```bash
ip addr show eth0
```

#### `ip link`

Интерфейсы (MAC адреса)

```bash
ip link
```

Отключение/включение интерфейса `eth0`

```bash
sudo ip link set eth0 down
```

```bash
sudo ip link set eth0 up
```

#### `ip neighbour`


ARP таблица

```
ip neighbour
```

#### `ip route`

Таблица маршрутизации

```
ip route
```

### Беспроводные устройства

Поиск беспроводных устройств

```
sudo lshw -C network
```

```
lspci
```

```
lsusb
```

Беспроводные интерфейсы (Wi-Fi)

```
iw dev
```

```
iwconfig
```


Список беспроводных устройств

```
sudo rfkill list
```

Отключить устройство

```
sudo rfkill block 1
```

Включить устройство

```
sudo rfkill unblock 1
```

### Контроль трафика

#### `iptables`

Блокировка исходящих запросов на определенный адрес с портом 443 (`https`)

```bash
sudo iptables -t filter -A OUTPUT -p tcp -d yandex.ru --dport 443 -j REJECT
```
где
- `t` - тип таблицы (`filter`, `nat` и др.)
- `A` - добавление правила
- `p` - протокол (`tcp`, `udp`, `icmp` или `all`)
- `d` - адрес получателя
- `dport` - порт получателя
- `j` - цель правила (то, что необходимо сделать) 

Отображение добавленного правила

```bash
sudo iptables -L -n
```

где

- `L` - вывод списка правил (по умолчанию таблицы `filter`)
- `n` - использовать при выводе числовые значения для портов и адресов

или

```bash
sudo iptables -t filter -L -n
```

Почему несколько?

```bash
host yandex.ru
```

Блокировка всех исходящих запросов на определенный адрес

```bash
sudo iptables -t filter -A OUTPUT -d yandex.ru -j REJECT
```

Создание правила для перенаправления запроса

```bash
sudo iptables -t nat -A OUTPUT -d google.ru -j DNAT --to-destination 94.100.180.200
```

- `to-destination` - замена адресата (для `DNAT`)

С указанием протокола и портов

```bash
sudo iptables -t nat -A OUTPUT -p tcp --dport 443 -d google.ru -j DNAT --to-destination 94.100.180.200:443
```

Весь трафик

```bash
sudo iptables -t nat -A OUTPUT -d 217.69.139.200 -j DNAT --to-destination 74.125.205.94
```

Отображение правил `nat` таблицы

```bash
sudo iptables -t nat -L
```

Отображения правил `nat` таблицы для исходящего трафика

```bash
sudo iptables -t nat -L OUTPUT -n
```

Удаление правила

```bash
sudo iptables -t nat -D OUTPUT 1
```

Удаление всех записей таблицы

```bash
sudo iptables -t nat -F
```

Разрешить входящий трафик на порт 23

```bash
sudo iptables -A INPUT -p tcp --dport 23 -j ACCEPT
```

Запретить входящий трафик на порт 23

```bash
sudo iptables -A INPUT -p tcp --dport 23 -j REJECT
```

Сохранить правила в файле

```bash
sudo iptables-save > iptables.rules
```

Загрузить правила из файла

```bash
sudo iptables-restore < iptables.rules
```


#### `ufw`


```bash
sudo ufw status|enable|disable
```

```bash
sudo ufw enable
```

```bash
sudo ufw allow 23
```

```bash
sudo ufw status
```

```bash
sudo ufw reject 23
```

```bash
sudo ufw disable
```


### Мониторинг соединений

#### `ss`

Отображение информации о сокетах

```
sudo ss -tuan
```
- `a` - все сокеты
- `l` - только сокеты в состоянии прослушивания (listening)
- `t` - tcp
- `u` - udp
- `n` - числовые значения хостов и портов
- `p` - показывать процессы
- `e` - подробная информация по сокетам

```
sudo ss -tuln
```

#### `lsof`

IP соединения

```
sudo lsof -i -P -n
```

```
sudo ss -tulnp
```

#### `nc`

```bash
nc -zv localhost 1-1000
```

```bash
nc -zv localhost 23
```

```bash
sudo nc -l -p 23
```

Если не работает, то разрешите входной трафик на порт 23 (ранее его запретили)

```bash
sudo iptables -R INPUT 1 -p tcp --dport 23 -j ACCEPT
```
где `R` - замена правила

```bash
nc localhost 23
```

### Удаленный доступ

#### `ssh`

Установите SSH сервер на `$REMOTE_HOST` (если не установлен)

```bash
sudo apt install openssh-server
```

Конфигурационные файлы

- на сервере
```bash
sudo nano /etc/ssh/sshd_config
```
- на клиенте (общие)
```bash
sudo nano /etc/ssh/ssh_config
```
- на клиенте для конкретного пользователя (если нет, то можно создать)
```bash
nano ~/.ssh/config
```

Управление SSH сервисом

```bash
sudo systemctl status|enable|disable|start|stop|restart ssh
```

Подключение с паролем

```bash
ssh $REMOTE_USERNAME@$REMOTE_HOST -p 22
```

Подключение по ключу

```bash
ssh-keygen -t rsa -P '' -f $HOME/.ssh/id_rsa
```

```bash
ssh-copy-id -i $HOME/.ssh/id_rsa.pub $REMOTE_USERNAME@$REMOTE_HOST
```

```bash
ssh -i ~/.ssh/id_rsa $REMOTE_USERNAME@$REMOTE_HOST
```

```bash
# TODO: add the key path to a config file
```

#### `scp`

Копирование файла на удаленный узел

```bash
scp -P 22 -i $HOME/.ssh/id_rsa $FILE $REMOTE_USERNAME@$REMOTE_HOST:$REMOTE_DIR
```

Копирование файла на локальный узел

```bash
scp -i $HOME/.ssh/id_rsa $REMOTE_USERNAME@$REMOTE_HOST:$FILE $LOCAL_DIR
```

Копирование каталога на удаленный узел 

```bash
scp -r -i $HOME/.ssh/id_rsa $LOCAL_DIR $REMOTE_USERNAME@$REMOTE_HOST:$REMOTE_DIR
```

#### Проброс портов

```bash
ssh -Nf -L $LOCAL_PORT:$REMOTE_HOST:$REMOTE_PORT -i $HOME/.ssh/id_rsa $REMOTE_USERNAME@$REMOTE_HOST
```

```
ssh -Nf -L 8080:127.0.0.1:8080 -i $HOME/.ssh/id_rsa alex@127.0.0.1
```

```bash
ps aux | grep ssh
kill $PID
```

#### Динамический проброс портов

```
ssh -D $LOCAL_PORT $REMOTE_USERNAME@$REMOTE_HOST
```

```
ssh -Nf -D 9090 alex@127.0.0.1
```

#### Запуск графический приложений по `ssh`

Открываем окно файлового менеджера сервера (удаленный узел) на стороне клиента (локальный узел)

```bash
ssh -X -n -i $HOME/.ssh/id_rsa $REMOTE_USERNAME@$REMOTE_HOST nautilus --new-window
```

```bash
ssh -X -n -i $HOME/.ssh/id_rsa alex@127.0.0.1 nautilus --new-window
```
