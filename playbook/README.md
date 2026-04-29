# Ansible Roles: ClickHouse, Vector, LightHouse

## Описание

Playbook разворачивает три сервиса с использованием ролей:
- **ClickHouse** — колоночная СУБД (внешняя роль AlexeySetevoi)
- **Vector** — агрегатор логов
- **LightHouse** — веб-интерфейс для ClickHouse

## Компоненты и их функции

**ClickHouse:**

устанавливается на хостах группы `clickhouse`;

создаётся БД logs (согласно `group_vars/clickhouse/vars.yml`);

используется роль `clickhouse` от Alexey V. Bobrov (поддерживает Ubuntu, Debian, EL).

**Vector:**

устанавливается на хостах группы `vector`;

настраивается через шаблоны (`vector.yaml.j2, vector.service.j2`);

собирает демо‑логи, преобразует их (добавляет имя хоста), выводит в консоль в формате (`JSON`);

запускается как `systemd‑сервис`.

**LightHouse:**

устанавливается на хостах группы `lighthouse`;

развёртывается через Git (ветка `master` по умолчанию);

конфигурируется `NGINX` для доступа к веб‑интерфейсу;

порт по умолчанию — `80` (можно переопределить).

## Переменные конфигурации

**ClickHouse** (`group_vars/clickhouse/vars.yml`):

- `clickhouse_version`: `23.8.9.54` - *версия может быть заменена на* `Latest`;

- `clickhouse_database`: `logs` - *назначает имя базы данных*.

**Vector** (`group_vars/vector/vars.yml`):

- `vector_version`: `0.55.0` - *версия может быть заменена на* `Latest`;

- `vector_install_dir`: `/opt/vector` - *задаёт путь к директории, куда будет распакован дистрибутив* `Vector`;

- `vector_config_dir`: `/etc/vector` - *определяет путь к директории с конфигурационными файлами* `Vector`;

- `vector_data_dir`: `/var/lib/vector` - *задаёт путь к директории для хранения временных данных Vector (кеши, буферы и т. д.)*.

**LightHouse** (`group_vars/lighthouse/vars.yml`):

- `lighthouse_nginx_port`: `80` - *устанавливает порт, на котором NGINX будет слушать запросы к веб‑интерфейсу LightHouse*;

- `lighthouse_nginx_server_name`: `_` - *задаёт значение директивы server_name в конфигурации NGINX для LightHouse*.

## Репозитории ролей

| Роль | Репозиторий |
|------|-------------|
| vector-role | https://github.com/OshchepkovDP/vector-role |
| lighthouse-role | https://github.com/OshchepkovDP/lighthouse-role |

## Быстрый старт

### Требования
- Ansible >= 2.10
- Python 3.x

### Установка ролей
```bash
ansible-galaxy install -r requirements.yml -p roles/
```

### Лицензия

Проект распространяется под лицензией `MIT`.

### Контакты
Автор ролей `vector-role` и `lighthouse-role`: **OshchepkovDP**.
Автор роли `clickhouse`: **Alexey V. Bobrov**.
