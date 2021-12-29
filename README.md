# 2021-DigitalOcean-K8s-Challenge: Project: Deploy an internal container registry on Kubernetes ##

## About me ##
I'm a Cloud Automation Team Lead that's trying to get more experience working with Kubernetes clusters and CI/CD.  


## Provisioning a Kubernetes cluster on Digital Ocean ##
In Digital Ocean console, click on "Kubernetes" section under "Manage" on the blue bar on the left side.

![Screenshot](digital-ocean-create-cluster.PNG)

I left the Kubernetes version at its default (1.21.5-do.0 at the time of this writing).

I selected the San Francisco datacenter.

VPC Network was left as default.

I made a couple adjustments to cluster capacity, as this is a learning exercise for me.
1. Node plan: $10/month plan (1 GB RAM, 1 vCPU per node)
2. 1 node total, instead of the recommended 3

![Screenshot](create_cluster_2.PNG)

Finally, name the cluster what you want, select the Project to place it in, and click "Create Cluster".

![Screenshot](cluster_create_3.PNG)

Provisioning the cluster takes a few minutes. 

While the cluster is being provisioned, we can download the kubeconfig yaml file to use with kubectl. 



## Installing Prerequisites ##
### WSL 
I chose to use WSL since my work laptop is Windows.  
Install kubectl:

    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
    echo "$(<kubectl.sha256)  kubectl" | sha256sum --check
    sudo su
    chmod +x kubectl
    mkdir -p ~/.local/bin/kubectl
    mv ./kubectl ~/.local/bin/kubectl
    kubectl version --client

## Connecting to you cluster ##

Download your kubeconfig file from digital ocean.Setup your kubectl configuration:
![Screenshot](download-kubeconfig-from-digital-ocean.PNG)

![Screenshot](cluster_connect_1.PNG)

Go to the directory where you have the downloaded config file, and Use the following command to test the connection to the Kubernetes cluster.  

    kubectl --kubeconfig="k8s-1-21-5-do-0-sfo3-1640635833119-kubeconfig.yml" get nodes

For me, this gave the following output:
> kubectl --kubeconfig="k8s-1-21-5-do-0-sfo3-1640635833119-kubeconfig.yml" get nodes
NAME                   STATUS   ROLES    AGE    VERSION
pool-gq8nsqb1b-ueht7   Ready    <none>   2m9s   v1.21.5

However you want to setup kubectl to use this config. I prefer to place it in my home .kube directory as a file titled 'config':
    
    mv k8s-1-21-5-do-0-sfo3-1640635833119-kubeconfig.yml ~/.kube/config