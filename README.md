# Инфраструктура для тестирования docker-контейнера

## Описание:

Данный модуль состоит из terraform-скрипта и ansible-ролей и предназначен для создания ифраструктуры для тестирования docker-контейнеров. В качестве облачного провайдера используется Yandex Cloud, в качестве источника docker-образов - dockerhub.

## Чтобы развернуть тестовое окружение необходимо:

1. Установить [terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

2. Установить [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

3. Задать параметры в файлах:
- terraform_server/variables.tf
- ansible_playbook/roles/deploy_env/vars/main.yml
- ansible_playbook/roles/deploy_app/vars/main.yml

4. Запустить terraform-скрипт командой:

- terraform apply

5. Отредактировать ip-адреса тестовых серверов в файле:

- ansible_playbook/hosts.txt

6. Запустить playbook.yaml командой:

- ansible-playbook playbook.yaml




