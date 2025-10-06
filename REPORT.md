# Лабораторная работа 1: Анализ логов в Bash

## Цель
Научиться анализировать системные логи Linux с помощью базовых команд bash.

## Решение

### A) Частотный анализ слов в syslog (TOP-5)

**Команда:**
```bash
cat /var/log/syslog | tr -cs '[:alnum:]' '\n' | tr '[:upper:]' '[:lower:]' | sort | uniq -c | sort -nr | head -n 5
```

**Результат:**
```
  54080 00
  49171 1
  38910 2025
  38755 alexfresh
  38699 loq
```

---

### B) Неудачные попытки входа (auth.log)

**Команда для поиска:**
```bash
grep -E "(Failed|Invalid)" /var/log/auth.log
```

**Результат:**
```
2025-09-25T18:45:59.727110+03:00 alexfresh-1-LOQ-15ARP9 dbus-daemon[1534]: [system] Failed to activate service 'org.freedesktop.UPower.PowerProfiles': timed out (service_start_timeout=25000ms)
```

**Команда для извлечения IP и подсчета TOP-10:**
```bash
grep -E "(Failed|Invalid)" /var/log/auth.log | awk '{print $NF}' | sed -E 's/(\d+\.\d+\.\d+\.)\d+/\1x/g' | sort | uniq -c | sort -nr | head -n 10
```

**Результат:**
```
      1 (service_start_timeout=25000ms)
```

> Примечание: В файле `/var/log/auth.log` не обнаружено IP-адресов, связанных с неудачными попытками входа.

---

### C) Установки пакетов (dpkg.log)

**Команда для поиска событий install:**
```bash
grep " install " /var/log/dpkg.log
```

**Результат:**
(Список всех установленных пакетов. Пример первых строк:)
```
2025-04-15 17:14:57 install base-passwd:amd64 <none> 3.6.6
2025-04-15 17:14:57 install base-files:amd64 <none> 13.6ubuntu2
2025-04-15 17:14:58 install libc6:amd64 <none> 2.41-6ubuntu1
...
```

**Команда для подсчета TOP-10 пакетов:**
```bash
grep " install " /var/log/dpkg.log | awk '{print $4}' | sort | uniq -c | sort -nr | head -n 10
```

**Результат:**
```
      1 zstd:amd64
      1 zlib1g:i386
      1 zlib1g:amd64
      1 zip:amd64
      1 zenity-common:all
      1 zenity:amd64
      1 yelp-xsl:all
      1 yelp:amd64
      1 yaru-theme-sound:all
      1 yaru-theme-icon:all
```

---

## Ключевые выводы

- Базовые команды (`grep`, `awk`, `sort`, `uniq`, `tr`) позволяют эффективно анализировать большие объемы текстовых данных.
- Конвейеры (`|`) — мощный инструмент для поэтапной обработки данных.
- Скрипты можно легко воспроизвести, что делает их удобными для автоматизации.

## Ответы на вопросы

**Как проверяли?**
Выполняли команды пошагово, сверялись с ожидаемым выводом, использовали `head`, `tail` для просмотра частей файлов.

**Как использовали AI?**
AI помогал сформулировать команды, объяснить их работу, предложить альтернативные варианты и исправить ошибки в синтаксисе.

---

## Воспроизводимость

Все команды можно запустить повторно в любой системе Linux с доступом к `/var/log/`.
