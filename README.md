# Kubernetes + Helm

## Установка
#### virtualbox
```
sudo apt install virtualbox virtualbox-ext-pack
```

#### minicube
```
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube-linux-amd64
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
```

#### kubectl
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```
#### helm
```
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

#### Проверим
```
minikube version
kubectl version -o json
helm version
```

## Запуск

#### Старт
```
minikube start
```

#### Инфо
```
kubectl config view
kubectl get nodes
kubectl get pods --all-namespaces
```

#### Dashboard
```
minikube dashboard
```

## Работа с чартами

#### Создаем свой чарт
```
helm create example-chart
tree example-chart/
cat example-chart/templates/deployment.yaml
cat example-chart/values.yaml
```

#### Устанавливаем его и смотрим
```
helm install example-chart example-chart/
kubectl port-forward svc/example-chart 8888:80
kubectl get services
```

#### Меняем конфиги
```
...
```

#### Проверяем синтаксис чарта, тестируем и устанавливаем
```
helm lint example-chart/
helm install example-chart --dry-run --debug example-chart/
helm upgrade example-chart example-chart/ 
```

#### Изменяем configmap и values
```
# configmap.yaml
<h1>Hello, {{ .Values.username }}!!!</h1>
```

#### Обновляем
```
helm upgrade example-chart example-chart/ --set username=Serega
```

#### Смотрим историю и откатываемся
```
helm history example-chart
helm rollback example-chart 1
helm uninstall example-chart
```

#### Создаем репозиторий с пакетом
```
helm package example-chart/
mkdir helm-local-repo
mv example-chart-0.1.0.tgz helm-local-repo/
helm repo index helm-local-repo/
```

#### Устанавливаем из собственного репозитория
```
sudo docker run -ti -v $(pwd)/helm-local-repo/:/usr/share/nginx/html -p 88:80 nginx
curl localhost:88/index.yaml
helm repo add example-repo http://localhost:88
helm repo list
helm search repo example
helm install example-chart-from-repo example-repo/example-chart
```





