# Лабораторная работа  
## Установка и настройка Snort 3 из исходников на Debian 12

**Цель работы:** получить практические навыки сборки, настройки и эксплуатации системы обнаружения вторжений Snort 3 на примере Debian 12.  
**Сервер:** `gate`  
**Учётная запись:** `debian` / пароль `debian`  
**Время выполнения:** 2–3 академических часа.

---

## 1. Исходные данные и требования

- Виртуальная машина с Debian 12.
- Имя хоста: `gate`.
- Пользователь `debian` с правами `sudo`.
- Доступ в интернет для загрузки исходных кодов.
- Сетевой интерфейс для прослушивания трафика (определяется самостоятельно, например `ens18`).
- Домашняя сеть (`HOME_NET`) — ваша подсеть, например `192.168.7.0/24`.

> Все команды выполняются от пользователя `debian`. Если `sudo` не настроен, переключитесь в root: `su -`.

---

## 2. Задание

1. Подготовить систему, установить зависимости.
2. Собрать и установить библиотеку LibDAQ.
3. Собрать и установить Snort 3 из исходников.
4. Настроить конфигурационный файл `snort.lua`.
5. Создать локальное правило для проверки работы IDS.
6. Создать systemd-сервис для автоматического запуска Snort.
7. Проверить работу: сгенерировать тестовый трафик и убедиться в срабатывании правила.
8. Оформить отчёт.

---

## 3. Порядок выполнения

### 3.1. Подготовка системы


Обновите систему и установите зависимости:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential autoconf automake libtool pkg-config cmake \
    flex bison g++ libdumbnet-dev libhwloc-dev libluajit-5.1-dev libssl-dev \
    libpcap-dev libpcre2-dev zlib1g-dev git curl wget ca-certificates \
    liblzma-dev uuid-dev cpputest libsqlite3-dev libcmocka-dev \
    libnetfilter-queue-dev libmnl-dev libunwind-dev ethtool
```

Определите сетевой интерфейс:

```bash
ip -br link
```

Запомните имя интерфейса (например, `ens18`). В дальнейшем заменяйте `<INTERFACE>` на него.

---

### 3.2. Сборка и установка LibDAQ

```bash
cd ~
git clone https://github.com/snort3/libdaq.git
cd libdaq
./bootstrap
./configure
make
sudo make install
sudo ldconfig
```

Пропишите путь к библиотекам:

```bash
echo "/usr/local/lib/daq_s3" | sudo tee /etc/ld.so.conf.d/daq_s3.conf
sudo ldconfig
```

Проверьте установку:

```bash
ls /usr/local/lib/daq_s3/daq/
```

---

### 3.3. Сборка и установка Snort 3

```bash
cd ~
git clone https://github.com/snort3/snort3.git
cd snort3
./configure_cmake.sh --prefix=/usr/local
cd build
make -j$(nproc)
sudo make install
sudo ldconfig
```

Проверьте:

```bash
snort -V
snort --daq-list
```

В списке DAQ должны присутствовать модули `pcap`, `afpacket` и другие.

---

### 3.4. Настройка `snort.lua`

Откройте конфигурацию:

```bash
sudo nano /usr/local/etc/snort/snort.lua
```

Найдите блок `ips = { ... }`. Установите переменные:

```lua
HOME_NET = '192.168.1.0/24',  -- замените на вашу подсеть
EXTERNAL_NET = '!$HOME_NET',
```

В том же блоке `ips` найдите параметр `rules` и добавьте подключение локальных правил:

```lua
rules = [[
    include $RULE_PATH/*.rules
    include /usr/local/etc/snort/rules/local.rules
]]
```

Создайте каталог для правил:

```bash
sudo mkdir -p /usr/local/etc/snort/rules
```

Проверьте конфигурацию:

```bash
sudo snort -c /usr/local/etc/snort/snort.lua -T
```

Должно появиться сообщение `Snort successfully validated the configuration!`.

---

### 3.5. Создание локального правила

Создайте файл `/usr/local/etc/snort/rules/local.rules`:

```bash
sudo nano /usr/local/etc/snort/rules/local.rules
```

Добавьте правило для обнаружения ICMP-пакетов:

```
alert icmp any any -> any any (msg:"ICMP test packet"; sid:1000001; rev:1;)
```

Проверьте конфигурацию ещё раз:

```bash
sudo snort -c /usr/local/etc/snort/snort.lua -T
```

---

### 3.6. Создание systemd-сервиса

Создайте системного пользователя:

```bash
sudo useradd -r -s /usr/sbin/nologin -M -c "Snort IDS" snort
```

Создайте каталог для логов:

```bash
sudo mkdir -p /var/log/snort
sudo chown snort:snort /var/log/snort
```

Создайте unit-файл:

```bash
sudo nano /etc/systemd/system/snort3.service
```

Вставьте следующее (замените `<INTERFACE>` на ваш интерфейс):

```ini
[Unit]
Description=Snort 3 Intrusion Detection Service
After=syslog.target network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/snort -c /usr/local/etc/snort/snort.lua \
    -i <INTERFACE> -l /var/log/snort -u snort -g snort -k none
ExecReload=/bin/kill -SIGHUP $MAINPID
User=snort
Group=snort
Restart=on-failure
RestartSec=5s
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_RAW CAP_IPC_LOCK
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_RAW CAP_IPC_LOCK

[Install]
WantedBy=multi-user.target
```

Отключите offload на интерфейсе:

```bash
sudo ethtool -K <INTERFACE> gro off lro off tso off gso off rx off tx off
```

> Если команда выдаёт ошибку для некоторых опций — это нормально, главное отключить те, что поддерживаются.

Запустите сервис:

```bash
sudo systemctl daemon-reload
sudo systemctl enable snort3.service
sudo systemctl start snort3.service
sudo systemctl status snort3.service
```

---

### 3.7. Тестирование

Убедитесь, что сервис активен:

```bash
sudo systemctl status snort3.service
```

Сгенерируйте трафик:

```bash
ping -c 4 8.8.8.8
```

Посмотрите лог предупреждений:

```bash
sudo tail -f /var/log/snort/alert_fast.txt
```

Если всё настроено верно, вы увидите сообщение о срабатывании правила `ICMP test packet`.

---
