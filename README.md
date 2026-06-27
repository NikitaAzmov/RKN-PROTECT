# RKN Protect

Защита Linux VPS от блокировок РКН, ТСПУ и DPI. Скрипт заточен под стек **Remnawave + VLESS + XTLS-Reality**, но подходит и для других VPN/Xray-нод.

> Форк [win64exe/rkn_protect](https://github.com/win64exe/rkn_protect) — **без смены SSH-порта** и **без открытия/закрытия портов в UFW**. Firewall хостера и ваши правила остаются нетронутыми.

---

## Быстрый старт

Замените `YOUR_REPO_URL` на ссылку на ваш репозиторий (добавите позже):

```bash
curl -fsSL YOUR_REPO_URL/rkn_protect.sh -o rkn_protect.sh
chmod +x rkn_protect.sh
sudo ./rkn_protect.sh all
```

Одной командой (когда будет готов raw-URL):

```bash
curl -fsSL YOUR_REPO_URL/rkn_protect.sh | sudo bash -s -- all
```

---

## Что делает скрипт

| Модуль | Команда | Зачем |
|--------|---------|-------|
| **sysctl** | `sysctl` | Защита от RST-инъекций, оптимизация TCP под BBR и Xray |
| **nftables** | `nftables` | TTL=128 (маскировка под Windows), блок ping/ICMP fingerprint |
| **fd-limits** | `fd-limits` | Лимит 1M файловых дескрипторов для Docker/Xray |
| **dns** | `dns` | DNS-over-TLS — защита от DNS-спуфинга провайдера |
| **fail2ban** | `fail2ban` | Блокировка IP при брутфорсе SSH |
| **ssh** | `ssh` | Hardening sshd по рекомендациям Lynis |
| **protocols** | `protocols` | Отключение dccp/sctp/rds/tipc |
| **autostart** | `autostart` | systemd-сервис для sysctl + nftables после reboot |

---

## Способы запуска

### Интерактивное меню

```bash
sudo ./rkn_protect.sh
```

### Все модули сразу

```bash
sudo ./rkn_protect.sh all
```

### Один модуль

```bash
sudo ./rkn_protect.sh sysctl
sudo ./rkn_protect.sh nftables
sudo ./rkn_protect.sh dns
sudo ./rkn_protect.sh fail2ban
# и т.д. — полный список: sudo ./rkn_protect.sh --help
```

### Статус и откат

```bash
sudo ./rkn_protect.sh status
sudo ./rkn_protect.sh rollback
sudo ./rkn_protect.sh fail2ban status
```

---

## Когда запускать

Рекомендуется запускать **после** установки Remnawave / Docker-стека, чтобы скрипт корректно поднял контейнеры после перезапуска Docker (модуль fd-limits).

---

## Совместимость

| ОС | Статус |
|----|--------|
| Debian 12 (bookworm) | ✓ |
| Debian 13 (trixie) | ✓ |
| Ubuntu 22.04 (jammy) | ✓ |
| Ubuntu 24.04 (noble) | ✓ |

---

## Проверка после установки

```bash
sudo ./rkn_protect.sh status

# или вручную:
nft list table inet rkn_protect
resolvectl status          # или: dig google.com @127.0.0.1
sysctl net.ipv4.tcp_rfc1337   # должно быть 1
fail2ban-client status sshd
```

Лог всех действий: `/var/log/rkn-protect.log`

---

## Отличия от оригинала

| | Оригинал | Этот форк |
|---|----------|-----------|
| Смена SSH-порта | ✓ | ✗ убрано |
| UFW open/close портов | ✓ | ✗ убрано |
| CLI-запуск модулей | только меню | меню + аргументы |
| README | подробный по каждому параметру | быстрый старт + таблица |

---

## Лицензия

MIT — как у [оригинального проекта](https://github.com/win64exe/rkn_protect).
