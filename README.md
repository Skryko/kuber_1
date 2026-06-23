Домашнее задание к занятию «Базовые объекты Kubernetes»

Цель работы

Развернуть Pod в Kubernetes и получить к нему доступ с локального компьютера через kubectl port-forward.

⸻

Задание 1. Создание Pod hello-world

Манифест Pod

Файл hello_world.yaml

apiVersion: v1
kind: Pod
metadata:
  name: hello-world
spec:
  containers:
    - name: web
      image: hashicorp/http-echo:1.0
      args:
        - "-text=hello world"
      ports:
        - containerPort: 5678

Создание Pod

kubectl apply -f hello_world.yaml

Проверка состояния Pod

kubectl get pods

Результат:

Подключение к Pod

Выполняем перенаправление порта:

kubectl port-forward pod/hello-world 8080:5678

В отдельном терминале проверяем доступность приложения:

curl http://127.0.0.1:8080

Полученный результат:

hello world

Скриншот результата:

⸻

Задание 2. Создание Service и подключение его к Pod

Создание Pod netology-web

Файл netology-web.yaml

apiVersion: v1
kind: Pod
metadata:
  name: netology-web
  labels:
    app: netology-web
spec:
  containers:
    - name: web
      image: hashicorp/http-echo:1.0
      args:
        - "-text=hello from netology service"
      ports:
        - containerPort: 5678

Создание Pod:

kubectl apply -f netology-web.yaml

⸻

Создание Service netology-svc

Файл netology-svc.yaml

apiVersion: v1
kind: Service
metadata:
  name: netology-svc
spec:
  selector:
    app: netology-web
  ports:
    - port: 80
      targetPort: 5678

Создание Service:

kubectl apply -f netology-svc.yaml

⸻

Проверка Service

Проверяем список сервисов:

kubectl get svc

Проверяем описание сервиса:

kubectl describe svc netology-svc

В результате сервис успешно обнаружил Pod:

Endpoints: 10.244.0.10:5678

Скриншот:

⸻

Подключение к Service

Выполняем перенаправление порта:

kubectl port-forward svc/netology-svc 8080:80

В отдельном терминале выполняем запрос:

curl http://127.0.0.1:8080

Получаем ответ:

hello from netology service

Скриншот результата:

⸻

Результат выполнения

В ходе выполнения работы были изучены базовые объекты Kubernetes:

* Pod;
* Labels;
* Selectors;
* Service;
* Port Forwarding.

Был успешно создан Pod hello-world и выполнено подключение к нему через kubectl port-forward.

Также был создан Pod netology-web, настроен Service netology-svc, после чего подтверждена корректная работа взаимодействия между Service и Pod.

Все объекты находятся в состоянии Running, а доступ к приложению успешно осуществляется через локальный порт.

⸻

Использованные команды

kubectl apply -f <manifest>.yaml
kubectl get pods
kubectl get svc
kubectl describe pod <pod_name>
kubectl describe svc <service_name>
kubectl logs <pod_name>
kubectl port-forward pod/<pod_name> <local_port>:<pod_port>
kubectl port-forward svc/<service_name> <local_port>:<service_port>
curl http://127.0.0.1:<port>
