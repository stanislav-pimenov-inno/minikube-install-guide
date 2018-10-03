# minikube-install-guide


0. Install chocolatey package manager
https://chocolatey.org/install

1. Install minikube
`choco install minikube`

2. Check kubectl installation with `kubectl version`.

If not execute: `choco install kubernetes-cli`

Execeutables included to the class can be found under chocolatey directory: `C:\ProgramData\chocolatey\bin`

3. Run minikube on VirtualBox

- Install VirtualBox
https://www.virtualbox.org/wiki/Downloads

- Run Minikube on VirtualBox: 
`minikube start --vm-driver="virtualbox" --alsologtostderr`
Make sure minikube running using `minikube status`
Sample output:
```
minikube: Running
cluster: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.101
```

- Stop Minukube:
`minikube stop`

- Increase RAM memory for minikube up to 4Gb using VirtualBox Settings


4. Install Helm 
`choco install kubernetes-helm`
