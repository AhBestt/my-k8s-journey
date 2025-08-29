# Install Tools
Set up kubernetes tools on you computer <br>

___
## kubectl
install kubectl on macOS <br>
install with Homebrew on macOS <br>

run the installation command: <br>
```bash
brew install kubectl
```
test to ensure the version you installed is up-to-date <br>
```bash
kubectl version --client
```

## Verify kubectl configuration
By default, kubectl configuration is located at `~/.kube/config` <br>

check that kubectl is properly configured by getting the cluster state: <br>
```bash
kubectl cluster-info
```
If you see a URL response, kubectl is correctly configured to access your cluster. <br>

___

## Start minikube
installation <br>
```bash
brew install minikube
```

start cluster <br>
```bash
minikube start
```
or If you want to start with specific version use command below <br>
```bash
 minikube start --kubernete-version vX.Y.Z 
```

interact with your cluster <br>
```bash
kubectl get pod -A
```

For additional insight into your cluster state, minikube bundles the Kubernetes Dashboard, allowing you to get easily acclimated to your new environment: <br>
```bash
minikube dashboard
```

___
## Deploy applications
### Service

Create a sample deployment and expose it on port 8080:
```bash
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl expose deployment hello-minikube --tpye=NodePort --port=8080
```

It may take a moment, but your deployment will soon show up when you run <br>
```bash
kubectl get service hello-minikube
```
The easiest way to access this service is to let minikube launch a web browser for you:
```bash
minikube service hello-minikube
```
Alternatively, use kubectl to forward the port:
```bash
kubectl port-forward service/hello-minikube 7080:8080
```

Tada! Your application is now available at http://localhost:7080/. <br>

You should be able to see the request metadata in the application output. Try changing the path of the request and observe the changes. Similarly, you can do a POST request and observe the body show up in the output.

___
