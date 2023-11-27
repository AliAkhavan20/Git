# Install gitlab runner into a vm
## update repo first
```
sudo apt update
```
## Install curl and wget packages
```
sudo apt install -y curl wget
```
## curl the address of runner script
```
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
```
## install Gitlab Runner
```
sudo apt install gitlab-runner
```
## Register runner into the gitlab server
```
gitlab-runner register  --url http://192.168.74.130  --token glrt-t1kaz-YiCtyUJLzPCsis
```
## choosing executer
### Choosing executer is important try to read about executer before using it.
```
choose executor: kubernetes
```
