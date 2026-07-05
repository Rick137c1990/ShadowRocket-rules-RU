📘 ShadowRocket Rules RU — Internal Modules Syntax Guide

# ⚙️ ShadowRocket Modules Syntax Specification

Этот документ описывает внутренний формат модулей ShadowRocket Rules RU.

Каждый модуль — это `.conf` файл, содержащий набор правил маршрутизации, групп доменов и логических блоков.

---

# 🧱 1. Общая структура модуля

Модуль состоит из последовательных блоков:

[METADATA]
[RULES]
[DOMAIN_GROUPS]
[FINAL_ACTION]

---

# 🧩 2. METADATA (мета-информация)

Используется для описания модуля.

```ini
# name: Russian Services
# version: 1.0
# description: Routing rules for RU services
# author: Rick

📌 Все строки начинаются с #

🌐 3. DOMAIN GROUPS (группы доменов)

Используются для логической группировки сервисов.

[DOMAIN_GROUP:VK]
vk.com
api.vk.com
cdn.vk.com
[DOMAIN_GROUP:YANDEX]
yandex.ru
api.yandex.ru
yastatic.net

📌 Особенности:

одна группа = одна логическая сущность
домены пишутся построчно
wildcard поддерживается:
*.vk.com
🚦 4. RULES (правила маршрутизации)

Основной блок логики.

Формат:

RULE,TYPE,VALUE,ACTION
📌 4.1 Типы правил
TYPE	Описание	Пример
DOMAIN	конкретный домен	vk.com
SUFFIX	домен и поддомены	.yandex.ru
KEYWORD	ключевое слово	google
GEOIP	геолокация IP	RU
📌 4.2 Примеры
RULE,DOMAIN,vk.com,DIRECT
RULE,DOMAIN,api.vk.com,DIRECT
RULE,SUFFIX,yandex.ru,DIRECT
RULE,GEOIP,RU,DIRECT
RULE,KEYWORD,google,PROXY
🎯 5. ACTION (действия)

Доступные действия:

ACTION	Значение
DIRECT	прямое соединение
PROXY	через прокси
REJECT	блокировка
🧠 6. Порядок выполнения

Правила обрабатываются сверху вниз:

1. DOMAIN (самый приоритетный)
2. SUFFIX
3. KEYWORD
4. GEOIP
5. fallback (FINAL_ACTION)
🧷 7. FINAL ACTION (поведение по умолчанию)

Если ни одно правило не подошло:

FINAL_ACTION,PROXY

или

FINAL_ACTION,DIRECT
🧬 8. Пример полного модуля
# name: Russian Services
# version: 1.0

[DOMAIN_GROUP:VK]
vk.com
api.vk.com

[DOMAIN_GROUP:YANDEX]
yandex.ru
api.yandex.ru

RULE,DOMAIN,vk.com,DIRECT
RULE,DOMAIN,api.vk.com,DIRECT
RULE,SUFFIX,yandex.ru,DIRECT
RULE,GEOIP,RU,DIRECT
RULE,KEYWORD,google,PROXY

FINAL_ACTION,PROXY
⚠️ 9. Рекомендации

✔ Не дублировать правила
✔ Сначала DOMAIN → потом GEOIP
✔ Минимизировать KEYWORD (он медленнее)
✔ Группы использовать для читаемости

🚀 10. Расширения (future ideas)

Планируемые возможности:

include других модулей:

INCLUDE,00_core.conf

условные правила:

IF TIME > 18:00 THEN PROXY

версии схемы:

SCHEMA_VERSION,1
```
