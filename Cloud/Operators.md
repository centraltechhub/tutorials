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

output:
![image](https://user-images.githubusercontent.com/93929892/196855953-8305cdd0-a220-4a59-87c5-82ffca129732.png)

Sample review of a particular kind.

![image](https://user-images.githubusercontent.com/93929892/196856651-c7a2596b-de87-472b-97a8-259e6c87ee12.png)

3. Create a new Operator project.

`mkdir liberty-operator`
`cd liberty-operator`
`operator-sdk init --plugins=ansible --domain centraltechhub.com`

output:
![image](https://user-images.githubusercontent.com/93929892/196857036-f68cda14-6f65-46e2-b9c2-64eb5b43d9ee.png)

The initialization command will create the neccessary template and artifacts under the liberty-operator directory. 

Create a LibertyInstall API.

`operator-sdk create api --group cache --version v1alpha1 --kind LibertyInstall --generate-role`

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
  
The watches.yaml file connects the LibertyInstall resource to the libertyinstall ansible role.

5. Open the roles/libertyinstall/defaults/main.yml file and update as follows.

```yaml
- name: start libertyinstall
  kubernetes.core.k8s:
    definition:
      kind: Deployment
      apiVersion: apps/v1
      metadata:
        name: '{{ ansible_operator_meta.name }}-libertyinstall'
        namespace: '{{ ansible_operator_meta.namespace }}'
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: libertyinstall
        template:
          metadata:
            labels:
              app: libertyinstall
          spec:
            containers:
            - name: libertyinstall
              command:
              - libertyinstall
              - -m=64
              - -o
              - modern
              - -v
              image: "docker.io/centraltechhub/sessionwebapp:V2"
               - name: ENV1
                 value: {{env1}}}
              ports:
                - containerPort: 9080

```

6. Edit the cache_v1alpha1_libertyinstall.yaml

```yaml
apiVersion: cache.centraltechhub.com/v1alpha1
kind: LibertyInstall
metadata:
  name: libertyinstall-sample
spec:
  env1: 'Hello World'
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

`make docker-build docker-push`

If the build is successful, you will be able to see the image present in the docker repository.

<img width="731" alt="image" src="https://user-images.githubusercontent.com/93929892/196863069-4d8bd143-4017-447f-8513-efd9cdff898b.png">



