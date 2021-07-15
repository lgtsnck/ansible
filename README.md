**Тестовое задание на позицию DevOps-инженер**

Чтобы запустить плейбуки потребуется Ansible версии 3.2.0, установленный через pip. Данные плейбуки запускаются на семействе Debian, т.к. используется пакетный менеджер apt. 

В файле inventory, с названием hosts впишите свои удаленные хосты, по умолчанию настроен лишь localhost в файле ansible.cfg.

Генерируем ssh ключ:

    ssh-keygen -t rsa

Скопируем ssh ключ на удалённый хост:

    ssh-copy-id -i ~/.ssh/id_rsa.pub test@127.0.0.1

Установка docker:

    ansible-playbook playbooks/install_docker.yml --extra-vars "ansible_user=test ansible_become_password=test"


Создаем пользователя devops с группами sudo, docker, www-data.
Запрещаем доступ с помощью пароля для всех пользователей и доступ по ssh для root, делаем sudo без запроса пароля:

    ansible-playbook playbooks/init_devops_user.yml --extra-vars "ansible_user=test ansible_become_password=test"


Включаем фаервол и оставляем открытыми только 22 и 80 порты:

    ansible-playbook playbooks/enable_UFW.yml

Разворачиваем nginx как Reverse Proxy и Grafana в docker-контейнерах:

    ansible-playbook playbooks/create_containers.yml

Переходим по адресу удалённого хоста(или localhost) по порту 80, должна открыться форма логина Grafana:

    http://127.0.0.1:80

На этом всё :-)