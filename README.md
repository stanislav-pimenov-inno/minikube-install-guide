# minikube-install-guide

## Setup Developer's environemt

0. Install chocolatey package manager
https://chocolatey.org/install

1. Install minikube
`choco install minikube`

2. Check kubectl installation with `kubectl version`.

If doesn't work execute: `choco install kubernetes-cli`

Execeutables included to the classpath can be found under chocolatey directory: `C:\ProgramData\chocolatey\bin`

3. Run minikube on VirtualBox

- Install VirtualBox
https://www.virtualbox.org/wiki/Downloads

- Run Minikube on VirtualBox: 
`minikube start --vm-driver="virtualbox" --alsologtostderr`

- Make sure minikube running using `minikube status`
Sample output:
```
minikube: Running
cluster: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.101
```
`minikube dashboard` - should open dashboard in your default browser

`kubectl get pods -n kube-system` - should list the pods from namespace `kube-system`

*IMPORTANT* make sure `C:\Users\<user>\.kube\config` was created  during minikube installation. 

- Stop Minukube:
`minikube stop`

- Increase RAM memory for minikube up to 8Gb using VirtualBox Settings or during start: `minikube start --memory=8192 --cpus=4 ...`

Minimal recommemde RAM: 4Gb

4. Install istio
Follow guide to install istio: https://istio.io/docs/setup/kubernetes/quick-start/#download-and-prepare-for-the-installation

Option 1 (without TLS) is acceptable

5. Install Helm 
`choco install kubernetes-helm`


6. Install Visual Studio Code

Install from here https://code.visualstudio.com/

Open the VS Code, add `Kubernetes` extension

## Deploying an application

### Reusing Docker from minikube
Open Gitbash and execute `eval $(minikube docker-env)`

### Build
Build you application using: `docker build -t <your image tag> <path to Dockerfile>`


### Deploy the app

`kubectl create -f <your app descriptor>.yaml`

## Context switch

`kubectl config use-context minikube`

or for AWS configuration

`kubectl config use-context aws`
