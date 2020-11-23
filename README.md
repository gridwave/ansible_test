## Описание решения.

В этом репозитории ansible-код для настройки сервера.
Как указывалось в задании приложение успешно разворачивается на ОС Ubuntu (тестировалось на 20.04, но должно работать и на более ранних версиях).
В решении задания старался придерживаться best-practices, там где то возможно и не подвергать код излишнему усложнению.

#### Ранбук:
1. git clone https://github.com/gridwave/ansible_test.git
2. cd ansible-test
3. Вносим исправления в *inventory/test-app.yml*, а именно, меняем IP-адрес целевого сервера в переменной *ansible_ssh_host*, добавляем или меняем переменные критичные для корректного подключения к серверу и исполнения python-кода (*ansible_ssh_|user* и пр.).
4. В целях экономии времени, переменные отвечающие за настройку сервера находятся в файле главного плейбука. Это не является best-practice, и в будущем они должны быть перенесены в *group_vars* или *host_vars*. Исключение составляет переменная **postgresql_users_password** которая зашифрована и подключается из отдельного файла *secrets.yml*.
Переменные которые должны быть изменены:
**nginx_vhosts.server_name**,
**nginx_vhosts.filename**,
**nginx_vhosts.extra_parameters.ssl_certificate**,
**nginx_vhosts.extra_parameters.ssl_certificate_key**,
**certbot_admin_email**,
**certbot_certs.domains**,
**test_app_host**
5. ansible-galaxy role install -r requirements.yml
6. ansible-playbook -i inventory/test-app.yml test-app.yml --ask-vault-pass

Сейчас тестовый код развёрнут по адресу https://159-203-173-201.sslip.io/

#### Возможные улучшения в ansible-коде:
1. Использовать environments и разнести inventory и vars по разным каталогам.
```
   environments/
     test/
       inventory.yml
       group_vars/
     dev/
       inventory.yml
       group_vars/
     prod/
       inventory.yml
       group_vars/
```
2. Избавится от неидемпотентных задач (генерация SECRET_KEY)

#### Предложения по общему улучшению:
1. Перенести приложения в docker-контейнер
2. Не использовать ansible для деплоя приложения, настроить CI/CD-пайплайн в GitLab-CI или GitHub Actions.
3. Использовать оркестратор
