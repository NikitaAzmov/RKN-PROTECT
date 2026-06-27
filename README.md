# RKN Opt

Оптимизация и защита Linux VPS от блокировок РКН, ТСПУ и DPI. Заточено под **Remnawave + VLESS + XTLS-Reality**, подходит и для других Xray-нод.

**Репозиторий:** [github.com/NikitaAzmov/RKN-PROTECT](https://github.com/NikitaAzmov/RKN-PROTECT)

---

## Быстрый старт

```bash
curl -fsSL https://raw.githubusercontent.com/NikitaAzmov/RKN-PROTECT/main/rknopt.sh -o rknopt.sh
chmod +x rknopt.sh
sudo ./rknopt.sh all
```

Одной командой:

```bash
curl -fsSL https://raw.githubusercontent.com/NikitaAzmov/RKN-PROTECT/main/rknopt.sh | sudo bash -s -- all
```

---

## Модули

| Модуль | Команда | Зачем |
|--------|---------|-------|
| **sysctl** | `sysctl` | Защита от RST-инъекций, TCP под BBR и Xray |
| **nftables** | `nftables` | TTL=128, блок ping/ICMP fingerprint |
| **fd-limits** | `fd-limits` | 1M файловых дескрипторов для Docker/Xray |
| **dns** | `dns` | DNS-over-TLS против DNS-спуфинга |
| **fail2ban** | `fail2ban` | Блокировка IP при брутфорсе SSH |
| **ssh** | `ssh` | Hardening sshd (Lynis) |
| **protocols** | `protocols` | Отключение dccp/sctp/rds/tipc |
| **autostart** | `autostart` | systemd-сервис после reboot |

---

## Запуск

```bash
sudo ./rknopt.sh              # меню
sudo ./rknopt.sh all          # всё сразу
sudo ./rknopt.sh sysctl       # один модуль
sudo ./rknopt.sh status       # статус
sudo ./rknopt.sh rollback     # откат
sudo ./rknopt.sh --help       # справка
```

Запускайте **после** установки Remnawave / Docker — модуль `fd-limits` перезапускает Docker.

---

## Совместимость

| ОС | Статус |
|----|--------|
| Debian 12 / 13 | ✓ |
| Ubuntu 22.04 / 24.04 | ✓ |

---

## Проверка

```bash
sudo ./rknopt.sh status
nft list table inet rknopt
sysctl net.ipv4.tcp_rfc1337   # = 1
```

Лог: `/var/log/rknopt.log`

---

## Лицензия

MIT
