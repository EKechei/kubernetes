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


## Comparison of Direct Pod Creation and Deployments in Kubernetes

| Feature                      | Direct Pod Creation                                   | Deployments                                      |
|------------------------------|-------------------------------------------------------|---------------------------------------------------|
| **Self-Healing**             | Manual intervention needed to restart failed Pods.    | Automatically replaces failed or deleted Pods.    |
| **Scaling**                  | Manual process to add or remove Pods.                 | Easily scalable by adjusting replica count.       |
| **Updates**                  | Requires manual update process, risk of downtime.     | Supports rolling updates with zero downtime.      |
| **Configuration Consistency**| Inconsistent, prone to manual errors.                 | Ensures consistent configuration across all Pods. |
| **Management Style**         | Imperative and manual.                                | Declarative, automated, and efficient.            |


