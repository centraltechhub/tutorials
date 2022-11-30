# **Operators**

**Initialize Minikube**

**Note:**
* This lab exercise was performed on Macbook. 
* As prerequisite Docker is already installed.
* Operators are software extension to Kubernetes that make use of custom resources to manage applications and their components. Minikube is installed to mimic Kubernetes environment to install and test the operator.
* Helm, Ansible or Go can be used as Operator SDK. Ansible is used in this tutorial.
* A demo web application is already built and containeraized. The image is available in centraltechhub/sessionwebapp:v1. Users are free to make use of this container image or use one of their own.
 


**1. Create a new minikube instance with 4GB memory and 2 CPUs.**

Execute the following command:
```CMD
minikube start --memory 4096 cpus 2
```

output:
![image](https://user-images.githubusercontent.com/93929892/196854997-6a5dea30-38b2-414b-80ed-45309ec04416.png)

run the following command to check if all the relevant pods are up and running.
```CMD
kubectl get pods --all-namespaces
```

![image](https://user-images.githubusercontent.com/93929892/196855249-94e1e9fc-e528-4b48-922a-4b6688309fdf.png)

2. Install Operator Lifecycle Manager

Operator Lifecycle Manage (OLM) is a component of the Operator Framework, an open source toolkit to manage Operators, in a streamlined and scalable way.

Execute the following command:
```CMD
operator-sdk olm install
```

output:
![image](https://user-images.githubusercontent.com/93929892/196855953-8305cdd0-a220-4a59-87c5-82ffca129732.png)

Sample review of a particular kind.

![image](https://user-images.githubusercontent.com/93929892/196856651-c7a2596b-de87-472b-97a8-259e6c87ee12.png)

3. Create a new Operator project.

Execute the following command:
```CMD
mkdir sessionwebapp-operator  
cd sessionwebapp-operator  
operator-sdk init --plugins=ansible --domain hub.docker.com
```

output:
<img width="1317" alt="image" src="https://user-images.githubusercontent.com/93929892/204734366-9ae63f8d-95fa-46ce-a684-a40598b0b42d.png">


The initialization command will create the neccessary template and artifacts under the sessionwebapp-operator directory. 

Create a SessionWebAppOperator API.

Execute the following command:
```CMD
operator-sdk create api --group cache --version v1alpha1 --kind SessionWebAppOperator --generate-role
```

output:
<img width="1595" alt="image" src="https://user-images.githubusercontent.com/93929892/204734650-e49e3cf0-d198-4de0-9c19-2f15d02104a8.png">


Open the directory using any editor.

<img width="338" alt="image" src="https://user-images.githubusercontent.com/93929892/204735036-9ce0356a-af63-4e3a-93bf-0c7cbf342948.png">


4. Reviewing the watches.yaml

```yaml
- version: v1alpha1
  group: cache.hub.docker.com
  kind: SessionWebAppOperator
  role: sessionwebappoperator
```
  
The watches.yaml file connects the SessionWebAppOperator resource to the sessionwebappoperator ansible role.

5. Open the roles/sessionwebappoperator/tasks/main.yml file and update as follows.

```yaml
- name: start sessionwebappoperator app
  kubernetes.core.k8s:
    definition:
      kind: Deployment
      apiVersion: apps/v1
      metadata:
        name: webapp-deployment
        namespace: default
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: webapp
        template:
          metadata:
            labels:
              app: webapp
          spec:
            containers:
            - name: webapp
              image: "docker.io/centraltechhub/sessionwebapp:v1"
              env:
                - name: ENV1
                  value: "{{env1}}"
              ports:
                - containerPort: 9080

- name: start sessionwebappoperator service
  kubernetes.core.k8s:
    definition:
      kind: Service
      apiVersion: apps/v1
      metadata:
        name: webapp-deployment
        namespace: default
      spec:
        selector:
          app: webapp
        ports:
          - protocol: TCP
            port: 80
            targetPort: 9080
            nodePort: 30000
        type: LoadBalancer

```

6. Edit the cache_v1alpha1_sessionwebappoperator.yaml

```yaml
apiVersion: cache.hub.docker.com/v1alpha1
kind: SessionWebAppOperator
metadata:
  name: sessionwebappoperator-sample
spec:
  env1: "TestEnvironment1"
```

7. Edit the Makefile

```yaml
Image URL to use all building/pushing image targets
IMG ?= controller:latest
```
to 
```yaml
IMG ?= centraltechhub/operators:v1 # Replace this value with the relevant docker repository and version
```

8.  Building the docker images and pushing it to the respository

Ensure to save all the files. Execute the following command from the project directory.

```CMD
make docker-build docker-push
```

If the build is successful, you will be able to see the image present in the docker repository.

<img width="731" alt="image" src="https://user-images.githubusercontent.com/93929892/196863069-4d8bd143-4017-447f-8513-efd9cdff898b.png">

9. Generate the operator install yaml file. 

Navigate to ../ibertyapp-operator/config/default
Execute the following command:

```CMD
kubectl kustomize config/default/ > config/default/libertyapp-opertor-install.yaml
```

The ibertyapp-opertor-install.yaml will be generated in the ../ibertyapp-operator/config/default directory. Open it and review.

Modify the following:
image: controller:latest to

Under the first kind: ClusterRole, 
- apiGroups:
  - apps
add services to resources list:

resources:
  - deployments
  - daemonsets
  - replicasets
  - statefulsets
  - services

save the file and exit.

10. Install the operator.

navigate to the operator root directory and execute the following command:

```CMD
kubectl create -f config/default/libertyapp-opertor-install.yaml
make deploy
```

check if the operator is running.

![image](https://user-images.githubusercontent.com/93929892/204195415-718c9876-ac3b-4c58-92f5-f6bd05de190a.png)

11. Install the sessionwebapp application.

Modify cache_v1alpha1_libertyappoperator.yaml

```YAML
apiVersion: cache.hub.docker.com/v1alpha1
kind: LibertyAppOperator
metadata:
  name: libertyappoperator-sample
spec:
  env1: "TestEnvironmentVar1"

```

Execute the following command:

```CMD
kubectl create -f config/samples/cache_v1alpha1_libertyappoperator.yaml
make deploy
```
