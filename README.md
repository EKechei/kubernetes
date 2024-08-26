# Kubernetes-architecture



## Control Plane(Master node)

1) API server - it is the front end of the Kubernetes control plane. It serves as the main entry point for all REST API calls used to manage the Kubernetes cluster.
   
2) etcd - Stores the entire state of the Kubernetes cluster, including information about nodes, pods, services, and other resources.
 
3) controller manager - Runs various controllers that ensure the desired state of the cluster is maintained.
   
4) scheduler - Assigns workloads (pods) to nodes based on resource availability and scheduling policies.
   
5) cloud-controller-manager - allows Kubernetes to interact with the underlying cloud provider’s APIs.

## Data plane(Worker node)

1) kubelet - this is used to talk to the API server and make sure the pods are running accordingly or not.
   
2) kube-proxy - this is a networking component, that assigns the IPs and balances the load.
   
3) container run time - provides the run time environment for containers to execute.

# Installing Kubernetes on a Laptop

1. Install Minikube
```
   https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download
```
3. Install Kubectl
```
   https://kubernetes.io/docs/tasks/tools/
```
# Kubernetes pods

Pods are the smallest deployable units of computing that you can create and manage in Kubernetes. A Pod is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers.
Below is an example of a Pod which consists of a container running the image ```nginx:1.14.2```.

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80

```
To create the Pod shown above, run the following command:
 ``` kubectl apply -f pod.yaml ```


## Direct Pod Creation vs. Deployments: Which Should You Choose in Kubernetes?
While Kubernetes allows you to manage application workloads by directly creating Pods, this approach often falls short in production environments. Let's compare direct Pod creation with using Deployments:


### Comparison of Direct Pod Creation and Deployments in Kubernetes

| Feature                      | Direct Pod Creation                                   | Deployments                                      |
|------------------------------|-------------------------------------------------------|---------------------------------------------------|
| **Self-Healing**             | Manual intervention is needed to restart failed Pods. | Automatically replaces failed or deleted Pods.    |
| **Scaling**                  | Manual process to add or remove Pods.                 | Easily scalable by adjusting replica count.       |
| **Updates**                  | Requires manual update process, risk of downtime.     | Supports rolling updates with zero downtime.      |
| **Configuration Consistency**| Inconsistent, prone to manual errors.                 | Ensures consistent configuration across all Pods. |
| **Management Style**         | Imperative and manual.                                | Declarative, automated, and efficient.            |


# Deployments

A Deployment provides declarative updates for Pods and ReplicaSets.

You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets or to remove existing Deployments and adopt all their resources with new Deployments.

### Creating a Deployment

The following is an example of a Deployment. It creates a ReplicaSet to bring up three nginx Pods:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80

```

Create the Deployment by running the following command:
``` kubectl apply -f deployment.yaml ```


As discussed, Deployments in Kubernetes manage the lifecycle of Pods, ensuring that the desired number of replicas are running and that they are updated in a controlled manner. Deployments handle application scaling, rolling updates, and self-healing, providing a robust mechanism to manage the state of your applications.


# Services

While Deployments ensure that your Pods are running and up-to-date, **Services** address the networking needs of these Pods.

### Why Services Are Important

Services provide a stable networking endpoint for accessing Pods, offering several key benefits:

- **Stable Network Identity**: Services give Pods a consistent IP address and DNS name, ensuring that other parts of your application or external clients can reliably access the Pods regardless of their IP changes.

- **Load Balancing**: Services automatically distribute traffic across multiple Pods, balancing the load and enhancing the availability of your application.

-**Service Discovery**: By using DNS names, Services allow different components of your application to find and communicate with each other easily.

- **Exposure Types**: Services can be configured to expose applications internally within the cluster or externally to the outside world, based on your needs.

