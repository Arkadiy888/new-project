# Инфраструктура для тестирования шаблонного приложения

## Описание:

Данный модуль состоит из terraform-скрипта и ansible-роли, CI с использованием gitlab-ci и предназначен для создания ифраструктуры и билда и деплоя шаблонного приложения на тестовый сервер. В качестве облачного провайдера используется Yandex Cloud, в качестве источника docker-образов - Container Registry Gitlab-CI.

## Чтобы развернуть тестовое окружение необходимо:

1. Установить [terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

2. Установить [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

3. Задать параметры в файлах:

- terraform_server/variables.tf
- ansible_runner/group_vars/app_servers.yaml
- ansible_runner/roles/deploy_runner/vars/main.yml

4. Создать файлы:

- terraform_runner/token.tf
  - Переменные для инициализации провайдера "Yandex Cloud"
- ansible_runner/roles/deploy_runner/defaults/main.yml
  - Переменная "registration_token" для регистрации "gitlab-runner")
- ansible_runner/roles/deploy_runner/files/id_rsa
  - Приватный ключ для подключения "gitlab-runner" к тестовому серверу)

5. Запустить terraform-скрипт командой:
```
terraform apply
```
## Чтобы сделать билд шаблонного приложения необходимо:

6. Сделать "commit" в репозиторий

## Чтобы сделать деплой шаблонного приложения необходимо:

7. Сделать "commit" в репозиторий из ветки "uat"
