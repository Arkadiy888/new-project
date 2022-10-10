Опишите решение для веб-приложения в kubernetes в виде yaml-манифеста. Оставляйте в коде комментарии по принятым решениям. Есть следующие вводные:

- у нас мультизональный кластер (три зоны), в котором пять нод
  - PodAntiAffinity распределяет по 1 поду на 1 ноду
- приложение требует около 5-10 секунд для инициализации
  - Readiness пробы обеспечивают отсутствие траффика в контейнер, пока он не готов для этого
  - liveness обеспечивает перезапуск контейнера, если он сломается
- по результатам нагрузочного теста известно, что 4 пода справляются с пиковой нагрузкой
  - HorizontalPodAutoscaler (2-4 пода)
  - 2 пода, так как если будет 1, и у него будет сбой, то будет downtime 5-10 секунд
  - 4 пода, так как 4 пода справляются с пиковой нагрузкой
- на первые запросы приложению требуется значительно больше ресурсов CPU, в дальнейшем потребление ровное в районе 0.1 CPU. По памяти всегда “ровно” в районе 128M memory
  - Resources (requests, limits)
  - requests настроены на cpu и memory
  - limits настроены только на memory, на cpu не настроены, так как на первые запросы приложению требуется значительно больше ресурсов cpu
- приложение имеет дневной цикл по нагрузке – ночью запросов на порядки меньше, пик – днём
  - С помощью средств kubernetes не нашел, как это можно реализовать, решил сделать через crontab с управляющего сервера
  - hpa-max-2.yaml применяем вечером (на ночь оставляем максимум 2 пода)
  - hpa-max-4.yaml применяем днем (возвращаем 4 пода для пиковой нагрузки днем)
```
0 8 * * * echo $(date) >> /home/arkadiy/test_task/hpa.log && /usr/local/bin/kubectl apply -f /home/arkadiy/test_task/hpa-max-4.yaml >> /home/arkadiy/test_task/hpa.log
0 20 * * * echo $(date) >> /home/arkadiy/test_task/hpa.log && /usr/local/bin/kubectl apply -f /home/arkadiy/test_task/hpa-max-2.yaml >> /home/arkadiy/test_task/hpa.log
```
- хотим максимально отказоустойчивый deployment
  - PriorityClass обеспечивает инициализацию пода на ноде, у которой не хватает ресурсов, за счет "вытеснения" менее приоритетных подов
  - PodAntiAffinity
- хотим минимального потребления ресурсов от этого deployment’а
  - HorizontalPodAutoscaler
  - Crontab









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
