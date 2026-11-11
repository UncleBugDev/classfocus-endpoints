# 🎯 ClassFocus Live Endpoint Discovery (Dead-Drop Resolver)

[![Status](https://img.shields.io/badge/System-Active-success?style=flat-square&logo=github)](https://raw.githubusercontent.com/UncleBugDev/classfocus-endpoints/main/endpoint.json)
[![Protocol](https://img.shields.io/badge/Protocol-TLS%201.3%20%2F%20WSS-blue?style=flat-square)](https://github.com/UncleBugDev/classfocus-endpoints)
[![Update Frequency](https://img.shields.io/badge/Sync-Every%203%20min-informational?style=flat-square)](https://github.com/UncleBugDev/classfocus-endpoints)

Публичный репозиторий-манифест для автоматического обнаружения активного адреса сервера **ClassFocus Enterprise** (`klass-server`).

---

## 📡 Текущий активный манифест (`endpoint.json`)

Клиентские станции (`ClassFocus.Agent` и `ClassFocus.Service`) обращаются к этому репозиторию напрямую:
```
https://raw.githubusercontent.com/UncleBugDev/classfocus-endpoints/main/endpoint.json
```

```json
{
  "version": "1.0",
  "updated_at_utc": "2026-11-11T12:00:00Z",
  "primary_url": "https://178.120.45.67:8443",
  "domain_url": null,
  "lan_url": "https://192.168.100.10:8443",
  "tailscale_url": "https://klass-server.tail48c9dc.ts.net",
  "web_socket_url": "wss://178.120.45.67:8443/hub/"
}
```

---

## 🏛️ Архитектура автоматической синхронизации

```mermaid
flowchart TD
    A["Роутер Белтелеком (Huawei HG8245H)\n• Смена внешнего PPPoE IP"] -->|WAN IP Check| B["klass-server (Авто-демон)\n• Скрипт: /usr/local/bin/classfocus-github-sync.sh\n• Таймер: каждые 3 минуты"]
    B -->|GitHub REST API (PUT)| C["GitHub Raw (Этот репозиторий)\n• UncleBugDev/classfocus-endpoints/endpoint.json"]
    C -->|HTTPS GET (Cache-Busting)| D["Компьютеры классов (ClassFocus.Agent)\n• Мгновенное восстановление связи <5 сек"]
```

---

## 🛡️ Безопасность
- Репозиторий предназначен только для чтения публичных эндпоинтов класса.
- Манифест не содержит учетных данных, паролей или токенов доступа.
- Все соединения между агентами и сервером защищены сквозным шифрованием TLS 1.3 и JWT авторизацией.
