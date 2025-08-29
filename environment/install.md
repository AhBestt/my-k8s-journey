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

### LoadBalancer
To access a LoadBalancer deployment, use the “minikube tunnel” command. Here is an example deployment: <br>
```bash
kubectl create deployment balanced --image=kicbase/echo-server:1.0
kubectl expose deployment balanced --type=LoadBalancer --port=8080
```
In another window, start the tunnel to create a routable IP for the ‘balanced’ deployment: <br>
```bash
minikube tunnel
```
To find the routable IP, run this command and examine the `EXTERNAL-IP` column: <br>
```bash
kubectl get services balanced
```
Your deployment is now available at <EXTERNAL-IP>:8080

___

### Ingress

Enable ingress addon:
```bash
minikube addons enable ingress
```

The following example creates simple echo-server services and an Ingress object to route to these services. <br>
```bash
kind: Pod
apiVersion: v1
metadata:
  name: foo-app
  labels:
    app: foo
spec:
  containers:
    - name: foo-app
      image: 'kicbase/echo-server:1.0'
---
kind: Service
apiVersion: v1
metadata:
  name: foo-service
spec:
  selector:
    app: foo
  ports:
    - port: 8080
---
kind: Pod
apiVersion: v1
metadata:
  name: bar-app
  labels:
    app: bar
spec:
  containers:
    - name: bar-app
      image: 'kicbase/echo-server:1.0'
---
kind: Service
apiVersion: v1
metadata:
  name: bar-service
spec:
  selector:
    app: bar
  ports:
    - port: 8080
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
    - http:
        paths:
          - pathType: Prefix
            path: /foo
            backend:
              service:
                name: foo-service
                port:
                  number: 8080
          - pathType: Prefix
            path: /bar
            backend:
              service:
                name: bar-service
                port:
                  number: 8080
---
```

Apply the contents <br>
```bash
kubectl apply -f https://storage.googleapis.com/minikube-site-examples/ingress-example.yaml
```

Wait for ingress address
```bash
kubectl get ingress
NAME              CLASS   HOSTS   ADDRESS          PORTS   AGE
example-ingress   nginx   *       <your_ip_here>   80      5m45s
```

Note for Docker Desktop Users: <br>
To get ingress to work you’ll need to open a new terminal window and run minikube tunnel and in the following step use 127.0.0.1 in place of <ip_from_above>. <br>

Now verify that the ingress works
```bash
$ curl <ip_from_above>/foo
Request served by foo-app
...

$ curl <ip_from_above>/bar
Request served by bar-app
...
```

