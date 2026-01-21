# Kubernates

1. [About](#about-k8s)
2. [Components](#components)
3. [Replicas](#replicasets)
4. [Deployments](#deployments)
5. [Services](#services)
6. [Ingress](#ingress)
7. [Other Stuff](#other-stuff)

## About K8s

Introduced by Google but later on CNCF accepted it and maintained it. Primarily for multiple containerized applications & orchestration of those containers can be done by K8s the same can done by swarm and service but is more useful for production. Read more  about features [here](https://kubernetes.io/)

It groups multiple containers into logical units. Here are some of the features of K8s

- PODs 

- Replication Controller

- Storage Management

- Resource Monitoring

- Health Check

- Services 

- Networking

- Secret management

- Rolling updates

## Components 
    
- Control pane components
    * etcd : to store the secrets and keys to manage the pods
    * Api Server: main entry point which delegates all requests and instructions by any client
    * Controler Manager: manages or controls the replicas, deployments pods states etc 
    * Scheduler: take care of scheduling the resources as per the requests by the server
    * container runtime (Docker/crai): Base on which master node runs

- Worker Components
    * kubelet: It is used by all the worker nodes in order to accept the instructions from the server of the Control pane
    * kubeproxy: This takes care of the communication of the pods and manages the network among the worker nodes
    * container runtime (docker): Base on which worker node runs

- Add ons
    * Web UI

**Note** Read more about components [here](https://kubernetes.io/docs/concepts/overview/components/)


## Installation

* **KubeAdm** installation [refer](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

* **Minikube or Docker desktop** [refer](https://kubernetes.io/docs/tutorials/hello-minikube/)
* **Kops** is a client thorugh which can install k8s on your aws provisioned machine but management will be done by the user.
    
## Steps to be followed for standalone installation using kubeAdm
1. Install docker or container runtime on your machine
2. Install kubeadm kubelet and kubectl
3. Cluster initialize 
    * Create and run kubeadm on the manager node as the admin user 
        > kubeadm init --apiserver-advertise=\<manager-ip> --pod-network-cidr=176.24.0.0/16 --ignore-preflight-errors=NumCPU
        * The above command will set all the components for the master node 
            - Check for the infrastructure setup whether correct or not
            - Pull the images for various components from the repository
            - Create cert dir and generate certificate signing authority
            - Create certificate and key for api-server
            - create certificate and key for various components
            - create a conf file for various component node
            - manifest files are created these are yml or definition files for various components which will be used by the client(kubelet) to start various components 
            - namespace will be created as a special space or virtual division for kube-system
            - rbac will be configured
            - Add-ons will be added (Kubeproxy and coredns)
    * Exit admin user and run the following commands
        > mkdir -p $HOME/.kube

        > sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

        > sudo chown $(id -u):$(id -g) $HOME/.kube/config
    * Check the nodes using kubectl
        > kubectl get nodes
    * Deploy a pod network for pods so that nodes can communicate use **calico** which is an addon that needs to be installed using the manifest file for it.
        > kubectl apply -f \<calico-manifest.yml-link>
    * Now your master node will be up and ready

- Namespace is the default virtual area that you can have to separate your pods so that there is a logical division of your services inside your cluster.
    > kubectl get pods --namespace kubesystem
- Connect the worker node to the master node as admin
    > kubeadm join \<master-ip>:6443 --token \<token> --discovery-token-ca-cert-hash \<certificate key>
    > kubectl get nodes -o wide 

## Controlers

### ReplicaSets  
- only the replicas are running or not

- controller which is used to define the content of the pod which is declared via controllers.
### Deployments
- Provides a declarative way to make sure that the running state of the pod is aligned with the declared/desired state.
- Invoke replicaset to make sure the replica is managed as per the declared value
- Invoke the image in the pod via template
- there are two ways to create deployment cmd and declarative way.

* Command way to create deployment:
    - Create Deployment
        > kubectl create deployment
            \<name> --image=\<image:version> --replicas=3
    - list deployments    
        > kubectl  get deployments
    - list replica sets
        > kubectl get rs
    - list pods
        > kubectl get pods
    -  filter  pods on the label
        > kubectl get pods -l \<labelkey type value(s)>
    -  describe deployment or replicaset pods to see detail
        > kubectl describe deploy \<deploy name>

        > kubectl describe rs \<deployment-name>

        > kubectl pod rs \<deployment-pod-name>

        * You will have the labels and selectors list there which can help you to select.
    - list all deployments, rs and pods
        > kubectl get deploy,rs,pods
    - delete rs 
        > kubectl delete rs \<replicaset-name>
        > kubectl delete deploy \<deploy-name>
    - To watch the status
        >   kubectl get po -w
    - To scale up and down
        > kubectl scale deploy \<deploy-name> --replicas=4
    - To rollout update image
        > kubectl set image deployment.app/\<deploy-name> \<container-name>=\<image:version>
    - To check the logs of the deployment
        > kubctl logs \<deployment-name>

     **Note:** Order of deployment creation and deletion should be like deploy, replicas, pods
* Declarative way (from a file) to create deployment

    - Create a definition in a file as \<deploy-name>.yaml 
    - The following things are important to be mentioned in the definition file
        - apiVersion
        - kind
        - metadata - your choice to give the labels
        - spec

            ```
            apiVersion: apps\v1
            kind: Deployment
            metadata:
                name: some-name-deployment
                labels:
                    app: some-value
                    env: some-value
            spec:
                replicas: 3
                selectors:
                    matchLabels:
                        app: some-value <this value should match with the container name>
                template:
                    metadata:
                        labels:
                            app: some-value
                            env: some-value
                    spec:
                        containers:
                        -   name: some-name <this value should match with selector:matchLabels:app>
                            image: \<image:version>
                            ports:
                            - containerPort: \<some-port>
            ``` 
    - To run the deployment from the definition
        > kubectl apply -f \<some-defination>.yaml
    - Any time you can change the definition and run the above command to scale up or down the pods

    **Note** The recommended way is to use a declarative way to scale up or down.
* Rolling up the updates
    - When your template version needs to be upgraded or the image has new updates how pods can roll up themselves with the updates
    - If you will modify the version in the deployment definition and try to run or apply the definitions.
    - Older containers will go down while other containers come up with new definitions. It happens in a sequential version.
    - While rolling updates new replica set will be created with new pods and the older set will have 0 pods.
    - to check the status of the rolling updates
        > kubectl rollout status deployment/\<deployment-name>
    - to describe the rollout of the deployment
        > kubectl describe deployment/\<deployment-name>
    - to check the history of the rollout
        > kubectl rollout history deployment/\<deployment-name>
    - to undo the rollout
        > kubectl rollout undo deployment/\<deployment-name>
        > kubectl rollout undo deployment/\<deployment-name> --to-revision=2
### Labels 
These are key-value pairs like a tag that can be used as metadata for the pods or objects. One label can be attached to multiple pods or objects. You can use them to select and perform some actions on the selected pods or objects.

* Type **equity** based =, !=
* Type **set** based on (label)

- Commands:

    > kubectl get nodes

    > kubectl get pods

    > kubectl get pods --show-labels
    
    > kubectl get pods -l \<label-key type Value(s)>
     
### Services
Services are something to control the access (building a network) for the pods or deployments (Objects/controllers) from inside the container or outside the container. It creates an abstraction layer to route or balance the load among the pods. It also has a health check available which keeps checking the health of the pods and this helps to balance the load among the pods.

**NOTE** Services use the selectors defined in deployment to map the container into the routing table to route the traffic to the correct container

* Type of Services
    - clusterIP: exposes the services only to internal containers/pods to communicate internally
    - nodePort: It will expose the port of the worker node generally any random port> 30000
    - load balancer: For cloud-based load balancers (eLB)
    - external Name: This is again used by the cloud using the DNS route53

* Creating service
    - cmd: 
        - Create a service for deployment

        **NOTE:** Please make sure the service name matches the label of your deployment
        > kubectl create service \<type>(nodePort) \<service-name> --tcp=\<service-port>:\<container-port>

        **NOTE** clusterIp no port will be exposed by service in nodePort any port > 30000 will be exposed randomly.

        - to list the service
        > kubectl get svc
        
        - to describe the svc
        > kubectl describe <service-name>

    - declarative:
        - create a service definition
            ```
            apiVersion: apps\v1
            kind: Service
            metadata:
                name: some-name-service
                labels:
                    service: some-value
            spec:
                type: NodePort
                ports:
                -   port:80 <service-port>
                    targetPort: 80 <container or pod port>
                    nodePort: 32345 <port exposed to client to connect>
                    protocol: tcp
                selector:
                    app: <container-label pod on which service will connect>  
            ```
        - to apply the file
            > kubectl apply -f \<service-name>.yml
        - to delete the service
            > kubectl delete svc \<service-name>

### Ingress
It is an object/controller to define routing rules for the external clients and how they can access services running on the worker node in the K8s cluster. An abstract layer over your services routes the traffic to the specific pod.
- It is not provided default.
- we can have some ingress controllers provided by (eks, ambassador, citrix and ngnix) 
- ngnix ingress controller.
- We have to install the controller on the master node
    > kubectl apply -f \<ngnix-ingress-controller-url>.yaml

**Note** We have to modify the services and update them to  ClusterIP mode before running in ingress mode
- Create or declare an ingress definition 
    ```
    apiVersion: extensions\v1beta1
    kind: Ingress
    metadata:
        name: some-name-service
        annotations:
            ngnix.ingress.kubernates.io/rewrite-target: /
    spec:
        rules:
        - http:
            paths:
            -  path: /webserver <path to be accessed from external client>
               backend:
                 serviceName: ngnix-service <service name of the pod/container to which we want to route>
                 servicePort: 80 <service port which is exposed by container or pod>
            -  path: /some-path <path to be accessed from external client>
               backend:
                 serviceName: some-service <service name of the pod/container to which we want to route>
                 servicePort: 8080 <service port which is exposed by container or pod>
    ```
- apply the object

You can access it on publicIP:\<ingress-service-port>

### Other stuff
* Network
* Volume

    Containers are **ephemeral** in nature so we can use external storage to retain the data external to the container instead of having storage inside the container.

    volume can be various types which can be mounted on your container
    - secret it is encrypted
    - persistentVolumeChain requires Storage class using Persistancevolume & claim 
    - ConfigMaps is decrypted but available outside the container similar to the central config server for the container.
    - volume needs to be created in the worker node

    ```
    apiVersion: apps\v1
            kind: Deployment
            metadata:
                name: some-name-deployment
                labels:
                    app: some-value
                    env: some-value
            spec:
                replicas: 3
                selectors:
                    matchLabels:
                        app: some-value <this value should match with the container name>
                template:
                    metadata:
                        labels:
                            app: some-value
                            env: some-value
                    spec:
                        containers:
                        -   name: some-name <this value should match with selector:matchLabels:app>
                            image: \<image:version>
                            ports:
                            - containerPort: \<some-port>
                            volumeMounts:
                            -   name: \<volume-name>
                                mountPath: \<path inside a container where it needs to be mounted>
                        volumes:
                        -   name: \<volume-name>
                            hostPath: // volume type
                                path:\<path where volume is created in worker node>
    ```                 
* Probs
    - liveness
    - readiness

