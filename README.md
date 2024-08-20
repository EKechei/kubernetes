# kubernetes-architecture



## Control Plane(Master node)

1) Api server - This is the heart of k8's, it acts as an entrypoint to the architecture and it listens to all the requests from data plane or from users and acts accordingly

2) etcd - This is like a storage space where its stores the info about clusters in key value format and used to restore and backup

3) controller manager - It is used to manage the controllers like replicaset, ingress controleers and make sure the controllers are running all the time

4) scheduler - as the name says it is used to schedule the resources in a nodes.



## Data plane(Worker node)

1) kubelet - this is used to talk to the api server and make sure the pods are running accordingly or not

2) kube-proxy - this is a networking component, assigns the IP's and balances the load.

3) container run time - provides the run time environment for containers to execute.
