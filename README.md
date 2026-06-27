# RKN Opt

Оптимизация и защита Linux VPS от блокировок РКН, ТСПУ и DPI.  
Заточено под **Remnawave + VLESS + XTLS-Reality**.

---

## Запуск

```bash
curl -fsSL https://raw.githubusercontent.com/NikitaAzmov/RKN-PROTECT/main/rknopt.sh -o rknopt.sh
chmod +x rknopt.sh
sudo ./rknopt.sh
```

Или одной командой:

```bash
curl -fsSL https://raw.githubusercontent.com/NikitaAzmov/RKN-PROTECT/main/rknopt.sh | sudo bash
```

После запуска откроется меню:

```
 1) Установить ВСЁ сразу (все модули + автозапуск)
 2) Выбрать отдельный модуль
 3) Статус всех модулей
 4) Откат изменений
```

В пункте **2** можно выбрать любой модуль отдельно: sysctl, nftables, DNS, fail2ban и т.д.

Запускайте **после** установки Remnawave / Docker — модуль fd-limits перезапускает Docker.

---

## Совместимость

Debian 12/13, Ubuntu 22.04/24.04

Лог: `/var/log/rknopt.log`
