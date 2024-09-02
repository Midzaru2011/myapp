# Argocd
Репозиторий для:

    - Jenkinsfile для декларативного pipeline, который выполняет следующие шаги:

        - Сборка докер образа тестового приложения из Dockerfile;
        - Отправка докер образа в репозиторий с указанным тэгом в DockerHub;
        - Изменение переменной  tag в values.yml для helm чарта тестового приложения.

    - Файл app2.yaml для создания приложения для ArgoCD на основе Helm Charts, размещенных в каталоге k8s
