# Miscellaneous:



### 1.  Describe how you would set up a system that would create and deploy an updated containerized application once a PR had been merged in GitHub.

1.  Developers create feature branches, make changes, and submit pull requests.
2.  Once the pull request is merged our Jenkins pipeline is triggered to automate the build and testing process.
3.  In your GitHub repository, go to Settings > Webhooks we need to setup webhooks.
4.  In the Jenkins job Enable the GitHub hook trigger for GITScm polling.
5.  This Jenkins pipeline will create a docker image and store it in the Nexus repository.
6.  After that this pipeline will trigger another Jenkins pipeline-b.
7.  Pipeline-b has a bash script that will change the docker image version tag into Kubernetes helm values.yaml file and create a pull request.
8.  we need to manually approve this pull request. Once this request gets approved argocd will detect the changes and deploy the application using a helm chart with a new image.

### 2.  Why choose a tool like Terraform over something like Ansible or Puppet for configuration management in a cloud environment?

Terraform is primarily focused on provisioning and managing infrastructure.
Terraform uses a declarative language (HCL) to define infrastructure.
It supports multiple cloud platforms.
It maintains state files and provides really good functions.
Documentation is really good practical and easy to learn.

Ansible and Puppet are used for configuration management.
like setting up any app or configuration on any VM.


### 3.  A developer wants to connect from his local system (connected via VPN to the internal network) to a service running in Kubernetes with a Container IP. How would you recommend he do this?

First, connect to a VPN.

1.  Check whether the Kubernetes service exposing your application is of type NodePort, LoadBalancer, or ClusterIP.
2.  If your service is of type NodePort. Find the NodePort assigned to your service (usually in the range 30000-32767).
3.  Use the IP address of any worker node in your Kubernetes cluster used service using the node IP and the Port. (This will work with VPN if the node is a private network)
4.  If your service is of type LoadBalancer then the cloud provider assigns an external IP by using that you can connect.
5.  If your service is of type ClusterIP, it is accessible only within the Kubernetes cluster. used a NodePort or LoadBalancer service.
