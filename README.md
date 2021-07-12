**Тестовое задание на позицию DevOps-инженер**

Чтобы запустить playbooks потребуется Ansible версии 3.2.0

Установка docker:

    ansible-playbook playbooks/install_docker.yml --extra-vars "ansible_user=write_user_host ansible_become_password=write_password_host"

Генерируем ssh ключ:

    ssh-keygen -t rsa

Создаем пользователя devops с группами sudo, docker, www-data.
Запрещаем по доступ с помощью пароля и доступ по ssh для root, делаем sudo без запроса пароля:

    ansible-playbook playbooks/init_devops_user.yml --extra-vars "ansible_user=write_user_host ansible_become_password=write_password_host"

После создания пользователя devops, к примеру, можно удалить старого пользователя.

Вписываем в файле конфигурации ansible.cfg путь к созданному ключу на удаленном хосте:

    private_key_file = /home/devops/.ssh/id_devops_rsa

Включаем фаервол и оставляем открытыми только 22 и 80 порты:

    ansible-playbook playbooks/enable_UFW.yml

Разворачиваем nginx как Reverse Proxy и Grafana:

    ansible-playbook playbooks/create_containers.yml