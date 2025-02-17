---
subtitle: Узнайте как сконфигурировать подключение к базе данных.
---
# Конфигурация базы данных

Конфигурация базы данных для вашего приложения находится в файле `config/database.php`. В этом файле вы можете определить все ваши подключения к базе данных, а также указать, какое подключение должно использоваться по умолчанию. В этом файле приведены примеры для всех поддерживаемых систем баз данных.

## Конфигурация SQLite 

База данных SQLite использует один файл в вашей файловой системе. Чтобы создать новую базу данных SQLite, используйте команду `touch`.

```bash
touch storage/database.sqlite
```

После этого вы можете настроить переменные окружения для использования базы данных, указав абсолютный путь в переменной `DB_DATABASE`.

```text
DB_CONNECTION=sqlite
DB_DATABASE=/absolute/path/to/database.sqlite
```

## Соединения на чтение/запись

Иногда вам может понадобиться использовать одно соединение с базой данных для операторов SELECT, а другое — для операторов INSERT, UPDATE и DELETE. Независимо от того, используете ли вы необработанные запросы, конструктор запросов или модель, это легко сделать.

Чтобы увидеть, как должны быть настроены соединения для чтения/записи, давайте посмотрим на этот пример:

```php
'mysql' => [
    'read' => [
        'host' => '192.168.1.1',
    ],
    'write' => [
        'host' => '196.168.1.2'
    ],
    'driver'    => 'mysql',
    'database'  => 'database',
    'username'  => 'root',
    'password'  => '',
    'charset'   => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix'    => '',
],
```

Обратите внимание, что в массив конфигурации добавлены два ключа: `read` и `write`. Оба этих ключа имеют значения массива, содержащие один ключ: `host`. Остальные параметры базы данных для соединений `read` и `write` будут объединены из основного массива `mysql`.

Указать параметры в массивах «чтение» и «запись» нужно только в том случае, если мы хотим переопределить значения в основном массиве. Таким образом, в этом случае `192.168.1.1` будет использоваться как соединение для чтения, а `192.168.1.2` будет использоваться как соединение для записи. Учетные данные базы данных, префикс, набор символов и все другие параметры в основном массиве mysql будут общими для обоих подключений.

<a id="oc-index-lengths-using-mysql-mariadb"></a>
## Длина индексов для MySQL / MariaDB

По умолчанию, October CMS использует кодировку `utf8mb4`. Если вы используете версию MySQL старше v5.7.7 или MariaDB старше v10.2.2, вам потребуется вручную настроить длину строки по умолчанию в миграциях, чтобы MySQL мог создавать для них индексы. Чтобы настроить длину строки по умолчанию, добавьте следующее в файл конфигурации **config/database.php** внутри массива `connections.mysql`.

```php
'varcharmax' => 191,
```
