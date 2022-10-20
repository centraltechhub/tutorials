# **Operators**

**Initialize Minikube**

1. Create a new minikube instance with 4GB memory and 3 CPUs.

`minikube start --memory 4096 cpus 3`

output:
![image](https://user-images.githubusercontent.com/93929892/196854997-6a5dea30-38b2-414b-80ed-45309ec04416.png)

run the following command to check if all the relevant pods are up and running.

`kubectl get pods --all-namespace`

![image](https://user-images.githubusercontent.com/93929892/196855249-94e1e9fc-e528-4b48-922a-4b6688309fdf.png)

2. Install Operator Lifecycle Manager

`operator-sdk olm install`
