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

