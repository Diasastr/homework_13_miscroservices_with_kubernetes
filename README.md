# Kubernetes Nginx та MySQL Проект

Цей README документ описує кроки для розгортання веб-додатку з використанням Kubernetes, включаючи Nginx як веб-сервер та MySQL як систему управління базами даних.

## Крок 1: Опис YAML Файлів (повні версії можна знайти в репо)

### `mysql-secrets.yml`

Цей файл визначає секрети (паролі) Kubernetes для зберігання облікових даних MySQL.
Для прикладу: 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
type: Opaque
data:
  MYSQL_ROOT_PASSWORD: [base64-encoded-password]
  MYSQL_DATABASE: [base64-encoded-database-name]
  MYSQL_USER: [base64-encoded-user]
  MYSQL_PASSWORD: [base64-encoded-user-password]
```
Для кодування паролів в base64 перед додаванням їх у файл mysql-secrets.yml. використала команду:

```bash
echo -n 'password' | base64
```

### `mysql-deployment.yml myapp-deployment.yml`
Ці файли описують розгортання MySQL чи NGINX на Kubernetes.

### `services.yml`
Визначає Kubernetes Services для Nginx та MySQL.

## Крок 2: Розгортання Компонентів

Використала команду 

```bash
kubectl apply -f <filename>
```
для кожного файлу YAML починаючи з секретів, застосунків і потім сервісів.

## Крок 3: Тестування

### `Port Forwarding для доступу до Nginx`
Використала команду 

```bash
kubectl port-forward --address 0.0.0.0 svc/my-nginx-service 8080:80
```
для доступу до Nginx через браузер [http://[ec2-instance-public-ip]:8080]

![Accessed nginx from browser](https://github.com/Diasastr/homework_13_miscroservices_with_kubernetes/assets/80010947/4cc73048-c5a3-4b2e-a644-5f00ce25e999)

### `Створення SQL Клієнта для тестування бази даних`
Використала команду для створення поду:

```bash
kubectl apply -f mysql-client-pod.yml
```
потім зайшла в под з командою

```bash
kubectl exec -it mysql-client -- /bin/bash
```
і під'єдналась до бази даних

```bash
mysql -h mysql-service -u $MYSQL_USER -p$MYSQL_PASSWORD
```
![Login to mysql-service](https://github.com/Diasastr/homework_13_miscroservices_with_kubernetes/assets/80010947/23d21ab2-5ae3-4bcd-8a0d-b1d2d4193d80)










