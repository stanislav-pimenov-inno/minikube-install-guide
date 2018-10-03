# minikube-install-guide

0. Install chocolatey package manager
https://chocolatey.org/install

1. Install minikube
`choco install minikube`

2. Check kube ctl install with `kubectl version`. If not execute: `choco install kubernetes-cli`
Execeutables included to the class can be found under chocolatey directory: `C:\ProgramData\chocolatey\bin`

3. Run minikube on VirtualBox

3.1 Install VirtualBox
https://www.virtualbox.org/wiki/Downloads

3.2 Run Minikube on VirtualBox: 
`minikube start --vm-driver="virtualbox" --alsologtostderr`

3.3 Stop Minukube:
`minikube stop`
Increase RAM memory for minikube up to 4Gb using VirtualBox Settings


4. Install Help 
`choco install kubernetes-helm`
