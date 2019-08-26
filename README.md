# minikube-install-guide

## Setup Developer's environmet

0. Install package manager

Windows: https://chocolatey.org/install

MacOS: `$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

1. Install minikube

Windows:
```
> choco install minikube
> choco install kubernetes-cli
```

Executables included to the classpath can be found under chocolatey directory: `C:\ProgramData\chocolatey\bin`

MacOS:
```
$ brew cask install minikube
$ brew install kubectl
```

2. Check kubectl installation with

Windows/MacOS `> kubectl version`.

3. Run minikube on VirtualBox

**IMPORTANT** Disable Hyper-V feature on your Windows PC:

from admin CMD: `> bcdedit /set hypervisorlaunchtype off`

- Install VirtualBox

Windows: https://www.virtualbox.org/wiki/Downloads ??? profit
MacOS: `brew cask install virtualbox`

- Run Minikube on VirtualBox: 
`> minikube start --memory=6144 --vm-driver="virtualbox" --alsologtostderr`

- Make sure minikube running using `> minikube status`

Sample output:
```
minikube: Running
cluster: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.101
```
`> minikube dashboard` - should open dashboard in your default browser

`> minikube ip` - to get minikube ip

`> kubectl get pods -n kube-system` - should list the pods from namespace `kube-system`

**IMPORTANT** make sure `C:\Users\<user>\.kube\config` was created  during minikube installation. 

- Stop Minukube:
`> minikube stop`

- *optional* Increase RAM memory for minikube up to 8Gb using VirtualBox Settings or during start:

`> minikube start --memory=8192 --cpus=4 ...`

Minimal recommended RAM: 4Gb

4. Install istio

Follow guide to install istio: 
https://istio.io/docs/setup/kubernetes/quick-start/#download-and-prepare-for-the-installation

**IMPORTANT:** 
- from Prerequisities section you need step 1
- Whithin installation steps: Option 1 (without TLS) is preferable 

Example (MacOS):
```
curl -L https://git.io/getLatestIstio | sh -
# add to you ~/.bash_profile: export PATH="$PATH:$HOME/istio-1.0.5/bin"
cd istio-1.0.5
kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml
kubectl apply -f install/kubernetes/istio-demo.yaml
# wait some time, then make sure istio services are up and running
kubectl get svc -n istio-system
```

Ensure that minikube cluster labelled not to use istio injection for default namespace:

`> kubectl label namespace default istio-injection=disabled --overwrite`

5. Install Helm 

Easy way:

Windows: `> choco install kubernetes-helm`

MacOS: `$ brew install kubernetes-helm`

 Manually: https://github.com/helm/helm/releases

 Init helm: `> helm init`
 
6. Install Visual Studio Code

Install from here https://code.visualstudio.com/

Open the VS Code, add `Kubernetes` extension

## Deploying an application

### Reusing Docker from minikube

- Gitbash/MacOs: `$ eval $(minikube docker-env)`

- Windows Command Prompt: 

`> minikube docker-env`

then follow the instruction provided in the command output:

```
> minikube docker-env
SET DOCKER_TLS_VERIFY=1
SET DOCKER_HOST=tcp://192.168.99.100:2376
SET DOCKER_CERT_PATH=C:\Users\stanislav_pimenov\.minikube\certs
SET DOCKER_API_VERSION=1.35
REM Run this command to configure your shell:
REM @FOR /f "tokens=*" %i IN ('minikube docker-env') DO @%i
```

### Build
Build you application using: `docker build -t <image tag> <path to Dockerfile>`

*Example*: `> docker build -t api-stub:1.0 .`

Make sure image appeared in docker: `> docker images`

### Deploy the app from image

- Create deployment
`> kubectl run <deployment name> --image=<image tag> --port=8888`

- Expose as a service
`> kubectl expose deployment <deployment name> --type="NodePort"`

- Scale deployment
`$ kubectl autoscale deployment wiremock --min=2 --max=5`

### Deploy the app with app description

`> kubectl apply -f <your app descriptor>.yaml`

### Add to Istion Service Mesh: Manual sidecar injection

If the namespace is marked as `istio-injection=disabled` then manual sidecar injection is needed:

https://istio.io/docs/setup/kubernetes/sidecar-injection/#manual-sidecar-injection

## Context switch

`> kubectl config use-context minikube`

or for AWS configuration

`> kubectl config use-context aws`

## Installing Redis with Helm Chart

Windows:
```
helm install --name dev-redis ^
--set password=secretPassword ^
--set master.persistence.enabled=false ^
--set-string master.podAnnotations."sidecar\.istio\.io/inject=false" ^
--set-string slave.podAnnotations."sidecar\.istio\.io/inject=false" ^
stable/redis
```

For Linux use `\` for multiline joining

## Port forwarding

To access pods that are not exposed as NodePort or LoadBalancer or via Ambassador Gateway use port forwarding.

Example for prometheus and grafana hosts:
```
> kubectl port-forward prometheus-84bd4b9796-cx24r 9090:9090 -n istio-system
> kubectl port-forward grafana-6cbdcfb45-g87q8 3000:3000 -n istio-system
```

Then acess them http://localhost:9090 and http://localhost:3000 correspondingly

## Access external resources from Istio service mesh

To access external services from service mesh a ServiceEntry need to be added:

https://istio.io/docs/reference/config/istio.networking.v1alpha3/#ServiceEntry

https://istio.io/docs/tasks/traffic-management/egress/

Example:

https://istio.io/blog/2018/egress-tcp/#mesh-external-service-entry-for-an-external-mysql-instance

## Useful links

[Managing Resources in Kubernetes](https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/)

[Running your own Docker containers in Minikube for Windows](https://medium.com/@maumribeiro/running-your-own-docker-images-in-minikube-for-windows-ea7383d931f6)

[Service types: NodePort vs LoadBalancer](https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0) 
