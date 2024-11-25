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

Добавляем в роль verbs: где ["watch", "list", "get"]

<img width="798" alt="verbs" src="https://github.com/user-attachments/assets/7cf92595-f3a1-4603-8d9b-fa6ca23d8213">

Проверяем (сейчас на сервере есть такой под - $ kubectl get pod)

<img width="556" alt="get added to role" src="https://github.com/user-attachments/assets/8055d98f-4d20-4312-a71b-cd6b5935d72f">


Проверяем что изменилось с тестовой машины

<img width="554" alt="fragment" src="https://github.com/user-attachments/assets/5f7fd376-3246-4d1d-b303-9316e6d07909">

```bash
root@kubectl:~/.kube/rbac# kubectl describe pod myapp-pod-8569b59bf7-gs9nm
Name:             myapp-pod-8569b59bf7-gs9nm
Namespace:        default
Priority:         0
Service Account:  default
Node:             kuber/192.168.43.233
Start Time:       Sat, 23 Nov 2024 09:48:00 +0100
Labels:           app=myapp
                  pod-template-hash=8569b59bf7
Annotations:      cni.projectcalico.org/containerID: ad61b6b70f164d4655ea11bc705f753c293df59cc64fff4febbdbcd38706dc91
                  cni.projectcalico.org/podIP: 10.1.106.134/32
                  cni.projectcalico.org/podIPs: 10.1.106.134/32
Status:           Running
IP:               10.1.106.134
IPs:
  IP:           10.1.106.134
Controlled By:  ReplicaSet/myapp-pod-8569b59bf7
Init Containers:
  init-myservice:
    Container ID:  containerd://cfdd9c98ac1c660a696665049a0878d09cb546555e095b424abca7fb53456a05
    Image:         busybox:1.28
    Image ID:      docker.io/library/busybox@sha256:141c253bc4c3fd0a201d32dc1f493bcf3fff003b6df416dea4f41046e0f37d47
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      until nslookup myservice.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for myservice; sleep 2; done
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Mon, 25 Nov 2024 18:55:19 +0100
      Finished:     Mon, 25 Nov 2024 18:56:19 +0100
    Ready:          True
    Restart Count:  4
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-722mr (ro)
Containers:
  network-multitool:
    Container ID:   containerd://e90e47b240514a4aa2287983e227f9dd98cffb79621980077a2b1b91dfccedc2
    Image:          wbitt/network-multitool
    Image ID:       docker.io/wbitt/network-multitool@sha256:d1137e87af76ee15cd0b3d4c7e2fcd111ffbd510ccd0af076fc98dddfc50a735
    Ports:          80/TCP, 443/TCP
    Host Ports:     0/TCP, 0/TCP
    State:          Running
      Started:      Mon, 25 Nov 2024 18:56:19 +0100
    Last State:     Terminated
      Reason:       Unknown
      Exit Code:    255
      Started:      Mon, 25 Nov 2024 18:20:16 +0100
      Finished:     Mon, 25 Nov 2024 18:54:37 +0100
    Ready:          True
    Restart Count:  4
    Limits:
      cpu:     200m
      memory:  512Mi
    Requests:
      cpu:     100m
      memory:  256Mi
    Environment:
      HTTP_PORT:   80
      HTTPS_PORT:  443
    Mounts:
      /usr/share/nginx/html/ from nginx-index-file (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-722mr (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  nginx-index-file:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      index-html-configmap
    Optional:  false
  kube-api-access-722mr:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:                      <none>
root@kubectl:~/.kube/rbac# kubectl logs myapp-pod-8569b59bf7-gs9nm
Defaulted container "network-multitool" out of: network-multitool, init-myservice (init)
The directory /usr/share/nginx/html is a volume mount.
Therefore, will not over-write index.html
Only logging the container characteristics:
WBITT Network MultiTool (with NGINX) - myapp-pod-8569b59bf7-gs9nm -  - HTTP: 80 , HTTPS: 443 . (Formerly praqma/network-multitool)
Replacing default HTTP port (80) with the value specified by the user - (HTTP_PORT: 80).
Replacing default HTTPS port (443) with the value specified by the user - (HTTPS_PORT: 443).

```
Команды logs и describe от УЗ kuber работают

6. Предоставьте манифесты и скриншоты и/или вывод необходимых команд.

[Config](https://github.com/sash3939/Kubernetes9-Access_Manage/blob/main/rbac/config)

[Role.yaml](https://github.com/sash3939/Kubernetes9-Access_Manage/blob/main/rbac/role.yaml)

[Role_binding.yaml](https://github.com/sash3939/Kubernetes9-Access_Manage/blob/main/rbac/role_binding.yaml)

------

### Правила приёма работы

1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

------
