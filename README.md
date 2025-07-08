# Kubernets

# Kind Installation 
    Download the kind Binary 
        curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64

    Make It Executable
        chmod +x ./kind

    Move It to a PATH
        sudo mv ./kind /usr/local/bin/kind

    Verify Installation 
        kind version

# Kubectl instllation
    sudo snap install kubectl --classic

    kubectl version --client



# Creating cluster
    1. single-node cluster
          kind create cluster

    2. default config 
        kind create cluster --name mycluster 

    3. Custom config (multi-node) 
        # cluster-config.yaml
        kind: Cluster
        apiVersion: kind.x-k8s.io/v1alpha4
        nodes:
        - role: control-plane
        - role: worker
        - role: worker

        kind create cluster --name multi-node --config cluster-config.yaml 

    4. 
        kind create cluster --image version  (for example : --image kindest/node:v1.28.0)




# Verifying the Cluster
    kubectl get nodes

# Cluster info
    kubectl cluster-info

# Cluster version
    kubectl version

# Listing Clusters
    kind get clusters

# Deleting a cluster
    kind delete cluster
            or 
    kind delete cluster --name <cluster_name>






kubectl get nodes -o wide

# Communicate to the cluster

    1. Create the Pod in Imperative way 
        kubectl run <your-pod-name> --image <image-name> --port <port-name>
            
            EX: kubectl run nginx-pod --image nginx:latest --port 80

        a) Status of the pod
            kubectl get pods

        b) Details about the pod 
            kubectl describe pod <your-pod-name>
                EX: kubectl describe pod nginx-pod  

        c) Get the logs 
            kubectl logs nginx-pod        

        d) Access the Nginx web page from our local browser 
            kubectl port-forward pod/<your-pod-name> <hostport:pod port>
            
            EX: kubectl port-forward pod/nginx-pod 8080:80

            To run this in background : kubectl port-forward pod/ngnx-pod 8080:80 &

        e) Delete the pod 
            kubectl delete pod <podname>

    2. Declarative way 
        a) Create a yaml file 
            apiVersion: v1
            kind: Pod
            metadata:
                name: nginx-pod-declarative # Name of your pod
                labels:
                app: nginx-declarative      # Labels 
            spec:
                containers:
                - name: nginx-container       # Name of the container within the Pod
                image: nginx:latest         # The Docker image to use
                ports:
                - containerPort: 80         # The port the application inside the container listens on     

        b) Apply the yaml file to the cluster
            kubectl apply -f <file-name>
                EX: kubectl apply -f declarative.yaml



# Multinode cluster 

    create a configuration file 
        EX:multinode_cluster.yaml

    kind create cluster --config <file_name> --name multicluster
        EX : kind create cluster --config multinode_cluster.yaml --name multicluster


    check node status 
        kubectl get nodes 


# Replication controller

    creat configuration file 
        replication_controller.yaml 

        kubectl apply -f replication_controller.yaml

    check status 
        kubectl get rc 

    describe 
        kubectl describe rc

    Scalling Replication controller
        a) kubectl scale rc <replication_name> --replicas=5
        b) usign yaml file 


# Replicaset 

    creat configuration file 
        replica_set.yaml

        kubectl apply -f <file_name>
            EX: kubectl apply -f replica_set.yaml

    check status 
        kubectl get rs 


# Deployment
    A Kubernetes Deployment automates the management of Pods via ReplicaSets. It provides rolling updates, rollback capability, and ensures high availability by maintaining the desired number of Pods during updates.


    Create the yaml file for deploymnt 
        deployment.yaml

        kubectl apply -f deployment.
        
    check status
        kubectl get deployment 
        kubectl get deployment <deplyment_name>


    delete 
        kubectl delete -f deployment.yaml

    scale up 
        kubectl scale deployment <deployment_name> --replicas=5 

        




