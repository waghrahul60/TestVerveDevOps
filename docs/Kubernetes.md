# Kunernetes



### 1. Why would one choose to run two containers in one k8s pod, rather than two separate pods?

Running two containers in a single Kubernetes pod instead of separate pods has multiple advantages depending on the use case. 

Advantages :
1.  Containers within the same pod share the same network namespace. They can communicate with each other by localhost. If we split them into separate pods, we need to use the Kubernetes DNS service to allow communication between pods.
2.  Containers within a pod can mount the same persistence volume.
3.  When we scale a pod both containers within the pod are scaled together.
4.  Each pod has its own IP address, but containers within the pod share the same IP.

UseCases :
1.  Init Container : 
When we wanted to run backend applications like spring boot we wanted to connect the Cloud SQL database.
When cloudsql DB is ready then only Spring bood app container starts deployment in this case we can use the init container.
init container will check if the cloudsql database is ready or not, if DB is ready then only it deploys the spring boot container.
If init container fails entire pod is masked as failed and Kubernetes recreates pod.

2.  SideCar Container :
A sidecar container runs alongside the primary application container within a Kubernetes Pod.
this is used for logging and monitoring, it will collect the metrics, logs and data from main application.
To connect the app with cloudsql Database we need to deploy the Cloud SQL Auth Proxy as a sidecar container alongside your application container in the same pod. Configure the proxy with the necessary credentials or service account.

### 2. What is the impact of deleting a k8s pause container?
1.  Deleting the pause container would result in the termination of the entire pod.
2.  The pause container has ip address associated with it within the pod's network namespace.
this ip is used for internal pod communication between container inside pod.
if the pause container is deleted the network namespace will disrupt communication between containers.
3. The lifecycle of the pod is also affected. If the pause container is deleted k8s considers the entire pod failed terminates the pod and then creates a new pod to replace it.
4.  It will also affect the shared volumes because both containers inside the pod share the same volumes.


### You are running a bare-metal k8s cluster in a standard data center environment (i.e. non-cloud). You have a deployment that needs to have a directory that can be written to by all the pods in a deployment. How would you accomplish this?