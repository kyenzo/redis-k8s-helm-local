# Local Redis deployment on Minikube using helm
### Pre-requisites:
1. Windows 10+ OS
2. Chocolatey package manager installed - https://chocolatey.org/install
```
$ Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = 
[System.Net.ServicePointManager]::SecurityProtocol -bor 3072; 
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1')) 
```
3. Docker Engine installed - https://docs.docker.com/desktop/windows/install/

### Install minikube:
Minikube
```
$ choco install minikube
```
In case you want to re-install Minikube, add the flag --force

### Install Helm:
```
$ choco install kubernetes-helm
```
Verify installation be check helm version:
```
$ helm version
```

### Start minikube
Docker Engine should be running first
```
minikube start
```

### Install kubectl:
```
$ choco install kubernetes-cli
```

Verify installation:
```
$ kubectl get all
```

### Add Bitnami and Stable repo to helm
bitnami:
```
$ helm repo add bitnami https://charts.bitnami.com/bitnami
```
Update repo:
```
$ repo update
```
Stable:
```
$ helm repo add stable https://charts.helm.sh/stable
```
Update repo:
```
$ repo update
```

### Deploy Redis using Helm
```
$ helm install my-release stable/redis
```
Verify Deployment:
```
$ kubectl get all
```

### Get your password 
```
$ kubectl get secret --namespace default my-release-redis 
-o jsonpath="{.data.redis-password}"
```
1. You will get a base64 coded String
2. Decode the string using an online [decoder](https://www.base64decode.org/)
3. Save the <REDIS-PASSWORD>


### Install redis-cli
```
$ npm install -g redis-cli
```

### Connect using the Redis CLI:
Master:
```
$ redis-cli -h my-release-redis-master -a <REDIS-PASSWORD>
```
Worker:
```
$ redis-cli -h my-release-redis-slave -a $REDIS_PASSWORD
```

### Forward / set ports:
```
$ kubectl port-forward --namespace default svc/my-release-redis-master 6379:6379
```
### Use redis cli
```
$ rdcli -h 127.0.0.1 -p 6379 -a UeBqYL6xbr
```

