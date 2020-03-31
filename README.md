# lisovenko_microservices
lisovenko microservices repository

kubernetes-4

1. Создана новая ветка kubernetes-4;
2. Установка пакетного менеджера helm;
3. Установка аддона tiller для обшения с API Kubernetes. Tiller является серверной частью helm-а;
Создаем tiller.yml и запускаем tiller - сервер.
helm init --service-account tiller
Проверяем kubectl get pods -n kube-system --selector app=helm
12:35 $ kubectl get pods -n kube-system --selector app=helm
NAME                             READY   STATUS        RESTARTS   AGE
tiller-deploy-6dbd4749b6-dk5hz   1/1     Running       1          6m19s
tiller-deploy-6dbd4749b6-svkqt   0/1     Terminating   0          26m

4. Создаем директорию Chart;
5. Разработка Chart’а для компонента ui приложения в файле Chart.yaml;
Перенос манифестов ui в ui/templates;
6. Шаблонизируем service.yaml, deployment.yaml, ingress.yaml и определим значения в values.yaml;
7. Установим несколько релизов ui и параметризируем значения;
8. Собираем пакеты для остальных компонентов;
9. Добавлен новый пул узлов для увеличения производительности;
10. Добавлен репозиторий Gitlab;
11. Запускаем gitlab и создаем новую группу с проектами ui, comment, post, reddit-deploy;
12. Переносим исходные коды в Gitlab_ci/ui , /comment, /post и пушим в gitlab;
13. Переносим содержимое директории Charts в Gitlab_ci/reddit-deploy и пушим в gitlab;
14. Создаем .gitlab-ci.yml и пушим из директорий ui/, comment/, post/;
15. Поправлены пайплайны сервисов comment и post.



kubernetes-3

1. Создана новая ветка kubernetes-3;
2. Рассмотрен плагин kube-dns, предоставляющий DNS сервис;
3. Рассмотрен Ingress Controller;
4. Настроен балансировщик нагрузки;
5. Внедрение для приложения TLS;
6. Настроен прием только HTTPS траффика;
7. Рассмотрены network policy;
8. Рассмотрен механизмы PersistentVolume и PersistentVolumeClaim.


kubernetes-2

1. Создана ветка kubernetes-2;
2. Установка и запуск Minikube;
minikube start
3. Проверка, что minikube-кластер развернут;
kubectl get nodes
4. Рассмотрена конфигурация kubectl, информацию можно посмотреть по ~/.kube/config;
kubectl конфигурируется для подключения к разным кластерам, под разными пользователями.
Текущий контекст: kubectl config current-context
Список всех контекстов: kubectl config get-contexts
5. Описание состояния приложения в yaml-манифестах в директории kubernetes/reddit;
Основные объекты - ресурсы Deployment
6. Рассмотрены возможности пробрасывания сетевых портов подов на локальную машину;
kubectl port-forward <pod-name> local_port:pod_port
7. Рассмотрен объект Service для связи компонентов между собой;
8. Запуск и проверка kubectl apply -n dev -f reddit/;
9. Рассмотрен тип сервиса NodePort для доступа к ui-сервису снаружи;
10. Проверка через minikube service ui;
11. Рассмотрен аддон dashboard и его возможности;
Включить аддон: minikube addons enable dashboard
Запуск: minikube dashboard
12. Рассмотрены namespace;
13. Разделение среды для разработки приложения от всего остального кластера;
14. Разворачиваем кластер Kubernetes в GCP;
15. Запускаем приложение в GCP.


kubernetes-1

1. Создана новая ветка kubernetes-1;
2. Пройден курс kubernetes-the-hard-way (3 контроллера, 1 воркер);
3. Поды запустились;
kubectl apply -f ui-deployment.yml
kubectl apply -f post-deployment.yml
kubectl apply -f mongo-deployment.yml
kubectl apply -f comment-deployment.yml


Logging-1

1. Создана новая ветка logging-1;
2. Обновлен код приложения и созданы новые образы;
3. Создан новый docker host в GCP;
4. Создан отдельный compose-файл docker-compose-logging.yml;
5. Создаем образ Fluentd;
6. Запускаем и смотрим логи сервиса post;
7. Определим драйвер для логирования для сервиса post внутри compose-файла;
8. Поднятие инфрастуктуры централизованной системы логгирования;
9. Настройка инструмента для визуализации и анализа логов - Kibana:
 - Создание нового index pattern;
 - Анализ графика, лог-сообщения, работа с поиском по полям;
 - Создание фильтра для парсинга json логов для выделения в поля некоторых событий;
 - Пересобираем образ и перезапускаем сервис fluentd;
 - Проверяем поиск;
 - Определяем драйвер логгирования для ui сервиса в compose-файле;
 - Рассмотрены регулярные выражеия и grok-щаблоны для парсинга;
10. Добавлен сервис логгирования Zipkin и рассмотрены его возможности.


Monitoring-2

1. Создана новая ветка monitoring-2;
2. Подготовка окружения, создание docker-host;
3. Выделение мониторинга из docker-compose.yml в docker-compose-monitoring.yml;
4. Запуск сервисов;
5. Знакомство с cAdvisor ui;
6. Внесение инструмента Grafana в docker-compose-monitoring.yml и выбор dashboard;
7. Создание dashboards для Grafana;
8. Настроил уведомление в slack от alertmanager.
9. Запушил собранные вами образы на DockerHub

https://hub.docker.com/u/lisovenko


Monitoring-1

1. Создана новая ветка monitoring-1;
2. Создаем новые правила фаерволла для Prometheus и Puma;
3. Создание docker хоста;
4. Запуск готового образа Prometheus с DockerHub;
5. Рассмотрены стандартные метрики;
6. Рассмотрены targets;
7. Собираем на основе готового образа из Dockerhub свой docker образ:
 - Создаем Dockerfile, который будет копировать файл конфигурации с нашей машины внутрь контейнера;
 - Определяем собираемые метрики в prometheus.yml;
 - Собираем docker образ Prometheus;
 - Собриаем docker образы при помощи скриптов docker_build.sh в каждой директории;
 - Определяем в docker-compose.yml новый сервис - Prometheus;
 - Удаляем директивы build в виду использования скрипта docker_build.sh для сборки образов docker;
 - Добавлена секция networks для сервиса Prometheus в docker-compose.yml для общения по сети с другими сервисами;
 - Поднятие сервисов, приложение и Prometheus - запустились.
8. Настройка отслеживания состояния микросервисов;
9. Использование Exporters.

https://hub.docker.com/u/lisovenko



Gitlab-ci-1

1. Создана новая ветка gitlab-ci-1;
2. Создан новый инстанс gitlab-ci;
3. Запуск gitlab через omnibus-установку и отключаем регистрацию новых пользователей;
4. Создание новой группы и проекта;
5. Добавляем remote;
6. Добавляем .gitlab-ci.yml;
7. Регистрируем runner;
8. После регистрации runner запустился;
9. Добавили исходный код reddir в репозиторий и изменили описание пайплайна в .gitlab-ci.yml;
10. Добавили simpletest.rb с тестом;
11. Добавлена библиотека для тестирования в Gemfile;
12. Изменяем пайплайн, чтобы deploy job стал определением окружения dev;
13. Определяем два новых этапа : stage и production, далаем их manual;
14. Добавление директивы only с регулярным выражением, описывающим версию тега;
15. Push с тегом и без;
16. Определение динамического окружения с помощью переменных;

Docker-4

1. Создана новая ветнка docker-4;
2. Подключаемся к ранее созданному docker-host;
3. Работа с сетью none, host, bridge;
4. Установка и работа с docker-compose;
5. Внедрение переменных в docker-compose.yml;
6. Базовое имя проекта зависит от директории, поменять можно с помощью COMPOSE_PROJECT_NAME.


Docker-3

1. Создана новая ветка docker-3;
2. На ранее созданном docker хосте установлено приложение из трех компонентов;
3. Работа с Dockerfile;
4. Оптимизация размера образа ui;
5. Создаем docker volume reddit_db и подключаем его к MongoDB.



Docker-2

1. Добавлен новый репозиторий;
2. Установлен docker;
3. Проведена работа с основными командами docker;
4. Установлен docker-machine;
5. Создан контейнер внутри инстанса;
6. Произведена загрузка образа в Docker Hub и обратно;
