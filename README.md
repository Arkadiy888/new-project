# Инфраструктура для шаблонного приложения

## Описание:

Данный модуль состоит из terraform-скрипта и ansible-ролей и предназначен для создания ифраструктуры, билда и деплоя шаблонного приложения, мониторинга серверов и анализа логов приложения.
В качестве облачного провайдера используется Yandex Cloud, в качестве источника docker-образов - Container Registry Gitlab-CI.

## Чтобы развернуть окружение необходимо:

1. Установить [terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

2. Установить [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

3. Задать параметры в файлах:

- terraform_server/variables.tf

- ansible_playbook/group_vars/elasticsearch_servers.yaml
- ansible_playbook/group_vars/monitoring_servers.yaml
- ansible_playbook/group_vars/prod_servers.yaml
- ansible_playbook/group_vars/runner_servers.yaml
- ansible_playbook/group_vars/test_servers.yaml

- ansible_playbook/roles/install_elasticsearch/vars/main.yml
- ansible_playbook/roles/install_fluentd/vars/main.yml
- ansible_playbook/roles/install_grafana/vars/main.yml
- ansible_playbook/roles/install_node_exporter/vars/main.yml
- ansible_playbook/roles/install_prometheus/vars/main.yml
- ansible_playbook/roles/install_runner/vars/main.yml

4. Создать файлы:

- terraform_server/token.tf
  - Переменные для инициализации провайдера "Yandex Cloud"
- ansible_playbook/roles/install_runner/defaults/main.yml
  - Переменная "registration_token" для регистрации "gitlab-runner"
- ansible_playbook/roles/install_runner/files/id_rsa
  - Приватный ключ для подключения "gitlab-runner" к серверам

5. Запустить terraform-скрипт командой:
```
terraform apply
```
6. Зайти в web-интерфейс grafana, выбрать в качестве источника данных prometheus (http://localhost:9090) и импортировать prod_environment_dashboard.json и test_environment_dashboard.json

7. Зайти в web-интерфейс kibana и создать индекс "fluentd-*"

## Чтобы сделать деплой шаблонного приложения необходимо:

8. Сделать "commit" в репозиторий
  - Из ветки "main" деплой необходимо сделать в ручном режиме






# 1. Выбор системы мониторинга

Дата: 2022-09-24

## Статус

Принято

## Контекст

Необходимо выбрать систему мониторинга для будущей инфраструктуры

Так же необходимо определить архитектурное решение для системы мониторинга

## Решение

Принято решение использовать в качестве системы мониторинга "Prometheus + Grafana" на одном сервере

Prometheus и Grafana быстро и легко устанавливаются и настраиваются, к тому же приложение будет отдавать метрики в формате Prometheus

## Последствия

Быстрый запуск системы мониторинга

Экономия денег при установке на один сервер (используется Yandex Cloud в качестве облачного провайдера)


## Описание инфраструктуры:

- runner-1
  - Раннер для Gitlab-CI

- group-for-prod-env
  - Группа виртуальных машин для prod-env, состоит из двух инстансов "prod-1" и "prod-2"

- load-balancer-for-prod-env
  - Балансировщик нагрузки, распределяет запросы пользователей на инстансы "prod-1" и "prod-2"

- test-1
  - Тестовый сервер, предназначен для тестирования нового функционала

- monitoring-1
  - Сервер мониторинга

- elasticsearch-1
  - Система управления логами

![dashboard](./image.jpg)





