# Redis deployment on Minikube using helm - Local
The repository contains:
1. Readme.md instructions file that explains how to deploy Redis to Minikube using Helm
2. Security is enabled by default in the official redis helm chart
3. values.yaml file that sets an extraFlag of loglevel to debug
4. Recomended Redis helm chart - Official Redis helm on GitHub: https://github.com/helm/charts/tree/master/stable/redis
5. It covers features such as - Cluster settings, Sentinel, networkPolicy, securityContext, persistence placeholder for volume, ExtraFlags section for Master and Slave, metrics, volumePermissins, and more.

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

### Add Stable repo to helm
Stable:
```
$ helm repo add stable https://charts.helm.sh/stable
```
Update repo:
```
$ helm repo update
```

# Deploy Redis using Helm
```
$ helm install my-release stable/redis -f values.yaml
```
Verify Deployment:
```
$ kubectl get all
```
# Redis is successfully deployed with security in debug mode

### Get your password 
```
$ kubectl get secret --namespace default my-release-redis 
-o jsonpath="{.data.redis-password}"
```
1. You will get a base64 coded String
2. Decode the string using an online [decoder](https://www.base64decode.org/)
3. Save the REDIS-PASSWORD


### Install redis-cli
```
$ npm install -g redis-cli
```

### Connect using the Redis CLI:
Master:
```
$ rdcli -h my-release-redis-master -a <REDIS-PASSWORD>
```
Worker:
```
$ rdcli -h my-release-redis-slave -a $REDIS_PASSWORD
```

### Forward / set ports:
```
$ kubectl port-forward --namespace default svc/my-release-redis-master 6379:6379
```
### Use redis cli
```
$ rdcli -h 127.0.0.1 -p 6379 -a <REDIS-PASSWORD>
```
