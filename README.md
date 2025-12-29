# Практика 3. Базовые команды Linux, Bash-скрипты.

# Вариант 2. Швыркова Яна

---

## Задание : Проверка доступности веб-сервиса через curl

Создайте скрипт, который:
- Проверяет доступность одного URL (например, `http://localhost:8080`)
- Использует `curl` для проверки HTTP-статуса
- Выводит: `URL - OK/FAIL` с текущей датой
- Если сервис недоступен, записывает ошибку в файл `errors-YYYY-MM-DD.log`

---

# 1. Создаем файл для скрипта

```bash
nano check_url.sh
```

---

# 2. Пишем в него скрипт

```bash
#!/bin/bash

URL_LOCAL="http://localhost:8080"
URL_EXTERNAL="https://eda.yandex.ru"

DATE=$(date +%Y-%m-%d)
ERROR_LOG="errors-${DATE}.log"
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')

check_and_log() {
    local url="$1"
    local http_code
    http_code=$(curl -o /dev/null -s -w "%{http_code}" --max-time 10 "$url")

    if [[ "$http_code" -ge 200 && "$http_code" -lt 400 ]]; then
        echo "$url - OK - $TIMESTAMP"
    else
        echo "$url - FAIL (HTTP $http_code) - $TIMESTAMP"
        echo "[$TIMESTAMP] $url > HTTP $http_code" >> "$ERROR_LOG"
    fi
}

# Проверяем оба адреса
check_and_log "$URL_LOCAL"
check_and_log "$URL_EXTERNAL"
```



<img width="1304" height="550" alt="image" src="https://github.com/user-attachments/assets/4e5a7881-d893-493c-8b7a-b4a2ea76c0a1" />


---

# 3. Делаем его исполняемым

```bash
chmod +x check_url.sh
```

---

# 4. Запускаем скрипт

```bash
./check_url.sh
```


<img width="586" height="67" alt="image" src="https://github.com/user-attachments/assets/2c10a754-fb3c-4312-94b6-470497dfddea" />

