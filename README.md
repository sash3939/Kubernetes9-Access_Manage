# Домашнее задание к занятию «Управление доступом»

### Цель задания

В тестовой среде Kubernetes нужно предоставить ограниченный доступ пользователю.

------

### Чеклист готовности к домашнему заданию

1. Установлено k8s-решение, например MicroK8S.
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключённым github-репозиторием.

------

### Инструменты / дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) RBAC.
2. [Пользователи и авторизация RBAC в Kubernetes](https://habr.com/ru/company/flant/blog/470503/).
3. [RBAC with Kubernetes in Minikube](https://medium.com/@HoussemDellai/rbac-with-kubernetes-in-minikube-4deed658ea7b).

------

### Задание 1. Создайте конфигурацию для подключения пользователя

1. Создайте и подпишите SSL-сертификат для подключения к кластеру.

<img width="815" alt="create certs" src="https://github.com/user-attachments/assets/40189ec9-a573-4b75-a8d5-bf7d35bd9124">

2. Настройте конфигурационный файл kubectl для подключения.
  
Переносим на машину с которой будем подключаться и настраиваем конфигурационный файл. Добавляем пользователя kuber (в моем случае), добавляем контекст и проверяем конфигурацию

<img width="604" alt="configuration" src="https://github.com/user-attachments/assets/bd53a64e-8c1e-4cd9-8d18-ba1ee0c00337">

3. Создайте роли и все необходимые настройки для пользователя.

<img width="454" alt="created role" src="https://github.com/user-attachments/assets/b2ea6379-cab6-404e-8134-fc34e802982b">

[role.yaml](https://github.com/sash3939/Kubernetes9-Access_Manage/blob/main/rbac/role.yaml)
[role_binding.yaml](https://github.com/sash3939/Kubernetes9-Access_Manage/blob/main/rbac/role_binding.yaml)

4. Предусмотрите права пользователя. Пользователь может просматривать логи подов и их конфигурацию (`kubectl logs pod <pod_id>`, `kubectl describe pod <pod_id>`).



5. Предоставьте манифесты и скриншоты и/или вывод необходимых команд.


------

### Правила приёма работы

1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

------
