Composer - менеджер пакетов для php. Устанавливает пакеты, указанные в файле `composer.json` и создаёт файл `composer.lock`, который фиксирует версии установленных пакетов.

После запуска команды `composer install` скачивает все библиотеки, указанные в файле `composer.json` в директорию vendor (создаёт её), после чего формирует файл `autoload.php` Файл подключается к проекту (к корню проекта один раз) с помощью команды `require` или `require_once`: `require_once('../vendor/autoload.php')`

Установить новый пакет/зависимость можно с помощью команды `composer require vendor/package_name`, при этом файлы package.lock и package.json обновятся автоматически.