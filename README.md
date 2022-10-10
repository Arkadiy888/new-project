# Инфраструктура для тестирования docker-контейнера

## Описание:

Данный модуль состоит из terraform-скрипта и ansible-ролей и предназначен для создания ифраструктуры для тестирования docker-контейнеров и мониторинга тестовых серверов. В качестве облачного провайдера используется Yandex Cloud, в качестве источника docker-образов - dockerhub.

## Чтобы развернуть тестовое окружение необходимо:

1. Установить [terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

2. Установить [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

3. Задать параметры в файлах:
- terraform_server/variables.tf
- ansible_app/group_vars/app_servers.yaml
- ansible_app/roles/deploy_app/vars/main.yml
- ansible_monitoring/group_vars/app_servers.yaml
- ansible_monitoring/roles/install_grafana/vars/main.yml

4. Создать файлы:

- terraform_server/token.tf
  - Переменные для инициализации провайдера "Yandex Cloud"
- ansible_monitoring/roles/install_prometheus/files/prometheus
- ansible_monitoring/roles/install_prometheus/files/promtool
  - Исполняемые файлы для prometheus

5. Запустить terraform-скрипт командой:
```
terraform apply
```








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
