# **Operators**

**Initialize Minikube**

This lab exercise was performed on Macbook. As prerequisite docker is already installed. Operators are software extension to Kubernetes that make use of custom resources to manage applications and their components. We will install minikube to mimic Kubernetes environment to install and test our operator. Helm, Ansible or Go can be used as Operator SDK. We will use Ansible in this tutorial.


1. Create a new minikube instance with 4GB memory and 2 CPUs.

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
mkdir libertyapp-operator  
cd libertyapp-operator  
operator-sdk init --plugins=ansible --domain hub.docker.com
```

output:
![image](https://user-images.githubusercontent.com/93929892/196857036-f68cda14-6f65-46e2-b9c2-64eb5b43d9ee.png)

The initialization command will create the neccessary template and artifacts under the liberty-operator directory. 

Create a LibertyApp API.

Execute the following command:
```CMD
operator-sdk create api --group cache --version v1alpha1 --kind LibertyAppOperator --generate-role
```

output:
![image](https://user-images.githubusercontent.com/93929892/196858296-61d700d3-78fa-47b2-85cc-d1fee5ac2f3d.png)

Open the directory using any editor.

<img width="546" alt="image" src="https://user-images.githubusercontent.com/93929892/196858434-7e6dbb10-1fce-4b33-b1bb-af9d8cec27bf.png">

4. Reviewing the watches.yaml

```yaml
- version: v1alpha1
  group: cache.centraltechhub.com
  kind: LibertyInstall
  role: libertyinstall
```
  
The watches.yaml file connects the LibertyAppOperator resource to the libertyappoperator ansible role.

5. Open the roles/libertyappoperator/tasks/main.yml file and update as follows.

```yaml
- name: start libertyappoperator app
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
              image: "docker.io/centraltechhub/sessionwebapp:V2"
              env:
                - name: ENV1
                  value: "{{env1}}"
              ports:
                - containerPort: 9080

- name: start libertyappoperator service
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

6. Edit the cache_v1alpha1_libertyinstall.yaml

```yaml
apiVersion: cache.hub.docker.com/v1alpha1
kind: LibertyAppOperator
metadata:
  name: libertyappoperator-sample
spec:
  env1: "Hello World"
```

7. Edit the Makefile

```yaml
Image URL to use all building/pushing image targets
IMG ?= controller:latest
```
to 
```yaml
IMG ?= centraltechhub/operators:V1
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
kubectl kustomize config/default/ > libertyapp-opertor-install.yaml
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
make install
```
