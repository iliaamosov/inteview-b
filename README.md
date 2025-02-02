Этот плейбук предназначен для автоматической настройки серверов с помощью Ansible.
Включает следующие задачи:
Настройка сети – переименование интерфейса и включение сетевого соединения
Обновление параметров CPU – отключение энергосбережения и перевод CPU в производительный режим
Шифрование диска – настройка LUKS-шифрования для второго диска
Сбор информации о процессоре – выполнение lscpu и сохранение данных на локальной машине


ansible-bhft
├── README.md
├── inventory
│   └── hosts.ini
├── playbook.yml
└── roles
    ├── cpu_info
    │   ├── README.MD
    │   ├── cpu_info.txt
    │   └── main.yml
    ├── cpu_update
    │   ├── README.md
    │   └── tasks
    │       └── main.yml
    ├── disk_encryption
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   └── tasks
    │       └── main.yml
    └── network_config
        ├── README.md
        └── tasks
            └── main.yml







Настройка сети (network_config)

Задачи:
Получает список сетевых интерфейсов
Переименовывает первый интерфейс в net0
Включает интерфейс net0
Выводит информацию об интерфейсе

Запуск командой:
ansible-playbook -i inventory/hosts.ini playbook.yml --tags network





Обновление параметров CPU (cpu_update)
Задачи:
Отключает C-state (энергосбережение CPU)
Переводит CPU в режим максимальной производительности
Проверяет текущие параметры


Запуск командой:
ansible-playbook -i inventory/hosts.ini playbook.yml --tags cpu







Шифрование второго диска (disk_encryption)
Задачи:
Проверяет, установлен ли cryptsetup, при необходимости устанавливает
Проверяет, зашифрован ли диск
Форматирует диск в LUKS (если не зашифрован)
Открывает диск и монтирует в /mnt/data

Запуск командой:
ansible-playbook -i inventory/hosts.ini playbook.yml --tags disk_encryption




Сбор информации о процессоре (cpu_info)
Задачи:
Выполняет команду lscpu на сервере
Сохраняет информацию о CPU в файл на сервере
Копирует этот файл на локальную машину

Запуск командой:
ansible-playbook -i inventory/hosts.ini playbook.yml --tags cpu_info





