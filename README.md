# Local Redis deployment on Minikube using helm
### Pre-requisites:
1. Windows 10+ OS
2. Chocolatey package manager installed - https://chocolatey.org/install
```
$ Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1')) 
```
3. Docker Engine installed - https://docs.docker.com/desktop/windows/install/

### Install minikube:
```
$ choco install minikube --force
```
