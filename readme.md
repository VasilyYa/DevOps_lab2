# Лаб2. Ansible part 1 (Ansible + Nginx)

Написать плейбук для установки nginx на целевые хосты и загрузки на них индексного файла с текстом "Hello from Ansible".

# Лог выполнения задания:

1. Создали на управляемых машинах юзера (runner), от имени которого будет запускаться автоматизация ansible, добавили этого пользователя в группу sudo, выдали ему доступ к sudo без ввода пароля (либо потом понадобится прокидывать пароль при запуске ansible playbook)
> sudo useradd -m runner

> sudo usermod -a -G sudo runner

> sudo passwd runner (задаем пароль)

> sudo visudo (дописали runner ALL=(ALL) NOPASSWD:ALL)

2. Для доступа с управляющей машины (dev5), создаем на ней ssh-ключ и удаленно с помощью команды ssh-copy-id прокидываем его в разрешенные на управляемых машинах:
> ssh-keygen

> ssh-copy-id -p 9026 -i /home/dev5/.ssh/id_ed25519 runner@213.171.24.89

> ssh-copy-id -i /home/dev5/.ssh/id_ed25519 runner@82.202.140.103

3. Устанавливаем ansible на управляющей машине (dev5)
> sudo apt install ansible

проверяем установку:
> ansible-playbook --version

4. создаем конфиги ansible (playbook и inventory)
см. файлы inventory.ini и playbook.yml

5. Запускаем плейбук
> ansible-playbook ./playbook.yml -i ./inventory.ini --check (тестовый прогон --check, но для некоторых команд может быть неидемпотентным!)

> ansible-playbook ./playbook.yml -i ./inventory.ini (боевой прогон)

> ansible-playbook ./playbook.yml -i ./inventory.ini --diff (с ключем --diff покажет изменения)

> ansible-playbook ./playbook.yml -i ./inventory.ini -vv (с ключем -v, -vv, -vvv, ... - добавление вербозности вывода)

> ansible-playbook ./playbook.yml -i ./inventory.ini --ask-become-pass (с ключем --ask-become-pass спросит пароль заданного в playbook пользователя, если потребуется, например если у него не настроен sudo без запроса пароля)

6. Проверяем результат работы установки nginx и обеспечения содержимого файла index.html
> curl 213.171.24.89

> curl 82.202.140.103

