# Инфраструктура для тестирования шаблонного приложения

## Описание:

Данный модуль состоит из terraform-скрипта и ansible-роли, CI с использованием gitlab-ci и предназначен для создания ифраструктуры и деплоя шаблонного приложения на тестовый сервер. В качестве облачного провайдера используется Yandex Cloud, в качестве источника docker-образов - Container Registry Gitlab-CI.

## Чтобы развернуть тестовое окружение необходимо:

1. Установить [terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

2. Установить [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

3. Задать параметры в файлах:

- terraform_server/variables.tf
- ansible_runner/group_vars/app_servers.yaml
- ansible_runner/roles/deploy_runner/vars/main.yml

4. Создать файлы:

- terraform_runner/token.tf (переменные для инициализации провайдера "Yandex Cloud")
- ansible_runner/roles/deploy_runner/defaults/main.yml (переменная "registration_token" для регистрации "gitlab-runner")
- ansible_runner/roles/deploy_runner/files/id_rsa (приватный ключ для подключения "gitlab-runner" к тестовому серверу)

5. Запустить terraform-скрипт командой:
```
terraform apply
```

## Чтобы сделать деплой шаблонного приложения необходимо:

6. Сделать "commit" в репозиторий из ветки "uat"




## Prerequisites

The dashboard has been tested with the following software versions:

* NGINX Prometheus Exporter >= 0.4.1
* Grafana >= v5.0.0
* Prometheus >= v2.0.0

A Prometheus data source needs to be [added](https://prometheus.io/docs/visualization/grafana/#using) before installing the dashboard.









## Graphs

The dashboard comes with 2 rows with the following graphs for NGINX metrics:

* Status
  * Up/Down graph per instance. It shows the `nginx_up` metric.
* Metrics
  * Processed connections (`nginx_connections_accepted` and `nginx_connections_handled` metrics). This graph shows an [irate](https://prometheus.io/docs/prometheus/latest/querying/functions/#irate) in a range of 5 minutes. Useful for seeing the variation of the processed connections in time. 
  * Active connections (`nginx_connections_active`, `nginx_connections_reading`, `nginx_connections_waiting` and `nginx_connections_writing`). Useful for checking what is happening right now.
  * Total Requests with an irate (5 minutes range too) of the total number of client requests (`nginx_http_requests_total`) over time.

