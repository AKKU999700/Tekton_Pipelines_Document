# Tekton Pipelines

# Getting start with Tasks
## Open your favorite editor & use Terminal

 ### Set up and run first Tekton Task
* Create a Kubernetes Cluster with [minikube.](https://minikube.sigs.k8s.io/)
* Install tekton pipeline
* Create a Task
* Use TaskRun to instantiate and run task
  
# Prerequisites
* [Install Minikube](https://minikube.sigs.k8s.io/docs/start/)  **The First step :- Installation**
* [Install Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
* Create a Kubernetes cluster

* Create a cluster
* ```
  minikube start --kubernetes-version v1.25.14
  ```

**1:-** minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes

 All you need is Docker (or similarly compatible) container or a Virtual Machine environment, and Kubernetes is a single command away: minikube start

### What you will need
* 2 CPUs or more
* 2GB of free memory
* 20GB of free disk space
* Internet connection
* Container or virtual machine manager, **such as: Docker, QEMU,hyperkit, hyper-V,KVM, Podman, VirtualBox, VMware**   

## Install Docker Engine on Ubuntu
requirement
To install Docker Engine, you need the 64-bit version of one of these Ubuntu versions:

### uninstall old version

Before you can install Docker Engine, you need to uninstall any conflicting packages.

Distro maintainers provide unofficial distributions of Docker packages in APT. You must uninstall these packages before you can install the official version of Docker Engine.

The unofficial packages to uninstall are:

install using apt repository.
*
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

### Add Docker's official GPG key:
```
sudo apt-get update
```
```
sudo apt-get install ca-certificates curl gnupg
```
```
sudo install -m 0755 -d /etc/apt/keyrings
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
```
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

#### Add the repository to Apt sources:
```
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
 sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt-get update
```

**Install the latest version, run:**
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Verify that the Docker Engine installation is successful by running the **hello-world** image.
```
sudo service docker start
```
```
sudo docker run hello-world
```
#### output
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ sudo docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
	(amd64)
 3. The Docker daemon created a new container from that image which runs the
	executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
	to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
# Installation for Linux
 **1:-** To install the latest minikube stable release on x86-64 Linux 
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

**2:-**  Install kubectl binary with curl on linux:

######     Download the latest release with the command:
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

#### Install kubectl
```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
        
#### Start a cluster using the docker driver:
```
minikube start --driver=docker
```
              Or
```
minikube start
```              

#### Output
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ minikube start
😄  minikube v1.32.0 on Ubuntu 22.04
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.28.3 on Docker 24.0.7 ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
	▪ Using image docker.io/registry:2.8.3
	▪ Using image gcr.io/k8s-minikube/kube-registry-proxy:0.0.5
	▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🔎  Verifying registry addon...
🌟  Enabled addons: default-storageclass, storage-provisioner, registry
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

```

You can check that the cluster was successfully created with kubectl:
```
kubectl cluster-info
```
#### Output
 The output confirms that Kubernetes is running:
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A
2/Tekton Pipeline$ kubectl cluster-info
Kubernetes control plane is running at https://192.168.49.2:8443
CoreDNS is running at https://192.168.49.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

# Install Tekton Pipelines
**To install the latest version of Tekton Pipelines, use kubectl:** 
```
kubectl apply --filename \
            https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml    
```
                 
#### Monitor the installation
```
kubectl get pods --namespace tekton-pipelines --watch
``` 

When both tekton-pipelines-controller and tekton-pipelines-webhook show 1/1 under the READY column, you are ready to continue.

#### Output:-
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$  kubectl get pods --namespace tekton-pipelines --watch
NAME                                       	READY   STATUS          	RESTARTS  	AGE
tekton-events-controller-5f96bb4dfc-lmv97  	0/1 	ContainerCreating   0         	61s
tekton-events-controller-7b56f85d69-xn9wn  	1/1 	Running         	14 (9m ago)   4d2h
tekton-pipelines-controller-5d7cfd9f8b-bzsw6   1/1 	Running         	15 (9m ago)   4d2h
tekton-pipelines-controller-769dd55949-f6z52   0/1 	ContainerCreating   0         	61s
tekton-pipelines-webhook-767cb77cfd-kb92m  	1/1 	Running         	14 (9m ago)   4d2h
tekton-pipelines-webhook-7d47885cbf-rx787  	0/1 	ContainerCreating   0         	57s

```

You have successfully installed Tekton Pipelines on your Kubernetes cluster.

**Press** 
Ctrl+c to stopping Monitoring

#### Create and run a basic task

A Task, represented in the API as an object of kind Task, defines a series of Steps that run sequentially to perform logic that the Task requires. Every Task runs as a pod on the Kubernetes cluster, with each step running in its own container.

**1:-** To create a Task, open your favorite editor and create a file named **hello-world.yaml** with the following content:
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: hello
spec:
  steps:
    - name: echo
      image: alpine
      script: |
        #!/bin/sh
        echo "Hello World"
  ```      

**2:-** Apply the changes your cluster:
```
kubectl apply --filename hello-world.yaml
```

#### Output
confirms task complete successfully 
```
^Cdurgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipelinekubectl apply --filename hello-world.yamlml
task.tekton.dev/hello created
```
  

**3:-** A TaskRun object instantiated and executed this Task. Create another file named 
     **hello-world-run.yaml** with the following content:
```   
apiVersion: tekton.dev/v1beta1
kind: TaskRun
metadata:
  name: hello-task-run
spec:
  taskRef:
    name: hello    
``` 

**4:-** Apply the changes to your cluster to launch the Task:
```
kubectl apply --filename hello-world-run.yaml
```
#### Output 
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ kubectl apply --filename hello-world-run.yaml
taskrun.tekton.dev/hello-task-run created
```

**5:-** Verify that everything worked correctly:
```
kubectl get taskrun hello-task-run   
```
#### Output of this command shows the status of the Task
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ kubectl get taskrun hello-task-run
NAME         	SUCCEEDED   REASON  	STARTTIME   COMPLETIONTIME
hello-task-run   True    	Succeeded   3m39s   	2m12s

``` 
The value True under SUCCEEDED confirms that TaskRun completed with no errors.

**Take a look at the logs:**
```
kubectl logs --selector=tekton.dev/taskRun=hello-task-run
```
#### Output
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ kubectl logs --selector=tekton.dev/taskRun=hello-task-run
Defaulted container "step-echo" out of: step-echo, prepare (init), place-scripts (init)
Hello World
```
# Getting start with Pipelines
### Create and run first Tekton pipelines
* Create two Tasks.
* Create a Pipeline containing your Tasks.
* Use PipelineRun to instantiate and run the Pipeline containing your Tasks.
### Prerequisites
**1:-** Complete the getting started with Tasks **Do not clean up your resources,skip the last section.**

**2:-** **[Install tkn, The Tekton CLI.](https://tekton.dev/docs/cli/)**

**Create and run a second Task**  
 You already have a **“Hello World!”** Task. To create a second **“Goodbye!”** 
 Create a new file name **goodbye-world.yaml** and add following content:
 ```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: goodbye
spec:
  params:
  - name: username
    type: string
  steps:
    - name: goodbye
      image: ubuntu
      script: |
        #!/bin/bash
        echo "Goodbye $(params.username)!"
```
This Task takes one parameter, **username.** Whenever this Task is used a value for that parameter must be passed to the Task.

**Apply:-**
```
kubectl apply --filename goodbye-world.yaml
```
#### Output
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ kubectl apply --filename goodbye-world.yaml
task.tekton.dev/goodbye created
```
When a Task is part of a Pipeline, Tekton creates a TaskRun object for every task in the Pipeline.

**Create and run a pipelines**

A Pipeline defines an ordered series of Tasks arranged in a specific execution order as part of the CI/CD workflow.

In this section you are going to create your first **Pipeline,** that will include both the **“Hello World!”** and **“Goodbye!”** Tasks.

**1:-** Create a new file named **hello-goodbye-pipeline.yaml** and add the following content:
```
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: hello-goodbye
spec:
  params:
  - name: username
    type: string
  tasks:
    - name: hello
      taskRef:
        name: hello
    - name: goodbye
      runAfter:
        - hello
      taskRef:
        name: goodbye
      params:
      - name: username
        value: $(params.username)
  	
```   
 
**2:-** Apply the Pipeline configuration to your cluster:
```
kubectl apply --filename hello-goodbye-pipeline.yaml
```

**3:-** A **PipelineRun,** represented in the API as an object of kind **PipelineRun,** sets the value for the parameters and executes a Pipeline. To create PipelineRun, create a new file named **hello-goodbye-pipeline-run.yaml** with the following:
```
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  name: hello-goodbye-run
spec:
  pipelineRef:
    name: hello-goodbye
  params:
  - name: username
    value: "Tekton"
```    

This sets the actual value for the **username** parameter: **"Tekton".**

**4:-** Start the Pipeline by applying the **PipelineRun** configuration to your cluster:

Apply:-
```
kubectl apply --filename hello-goodbye-pipeline-run.yaml
```
#### Output
```
pipelinerun.tekton.dev/hello-goodbye-run created
Tekton now starts running the Pipeline.
```
Tekton now starts running the Pipeline.
To see the logs of the PipelineRun, use the following command:
Apply:-
```
tkn pipelinerun logs hello-goodbye-run -f -n default
```
### Output Shows
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ tkn pipelinerun logs hello-goodbye-run -f -n default
[hello : echo] Hello World

[goodbye : goodbye] Goodbye Tekton
!
```

To learn about Tekton Triggers, skip this section and proceed to the next **Getting start with Triggers**
To delete the cluster that you created for this guide run:
```
Minikube delete
```
#### Output:-
```
🔥  Deleting "minikube" in docker ...
🔥  Deleting container "minikube" ...
🔥  Removing /home/user/.minikube/machines/minikube ...
💀  Removed all traces of the "minikube" cluster.
```

# Getting start with Triggers
* Install Tekton Triggers.
* Create a TriggerTemplate.
* Create a TriggerBind.
* Create an EventListener.


This guide uses a local cluster with minikube

#### Install Tekton Trigger

**1:-** Use kubectl to install Tekton Triggers:

kubectl apply --filename \
```
https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/triggers/latest/interceptors.yaml
```
**2:-** Monitor the installation:
Apply:-
```
kubectl get pods --namespace tekton-pipelines --watch
```
#### Output:-
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ kubectl get pods --namespace tekton-pipelines --watch
NAME                                             	READY   STATUS	RESTARTS   AGE
tekton-events-controller-7b56f85d69-njpwn        	1/1 	Running   0      	62m
tekton-pipelines-controller-5d7cfd9f8b-zxl9v     	1/1 	Running   0      	62m
tekton-pipelines-webhook-767cb77cfd-j4rwv        	1/1 	Running   0      	62m
tekton-triggers-controller-7dfd5d457f-959tf      	1/1 	Running   0      	2m20s
tekton-triggers-core-interceptors-7d89b9ddd9-jmfpr   1/1 	Running   0      	2m7s
tekton-triggers-webhook-744b598f85-tt27p         	1/1 	Running   0      	2m20s
```

Press Ctrl + C to stop monitoring.

**1:-** **Create a Trigger Template**

Create a new file named **trigger-template.yaml** and add the following:
```
apiVersion: triggers.tekton.dev/v1beta1
kind: TriggerTemplate
metadata:
  name: hello-template
spec:
  params:
  - name: username
    default: "Kubernetes"
  resourcetemplates:
  - apiVersion: tekton.dev/v1beta1
    kind: PipelineRun
    metadata:
      generateName: hello-goodbye-run-
    spec:
      pipelineRef:
        name: hello-goodbye
      params:
      - name: username
        value: $(tt.params.username)
 ```

The **PipelineRun** object that you created in the previous tutorial is now included in the template declaration. This trigger expects the **username** parameter to be available; if it’s not, it assigns a default value: **“Kubernetes”.**

Apply the TriggerTemplate to your cluster:
```
kubectl apply -f trigger-template.yaml 
```


#### Output:-
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipelinekubectl apply -f trigger-template.yamlml
triggertemplate.triggers.tekton.dev/hello-template created
```

### Create a TriggerBinding
A TriggerBinding executes the TriggerTemplate, the same way you had to create a PipelineRun to execute the Pipeline

Create a file named **trigger-binding.yaml** with the following content:
```
apiVersion: triggers.tekton.dev/v1beta1
kind: TriggerBinding
metadata:
  name: hello-binding
spec: 
  params:
  - name: username
    value: $(body.username)
```    

This TriggerBinding gets some information and saves it in the username variable.

 Apply the TriggerBinding:
```
kubectl apply -f trigger-binding.yaml
```
```
Output:-
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ kubectl apply -f trigger-binding.yaml
triggerbinding.triggers.tekton.dev/hello-binding created
```

### Create an EventListener

The EventListener object encompasses both the TriggerTemplate and the TriggerBinding.

Create a file named **event-listener.yaml** and add the following:

**1:-** Create a file named **event-listener.yaml** and add the following:
```
apiVersion: triggers.tekton.dev/v1beta1
kind: EventListener
metadata:
  name: hello-listener
spec:
  serviceAccountName: tekton-robot
  triggers:
    - name: hello-trigger 
      bindings:
      - ref: hello-binding
      template:
        ref: hello-template
```        

This declares that when an event is detected, it will run the TriggerBinding and the TriggerTemplate.

2:- The EventListener requires a service account to run. To create the service account for this example 

create a file named **rbac.yaml** and add the following:
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tekton-robot
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: triggers-example-eventlistener-binding
subjects:
- kind: ServiceAccount
  name: tekton-robot
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: tekton-triggers-eventlistener-roles
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: triggers-example-eventlistener-clusterbinding
subjects:
- kind: ServiceAccount
  name: tekton-robot
  namespace: default
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: tekton-triggers-eventlistener-clusterroles
```  

This service account allows the EventListener to create PipelineRuns.

**3:-** Apply the file to your cluster: 

```   
kubectl apply -f rbac.yaml                              
```

#### Output:- 
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ kubectl apply -f rbac.yaml
serviceaccount/tekton-robot created
rolebinding.rbac.authorization.k8s.io/triggers-example-eventlistener-binding created
clusterrolebinding.rbac.authorization.k8s.io/triggers-example-eventlistener-clusterbinding created
```

### Running the Trigger
You have everything you need to run this Trigger and start listening for events.

**1:-** Create the EventListener:
```
kubectl apply -f event-listener.yaml
```     
#### Output   
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ kubectl apply -f event-listener.yaml
eventlistener.triggers.tekton.dev/hello-listener created
```

**2:-** To communicate outside the cluster, enable port-forwarding:
```
kubectl port-forward service/el-hello-listener 8080
```     
The **output** confirms that port-forwarding is working:
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ kubectl port-forward service/el-hello-listener 8080
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

 Keep this service running, don’t close the terminal.

### Monitor the  Trigger
Open a new terminal and submit a payload to the cluster:
```
curl -v \
   -H 'content-Type: application/json' \
   -d '{"username": "Tekton"}' \
   http://localhost:8080
```   

You can change “Tekton” for any string you want. This value will be ultimately read by the goodbye-world Task.

The response is successful:
```
< HTTP/1.1 202 Accepted
< Content-Type: application/json
< Date: Thu, 23 Nov 2023 09:39:55 GMT
< Content-Length: 164
<
{"eventListener":"hello-listener","namespace":"default","eventListenerUID":"2e33729d-1664-4041-88a4-a0585d908de8","eventID":"22453df9-346c-49c4-8ad1-107320eb8245"}
* Connection #0 to host localhost left intact
```

This event triggers a PipelineRun, check the PipelineRuns on your cluster :

Run its command
```
kubectl get pipelineruns
```

The output confirms the pipeline is working:
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ kubectl get pipelineruns
NAME                  	SUCCEEDED   REASON  	START TIME   COMPLETION TIME
hello-goodbye-run     	True    	Succeeded   111m    	109m
hello-goodbye-run-jqmjm   True    	Succeeded   7m12s   	5m51s
```

You see two PipelineRuns, the first one created in the previous guide, the last one was created by the Trigger.

Check the PipelineRun logs. The name is auto-generated adding a suffix for every run, in this case it’s hello-goodbye-run-jqmjm. Use your own PiepelineRun name in the following command to see the logs:

Apply:-
 tkn pipelinerun logs 
 ```
 hello-goodbye-run-jqmjm -f
```
```
Output
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ tkn pipelinerun logs hello-goodbye-run-jqmjm -f
[hello : echo] Hello World

[goodbye : goodbye] Goodbye Tekton!
```

 
Both Tasks completed successfully. Congratulations!

Press Ctrl + C in the terminal running the port-forwarding process to stop it.

Delete the cluster:
```
minikube delete
```

#### Output:-
```
durgesh@durgesh-HP-250-G6-Notebook-PC:/media/durgesh/08B292C3B292B4A2/Tekton Pipeline$ minikube delete
🔥  Deleting "minikube" in docker ...
🔥  Deleting container "minikube" ...
🔥  Removing /home/durgesh/.minikube/machines/minikube ...
💀  Removed all traces of the "minikube" cluster.
```









                     

  





